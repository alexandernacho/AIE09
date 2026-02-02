# Session Learning Summary & Video Script Generator

Create a comprehensive markdown file documenting session learnings and generate a 5-minute video script.

## Context Sources
- Current session conversation and questions asked
- Jupyter notebook implementations and code
- Assignments completed during the session
- Session topics and learning objectives

## Instructions

1. **Gather Session Information**:
   - What was the session topic/number?
   - What Jupyter notebooks were used?
   - What questions were asked and explored?
   - What assignments were completed?
   - What challenges were encountered and solved?

2. **Part A: Learning Summary** (400-600 words):

   **Section 1: Overview**
   - Session title and number
   - Main learning objectives
   - Key technologies/concepts covered

   **Section 2: Questions Asked & Answers**
   - List each significant question from the conversation
   - Provide the answer/explanation that was developed
   - Include relevant code snippets or examples
   - Explain the "why" behind each concept

   **Section 3: Notebook Activities & Implementations**
   - Break down major sections of the notebook
   - Explain what each section accomplishes
   - Describe the implementation approach
   - Note any challenges and how they were solved
   - Include key learnings and best practices discovered

   **Section 4: Key Takeaways**
   - 4-6 actionable insights
   - Practical applications
   - Connections to other AIE9 sessions
   - Resources for deeper learning

3. **Part B: 5-Minute Video Script** (800-1000 words, ~2 minutes per section):

   **Format Guidelines**:
   - Conversational, engaging tone
   - Break into 5 logical sections (60 seconds each)
   - Include visual cues directly pointing to the code (Jupyter notebook). No other visuals will be used or created.
   - Speaker notes for emphasis and pacing
   - Clear transitions between sections

   **Section Structure**:

   **[0:00-1:00] Hook & Context**
   - Brief context of what will be shown and what we've learned. Not too attention grabbing.
   - Why this matters

   **[1:00-2:00] Concept Explanation**
   - Core concept or technique from the session
   - Real-world application
   - Why it's important

   **[2:00-3:00] Code Walkthrough**
   - Key code from the notebook
   - Explain the approach
   - Show the output/result

   **[3:00-4:00] Live Demo/Example**
   - Show something working
   - Interactive or result-focused
   - Connect back to the concept

   **[4:00-5:00] Wrap-up & Next Steps**
   - Summary of what was learned

## Output File Format

File should be named: `session-X-TITLE.md` (e.g., `session-2-dense-vector-retrieval.md`)

Location: `/Users/alexandercoenegrachts/Documents/code/AIE09/learnings/`

```markdown
# Session X: [Title]

**Date**: [Session Date]
**Status**: [In Progress / Completed]

---

## Part A: Learning Summary

### Overview

[Session title, number, objectives]

### Questions Asked & Answers

#### Question 1: [Question Title]

**Question**: [The specific question]

**Answer**: [Detailed explanation]

\`\`\`python
# Code example if applicable
\`\`\`

**Key Insight**: [Why this matters]

#### Question 2: [Continue pattern...]

### Notebook Activities & Implementations

#### Activity 1: [Activity Name]

**Objective**: [What this section does]

**Approach**: [How it works]

\`\`\`python
# Implementation code
\`\`\`

**Challenges & Solutions**:
- Challenge 1: [Description] → Solution: [How it was resolved]
- Challenge 2: [Description] → Solution: [How it was resolved]

**Learning**: [Key takeaway from this activity]

### Key Takeaways

1. **[Insight 1]**: [Explanation]
2. **[Insight 2]**: [Explanation]
3. **[Insight 3]**: [Explanation]
4. **[Insight 4]**: [Explanation]
5. **[Insight 5]**: [Explanation]

**Connections to AIE9**: [How this relates to other sessions or the bigger picture]

**Further Learning**: [Resources or next steps]

---

## Part B: 5-Minute Video Script

**Video Title**: [Title matching learning summary]

**Target Audience**: Developers learning AI/ML concepts

**Tone**: Educational, engaging, practical

---

### [0:00-1:00] Hook & Context

[Opening line that hooks attention]

**Speaker Notes**: [Delivery notes - pacing, emphasis, etc.]

**Visuals**: [SHOW: title slide] [SHOW: problem statement]

---

### [1:00-2:00] Concept Explanation

[Explanation of core concept]

**Speaker Notes**: [Pause here for emphasis], [Slow down when explaining X]

**Visuals**: [SHOW: diagram or visual explanation] [DEMO: interactive visualization]

---

### [2:00-3:00] Code Walkthrough

[Walk through key code]

**Speaker Notes**: [Explain each line], [Highlight the important part]

**Visuals**: [SHOW: code on screen] [HIGHLIGHT: this function] [SHOW: code output]

---

### [3:00-4:00] Live Demo/Example

[Show something working in action]

**Speaker Notes**: [Narrate what's happening], [Explain the results]

**Visuals**: [RECORD: running the code] [SHOW: output/results] [ANNOTATE: key results]

---

### [4:00-5:00] Wrap-up & Next Steps

[Summarize and motivate next steps]

**Speaker Notes**: [Recap key points], [Emphasize takeaways]

**Visuals**: [SHOW: summary slide] [SHOW: resources] [SHOW: next session preview]


---

*This learning summary was generated at the end of Session X as part of AIE9 learning documentation.*
```

## Tips for Great Summaries

1. **Be Specific**: Reference exact line numbers in notebooks, specific function names, concrete examples
2. **Show Your Work**: Include code snippets that show what was implemented
3. **Explain the Why**: Don't just describe what was done, explain why each decision was made
4. **Connect Concepts**: Show how today's learning builds on previous sessions
5. **Make It Actionable**: Ensure takeaways are things someone could apply to their own projects

## Video Script Tips

1. **Opening Hook**: Start with a problem, question, or surprising fact
2. **Pacing**: Vary sentence length and pause for emphasis
3. **Concrete Examples**: Use specific, relatable examples from the code
4. **Visual Markers**: Be explicit about what should be on screen at each moment
5. **Closing Strong**: End with a clear takeaway and next steps

## File Management

- Save all session learning files in `/learnings/`
- Use consistent naming: `session-X-TITLE.md`
- Include date and status at the top
- Keep backups in git
- Once done copy the file into the relevant `/learnings/` folder in the Obsidian vault using:

```bash
cp /Users/[USERNAME]/Documents/code/AIE09/learnings/session-[NUMBER]-[TITLE]-learnings.md "/Users/[USERNAME]/Library/Mobile Documents/com~apple~CloudDocs/obsidian-vault/AIE9/Session_[NUMBER]_[FORMATTED_TITLE]/learnings/session-[NUMBER]-[TITLE]-learnings.md"
```

**Example**:
```bash
cp /Users/alexandercoenegrachts/Documents/code/AIE09/learnings/session-2-dense-vector-retrieval-learnings.md "/Users/alexandercoenegrachts/Library/Mobile Documents/com~apple~CloudDocs/obsidian-vault/AIE9/Session_2_Dense_Vector_Retrieval/learnings/session-2-dense-vector-retrieval-learnings.md"
```
