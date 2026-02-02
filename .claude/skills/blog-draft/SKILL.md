---
name: blog-draft
description: Blog Draft Generator - Create longer-form article drafts based on AIE9 session content, aligned with Product Coder business context
---

# Blog Draft Generator

Create longer-form article drafts based on AIE9 session content or completed assignments, written for Product Managers transforming into Product Engineers.

## Usage

```
/blog-draft [session_number]
```

- **With session number:** Generate blog based on that session (e.g., `/blog-draft 2`)
- **Without argument:** Ask which session to write about

## MANDATORY: Business Context

**ALWAYS write content aligned with the Product Coder business context below.**

<business-context>
# Business Focus: Product Coder

## Core Identity

**From Product Manager to Product Engineer.**
*Stop being half a product person.*

I champion the transformation of Product Managers into Product Engineers through
vibe coding with AI. My worldview: **Product thinking + technical execution = unstoppable.**

## The Core Thesis

The job of Product Manager will disappear — at least in its current form. PMs without
technical skills will lose out to their more technically savvy counterparts. The gap
between those who can ship and those who can't is widening every month.

But here's what nobody talks about: **the PMs who can harness AI will transform into
something new.** They'll bundle business skills with technical skills. They'll become
Product Engineers. And they'll be unstoppable.

## The Vibe Coding Trajectory

This is crucial for convincing skeptics:

- **2024:** We were skeptical. Could you really build production software by talking to an AI? It felt like a toy, a novelty, maybe useful for prototypes at best.
- **2025:** We started to commit. The tools got better. The workflows matured. People started shipping real products — not demos, not mockups, *real products*.
- **2026:** We fully embrace it.

**The key insight:** Vibe coding will never be as bad as it is today. Models get better
every month. What feels unreliable now will be orders of magnitude more capable in a year.

## What Vibe Coding IS and ISN'T

**This is NOT:**
- Toy projects or weekend hacks
- Prompting ChatGPT for code snippets
- "No-code" tools that hit walls fast
- Trying to become a traditional developer

**This IS:**
- Production-quality, shippable code
- Real engineering practices (git, testing, CI/CD)
- AI-assisted development that scales
- The bridge from "I have an idea" to "I shipped it"
- Becoming complete — not becoming a developer

## Target Audience

### Primary Persona: The Ambitious PM
- 3-7 years of PM experience
- Has always been "technically curious" but never had the right path
- Sees AI as an opportunity, not a threat
- Frustrated by dependency on engineering for validation
- Wants to stand out, get promoted, or eventually start something

### Secondary Persona: The Technical-Adjacent PM
- Has some technical background (maybe studied CS, worked adjacent to engineering)
- Never fully committed to coding
- Knows enough to be dangerous, wants to be proficient
- Sees vibe coding as the bridge they've been waiting for

### What They Want (lead with this)
- To be the PM everyone wants on their team
- To ship their own ideas without dependencies
- To have technical credibility in the room
- To future-proof their career

### What They Fear (acknowledge, but don't lead with fear)
- Becoming irrelevant as AI changes product work
- Being stuck in "spec and wait" mode forever
- Watching less experienced PMs leapfrog them because they can build

## Content Pillars (ONLY write about these)

1. **The PM to PE Transformation**
   - The role is splitting in two: PMs who can ship vs. PMs who can only spec
   - Mindset shifts required to become complete
   - Career implications and opportunities
   - Why this is about empowerment, not survival

2. **Vibe Coding as the Enabler**
   - The 2024→2025→2026 trajectory
   - Why skeptics are wrong (the curve only goes up)
   - How AI-assisted coding lowers the barrier to building
   - Beyond toys, into production

3. **Practical Building for Product People**
   - How to go from idea to working code
   - Tool recommendations and workflows
   - When AI code is good enough vs needs review
   - Integrating AI-built features into real products

4. **Stop Waiting on Engineering**
   - Build prototypes, ship MVPs, prove ideas by building them
   - Real examples of PMs who ship (including my own builds)
   - Before/after transformation stories
   - Specific tools and techniques used

## Key Messages to Reinforce

1. **The role is splitting in two.** PMs who can ship vs. PMs who can only spec. The gap is widening every month.

2. **Vibe coding is the enabler.** It will never be as bad as it is today — models only get better.

3. **This isn't about becoming a developer.** It's about becoming complete. Product thinking + technical execution.

4. **Stop waiting on engineering.** Build prototypes, ship MVPs, prove your ideas work by building them.

## Tone & Voice

- Confident but not arrogant
- Direct, clear, no fluff
- Speaks from experience
- Slightly provocative, challenges status quo
- Practical over theoretical
- **Lead with empowerment, not fear**

**Framing:**
- The role is evolving — you can watch it change around you, or lead the change yourself
- This is an opportunity for ambitious PMs, not a lifeboat for scared ones
- Vibe coding is the bridge that was always missing

## Credibility & Proof Points to Reference

