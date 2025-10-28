# ⚡ Flight Plan Commands

**Version:** 1.0  
**Last Updated:** 2025-10-25

---

## Overview

**Initial Setup (First Time):**
```
"flight-plan init"         ← Preview project structure
"flight-plan init apply"   ← Generate projects
```

**Ongoing Work:**
```
"flight-plan status"       ← Check progress, get guidance
"flight-plan sync"         ← Sync solution PRD changes to projects
"flight-plan prd refresh"  ← Update tracking after manual PRD edits
```

AI interprets these naturally and performs the actions.

---

## What is Spec-Kit?

**[Spec-Kit](https://github.com/github/spec-kit)** is GitHub's toolkit for Spec-Driven Development.

**Flight Plan vs Spec-Kit:**
- **Flight Plan** → Project-level (8 phases, PRDs, overall progress)
- **Spec-Kit** → Feature-level (individual features, `/speckit.spec`, `/speckit.implement`)

**Integration:**
Flight Plan provides context (phase, standards) → Spec-Kit uses it for feature development.

**Optional:** Run `flight-plan status` in any project and AI will ask if you want SpecKit.

**Learn more:** [github.com/github/spec-kit](https://github.com/github/spec-kit)

---

## 🛡️ Safety & Validation

**Every Flight Plan command validates prerequisites BEFORE proceeding.**

### The Problem
Without validation, AI might:
- ❌ Use examples/ folder as your project template
- ❌ Invent content when files are missing
- ❌ Generate structure based on assumptions

### The Solution
**All commands follow this pattern:**

```
1. Check prerequisites (files exist, context valid)
2. If missing → STOP and tell user what's needed
3. If present → Proceed with command
```

**NEVER:**
- Use examples/ as source material
- Generate without required files
- Assume or invent missing information

**ALWAYS:**
- Validate files exist first
- Read ONLY user's files (not examples)
- Ask user if essential information missing

---

## `flight-plan help`

**Works from anywhere - detects context and shows appropriate commands.**

**What it does:**
1. Checks current directory
2. Detects if solution-level, project-level, or wrong location
3. Shows relevant commands WITHOUT reading files

**Example outputs:**

**From project directory:**
```
📋 Flight Plan - Project Commands

You're in a project directory.

Available commands:
• flight-plan status           - Show status + guidance
• flight-plan prd refresh      - Preview tracking updates
• flight-plan prd refresh apply - Apply tracking updates  
• flight-plan note [text]      - Add activity note

Tip: Run "flight-plan status" for guidance on what to do next.

See full docs: ../flight-plan-solution/FLIGHT-PLAN-COMMANDS.md
```

**From solution directory:**
```
📋 Flight Plan - Solution Commands

You're in the flight-plan-solution directory.

Available commands:
• flight-plan init             - Preview project structure
• flight-plan init apply       - Generate projects
• flight-plan status           - Show all projects' progress
• flight-plan sync             - Preview PRD changes
• flight-plan sync apply       - Apply PRD changes

Tip: Start with "flight-plan init" to preview your solution.

See full docs: ./FLIGHT-PLAN-COMMANDS.md
```

**From wrong location:**
```
❌ Not in a Flight Plan directory

This command works in:
- flight-plan-solution/ directory (solution-level)
- A project directory (project-level)

Current directory doesn't have:
- solution-prd-v*.md (solution-level marker)
- project-prd.md (project-level marker)

Navigate to the correct directory first.
```

---

## Command Reference

### Solution-Level Commands
(Context: directory with `solution-prd-v*.md`)

| Command | Purpose |
|---------|---------|
| `flight-plan help` | Show available commands |
| `flight-plan init` | Preview project structure from PRD |
| `flight-plan init apply` | Generate projects and tracking |
| `flight-plan status` | Show all projects' progress |
| `flight-plan sync` | Preview PRD changes |
| `flight-plan sync apply` | Apply PRD changes to projects |

**Note:** `init` is the first command you run. Others are for ongoing work.

### Project-Level Commands
(Context: directory with `project-prd.md`)

| Command | Purpose |
|---------|---------|
| `flight-plan help` | Show available commands |
| `flight-plan status` | Show status + guidance on what to do next |
| `flight-plan prd refresh` | Preview tracking changes |
| `flight-plan prd refresh apply` | Apply tracking updates |
| `flight-plan note [text]` | Add activity note |

**Note:** 
- Phase transitions happen naturally via conversation, not commands
- Tests run automatically during `flight-plan status` in build phases (5-6)

---

## Command Pattern: Preview → Apply

**Major Flight Plan commands use explicit `apply` subcommand:**

**Preview (default):**
- `flight-plan init` - Shows project structure
- `flight-plan sync` - Shows what will change
- `flight-plan prd refresh` - Shows tracking updates

**Apply (execute):**
- `flight-plan init apply` - Actually creates projects
- `flight-plan sync apply` - Actually updates files
- `flight-plan prd refresh apply` - Actually updates tracking

**Why this pattern?**
- ✅ Preview is safe by default (no changes)
- ✅ No ambiguity about when changes happen
- ✅ No prompts or waiting - you decide next command
- ✅ Natural conversation between preview and apply

### Example

```
You: "flight-plan sync"

AI: [Shows detailed preview with changes]
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    SOLUTION-LEVEL CHANGES:
      + New MCP server: GitHub MCP
    
    PROJECT CHANGES:
    • backend-api:
      ~ Database: PostgreSQL → MongoDB
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# User can discuss, ask questions in separate messages

You: "What will happen to existing database data?"

AI: [Explains migration considerations]

You: "flight-plan sync apply"

AI: ✅ Updated backend-api/docs/project-prd.md
    ✅ Updated backend-api/memory/constitution.md
    ...
```

**This applies to:**
- `flight-plan init` / `flight-plan init apply`
- `flight-plan sync` / `flight-plan sync apply`
- `flight-plan prd refresh` / `flight-plan prd refresh apply`

---

## PRD Auto-Revision System

**Flight Plan automatically versions PRDs when you resolve questions during preview/sync.**

### Version Naming Convention

**User creates major versions:**
- `solution-prd-v1.md` - Initial PRD (v1.0 implicit)
- `solution-prd-v2.md` - Major update (v2.0 implicit)
- `solution-prd-v3.md` - Another major update (v3.0 implicit)

**Flight Plan creates minor versions (auto-revisions):**
- `solution-prd-v1-1.md` - v1.1 (resolved questions from v1.0)
- `solution-prd-v1-2.md` - v1.2 (more resolutions)
- `solution-prd-v2-1.md` - v2.1 (resolved questions from v2.0)

### Auto-Revision Header

Every auto-revision includes metadata:
```markdown
<!-- Flight Plan Auto-Revision -->
<!-- Version: 1.1 -->
<!-- Base: solution-prd-v1.md -->
<!-- Resolved: question-1 (Database choice), question-2 (Auth strategy) -->
<!-- Date: 2025-10-27T15:30:00Z -->

# Task Manager Solution
[Rest of PRD with resolved questions removed/updated]
```

### Version Detection

Flight Plan automatically detects the latest PRD:
1. Lists all `solution-prd-v*.md` files
2. Parses versions (v1.md = 1.0, v1-1.md = 1.1, v2.md = 2.0, v2-1.md = 2.1)
3. Finds highest major version
4. Within that major, finds highest minor
5. **Uses that file**

**Examples:**
```
Files: v1.md, v1-1.md, v1-2.md
→ Uses: v1-2.md (latest in v1.x series)

Files: v1.md, v1-1.md, v2.md
→ Uses: v2.md (latest major version)

Files: v1.md, v1-1.md, v2.md, v2-1.md, v2-2.md
→ Uses: v2-2.md (latest in v2.x series)
```

### When Auto-Revisions are Created

**During `flight-plan init apply`:**
- If you resolved questions during preview
- Creates next minor version
- Projects generated using auto-revision

**During `flight-plan sync apply`:**
- If you resolved new questions during sync preview
- Creates next minor version
- Projects updated using auto-revision

### Git Workflow

**Flight Plan never runs git commands. You control all commits.**

Recommended workflow:
```bash
# After Flight Plan creates auto-revision
git add solution-prd-v1-1.md
git commit -m "PRD v1.1: Resolved database and auth questions"

# Or commit with generated projects
git add .
git commit -m "Initial project setup with v1.1 resolutions"
```

### Cleanup Old Versions

**You can safely delete old PRD files anytime.**

Flight Plan only looks at what files currently exist - it doesn't track history or expect specific files.

Examples:
```bash
# Keep only latest in each major version
rm solution-prd-v1.md solution-prd-v1-1.md
# v1-2.md still works fine

# Keep only current major version
rm solution-prd-v1*.md
# v2-1.md still works fine

# Keep only latest overall
rm solution-prd-v1*.md solution-prd-v2.md
# v2-1.md still works fine
```

**Flight Plan will:**
- Always detect the highest version from existing files
- Never complain about missing older versions
- Work with any subset of PRD files

---

## Solution-Level Commands

### Context Detection

**How AI knows this is solution-level:**
1. Check current directory for `solution-prd-v*.md`
2. Check for `framework/` subdirectory
3. Check for `framework/ai-refs/` directory
4. If found: Solution-level context

---

### `flight-plan help`

**Context:** Any (works everywhere)

**Purpose:** Show available commands for current context

**Prerequisites Check:**
NONE - This command never tries to read files, just checks what exists.

**What it does:**
1. Checks current directory for markers:
   - `solution-prd-v*.md` → Solution-level
   - `project-prd.md` → Project-level
   - Neither → Wrong location
2. Shows appropriate command list
3. Provides tip for next action

**Example:**
```bash
cd task-manager-ui/

"flight-plan help"

📋 Flight Plan - Project Commands

You're in a project directory.

Available commands:
• flight-plan status           - Show status + guidance
• flight-plan prd refresh      - Preview tracking updates
• flight-plan prd refresh apply - Apply tracking updates  
• flight-plan note [text]      - Add activity note

Tip: Run "flight-plan status" for guidance on what to do next.

See full docs: ../flight-plan-solution/FLIGHT-PLAN-COMMANDS.md
```

**Key Point:** This command NEVER reads file contents, only checks if marker files exist. No errors if files are missing - it tells you that's the problem.

---

### `flight-plan status`

**Context:** Solution-level (has `solution-prd-v*.md`)

**Purpose:** Show progress across all projects

**Prerequisites Check:**
Before proceeding, verify you're NOT running from the Flight Plan repository:

**Step 1: Check if this is the Flight Plan REPO (wrong location)**
```bash
pwd
ls
```

Signs you're in the REPO (not a solution):
- Directory name is "FlightPlan" (not "flight-plan-solution")
- Contains `test-case/` directory
- Contains `.git/` directory  
- Contains `examples/` directory

If ANY of these are true:
```
❌ Cannot show status: Running from Flight Plan repository!

You need to set up a solution directory first.

Current location: [show pwd]

You need to:
1. Create a solution directory somewhere OUTSIDE this repo
2. Copy Flight Plan AS "flight-plan-solution" into your solution
3. Add your PRD there
4. Run "flight-plan status" from that flight-plan-solution/ directory

See FLIGHT-PLAN-INIT.md for detailed setup.
```

**Step 2: Verify prerequisites**
1. ✅ `solution-prd-v*.md` exists in current directory
2. ✅ At least one project exists in parent directory (`../`)

If PRD missing:
```
❌ Cannot show status: No solution PRD found

Required: solution-prd-v1.md (or higher version)
This command works in flight-plan-solution/ directory.
```

If no projects exist:
```
❌ Cannot show status: No projects found

Run "flight-plan init apply" first to generate projects.
```

**What it does:**
1. Reads all project `.flight-plan/current.md` files
2. Shows current phase per project
3. Lists blockers
4. Shows overall solution health

**Example:**
```bash
cd flight-plan-solution/

"flight-plan status"

📊 MyApp Status (solution-prd-v1)

✅ backend-api      Phase 5 (Build Code)      - In progress
⚠️  frontend        Phase 3 (Design)          - 2 blockers
✅ notifications    Phase 1 (Define Mission)  - Starting

Overall: 3 projects, 2 blockers
```

---

### `flight-plan sync`

**Context:** Solution-level (has `solution-prd-v*.md`)

**Purpose:** Sync solution PRD changes to project PRDs

**Usage:**
- `flight-plan sync` - Preview changes only
- `flight-plan sync apply` - Apply changes to projects

**Prerequisites Check:**
Before proceeding, verify:
1. ✅ Multiple PRD versions exist (`solution-prd-v1.md`, `solution-prd-v2.md`, etc.)
2. ✅ At least one project exists in parent directory (`../`)

If missing:
```
❌ Cannot sync: Need at least 2 PRD versions

Current: solution-prd-v1.md only
Needed: solution-prd-v2.md (or higher)

To sync:
1. Create new PRD version (e.g., solution-prd-v2.md)
2. Run "flight-plan sync" to preview changes
3. Run "flight-plan sync apply" to update projects
```

**What preview does:**
1. Detects latest `solution-prd-v*.md` (v2, v3, etc.)
2. Compares with previous version
3. Shows changes per project
4. Detects Spec-Kit per project
5. Lists files that will be updated

**What apply does:**
1. Creates auto-revision PRD if resolutions were staged (e.g., v2-1.md)
2. Updates `docs/project-prd.md` files to new version
3. Updates `docs/project-rules.md` if solution rules changed
4. Updates `memory/constitution.md` if Spec-Kit enabled
5. Updates `solution-rules.md` if needed
6. **Appends to `ai-refs/solution-overview.md` PRD version history:**
   - Logs the sync operation
   - Records which PRD version was synced to
   - Notes which projects were updated
   - Lists resolved questions if auto-revision created

**Example:**
```bash
cd flight-plan-solution/
vim solution-prd-v2.md       # Create new version

"flight-plan sync"

📋 Sync Preview (v1 → v2)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SOLUTION-LEVEL CHANGES:
  + New MCP server: GitHub MCP (for issue tracking)
  ~ Updated: Test coverage standard 70% → 80%

PROJECT CHANGES:

• backend-api:
  ~ Database: PostgreSQL → MongoDB
  ~ Updated quality standard (from solution)
  → Spec-Kit: Detected (will update constitution.md)

• frontend:
  ✓ No changes

• notifications:
  + Added open question: "Email provider?"
  + Updated quality standard (from solution)
  → Spec-Kit: Not enabled

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Will update:
✓ solution-rules.md (add GitHub MCP, update standards)
✓ backend-api/docs/project-prd.md
✓ backend-api/docs/project-rules.md
✓ backend-api/memory/constitution.md (tech stack)
✓ notifications/docs/project-prd.md
✓ notifications/docs/project-rules.md
✓ ai-refs/solution-overview.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# User can discuss, ask questions

"flight-plan sync apply"

✅ Updated: solution-rules.md
✅ Updated: backend-api/docs/project-prd.md
✅ Updated: backend-api/docs/project-rules.md
✅ Updated: backend-api/memory/constitution.md
✅ Updated: notifications/docs/project-prd.md
✅ Updated: notifications/docs/project-rules.md
✅ Updated: ai-refs/solution-overview.md

Sync complete. All projects now at v2.

📝 ai-refs/solution-overview.md now shows:

## PRD Version History

### v2.0 (2025-10-27 16:45)
- **Action:** Synced projects from v1 to v2
- **Base PRD:** solution-prd-v2.md (user-created major version)
- **Updated Projects:** backend-api, notifications
- **Notes:** Added GitHub MCP, updated test coverage to 80%

### v1.1 (2025-10-27 15:30)
- **Action:** Initial project generation
- **Base PRD:** solution-prd-v1.md → solution-prd-v1-1.md
- **Generated Projects:** backend-api, frontend, notifications
- **Notes:** Resolved question-1 (Database), question-2 (Auth)
```

**Important:** This command does NOT run Git commands. Git is the user's responsibility.

---

## Project-Level Commands

### Context Detection

**How AI knows this is project-level:**
1. Check current directory for `docs/project-prd.md`
2. Check for `.flight-plan/` subdirectory
3. Check for `docs/project-rules.md`
4. If found: Project-level context
4. If found: Project-level context

---

### `flight-plan status`

**Context:** Project-level (has `project-prd.md`)

**Purpose:** Show this project's current status

**Prerequisites Check:**

**Step 1: Check if running from Flight Plan repository (WRONG)**
```bash
pwd
ls
```

Signs you're in the Flight Plan REPO (not a project):
- Directory name is "FlightPlan" (repo name)
- Contains `test-case/` directory
- Contains `.git/` directory
- Contains `examples/` directory
- Contains `GENERATOR.md` file

If ANY of these are true:
```
❌ Cannot show status: You're in the Flight Plan repository!

Current location: [show pwd]

This command works in a PROJECT directory, not the Flight Plan repo.

You need to:
1. Set up a solution directory with flight-plan-solution/ 
2. Run "flight-plan init apply" to generate projects
3. Navigate to a project directory
4. Run "flight-plan status" from THERE

See FLIGHT-PLAN-INIT.md for setup instructions.
```

**STOP HERE if in Flight Plan repo.**

---

**Step 2: Verify project files exist**

Before proceeding, verify:
1. ✅ `project-prd.md` exists in current directory
2. ✅ `.flight-plan/current.md` exists
3. ✅ `.flight-plan/config.json` exists

If missing:
```
❌ Cannot show status: Missing project files

This command works in a project directory.

Expected files:
- project-prd.md (project requirements)
- .flight-plan/current.md (progress tracking)
- .flight-plan/config.json (project config)

Current directory: [show actual path]

If starting fresh, use "flight-plan init apply" from your solution directory first.
```

**What it does:**
1. Reads `.flight-plan/current.md` from current directory
2. Reads `.flight-plan/config.json` from current directory
3. Shows current phase, activity, blockers
4. Shows PRD sync status
5. If build phase (5-6): Runs tests automatically
6. If SpecKit not prompted: Asks about SpecKit integration

**Example:**
```bash
cd backend-api/

"flight-plan status"

📊 backend-api Status

Phase: 5 - Build Code
Activity: Implementing user authentication
PRD: v2 (synced 2025-10-25)

Open Items:
- Decide caching strategy
- Review API contracts with frontend

Recent Notes:
- 2025-10-25: Started auth module
- 2025-10-24: Database migration complete
```

---

### `flight-plan prd refresh`

**Context:** Project-level (has `docs/project-prd.md`)

**Purpose:** Update tracking after manual PRD edits

**Usage:**
- `flight-plan prd refresh` - Preview changes only
- `flight-plan prd refresh apply` - Apply tracking updates

**Prerequisites Check:**

**Step 1: Check if running from Flight Plan repository (WRONG)**

Signs you're in the Flight Plan REPO:
- Contains `test-case/`, `.git/`, `examples/`, or `GENERATOR.md`

If in repo:
```
❌ Cannot refresh: You're in the Flight Plan repository!

This command works in a PROJECT directory.
Navigate to a project, then run "flight-plan prd refresh".
```

**STOP HERE if in Flight Plan repo.**

---

**Step 2: Verify project files**

Before proceeding, verify:
1. ✅ `project-prd.md` exists in current directory
2. ✅ `.flight-plan/current.md` exists

If missing:
```
❌ Cannot refresh: Missing project files

This command works in a project directory.

Expected files:
- project-prd.md (project requirements)
- .flight-plan/current.md (progress tracking)

If starting fresh, use "flight-plan init" from flight-plan-solution/ first.
```

**What preview does:**
1. Reads current `project-prd.md`
2. Compares with `.flight-plan/current.md` state
3. Detects Spec-Kit if enabled (checks for `memory/constitution.md`)
4. Shows what changed
5. Lists files that will be updated

**What apply does:**
1. Updates `.flight-plan/current.md` (open items, notes)
2. Updates `memory/constitution.md` if Spec-Kit enabled and tech stack changed
3. Logs changes with timestamp

**Example:**
```bash
cd backend-api/
vim project-prd.md           # Edit PRD manually

"flight-plan prd refresh"

📋 Refresh Preview
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Detected changes in project-prd.md:

OPEN QUESTIONS:
  + Added: "question-3: Caching strategy?"
  - Removed: "question-1: Database choice" (RESOLVED)

TECH STACK:
  + Added: Redis (caching layer)
  
QUALITY STANDARDS:
  ~ Changed: Test coverage 70% → 80%

CONSTRAINTS:
  + Added: Must support 1000 req/min

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SPEC-KIT: Detected
  Features in specs/: 3
  - specs/001-user-auth/
  - specs/002-profile-mgmt/
  - specs/003-payments/
  → Will update memory/constitution.md (tech stack changed)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Will update:
✓ .flight-plan/current.md (open items, notes)
✓ memory/constitution.md (tech stack: Redis)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# User can discuss, ask questions

"flight-plan prd refresh apply"

✅ Updated: .flight-plan/current.md
   - Added open item: question-3
   - Removed blocker: question-1 (resolved)
   - Added note: "Redis added for caching"
✅ Updated: memory/constitution.md
   - Tech stack: Added Redis
✅ Logged: PRD refresh at 2025-10-25 16:30 UTC

Refresh complete.
```

**Use this after:**
- Manually editing project-prd.md
- Resolving open questions in PRD
- Updating requirements or tech stack

**Important:** This command does NOT run Git commands.

---

### Phase Transitions (Natural, via `flight-plan next`)

**Users don't manually manage phases.** The AI guides phase transitions naturally.

**How it works:**

1. **User runs `flight-plan next` regularly**
2. **AI checks if current phase objectives are complete**
3. **When phase is complete, AI says:**
   ```
   ✅ Phase 1 (Define Mission) Complete!
   
   You've resolved all blockers and defined requirements.
   
   Ready to move to Phase 2: Build Context?
   This phase focuses on gathering examples and research.
   
   Say 'yes' to proceed to Phase 2.
   ```

4. **User says "yes"**
5. **AI updates `.flight-plan/current.md` automatically**
6. **AI shows Phase 2 objectives**

**Phase transition is conversational, not command-based.**

---

### `flight-plan note`

**Location:** Any project directory

**Purpose:** Add timestamped activity note

**What it does:**
1. Adds note to `.flight-plan/current.md`
2. Timestamps entry

**Example:**
```bash
"flight-plan note Completed authentication module"

✅ Added note to .flight-plan/current.md

Recent Notes now shows:
- 2025-10-25 16:30: Completed authentication module
- 2025-10-25 10:00: Started auth module
```

---

### SpecKit Integration (via `flight-plan status`)

**SpecKit setup is integrated into `flight-plan status` command - not a separate command.**

**How it works:**

When user first runs `flight-plan status` in a project:
1. AI checks `.flight-plan/config.json` for `speckit_prompted` field
2. If not prompted: AI asks "Would you like SpecKit? (yes/no)"
3. If yes: AI reads FLIGHT-PLAN-SPECKIT-SETUP.md and guides through setup
4. If no: Updates config.json, never asks again

**For AI Implementers:**
- SpecKit prompting happens IN `flight-plan status`, not as separate command
- Check `.flight-plan/config.json` in current directory (project root)
- Read `../flight-plan-solution/FLIGHT-PLAN-SPECKIT-SETUP.md` for setup instructions
- Never create memory/ or specs/ directories (SpecKit does this)
- Only update `.flight-plan/config.json` in current directory to track status
- All file reads should be relative to project directory, not parent


---

## Advanced Usage

### Conversational Commands

You don't need exact syntax. AI interprets naturally:

**Instead of:** `flight-plan status --full`  
**Say:** "Show me the full status"

**Instead of:** `flight-plan sync backend-api`  
**Say:** "Sync just the backend project"

**Instead of:** `flight-plan note --type decision Using JWT`  
**Say:** "Log a decision: we're using JWT for auth"

AI understands intent and performs correct action.

---

### Working with Open Questions

**Scenario:** PRD has an open question that's now resolved.

**Project-level:**
```bash
cd backend-api/
vim project-prd.md           # Remove resolved question

"flight-plan prd refresh"
```

AI automatically:
- Removes from blockers
- Updates current.md
- Logs resolution

**No need to manually update multiple files.**

---

### Cross-Project Changes

**Scenario:** You updated `solution-rules.md` to add a new MCP server.

**Solution-level:**
```bash
cd flight-plan-solution/
vim solution-rules.md        # Add MCP server

"Sync solution rules to all projects"
```

AI will:
- Update each project's `project-rules.md` if it inherits the change
- Notify which projects are affected
- Log the update

---

## Git Integration

**Flight Plan does NOT run Git commands.**

Your workflow:
1. Make changes (via Flight Plan commands)
2. Review changes (files updated)
3. Commit manually:
   ```bash
   git add .
   git commit -m "Updated project-prd.md: Added caching requirements"
   ```

**Why?** 
- You control Git workflow
- You write meaningful commit messages
- You decide when to commit

Flight Plan manages files. You manage Git.

---

## Command Philosophy

**Intent over syntax:**
- Say what you want in natural language
- AI interprets and performs action
- No need to memorize exact commands

**Transparency:**
- AI shows what it will do
- User confirms
- Changes are logged

**No magic:**
- Read from files
- Write to files
- That's it

---

## Troubleshooting

**"Command not recognized"**

The AI might not have loaded this file. Say:
```
"Read FLIGHT-PLAN-COMMANDS.md and then run flight-plan status"
```

**"Changes not syncing"**

Check:
1. Are you in the right directory?
   - Solution commands: `flight-plan-solution/`
   - Project commands: Project directory
2. Is the file structure correct?
3. Does the latest PRD exist?

**"AI asks too many questions"**

Flight Plan prefers confirmation over assumptions. This is by design.

If you trust the AI's interpretation:
```
"Run flight-plan sync and auto-confirm all changes"
```

---

## Examples

### Example 1: New PRD Version

```bash
cd flight-plan-solution/

# Create new PRD version
vim solution-prd-v2.md       # Add new features

# Sync to projects
"flight-plan sync"

AI: Shows preview of changes
You: "yes"
AI: Updates project PRDs, logs changes

# Commit
git add .
git commit -m "PRD v2: Added notification features"
```

---

### Example 2: Working on a Project

```bash
cd backend-api/

# Check status
"flight-plan status"

# Work on code
vim src/auth.js

# Add note
"flight-plan note Implemented JWT authentication"

# Move to next phase when ready
"flight-plan phase 6"
```

---

### Example 3: Resolving Open Questions

```bash
cd backend-api/

# Edit PRD to resolve question
vim project-prd.md           # Remove "question-2: Caching?"
                             # Add: Using Redis for caching

# Refresh tracking
"flight-plan prd refresh"

AI: Detected change, removed from blockers
```

---

### Example 4: Enabling Spec-Kit

```bash
cd frontend/

# Enable Spec-Kit
"flight-plan enable-speckit"

# Now use Spec-Kit
"/speckit.spec user-profile"

AI: Creates spec, references Flight Plan phase and standards
```

---

## See Also

- **FLIGHT-PLAN-INIT.md** - Initial setup
- **FLIGHT-PLAN-PHASES.md** - 8 phases explained
- **GENERATOR.md** - How generation works
- **README.md** - Overview

---

**Version:** 1.0  
**Compatible with:** Flight Plan v1.0  
**Last Updated:** 2025-10-25
