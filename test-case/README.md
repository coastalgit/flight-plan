# Flight Plan Test Case

This directory contains a complete test case for validating the Flight Plan workflow from start to finish.

## 🎯 Purpose

Test the entire Flight Plan methodology with a realistic two-project solution:
- Frontend (React SPA)
- Backend (Express API)

## 📋 Test Flow

### Step 1: Brainstorm (✅ Complete)

**File:** `brainstorm-task-manager.md`

This file simulates a real AI brainstorming session where someone discusses:
- Initial idea and requirements
- Technical stack decisions
- Feature scope
- Architecture design
- Open questions and resolutions
- Design rationale (important for Section 11 in PRD)

### Step 2: Brief Builder → PRD

**Run this command:**
```
# In your AI of choice (Cursor, Claude, etc.)
# Open the FlightPlan directory

"Read BRIEF-BUILDER-v1.1.md and process test-case/brainstorm-task-manager.md"
```

**Expected Output:** `test-case/task-manager-prd-v1.md`

**What to check:**
- ✅ Section 1-10 have content from brainstorm
- ✅ Section 11 (Design Rationale & Context) captures WHY decisions were made
- ✅ Two projects identified: task-manager-ui, task-manager-api
- ✅ Tech stacks correctly extracted
- ✅ Internal dependency (frontend → backend) documented
- ✅ Open questions resolved (should be 0)

### Step 3: Initialize Flight Plan Solution

**Commands:**
```bash
# Create solution directory
mkdir test-task-manager
cd test-task-manager

# Copy Flight Plan tooling
cp -r ../flight-plan-solution ./

# Copy generated PRD
cp ../test-case/task-manager-prd-v1.md ./flight-plan-solution/solution-prd-v1.md

# Open in Cursor
# Option A: Open flight-plan-solution/
# Option B: Open test-task-manager/ (see all projects in explorer)

# Preview structure
"flight-plan init"

# Review the preview, then apply
"flight-plan init apply"
```

**Expected Result:**
```
test-task-manager/
├── flight-plan-solution/
│   ├── solution-prd-v1.md
│   ├── solution-rules.md
│   ├── ai-refs/
│   └── [all Flight Plan files]
├── task-manager-ui/
│   ├── project-prd.md
│   ├── project-rules.md
│   ├── .flight-plan/
│   ├── .cursor/rules/
│   ├── README.md
│   └── src/
└── task-manager-api/
    ├── project-prd.md
    ├── project-rules.md
    ├── .flight-plan/
    ├── .cursor/rules/
    ├── README.md
    └── src/
```

**What to check:**
- ✅ Both projects created alongside flight-plan-solution/
- ✅ Each project has project-prd.md with correct content
- ✅ Each project has .flight-plan/config.json
- ✅ README files reference `flight-plan status` (not old commands)
- ✅ Cursor rules reference correct commands

### Step 4: Test Frontend Project (task-manager-ui)

**Commands:**
```bash
cd task-manager-ui/

# First command in any project
"flight-plan status"
```

**Expected Behavior:**
1. ✅ Shows Phase 1 status
2. ✅ Asks: "Would you like to enable SpecKit for feature-level development? (yes/no)"
3. ✅ Lists Phase 1 objectives from project-prd.md
4. ✅ Shows any blockers (if open questions exist)
5. ✅ Guides on what to do next

**What to test:**
- **Say "no" to SpecKit** for first project (test rejection flow)
- ✅ Check `.flight-plan/config.json` updated with `speckit_prompted: true, speckit_enabled: false`
- ✅ Run `"flight-plan status"` again - should NOT prompt for SpecKit again
- **Phase 1 work:**
  - Review project-prd.md
  - Ask questions about requirements
  - Test conversational phase transition: "I think we're ready for Phase 2"
  - ✅ AI should guide naturally to Phase 2 (no explicit command needed)

### Step 5: Test Backend Project (task-manager-api)

**Commands:**
```bash
cd ../task-manager-api/

"flight-plan status"
```

