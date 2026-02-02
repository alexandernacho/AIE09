# Assignment Starter Checklist

Initialize work on a bootcamp session assignment with proper context and setup.

**Session Number**: $ARGUMENTS

## Pre-Flight Checklist

Execute these steps in order:

### 1. Pull Latest Course Materials
```bash
git checkout main
git pull upstream main --allow-unrelated-histories
git push origin main
```

### 2. Branch Setup
- [ ] Create assignment branch: `git checkout -b lesson$ARGUMENTS-assignment`
- [ ] Verify clean working state

### 3. Check AIE8 for Prior Work
- [ ] Find equivalent session in `/Users/alexandercoenegrachts/Documents/code/AIE8`
- [ ] Look for existing assignment notebook or solutions
- [ ] Identify helper libraries or utilities used
- [ ] Note dependencies and patterns

### 4. Gather Session Context
Run `/session-context $ARGUMENTS` to understand:
- [ ] Main concepts and technologies
- [ ] Prerequisites from earlier sessions
- [ ] Assignment requirements (Build/Ship/Share)
- [ ] Any notes in Obsidian vault

### 5. Environment Setup
- [ ] Navigate to session directory in AIE09
- [ ] Check Python version (3.12 recommended)
- [ ] Install dependencies: `uv sync` or `pip install -r requirements.txt`
- [ ] Verify `.env` file has required API keys:
  - OPENAI_API_KEY (required)
  - ANTHROPIC_API_KEY (optional)

### 6. Assignment Requirements Review
Standard **Build → Ship → Share** submission:
- **Build**: Complete the notebook/code with working implementation
- **Ship**: Record 5-min Loom walkthrough
- **Share**: Social media post with learnings (tag @AIMakerspace)

### 7. Scaffold Working Environment
- [ ] Open assignment notebook/files
- [ ] Identify all TODO/exercise sections
- [ ] Note any "Advanced Build" optional sections
- [ ] Review Obsidian notes for context
- [ ] If no session folder is present in the Obsidian vault (`/Users/alexandercoenegrachts/Library/Mobile Documents/com~apple~CloudDocs/obsidian-vault/AIE9`), do so by adding the following structure
    - /AIE9/ #Obsidian vault location
      ├── /`Session_X_Session_Title`/
      │   ├── /`Pre-work`
      │   ├── /`learnings`/ 

## Output Format

```markdown
## Assignment: Session $ARGUMENTS

### Git Status
- Branch: `lesson$ARGUMENTS-assignment`
- Working directory: [clean/dirty]

### Directories
- AIE09: `[session_dir]/`
- AIE8 Reference: `[equivalent_session]/`

### Environment
- Python: [version]
- Dependencies: [ready/needs setup]

### Assignment Details
- Path: [notebook/file path]
- Sections to complete: [count]
- Advanced sections: [yes/no]

### Prior Work in AIE8
[Summary of existing code that can be leveraged]

### API Keys Status
- [ ] OPENAI_API_KEY
- [ ] [others as needed]

### Obsidian Notes
[Any relevant notes found]

### Ready to Start
[Yes/No + any blockers]
```

## Quick Start Commands

```bash
# Navigate and setup
cd /Users/alexandercoenegrachts/Documents/code/AIE09/[session_directory]
uv sync

# Start Jupyter
jupyter notebook [assignment_notebook].ipynb
```

## Submission Workflow

When complete:
```bash
git add .
git commit -m "Completed lesson $ARGUMENTS assignment"
git push origin lesson$ARGUMENTS-assignment
```

Then:
1. Record Loom video
2. Create social post (`/twitter-thread` or `/linkedin-post`)
3. Submit via course form
