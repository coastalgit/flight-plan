# ⚡ Flight Plan Commands

**Version:** 1.0  
**Last Updated:** 2025-10-25

---

## Overview

After initial setup (`flight-plan init`), use these commands for ongoing work.

**Just say to your AI (Cursor, Claude, etc):**
```
"flight-plan status"
"flight-plan sync"
"flight-plan prd refresh"
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

**Optional:** Use `flight-plan enable-speckit` to set up integration.

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

## Command Reference

### Solution-Level Commands
(Context: directory with `solution-prd-v*.md`)

| Command | Purpose |
|---------|---------|
| `flight-plan status` | Show all projects' progress |
| `flight-plan sync` | Preview PRD changes |
| `flight-plan sync apply` | Apply PRD changes to projects |

### Project-Level Commands
(Context: directory with `project-prd.md`)

| Command | Purpose |
|---------|---------|
| `flight-plan status` | Show this project's status |
| `flight-plan prd refresh` | Preview tracking changes |
| `flight-plan prd refresh apply` | Apply tracking updates |
| `flight-plan note [text]` | Add activity note |
| `flight-plan phase [N]` | Move to phase N |
| `flight-plan enable-speckit` | Set up Spec-Kit integration |

---

## Command Pattern: Preview → Apply

**Major Flight Plan commands use explicit `apply` subcommand:**

**Preview (default):**
- `flight-plan sync` - Shows what will change
- `flight-plan prd refresh` - Shows tracking updates
- `flight-plan init` - Shows project structure

**Apply (execute):**
- `flight-plan sync apply` - Actually updates files
- `flight-plan prd refresh apply` - Actually updates tracking
- `flight-plan init apply` - Actually creates projects

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

AI: ✅ Updated backend-api/project-prd.md
    ✅ Updated backend-api/memory/constitution.md
    ...
```

**This applies to:**
- `flight-plan init` / `flight-plan init apply`
- `flight-plan sync` / `flight-plan sync apply`
- `flight-plan prd refresh` / `flight-plan prd refresh apply`

---

## Solution-Level Commands

### Context Detection

**How AI knows this is solution-level:**
1. Check current directory for `solution-prd-v*.md`
2. Check for `templates/` subdirectory
3. Check for `ai-refs/` directory
4. If found: Solution-level context

---

### `flight-plan status`

**Context:** Solution-level (has `solution-prd-v*.md`)

**Purpose:** Show progress across all projects

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
1. Updates `project-prd.md` files
2. Updates `project-rules.md` if solution rules changed
3. Updates `memory/constitution.md` if Spec-Kit enabled
4. Updates `solution-rules.md` if needed
5. Logs all changes

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
✓ backend-api/project-prd.md
✓ backend-api/project-rules.md
✓ backend-api/memory/constitution.md (tech stack)
✓ notifications/project-prd.md
✓ notifications/project-rules.md
✓ ai-refs/solution-overview.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# User can discuss, ask questions

"flight-plan sync apply"

✅ Updated: solution-rules.md
✅ Updated: backend-api/project-prd.md
✅ Updated: backend-api/project-rules.md
✅ Updated: backend-api/memory/constitution.md
✅ Updated: notifications/project-prd.md
✅ Updated: notifications/project-rules.md
✅ Updated: ai-refs/solution-overview.md

Sync complete. All projects now at v2.
```

**Important:** This command does NOT run Git commands. Git is the user's responsibility.

---

## Project-Level Commands

### Context Detection

**How AI knows this is project-level:**
1. Check current directory for `project-prd.md`
2. Check for `.flight-plan/` subdirectory
3. Check for `project-rules.md`
4. If found: Project-level context

---

### `flight-plan status`

**Context:** Project-level (has `project-prd.md`)

**Purpose:** Show this project's current status

**What it does:**
1. Reads `.flight-plan/current.md`
2. Shows current phase, activity, blockers
3. Shows PRD sync status

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

**Context:** Project-level (has `project-prd.md`)

**Purpose:** Update tracking after manual PRD edits

**Usage:**
- `flight-plan prd refresh` - Preview changes only
- `flight-plan prd refresh apply` - Apply tracking updates

**Prerequisites Check:**
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

### `flight-plan phase`

**Location:** Any project directory

**Purpose:** Move to a new phase

**What it does:**
1. Updates phase in `.flight-plan/current.md`
2. Resets phase activities checklist
3. Logs transition

**Example:**
```bash
"flight-plan phase 2"

📋 Moving to Phase 2: Build Context

Updated:
✅ .flight-plan/current.md (Phase 1 → Phase 2)
✅ Reset phase activities checklist

Phase 2 Objectives:
- Gather examples and patterns
- Research technical approaches
- Document findings

See: ../flight-plan-solution/FLIGHT-PLAN-PHASES.md
```

---

### `flight-plan enable-speckit`

**Location:** Any project directory

**Purpose:** Set up Spec-Kit integration

**What it does:**
1. Creates `memory/constitution.md` (references Flight Plan files)
2. Creates `specs/` directory
3. Shows how to use Spec-Kit with Flight Plan

**Example:**
```bash
cd backend-api/

"flight-plan enable-speckit"

✅ Created memory/constitution.md
✅ Created specs/
✅ Spec-Kit integration ready

constitution.md references:
- project-prd.md (project specs)
- .flight-plan/current.md (phase/status)
- ../../flight-plan-solution/FLIGHT-PLAN-PHASES.md (phase standards)

You can now use Spec-Kit commands. The AI will:
- Know what phase you're in
- Apply appropriate standards
- Reference Flight Plan context

Next steps:
- Use Spec-Kit for feature development
- AI will automatically apply phase-appropriate standards
```

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
