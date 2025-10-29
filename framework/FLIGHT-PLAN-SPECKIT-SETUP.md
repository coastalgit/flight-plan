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

### STEP 4: Generate SpecKit Integration Files

**Generate THREE files from your PRD:**

---

#### 4A. Generate `.specify/memory/constitution.md` (Principles & Guidelines ONLY)

```markdown
# [Project Name] Constitution

**Generated by Flight Plan**  
**Date:** [current date]  
**Phase:** [phase number] - [phase name]

---

## 🔗 Flight Plan Integration

This project uses Flight Plan for lifecycle management (8-phase methodology).

**Always consult these files:**
- `docs/project-prd.md` - Single source of truth for requirements
- `docs/project-spec.md` - Feature specifications (for SpecKit)
- `docs/project-plan.md` - Implementation plan (for SpecKit)
- `.flight-plan/current.md` - Current phase, tasks, blockers
- `.flight-plan/FLIGHT-PLAN-PHASES.md` - Phase standards

**Current Phase:** [phase number] - [phase name]

---

## 📋 Development Principles

### Phase-Based Standards

Apply standards based on current Flight Plan phase:

- **Phase 1-2** (Define/Research): Focus on requirements clarity, gather context
- **Phase 3-4** (Design/Setup): Establish foundation and architecture
- **Phase 5-6** (Build/Test): Ensure quality and refinement
- **Phase 7-8** (Deploy/Operate): Production readiness

See `.flight-plan/FLIGHT-PLAN-PHASES.md` for detailed phase standards.

### Quality Requirements

**ALL implementations must satisfy quality standards defined in `docs/project-prd.md`:**
- Test coverage requirements
- Performance targets
- Security requirements
- Accessibility standards
- Code quality standards

**Check `docs/project-prd.md` for specific metrics and thresholds.**

### Workflow with Flight Plan

**When creating features:**
1. Check current phase in `.flight-plan/current.md`
2. Apply phase-appropriate standards
3. Reference `docs/project-spec.md` for requirements
4. Reference `docs/project-plan.md` for tech approach
5. Note any blockers from Flight Plan

**After implementation:**
1. Verify against quality standards in PRD
2. Update `.flight-plan/current.md` if completing phase objectives
3. Run `flight-plan status` to check overall progress

---

## 🎨 Project-Specific Guidelines

[Extract any development workflow rules or coding standards from docs/project-rules.md]

---

**This constitution provides PRINCIPLES. See `docs/project-spec.md` and `docs/project-plan.md` for actual requirements and implementation details.**

---

**Generated:** [timestamp]  
**Flight Plan Version:** 1.0
```

---

#### 4B. Generate `docs/project-spec.md` (Feature Specifications)

**Extract from `docs/project-prd.md`:**

```markdown
# [Project Name] - Feature Specification

**Generated from:** docs/project-prd.md  
**Date:** [current date]  
**For:** SpecKit integration

---

## Overview

[Extract from PRD Section 1 - Project overview and purpose]

---

## User Stories & Requirements

[Extract from PRD Section 3 - Functional Requirements]
[Extract user stories]
[Format as clear, actionable requirements]

---

## Success Criteria

[Extract from PRD - Success Criteria section]
[What needs to be true for this to be successful]

---

## Constraints

[Extract from PRD - Technical Constraints]
[Extract from PRD - Business Constraints]

---

## Open Questions

[Extract from PRD Section 8 - Open Questions]
[List any blockers from .flight-plan/current.md]

---

**This spec is synchronized with Flight Plan PRD.**
**When project-prd.md changes, run `flight-plan prd refresh apply` to regenerate this file.**
```

---

#### 4C. Generate `docs/project-plan.md` (Implementation Plan)

**Extract from `docs/project-prd.md`:**

```markdown
# [Project Name] - Implementation Plan

**Generated from:** docs/project-prd.md  
**Date:** [current date]  
**For:** SpecKit integration

---

## Technology Stack

[Extract from PRD Section 5 - Technology Stack]

**Language:** [from PRD]  
**Framework:** [from PRD]  
**Database:** [from PRD]  
**Deployment:** [from PRD]

**Key Dependencies:**
[List from PRD]

---

## Architecture

[Extract from PRD Section 4 - Architecture]
[Include diagrams if present]

---

## Data Model

[Extract from PRD - Data Model section if present]
[Database schema, entities, relationships]

---

## API Design

[Extract from PRD - API Design section if present]
[Endpoints, request/response formats]

---

## Implementation Approach

[Extract from PRD - Implementation sections]
[How components fit together]
[Integration points]

---

## Quality Standards

[Extract from PRD - Quality Standards section]

**Test Coverage:** [target]  
**Performance:** [targets]  
**Security:** [requirements]

---

## Dependencies

### Internal Dependencies
[Extract from PRD - dependencies on other projects]

### External Dependencies
[Extract from PRD - third-party services]

---

**This plan is synchronized with Flight Plan PRD.**
**When project-prd.md changes, run `flight-plan prd refresh apply` to regenerate this file.**
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
  "speckit_files_generated": true,
  "current_phase": [existing],
  "last_updated": "[current date]"
}
```

