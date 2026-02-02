# Session 3: The Agent Loop - Building Production Agents with LangChain 1.0

**Date**: February 2, 2026
**Duration**: ~2.5 hours
**Status**: Completed

---

## Part A: Learning Summary

### Overview

Session 3 transitioned from RAG retrieval (Session 2) to autonomous agents that can reason and use tools. The session covered the agent loop architecture, LangChain 1.0's `create_agent` API, custom tool creation, middleware for production guardrails, and Agentic RAG—where an agent decides when and how to retrieve information rather than following a fixed retrieve-then-generate pattern.

**Main Learning Objectives:**
- Understand what an "agent" is and how the agent loop works
- Learn LangChain core constructs (Runnables, LCEL)
- Master the `create_agent` function and middleware system
- Build an Agentic RAG application using Qdrant

**Key Technologies Covered:**
- LangChain 1.0 (`create_agent`, Runnables, LCEL)
- LangGraph (underlying agent runtime)
- Qdrant (in-memory vector database)
- LangSmith (tracing and observability)
- Firecrawl (web scraping API)
- Middleware hooks (`before_model`, `after_model`, `ModelCallLimitMiddleware`)

---

### Questions Asked & Answers

#### Question 1: What determines whether the agent continues calling tools or returns a final answer?

**Question**: In the agent loop, what determines whether the agent continues to call tools or returns a final answer to the user?

**Answer**: The model itself decides whether another tool call is needed or if it has gathered sufficient information to respond. The LLM evaluates the current conversation state and either:
- Outputs a tool call (continue the loop)
- Outputs a final response (exit the loop)

Guardrails like `ModelCallLimitMiddleware` act as safety limits—the agent can be stopped after N iterations to prevent infinite loops and runaway costs.

```python
# The agent loop decision flow:
# 1. Model receives state (messages, tool results)
# 2. Model decides: call tool OR respond
# 3. If tool call → execute tool → loop back to 1
# 4. If response → exit loop and return to user
```

**Key Insight**: The agent has autonomy within constraints. It reasons about what it needs, but middleware ensures it can't spin forever.

---

#### Question 2: Why are tool docstrings so important?

**Question**: Looking at the `calculate` and `get_current_time` tools, why is the docstring so important for each tool?

**Answer**: The docstring is literally what the LLM sees when deciding which tool to use. All tool names, descriptions (docstrings), and parameter schemas get injected into the prompt. A poor docstring = poor tool selection.

```python
@tool
def calculate(expression: str) -> str:
    """Evaluate a mathematical expression. Use this for any math calculations.

    Args:
        expression: A mathematical expression to evaluate (e.g., '2 + 2', '10 * 5')
    """
```

The docstring tells the agent:
1. **What the tool does** ("Evaluate a mathematical expression")
2. **When to use it** ("Use this for any math calculations")
3. **How to call it** (Args section defines the parameter schema)

**Key Insight**: Think of docstrings as the tool's "sales pitch" to the LLM. If it's vague, the agent won't know when to use it. If it's too narrow, the agent might skip it when it shouldn't.

---

#### Question 3: How does Agentic RAG differ from traditional RAG?

**Question**: What are the advantages and potential disadvantages of letting the agent decide when to retrieve information?

**Answer**:

**Traditional RAG**: Query → Retrieve → Generate (always retrieves, fixed pattern)

**Agentic RAG**: Query → Agent decides → Maybe retrieve → Maybe use other tools → Generate

**Advantages:**
- **Efficiency**: Skips retrieval when unnecessary (math questions, time queries)
- **Multi-step reasoning**: Can retrieve, evaluate, then retrieve again with a refined query
- **Tool composition**: Combines RAG with other capabilities (calculations, web scraping)
- **Query reformulation**: Can rephrase queries for better retrieval

**Disadvantages:**
- **Non-determinism**: Agent might wrongly skip retrieval or over-retrieve
- **Cost and latency**: More model calls for decision-making
- **Harder to debug**: Multiple decision points increase complexity
- **Unpredictable paths**: Same query might take different routes

**Key Insight**: Agentic RAG trades predictability for flexibility. Use it when tasks benefit from reasoning; use traditional RAG when you need consistent, auditable pipelines.

