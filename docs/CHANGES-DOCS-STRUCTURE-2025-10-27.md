# Documentation Structure Changes - 2025-10-27

## Summary

Moved `project-prd.md` and `project-rules.md` into `docs/` directory to keep project root clean and organized.

## New Project Structure

```
project/
├── docs/
│   ├── project-prd.md          # Single source of truth (moved here)
│   ├── project-rules.md        # AI integration (moved here)
│   ├── third-party/            # External API specs
│   ├── snippets/               # Code examples
│   └── research/               # Background docs
├── FLIGHT-PLAN-COMMANDS.md     # Standalone copy
├── FLIGHT-PLAN-PHASES.md       # Standalone copy
├── .flight-plan/
│   ├── current.md
│   ├── config.json
│   └── history/
├── .cursor/rules/
│   └── flight-plan.mdc         # Points to docs/project-rules.md
├── memory/                     # SpecKit (if enabled)
├── specs/                      # SpecKit (if enabled)
└── src/                        # Source code
```

## Files Updated

### 1. `GENERATOR.md`
- ✅ Updated project structure diagram
- ✅ Changed template paths: `docs/project-prd.md`, `docs/project-rules.md`
- ✅ Updated README generation template to reference new paths

### 2. `templates/cursor-rule.mdc.template`
- ✅ Changed pointer from `project-rules.md` to `docs/project-rules.md`

### 3. `templates/project-rules.md.template`
- ✅ Updated file references to use `../` for root files
- ✅ Changed `project-prd.md` to `project-prd.md` (same directory)
- ✅ Updated command reference: `../FLIGHT-PLAN-COMMANDS.md`

### 4. `FLIGHT-PLAN-COMMANDS.md`
- ✅ Updated context detection to look for `docs/project-prd.md`
- ✅ Updated `flight-plan sync` command to update `docs/project-prd.md` and `docs/project-rules.md`
- ✅ Updated `flight-plan prd refresh` context check
- ✅ Updated all example outputs to show new paths:
  - `backend-api/docs/project-prd.md`
  - `backend-api/docs/project-rules.md`

### 5. `README.md`
- ✅ Updated "What Gets Generated" section with new structure
- ✅ Highlighted that projects are STANDALONE

## Benefits

✅ **Cleaner root directory** - Only build artifacts and core files at root
✅ **All documentation grouped** - PRD, rules, and reference materials in one place
✅ **Projects still standalone** - Local copies of FLIGHT-PLAN-COMMANDS.md and FLIGHT-PLAN-PHASES.md
✅ **Familiar structure** - Same pattern as having a `docs/` folder for reference materials
✅ **Easy to navigate** - AI and humans both find files logically organized

## Commands Updated

**Solution-level `flight-plan sync` now updates:**
- `docs/project-prd.md` (not root `project-prd.md`)
- `docs/project-rules.md` (not root `project-rules.md`)

**Project-level `flight-plan prd refresh` now reads:**
- `docs/project-prd.md` for context

## Migration

For existing projects created before this change:
```bash
cd your-project/
mkdir docs
mv project-prd.md docs/
mv project-rules.md docs/
```

Then update `.cursor/rules/flight-plan.mdc` to point to `docs/project-rules.md`.

---

**Change Date:** 2025-10-27
**Impact:** All new projects, sync, and prd refresh operations

