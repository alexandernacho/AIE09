# Dense Vector Retrieval: Building Search That Actually Works

*Stop delegating search to engineering. Learn how to build retrieval systems that ship.*

## Introduction

Nobody warns you about this when you become a PM: the quality of your search depends on what you feed it, not which LLM you choose.

Session 2 of the AI Engineering bootcamp drills into one core idea. Dense vector retrieval isn't abstract mathematics. It's what connects a user asking a question to an AI that actually knows the answer. You can build it yourself.

Your users ask your products questions every single day. They search for information. They interact with help systems. All of those succeed or fail on one thing—can the system find what it needs? For years, that's meant handing off the problem to engineering. You'd describe what you wanted, hope they implemented it right, and wait around.

Not anymore. I'm going to walk you through building a working retrieval system from scratch. By the end, you'll see why retrieval matters far more than the LLM behind it. You'll have actual code in your hands. You'll be able to test your search ideas without anyone else's permission.

## Setup

Install the required libraries:

```bash
pip install openai numpy
```

Set your OpenAI API key:

```bash
export OPENAI_API_KEY="sk-..."
```

## Prerequisites

- Some Python. You need to know loops, functions, dictionaries. That's it.
- An OpenAI API key. Costs pennies for testing.
- A text file with your knowledge base. Could be a FAQ, documentation, anything.

The code is straightforward. The concepts are even simpler. No advanced math required.

## What Dense Vector Retrieval Actually Is

Let's start with the problem it solves.

You're building a customer support chatbot. Someone asks: "What exercises help with lower back pain?"

Your chatbot needs to do two things:

1. **Find the right information** (retrieval)
2. **Write a decent answer** using that information (generation)

Most PMs fixate on #2. Which model? Which prompt? What tone? Here's what actually happens: if you fail at #1, everything else is pointless. Your LLM could be brilliant. Feed it garbage context and you get garbage answers.

Dense vector retrieval fixes this. Convert documents to numbers. Convert the question to numbers. Find which numbers are closest. Pass those to the LLM.

That's it. Simple and powerful.

### Why Vectors?

To a computer, "exercise" and "workout" are completely different. To you, they mean the same thing.

Embeddings bridge that gap. They turn words and sentences into coordinates in high-dimensional space—usually 1,536 dimensions with modern models. Things that mean the same thing end up near each other in that space.

When someone asks about "exercise," the system doesn't search for that exact word. It searches for everything semantically nearby. It finds "workout," "physical activity," "stretching"—things it's never been specifically trained on as synonyms, yet things that clearly mean the same thing.

That's the whole power here. Meaning, not keywords.

### The RAG Architecture

Retrieval Augmented Generation is the fancy name. Two pieces:

1. **Retrieval**: Turn the question into a vector → Search the database → Pull out the top K most similar documents
2. **Generation**: Pass those documents to the LLM → LLM writes an answer grounded in real context

Retrieval does 80% of the actual work. Get it right and generation becomes almost trivial.

## Building Your First Retrieval System

Time to actually build this. Real code. Real example.

### Step 1: Load Your Knowledge Base

You start with documents. Health guides. FAQs. Internal wikis. Whatever you want the system to search.

```python
# Load your knowledge base
with open("health_guide.txt", "r") as f:
    raw_text = f.read()

# Split into basic documents (we'll chunk further in step 2)
documents = [{"content": raw_text}]
```

### Step 2: Split Into Chunks

Your source documents are probably massive. A 50-page document won't fit neatly into an embedding or context window.

Split them. This is chunking. Most people underestimate how much it matters.

```python
def chunk_text(text, chunk_size=500, overlap=50):
    """Split text into overlapping chunks."""
    chunks = []
    for i in range(0, len(text), chunk_size - overlap):
        chunk = text[i:i + chunk_size]
        if chunk.strip():  # Only add non-empty chunks
            chunks.append(chunk)
    return chunks

# Split your raw text
raw_text = documents[0]["content"]
chunks = chunk_text(raw_text, chunk_size=500, overlap=50)
# Now you have many smaller, overlapping chunks
```

