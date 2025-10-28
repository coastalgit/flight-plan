# Directory Validation Fixes - 2025-10-27

## Problem

When running Flight Plan commands (especially with Claude Code), the AI was:
1. Stepping back a directory (running `cd ..`)
2. Looking for repo markers like `test-case/`, `examples/`, `.git/` in parent directories
3. Using shell commands like `pwd` and `ls` which triggered navigation behavior

**Root Cause:**
- Instructions told AI to run `pwd` and `ls` commands
- Instructions didn't explicitly say "check CURRENT directory only"
- Instructions didn't say "DO NOT navigate"
- Shell commands were interpreted as "let me explore the file system"

## Fixes Applied

### 1. `framework/FLIGHT-PLAN-INIT.md` - STEP 0

**Before:**
```markdown
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
```

**After:**
```markdown
**DO NOT navigate directories. DO NOT run `cd ..`. Check ONLY the current working directory.**

**Step 1: Check current directory for repo markers**

Check if these files/directories exist in the CURRENT directory:
- `test-case/` (directory)
- `examples/` (directory)
- `GENERATOR.md` (file in root - not in framework/)

**Use file system checks (not shell commands):**
- Check if file/directory exists: `test-case/README.md`
- Check if file exists: `GENERATOR.md` (at root level)
```

**Key Changes:**
- ✅ Added explicit "DO NOT navigate" instruction at top
- ✅ Removed `pwd` and `basename` shell commands
- ✅ Changed from "contains X" to "check if X exists" (file system checks)
- ✅ Added specific file checks instead of vague directory checks
- ✅ Removed ambiguous "current directory name is FlightPlan" check

### 2. `framework/FLIGHT-PLAN-INIT.md` - STEP 3

**Before:**
```markdown
### STEP 3: DETECT CONTEXT & NAVIGATE

**Check current directory:**
```bash
pwd  # or cd on Windows
ls   # What's here?
```

**Navigate if needed:**
```bash
# If in parent directory:
cd flight-plan-solution/
```
```

**After:**
```markdown
### STEP 3: DETECT CURRENT LOCATION (NO NAVIGATION)

**DO NOT run shell commands like `pwd` or `ls`. DO NOT navigate with `cd`. Use file system checks only.**

**Scenario A: Already in flight-plan-solution/**
- Check if `solution-prd-v*.md` exists in current directory (try to read it)
- Check if `framework/` directory exists (try to read `framework/GENERATOR.md`)
- Check if `README.md` exists in current directory

If YES to all: You're in `flight-plan-solution/`
- File paths: Use `.` (e.g., `./README.md`, `./framework/GENERATOR.md`)

**Scenario B: In parent directory (MyApp/)**
- Check if `flight-plan-solution/` subdirectory exists (try to read `flight-plan-solution/README.md`)
- Check if `flight-plan-solution/solution-prd-v*.md` exists

If YES: You're in parent directory
- File paths: Use `flight-plan-solution/` prefix (e.g., `flight-plan-solution/README.md`)

**CRITICAL: Do NOT navigate directories. Just detect location and adjust file read paths accordingly.**
```

**Key Changes:**
- ✅ Changed title from "DETECT CONTEXT & NAVIGATE" to "DETECT CURRENT LOCATION (NO NAVIGATION)"
- ✅ Removed all shell commands (`pwd`, `ls`, `cd`)
- ✅ Added explicit "DO NOT run shell commands" instruction
- ✅ Changed from navigation to path adjustment
- ✅ AI now adjusts file paths instead of navigating

### 3. `framework/FLIGHT-PLAN-COMMANDS.md` - Solution-level `flight-plan status`

**Before:**
```markdown
**Step 1: Check if this is the Flight Plan REPO (wrong location)**
```bash
pwd
ls
```

Signs you're in the REPO (not a solution):
- Directory name is "FlightPlan" (not "flight-plan-solution")
- Contains `test-case/` directory
- Contains `.git/` directory  
- Contains `examples/` directory
```

**After:**
```markdown
**Step 1: Check if this is the Flight Plan REPO (wrong location)**

**DO NOT run shell commands. Use file system checks only.**

Check if these exist in CURRENT directory:
- `test-case/` directory (check if `test-case/README.md` exists)
- `examples/` directory (check if `examples/` exists)
- `GENERATOR.MD` file at root level (not in framework/)
```

**Key Changes:**
- ✅ Removed `pwd` and `ls` shell commands
- ✅ Added explicit "DO NOT run shell commands" instruction
- ✅ Changed to file system existence checks
- ✅ Made checks specific (e.g., `test-case/README.md` not just `test-case/`)

## Why Shell Commands Caused Issues

### With `pwd` and `ls`:
1. AI sees: "run `pwd`" → Executes in terminal
2. AI sees: "run `ls`" → Executes in terminal
3. AI thinks: "I need to explore the file system"
4. AI might run: `cd ..` to check parent
5. AI might run: `ls ../` to see what's there
6. **Result:** Unexpected navigation behavior

### With File System Checks:
1. AI sees: "check if `test-case/README.md` exists"
2. AI uses: Internal file existence check (no shell)
3. AI thinks: "Just check this specific file"
4. AI stays: In current directory
5. **Result:** No navigation, just checks

## Testing Strategy

For AI agents processing these commands:

**OLD (would cause navigation):**
```
1. Run pwd
2. Run ls
3. See what's in parent directory
4. Navigate around to find markers
```

**NEW (no navigation):**
```
1. Try to read test-case/README.md
   - If exists: Stop, wrong directory
   - If doesn't exist: Continue
2. Try to read GENERATOR.md (root level)
   - If exists: Stop, wrong directory
   - If doesn't exist: Continue
3. Try to read solution-prd-v1.md
   - If exists: Correct directory
   - If doesn't exist: Check for flight-plan-solution/ subdirectory
```

## Result

**Before:**
- ❌ AI ran `pwd` and `ls` commands
- ❌ AI navigated directories with `cd`
- ❌ AI checked parent directories
- ❌ Claude Code especially prone to this behavior
- ❌ Unpredictable "stepping back" behavior

**After:**
- ✅ AI uses file system checks only
- ✅ No shell commands executed
- ✅ Checks ONLY current directory
- ✅ Explicit "DO NOT navigate" instructions
- ✅ Consistent behavior across all AI models
- ✅ No unexpected directory changes

## Files Updated

1. **`framework/FLIGHT-PLAN-INIT.md`**
   - STEP 0: Removed `pwd`/`basename`, added "DO NOT navigate"
   - STEP 3: Removed all shell commands, changed to file path adjustment

2. **`framework/FLIGHT-PLAN-COMMANDS.md`**
   - Solution-level `flight-plan status`: Removed `pwd`/`ls`, added file system checks

## Benefits

✅ **Predictable Behavior** - AI stays in current directory
✅ **No Shell Execution** - Safer, no unintended commands
✅ **Cross-AI Compatibility** - Works same in Claude Desktop, Claude Code, ChatGPT, Gemini
✅ **Clear Instructions** - Explicit about what NOT to do
✅ **File System Native** - Uses AI's built-in file reading capabilities

---

**Change Date:** 2025-10-27
**Impact:** All directory validation in init and status commands
**Breaking:** No - only improves validation behavior
**Tested With:** Claude Code (reported issue), Claude Desktop
**Benefit:** No more unexpected directory navigation

