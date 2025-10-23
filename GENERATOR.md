# 🧠 Flight Plan Generator — Local AI Instructions

**Generator Spec Version:** 1.0  
**Last Updated:** 2025-10-22

---

**You are reading this in `flight-plan-solution/` directory.**

Your job: Read `solution-prd-v*.md` and generate the complete project structure.

---

## ⚠️ BEFORE YOU START

**Check if this is initial setup or an update:**

Look for existing project directories (excluding `flight-plan-solution/`, `templates/`, `examples/`, `ai-refs/`).

### If Projects Already Exist:

```
⚠️ Projects detected in this directory.

This appears to be an existing solution. For updates, use:

  flight-plan sync

See FLIGHT-PLAN-COMMANDS.md for ongoing operations.

The GENERATOR is only for initial setup.
```

**Stop here. Do not regenerate.**

### If No Projects Exist:

Continue with initial generation below...

---

## STEP 1: READ THE PRD

**Find and read the latest PRD version:**

Look for files matching: `solution-prd-v*.md`

Examples:
- `solution-prd-v1.md` only → Read v1
- `solution-prd-v1.md` and `solution-prd-v2.md` → Read v2 (latest)
- `solution-prd-v1.md`, `v2.md`, `v3.md` → Read v3 (latest)

**Always read the highest version number.**

**⚠️ Do not invent missing projects or technologies. If unclear → ask user.**

This contains:
- Projects to create
- Technology stack
- Architecture
- Open questions
- Change history (if v2+)

---

## STEP 2: ASK CLARIFYING QUESTIONS

**If essential information is missing, ASK THE USER:**

Essential = needed to generate working structure:
- Project names
- Basic tech stack per project  
- Which projects depend on each other

**Ask about:**
- Missing technology choices (language, framework, database)
- Unclear project relationships
- Ambiguous architecture
- Deployment targets if needed for config

**Don't ask about:**
- Things clearly stated in PRD
- Minor details that can be "To be determined"
- Non-essential configuration

---

## STEP 3: GENERATE STRUCTURE

For each project in the PRD, create:

```
[project-name]/
├── .flight-plan/
│   ├── current.md              # Progress tracking
│   ├── requirements.md         # WHAT (tech-agnostic, PM-friendly)
│   ├── implementation.md       # HOW (tech-specific details)
│   ├── history/                # Empty
│   ├── prompts/
│   │   ├── successful.md       # Empty template
│   │   └── failed.md           # Empty template
│   └── decisions/              # Empty
│
├── docs/
│   ├── third-party/            # Empty
│   ├── snippets/               # Empty
│   ├── research/               # Empty
│   └── logs/                   # Empty
│
├── .cursor/
│   └── rules/
│       └── flight-plan.mdc     # Copy from templates/
│
└── README.md                   # Generate from PRD
```

**File Purposes:**
- **requirements.md** - Business requirements, user stories, success criteria (no tech)
- **implementation.md** - Technology stack, architecture, framework choices, deployment

---

## STEP 4: CREATE COORDINATION FILES

In `flight-plan-solution/` root:

```
ai-refs/
├── solution-overview.md        # Cross-project status (include PRD version)
├── notes.md                    # User's personal notes/thoughts
└── [your-name].md              # Your working reference (e.g., cursor.md)
```

**IMPORTANT:** Rebuild `solution-overview.md` whenever a new PRD version is adopted. Include PRD version and date at top. This ensures all AIs see current project relationships and dependencies.

---

## TEMPLATES

Use files in `templates/` directory:
- `flight-plan-current.md.template` → `.flight-plan/current.md`
- `flight-plan-requirements.md.template` → `.flight-plan/requirements.md`
- `flight-plan-implementation.md.template` → `.flight-plan/implementation.md`
- `cursor-rule.mdc.template` → `.cursor/rules/flight-plan.mdc`

Fill in project-specific details from the PRD.

### Template Variables Reference

Replace these placeholders with values from the PRD:

