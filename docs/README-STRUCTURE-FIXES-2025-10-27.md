# README Directory Structure Fixes - 2025-10-27

## Problem

README had outdated project directory structures showing files in wrong locations, and missing explicit references to where SpecKit can find the PRD.

**Issues Found:**
1. First directory structure (lines 96-120) showed `project-prd.md` and `project-rules.md` in project root instead of `docs/`
2. First structure missing `.flight-plan/` standalone files (COMMANDS, PHASES, SPECKIT-SETUP)
3. First structure missing explicit SpecKit PRD reference in `memory/constitution.md` comment
4. Second directory structure (lines 290-312) had vague SpecKit reference
5. SpecKit "will automatically" section referenced `project-prd.md` without path

## Fixes Applied

### 1. First Directory Structure (Complete Rewrite)

**Before:**
```
├── project-a/
│   ├── project-prd.md                  # Wrong location!
│   ├── project-rules.md                # Wrong location!
│   ├── .flight-plan/
│   │   ├── current.md
│   │   └── history/
│   ├── .cursor/rules/
│   │   └── flight-plan.mdc         # Points to project-rules.md (wrong path)
│   ├── CLAUDE.md (optional)        # Points to project-rules.md (wrong path)
│   ├── docs/                       # Reference materials
│   ├── src/
│   └── README.md
```

**After:**
```
├── project-a/
│   ├── docs/
│   │   ├── project-prd.md              # ✅ Correct location
│   │   ├── project-rules.md            # ✅ Correct location
│   │   ├── third-party/                # External API specs
│   │   ├── snippets/                   # Code examples
│   │   └── research/                   # Background docs
│   ├── .flight-plan/
│   │   ├── FLIGHT-PLAN-COMMANDS.md     # ✅ Standalone copy shown
│   │   ├── FLIGHT-PLAN-PHASES.md       # ✅ Standalone copy shown
│   │   ├── FLIGHT-PLAN-SPECKIT-SETUP.md # ✅ Standalone copy shown
│   │   ├── current.md
│   │   ├── config.json                 # ✅ Config shown
│   │   └── history/
│   ├── .cursor/rules/
│   │   └── flight-plan.mdc             # ✅ Points to docs/project-rules.md
│   ├── CLAUDE.md (optional)            # ✅ Points to docs/project-rules.md
│   ├── memory/                         # ✅ SpecKit section added
│   │   └── constitution.md             # ✅ References docs/project-prd.md, docs/project-rules.md
│   ├── specs/                          # ✅ SpecKit section added
│   ├── src/
│   └── README.md                       # ✅ Marked as informational only
```

**Changes:**
- ✅ Moved `project-prd.md` and `project-rules.md` into `docs/` subdirectory
- ✅ Expanded `docs/` to show subdirectories (third-party, snippets, research)
- ✅ Added `.flight-plan/` standalone files (COMMANDS, PHASES, SPECKIT-SETUP)
- ✅ Added `config.json` to `.flight-plan/`
- ✅ Updated pointer file references to `docs/project-rules.md`
- ✅ Added `memory/` section with explicit PRD reference
- ✅ Added `specs/` section
- ✅ Added note that README is "informational only"

### 2. Second Directory Structure (Comment Fix)

**Before:**
```
├── memory/                     # SpecKit constitution (if enabled)
│   └── constitution.md         # References Flight Plan files
```

**After:**
```
├── memory/                     # SpecKit constitution (if enabled)
│   └── constitution.md         # References docs/project-prd.md, docs/project-rules.md
```

**Change:**
- ✅ Made explicit which files SpecKit references (not just "Flight Plan files")

### 3. SpecKit "Will Automatically" Section

**Before:**
```markdown
Spec-Kit will automatically:
- Check your current Flight Plan phase
- Apply phase-appropriate standards
- Reference project-prd.md for constraints  ← No path!
- Use quality gates from your PRD
- Always work with latest SpecKit practices
```

**After:**
```markdown
Spec-Kit will automatically:
- Check your current Flight Plan phase (from `.flight-plan/current.md`)
- Apply phase-appropriate standards (from `.flight-plan/FLIGHT-PLAN-PHASES.md`)
- Reference `docs/project-prd.md` for requirements and constraints  ← Full path + source file!
- Use quality gates from your PRD
- Always work with latest SpecKit practices
```

**Changes:**
- ✅ Added source files in parentheses for each bullet
- ✅ Changed `project-prd.md` to `docs/project-prd.md` with full path
- ✅ Clarified "constraints" to "requirements and constraints"

## Why This Matters

### For AI Agents:
- ✅ **Sees correct structure** when reading README
- ✅ **Knows where PRD lives** (`docs/project-prd.md`)
- ✅ **Understands SpecKit integration** (references PRD from `memory/constitution.md`)
- ✅ **Sees standalone nature** (all Flight Plan files in `.flight-plan/`)

### For Users:
- ✅ **Accurate documentation** - structure matches reality
- ✅ **Clear SpecKit integration** - knows what files SpecKit uses
- ✅ **Better understanding** - sees complete project layout
- ✅ **Correct expectations** - knows where files will be generated

### For SpecKit:
- ✅ **Clear PRD location** - `docs/project-prd.md` explicitly referenced
- ✅ **Constitution references** - `memory/constitution.md` points to PRD
- ✅ **Phase awareness** - knows to check `.flight-plan/current.md`
- ✅ **Standards alignment** - references `.flight-plan/FLIGHT-PLAN-PHASES.md`

## Complete Project Structure (Now Correct)

```
MyApp/your-project/
├── docs/                               ← All documentation
│   ├── project-prd.md                  ← Single source of truth
│   ├── project-rules.md                ← AI integration
│   ├── third-party/                    ← External API specs
│   ├── snippets/                       ← Code examples
│   └── research/                       ← Background docs
├── .flight-plan/                       ← Flight Plan internals (standalone)
│   ├── FLIGHT-PLAN-COMMANDS.md         ← Command execution logic
│   ├── FLIGHT-PLAN-PHASES.md           ← Phase standards
│   ├── FLIGHT-PLAN-SPECKIT-SETUP.md    ← SpecKit setup guide
│   ├── current.md                      ← Current status
│   ├── config.json                     ← Configuration
│   └── history/                        ← Milestones
├── .cursor/rules/
│   └── flight-plan.mdc                 ← Points to docs/project-rules.md
├── CLAUDE.md (optional)                ← Points to docs/project-rules.md
├── memory/                             ← SpecKit (if enabled)
│   └── constitution.md                 ← References docs/project-prd.md, docs/project-rules.md
├── specs/                              ← SpecKit feature specs (if enabled)
├── src/                                ← Your code
└── README.md                           ← Project overview (informational only)
```

## Result

**Before:**
- ❌ First structure showed files in wrong locations
- ❌ Missing standalone Flight Plan files in structure
- ❌ Vague SpecKit references ("Flight Plan files")
- ❌ No explicit PRD paths for SpecKit
- ❌ Incomplete view of project structure

**After:**
- ✅ Both structures show correct file locations
- ✅ Standalone Flight Plan files visible
- ✅ Explicit SpecKit references with paths
- ✅ Clear PRD location (`docs/project-prd.md`)
- ✅ Complete, accurate project structure
- ✅ SpecKit knows exactly where to find PRD

---

**Change Date:** 2025-10-27
**Impact:** README documentation accuracy
**Breaking:** No - only fixes documentation
**Benefit:** AI and users see correct project structure, SpecKit knows where to find PRD

