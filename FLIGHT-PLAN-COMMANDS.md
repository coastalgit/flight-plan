# ⚡ Flight Plan Commands — Ongoing Operations

**Commands Version:** 1.0  
**Last Updated:** 2025-10-22

---

**Philosophy:** Commands describe intent, not syntax. Any AI or human following this document must prefer accuracy and traceability over speed.

---

## Overview

After initial setup with GENERATOR.md, use these commands for ongoing work.

**Just say to your local AI (Cursor, Claude, etc):**
```
"flight-plan status"
"flight-plan sync"
"flight-plan review"
"flight-plan help"
```

### TL;DR Command Reference

| Command | Purpose | Reads | Writes |
|---------|---------|-------|--------|
| **help** | show all commands | this file | – |
| **status** | show progress summary | current.md | – |
| **sync** | apply latest PRD changes | solution-prd-v*.md | requirements.md, implementation.md |
| **evaluate** | compare requirements vs implementation | requirements.md, implementation.md | – |
| **review** | list blockers, TODOs, quality metrics | all current/specs | – |
| **note** | add log / decision / milestone | – | logs/, decisions/, history/ |
| **deps** | analyze dependencies & impacts | implementation.md | – |

**Safety:**
- 🟢 Read-only: `status`, `evaluate`, `review`, `deps`
- 🟠 Write: `sync`, `note`

---

## Commands

### `flight-plan help`

**Show all available commands with examples**

```
Lists this command reference with:
- Command syntax
- Available options
- Example usage
- What each command does
```

---

### `flight-plan status`

**Show solution-wide progress summary**

**Syntax:**
- `flight-plan status` (default: --brief)
- `flight-plan status --brief` - High-level overview
- `flight-plan status --full` - Detailed status with all tasks
- `flight-plan status --report` - Comprehensive report (all details + metrics)

**What it does:**
1. Reads all project `.flight-plan/current.md` files
2. Shows current phase for each project
3. Lists active tasks and blockers
4. Indicates overall solution health

**--brief output:**
```
📊 Solution Status (from solution-prd-v2.md)

✅ guest-ui          Phase 3 (Design) - On track
⚠️  backend          Phase 2 (Context) - 2 blockers
🚀 admin-ui          Phase 5 (Build)   - In progress
❌ notifications     Phase 1 (Define)  - Blocked (auth decision)

Overall: 3/4 projects active, 1 blocked
```

**--full output:**
Includes all tasks, completed items, next steps per project

**--report output:**
Comprehensive report with metrics, velocity, timeline projections

---

### `flight-plan sync`

**Sync projects with latest PRD version**

**Syntax:**
- `flight-plan sync` - Sync all projects
- `flight-plan sync [project-name]` - Sync specific project
- `flight-plan sync --refresh` - Rebuild solution-overview.md only

**What it does:**
1. Finds latest `solution-prd-v*.md`
2. Compares with previous version
3. Shows what changed
4. Asks what to update
5. Updates `.flight-plan/requirements.md` and `implementation.md` in affected projects
6. Logs changes in `.flight-plan/decisions/`
7. Rebuilds `ai-refs/solution-overview.md`

**Example workflow:**
```
User: "flight-plan sync"

AI: "Latest PRD is v3 (previous was v2).
Changes detected:
- backend: Database changed PostgreSQL → MongoDB
- notifications: New project added
- admin-ui: No changes

Update backend? (y/n)"
```

---

### `flight-plan evaluate`

**Evaluate project against requirements**

**Syntax:**
- `flight-plan evaluate [project-name]`

