# 🚀 Flight Plan Initialization

**Command:** `flight-plan init` or `flight-plan init apply`

**Usage:**
- `flight-plan init` - Preview project structure only
- `flight-plan init apply` - Generate projects

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

### STEP 0: CRITICAL DIRECTORY VALIDATION ⚠️

**BEFORE doing ANYTHING (including reading PRD), validate you're NOT in the repo itself:**

**Step 1: Check current directory name and structure**
```bash
pwd
basename $(pwd)  # or Get-Location on Windows
```

**Step 2: Check if this looks like the Flight Plan REPOSITORY (wrong location)**

Signs you're in the REPO (not a solution):
- Current directory name is "FlightPlan" or similar (not "flight-plan-solution")
- Contains `test-case/` directory
- Contains `.git/` directory
- Contains `examples/` directory

**If ANY of these are true, STOP:**
```
❌ STOP: You're running from the Flight Plan repository itself!

This is WRONG. You need to set up a solution directory first.

Current location: [show pwd]

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

**Step 3: Verify directory name contains "flight-plan-solution"**

If current directory path does NOT contain "flight-plan-solution":
```
⚠️  Warning: Not in a "flight-plan-solution" directory

Current directory: [show pwd]

Expected: Your path should contain "flight-plan-solution"

Example:
  C:\MySolution\flight-plan-solution\  ← Should be here
  ~/MySolution/flight-plan-solution/   ← Should be here

This keeps Flight Plan tooling separate from generated projects.

Recommended: Set up correctly, then run "flight-plan init" again.
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

### STEP 3: DETECT CONTEXT & NAVIGATE

**User might open parent directory in Cursor, so check where we are:**

**Check current directory:**
```bash
pwd  # or cd on Windows
ls   # What's here?
```

**Scenario A: Already in flight-plan-solution/**
```
Current: MyApp/flight-plan-solution/
Files here: solution-prd-v1.md, README.md, framework/
Action: Continue (already in correct location)
Project target: ../ (MyApp/)
```

**Scenario B: In parent directory (MyApp/)**
```
Current: MyApp/
Files here: flight-plan-solution/ (subdirectory)
Action: Navigate to flight-plan-solution/ first
Project target: Current directory (.)
```

**Determine project creation path:**
- If in `flight-plan-solution/`: Create projects in `../`
- If in parent with `flight-plan-solution/` subdir: Create projects in `./`

**Navigate if needed:**
```bash
# If in parent directory:
cd flight-plan-solution/
```

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

📋 OPEN QUESTIONS FROM PRD (Section 8)

⚠️  These questions from your PRD will become project blockers if unresolved:

[IF PRD Section 8 has questions, list them:]
• question-1: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]
• question-2: [Question text from PRD] (Priority: High/Medium/Low)
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

