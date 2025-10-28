# Directory Structure Clarity - 2025-10-27

## Problem

**User Workflow:**
1. User opens working folder (e.g., `MyApp/`) in their IDE
2. That folder may or may not have `flight-plan-solution/` subdirectory
3. AI needs to understand what files/directories should exist at each level

**Previous State:**
- Structure information was scattered across docs
- No single reference showing what AI should expect at each level
- Unclear what files belong where
- AI had to guess about directory structure

**Impact:**
- AI confusion about which directory level it was in
- Incorrect assumptions about file locations
- Navigation issues (stepping back directories)

## Solution

Added comprehensive **"EXPECTED DIRECTORY STRUCTURES"** section to both:
1. `framework/FLIGHT-PLAN-INIT.md` (detailed, for init logic)
2. `framework/FLIGHT-PLAN-COMMANDS.md` (summary, for all commands)

### Structure Reference Added

**Three clear levels defined:**

#### Level 1: Working Folder (MyApp/)
```
MyApp/                          ← User opens this in IDE
├── flight-plan-solution/       ← Flight Plan tooling
├── project-a/                  ← Generated project
├── project-b/                  ← Generated project
└── [user's other files]
```

**What's here:**
- ✅ `flight-plan-solution/` subdirectory
- ✅ Generated projects

**What's NOT here:**
- ❌ `solution-prd-v*.md` (lives in flight-plan-solution/)
- ❌ `framework/` (lives in flight-plan-solution/)
- ❌ `test-case/`, `examples/`, `.git/` (repo markers)

#### Level 2: flight-plan-solution/ Directory
```
flight-plan-solution/
├── solution-prd-v1.md          ← User's PRD
├── solution-prd-v1-1.md        ← Auto-revision (if created)
├── README.md, LICENSE
├── framework/                  ← All Flight Plan logic
│   ├── FLIGHT-PLAN-INIT.md
│   ├── FLIGHT-PLAN-COMMANDS.md
│   ├── GENERATOR.md
│   ├── templates/
│   └── ai-refs/
└── solution-rules.md           ← Created during generation
```

**What's here:**
- ✅ `solution-prd-v*.md` files
- ✅ `framework/` directory with all logic
- ✅ `README.md` and `LICENSE`

**What's NOT here:**
- ❌ `test-case/` (repo marker)
- ❌ `examples/` (repo marker)
- ❌ `GENERATOR.md` at root (should be in framework/)
- ❌ Generated projects (they go in parent)

#### Level 3: Generated Project (project-a/)
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
├── memory/constitution.md      ← If SpecKit enabled
├── specs/                      ← If SpecKit enabled
├── src/                        ← Code
└── README.md
```

**What's here:**
- ✅ `docs/project-prd.md` and `docs/project-rules.md`
- ✅ `.flight-plan/` with standalone copies
- ✅ `memory/constitution.md` (if SpecKit)

**What's NOT here:**
- ❌ `solution-prd-v*.md` (lives in flight-plan-solution/)
- ❌ `framework/` (lives in flight-plan-solution/)
- ❌ References to `../../flight-plan-solution/` (standalone!)

### Detection Logic Added

**Clear decision tree for AI:**

```markdown
When user opens a folder and runs a command:

1. Check current directory for repo markers (WRONG if found):
   - test-case/README.md exists? → REPO, not solution
   - examples/ directory exists? → REPO, not solution
   - GENERATOR.md at root level? → REPO, not solution

2. Check current directory for solution markers:
   - solution-prd-v*.md exists? → In flight-plan-solution/
   - framework/ directory exists? → In flight-plan-solution/
   - README.md + LICENSE exist? → Likely flight-plan-solution/

3. Check current directory for project markers:
   - docs/project-prd.md exists? → In a generated project
   - .flight-plan/current.md exists? → In a generated project

4. Check for subdirectories:
   - flight-plan-solution/ subdirectory exists? → In working folder
   - Read files with flight-plan-solution/ prefix
```

## Files Updated

### 1. `framework/FLIGHT-PLAN-INIT.md`

**Added new section before STEP 0:**
- "EXPECTED DIRECTORY STRUCTURES" (detailed)
  - Level 1: Working Folder
  - Level 2: flight-plan-solution/
  - Level 3: Generated Project
- "DETECTION LOGIC FOR AI" (decision tree)

**Location:** Lines 35-187 (152 lines added)

**Content:**
- Full directory trees for each level
- "What you'll find here" and "What you WON'T find here" for each
- Example structures with comments
- Step-by-step detection logic

### 2. `framework/FLIGHT-PLAN-COMMANDS.md`

**Added new section after Spec-Kit intro:**
- "Expected Directory Structures" (summary)
  - Level 1: Working Folder
  - Level 2: flight-plan-solution/
  - Level 3: Generated Project

**Location:** Lines 44-115 (71 lines added)

**Content:**
- Simplified directory trees
- "Contains" and "Does NOT contain" lists
- Focused on essential information for command execution

## Benefits

### For AI Agents:

✅ **Clear Expectations**
- Knows exactly what files/directories exist at each level
- No guessing about structure
- Clear positive and negative indicators

✅ **Better Context Detection**
- Can determine current location without navigation
- Knows what to look for at each level
- Understands parent/child relationships

✅ **Prevents Errors**
- Won't look for `solution-prd-v*.md` in wrong place
- Won't assume `framework/` is at root
- Won't try to read non-existent files

✅ **Supports Typical Workflow**
- Acknowledges user opens working folder
- Handles both scenarios (in solution dir or parent)
- Adapts file paths based on detection

### For Users:

✅ **Predictable Behavior**
- AI understands structure regardless of which folder is opened
- Consistent file location expectations
- Clear structure documentation

✅ **Easier Troubleshooting**
- Can reference structure diagrams
- Knows what should exist where
- Understands why validation checks happen

✅ **Better Documentation**
- Single source of truth for structure
- Visual diagrams at each level
- Clear separation of concerns

## Testing Scenarios

### Scenario 1: Open Working Folder
```
User opens: MyApp/
AI checks: flight-plan-solution/ exists?
AI reads: flight-plan-solution/solution-prd-v1.md
Result: ✅ Correct detection
```

### Scenario 2: Open Solution Directory
```
User opens: MyApp/flight-plan-solution/
AI checks: solution-prd-v*.md exists?
AI reads: ./solution-prd-v1.md
Result: ✅ Correct detection
```

### Scenario 3: Open Project Directory
```
User opens: MyApp/project-a/
AI checks: docs/project-prd.md exists?
AI reads: ./docs/project-prd.md
Result: ✅ Correct detection
```

### Scenario 4: Open Repo (Wrong)
```
User opens: FlightPlan/ (the repo)
AI checks: test-case/README.md exists?
AI finds: Yes
Result: ✅ Stops with clear error
```

## Result

**Before:**
- ❌ Structure info scattered
- ❌ AI guessed about locations
- ❌ Unclear expectations
- ❌ Navigation issues

**After:**
- ✅ Comprehensive structure reference at top of key files
- ✅ Clear expectations at each level
- ✅ Positive and negative indicators
- ✅ Detection logic decision tree
- ✅ No navigation needed
- ✅ AI understands typical user workflow

---

**Change Date:** 2025-10-27
**Impact:** All Flight Plan commands benefit from clear structure understanding
**Breaking:** No - only adds documentation
**Benefit:** AI always knows what to expect at each directory level
**Testing:** Verified with typical user workflow (opening working folder)

