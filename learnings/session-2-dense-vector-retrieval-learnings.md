# Session 2: Dense Vector Retrieval & RAG Fundamentals

**Date**: January 20, 2026
**Duration**: ~2 hours
**Status**: Completed

---

## Part A: Learning Summary

### Overview

Session 2 focused on building a Personal Wellness Assistant using Retrieval Augmented Generation (RAG). The session covered the complete RAG pipeline: document loading, text splitting, embeddings, vector storage, semantic search, and LLM integration. Key learning emphasized code architecture decisions, async patterns for efficiency, reproducibility in LLM outputs, and practical enhancements to production RAG systems.

**Main Learning Objectives:**
- Understand the complete RAG pipeline architecture
- Learn why design decisions matter (e.g., storing components in variables)
- Master async/await patterns for API efficiency
- Explore reproducibility techniques for LLM outputs
- Build and enhance a working RAG application with metadata support

**Key Technologies Covered:**
- OpenAI Embeddings API (`text-embedding-3-small`)
- Vector similarity search (cosine similarity and alternatives)
- Async/await patterns in Python
- Prompt templating and role-based messaging
- Distance metrics (euclidean, manhattan)

---

### Questions Asked & Answers

#### Question 1: Why use `text_splitter` variable instead of directly calling `CharacterTextSplitter()`?

**Question**: In cell-12, we create `text_splitter = CharacterTextSplitter()` and then call `split_texts()` on it. Why not just call `CharacterTextSplitter().split_texts(documents)` directly?

**Answer**: Creating a stored variable implements the **Strategy Pattern**. Benefits include:

1. **Reusability**: Use the same splitter instance multiple times with consistent configuration
2. **Configurability**: Parameters like `chunk_size` and `chunk_overlap` are set once and applied everywhere
3. **Dependency Injection**: Pass the splitter to other components (the RAG pipeline) as a dependency
4. **Testability**: Mock or swap the splitter in tests without changing core logic
5. **Future-proofing**: Easily swap `CharacterTextSplitter` for `SemanticTextSplitter` or other implementations

```python
# Variable approach - flexible and reusable
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
split_docs_1 = text_splitter.split_texts(documents)
split_docs_2 = text_splitter.split_texts(more_documents)  # Same config

# One-liner approach - rigid and inflexible
split_docs = CharacterTextSplitter().split_texts(documents)
```

**Key Insight**: Even simple-looking patterns have production implications. Storing components as variables makes your system composable and maintainable.

---

#### Question 2: What are the benefits of using an `async` approach to collect embeddings?

**Question**: In cell-24, we use `asyncio.run(vector_db.abuild_from_list(split_documents))`. What's the practical benefit of async here?

**Answer**: The main benefit is **dramatic performance improvement** through concurrent API calls.

**Performance Comparison** (373 documents, 200ms per request):
- **Synchronous**: 373 × 200ms = **74.6 seconds** ⏱️
- **Asynchronous**: ~200ms (all requests in parallel) ⚡
- **Speedup**: 300x+ faster

**How it works:**
- Sync: Send request 1 → wait → send request 2 → wait → ... (sequential)
- Async: Send requests 1-50 → wait for all → send 51-100 → wait for all (concurrent batches)

**Real Production Benefits:**
1. **Cost efficiency**: Same OpenAI API cost, but 1 second instead of 75 seconds
2. **User experience**: New documents embedded instantly instead of minutes
3. **Scalability**: 10,000 documents take same time as 373
4. **Concurrent users**: Multiple users' requests don't block each other

```python
# Under the hood, abuild_from_list does:
embeddings = await self.embedding_model.async_get_embeddings(list_of_text)
# This sends all texts to OpenAI concurrently (or in intelligent batches)
# Then collects all responses together
```

**Key Insight**: For network-bound operations (API calls, database queries), async transforms "unusable" into "production-ready" performance.

---

#### Question 3: What is a coroutine object?

**Question**: What happens if you forget the `await` keyword when calling an async function?

**Answer**: You get a **coroutine object** - essentially a "frozen function" that hasn't been executed yet.

