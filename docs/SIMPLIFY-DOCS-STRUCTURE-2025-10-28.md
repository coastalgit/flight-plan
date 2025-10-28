# Simplify docs/ Directory Structure - 2025-10-28

**Date:** 2025-10-28  
**Issue:** Unnecessary subdirectories in `docs/`  
**Solution:** Flat `docs/` structure with just PRD and rules

---

## The Problem

When generating projects, the system was creating subdirectories within `docs/`:
```
docs/
├── project-prd.md
├── project-rules.md
├── third-party/         ← Unnecessary
├── snippets/            ← Unnecessary
└── research/            ← Unnecessary
```

**User feedback:**
> "When creating projects it is creating sub dirs in docs - research, snippets, and third-party! No need for this - just stick in docs!"

---

## The Solution

**Simplified `docs/` structure:**
```
docs/
├── project-prd.md
└── project-rules.md
```

**Benefits:**
- Cleaner structure
- Users can add their own files/folders as needed
- No empty placeholder directories
- Simpler to understand

**Rationale:**
- Users will create their own docs organization if needed
- Prescribing subdirectories adds unnecessary complexity
- PRD and rules are the only required docs files

---

## Files Updated

### 1. `framework/GENERATOR.md`
**Project structure diagram (lines 337-354):**
- Removed `third-party/` subdirectory
- Removed `snippets/` subdirectory
- Removed `research/` subdirectory

**Before:**
```
├── docs/
│   ├── project-prd.md
│   ├── project-rules.md
│   ├── third-party/            # Empty (for external API specs)
│   ├── snippets/               # Empty (for code examples)
│   └── research/               # Empty (for background docs)
```

**After:**
```
├── docs/
│   ├── project-prd.md
│   └── project-rules.md
```

### 2. `README.md`
**Two locations updated:**

**Location 1 - Complete Directory Structure (lines 159-165):**
- Removed subdirectories from project-a example

**Location 2 - What Gets Generated (lines 350-357):**
- Removed subdirectories from project structure diagram

---

## Impact

**Breaking:** No - projects still work exactly the same
**Migration:** Existing projects keep their subdirectories if already created
**Future:** New projects get cleaner structure

---

## Key Takeaway

**Less is more:**
- Only create what's actually required
- Let users add their own organization
- Keep generated structure minimal and clean

---

**Change Date:** 2025-10-28  
**User Request:** Simplify docs structure  
**Files Updated:**
- `framework/GENERATOR.md` - Project structure diagram (lines 337-354)
- `README.md` - Two directory structure examples (lines 159-165, 350-357)
- `framework/FLIGHT-PLAN-PHASES.md` - Phase 2 outputs (lines 46-49)
- `framework/FLIGHT-PLAN-INIT.md` - Level 3 directory structure (lines 155-187)
- `docs/PATH-CLARITY-FIXES-2025-10-27.md` - Updated with note about simplification

