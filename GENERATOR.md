# 🧠 Flight Plan Generator — AI Instructions

**Generator Version:** 1.0  
**Last Updated:** 2025-10-25

---

## Execution Flow

This file is called by FLIGHT-PLAN-INIT.md when user says "flight-plan init apply"

```
Preview Phase (FLIGHT-PLAN-INIT.md)  →  Execution Phase (THIS FILE)
     ↓ User sees preview                    ↓ Actually creates files
     ↓ Asks questions                       ↓ Follows these instructions
     ↓ Says "flight-plan init apply"        ↓ Reports completion
```

---

## Context Detection

**How to know you're in the right place:**
1. Check for `solution-prd-v*.md` in current directory
2. Check for `templates/` subdirectory
3. Check for this file (`GENERATOR.md`)
4. If all present: You're in `flight-plan-solution/` context

**Purpose:** Generate complete project structure from `solution-prd-v*.md`.

**Output Location:** Projects created in **parent directory** (`../`), keeping Flight Plan tooling separate from actual projects.

**Trigger:** This is ONLY called from `flight-plan init apply` (via FLIGHT-PLAN-INIT.md)

---

## ⚠️ BEFORE YOU START

**This tool is for initial setup only.**

### Prerequisites Check

**1. Verify solution-prd-v*.md exists in current directory:**
   - Must find at least one: `solution-prd-v1.md`, `solution-prd-v2.md`, etc.
   - If missing: **STOP and tell user (see FLIGHT-PLAN-INIT.md Step 0)**

**2. Check for existing projects in parent directory (`../`):**

### If PRD Missing:

```
❌ FATAL: No solution PRD found

Cannot generate without solution-prd-v*.md file.
See FLIGHT-PLAN-INIT.md for proper workflow.

NEVER use examples/ folder as source material!
```

**STOP. Do not proceed.**

---

### If Projects Already Exist:

```
⚠️ Projects detected.

This appears to be an existing solution. For updates, use:

  flight-plan sync

See FLIGHT-PLAN-COMMANDS.md for ongoing operations.

GENERATOR is for initial setup only.
```

**Stop here. Do not regenerate.**

---

### If PRD Exists AND No Projects Exist:

Continue with generation below...

---

## STEP 1: READ THE PRD

Find and read the latest PRD version in **current directory only**: `solution-prd-v*.md`

Examples:
- `solution-prd-v1.md` only → Read v1
- Both v1 and v2 → Read v2 (latest)
- v1, v2, v3 → Read v3 (latest)

**Always read the highest version number.**

**🚨 CRITICAL - Read ONLY user's PRD:**
- ✅ Read: `solution-prd-v*.md` in current directory
- ❌ NEVER read: `examples/` folder
- ❌ NEVER read: Example PRDs from templates
- ❌ NEVER use: portfolio-site or any example as template

**Extract from user's PRD:**
- Projects to create (Section 3)
- Technology stack per project
- Architecture (Section 4)
- Technology decisions (Section 5)
- Development tools (Section 6)
- Open questions (Section 7)
- MCP servers or shared tools (if mentioned)

**⚠️ Do not invent.** Use defaults or "To be determined" for missing info.

---

## STEP 2: EXTRACT INFORMATION

Read the latest PRD version: `solution-prd-v*.md`

Extract all required information using template variables table (bottom of this file).

**If information is missing:**
- Use "To be determined" for technology choices
- Use "Not applicable" for optional items (e.g., database if not needed)
- Use reasonable defaults for development standards
- Trust that preview phase resolved critical questions

**Do NOT ask questions at this stage** - you're in execution mode.

The preview phase (FLIGHT-PLAN-INIT.md) handled clarifications.

**This is silent execution:**
- No confirmations needed
- No "Should I...?" prompts
- Just create the structure as extracted from PRD

---

## STEP 3: EXTRACT SOLUTION-WIDE RULES

**Identify solution-level rules to go in `solution-rules.md`:**

Look for in PRD:
- MCP servers mentioned
- Development tools (Git, testing, CI/CD)
- Security standards
- Quality standards (coverage, performance)
- Shared services (auth, database)
- Solution architecture

**These apply to ALL projects.**

---

## STEP 4: GENERATE solution-rules.md

Create `./solution-rules.md` (in flight-plan-solution directory):

Use `templates/solution-rules.md.template`

**Fill in:**
- MCP servers (if mentioned in PRD Section 6)
- Development tools (from PRD Section 6)
- Security standards (from PRD)
- Quality standards (test coverage, etc.)
- Solution architecture (from PRD Section 4)
- Git workflow
- CI/CD approach

**If not mentioned:** Use reasonable defaults or "To be determined"

---

## STEP 5: CREATE COORDINATION FILES

In `./ai-refs/` (current directory):

```
ai-refs/
├── solution-overview.md    # All projects, dependencies, status
├── notes.md                # Empty (user's personal notes)
└── cursor.md               # Empty (AI working reference)
```