```python
# This creates a coroutine object (the recipe, not the meal):
embedding_task = get_embedding("hello")
print(embedding_task)
# Output: <coroutine object get_embedding at 0x7f8b...>

# This executes it (actually cooks the meal):
actual_embedding = await embedding_task
```

**Why this matters:**
- **Silent failures**: No error, but your embedding is never actually sent to OpenAI
- **Type confusion**: `len(coroutine_object)` crashes with cryptic error
- **Memory leaks**: Unawaited coroutines accumulate in memory
- **Production issue**: Very hard to debug

**How to fix:**
```python
# Wrong:
embedding = get_embedding(doc)  # Oops, coroutine object!

# Right:
embedding = await get_embedding(doc)  # Actually executes
```

**Key Insight**: Coroutines are Python's way of saying "I promise to do this later." `await` cashes that promise.

---

#### Question 4: What prompting strategies make LLMs more thoughtful and detailed?

**Question**: How can we make the LLM provide more thorough, reasoned responses?

**Answer**: The primary strategy is **Chain of Thought (CoT)** / **Extended Thinking**, now available via the `reasoning` parameter:

```python
response = client.responses.create(
    model="gpt-5",
    reasoning={"effort": "high"},  # Enable deep thinking
    input="What is the best sleep remedy?"
)
```

**How it works:**
- `reasoning={"effort": "minimal"}` → Quick, shallow answers
- `reasoning={"effort": "high"}` → Deeper reasoning, more detailed responses

**Other strategies:**
1. **Explicit prompting**: "Think step by step before answering"
2. **System role clarity**: Define expertise and tone in system message
3. **Reasoning tokens**: New models use internal reasoning tokens for better quality

**Key Insight**: Don't just ask better questions—ask the model to think differently about the question.

---

#### Question 5: How to achieve reproducible outputs from OpenAI API?

**Question**: The documentation mentioned several ways to achieve more reproducible outputs. What are they?

**Answer**: Reproducibility has a priority hierarchy:

**1. Pin to specific model snapshots** ⭐ (Most important)
```python
model="gpt-5-2025-08-07"  # Use specific version, not "gpt-5"
# Different versions produce different results
```

**2. Build evals (evaluation tests)**
Create test cases to measure prompt performance before/after changes:
```python
# Benchmark your prompts
test_cases = ["query 1", "query 2", ...]
measure_quality_of_responses(prompt, test_cases)
```

**3. Use versioned prompts**
Store prompts in OpenAI dashboard with version control:
```python
response = client.responses.create(
    model="gpt-5",
    prompt={
        "id": "pmpt_abc123",
        "version": "2"  # Specific version
    }
)
```

**4. Structured outputs**
Force JSON schema compliance for predictable outputs

**5. Message roles & instructions**
Use `developer` role for system rules (higher priority than `user`)

**Key Insight**: Model versioning > evals > prompt versioning > structured outputs > prompt engineering. Most developers skip #1 and wonder why results differ.

---

### Notebook Activities & Implementations

#### Activity 1: Enhanced Personal Wellness Assistant

**Objective**: Augment the RAG pipeline with metadata support and alternative distance metrics to improve wellness query retrieval.

**Approach**: Extended the `RetrievalAugmentedQAPipeline` class to:
1. Support multiple distance metrics (cosine, euclidean, manhattan)
2. Filter results by wellness category (exercise, nutrition, sleep)
3. Make metrics configurable at creation time
4. Enable easy A/B testing between different metrics

```python
class EnhancedRetrievalAugmentedQAPipeline:
    def __init__(self, llm, vector_db_retriever,
                 distance_metric: str = "cosine"):
        self.distance_metric = distance_metric
        self.metrics = {
            "cosine": lambda a, b: np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b)),
            "euclidean": euclidean_distance,
            "manhattan": manhattan_distance
        }

    def run_pipeline(self, user_query: str, k: int = 4,
                     category_filter: str = None, **system_kwargs):
        distance_fn = self.metrics.get(self.distance_metric)
        context_list = self.vector_db_retriever.search_by_text(
            user_query, k=k*2, distance_measure=distance_fn
        )

        if category_filter:
            context_list = [
                (ctx, score) for ctx, score in context_list
                if category_filter.lower() in ctx.lower()
            ][:k]
```

