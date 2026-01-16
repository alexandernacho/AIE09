# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Workspace Overview

This is a **multi-folder workspace** for the AI Engineering Bootcamp (Cohort 9). The workspace combines three locations:

### 1. AIE09 - Current Course Repository
**Path:** `/Users/alexandercoenegrachts/Documents/code/AIE09`

The main repository for Cohort 9 with course materials, assignments, and documentation.

```
AIE09/
├── 00_AIE_Quicklinks/        # Navigation hub for all course materials
├── 00_Docs/                  # Documentation & prerequisites
│   ├── Prerequisites/
│   │   ├── Initial_Setup/    # 6-step onboarding (Git, repos, branches, APIs)
│   │   └── The_AI_Engineer_Onramp_Cohort_2/  # 4-week prep sessions
│   └── Session_Sheets/       # Session content
└── 01_Vibe_Check/            # Session 1 assignment
```

### 2. AIE8 - Previous Cohort Reference
**Path:** `/Users/alexandercoenegrachts/Documents/code/AIE8`

Previous cohort repository with partially completed assignments. **Reference this for overlap** - many AIE9 assignments have equivalent content in AIE8. Check here before starting new work to avoid duplication.

```
AIE8/
├── 01_Prototyping Best Practices & Vibe Check
├── 02_Embeddings_and_RAG
├── 03_End-to-End_RAG
├── 04_Production_RAG
├── 05_Our_First_Agent_with_LangGraph
├── 06_Multi_Agent_with_LangGraph
├── 07_Synthetic_Data_Generation_and_LangSmith
├── 08_Evaluating_RAG_With_Ragas
├── 09_Advanced_Retrieval
├── 10_Open_DeepResearch
├── 12_OpenAI_Agents_SDK
├── 13_MCP
├── 14_LangGraph_Platform
├── 15_A2A_LangGraph
└── 16_Production_RAG_and_Guardrails
```

### 3. Obsidian Vault - Notes & Content
**Path:** `/Users/alexandercoenegrachts/Library/Mobile Documents/com~apple~CloudDocs/obsidian-vault/AIE9`

Personal knowledge base containing:
- Class notes
- Session transcripts
- Article drafts
- Social media post ideas related to AIE9

## Workflow Guidelines

When working on assignments:
1. **Check AIE8 first** - Look for matching/similar assignments in AIE8 to reference previous work
2. **Use Obsidian vault** - Reference notes and transcripts for context
3. **Complete in AIE09** - Submit all new work in the AIE09 repository

## Commands

### Environment Verification
```bash
# Run the full setup checker (from AIE09 root)
chmod +x 00_Docs/Prerequisites/Initial_Setup/scripts/setup-checker.sh
./00_Docs/Prerequisites/Initial_Setup/scripts/setup-checker.sh
```

### Tech Stack
- Python 3.8+ (3.12 recommended)
- `uv` package manager (preferred over pip)
- OpenAI API key (OPENAI_API_KEY environment variable)
- Optional: Anthropic API key, Docker, Jupyter

## Key Files

- `00_AIE_Quicklinks/README.md` - Master navigation with links to all sessions
- `.cursor/rules/teaching-mode.mdc` - Cursor IDE teaching guidelines (for IDE interactions)

## Assignment Workflow

This repository uses a specific Git workflow for handling course assignments.

### Git Repository Structure

- **Upstream**: AI Makerspace's course materials (read-only, pull only)
- **Origin**: Your fork on GitHub (push your work here)
- **Local**: Your machine where you edit files

### Starting a New Assignment

#### Step 1: Pull New Course Materials

First, ensure you're on main and pull the latest materials from upstream:

```bash
git checkout main
git pull upstream main --allow-unrelated-histories
git push origin main
```

#### Step 2: Create an Assignment Branch

Create a new branch for the assignment work:

```bash
git checkout -b lessonX-assignment
```

Replace `lessonX` with the appropriate lesson number (e.g., `lesson1-assignment`, `lesson2-assignment`).

### Completing an Assignment

#### Step 3: Do the Assignment Work

Make changes, fill in answers, complete the homework activities.

#### Step 4: Stage Your Changes

```bash
git add .
```

Or stage specific files:

```bash
git add path/to/file.md
```

#### Step 5: Commit Your Work

```bash
git commit -m "Completed lesson X assignment"
```

Use descriptive commit messages that explain what was done.

#### Step 6: Push to Your Remote

```bash
git push origin lessonX-assignment
```

### Assignment Locations

| Session | Assignment Location |
|---------|---------------------|
| Session 1 | `01_Vibe_Check/README.md` |

### Key Rules

1. **Never work directly on `main`** - Always create a feature/assignment branch
2. **Only pull from upstream** - Never push to upstream
3. **Push completed work to origin** - This is your GitHub fork
4. **Branch names** - Use format `lessonX-assignment` or similar descriptive names

### Weekly Workflow Summary

```bash
# 1. Get new materials
git checkout main
git pull upstream main --allow-unrelated-histories
git push origin main

# 2. Create assignment branch
git checkout -b lessonX-assignment

# 3. Do work...

# 4. Stage, commit, push
git add .
git commit -m "Completed lesson X assignment"
git push origin lessonX-assignment
```
