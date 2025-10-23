# 🧠 Flight Plan Generator — AI Instructions

**Generator Version:** 1.0  
**Last Updated:** 2025-10-25

---

**Context:** You are reading this from `MyApp/flight-plan-solution/`.

**Purpose:** Generate complete project structure from `solution-prd-v*.md`.

**Output Location:** Projects created in **parent directory** (`../`), keeping Flight Plan tooling separate from actual projects.

---

## ⚠️ BEFORE YOU START

**This tool is for initial setup only.**

Check for existing projects in the parent directory (`../`):

### If Projects Already Exist:

```
⚠️ Projects detected.

This appears to be an existing solution. For updates, use:

  flight-plan sync

See FLIGHT-PLAN-COMMANDS.md for ongoing operations.

GENERATOR is for initial setup only.
```

**Stop here. Do not regenerate.**

### If No Projects Exist:

Continue with generation below...

---

## STEP 1: READ THE PRD

Find and read the latest PRD version: `solution-prd-v*.md`

Examples:
- `solution-prd-v1.md` only → Read v1
- Both v1 and v2 → Read v2 (latest)
- v1, v2, v3 → Read v3 (latest)

**Always read the highest version number.**

**Extract:**
- Projects to create (Section 3)
- Technology stack per project
- Architecture (Section 4)
- Technology decisions (Section 5)
- Development tools (Section 6)
- Open questions (Section 7)
- MCP servers or shared tools (if mentioned)

**⚠️ Do not invent. If essential info is missing → ask user.**

---

## STEP 2: ASK CLARIFYING QUESTIONS (During Preview)

**This happens during the preview/discussion phase, NOT after showing preview.**

**Ask ONLY if essential information is missing for the preview:**

Essential = needed for working structure:
- Project names
- Basic tech stack (language/framework)
- Project relationships

**Ask about:**
- Missing technology choices
- Unclear project dependencies
- Ambiguous architecture
- Deployment platforms if critical

**Don't ask about:**
- Things clearly stated in PRD
- Minor details (mark as "To be determined")
- Non-essential configuration

**Remember:** User will see full preview and can ask questions there too.

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
│   └── history/            # Empty directory
├── .cursor/
│   └── rules/
│       └── flight-plan.mdc # Use templates/cursor-rule.mdc.template
├── docs/                   # Empty directory
├── src/                    # Empty directory
└── README.md               # Generate from PRD
```

**Do NOT create:**
- `memory/constitution.md` (only via `flight-plan enable-speckit`)
- `specs/` (only via `flight-plan enable-speckit`)
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
- **Rules:** `project-rules.md` - How AI should work in this project

## Flight Plan Integration

This project uses Flight Plan methodology (8 phases).

**Commands:**
- `flight-plan status` - Show current state
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

**Display this message:**

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

🚀 Next Steps:

1. Review generated projects:
   cd ../[any-project]/
   cat project-prd.md

2. Start working on a project:
   cd ../[project]/
   "flight-plan status"

3. Optional - Enable Spec-Kit:
   cd ../[project]/
   "flight-plan enable-speckit"

4. See available commands:
   cat ../flight-plan-solution/FLIGHT-PLAN-COMMANDS.md

All projects start in Phase 1 (Define Mission).
The AI in each project will guide you through Flight Plan phases.

Happy building! 🛫
```

---

## IMPORTANT NOTES

**Preview → Discuss → Confirm → Execute:**
- ALWAYS triggered via FLIGHT-PLAN-INIT.md first (which shows preview)
- DO NOT execute generation without explicit confirmation
- Wait for user to say "generate our flight plan" or similar
- Have natural conversation during preview phase
- NO automatic "Proceed? (y/n)" prompts

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