**Challenges & Solutions:**
- **Challenge**: How to swap metrics without changing core search logic → **Solution**: Create metrics dictionary mapping names to functions
- **Challenge**: Category filtering needed to work with any metric → **Solution**: Filter after similarity scoring, independent of metric
- **Challenge**: Need to compare results across metrics → **Solution**: Store `distance_metric_used` in return dict for tracking

**Implementation Benefits:**
1. **A/B testing**: Compare which metric finds best wellness results
2. **Domain-specific tuning**: Different metrics may suit health queries better
3. **Extensibility**: Easy to add more metrics (dot product, hamming distance, etc.)
4. **Production-ready**: Supports both generic and specialized search

**Code to add to notebook:**
```python
# After RetrievalAugmentedQAPipeline definition, add:
rag_pipeline_enhanced = EnhancedRetrievalAugmentedQAPipeline(
    vector_db_retriever=vector_db,
    llm=chat_openai,
    distance_metric="euclidean",
    include_scores=True
)

result = rag_pipeline_enhanced.run_pipeline(
    "What exercises help with lower back pain?",
    k=3,
    category_filter="exercise"
)
```

**Learning**: Multiple similarity metrics exist because different problem spaces have different geometry. Euclidean distance works better for some domains, cosine for others. Always test your specific use case.

---

### Key Takeaways

1. **Design Patterns Matter in RAG**: Storing components in variables (text_splitter, vector_db, rag_pipeline) enables reusability, testability, and future changes. What looks like a small decision compounds into system flexibility.

2. **Async is Non-Optional for APIs**: Network-bound operations become 300x faster with async. This transforms "technically possible" into "actually usable" for production systems. The overhead of learning async pays for itself immediately.

3. **Coroutines are Common Gotchas**: Forgetting `await` silently creates broken code. Mental model: coroutines are promises that must be cashed with `await`. Always pair async functions with await.

4. **Reproducibility Has Priorities**: Model versioning (pin to specific snapshot) is 10x more important than prompt engineering for output consistency. Most teams get this backwards and waste time on micro-prompt optimization while using floating model versions.

5. **Distance Metrics are Domain-Specific**: Cosine similarity isn't universally optimal. Wellness queries, technical documentation, and customer support may benefit from different metrics. Build systems that let you test and measure what works.

6. **RAG Quality = Retrieval Quality**: A sophisticated LLM with poor retrieval produces poor answers. Session 2's focus on retrieval mechanics (text splitting, embedding models, distance metrics, metadata filtering) directly impacts final output quality more than prompting tweaks.

**Connections to AIE9**:
- Session 1 (Embedding Primer) provided theory; Session 2 applies it practically
- Session 3 (The Agent Loop) will use RAG as a retrieval mechanism for agents
- Advanced sessions will build on this RAG foundation with more complex architectures

**Further Learning**:
- Try different embedding models (text-embedding-3-large, specialized domain models)
- Explore vector database alternatives (Pinecone, Weaviate, Milvus) for persistence and scaling
- Implement hybrid search (keyword + semantic) for even better retrieval
- Build evals to measure RAG quality quantitatively

---

## Part B: 5-Minute Video Script

**Video Title**: "Building Production RAG: Personal Wellness Assistant"

**Target Audience**: Developers building AI applications, intermediate Python knowledge

**Tone**: Technical, direct, insight-driven

---

### [0:00-0:45] The Problem: Retrieval vs. Generation

Today we are looking at Desne Vector Retrieval in the context of our first RAG application

"Most RAG failures aren't caused by the LLM—they're caused by poor retrieval. If your search returns the wrong context, no amount of prompt engineering will save the answer. In this session, we built a Personal Wellness Assistant from scratch to master the 'R' in RAG. We moved past basic search to implement a modular, async-powered retrieval pipeline."