---

#### Question 4: What middleware hooks would you use in production?

**Question**: Describe a real-world scenario where middleware would be essential for a production agent.

**Answer**: Consider a customer support agent that can look up orders, issue refunds, and escalate tickets.

| Hook | Timing | Purpose |
|------|--------|---------|
| `token_budget_guard` | `before_model` | Kill runaway agents before draining API budget |
| `pii_scrubber` | `before_model` | GDPR/HIPAA compliance—don't send PII to third-party APIs |
| `sensitive_action_gate` | `after_model` | Human-in-the-loop for refunds/deletions |
| `observability_hook` | `after_model` | Production debugging requires telemetry |

```python
@before_model
def pii_scrubber(state, runtime):
    """Remove PII before sending to model."""
    # Scrub SSN, credit cards, etc.
    return sanitized_state

@after_model
def sensitive_action_gate(state, runtime):
    """Require human approval for refunds over $100."""
    if detected_refund_action and amount > 100:
        pause_for_human_approval()
```

**Key Pattern**:
- `before_model` = Control what goes IN (budget, compliance, context)
- `after_model` = Control what comes OUT (safety gates, logging, routing)

**Key Insight**: `ModelCallLimitMiddleware` is your last line of defense. Even if everything else fails, "max 15 model calls" prevents infinite loops from bankrupting you at 3am.

---

### Notebook Activities & Implementations

#### Activity 1: Custom Firecrawl Web Scraping Tool

**Objective**: Create a custom tool that scrapes websites using Firecrawl and integrates it into the agent.

**Approach**: Used the `@tool` decorator to wrap Firecrawl's scrape API, making web content accessible to the agent.

```python
from firecrawl import Firecrawl

firecrawl = Firecrawl(api_key=os.environ["FIRECRAWL_API_KEY"])

@tool
def scrape_website(url: str) -> str:
    """Scrape a website and return the content."""
    result = firecrawl.scrape(url, formats=["markdown"])

    # Response is a Document object with .markdown attribute
    if hasattr(result, 'markdown') and result.markdown:
        return result.markdown
    return f"No content found for {url}"
```

**Challenges & Solutions:**

1. **Challenge**: `ModuleNotFoundError` despite installing firecrawl-py
   - **Diagnosis**: Project uses `uv` for dependency management, not pip
   - **Solution**: `uv add firecrawl-py` instead of `pip install firecrawl-py`
   - **Learning**: Check how the project manages dependencies (pip vs uv vs poetry)

2. **Challenge**: Firecrawl returned empty content even though dashboard showed success
   - **Diagnosis**: Response structure was `Document` object, not dict
   - **Solution**: Added debug logging to inspect actual response type and structure
   - **Learning**: API response structures vary between versions; always log and inspect

```python
# Debug pattern for unknown response structures
print(f"Type: {type(result)}")
print(f"Keys: {result.keys() if hasattr(result, 'keys') else 'N/A'}")
print(f"Has markdown attr: {hasattr(result, 'markdown')}")
```

**Key Insight**: External APIs often have underdocumented response structures. Build tools defensively with multiple fallback paths.

---

#### Activity 2: Enhanced RAG Tool with Metadata, Reranking, and Citations

**Objective**: Improve the basic RAG retriever with topic filtering, relevance reranking, and structured citations.

**Three Improvements Implemented:**

**1. Metadata Filtering** — Tag chunks by topic for targeted retrieval:

```python
def categorize_chunk(text: str) -> str:
    """Simple keyword-based categorization."""
    text_lower = text.lower()
    if any(word in text_lower for word in ["exercise", "workout", "fitness"]):
        return "fitness"
    elif any(word in text_lower for word in ["sleep", "rest", "insomnia"]):
        return "sleep"
    # ... more categories
    return "general"

docs_with_metadata = [
    Document(
        page_content=chunk,
        metadata={"topic": categorize_chunk(chunk), "chunk_id": i}
    )
    for i, chunk in enumerate(chunks)
]
```

**2. Reranking** — Retrieve more, filter down to best:

```python
# Get 6 results, return top 3 after reranking
base_retriever = enhanced_vector_store.as_retriever(search_kwargs={"k": 6})
results = base_retriever.invoke(query)

# Simple keyword overlap scoring
def relevance_score(doc):
    query_words = set(query.lower().split())
    doc_words = set(doc.page_content.lower().split())
    return len(query_words & doc_words) / max(len(query_words), 1)

scored_results = sorted([(doc, relevance_score(doc)) for doc in results],
                        key=lambda x: x[1], reverse=True)[:3]
```

**3. Source Citations** — Structured metadata in responses:

```python
formatted.append(
    f"[Source {i} | Topic: {topic} | Chunk: {chunk_id} | Relevance: {score:.2f}]\n{doc.page_content}"
)
```

**Why These Matter:**

| Improvement | Problem Solved | Production Benefit |
|-------------|---------------|-------------------|
| Metadata filtering | Irrelevant results in specific queries | User asks about sleep, gets only sleep content |
| Reranking | Embedding similarity ≠ relevance | Keyword overlap catches semantic misses |
| Citations | "Where did this come from?" | LLM can cite sources, users can verify |

**Key Insight**: The retrieve-more-filter-down pattern (6→3) is a production standard. Vector search gets you "in the neighborhood"; reranking picks the best neighbors.

---

### Key Takeaways

1. **The Agent Loop is Autonomous but Bounded**: The LLM decides when to call tools and when to respond, but middleware enforces guardrails. Production agents need both flexibility and hard limits.

2. **Tool Docstrings Are Prompts**: The docstring is what the LLM sees. Write them like you're explaining the tool to a new engineer—clear purpose, when to use it, and how to call it.

3. **Agentic RAG > Traditional RAG for Complex Tasks**: When queries need reasoning, multi-step retrieval, or tool composition, let the agent decide. For simple Q&A, traditional RAG is simpler and more predictable.

4. **Middleware Hooks Follow Input/Output Logic**: `before_model` controls inputs (budget, compliance, sanitization). `after_model` controls outputs (safety gates, logging, routing). Design middleware with this mental model.

5. **uv vs pip Matters**: Modern Python projects use `uv` for dependency management. Installing packages with `pip` when the venv was created with `uv` causes "module not found" errors even though pip shows the package installed.

6. **Debug External APIs Systematically**: Response structures vary between API versions and documentation is often incomplete. Always log `type(result)`, available keys/attributes, and sample content before writing production code.

**Connections to AIE9**:
- Session 2's RAG retriever becomes a tool in Session 3's agent
- Middleware concepts will extend to more complex multi-agent systems
- Qdrant setup carries forward to vector-based agent memory

**Further Learning**:
- Explore LangGraph for custom agent workflows beyond `create_agent`
- Implement cross-encoder reranking (ms-marco-MiniLM) for better relevance
- Build observability with LangSmith traces for production debugging
- Try human-in-the-loop middleware for sensitive actions

---

## Part B: 5-Minute Video Script

**Video Title**: "The Agent Loop: From RAG to Autonomous Agents"

**Target Audience**: Developers building AI applications, familiar with LLMs and basic RAG

**Tone**: Technical, practical, insight-focused

---

### [0:00-1:00] Hook & Context

"In Session 2, we built a RAG pipeline—retrieve documents, pass to LLM, generate response. It works, but it's rigid. What if the user asks a math question? The pipeline still retrieves documents it doesn't need.

Today we're building something smarter: an agent that decides for itself when to retrieve, when to calculate, and when to just answer. This is the shift from pipeline to autonomy—and it's how production AI systems actually work.

We'll cover three things: the agent loop, custom tools, and middleware for production guardrails."

**Speaker Notes**: Start by showing the RAG pipeline from Session 2, then contrast with agent architecture. Emphasize the shift from "fixed steps" to "reasoning".

**Visuals**:
- [SHOW: Traditional RAG diagram: Query → Retrieve → Generate]
- [SHOW: Agent loop diagram: Query → Decide → Tool/Retrieve/Answer → Repeat]

---

### [1:00-2:00] The Agent Loop Explained

"Every agent runs a simple loop. The model receives the current state—messages, tool results, whatever context it has. Then it decides: do I need to call a tool, or can I answer now?

If it calls a tool, the result gets added to the conversation, and we loop back. If it answers, we exit the loop and return to the user.