Different content types need different chunking. A technical manual wants bigger chunks. A FAQ wants smaller ones. Medical content might need different metadata than a sales playbook. You'll experiment. You should.

Storing your `splitter` as a variable matters for exactly this reason. Swap it out. Try a different strategy. Measure which one actually works. Then you iterate without rewriting code.

### Step 3: Convert Chunks to Vectors

Each chunk becomes a vector now.

```python
from openai import OpenAI
import asyncio

client = OpenAI()  # Uses OPENAI_API_KEY env variable

# Use async to run API calls in parallel
async def get_embeddings_async(texts):
    """Get embeddings for multiple texts in parallel."""
    tasks = [
        asyncio.to_thread(
            lambda text=text: client.embeddings.create(
                model="text-embedding-3-small",
                input=text
            ).data[0].embedding
        )
        for text in texts
    ]
    return await asyncio.gather(*tasks)

vectors = asyncio.run(get_embeddings_async(chunks))
```

Why async? Embedding 373 documents one by one is brutal.

- **Sequential**: 373 documents × 200ms per API call = 74.6 seconds
- **Async (batched)**: ~200ms total (all requests in parallel)

That's 300x faster. Not because I'm obsessed with speed. Because waiting 75 seconds per test destroys your ability to iterate. With async, you test 10 different approaches in the time it takes to test one sequentially.

### Step 4: Store in a Vector Database

You need to store vectors somewhere so searches are fast.

```python
import numpy as np

class SimpleVectorDB:
    def __init__(self):
        self.vectors = []
        self.documents = []

    def add_documents(self, docs, vectors):
        """Store documents and their vectors."""
        self.documents.extend(docs)
        self.vectors.extend(vectors)

    def search(self, query_vector, k=4):
        """Find top K most similar documents."""
        query_vector = np.array(query_vector)
        similarities = []

        for vec in self.vectors:
            # Cosine similarity
            similarity = np.dot(query_vector, vec) / (
                np.linalg.norm(query_vector) * np.linalg.norm(vec)
            )
            similarities.append(similarity)

        # Get top K indices
        top_indices = np.argsort(similarities)[-k:][::-1]
        return [self.documents[i] for i in top_indices]

# Create and populate database
vector_db = SimpleVectorDB()
vector_db.add_documents(chunks, vectors)
```