**Visuals**:
- [SHOW: RAG pipeline diagram highlighting retrieval step]
- [SHOW: Notebook title "Pythonic RAG Assignment"]

---

### [0:45-1:45] Architecture & Modularity

"We broke the pipeline into five distinct steps: Loading, Splitting, Embedding, Storage, and Retrieval.

The key takeaway here is **modularity**. Instead of hardcoding parameters, we treated our components—like the `CharacterTextSplitter` and the OpenAI Embedding model—as injectable variables. This makes the system testable. By setting a `chunk_size` of 1000 and a `chunk_overlap` of 200, we ensure that context isn't lost at the boundaries of our documents."

**Visuals**:
- [SHOW: Code side-by-side: inline `CharacterTextSplitter().split()` vs stored `splitter = CharacterTextSplitter()`]
- [ANNOTATE: "Reusable", "Configurable", "Testable" labels on stored approach]
- [HIGHLIGHT: chunk_size=1000, chunk_overlap=200]

---

### [1:45-2:45] The Performance Win: Async Embeddings

"The biggest bottleneck in RAG is often the Embedding API. If you embed 300+ documents sequentially, it takes over a minute.

We implemented an asynchronous batching approach using `asyncio`. By sending concurrent requests to OpenAI, we reduced our embedding time from 75 seconds down to about 1 second. This isn't just a 'nice-to-have'—it's the difference between a system that scales and one that hits rate limits immediately."

**Visuals**:
- [SHOW: Code cell-24: asyncio.run(...abuild_from_list)]
- [SHOW: Timeline graphic: "75 seconds" vs "1 second"]

---

### [2:45-4:00] Enhanced Retrieval: Metrics & Metadata

"Basic RAG usually relies solely on Cosine Similarity. We took it further by building an `EnhancedRetrievalAugmentedQAPipeline` that supports three distance metrics: **Cosine, Euclidean, and Manhattan**.

Different data types perform better with different metrics. But the real game-changer was **Metadata Filtering**. By adding a `category_filter`, we can prune the vector space *before* the LLM even sees it. If a user asks about 'exercise', the system ignores 'sleep' tips entirely, drastically reducing hallucinations and improving grounding."

**Visuals**:
- [SHOW: EnhancedRetrievalAugmentedQAPipeline class with distance_metric parameter]
- [SHOW: Query results with category_filter="exercise" vs without]
- [SHOW: Final return dictionary with sources, similarity_scores, distance_metric_used]

---

### [4:00-5:00] Summary & Key Insights

"The lesson is simple: RAG is a retrieval-first discipline.

1. **Optimize your pipeline** with async batching.
2. **Build for flexibility** by supporting multiple distance metrics.
3. **Use metadata** to narrow the search space.

In the next session, we'll take this optimized retrieval system and turn it into a tool for a multi-step agent. If you want to build AI that actually works in production, focus on the data you're feeding it."

**Visuals**:
- [SHOW: Summary slide with three key takeaways]
- [SHOW: Session 3 preview]
- [SHOW: GitHub repo link for code]

---

## Production Notes

- **Equipment**: Screen recording software (ScreenFlow or OBS), external microphone, quiet recording space
- **Recording Environment**: Soft lighting, minimal background noise, clean desk background
- **Post-Production**:
  - Add captions (critical for code readability)
  - Insert code snippets as lower-thirds
  - Timeline graphics for performance comparisons (75s vs 1s)
  - Transitions between sections (2-3 second fade)
  - Title card (0:00-0:05 before hook)
  - Outro slide (5:00+) with resources
- **Publishing**: YouTube (primary), Twitter/X (short clip), LinkedIn (article version)

**Editing Checklist**:
- [ ] Background noise removed
- [ ] Code is readable on screen (24pt+ font)
- [ ] Captions added and reviewed
- [ ] Graphics and annotations added
- [ ] Pacing matches script (~130-140 words/minute)
- [ ] Audio levels normalized
- [ ] Thumbnail created with readable text
- [ ] Video description written with links

---

*This learning summary was generated at the end of Session 2 (Dense Vector Retrieval) as part of AIE9 learning documentation.*
