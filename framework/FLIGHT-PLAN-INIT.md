# 🚀 Flight Plan Initialization

**Command:** `flight-plan init` or `flight-plan init apply`

**Usage:**
- `flight-plan init` - Preview project structure only
- `flight-plan init apply` - Generate projects

---

## 💡 FOR AI AGENTS: YOUR ROLE IN "flight-plan init"

**You are reading the execution instructions for "flight-plan init" command.**

### What This File Is:
This is **your step-by-step guide** for executing the `flight-plan init` command. The user has typed "flight-plan init" and you're now reading this to understand what to do.

### How You Got Here:
1. User typed: `"flight-plan init"`
2. You read: `framework/FLIGHT-PLAN-COMMANDS.md`
3. It directed you to: Read this file for execution steps
4. You're here now: Ready to follow the steps below

### What You'll Do:
**Follow the numbered STEPS below** (STEP 0, STEP 1, STEP 2, etc.) using your native capabilities:
- **Read files:** solution-prd-v*.md, README.md, GENERATOR.md
- **Detect location:** Check which directory we're in
- **Show preview:** Display what will be generated
- **Create files:** (only during "flight-plan init apply")

### Your Execution Method:
- ✅ Use your **file reading** tools
- ✅ Use your **file writing** tools (during apply)
- ✅ Use your **file system checks** (does file exist?)
- ✅ **Display output** to user
- ❌ No shell commands needed

### About npm/npx/uv References:
- If you see `npx` or `uv` commands mentioned later
- Those are for **SpecKit** (optional tool, separate from Flight Plan)
- You **guide the user** to run those (later, if they choose)
- You don't execute them yourself

### Ready to Start?
**Proceed to STEP 0 below** and follow each step in order. Use your file operations to perform the actions described.

**Flight Plan = These instructions. You = The executor using your capabilities.**

---

## Quick Reference

**Two-Step Process:**
1. `flight-plan init` → Preview structure (read-only, informational)
2. `flight-plan init apply` → Generate projects (creates files)

**During preview:** 
- Ask questions about the preview itself
- Explore what will be created
- Verify PRD was parsed correctly
- **Resolve open questions from PRD** (edit PRD, re-run `flight-plan init` to see updated preview)

**After preview:** Say "flight-plan init apply" to proceed

**Tip:** Resolve as many open questions as possible during preview. They become project blockers if unresolved.

---

## YOUR ROLE

When user says **"flight-plan init"**, show preview. When they say **"flight-plan init apply"**, generate structure.

---

## EXPECTED DIRECTORY STRUCTURES

**USER TYPICALLY OPENS:** Working folder (e.g., `MyApp/`) which may or may not contain `flight-plan-solution/`

### LEVEL 1: Working Folder (e.g., MyApp/)

**What you'll find here:**
- `flight-plan-solution/` subdirectory (if user has set up Flight Plan)
- `project-a/`, `project-b/`, etc. (if projects have been generated)
- User's other files

**What you WON'T find here:**
- ❌ `solution-prd-v*.md` (lives inside flight-plan-solution/)
- ❌ `framework/` (lives inside flight-plan-solution/)
- ❌ `test-case/`, `examples/`, `.git/` (these are repo markers)

**Example structure:**
```
MyApp/                          ← User opens this in IDE
├── flight-plan-solution/       ← Flight Plan tooling
├── project-a/                  ← Generated project (if any)
├── project-b/                  ← Generated project (if any)
└── [user's other files]
```

### LEVEL 2: flight-plan-solution/ Directory

**What you'll find here:**
- `solution-prd-v*.md` (one or more PRD versions)
- `README.md` (Flight Plan documentation)
- `LICENSE` (license file)
- `framework/` (all Flight Plan logic)
  - `FLIGHT-PLAN-INIT.md` (this file!)
  - `FLIGHT-PLAN-COMMANDS.md`
  - `FLIGHT-PLAN-PHASES.md`
  - `FLIGHT-PLAN-SPECKIT-SETUP.md`
  - `GENERATOR.md`
  - `BRIEF-BUILDER-v*.md`
  - `templates/` (file templates)
  - `ai-refs/` (working files, created during generation)
    - `solution-overview.md` (created on init/sync)
    - `notes.md` (created on init)
    - `cursor.md` (staging file for resolutions)
