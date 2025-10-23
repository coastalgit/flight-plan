# 🚀 Flight Plan Initialization

**Trigger phrase:** "flight-plan init" or "initialize flight-plan"

---

## YOUR ROLE

When user says **"flight-plan init"**, follow this sequence:

### STEP 1: LOAD CONTEXT

Read these files in order to understand the system:

1. **README.md** - Understand the Flight Plan system and workflow
2. **solution-prd-v*.md** - The user's specific project requirements (latest version)
3. **GENERATOR.md** - How generation works (don't execute yet, just understand)

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

**DO NOT generate yet. DO NOT prompt "Proceed? (y/n)"**

Instead, have a natural conversation:

```
This is what I understand from your PRD. 

Let me know if:
- Tech stack looks correct for each project
- Project names and structure make sense  
- Dependencies are captured correctly
- Anything needs clarification

When you're ready to create this structure, just say:
"generate our flight plan" or "create the structure" or "build it"
```

**Natural discussion - NOT a menu.**

The user might:
- Ask questions about the structure
- Verify tech stack extraction
- Clarify dependencies
- Question technology decisions
- Want to resolve open questions before generating
- Request changes to the preview

**Answer their questions using context from README, PRD, and GENERATOR.**

**Wait for explicit confirmation phrases like:**
- "generate our flight plan"
- "create the structure"
- "build it"
- "make it happen"
- "let's do it"

---

### STEP 4: WAIT FOR GENERATION TRIGGER

Only when user says one of these phrases:

- "generate our flight plan"
- "create the structure"  
- "build it"
- "make it happen"

Then and ONLY then:
1. Read **GENERATOR.md** in full
2. Follow all instructions in GENERATOR.md
3. Create the structure
4. Report what was created

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