**solution-overview.md:**
```markdown
# Solution Overview: [Solution Name]

**PRD Version:** v[N]  
**Last Updated:** [date]

## Projects

### [project-1-name]
- Type: [from PRD]
- Status: Phase 1 - Define Mission
- Location: ../[project-1-name]/
- Dependencies: [internal + external]

### [project-2-name]
- Type: [from PRD]
- Status: Phase 1 - Define Mission
- Location: ../[project-2-name]/
- Dependencies: [internal + external]

## Architecture

[Diagram from PRD Section 4]

## Shared Tools

[List from solution-rules.md]
```

---

## STEP 6: GENERATE PROJECTS

For each project in the PRD, create in parent directory (`../`):

```
../[project-name]/
├── project-prd.md          # Use templates/project-prd.md.template
├── project-rules.md        # Use templates/project-rules.md.template
├── .flight-plan/
│   ├── current.md          # Use templates/flight-plan-current.md.template
│   ├── config.json         # Use templates/config.json.template
│   └── history/            # Empty directory
├── .cursor/
│   └── rules/
│       └── flight-plan.mdc # Use templates/cursor-rule.mdc.template
├── docs/                   # Empty directory
├── src/                    # Empty directory
└── README.md               # Generate from PRD
```

**Do NOT create yet (created by setup-speckit command):**
- `memory/constitution.md` (only via `flight-plan setup-speckit`)
- `specs/` (only via `flight-plan setup-speckit`)
- `CLAUDE.md` (optional, user can add later)

---

## STEP 7: FILL TEMPLATES

### project-prd.md (Single Source of Truth)

Use `templates/project-prd.md.template`

**Fill from PRD:**

| Field | Source | Example |
|-------|--------|---------|
| PROJECT_NAME | Section 3 | "backend-api" |
| SOLUTION_NAME | Section 1 title | "MyApp" |
| SOLUTION_VERSION | PRD version | "v1" |
| PROJECT_PURPOSE | Section 3 | "REST API for frontend" |
| PROJECT_TYPE | Section 3 | "Backend API" |
| TARGET_USERS | Section 2 or infer | "Frontend, mobile apps" |
| PROBLEM_STATEMENT | Section 1 | Extract verbatim |
| FUNCTIONAL_REQUIREMENTS | Section 3 | Extract per project |
| USER_STORIES | Infer from requirements | "As a frontend, I want..." |
| SUCCESS_CRITERIA | Section 3 | Extract metrics |
| LANGUAGE | Section 3 | "Node.js" or "TBD" |
| FRAMEWORK | Section 3 | "Express" or "TBD" |
| DATABASE | Section 3 | "PostgreSQL" or "N/A" |
| DEPLOYMENT | Section 3 | "Railway" or "TBD" |
| ARCHITECTURE_DESCRIPTION | Section 4 | Project-specific part |
| INTERNAL_DEPENDENCIES | Section 3 | Other projects |
| EXTERNAL_DEPENDENCIES | Section 3 | Third-party services |
| OPEN_QUESTIONS | Section 7 | Filter by project |
| TECHNOLOGY_DECISIONS | Section 5 | Filter by project |

**Open Questions:** Only include questions that affect THIS project (check "Blocks" field)

---

### project-rules.md (AI Integration)

Use `templates/project-rules.md.template`

**Fill:**
- Inherited MCP servers (from solution-rules.md)
- Project-specific MCP servers (if any)
- Technical stack (from project-prd.md)
- Quality standards (from PRD + solution-rules.md)
- Project-specific rules (API conventions, error handling, etc.)

**Default Spec-Kit:** `SPECKIT_ENABLED: No`

---

### .flight-plan/current.md (Status Tracking)

Use `templates/flight-plan-current.md.template`

**Fill:**
- PROJECT_NAME
- LAST_UPDATED (current date)
- PRD_VERSION (from PRD filename)
- SYNC_DATE (current date)
- OPEN_ITEMS (from filtered Open Questions)
- RECENT_NOTES (empty or "Initial setup")

**Phase:** Always start at "1 - Define Mission"

---

### .cursor/rules/flight-plan.mdc (Cursor Pointer)

Use `templates/cursor-rule.mdc.template`

**Fill:**
- PROJECT_NAME
- GENERATION_DATE (current date)

**No other changes needed** - template points to project-rules.md

---

### .flight-plan/config.json (Project Configuration)

Use `templates/config.json.template`

**Fill with:**
```json
{
  "project_name": "[project-name from PRD]",
  "generated_date": "[current date in YYYY-MM-DD HH:MM UTC]",
  "prd_version": "v1",
  "speckit_enabled": false,
  "speckit_prompted": false,
  "current_phase": 1,
  "last_updated": "[current date in YYYY-MM-DD HH:MM UTC]"
}
```