| Variable | Source (PRD Section) | Example | If Missing |
|----------|---------------------|---------|------------|
| `{{PROJECT_NAME}}` | Section 3 - Project name | "portfolio-site" | Ask user |
| `{{PROJECT_TYPE}}` | Section 3 - Type field | "Frontend (Static Site)" | "To be determined" |
| `{{PRD_VERSION}}` | Filename version | "v1" | Use latest |
| `{{SOLUTION_NAME}}` | Section 1 - Title | "Portfolio Website" | Use PRD title |
| `{{CREATED_DATE}}` | Current date | "2025-10-22 14:00 UTC" | Generate now |
| `{{LAST_UPDATED}}` | Current date | "2025-10-22 14:00 UTC" | Generate now |
| `{{GENERATION_DATE}}` | Current date | "2025-10-22 14:00 UTC" | Generate now |
| `{{PROJECT_PURPOSE}}` | Section 3 - Purpose | "Personal portfolio..." | Extract from PRD |
| `{{TOOLS_LIST}}` | Section 6 - Development Tools | "VS Code, Git, Chrome DevTools" | "To be determined" |
| `{{BLOCKERS_FROM_PRD}}` | Section 7 - Open Questions | "question-1: Layout approach" | "None currently" |
| `{{RELATED_PROJECTS}}` | Section 3 - Dependencies | "backend, admin-ui" | "None (standalone)" |
| `{{TECH_STACK_SUMMARY}}` | Section 3 - Technology | "HTML/CSS/JS on Netlify" | Extract or ask |
| `{{LANGUAGE}}` | Section 3 - Language | "JavaScript" | Ask if missing |
| `{{FRAMEWORK}}` | Section 3 - Framework | "None (vanilla)" | Ask if missing |
| `{{DATABASE_TYPE}}` | Section 3 - Database | "Not applicable" | "To be determined" |
| `{{DEPLOYMENT_PLATFORM}}` | Section 3 - Deployment | "Netlify" | Ask if missing |
| `{{TARGET_USERS}}` | Section 2 or infer | "Potential clients, recruiters" | Infer from problem |
| `{{PROBLEM_STATEMENT}}` | Section 1 | "Need online presence..." | Extract verbatim |
| `{{USER_STORIES}}` | Infer from problem | "As a client, I want..." | Generate from problem |
| `{{OPEN_QUESTIONS_FROM_PRD}}` | Section 7 | "- question-1: Layout..." | Copy directly |

**If PRD is missing required info:**
- Essential fields (PROJECT_NAME, TECH_STACK) → Ask user
- Optional fields → Use "To be determined"
- Never invent details → Mark as TBD or ask

**Date Format:** Always UTC ISO 8601 (YYYY-MM-DD HH:MM UTC)

---

## WHAT TO GENERATE

### .flight-plan/current.md
```markdown
# Flight Plan: [project-name]

**Status:** Phase 1 - Define Mission (STARTING)
**Created:** [today's date]
**Project Type:** [from PRD]

## Current Phase: 1 - DEFINE THE MISSION

### ✅ Completed Tasks
- [ ] Problem statement defined
- [ ] Requirements documented in spec.md
- [ ] Success metrics established

### 🛠 Tools Being Used
[List from PRD]

### 📋 Next Steps
1. Complete requirements in spec.md
2. Define success metrics
3. Move to Phase 2

### 🚧 Blockers
[List from PRD Open Questions if they block this project]

---

## Project Context

**Solution:** [from PRD]
**Related Projects:** [from PRD]
**Tech Stack:** [from PRD]
```

### .flight-plan/requirements.md (WHAT - Tech Agnostic)
Extract business requirements from PRD:
```markdown
# [project-name] - Requirements

**Version:** 0.1
**Status:** Draft
**PRD Source:** solution-prd-v[N].md

## Overview
[From PRD project purpose - no tech mentioned]

## User Stories
[Extract from PRD]

## Functional Requirements
[What the system must do - no implementation details]

## Success Criteria
[How we know it works - measurable outcomes]

## Constraints
[Business/regulatory/budget constraints]

## Open Questions
[From PRD - questions affecting this project]
```

### .flight-plan/implementation.md (HOW - Tech Specific)
Extract technical details from PRD:
```markdown
# [project-name] - Implementation

**Version:** 0.1
**Status:** Draft
**PRD Source:** solution-prd-v[N].md

## Technology Stack
[From PRD - language, framework, database, deployment]

## Architecture
[Technical architecture - APIs, data flow, integrations]

## Dependencies
**Internal:** [Other projects from PRD]
**External:** [Third-party services, libraries]

## Deployment
[Hosting, CI/CD, environments]

## Technical Decisions
[Key tech choices and reasoning from PRD]
```

### .cursor/rules/flight-plan.mdc
```markdown
---
description: Flight Plan integration
alwaysApply: true
---

# Flight Plan Awareness

On first interaction:
1. Read ../../flight-plan-solution/solution-prd-v[latest].md
2. Read ./.flight-plan/current.md (progress)
3. Read ./.flight-plan/requirements.md (what to build)
4. Read ./.flight-plan/implementation.md (how to build)

When working:
- Update .flight-plan/current.md
- Update implementation.md for tech changes
- Update requirements.md for scope changes
- Log successful prompts
- Note decisions
```

---

## AFTER GENERATION

Tell the user:

```
✅ Structure generated!

Created projects:
- [list projects]

Each has:
- .flight-plan/ (progress tracking)
- docs/ (reference materials)
- .cursor/rules/ (IDE integration)

To start:
cd [any-project]/
Open .flight-plan/current.md
Begin Phase 1: Define the Mission
```

---

## YOUR WORKING FILE

Create `flight-plan-solution/ai-refs/[your-name].md`:

```markdown
# [Your Name] Working Reference

**Created:** [date]
**Solution:** [from PRD]

## Projects Status
[List each project and current phase]

## Last Actions
[What you just generated]

## Next Session
When I return:
1. Read this file
2. Check solution-overview.md
3. Continue work
```

---

**That's it! Read PRD → Ask questions if needed → Generate structure → Done.**
