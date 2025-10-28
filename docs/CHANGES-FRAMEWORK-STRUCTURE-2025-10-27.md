# Framework Directory Structure - 2025-10-27

## Summary

Moved all Flight Plan framework files into a new `framework/` directory and created a `docs/` directory for change documentation. This keeps the solution root clean and focused on user content (PRDs).

## Actual Directory Structure

```
FlightPlan/  (repository root)
├── README.md                       # Entry point
├── LICENSE                         # License
├── .gitignore                      # Git ignore rules
├── examples/                       # Example projects
├── test-case/                      # Test cases
├── docs/                           # NEW - Change documentation
│   ├── CHANGES-FRAMEWORK-STRUCTURE-2025-10-27.md
│   ├── CHANGES-DOCS-STRUCTURE-2025-10-27.md
│   └── FIXES-2025-10-27.md
└── framework/                      # NEW - All framework files
    ├── BRIEF-BUILDER-v1.1.md
    ├── GENERATOR.md
    ├── FLIGHT-PLAN-INIT.md
    ├── FLIGHT-PLAN-COMMANDS.md
    ├── FLIGHT-PLAN-PHASES.md
    ├── FLIGHT-PLAN-SPECKIT-SETUP.md
    ├── templates/
    │   ├── project-prd.md.template
    │   ├── solution-rules.md.template
    │   ├── project-rules.md.template
    │   ├── cursor-rule.mdc.template
    │   ├── config.json.template
    │   ├── constitution.md.template
    │   └── flight-plan-current.md.template
    └── ai-refs/                    # (empty - created by generator)
```

## User's Solution Structure (When Copied)

```
MyApp/flight-plan-solution/
├── solution-prd-v*.md              # PRDs stay in root
├── README.md                       # Stays in root
├── LICENSE                         # Stays in root
└── framework/                      # Framework files copied here
    ├── BRIEF-BUILDER-v1.1.md
    ├── GENERATOR.md
    ├── FLIGHT-PLAN-INIT.md
    ├── FLIGHT-PLAN-COMMANDS.md
    ├── FLIGHT-PLAN-PHASES.md
    ├── FLIGHT-PLAN-SPECKIT-SETUP.md
    ├── solution-rules.md           # Generated here
    ├── templates/
    └── ai-refs/                    # Generated here
```

## Files Updated

### 1. `FLIGHT-PLAN-INIT.md`
- ✅ Updated directory structure examples to show `framework/`
- ✅ Changed `GENERATOR.md` references to `framework/GENERATOR.md`
- ✅ Updated context loading step

### 2. `GENERATOR.md`
- ✅ Updated template paths: `./templates/` → `./framework/templates/`
- ✅ Updated output paths for `solution-rules.md` → `./framework/solution-rules.md`
- ✅ Updated ai-refs creation path → `./framework/ai-refs/`
- ✅ All template references now use `framework/templates/`

### 3. `FLIGHT-PLAN-COMMANDS.md`
- ✅ Updated solution-level context detection to look for `framework/` directory
- ✅ Changed context check from `templates/` to `framework/`
- ✅ Changed context check from `ai-refs/` to `framework/ai-refs/`

### 4. `README.md`
- ✅ Updated "What's Included" structure diagram
- ✅ Updated "Complete Directory Structure" diagram
- ✅ Shows framework/ organization

### 5. All Template References in `GENERATOR.md`
Updated all 6 template usage references:
- ✅ `solution-rules.md.template`
- ✅ `project-prd.md.template`
- ✅ `project-rules.md.template`
- ✅ `flight-plan-current.md.template`
- ✅ `cursor-rule.mdc.template`
- ✅ `config.json.template`

All now reference `framework/templates/[filename]`.

## Benefits

✅ **Cleaner solution root** - Only PRDs and entry point files (README, LICENSE)
✅ **Framework is isolated** - All AI/tooling files in one place
✅ **Clear separation** - User content (PRDs) vs. framework
✅ **Easier navigation** - Less clutter in root directory
✅ **Familiar pattern** - Like `node_modules/`, `.git/`, etc.

## Migration for Existing Solutions

**Note:** This structure applies to the Flight Plan repository. When users copy it as `flight-plan-solution/`, the structure is maintained.

If you have an existing flight-plan-solution directory from an older version:

```bash
cd flight-plan-solution/

# Create framework directory
mkdir framework
mkdir framework/ai-refs

# Move framework files
mv GENERATOR.md framework/
mv FLIGHT-PLAN-INIT.md framework/
mv FLIGHT-PLAN-COMMANDS.md framework/
mv FLIGHT-PLAN-PHASES.md framework/
mv BRIEF-BUILDER-v*.md framework/
mv FLIGHT-PLAN-SPECKIT-SETUP.md framework/
mv solution-rules.md framework/ (if exists)
mv templates/ framework/
mv ai-refs/ framework/ (if exists)

# Verify
ls  # Should only see: solution-prd-v*.md, README.md, LICENSE, framework/
```

## What Doesn't Change

✅ **Project structure** - Projects are unchanged
✅ **Commands** - All `flight-plan` commands work the same
✅ **PRD workflow** - PRDs still in solution root
✅ **Project generation** - Still creates projects alongside solution
✅ **AI integration** - AI reads from framework/ automatically

## Impact

- **New users**: Will get clean structure automatically
- **Existing users**: Can migrate manually or start fresh
- **Commands**: No changes needed - paths updated internally
- **Templates**: Reference framework/ paths automatically

---

**Change Date:** 2025-10-27
**Scope:** Solution-level directory structure
**Breaking:** No - only affects new installations