**Expected Behavior:**
1. ✅ Shows Phase 1 status (independent from frontend)
2. ✅ Asks about SpecKit again (this project's own config)
3. ✅ Shows backend-specific objectives

**What to test:**
- **Say "yes" to SpecKit** for this project (test acceptance flow)
- ✅ AI should:
  1. Check if `uv` is installed (show command: `uv --version`)
  2. Fetch actual SpecKit README from GitHub
  3. Show real installation commands
  4. Wait for user to install
  5. Update `.flight-plan/config.json`
- ✅ Check that SpecKit created `memory/` and `specs/` directories
- ✅ Check `memory/constitution.md` references Flight Plan files

### Step 6: Test Phase Progression

**Test in both projects:**

**Phase 1 → Phase 2:**
- Work through Phase 1 objectives
- Say: "Ready to move to Phase 2"
- ✅ AI should transition naturally (no command needed)

**Phase 5 → Phase 6 (Build phases):**
- Mock some basic code files
- Run `"flight-plan status"` in Phase 5
- ✅ Should show: "🧪 Tests: [results]" automatically
- ✅ Should NOT require explicit "flight-plan test" command

**Phase 8 (Complete):**
- Work through all phases
- ✅ AI should recognize completion
- ✅ Should offer congratulations and next steps

### Step 7: Test Cross-Project Coordination

**Commands:**
```bash
cd flight-plan-solution/

"flight-plan status"
```

**Expected Behavior:**
- ✅ Shows overview of both projects
- ✅ Lists phases for each project
- ✅ Shows any blockers across projects
- ✅ Recommends which project to work on next

### Step 8: Test PRD Sync

**Simulate a PRD change:**
1. Edit `solution-prd-v1.md` → rename to `solution-prd-v2.md`
2. Make a change (e.g., add a new requirement)

**Commands:**
```bash
# In flight-plan-solution/
"flight-plan sync"        # Preview
"flight-plan sync apply"  # Apply changes
```

**Expected:**
- ✅ Shows diff of changes
- ✅ Updates both project PRDs
- ✅ Updates tracking in .flight-plan/current.md

---

## ✅ Success Criteria

This test case validates Flight Plan if:

1. **Generation:**
   - ✅ Projects created in correct location
   - ✅ All template files filled correctly
   - ✅ No references to old/non-existent commands

2. **SpecKit Integration:**
   - ✅ Consistent prompting behavior
   - ✅ Fetches real installation instructions
   - ✅ "No" decision is remembered
   - ✅ "Yes" decision enables SpecKit properly

3. **Phase Workflow:**
   - ✅ Starts in Phase 1
   - ✅ Transitions naturally (conversational)
   - ✅ Testing integrated into build phases
   - ✅ No jumping directly to Phase 2

4. **Command References:**
   - ✅ No `flight-plan next`
   - ✅ No `flight-plan setup-speckit`
   - ✅ No `flight-plan enable-speckit`
   - ✅ No `flight-plan test` (integrated)
   - ✅ No `flight-plan phase [N]`

5. **Path Handling:**
   - ✅ Works when opening parent directory in Cursor
   - ✅ Works when opening project directory directly
   - ✅ No absolute path errors
   - ✅ Validates current directory correctly

---

## 🐛 Known Issues to Watch For

Based on previous bugs, specifically check:

1. **README files** - Should reference `flight-plan status`, not old commands
2. **SpecKit prompting** - Should be consistent across all projects
3. **Absolute paths** - Should never see `c:\...` style paths in errors
4. **Phase jumping** - Should not skip Phase 1 guidance
5. **Directory detection** - Should work regardless of where Cursor is opened

---

## 📝 Notes

- This is a **test case**, not a production app
- The brainstorm intentionally includes design rationale for Section 11 testing
- Two projects test internal dependencies and coordination
- Simple scope allows quick testing of full workflow
- No deployment requirements keeps focus on Flight Plan methodology

---

**Version:** 1.0  
**Created:** October 27, 2025  
**Purpose:** Validate Flight Plan fixes and workflow