A vector database is simple: store documents and their vectors, then find the closest matches using distance metrics (we'll cover that).

### Step 5: Connect Retrieval to Generation

Wire them together.

```python
class RAGPipeline:
    def __init__(self, vector_db):
        self.vector_db = vector_db
        self.client = OpenAI()

    def run(self, user_query: str, k: int = 4):
        # Step 1: Get query embedding
        query_embedding = self.client.embeddings.create(
            model="text-embedding-3-small",
            input=user_query
        ).data[0].embedding

        # Step 2: Find relevant documents
        retrieved_docs = self.vector_db.search(query_embedding, k=k)

        # Step 3: Combine into context
        context = "\n".join(retrieved_docs)

        # Step 4: Generate answer
        response = self.client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {
                    "role": "system",
                    "content": "You are a helpful assistant. Answer using the provided context."
                },
                {
                    "role": "user",
                    "content": f"Context:\n{context}\n\nQuestion: {user_query}"
                }
            ]
        )

        return {
            "response": response.choices[0].message.content,
            "context": context,
            "retrieved_count": len(retrieved_docs)
        }

# Use it
pipeline = RAGPipeline(vector_db)
result = pipeline.run("What exercises help with lower back pain?")
print(result["response"])
```

Done. You have a working retrieval-augmented generation system.

## What Actually Matters

You've built something functional. What separates good from broken?

### Retrieval Quality Beats Everything Else

This is the key thing to understand:

**A cheap LLM with great context beats an expensive LLM with garbage context. Every time.**

An expensive model fed garbage produces bad answers. A cheap model fed perfect context produces good ones. Most PMs have this backwards. We've been optimizing the wrong lever.

Test this yourself. Pick a question your support team answers constantly. Search your knowledge base the way your system would. If the top 4 results are bad, no LLM fixes that. If the top 4 are perfect, even a cheap model gives you good answers.

Your job as a PM becomes ensuring retrieval works. Not choosing between GPT-5 and Claude-7.

### Distance Metrics: Cosine vs. Euclidean vs. Manhattan

You can measure closeness in vector space different ways. Cosine similarity—how much two vectors point the same direction—is standard. But there are alternatives.

```python
def cosine_similarity(a, b):
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

def euclidean_distance(a, b):
    return np.sqrt(np.sum((a - b)**2))

def manhattan_distance(a, b):
    return np.sum(np.abs(a - b))
```

Different metrics rank results differently. Your health FAQ might work better with cosine. Your product documentation might work better with euclidean.

Test them. Keeping your vector database configurable means you can swap metrics, measure which one ranks better, and ship the winner.

### Filtering: Add Domain Knowledge

Vector search gives you relevance. Add business logic on top.

```python
class EnhancedRetriever:
    def search_by_text_and_category(self, query: str, category: str, k: int = 4):
        # Get top candidates
        candidates = self.vector_db.search_by_text(query, k=10)

        # Filter to specific category
        filtered = [doc for doc in candidates if doc.metadata["category"] == category]

        # Return top K
        return filtered[:k]
```

Sometimes you want "exercises for lower back pain" from exercises, not nutrition. A vector search might rank a nutrition article high because it mentions backs. You know contextually you need exercise advice.

This is where you add PM value. You understand your domain. The model doesn't.

## When You Ship It

That code above gets you started. Production changes a few things:

1. **Async everything**: Single API calls are death. Batch them. That 300x speedup isn't negotiable.

2. **Monitor retrieval**: Track which queries give bad results. Build a dataset of failures. Use it to improve.

3. **Version your vectors**: Change your chunking strategy and old vectors become garbage. Track which embedding model version created each vector.

4. **Make it configurable**: Keep `splitter`, `embedding_model`, `vector_db`, and `distance_metric` as variables. You're not overthinking it. You're enabling experimentation.

5. **Test together**: A/B test cosine against euclidean. Try different chunk sizes. Test with and without filtering. Measure what actually improves answers.

## Key Takeaways

1. Retrieval quality beats LLM choice. Garbage context ruins everything. Good context saves even cheap models.

2. Async batching is non-negotiable. 300x improvements aren't theoretical—they're the difference between iterating rapidly and waiting around.

3. Distance metrics matter for your data. Test cosine, euclidean, manhattan. Measure which produces better results for your content.

4. Keep components as variables. Your splitter, embedding model, vector database, distance metric. This is how you experiment without rewriting everything.

5. Filtering beats pure retrieval. Business logic matters. You understand your domain better than any model.

6. You can ship this yourself. No ML degree required. Load documents. Embed them. Store vectors. Search them. Pass results to an LLM. Done.

## Conclusion

This is what becoming a Product Engineer means.

You used to brief an engineer. Now you build a working search system yourself, measure what works, hand them something validated. You're not becoming a developer. You're becoming complete—product intuition plus technical capability.

Dense vector retrieval is fundamental to how modern AI systems find knowledge. Now you know how to build it.

Next: grab this code. Load your company's knowledge base. Experiment with chunk sizes, distance metrics, filters. Measure what improves answers. Ship it.

That's vibe coding. That's the path from idea to working code. That's what product work looks like now.

## Resources

- [AI Makerspace Bootcamp](https://aimakerspace.io/)
- [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401)
- [Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks](https://arxiv.org/abs/1908.10084)
- [OpenAI Embeddings API](https://platform.openai.com/docs/guides/embeddings)
- [Session 2 Code: Pythonic RAG Assignment](https://github.com/AI-Maker-Space/AIE9/tree/main/02_Dense_Vector_Retrieval)

---

*This post is part of my journey from Product Manager to Product Engineer, learning to build with AI through AI Makerspace's AI Engineering Bootcamp (AIE9).*