**Note:** SpecKit status will be updated after user responds to SpecKit prompt in STEP 2.

---

### README.md (Project Overview)

Generate from PRD:

```markdown
# [Project Name]

**Part of:** [Solution Name]  
**Type:** [Project Type]  
**Status:** Phase 1 - Define Mission

## Overview

[Project purpose from PRD]

## Tech Stack

- **Language:** [from PRD]
- **Framework:** [from PRD]
- **Database:** [from PRD]
- **Deployment:** [from PRD]

## Quick Start

[Add basic setup instructions]

## Documentation

- **Project PRD:** `project-prd.md` - Complete specifications
- **Status:** `.flight-plan/current.md` - Current phase and progress
- **Configuration:** `.flight-plan/config.json` - Project settings
- **Rules:** `project-rules.md` - How AI should work in this project

## Flight Plan Integration

This project uses Flight Plan methodology (8 phases).

**Commands:**
- `flight-plan next` - Get guidance on what to do next
- `flight-plan status` - Show current state
- `flight-plan setup-speckit` - Enable SpecKit integration (optional)
- `flight-plan prd refresh` - Update tracking from PRD changes

See: `../flight-plan-solution/FLIGHT-PLAN-COMMANDS.md`

## Architecture

[Include diagram from PRD if relevant to this project]

## Dependencies

**Internal:** [Other projects from same solution]  
**External:** [Third-party services]
```

---

## TEMPLATE VARIABLES REFERENCE

All templates use `{{VARIABLE}}` placeholders:

| Variable | Description | Default if Missing |
|----------|-------------|--------------------|
| PROJECT_NAME | Project folder name | Ask user |
| SOLUTION_NAME | Solution title | From PRD Section 1 |
| SOLUTION_VERSION | PRD version (v1, v2) | Use latest |
| PROJECT_TYPE | Frontend/Backend/Functions | "TBD" |
| PROJECT_PURPOSE | One-line description | Extract from PRD |
| LANGUAGE | Programming language | "To be determined" |
| FRAMEWORK | Web/app framework | "To be determined" |
| DATABASE | Database system | "Not applicable" |
| DEPLOYMENT | Hosting platform | "To be determined" |
| CREATED_DATE | Generation timestamp | Now (UTC) |
| LAST_UPDATED | Last modified | Now (UTC) |
| GENERATION_DATE | Template fill date | Now (UTC) |
| PRD_VERSION | Source PRD version | v1, v2, etc. |
| OPEN_ITEMS | Filtered open questions | Per project |
| INTERNAL_DEPENDENCIES | Same-solution projects | Extract from PRD |
| EXTERNAL_DEPENDENCIES | Third-party services | Extract from PRD |

**Date Format:** Always `YYYY-MM-DD HH:MM UTC`

---

## AFTER GENERATION

**Show Completion Summary:**

```
✅ Flight Plan Initialized

Generated [N] projects in ../

📁 Structure:
../[project-1]/
../[project-2]/
../[project-3]/

📋 Solution Files:
./solution-rules.md (shared rules)
./ai-refs/ (coordination)

All projects start in Phase 1 (Define Mission).

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 Next Steps:

1. Navigate to any project:
   cd ../[project-name]/

2. Get guidance on what to do next:
   "flight-plan status"

The AI will guide you through:
- Reviewing your project requirements
- Resolving any open questions
- Test results (automatically shown in build phases)
- Optional SpecKit setup (if you want feature-level development)
- Moving through Flight Plan phases naturally

See all commands:
  cat ../flight-plan-solution/FLIGHT-PLAN-COMMANDS.md

Happy building! 🛫
```

**That's it! Generation is complete.**

**SpecKit prompting happens INSIDE each project when user runs `flight-plan next`.**

---

## IMPORTANT NOTES

**Apply Pattern:**
- ALWAYS triggered via `flight-plan init apply` command
- FLIGHT-PLAN-INIT.md handles preview with `flight-plan init`
- This file (GENERATOR) is ONLY called when user says "flight-plan init apply"
- DO NOT execute generation without "apply" in the command
- Preview happens in FLIGHT-PLAN-INIT.md, execution happens here

**Extraction, Not Invention:**
- Do NOT invent details not in PRD
- Mark unknowns as "To be determined"
- Ask user if essential info is missing

**File Locations:**
- Projects: Parent directory (`../`)
- Solution files: Current directory (`./`)
- Templates: `./templates/`

**Version Consistency:**
- All projects reference same PRD version
- solution-overview.md shows PRD version
- Each project-prd.md notes source version

**Minimal by Default:**
- Don't create Spec-Kit files (memory/, specs/)
- Don't create AI pointer files beyond .cursor
- User can add CLAUDE.md, etc. later

---

**Generator Version:** 1.0  
**Compatible with:** Flight Plan v1.0, solution-prd-v*.md format  
**Last Updated:** 2025-10-25
