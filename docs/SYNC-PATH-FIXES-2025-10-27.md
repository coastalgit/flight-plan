# Sync Command Path Fixes - 2025-10-27

## Problem

The `flight-plan sync` and `flight-plan prd refresh` commands had incomplete/incorrect file path references, missing the `framework/` prefix for solution files and `docs/` prefix for project files.

## Files Fixed

### 1. `framework/FLIGHT-PLAN-COMMANDS.md` - `flight-plan sync` Command

**Section: What apply does**

**Before:**
```markdown
5. Updates `solution-rules.md` if needed
6. **Appends to `ai-refs/solution-overview.md` PRD version history:**
```

**After:**
```markdown
2. Updates `docs/project-prd.md` files in each project to new version
3. Updates `docs/project-rules.md` in each project if solution rules changed
4. Updates `memory/constitution.md` in each project if Spec-Kit enabled
5. Updates `framework/solution-rules.md` if needed
6. **Appends to `framework/ai-refs/solution-overview.md` PRD version history:**
```

**Section: Example Output**

**Before:**
```bash
✅ Updated: solution-rules.md
✅ Updated: backend-api/docs/project-prd.md
✅ Updated: backend-api/docs/project-rules.md
✅ Updated: backend-api/memory/constitution.md
✅ Updated: notifications/docs/project-prd.md
✅ Updated: notifications/docs/project-rules.md
✅ Updated: ai-refs/solution-overview.md

📝 ai-refs/solution-overview.md now shows:
```

**After:**
```bash
✅ Updated: framework/solution-rules.md
✅ Updated: backend-api/docs/project-prd.md
✅ Updated: backend-api/docs/project-rules.md
✅ Updated: backend-api/memory/constitution.md
✅ Updated: notifications/docs/project-prd.md
✅ Updated: notifications/docs/project-rules.md
✅ Updated: framework/ai-refs/solution-overview.md

📝 framework/ai-refs/solution-overview.md now shows:
```

### 2. `framework/FLIGHT-PLAN-COMMANDS.md` - `flight-plan prd refresh` Command

**Section: Verify project files**

**Before:**
```markdown
Before proceeding, verify:
1. ✅ `project-prd.md` exists in current directory
2. ✅ `.flight-plan/current.md` exists

Expected files:
- project-prd.md (project requirements)
- .flight-plan/current.md (progress tracking)
```

**After:**
```markdown
Before proceeding, verify:
1. ✅ `docs/project-prd.md` exists
2. ✅ `.flight-plan/current.md` exists

Expected files:
- docs/project-prd.md (project requirements)
- .flight-plan/current.md (progress tracking)
```

**Section: What preview does**

**Before:**
```markdown
1. Reads current `project-prd.md`
```

**After:**
```markdown
1. Reads current `docs/project-prd.md`
```

**Section: Example**

**Before:**
```bash
cd backend-api/
vim project-prd.md           # Edit PRD manually
```

**After:**
```bash
cd backend-api/
vim docs/project-prd.md      # Edit PRD manually
```

## Complete Sync File Path Reference

When `flight-plan sync apply` runs, it updates:

### Solution-Level Files (in flight-plan-solution/)
- `framework/solution-rules.md` ← Solution-wide standards
- `framework/ai-refs/solution-overview.md` ← PRD version history (append)
- `solution-prd-vX-Y.md` ← Auto-revision PRD if questions resolved (create)

### Project-Level Files (in each project/)
- `docs/project-prd.md` ← Updated to new PRD version
- `docs/project-rules.md` ← Updated if solution rules changed
- `memory/constitution.md` ← Updated if SpecKit enabled and tech changed

### Files NOT Updated by Sync
- `.flight-plan/current.md` ← Only updated by project-level `prd refresh`
- `.flight-plan/config.json` ← Only updated manually or by setup commands
- `README.md` ← Informational only, never auto-updated

## Complete PRD Refresh File Path Reference

When `flight-plan prd refresh apply` runs (project-level), it updates:

### Project-Level Files (in current project/)
- `.flight-plan/current.md` ← Open items, notes, status
- `memory/constitution.md` ← If SpecKit enabled and tech stack changed

### Files NOT Updated by PRD Refresh
- `docs/project-prd.md` ← User edits this manually, refresh reads it
- `docs/project-rules.md` ← Only updated by solution-level sync
- `README.md` ← Informational only, never auto-updated

## Why This Matters

**✅ Correct Paths = Correct Behavior:**
- AI knows EXACTLY which files to update
- No confusion between solution and project files
- Clear separation of concerns

**✅ Accurate Examples:**
- Users see realistic output
- They know what files will change
- Can verify changes happened correctly

**✅ Prevents Errors:**
- AI won't try to update `solution-rules.md` in wrong location
- AI won't look for `project-prd.md` in root instead of `docs/`
- All file operations use correct paths

## Testing

### When user runs `flight-plan sync apply`:
- ✅ AI reads `framework/solution-rules.md` (not `solution-rules.md`)
- ✅ AI updates `framework/ai-refs/solution-overview.md` (not `ai-refs/...`)
- ✅ AI updates `docs/project-prd.md` in each project (not `project-prd.md`)
- ✅ Output messages show correct paths with `framework/` prefix

### When user runs `flight-plan prd refresh`:
- ✅ AI looks for `docs/project-prd.md` (not `project-prd.md`)
- ✅ Error messages show correct path expectations
- ✅ Example output shows correct vim command: `vim docs/project-prd.md`

## Result

**Before:**
- ❌ Missing `framework/` prefix for solution files
- ❌ Missing `docs/` prefix for some project file references
- ❌ Inconsistent path references across documentation
- ❌ Could cause AI to look in wrong locations

**After:**
- ✅ All solution files use `framework/` prefix
- ✅ All project PRD/rules files use `docs/` prefix
- ✅ Consistent path references everywhere
- ✅ AI knows exact file locations for all operations

---

**Change Date:** 2025-10-27
**Impact:** Solution-level `sync` and project-level `prd refresh` commands
**Breaking:** No - only fixes documentation, doesn't change behavior
**Benefit:** AI uses correct file paths for all sync operations

