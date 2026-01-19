# Today I Learned (TIL)

Generate a quick, shareable "Today I Learned" snippet from current work.

## Instructions

1. **Identify the Learning**:
   - What specific thing did you just learn or figure out?
   - Can be technical, conceptual, or a useful trick
   - Should be concise and immediately useful

2. **Format Options**:

   **Twitter-style TIL** (under 280 chars):
   ```
   TIL: [concise insight]

   [optional: one-line code example or key point]

   #TIL #[relevant_tag]
   ```

   **Slack/Discord-style TIL** (slightly longer):
   ```
   TIL: [insight title]

   [2-3 sentence explanation]

   Example:
   [code snippet or concrete example]
   ```

   **Dev.to/Blog-style TIL** (mini-article):
   ```
   # TIL: [Title]

   ## The Problem
   [What you were trying to do]

   ## The Solution
   [What you learned]

   ## Code
   [snippet]

   ## Why It Matters
   [brief explanation]
   ```

3. **Good TIL Topics from AIE9**:
   - LangChain/LangGraph tricks
   - Python patterns for ML
   - Debugging strategies
   - Performance optimizations
   - API quirks and workarounds
   - Evaluation metrics insights
   - Prompt engineering discoveries

## Output Format

Provide all three versions so user can choose:

```markdown
## Twitter TIL
[280 char version]

## Slack/Discord TIL
[medium version]

## Blog TIL
[full version]
```

## Examples

**Topic: Reranking in RAG**
- Twitter: "TIL: Adding a reranker after initial retrieval can boost RAG accuracy by 20%+ with minimal latency cost. Cohere's rerank API is surprisingly easy to integrate."
- Longer: Explains the why and shows the code

**Topic: LangGraph state management**
- Twitter: "TIL: LangGraph's `add_messages` reducer automatically handles message deduplication. No more manual ID tracking!"
- Longer: Shows before/after code comparison