- `solution-rules.md` (created during init/sync)

**What you WON'T find here:**
- ❌ `test-case/` (repo marker)
- ❌ `examples/` (repo marker)
- ❌ `.git/` (repo marker)
- ❌ `GENERATOR.md` at root (should be in framework/)
- ❌ Generated projects (they go in parent directory)

**Example structure:**
```
flight-plan-solution/           ← Flight Plan tooling
├── solution-prd-v1.md          ← User's PRD (required!)
├── solution-prd-v1-1.md        ← Auto-revision (if created)
├── README.md                   ← Documentation
├── LICENSE
├── framework/                  ← All logic here
│   ├── FLIGHT-PLAN-INIT.md
│   ├── FLIGHT-PLAN-COMMANDS.md
│   ├── FLIGHT-PLAN-PHASES.md
│   ├── FLIGHT-PLAN-SPECKIT-SETUP.md
│   ├── GENERATOR.md
│   ├── BRIEF-BUILDER-v1.1.md
│   ├── templates/
│   │   ├── project-prd.md.template
│   │   ├── project-rules.md.template
│   │   ├── solution-rules.md.template
│   │   └── ...
│   └── ai-refs/                ← Created during generation
│       ├── solution-overview.md
│       ├── notes.md
│       └── cursor.md
└── solution-rules.md           ← Created during generation
```

### LEVEL 3: Generated Project Directory (e.g., project-a/)

**What you'll find here:**
- `docs/` (all documentation)
  - `project-prd.md`
  - `project-rules.md`
- `.flight-plan/` (Flight Plan internals - standalone copies)
  - `FLIGHT-PLAN-COMMANDS.md`
  - `FLIGHT-PLAN-PHASES.md`
  - `FLIGHT-PLAN-SPECKIT-SETUP.md`
  - `current.md`
  - `config.json`
  - `history/`
- `.cursor/rules/flight-plan.mdc` (pointer to docs/project-rules.md)
- `CLAUDE.md`, `GEMINI.md` (optional pointers)
- `.specify/` (if SpecKit enabled)
  - `memory/constitution.md`
  - `scripts/`
  - `templates/`
