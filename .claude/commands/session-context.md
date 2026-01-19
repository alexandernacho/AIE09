# Session Context Gatherer

Pull relevant context for a specific bootcamp session from all available sources.

**Session Number**: $ARGUMENTS

## Sources

1. **AIE09 Repository**: `/Users/alexandercoenegrachts/Documents/code/AIE09`
2. **AIE8 Reference**: `/Users/alexandercoenegrachts/Documents/code/AIE8`
3. **Obsidian Vault**: `/Users/alexandercoenegrachts/Library/Mobile Documents/com~apple~CloudDocs/obsidian-vault/AIE9`

## Instructions

1. **Parse Session Number**: Extract from arguments (e.g., "1", "01", "session 1" all map to Session 01)

2. **Gather from AIE09 Repository**:
   - Read the session's README.md for overview and links
   - Identify notebooks and their structure
   - Check `00_AIE_Quicklinks/README.md` for session links
   - Note assignment requirements

3. **Cross-Reference AIE8**:
   - Find equivalent session in AIE8 (see mapping below)
   - Check what code/solutions exist there
   - Note any prior work done

4. **Search Obsidian Vault**:
   - Look for session-specific notes in the AIE9 folder
   - Find transcript excerpts or key takeaways
   - Check for article drafts or social media ideas
   - Common note patterns: `Session X Notes.md`, `AIE9-SessionX.md`

## Session Directory Mapping

| Session | AIE09 Directory | AIE8 Equivalent |
|---------|-----------------|-----------------|
| 01 | `01_Vibe_Check` | `01_Prototyping Best Practices & Vibe Check` |
| 02 | TBD | `02_Embeddings_and_RAG` |
| 03 | TBD | `03_End-to-End_RAG` |
| 04 | TBD | `04_Production_RAG` |
| 05 | TBD | `05_Our_First_Agent_with_LangGraph` |
| 06 | TBD | `06_Multi_Agent_with_LangGraph` |
| 07 | TBD | `07_Synthetic_Data_Generation_and_LangSmith` |
| 08 | TBD | `08_Evaluating_RAG_With_Ragas` |
| 09 | TBD | `09_Advanced_Retrieval` |
| 10 | TBD | `10_Open_DeepResearch` |
| 12 | TBD | `12_OpenAI_Agents_SDK` |
| 13 | TBD | `13_MCP` |
| 14 | TBD | `14_LangGraph_Platform` |
| 15 | TBD | `15_A2A_LangGraph` |
| 16 | TBD | `16_Production_RAG_and_Guardrails` |

## Output Format

```markdown
## Session $ARGUMENTS Overview

### Topic & Focus
[Main topic and learning objectives]

### Key Concepts
- [Concept 1]
- [Concept 2]

### Technologies Used
- [Tech 1]: [purpose]

### AIE09 Materials
- **Directory**: [path]
- **Assignment**: [path if exists]
- **Status**: [not started/in progress/complete]

### AIE8 Reference
- **Equivalent Session**: [path]
- **Prior Work Available**: [yes/no + description]
- **Reusable Code**: [list]

### Obsidian Notes Found
- [Note 1]: [summary]
- [Note 2]: [summary]

### Session Links
- Notion Sheet: [link from quicklinks]
- Recording: [link]
- Slides: [link]

### Connection to Other Sessions
- **Builds on**: [prior sessions]
- **Leads to**: [future sessions]
```