---

### STEP 6: Show Completion

```
✅ SpecKit configured successfully!

Generated files from your PRD:
✓ .specify/memory/constitution.md (principles & guidelines)
✓ docs/project-spec.md (feature specifications from PRD)
✓ docs/project-plan.md (implementation plan from PRD)
✓ .flight-plan/config.json (tracking updated)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 NEXT STEPS:

Point SpecKit at your Flight Plan files:

  /speckit.specify from docs/project-spec.md
  /speckit.plan from docs/project-plan.md
  /speckit.tasks
  /speckit.implement

Optional enhancement commands:
  /speckit.clarify       - Ask clarifying questions (before /speckit.plan)
  /speckit.checklist     - Generate quality checklist (after /speckit.plan)
  /speckit.analyze       - Check consistency (before /speckit.implement)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SpecKit now reads from your Flight Plan:
✓ docs/project-spec.md - All requirements from PRD
✓ docs/project-plan.md - Tech stack, architecture, approach
✓ .specify/memory/constitution.md - Principles & workflow

No need to re-type requirements - everything is already there!

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️  FOR AI: STOP HERE

DO NOT offer to:
- "kick off a first feature spec"
- "start with a feature"
- "create a spec for X"
- "run /speckit.specify for you"
- ANY proactive suggestions

The user will use SpecKit commands when THEY decide to.
Your job is DONE after showing the success message above.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## When PRD Changes

**After updating `docs/project-prd.md`:**

```bash
# Update tracking and regenerate SpecKit files
flight-plan prd refresh
flight-plan prd refresh apply
```

This will:
1. Update `.flight-plan/current.md`
2. Regenerate `docs/project-spec.md` (if requirements changed)
3. Regenerate `docs/project-plan.md` (if tech stack/architecture changed)
4. Update `.specify/memory/constitution.md` (if principles/guidelines changed)
5. Keep SpecKit synchronized with Flight Plan

**Then tell SpecKit to re-read the updated files:**
```bash
/speckit.specify from docs/project-spec.md
/speckit.plan from docs/project-plan.md
```

---

## Error Handling

### If SpecKit Already Configured

**Check `.flight-plan/config.json` for `speckit_enabled: true`:**

```
⚠️  SpecKit already configured

SpecKit files already exist:
- .specify/memory/constitution.md
- docs/project-spec.md
- docs/project-plan.md

To reconfigure:
1. Delete the files you want to regenerate
2. Re-enable SpecKit via "flight-plan status"

Or edit the files directly if you need manual changes.
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
- ✅ Generate THREE files: constitution.md, project-spec.md, project-plan.md
- ✅ Extract from PRD (don't duplicate, transform)
- ✅ Use standalone project files (no ../../flight-plan-solution/ references)
- ✅ Read `.flight-plan/FLIGHT-PLAN-PHASES.md` (local copy in project)

**DON'T:**
- ❌ Run this from solution directory
- ❌ Look for `memory/` or `specs/` at root level (wrong structure)
- ❌ Put requirements/tech stack in constitution.md (put in spec/plan files)
- ❌ Create SpecKit's internal files (specs/[feature]/) - let SpecKit do that
- ❌ Reference files outside project directory
- ❌ Use examples/ folder for content

---

## Summary

**The complete flow is:**
1. User runs `flight-plan status` in project (first time)
2. AI shows status, then asks about SpecKit
3. User answers "yes"
4. AI guides: User runs `specify init . --ai cursor-agent --here`
5. AI automatically generates THREE files from PRD:
   - `.specify/memory/constitution.md` (principles & guidelines)
   - `docs/project-spec.md` (requirements from PRD)
   - `docs/project-plan.md` (tech stack & architecture from PRD)
6. User points SpecKit at Flight Plan files:
   - `/speckit.specify from docs/project-spec.md`
   - `/speckit.plan from docs/project-plan.md`
   - `/speckit.tasks`
   - `/speckit.implement`

**Key principles:**
- ✅ AUTOMATE extraction from PRD to SpecKit files
- ✅ NO duplication - transform PRD into SpecKit format
- ✅ User never re-types requirements
- ✅ SpecKit reads from Flight Plan files, creates its own internal structure

---

**Version:** 3.0  
**Compatible with:** Flight Plan v1.0, SpecKit (latest)  
**Last Updated:** 2025-10-28