**What it does:**
1. Reads `.flight-plan/requirements.md` (what should be built)
2. Reads `.flight-plan/implementation.md` (how it's built)
3. Compares against actual code/structure
4. Shows gaps, mismatches, or deviations
5. Suggests next steps

**Example output:**
```
📋 Evaluation: backend

Requirements Coverage:
✅ User authentication         Implemented
✅ Booking CRUD operations     Implemented
⚠️  Payment processing         Partial (Stripe integrated, webhooks pending)
❌ Email notifications         Not started

Implementation Status:
✅ PostgreSQL schema defined
✅ Express routes created
⚠️  Tests at 45% coverage (target: 80%)
❌ Error handling incomplete

Recommendation: Focus on email notifications (Phase 5 task)
```

---

### `flight-plan review`

**Review all blockers, TODOs, open questions, and quality metrics**

**What it does:**
1. Scans all projects for:
   - Blockers in `current.md`
   - TODO items from PRD
   - Open questions
   - Coverage gaps
   - Quality gates
2. Groups by priority
3. Shows which projects affected

**Example output:**
```
🔍 Solution Review

HIGH PRIORITY (3):
- [ ] TODO-001: Choose auth provider (blocks: backend, guest-ui, admin-ui)
- [ ] Blocker: Database schema review pending (backend)
- [ ] Question-5: Payment provider selection (backend)

MEDIUM PRIORITY (4):
- [ ] TODO-003: Email service provider (notifications)
- [ ] Blocker: Design system not finalized (guest-ui)
- [ ] Coverage: backend at 45% (target: 80%)
...

Total: 8 items requiring attention
Quality gates: 2/4 passing
```

---

### `flight-plan note`

**Add timestamped notes to project logs**

**Syntax:**
- `flight-plan note [message]` - General note
- `flight-plan note --type decision [message]` - Decision log
- `flight-plan note --type milestone [message]` - Milestone

**What it does:**
1. Adds timestamped entry to appropriate log
2. Cross-references in `current.md` if relevant
3. Updates solution-overview.md if significant

**Examples:**
```
"flight-plan note Switched to React Query for data fetching"
→ Adds to docs/logs/[date].md

"flight-plan note --type decision Using JWT for auth tokens"
→ Creates .flight-plan/decisions/jwt-auth-decision.md

"flight-plan note --type milestone Phase 1 complete, moving to Phase 2"
→ Adds to .flight-plan/history/[date]-phase-1-complete.md
```

---

### `flight-plan deps`

**Analyze project dependencies and impact**

**Syntax:**
- `flight-plan deps` - Show all dependencies
- `flight-plan deps --impact [change]` - Impact analysis

**What it does:**
1. Reads all project `implementation.md` files
2. Extracts internal/external dependencies
3. Creates dependency graph
4. Shows critical path

**Example output:**
```
📊 Project Dependencies

guest-ui → backend (API calls)
admin-ui → backend (API calls)
backend → notifications (triggers)
backend → PostgreSQL (storage)
notifications → SendGrid (email)

Critical path: guest-ui → backend → PostgreSQL

"flight-plan deps --impact backend database change"
→ Impact: guest-ui, admin-ui, notifications
   Reason: All depend on backend schema
   Action: Update API contracts, test integrations
```

---

## How Commands Work

### File Locations Commands Read:

```
flight-plan-solution/
├── solution-prd-v*.md          ← Latest version info
├── ai-refs/
│   ├── solution-overview.md    ← Cross-project status
│   └── notes.md                ← Your notes
└── [project]/
    └── .flight-plan/
        ├── current.md          ← Progress, blockers, tasks
        ├── requirements.md     ← What to build
        ├── implementation.md   ← How it's built
        ├── decisions/          ← Decision logs
        └── history/            ← Milestones
```

### What Commands Update:

- **status** - Reads only, no updates
- **sync** - Updates requirements.md, implementation.md, solution-overview.md
- **evaluate** - Reads only, provides recommendations
- **review** - Reads only, aggregates data
- **note** - Writes to logs/, decisions/, history/
- **deps** - Reads only, analyzes relationships

---

## Context for AI

**When user runs a command:**

1. **Parse command** - Identify which command and options
2. **Gather context** - Read relevant files
3. **Process** - Perform analysis/updates
4. **Present results** - Clear, actionable output
5. **Offer next steps** - Suggest what to do next

**Always:**
- Include timestamps (UTC ISO 8601)
- Cross-reference PRD version
- Show file paths for verification
- Provide actionable recommendations

---

## 🧠 AI Prompt Style

**When using these commands inside a chat or IDE, prefix them with natural language:**

**Good examples:**
- "Run flight-plan status and tell me what's blocking progress"
- "Please execute flight-plan deps --impact backend change"
- "Do a flight-plan review and highlight high-priority items"
- "flight-plan sync backend and show me what changed"

**Why this works:**
- AI understands the command structure
- Natural language provides context
- Commands describe intent, AI figures out execution
- You can ask follow-up questions naturally

---

## Example Session

```
User: "flight-plan status --brief"
AI: [Shows 4 projects, 2 blockers, 8 TODOs]

User: "flight-plan review"
AI: [Lists blockers, TODOs, and quality metrics by priority]

User: "Tell me more about TODO-001"
AI: [Explains auth provider decision needed]

User: "We're going with Auth0"
AI: "Great! Should I:"
    1. Update backend implementation.md
    2. Resolve TODO-001
    3. Log this decision
    4. Check if this unblocks other projects

User: "Yes, all of that"
AI: [Performs updates, shows summary]

User: "flight-plan status"
AI: [Updated status - 1 less blocker, notifications unblocked]
```

---

## Adding Custom Commands

You can extend with project-specific commands:

```
"flight-plan test-coverage"
"flight-plan deploy-readiness"
"flight-plan security-audit"
```

Just describe what you want analyzed and the AI will:
1. Identify relevant files to read
2. Perform analysis
3. Present findings
4. Suggest actions

---

## ⚠️ Safety Clause

**Never overwrite user code or delete files.**

All write operations must:
- Create new files (logs, decisions, milestones)
- Update version-tracked files (requirements.md, implementation.md)
- Create versioned copies when uncertain
- Ask before any destructive operation

**Commands marked 🟠 (Write) will:**
- Show what they'll change before writing
- Create backups or versioned copies
- Log all modifications
- Never touch source code directories

**If uncertain, always ask the user first.**

---

**Flight Plan Commands Version:** 1.0  
**Compatible with:** GENERATOR.md v1.0, BRIEF-BUILDER v1.0