When establishing authority:
- Worked as PM, worked as dev, worked with devs
- Led teams and shipped products end-to-end
- Worked with Bloomberg, Reuters, Ledger
- Built MCP servers now used by enterprise companies
- Everything shipped in the past year is vibe coded
- The Product Coder platform itself is vibe coded
</business-context>

## MANDATORY: Content Sources

For EVERY blog draft, gather source material from BOTH locations:

### 1. Obsidian Vault (Notes & Transcripts)
```
/Users/alexandercoenegrachts/Library/Mobile Documents/com~apple~CloudDocs/obsidian-vault/AIE9
```

Structure:
- `Session_X_Topic/` - Session-specific notes
- `Session_X_Topic/Pre-work/` - Pre-reading materials
- `Transcripts/` - Session transcripts

### 2. Session Sheets (Official Materials)
```
/Users/alexandercoenegrachts/Documents/code/AIE09/00_Docs/Session_Sheets
```

Contains official session documentation with learning objectives and key concepts.

### 3. Code & Notebooks (if applicable)
Check the relevant session folder in the main repo for code examples:
```
/Users/alexandercoenegrachts/Documents/code/AIE09
```

## Process

1. **Internalize Business Context** - Read the embedded context above thoroughly

2. **Gather Source Material** from all locations above for the specified session

3. **Determine Article Angle** - Connect session content to Product Coder themes:
   - How does this enable PMs to build?
   - What can a PM do with this knowledge that they couldn't before?
   - How does this reduce dependency on engineering?

4. **Draft the Article** following the structure below

5. **MANDATORY: Run /humanize** on the completed draft to remove AI patterns

## Article Structure

**Title**: Clear, searchable, includes key technology
- Pattern: "How to [achieve X] with [technology]"
- Pattern: "[Technology] Deep Dive: [specific topic]"
- Pattern: "Building [thing] with [stack]: A PM's Guide"

**Introduction** (150-200 words):
- Hook: Why this matters for PMs learning to build
- Context: Brief background connecting to vibe coding
- Promise: What reader will learn and be able to do

**Background/Prerequisites** (100-150 words):
- Required knowledge (minimal - PMs can follow)
- Tools/setup needed
- Links to resources

**Main Content** (800-1200 words):
- Logical progression of concepts
- Code examples with clear explanations (assume PM audience)
- Diagrams or architecture overviews (describe for later creation)
- Common pitfalls and solutions
- **Frame everything through "what can you build with this"**

**Key Takeaways** (bulleted list):
- 3-5 main points
- Actionable insights for PMs

**Conclusion** (100-150 words):
- Summary connecting to PM→PE transformation
- Next steps for reader
- Call to action

**Resources**:
- Links to code/repo
- Further reading
- AI Makerspace mention

## Output Format

```markdown
# [Title]

*[Subtitle connecting to Product Coder theme]*

## Introduction

[Hook connecting to PM transformation and context]

## Prerequisites

- [Prereq 1 - keep accessible for PMs]
- [Prereq 2]

## [Main Section 1]

[Content with code examples, explained for product people]

```python
# Example code with clear comments
```

## [Main Section 2]

[Continue pattern - always connect back to "what you can build"]

## Key Takeaways

1. [Takeaway 1 - actionable for PMs]
2. [Takeaway 2]
3. [Takeaway 3]

## Conclusion

[Wrap up connecting to Product Engineer journey and CTA]

## Resources

- [AI Makerspace Bootcamp](https://aimakerspace.io/)
- [GitHub Repo](link)
- [Further reading](links)

---
*This post is part of my journey from Product Manager to Product Engineer, learning to build with AI through AI Makerspace's AI Engineering Bootcamp (AIE9).*
```

## MANDATORY: Final Step

After generating the draft, ALWAYS run:
```
/humanize [output_file_path]
```

This removes AI patterns and makes the content sound natural and human-written.

## Platform-Specific Notes

**Dev.to**: Add front matter with tags
**Medium**: Optimize for reading time (5-7 min ideal)
**Hashnode**: Can include code playgrounds
**Personal blog**: Full creative freedom

## Suggested Topics by Session

| Session | Potential Blog Topics (PM Angle) |
|---------|----------------------------------|
| 01 | "The Vibe Check: Why PMs Should Learn AI Engineering" |
| 02 | "Dense Vector Retrieval: Building Search That Actually Works" |
| 03 | "Building Your First RAG System: A PM's Practical Guide" |
| 04 | "LCEL vs Traditional Chains: Which Should You Use?" |
| 05-06 | "From Single Agent to Multi-Agent: When and Why" |
| 07-08 | "Evaluating RAG: Metrics That Matter for Product" |
| 09 | "Advanced Retrieval: Making Your AI Actually Find Things" |
| 12-13 | "OpenAI Agents SDK vs LangGraph: A Product Perspective" |
| 14-16 | "Taking LangGraph to Production: What I Learned" |
