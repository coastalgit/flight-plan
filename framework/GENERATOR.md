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

**Users may open parent directory in Cursor, so we must detect context:**

**Check current working directory:**
```bash
pwd  # or cd on Windows
ls   # What files/folders are here?
```

**Scenario A: In flight-plan-solution/ directory**
```
Current: MyApp/flight-plan-solution/
Files here: solution-prd-v*.md, README.md, framework/
Project target: ../ (parent directory = MyApp/)
Templates: framework/templates/
```

**Scenario B: In parent directory (MyApp/)**
```
Current: MyApp/
Files here: flight-plan-solution/ (subdirectory visible)
Project target: ./ (current directory = MyApp/)
Need to: cd flight-plan-solution/ to read framework/templates
```

**Purpose:** Generate complete project structure from `solution-prd-v*.md`.

**Output Location:** Projects created **alongside** flight-plan-solution/ directory.

**Trigger:** This is ONLY called from `flight-plan init apply` (via FLIGHT-PLAN-INIT.md)

---

## ⚠️ CRITICAL: Validate NOT Running from Repo

**BEFORE doing ANYTHING, check if running from Flight Plan repository itself:**

**Step 1: Check current directory for repo markers**
```bash
pwd
ls
```

**Signs you're in the REPO (WRONG):**
- Current directory name is "FlightPlan" (not "flight-plan-solution")
- Contains `test-case/` directory
- Contains `.git/` directory
- Contains `examples/` directory

**If ANY of these exist, STOP IMMEDIATELY:**
```
❌ STOP: You're running from the Flight Plan repository itself!

This is WRONG. Projects would be created in the repo's parent directory.

Current location: [show pwd]

You need to:
1. Create a solution directory somewhere OUTSIDE this repo
2. Copy this Flight Plan directory AS "flight-plan-solution" into your solution
3. Add your PRD to that flight-plan-solution/ directory
4. Run "flight-plan init apply" from THERE

Expected structure:
  YourSolution/
  └── flight-plan-solution/
      ├── GENERATOR.md
      ├── templates/
      ├── solution-prd-v1.md  ← Your PRD
      └── ...

This ensures projects are created in YourSolution/, not in [parent of this repo].

See README.md section "Set Up Your Solution Directory" for details.
```

**STOP HERE. Do not proceed. Do not read PRD. Do not generate anything.**

---

**Step 2: If not in repo, verify directory path contains "flight-plan-solution"**

If current path does NOT contain "flight-plan-solution":
```
⚠️  Warning: Not in a "flight-plan-solution" directory

Current: [show pwd]

Expected: Path should contain "flight-plan-solution"

Recommended structure:
  YourSolution/
  └── flight-plan-solution/  ← Should be here

Please set up correctly first.
```

**STOP HERE if not in flight-plan-solution directory.**

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

Create `./framework/solution-rules.md` (in framework directory):

Use `framework/templates/solution-rules.md.template`

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

In `./framework/ai-refs/` (framework directory):

```
framework/ai-refs/
├── solution-overview.md    # All projects, dependencies, status
├── notes.md                # Empty (user's personal notes)
└── cursor.md               # Empty (AI working reference)
```

**solution-overview.md:**
```markdown
# Solution Overview: [Solution Name]

**PRD Version:** v[N]  
**Last Updated:** [date]

---

## PRD Version History

### v[N] ([date])
- **Action:** Initial project generation
- **Base PRD:** solution-prd-v[N].md
- **Generated Projects:** [project-1], [project-2], [project-3]
- **Notes:** [If auto-revision, list resolved questions; otherwise "User-created major version"]

---

## Projects

### [project-1-name]
- Type: [from PRD]
- Status: Phase 1 - Define Mission
- PRD Version: v[N] (generated from this version)
- Location: ../[project-1-name]/
- Dependencies: [internal + external]

### [project-2-name]
- Type: [from PRD]
- Status: Phase 1 - Define Mission
- PRD Version: v[N] (generated from this version)
- Location: ../[project-2-name]/
- Dependencies: [internal + external]

---

## Architecture

[Diagram from PRD Section 4]

---

## Shared Tools

[List from solution-rules.md]
```

---

## STEP 6: GENERATE PROJECTS

**Determine project creation path based on context (from Step 1):**

**If currently in flight-plan-solution/ directory:**
- Template path: `./framework/templates/`
- Project path: `../[project-name]/`

**If currently in parent directory (MyApp/):**
- Template path: `./flight-plan-solution/framework/templates/`
- Project path: `./[project-name]/`

**Project structure to create:**

```
[project-path]/[project-name]/
├── docs/
│   ├── project-prd.md          # Use templates/project-prd.md.template
│   └── project-rules.md        # Use templates/project-rules.md.template
├── .flight-plan/
│   ├── FLIGHT-PLAN-COMMANDS.md       # Use templates/project-commands.md.template
│   ├── FLIGHT-PLAN-PHASES.md         # COPY from flight-plan-solution/framework/
│   ├── FLIGHT-PLAN-SPECKIT-SETUP.md  # COPY from flight-plan-solution/framework/
│   ├── current.md                    # Use templates/flight-plan-current.md.template
│   ├── config.json                   # Use templates/config.json.template
│   └── history/                      # Empty directory
├── .cursor/
│   └── rules/
│       └── flight-plan.mdc     # Use templates/cursor-rule.mdc.template
├── src/                        # Empty directory
└── README.md                   # Generate from PRD
```

