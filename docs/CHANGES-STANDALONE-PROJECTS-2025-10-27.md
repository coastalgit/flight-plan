# Standalone Projects Structure - 2025-10-27

## Summary

Projects are now COMPLETELY STANDALONE with:
1. Flight Plan reference files moved to `.flight-plan/` directory
2. ALL references within project boundaries (NO `../../` paths)
3. Clear documentation in project README about file locations

## Critical Changes

### 1. File Locations

**Before:**
```
project/
├── docs/
│   ├── project-prd.md
│   └── project-rules.md
├── FLIGHT-PLAN-COMMANDS.md     ← In root
├── FLIGHT-PLAN-PHASES.md        ← In root
└── .flight-plan/
    ├── current.md
    └── config.json
```

**After:**
```
project/
├── docs/
│   ├── project-prd.md
│   └── project-rules.md
└── .flight-plan/
    ├── FLIGHT-PLAN-COMMANDS.md  ← Moved here
    ├── FLIGHT-PLAN-PHASES.md    ← Moved here
    ├── current.md
    ├── config.json
    └── history/
```

### 2. Path References - ALL Local

**`docs/project-rules.md` now references:**
- ✅ `../.flight-plan/current.md` (not `../../flight-plan-solution/...`)
- ✅ `../.flight-plan/FLIGHT-PLAN-PHASES.md` (not `../FLIGHT-PLAN-PHASES.md`)
- ✅ `../.flight-plan/FLIGHT-PLAN-COMMANDS.md` (not `../FLIGHT-PLAN-COMMANDS.md`)
- ✅ `project-prd.md` (same directory)

**NO references outside project directory!**

### 3. Project README Updated

The generated project README now documents:
```markdown
## Documentation

- **Project PRD:** `docs/project-prd.md` - Complete specifications
- **Project Rules:** `docs/project-rules.md` - How AI should work
- **Current Status:** `.flight-plan/current.md` - Current phase
- **Configuration:** `.flight-plan/config.json` - Project settings
- **Commands Reference:** `.flight-plan/FLIGHT-PLAN-COMMANDS.md` - All commands
- **Phase Standards:** `.flight-plan/FLIGHT-PLAN-PHASES.md` - Phase workflow
```

## Files Updated

### 1. `framework/GENERATOR.md`
- ✅ Updated project structure diagram - files now in `.flight-plan/`
- ✅ Updated README template to document new locations
- ✅ Updated copy instructions for Flight Plan files

### 2. `framework/templates/project-rules.md.template`
- ✅ Line 13: `../.flight-plan/FLIGHT-PLAN-PHASES.md` (was `../FLIGHT-PLAN-PHASES.md`)
- ✅ Line 14: `../.flight-plan/FLIGHT-PLAN-COMMANDS.md` (was `../FLIGHT-PLAN-COMMANDS.md`)
- ✅ Line 76: `../.flight-plan/FLIGHT-PLAN-PHASES.md` (Flight Plan Integration section)
- ✅ Line 95: `../.flight-plan/FLIGHT-PLAN-PHASES.md` (SpecKit section)
- ✅ Line 114: `../.flight-plan/FLIGHT-PLAN-COMMANDS.md` (Development Workflow section)
- ✅ Line 74: `../.flight-plan/current.md` (already correct)

### 3. `README.md`
- ✅ Updated project structure diagram
- ✅ Moved FLIGHT-PLAN-COMMANDS.md and FLIGHT-PLAN-PHASES.md under `.flight-plan/`
- ✅ Updated SpecKit references to show correct paths
- ✅ Emphasized "COMPLETELY STANDALONE" with no external dependencies

## Benefits

✅ **True Standalone Projects**
- Can move project anywhere without breaking references
- No dependency on solution directory structure
- Can share project as standalone unit

✅ **Cleaner Root Directory**
- Project root only has `docs/`, `.flight-plan/`, `.cursor/`, `src/`, `README.md`
- All Flight Plan files organized in `.flight-plan/`
- Follows convention of hidden directories for tooling (like `.git/`)

✅ **Clear for AI**
- Project README explicitly documents all file locations
- `.cursor/rules/flight-plan.mdc` points to `docs/project-rules.md`
- `docs/project-rules.md` tells AI where everything is
- When user types `flight-plan status`, AI reads `.flight-plan/FLIGHT-PLAN-COMMANDS.md`

✅ **No Path Confusion**
- All paths relative to project root
- No `../../` references to break
- Simple for AI to understand: "everything I need is here"

## Migration for Existing Projects

If you have a project generated before this change:

```bash
cd your-project/

# Move Flight Plan files to .flight-plan/
mv FLIGHT-PLAN-COMMANDS.md .flight-plan/
mv FLIGHT-PLAN-PHASES.md .flight-plan/

# Update docs/project-rules.md
# Change all references from:
#   ../FLIGHT-PLAN-COMMANDS.md → ../.flight-plan/FLIGHT-PLAN-COMMANDS.md
#   ../FLIGHT-PLAN-PHASES.md → ../.flight-plan/FLIGHT-PLAN-PHASES.md

# Verify
ls .flight-plan/  # Should see: FLIGHT-PLAN-COMMANDS.md, FLIGHT-PLAN-PHASES.md, current.md, config.json
```

## Testing

When user opens a fresh project and types `flight-plan status`:
1. AI reads `.cursor/rules/flight-plan.mdc` → points to `docs/project-rules.md`
2. AI reads `docs/project-rules.md` → tells AI to read `../.flight-plan/FLIGHT-PLAN-COMMANDS.md`
3. AI reads `.flight-plan/FLIGHT-PLAN-COMMANDS.md` → finds `flight-plan status` section
4. AI executes the command logic
5. All file reads stay within project boundaries

## Impact

- **New projects:** Get correct structure automatically
- **Existing projects:** Need manual migration (or regenerate)
- **Commands:** No changes - AI reads from new locations automatically
- **SpecKit:** Works correctly with local references

---

**Change Date:** 2025-10-27
**Scope:** Project-level file organization and path references
**Breaking:** Yes for existing projects (need migration or regeneration)
**Benefit:** TRUE standalone projects that can be moved/shared independently

