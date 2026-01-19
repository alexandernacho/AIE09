# Compare with AIE8 Reference

Find matching assignments or code in the AIE8 (previous cohort) repository and compare approaches.

## Context

- **Working in**: AIE09 (current cohort)
- **Reference**: AIE8 at `/Users/alexandercoenegrachts/Documents/code/AIE8`
- **Goal**: Leverage prior work, avoid duplication, learn from previous implementations

## Instructions

1. **Identify Current Context**: Determine which session/assignment you're working on:
   - Current git branch (e.g., `lesson1-assignment`)
   - Open files or notebooks
   - User's specified focus area

2. **Map to AIE8 Equivalent**: Find the corresponding session in AIE8:

   | AIE09 Session | AIE8 Equivalent |
   |---------------|-----------------|
   | 01_Vibe_Check | 01_Prototyping Best Practices & Vibe Check |
   | (upcoming) | 02_Embeddings_and_RAG |
   | (upcoming) | 03_End-to-End_RAG |
   | (upcoming) | 04_Production_RAG |
   | (upcoming) | 05_Our_First_Agent_with_LangGraph |
   | (upcoming) | 06_Multi_Agent_with_LangGraph |
   | (upcoming) | 07_Synthetic_Data_Generation_and_LangSmith |
   | (upcoming) | 08_Evaluating_RAG_With_Ragas |
   | (upcoming) | 09_Advanced_Retrieval |

3. **Search AIE8 Repository**: Look for relevant code:
   - Check the corresponding session folder
   - Look for completed notebooks, solution patterns
   - Check shared libraries: `aimakerspace/`, `langgraph_agent_lib/`

4. **Compare Approaches**: When you find relevant code, analyze:
   - **Architecture differences**: How does the reference structure the solution?
   - **Library usage**: What LangChain/LangGraph patterns are used?
   - **Key techniques**: Specific strategies and implementations

5. **Highlight Reusable Code**: Identify code that can be adapted:
   - Utility functions and helpers
   - Configuration patterns
   - Prompt templates
   - Graph/chain definitions

## Output Format

```markdown
## Session Context
- **Current (AIE09)**: [session being worked on]
- **Reference (AIE8)**: [matching session/file path]

## Approach Comparison
| Aspect | Your Approach | AIE8 Reference |
|--------|--------------|----------------|
| [aspect] | [description] | [description] |

## Reusable Components from AIE8
- [ ] [component 1]: [path and description]
- [ ] [component 2]: [path and description]

## Key Differences to Note
1. [difference 1]
2. [difference 2]

## Recommendations
- [actionable recommendation for adapting AIE8 code]
```
