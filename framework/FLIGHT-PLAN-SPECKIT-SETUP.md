# SpecKit Setup Instructions for AI

**Version:** 3.0  
**Last Updated:** 2025-10-28  
**Context:** PROJECT-LEVEL ONLY (run from within a generated project)

---

## ⚠️ CRITICAL: PROJECT-LEVEL COMMAND ONLY

**This command runs IN a generated project directory, NOT at solution level.**

**Expected location:**
```
MyApp/
├── flight-plan-solution/
└── project-a/                  ← Run "flight-plan setup-speckit" HERE
    ├── docs/
    │   ├── project-prd.md
    │   └── project-rules.md
    └── .flight-plan/
        ├── FLIGHT-PLAN-COMMANDS.md
        └── current.md
```

---

## What is SpecKit?

**SpecKit** is GitHub's toolkit for Spec-Driven Development.

**Source:** https://github.com/github/spec-kit

**What it provides:**
- Feature specifications (`/speckit.specify`)
- Implementation plans (`/speckit.plan`)
- Task breakdowns (`/speckit.tasks`)
- Execution tracking (`/speckit.implement`)

**Flight Plan Integration:**
- Flight Plan manages project lifecycle (8 phases, overall progress)
- SpecKit manages individual features (specs, plans, tasks)
- Flight Plan provides context → SpecKit uses it for feature development

---

## How User Learns About SpecKit

**Users discover SpecKit through `flight-plan status`:**

When user first runs `flight-plan status` in a project:
1. AI checks `.flight-plan/config.json` for `speckit_prompted` field
2. If not prompted yet: AI asks about SpecKit
3. If yes: AI shows installation steps (from this file)
4. After installation: AI runs `flight-plan setup-speckit` automatically

**Users DO NOT manually run "flight-plan setup-speckit"** - it's triggered by the status command flow.

---

## SpecKit Directory Structure (Actual)

**After running `specify init . --ai cursor-agent --here`, SpecKit creates:**

```
project-a/
├── .specify/                   ← SpecKit's directory
│   ├── memory/
│   │   └── constitution.md    ← Default template
│   ├── scripts/
│   │   └── [helper scripts]
│   └── templates/
│       └── [spec templates]
├── docs/
│   ├── project-prd.md
│   └── project-rules.md
└── .flight-plan/
    └── current.md
```

**IMPORTANT:**
- ❌ NOT `memory/` at root level
- ❌ NOT `specs/` directory (created later by `/speckit.specify`)
- ✅ `.specify/` directory with subdirectories

---

## Official SpecKit Installation

### Step 1: Install `uv` (Prerequisite)

SpecKit requires `uv` (Python package installer).

**Check if installed:**
```bash
uv --version
```

**If not installed:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**To upgrade existing installation:**
```bash
uv self update
```

---

### Step 2: Install SpecKit (Recommended Method)

**Persistent installation (recommended):**

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**Verify installation:**
```bash
specify --version
```

**To upgrade later:**
```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

---

### Step 3: Initialize SpecKit in Project

**From your project directory:**

```bash
specify init . --ai cursor-agent --here
```

**Available AI agents:**
- `claude` - Claude Code
- `copilot` - GitHub Copilot
- `cursor-agent` - Cursor
- `gemini` - Gemini CLI
- `windsurf` - Windsurf
- `qwen` - Qwen Code
- `roo` - Roo Code
- And more...

**Additional options:**
- `--here` - Initialize in current directory (required for existing projects)
- `--force` - Force merge/overwrite
- `--no-git` - Skip git repository initialization

---

## Flight Plan Integration Flow

### When Running `flight-plan status` (First Time)

**If SpecKit not prompted yet:**

```
📊 [project-name] Status

Phase: 1 - Define Mission
Activity: Initial requirements gathering

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🌱 Optional: SpecKit Integration

SpecKit provides feature-driven development with specs, plans, and tasks.

Would you like to set up SpecKit for this project? [y/n]
```

---

### If User Says "yes"

**Show installation steps:**

```
✅ SpecKit Setup for [project-name]

Project: [project-name]
Current Phase: Phase [N] ([phase-name])
Tech Stack: [from project-prd.md]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📥 INSTALLATION STEPS:

Step 1: Install SpecKit

  uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