- `src/` (user's code)
- `README.md` (project overview)

**What you WON'T find here:**
- ❌ `solution-prd-v*.md` (lives in flight-plan-solution/)
- ❌ `framework/` (lives in flight-plan-solution/)
- ❌ References to `../../flight-plan-solution/` (projects are standalone)

**Example structure:**
```
project-a/                      ← Standalone project
├── docs/
│   ├── project-prd.md          ← Single source of truth
│   └── project-rules.md        ← AI integration
├── .flight-plan/               ← Standalone copies
│   ├── FLIGHT-PLAN-COMMANDS.md
│   ├── FLIGHT-PLAN-PHASES.md
│   ├── FLIGHT-PLAN-SPECKIT-SETUP.md
│   ├── current.md
│   ├── config.json
│   └── history/
├── .cursor/rules/
│   └── flight-plan.mdc
├── .specify/                   ← If SpecKit enabled
│   ├── memory/
│   │   └── constitution.md
│   ├── scripts/
│   └── templates/
├── src/                        ← Code
└── README.md
```

---

## DETECTION LOGIC FOR AI

When user opens a folder in IDE and runs a command:

**1. Check current directory for repo markers (WRONG if found):**
- `test-case/README.md` exists? → REPO, not solution
- `examples/` directory exists? → REPO, not solution
- `GENERATOR.md` at root level? → REPO, not solution

**2. Check current directory for solution markers:**
- `solution-prd-v*.md` exists? → In flight-plan-solution/
- `framework/` directory exists? → In flight-plan-solution/
- `README.md` + `LICENSE` exist? → Likely flight-plan-solution/

**3. Check current directory for project markers:**
- `docs/project-prd.md` exists? → In a generated project
- `.flight-plan/current.md` exists? → In a generated project

**4. Check for subdirectories:**
- `flight-plan-solution/` subdirectory exists? → In working folder
- Read files with `flight-plan-solution/` prefix

---

### STEP 0: CRITICAL DIRECTORY VALIDATION ⚠️

**BEFORE doing ANYTHING (including reading PRD), validate you're NOT in the repo itself:**

**DO NOT navigate directories. DO NOT run `cd ..`. Check ONLY the current working directory.**

**Step 1: Check current directory for repo markers**

Check if these files/directories exist in the CURRENT directory:
- `test-case/` (directory)
- `examples/` (directory)
- `GENERATOR.md` (file in root - not in framework/)

**Use file system checks (not shell commands):**
- Check if file/directory exists: `test-case/README.md`
- Check if file exists: `GENERATOR.md` (at root level)

**If ANY of these exist in CURRENT directory, STOP:**
```
❌ STOP: You're running from the Flight Plan repository itself!

This is WRONG. You need to set up a solution directory first.

You need to:
1. Create a solution directory somewhere OUTSIDE this repo
2. Copy this Flight Plan directory AS "flight-plan-solution" into your solution
3. Add your PRD to that flight-plan-solution/ directory
4. Run "flight-plan init" from THERE

Example structure you need:
  MySolution/
  └── flight-plan-solution/
      ├── solution-prd-v1.md  ← Your PRD
      ├── README.md
      ├── LICENSE
      └── framework/
          ├── GENERATOR.md
          ├── templates/
          └── ai-refs/
      └── ...

Flight Plan cannot run from the repository itself.
```

**STOP HERE. Do not proceed. Do not read PRD. Do not generate anything.**

---

**Step 2: Verify you're in "flight-plan-solution" directory**

Check if current working directory name is "flight-plan-solution".

If NOT in "flight-plan-solution" directory:
```
⚠️  Warning: Not in a "flight-plan-solution" directory

Expected directory name: flight-plan-solution

Example correct paths:
  C:\MySolution\flight-plan-solution\  ← Should be here
  ~/MySolution/flight-plan-solution/   ← Should be here

This keeps Flight Plan tooling separate from generated projects.

Recommended: Navigate to flight-plan-solution/ directory, then run "flight-plan init" again.
```

**STOP HERE. Do not proceed.**

---

### STEP 1: PREREQUISITES CHECK ⚠️

**Once in correct directory, verify required files exist:**

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

---

### STEP 2: VERIFY NOT IN REPO ⚠️

**Check directory structure (verify not running from repo)**

**Check parent directory contents:**
```bash
ls ../ 
```

**If parent contains Flight Plan repo files (LICENSE, examples/, templates/ at same level):**
```
❌ STOP: You're running from the Flight Plan repository itself!

This will create projects in the wrong location.

Correct setup:
1. Create your solution directory:
   mkdir MyApp
   cd MyApp

2. Copy Flight Plan tools here:
   cp -r /path/to/FlightPlan/ ./flight-plan-solution/

3. Copy your PRD:
   cp solution-prd-v1.md ./flight-plan-solution/

4. Open MyApp/flight-plan-solution/ in Cursor

5. Run "flight-plan init" again

This ensures projects are created in MyApp/, not in the repo's parent directory.

See README.md section "Set Up Your Solution Directory" for details.
```

**STOP HERE. Do not proceed until user has correct structure.**

**If PRD exists AND directory structure is correct:** Continue to Step 3...

---

### STEP 3: DETECT CURRENT LOCATION (NO NAVIGATION)

**User might open parent directory in Cursor, so detect where we are:**

**DO NOT run shell commands like `pwd` or `ls`. DO NOT navigate with `cd`. Use file system checks only.**

**Scenario A: Already in flight-plan-solution/**
- Check if `solution-prd-v*.md` exists in current directory (try to read it)
- Check if `framework/` directory exists (try to read `framework/GENERATOR.md`)
- Check if `README.md` exists in current directory

If YES to all: You're in `flight-plan-solution/`
- Project target directory: `../` (parent directory)
- File paths: Use `.` (e.g., `./README.md`, `./framework/GENERATOR.md`)
- Continue to STEP 4

**Scenario B: In parent directory (MyApp/)**
- Check if `flight-plan-solution/` subdirectory exists (try to read `flight-plan-solution/README.md`)
- Check if `flight-plan-solution/solution-prd-v*.md` exists

If YES: You're in parent directory
- Project target directory: `./` (current directory - projects go here)
- File paths: Use `flight-plan-solution/` prefix (e.g., `flight-plan-solution/README.md`)
- Continue to STEP 4 with adjusted paths

**If neither scenario matches:**
```
❌ Cannot locate flight-plan-solution directory or required files

Run "flight-plan init" from one of these locations:
- Inside flight-plan-solution/ directory
- Parent directory that contains flight-plan-solution/
```

**CRITICAL: Do NOT navigate directories. Just detect location and adjust file read paths accordingly.**

---

### STEP 4: LOAD CONTEXT

Read these files in order to understand the system:

1. **README.md** - Understand the Flight Plan system and workflow (INFORMATIONAL ONLY)
2. **solution-prd-v*.md** - The user's specific project requirements (latest version)
3. **framework/GENERATOR.md** - How generation works (don't execute yet, just understand)

**⚠️ NEVER read examples/ folder for project content - only read user's PRD**

**From the PRD, extract:**
- Section 1: Solution name
- Section 3: Project list (names, types, descriptions)
- Section 4: Architecture and dependencies
- Section 5: Technology decisions
- Section 6: Development tools and MCP servers
- **Section 8: OPEN QUESTIONS** ← These become blockers if unresolved!
- Section 9: Technical specifications per project

**CRITICAL:** Open Questions from Section 8 MUST be shown in preview. If PRD has no Section 8 or it's empty, say "No open questions."

---

### STEP 5: SHOW PREVIEW

Present a clear preview of what will be generated.

**🚨 CRITICAL: ALWAYS WRAP PREVIEW IN CODE BLOCKS 🚨**

**You MUST wrap the entire preview output in triple backticks (code blocks):**

````
```
🎯 Flight Plan Preview: ...
[your entire preview here]
```
````

**Why?** Without code blocks, CLI/IDE markdown renderers collapse ALL blank lines, making output unreadable even if your source is correct. This took 6 attempts to discover during testing with Claude Code.

**Code blocks preserve:**
- Blank lines between sections
- Blank lines after list items
- All spacing and formatting

**Without code block wrapper = Wall of unreadable text!**

---

**INSIDE CODE BLOCKS: Formatting Rules**

- ✅ Use blank lines between sections for readability
- ✅ Use bullet points (•) with space after for lists
- ✅ Use line breaks after each list item
- ✅ Indent nested content with 2 spaces
- ✅ Keep each decision/question on separate lines
- ✅ Add blank line after each project in PROJECT DETAILS
- ❌ DO NOT run text together in continuous paragraphs
- ❌ DO NOT omit line breaks between items

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

[For each project, format with blank line after each project block:]

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

• **[project-2-name]**
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

[Continue with blank line between each project...]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏗️  ARCHITECTURE (from PRD)

[Show high-level architecture if mentioned in PRD]
[Use ASCII diagram with clear line breaks:]

[Example for simple architecture:]
  User → portfolio-site → Netlify CDN
  Contact Form → Netlify Forms → Email

[Example for complex architecture - use blank lines between tiers:]
  Client Layer:
    frontend (React)
  
  API Layer:
    backend-api (Express)
      ↓
    auth-service (JWT)
  
  Data Layer:
    PostgreSQL database
    Redis cache

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔧 TECHNOLOGY DECISIONS (from PRD)

[Extract from PRD Section 5 and Section 11 (Design Rationale)]
[Format as bullet list with blank line after each decision:]

• [Decision 1 name]: [Brief reason from PRD]

• [Decision 2 name]: [Brief reason from PRD]

• [Decision 3 name]: [Brief reason from PRD]

[Example:]
• Database Choice (PostgreSQL): Chosen for ACID compliance and JSON support for flexible data modeling

• Authentication (JWT): Stateless auth for microservices, easier horizontal scaling

• Hosting (Vercel): Zero-config deployments, built-in CDN, serverless functions

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 OPEN QUESTIONS FROM PRD (Section 8)

⚠️  These questions from your PRD will become project blockers if unresolved:

[IF PRD Section 8 has questions, format with blank lines between each:]

• question-1: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]

• question-2: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]

• question-3: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]

[IF PRD Section 8 is empty or missing:]
✅ No open questions found in PRD.

💡 RECOMMENDATION: Resolve questions NOW during preview phase

How to resolve:
1. Discuss the questions with AI ("For Q1, I want PostgreSQL")
2. AI stages your resolutions (doesn't modify PRD yet)
3. Run "flight-plan init" again to see updated preview
4. Continue until satisfied
5. Run "flight-plan init apply"
   → Creates solution-prd-v[X]-[Y].md with resolutions
   → Generates projects with fewer blockers

Example: If using solution-prd-v2.md, Flight Plan creates solution-prd-v2-1.md

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⏱️  TIMELINE (from PRD if mentioned)

Expected Duration: [from PRD]
Team Size: [from PRD]
Constraints: [from PRD]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### STEP 6: DISCUSS & REFINE

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

**What happens now:**

**Option 1: User asks questions about the PREVIEW:**
- ✅ "What does project-rules.md do?"
- ✅ "Why is the backend using PostgreSQL?"
- ✅ "What's the difference between solution-rules and project-rules?"
- ✅ "Will SpecKit be set up automatically?"
→ Answer using context from README, PRD, and framework/GENERATOR

**Option 2: User wants to resolve open questions:**
- ✅ "Let's resolve question-1 about the database"
- ✅ "For question-2, I think we should use JWT authentication"
→ Discuss the questions and help them decide
→ AI stages resolutions in conversation context
→ Next preview shows simulated results
→ On `apply`, Flight Plan creates auto-revision PRD (e.g., v1-1.md)

**Option 3: User wants to change tech stack:**
- ✅ "Actually, let's use MongoDB instead of PostgreSQL"
→ Stage the change in conversation
→ On `apply`, creates auto-revision with changes

**Option 4: User is satisfied with preview:**
- ✅ "flight-plan init apply"
→ Creates auto-revision PRD if resolutions were made
→ Generates projects (Step 7)

**Preview is iterative:**
- User can run `flight-plan init` multiple times
- Resolve questions conversationally during preview
- When happy, `flight-plan init apply` creates auto-revision + projects

**Answer their questions using context from README, PRD, and framework/GENERATOR.**

---

### STEP 7: WAIT FOR APPLY COMMAND

Only when user says **"flight-plan init apply"**:

**First: Handle any staged resolutions**

1. **Detect current PRD version:**
   - List all `solution-prd-v*.md` files
   - Parse version numbers (v1.md = 1.0, v1-1.md = 1.1, v2.md = 2.0, v2-1.md = 2.1)
   - Find highest major version, then highest minor within that major
   - That's the "current PRD"

2. **If resolutions were staged during preview:**
   - Calculate next version (v1.md → v1-1.md, v1-1.md → v1-2.md, v2.md → v2-1.md)
   - Create new PRD file with resolutions integrated:
     ```markdown
     <!-- Flight Plan Auto-Revision -->
     <!-- Version: X.Y -->
     <!-- Base: solution-prd-vX.md or solution-prd-vX-Y.md -->
     <!-- Resolved: question-1 (Database), question-2 (Auth) -->
     <!-- Date: [timestamp] -->
     
     [Copy of base PRD with resolved questions removed/updated]
     ```
   - Show: "✅ Created solution-prd-vX-Y.md with your resolutions"

3. **If no resolutions staged:**
   - Use current PRD as-is

**Then: Generate projects**

1. Read **framework/GENERATOR.md** in full
2. Follow all instructions in framework/GENERATOR.md
3. Use the PRD (current or newly created auto-revision)
4. Create the structure
5. Report what was created

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
- **Follow framework/GENERATOR.md exactly** when creating structure

---

**Version:** 1.0  
**Last Updated:** 2025-10-25