**Result:** Projects created alongside flight-plan-solution/ directory.

**CRITICAL:** `.flight-plan/` files to make project STANDALONE:
- `FLIGHT-PLAN-COMMANDS.md` - Generated from templates/project-commands.md.template (PROJECT-ONLY commands)
- `FLIGHT-PLAN-PHASES.md` - COPY from framework/ (phase standards, no refs to solution)
- `FLIGHT-PLAN-SPECKIT-SETUP.md` - COPY from framework/ (SpecKit setup, no refs to solution)

**IMPORTANT:** PRD and rules are in `docs/` to keep root clean.

**Do NOT create yet (created by SpecKit when enabled):**
- `.specify/` directory (created by `specify init . --ai cursor-agent --here`)
- `.specify/memory/constitution.md` (generated by `flight-plan setup-speckit` after SpecKit installed)
- Feature specs directories (created by `/speckit.specify` commands)
- `CLAUDE.md` or `CURSOR.md` (optional, created by SpecKit)

---

## STEP 7: FILL TEMPLATES

### docs/project-prd.md (Single Source of Truth)

Use `framework/templates/project-prd.md.template`

Create in `docs/` subdirectory of each project.

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

### docs/project-rules.md (AI Integration)

Use `framework/templates/project-rules.md.template`

Create in `docs/` subdirectory of each project.

**Fill:**
- Inherited MCP servers (from solution-rules.md)
- Project-specific MCP servers (if any)
- Technical stack (from project-prd.md)
- Quality standards (from PRD + solution-rules.md)
- Project-specific rules (API conventions, error handling, etc.)

**Default Spec-Kit:** `SPECKIT_ENABLED: No`

---

### .flight-plan/current.md (Status Tracking)

Use `framework/templates/flight-plan-current.md.template`

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

Use `framework/templates/cursor-rule.mdc.template`

**Fill:**
- PROJECT_NAME
- GENERATION_DATE (current date)

**No other changes needed** - template points to project-rules.md

---

### .flight-plan/config.json (Project Configuration)

Use `framework/templates/config.json.template`

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

---

## ⚠️ FOR AI AGENTS

**This README is INFORMATIONAL ONLY.**

When working in this project:
- ✅ **Read:** `docs/project-rules.md` for all project context
- ✅ **Read:** `docs/project-prd.md` for specifications
- ✅ **Read:** `.flight-plan/current.md` for current status
- ✅ **Read:** `.flight-plan/FLIGHT-PLAN-COMMANDS.md` when user runs commands
- ❌ **DO NOT** treat README as execution instructions

**This README explains the project. `docs/project-rules.md` has the actual AI context.**

---

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

- **Project PRD:** `docs/project-prd.md` - Complete specifications
- **Project Rules:** `docs/project-rules.md` - How AI should work in this project
- **Current Status:** `.flight-plan/current.md` - Current phase and progress
- **Configuration:** `.flight-plan/config.json` - Project settings
- **Commands Reference:** `.flight-plan/FLIGHT-PLAN-COMMANDS.md` - All Flight Plan commands (standalone)
- **Phase Standards:** `.flight-plan/FLIGHT-PLAN-PHASES.md` - Phase workflow and standards (standalone)
- **SpecKit Setup:** `.flight-plan/FLIGHT-PLAN-SPECKIT-SETUP.md` - SpecKit installation guide (standalone)

## Flight Plan Integration

This project uses Flight Plan methodology (8 phases).

**Getting Started:**
1. Run `flight-plan help` to see available commands
2. Run `flight-plan status` for guidance on what to do next
3. AI will guide you through current phase objectives
4. Testing runs automatically during build phases
5. Optional: Enable SpecKit when prompted for feature-level development

**Other Commands:**
- `flight-plan prd refresh` - Update tracking from PRD changes
- `flight-plan note [text]` - Add activity note

**Note:** This project is STANDALONE. All Flight Plan documentation is stored locally in `.flight-plan/` and `docs/`.

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

Generated [N] projects

📁 Structure (adjust paths based on where you ran command):
- If in flight-plan-solution/: Projects at ../[project-name]/
- If in parent directory: Projects at ./[project-name]/

All projects are alongside flight-plan-solution/ directory

All projects start in Phase 1 (Define Mission).

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 Next Steps:

1. Navigate to any project:
   - If in flight-plan-solution/: cd ../[project-name]/
   - If in parent directory: cd [project-name]/

2. Get guidance on what to do next:
   "flight-plan status"

The AI will guide you through:
- Reviewing your project requirements
- Resolving any open questions
- Test results (automatically shown in build phases)
- Optional SpecKit setup (if you want feature-level development)
- Moving through Flight Plan phases naturally

See all commands:
  - If in flight-plan-solution/: cat ../flight-plan-solution/FLIGHT-PLAN-COMMANDS.md
  - If in parent directory: cat flight-plan-solution/FLIGHT-PLAN-COMMANDS.md

Happy building! 🛫
```

**That's it! Generation is complete.**

**SpecKit prompting happens INSIDE each project when user runs `flight-plan status` (first time).**

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
- Don't create `.specify/` directory or SpecKit files (SpecKit creates these)
- Don't create AI pointer files beyond .cursor
- User can enable SpecKit via `flight-plan status` prompt

---

**Generator Version:** 1.0  
**Compatible with:** Flight Plan v1.0, solution-prd-v*.md format  
**Last Updated:** 2025-10-25