Step 2: Initialize SpecKit in this project

  specify init . --ai cursor-agent --here

  (Say 'y' to the warning about existing files - it's safe)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

After installation completes, run:

  flight-plan setup-speckit

This will configure SpecKit's constitution with your Flight Plan context.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Update `.flight-plan/config.json`:**
```json
{
  "speckit_prompted": true,
  "speckit_enabled": false
}
```

---

### If User Says "no"

```
📝 Noted: SpecKit setup skipped

You can enable SpecKit later by running "flight-plan status" again.

Updated .flight-plan/config.json
```

**Update `.flight-plan/config.json`:**
```json
{
  "speckit_prompted": true,
  "speckit_enabled": false
}
```

---

## How to Execute `flight-plan setup-speckit`

**This runs AFTER user has installed SpecKit.**

### STEP 1: Validate Context

**Check you're in a project directory:**

1. ✅ Check if `docs/project-prd.md` exists
2. ✅ Check if `.flight-plan/` directory exists

If missing:
```
❌ Cannot setup SpecKit: Not in a project directory

This command works in a project directory with:
- docs/project-prd.md
- .flight-plan/current.md

Current location: [show pwd]
```

**STOP if validation fails.**

---

### STEP 2: Check SpecKit Installation

**Check if `.specify/` directory exists:**

```bash
# Check for .specify/ directory
ls .specify/
```

If NOT found:
```
❌ SpecKit not initialized

Please run SpecKit installation first:

  specify init . --ai cursor-agent --here

Then run "flight-plan setup-speckit" again.
```

**STOP if SpecKit not installed.**

---

### STEP 3: Read Project Context

**Read these files from current directory (project root):**

1. **docs/project-prd.md** - Tech stack, quality standards, requirements
2. **docs/project-rules.md** - AI rules, MCP servers
3. **.flight-plan/current.md** - Current phase, tasks, blockers
4. **.flight-plan/FLIGHT-PLAN-PHASES.md** - Phase standards (standalone copy)

**Extract from project-prd.md:**
- Project name
- Technology stack (languages, frameworks, databases)
- Quality standards (test coverage, performance, security)
- Open questions / blockers
- Constraints

**Extract from current.md:**
- Current phase number and name
- Active tasks
- Blockers

**Extract from project-rules.md:**
- MCP servers configured
- AI behavior rules
- Project-specific standards

---

### STEP 4: Generate Constitution

**Generate `.specify/memory/constitution.md` with Flight Plan context:**

```markdown
# [Project Name] Constitution

**Generated by Flight Plan**  
**Date:** [current date]  
**Phase:** [phase number] - [phase name]

---

## 🔗 Flight Plan Integration

This project uses Flight Plan for lifecycle management (8-phase methodology).

**Context Files:**
- `docs/project-prd.md` - Requirements, tech stack, quality standards
- `.flight-plan/current.md` - Current phase, tasks, blockers
- `docs/project-rules.md` - AI rules, MCP servers, standards
- `.flight-plan/FLIGHT-PLAN-PHASES.md` - Phase standards

**Current Phase:** [phase number] - [phase name]

### Phase Standards

Apply standards based on current Flight Plan phase:

- **Phase 1-2** (Define/Plan): Focus on requirements clarity
- **Phase 3-4** (Setup/Build): Establish foundation and architecture
- **Phase 5-6** (Test/Polish): Ensure quality and refinement
- **Phase 7-8** (Deploy/Operate): Production readiness

See `.flight-plan/FLIGHT-PLAN-PHASES.md` for detailed phase standards.

---

## 🎯 Project Context

**Name:** [project name]

**Purpose:** [from project-prd.md]

**Type:** [Frontend/Backend/etc from project-prd.md]

---

## 🛠️ Technology Stack

**Language:** [from project-prd.md]

**Framework:** [from project-prd.md]

**Database:** [from project-prd.md or "Not applicable"]

**Deployment:** [from project-prd.md]

**Key Dependencies:**
[List from project-prd.md]

---

## ✅ Quality Standards

**From Flight Plan PRD:**

**Test Coverage:** [from project-prd.md]

**Performance:** [from project-prd.md]

**Security:** [from project-prd.md]

**Accessibility:** [from project-prd.md]

**Code Quality:** [from project-prd.md]

All SpecKit implementations must satisfy these standards.

---

## 🚧 Current Constraints

[List constraints from project-prd.md]

---

## 🔍 Open Questions / Blockers

[List from .flight-plan/current.md]

When creating specs, address these questions or note dependencies.

---

## 🤖 AI Integration

**MCP Servers Configured:**
[List from project-rules.md]

**AI Behavior Rules:**
[Extract relevant rules from project-rules.md]

---

## 📋 SpecKit Workflow with Flight Plan

### Creating a Feature Spec

1. Check current phase in `.flight-plan/current.md`
2. Apply phase-appropriate standards from `.flight-plan/FLIGHT-PLAN-PHASES.md`
3. Reference tech stack from this constitution
4. Ensure quality standards are met
5. Note any blockers from Flight Plan

### During Implementation

1. Follow tech stack defined above
2. Apply quality standards from PRD
3. Update Flight Plan progress if completing phase objectives
4. Use MCP servers as configured

### After Implementation

1. Verify against quality standards
2. Update Flight Plan if feature completes a phase objective
3. Run `flight-plan status` to check overall progress

---

## 🎨 Project-Specific Principles

[Any additional principles from project-prd.md or project-rules.md]

---

**This constitution is synchronized with Flight Plan.**
**When project-prd.md changes, run `flight-plan prd refresh apply` to update this file.**

---

**Generated:** [timestamp]  
**Flight Plan Version:** 1.0  
**SpecKit Version:** [specify --version]
```

---

### STEP 5: Update Tracking

**Update `.flight-plan/config.json`:**

```json
{
  "project_name": "[project-name]",
  "generated_date": "[existing]",
  "prd_version": "[existing]",
  "speckit_enabled": true,
  "speckit_prompted": true,
  "speckit_configured_date": "[current date YYYY-MM-DD HH:MM UTC]",
  "current_phase": [existing],
  "last_updated": "[current date]"
}
```

---

### STEP 6: Show Completion

```
✅ SpecKit configured successfully!

Updated files:
✓ .specify/memory/constitution.md (generated from Flight Plan context)
✓ .flight-plan/config.json (tracking updated)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 NEXT STEPS:

Use SpecKit commands for feature development:

  /speckit.specify       - Create feature specification
  /speckit.plan          - Create implementation plan
  /speckit.tasks         - Break down into tasks
  /speckit.implement     - Execute implementation

Optional enhancement commands:
  /speckit.clarify       - Ask clarifying questions (before /speckit.plan)
  /speckit.checklist     - Generate quality checklist (after /speckit.plan)
  /speckit.analyze       - Check consistency (before /speckit.implement)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SpecKit now has full context from your Flight Plan:
✓ Current phase and standards
✓ Tech stack and constraints
✓ Quality requirements
✓ Open questions and blockers

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## When PRD Changes

**After updating `docs/project-prd.md`:**

```bash
# Update tracking and constitution
flight-plan prd refresh
flight-plan prd refresh apply
```

This will:
1. Update `.flight-plan/current.md`
2. Update `.specify/memory/constitution.md` (if tech stack changed)
3. Keep SpecKit synchronized with Flight Plan

---

## Error Handling

### If SpecKit Already Configured

**Check `.flight-plan/config.json` for `speckit_enabled: true`:**

```
⚠️  SpecKit already configured

.specify/memory/constitution.md already exists and is configured.

To reconfigure:
1. Remove .specify/memory/constitution.md
2. Run "flight-plan setup-speckit" again

Or edit .specify/memory/constitution.md directly.
```

### If User Hasn't Installed SpecKit

```
❌ SpecKit not found

Please install SpecKit first:

  uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
  specify init . --ai cursor-agent --here

Then run "flight-plan setup-speckit" again.
```

---

## Important Notes

**DO:**
- ✅ Run this from PROJECT directory (has docs/project-prd.md)
- ✅ Check for `.specify/` directory (not `memory/` at root)
- ✅ Generate constitution.md from project files
- ✅ Use standalone project files (no ../../flight-plan-solution/ references)
- ✅ Read `.flight-plan/FLIGHT-PLAN-PHASES.md` (local copy in project)

**DON'T:**
- ❌ Run this from solution directory
- ❌ Look for `memory/` or `specs/` at root level (wrong structure)
- ❌ Manually edit constitution.md (generate it)
- ❌ Reference files outside project directory
- ❌ Use examples/ folder for content

---

## Summary

**The flow is:**
1. User runs `flight-plan status` in project
2. AI asks about SpecKit (if not prompted before)
3. User installs SpecKit: `specify init . --ai cursor-agent --here`
4. User runs: `flight-plan setup-speckit`
5. AI reads project context (PRD, current phase, rules)
6. AI generates `.specify/memory/constitution.md` with full context
7. SpecKit is now integrated with Flight Plan!

**Key principle:** AUTOMATE EVERYTHING. No manual editing.

---

**Version:** 3.0  
**Compatible with:** Flight Plan v1.0, SpecKit (latest)  
**Last Updated:** 2025-10-28
