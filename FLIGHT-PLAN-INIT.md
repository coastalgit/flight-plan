# 🚀 Flight Plan Initialization

**Command:** `flight-plan init` or `flight-plan init apply`

**Usage:**
- `flight-plan init` - Preview project structure only
- `flight-plan init apply` - Generate projects

---

## Quick Reference

**Two-Step Process:**
1. `flight-plan init` → Preview structure (read-only)
2. `flight-plan init apply` → Generate projects (creates files)

**During preview:** Ask questions, verify understanding  
**After preview:** Say "flight-plan init apply" to proceed

---

## YOUR ROLE

When user says **"flight-plan init"**, show preview. When they say **"flight-plan init apply"**, generate structure.

### STEP 0: PREREQUISITES CHECK ⚠️

**BEFORE doing anything, verify required files exist:**

1. **Check for solution-prd-v*.md in current directory**
   - Look for: `solution-prd-v1.md`, `solution-prd-v2.md`, etc.
   - Need: At least ONE version file

**If PRD file is missing:**
```
❌ Cannot proceed: No solution PRD found

Required: solution-prd-v1.md (or higher version)
Location: This directory (flight-plan-solution/)

To create the PRD:
1. Prepare your brainstorm/brief
2. Use BRIEF-BUILDER-v1.0.md to generate the PRD
3. Copy the generated PRD to this directory as solution-prd-v1.md
4. Then run "flight-plan init" again

IMPORTANT: Do NOT use examples/ folder as a template!
Each solution needs its own PRD based on YOUR requirements.
```

**STOP HERE. Do not proceed. Do not show preview. Do not use examples.**

**If PRD exists:** Continue to Step 1...

---

### STEP 1: LOAD CONTEXT

Read these files in order to understand the system:

1. **README.md** - Understand the Flight Plan system and workflow
2. **solution-prd-v*.md** - The user's specific project requirements (latest version)
3. **GENERATOR.md** - How generation works (don't execute yet, just understand)

**⚠️ NEVER read examples/ folder for project content - only read user's PRD**

---

### STEP 2: SHOW PREVIEW

Present a clear preview of what will be generated:

```
🎯 Flight Plan Preview: [Solution Name from PRD]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 SUMMARY
PRD Version: v[N]
Projects: [N] projects identified
Location: Projects will be created in ../
Author: [from PRD if mentioned]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 DIRECTORY STRUCTURE

MyApp/
├── flight-plan-solution/
│   ├── solution-prd-v1.md (existing)
│   ├── solution-rules.md (will create)
│   └── ai-refs/ (will create)
│       ├── solution-overview.md
│       ├── notes.md
│       └── cursor.md
│
├── [project-1-name]/ (will create)
│   ├── project-prd.md
│   ├── project-rules.md
│   ├── .flight-plan/
│   │   └── current.md
│   ├── .cursor/rules/flight-plan.mdc
│   └── README.md
│
├── [project-2-name]/ (will create)
│   └── [same structure]
│
└── [project-3-name]/ (will create)
    └── [same structure]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📦 PROJECT DETAILS

[For each project, show:]

• **[project-name]**
  Type: [Frontend/Backend/Functions/etc from PRD]
  Purpose: [one-line from PRD]
  
  Technology Stack:
    Language: [from PRD or "To be determined"]
    Framework: [from PRD or "To be determined"]
    Database: [from PRD or "Not applicable" or "To be determined"]
    Deployment: [from PRD or "To be determined"]
    
  Dependencies:
    Internal: [other projects from PRD]
    External: [third-party services from PRD]

[Repeat for each project]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏗️  ARCHITECTURE (from PRD)

[Show high-level architecture if mentioned in PRD]
Example:
  User → portfolio-site → Netlify CDN
  Contact Form → Netlify Forms → Email

[Or if complex:]
  frontend → backend-api → PostgreSQL
              ↓
          auth-service
              ↓
          email-functions

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔧 TECHNOLOGY DECISIONS (from PRD)

[List key tech decisions and reasoning:]
• [Decision 1]: [Reason from PRD]
• [Decision 2]: [Reason from PRD]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️  OPEN QUESTIONS (from PRD)

[List with priority:]
• question-1: [Question text] (Priority: High/Medium/Low)
  → Will be added to: [project-name] blockers
• question-2: [Question text] (Priority: High/Medium/Low)
  → Will be added to: [project-name] blockers

Each project will only see questions that affect them.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⏱️  TIMELINE (from PRD if mentioned)

Expected Duration: [from PRD]
Team Size: [from PRD]
Constraints: [from PRD]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### STEP 3: DISCUSS & REFINE

After showing preview, end with separator and **wait silently**.

**Do not add any prompt:**
❌ "Ready to proceed?"
❌ "Shall I generate this?"
❌ "Does this look correct?"
❌ "Would you like me to create this structure?"
✅ Just stop after the separator line

End preview like this:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Then STOP. Say nothing more.**

The preview shows everything. User can:
- Ask questions about the structure
- Verify tech stack extraction
- Clarify dependencies
- Question technology decisions
- Want to resolve open questions
- Request changes to the preview
- Say "flight-plan init apply" to proceed

**Answer their questions using context from README, PRD, and GENERATOR.**

---

### STEP 4: WAIT FOR APPLY COMMAND

Only when user says **"flight-plan init apply"**:

1. Read **GENERATOR.md** in full
2. Follow all instructions in GENERATOR.md
3. Create the structure
4. Report what was created

**Do NOT execute on:**
- "generate our flight plan" (old pattern)
- "create the structure" (old pattern)
- "build it" (old pattern)
- Any other phrases

**ONLY on:** `flight-plan init apply`

---

## IMPORTANT

- **Show ALL tech stack details** - language, framework, database, deployment
- **Indicate "To be determined"** if PRD doesn't specify something
- **Extract architecture** from PRD Section 4
- **List technology decisions** from PRD Section 5
- **Don't rush to generate** - let user verify understanding first
- **Answer questions** using the README and PRD context
- **Only generate** when explicitly triggered
- **Follow GENERATOR.md exactly** when creating structure

---

**Version:** 1.0  
**Last Updated:** 2025-10-25