Here's what makes this powerful: the agent can chain multiple tools. Ask it 'What time is it, and divide 100 by the current hour?' It calls `get_current_time`, extracts the hour, calls `calculate`, then synthesizes the answer. No hardcoded workflow—the model figures out the steps."

**Speaker Notes**: Walk through the agent loop diagram slowly. Show the multi-tool example output from cell-18 where the agent chains time and calculation.

**Visuals**:
- [SHOW: Agent loop diagram with Model → Tool → Model → Answer flow]
- [SHOW: Cell-19 output showing HUMAN → AI → TOOL → AI → TOOL → AI message sequence]
- [HIGHLIGHT: The agent making two tool calls autonomously]

---

### [2:00-3:00] Building Tools with @tool

"Creating tools is straightforward. Use the `@tool` decorator, write a clear docstring, and return a string. But here's the critical part: the docstring is what the LLM sees when deciding which tool to use.

Look at this scrape_website tool. The docstring says 'Scrape a website and return the content. Use this when the user asks for information from a specific website.' That's the agent's instruction manual.

If your docstring is vague—like just 'Scrape URL'—the agent won't know when to use it. Write docstrings like you're explaining the tool to a new engineer."

**Speaker Notes**: Show the Firecrawl tool implementation. Emphasize that docstrings are prompts, not documentation.

**Visuals**:
- [SHOW: Cell-25 with scrape_website tool definition]
- [HIGHLIGHT: The docstring and Args section]
- [SHOW: Cell-26 output where agent successfully scrapes moltbook.com]

---

### [3:00-4:00] Middleware for Production

"Here's where most tutorials stop—but production agents need guardrails. LangChain's middleware hooks let you intercept the agent loop at two points: before the model runs and after.

`before_model` controls inputs—token budgets, PII scrubbing, context modification. `after_model` controls outputs—logging, safety gates, routing decisions.

The `ModelCallLimitMiddleware` is your last line of defense. Set a hard cap of 10 or 15 model calls per run. Even if your agent gets confused and loops, it can't burn through your API budget at 3am."

**Speaker Notes**: Show the logging middleware output, then explain the call limiter configuration. Stress that limits are non-negotiable in production.

**Visuals**:
- [SHOW: Cell-40 with log_before_model and log_after_model]
- [SHOW: Cell-41 with ModelCallLimitMiddleware configuration]
- [SHOW: Cell-44 output showing "[LOG] Model call #1" trace]

---

### [4:00-5:00] Wrap-up: Agentic RAG in Action

"We put it all together with Agentic RAG—the agent decides when to search the knowledge base. Ask about sleep tips? It retrieves. Ask for a calculation? It skips retrieval entirely.

The enhanced version adds metadata filtering, reranking, and citations. Filter by topic, score by keyword overlap, return structured sources. This isn't just cleaner—it gives the LLM signals about confidence and lets users verify answers.

Three takeaways: First, agents are loops with autonomy—they reason about what to do next. Second, tool docstrings are prompts—write them clearly. Third, middleware is mandatory for production—budget limits, safety gates, observability.

Next session, we'll take these agents and make them work together—multi-agent systems. See you there."

**Speaker Notes**: End with the enhanced RAG tool output showing topic filtering and relevance scores. Recap the three main points clearly.

**Visuals**:
- [SHOW: Cell-44 output with wellness agent using RAG selectively]
- [SHOW: Cell-53 enhanced tool with metadata, reranking, citations]
- [SHOW: Summary slide with three takeaways]

---

## Production Notes

- **Code Visibility**: Ensure notebook cells are readable (20pt+ font in recording)
- **Output Clarity**: Pause on agent message traces to show loop behavior
- **Middleware Demo**: Show the logging output in real-time if possible
- **Timing**: Practice the transitions—this script is dense

**Editing Checklist**:
- [ ] Agent loop diagram animated (steps appear sequentially)
- [ ] Code highlights match narration timing
- [ ] Middleware output scrolling is readable
- [ ] Captions added for code sections
- [ ] Audio levels balanced between narration and demo

---

*This learning summary was generated at the end of Session 3 (The Agent Loop) as part of AIE9 learning documentation.*
