# CRITICAL: Project Commands File Fix - 2025-10-28

**Date:** 2025-10-28  
**Severity:** CRITICAL  
**Issue:** Projects were getting solution-level commands instead of project-only commands  
**Impact:** 8 hours of testing wasted due to wrong commands in projects

---

## The Problem

**CRITICAL MISTAKE:** When generating projects, `FLIGHT-PLAN-COMMANDS.md` was being COPIED from `framework/` into each project's `.flight-plan/` directory.

**Result:** Projects had solution-level commands that don't work in project context:
- ❌ `flight-plan init` (solution-level only)
- ❌ `flight-plan init apply` (solution-level only)
- ❌ `flight-plan sync` (solution-level only)
- ❌ References to "solution PRD"
- ❌ References to syncing projects

**User frustration:**
> "THIS IS GETTING INSANE - YOU ARE WASTING MY TIME!!!!!!!!!!!!!!!
> I keep having to clear down these projects and start again to test - its been 8 hours!
> once again the project files are nonsense!!
> readme contains ref to: flight-plan help, flight-plan status, flight-plan prd refresh, flight-plan note
> The FLIGHT-PLAN-COMMANDS.md however STILL has ref to "solution" and "sync" and "init" ffs!!!!! This is basic stuff!"

---

## The Root Cause

**GENERATOR.md was copying the WRONG file:**
```
.flight-plan/
├── FLIGHT-PLAN-COMMANDS.md  ← COPIED entire solution+project commands file
```

**Should have been:**
```
.flight-plan/
├── FLIGHT-PLAN-COMMANDS.md  ← PROJECT-ONLY commands (generated from template)
```

---

## The Solution

### 1. Created NEW Template: `project-commands.md.template`

**File:** `framework/templates/project-commands.md.template`

**Contains ONLY project-level commands:**
- ✅ `flight-plan help`
- ✅ `flight-plan status`
- ✅ `flight-plan prd refresh`
- ✅ `flight-plan note`

**Does NOT contain:**
- ❌ `flight-plan init`
- ❌ `flight-plan init apply`
- ❌ `flight-plan sync`
- ❌ Any solution-level commands
- ❌ Any references to "solution PRD"
- ❌ Any references to syncing projects

### 2. Updated GENERATOR.md

**Changed project structure diagram:**
```diff
├── .flight-plan/
-│   ├── FLIGHT-PLAN-COMMANDS.md       # COPY from flight-plan-solution/framework/
+│   ├── FLIGHT-PLAN-COMMANDS.md       # Use templates/project-commands.md.template
│   ├── FLIGHT-PLAN-PHASES.md         # COPY from flight-plan-solution/framework/
│   ├── FLIGHT-PLAN-SPECKIT-SETUP.md  # COPY from flight-plan-solution/framework/
```

**Updated standalone files description:**
```diff
-**CRITICAL:** Copy these files into `.flight-plan/` to make project STANDALONE:
-- `FLIGHT-PLAN-COMMANDS.md` - Full command execution logic (NO external refs)
+**CRITICAL:** `.flight-plan/` files to make project STANDALONE:
+- `FLIGHT-PLAN-COMMANDS.md` - Generated from templates/project-commands.md.template (PROJECT-ONLY commands)
 - `FLIGHT-PLAN-PHASES.md` - COPY from framework/ (phase standards, no refs to solution)
 - `FLIGHT-PLAN-SPECKIT-SETUP.md` - COPY from framework/ (SpecKit setup, no refs to solution)
```

---

## What Project README Says (CORRECT)

```markdown
**Getting Started:**
1. Run `flight-plan help` to see available commands
2. Run `flight-plan status` for guidance on what to do next

**Other Commands:**
- `flight-plan prd refresh` - Update tracking from PRD changes
- `flight-plan note [text]` - Add activity note
```

**No mention of `init`, `sync`, or solution-level commands!**

---

## What `.flight-plan/FLIGHT-PLAN-COMMANDS.md` Now Contains

**Overview section:**
```
Available commands:
• flight-plan help        - Show this help
• flight-plan status      - Check progress & get guidance
• flight-plan prd refresh - Update after manual PRD edits  
• flight-plan note        - Add quick note to tracking
```

**Full sections for:**
1. `flight-plan help`
2. `flight-plan status` (with SpecKit prompting, testing, phase transitions)
3. `flight-plan prd refresh`
4. `flight-plan note`

**NO sections for init/sync/solution commands!**

---

## File Structure Comparison

### Solution Level (framework/FLIGHT-PLAN-COMMANDS.md)
**Contains:**
- `flight-plan init`
- `flight-plan init apply`
- `flight-plan status` (solution-level)
- `flight-plan sync`
- `flight-plan prd refresh` (solution-level)

### Project Level (.flight-plan/FLIGHT-PLAN-COMMANDS.md from template)
**Contains:**
- `flight-plan help`
- `flight-plan status` (project-level)
- `flight-plan prd refresh` (project-level)
- `flight-plan note`

**NO solution commands!**

---

## Testing Checklist

After this fix, when AI generates a project:

✅ Project README lists: help, status, prd refresh, note  
✅ Project `.flight-plan/FLIGHT-PLAN-COMMANDS.md` has ONLY those commands  
✅ NO references to "solution PRD" in project commands file  
✅ NO references to "sync" in project commands file  
✅ NO references to "init" in project commands file  
✅ User running `flight-plan help` in project sees project commands only  
✅ User running `flight-plan status` in project gets project-level guidance  

---

## Files Changed

1. **`framework/templates/project-commands.md.template`** (NEW)
   - Complete project-only commands file
   - No solution-level commands
   - No external references

2. **`framework/GENERATOR.md`**
   - Line 343: Changed from COPY to template usage
   - Lines 358-361: Updated standalone files description

3. **`docs/CRITICAL-PROJECT-COMMANDS-FIX-2025-10-28.md`** (NEW)
   - This documentation

---

## Apology

This was a FUNDAMENTAL ERROR in the project generation logic. Projects should NEVER have received solution-level commands. The user wasted 8 hours of testing time discovering this through trial and error.

**This should have been caught immediately during first project generation test.**

---

**Change Date:** 2025-10-28  
**Severity:** CRITICAL  
**Impact:** ALL generated projects  
**Breaking:** YES - requires regeneration of all test projects  
**User Time Lost:** 8 hours  
**Priority:** HIGHEST - blocks all testing

