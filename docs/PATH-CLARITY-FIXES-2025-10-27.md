# Path Clarity Fixes - 2025-10-27

## Problem

Files referenced without paths, making it unclear where AI should look for them.

**Examples:**
- "project-prd.md" instead of "docs/project-prd.md"
- "project-rules.md" instead of "docs/project-rules.md"
- "current.md" instead of ".flight-plan/current.md"

This caused confusion for AI agents trying to locate files.

## Files Updated

### 1. `README.md` (Solution Root)

**Section: AI Integration**

**Before:**
```markdown
Each project gets a `project-rules.md` that tells the AI:
- Where to find project specs (project-prd.md)
- What solution-wide tools are available (solution-rules.md)
- Current phase and status (.flight-plan/current.md)

**AI-specific pointer files:**
- `.cursor/rules/flight-plan.mdc` → points to project-rules.md
```

**After:**
```markdown
Each project gets a `docs/project-rules.md` that tells the AI:
- Where to find project specs (`docs/project-prd.md`)
- Current phase and status (`.flight-plan/current.md`)
- Flight Plan commands (`.flight-plan/FLIGHT-PLAN-COMMANDS.md`)
- Phase standards (`.flight-plan/FLIGHT-PLAN-PHASES.md`)

**AI-specific pointer files:**
- `.cursor/rules/flight-plan.mdc` → points to `docs/project-rules.md`
```

**Also updated:**
```markdown
The AI will:
1. Check your current phase (`.flight-plan/current.md`)
2. Apply phase-appropriate standards (`.flight-plan/FLIGHT-PLAN-PHASES.md`)
3. Use available tools (MCP servers listed in `docs/project-rules.md`)
```

### 2. `framework/FLIGHT-PLAN-COMMANDS.md`

**Multiple locations updated:**

**1. Help command - Wrong location message:**
- Before: `- project-prd.md (project-level marker)`
- After: `- docs/project-prd.md (project-level marker)`

**2. Project-Level Commands heading:**
- Before: `(Context: directory with project-prd.md)`
- After: `(Context: directory with docs/project-prd.md)`

**3. Help command - What it does:**
- Before: `- project-prd.md → Project-level`
- After: `- docs/project-prd.md → Project-level`

**4. flight-plan status - Context:**
- Before: `**Context:** Project-level (has project-prd.md)`
- After: `**Context:** Project-level (has docs/project-prd.md)`

**5. flight-plan status - Prerequisites:**
- Before:
  ```
  1. ✅ project-prd.md exists in current directory
  2. ✅ .flight-plan/current.md exists
  3. ✅ .flight-plan/config.json exists
  ```
- After:
  ```
  1. ✅ docs/project-prd.md exists
  2. ✅ docs/project-rules.md exists
  3. ✅ .flight-plan/current.md exists
  4. ✅ .flight-plan/config.json exists
  ```

### 3. `framework/templates/solution-rules.md.template`

**Context Files section:**

**Before:**
```markdown
**Context Files for Any AI:**
1. Read this file first (solution-rules.md)
2. Read project-rules.md (project-specific)
3. Read project-prd.md (what to build)
4. Read .flight-plan/current.md (status)
```

**After:**
```markdown
**Context Files for Any AI (when in a project):**
1. Read `docs/project-rules.md` (project-specific, includes solution standards)
2. Read `docs/project-prd.md` (what to build - single source of truth)
3. Read `.flight-plan/current.md` (current phase and status)
4. Read `.flight-plan/FLIGHT-PLAN-COMMANDS.md` (when user runs commands)
```

## Complete Path Reference

For AI agents, here are ALL the files and their correct paths:

### Solution-Level Files
- `solution-prd-v*.md` (root)
- `README.md` (root)
- `LICENSE` (root)
- `framework/FLIGHT-PLAN-INIT.md`
- `framework/FLIGHT-PLAN-COMMANDS.md`
- `framework/FLIGHT-PLAN-PHASES.md`
- `framework/FLIGHT-PLAN-SPECKIT-SETUP.md`
- `framework/BRIEF-BUILDER-v1.1.md`
- `framework/GENERATOR.md`
- `framework/solution-rules.md` (generated)
- `framework/templates/*.template`
- `framework/ai-refs/solution-overview.md` (generated)
- `framework/ai-refs/notes.md` (generated)
- `framework/ai-refs/cursor.md` (generated)

### Project-Level Files
- `docs/project-prd.md` ← Single source of truth
- `docs/project-rules.md` ← AI integration layer
- `.flight-plan/FLIGHT-PLAN-COMMANDS.md` (standalone copy)
- `.flight-plan/FLIGHT-PLAN-PHASES.md` (standalone copy)
- `.flight-plan/FLIGHT-PLAN-SPECKIT-SETUP.md` (standalone copy)
- `.flight-plan/current.md` (status tracking)
- `.flight-plan/config.json` (configuration)
- `.flight-plan/history/` (milestones)
- `.cursor/rules/flight-plan.mdc` (pointer)
- `README.md` (project overview)
- `src/` (source code)

## Benefits

✅ **Crystal Clear Paths**
- AI knows EXACTLY where to find each file
- No ambiguity about file locations
- Reduces "file not found" errors

✅ **Consistent References**
- All documentation uses full paths
- Easier to grep/search for file references
- Clear separation: root vs docs/ vs .flight-plan/

✅ **Better AI Behavior**
- AI doesn't guess about file locations
- Follows explicit paths in documentation
- Works consistently across all AI models

## Testing

When AI reads README or FLIGHT-PLAN-COMMANDS.md:
- ✅ Sees `docs/project-prd.md` → knows to look in docs/ subdirectory
- ✅ Sees `.flight-plan/current.md` → knows to look in .flight-plan/ subdirectory
- ✅ Sees `docs/project-rules.md` → knows full path immediately

No guessing, no errors!

---

**Change Date:** 2025-10-27
**Impact:** Documentation clarity
**Breaking:** No - only improves existing references
**Benefit:** AI knows exact file locations

---

## Update: Docs Structure Simplified (2025-10-28)

The original `docs/` subdirectories (`third-party/`, `snippets/`, `research/`) were later removed to simplify the structure. Projects now only have:
- `docs/project-prd.md`
- `docs/project-rules.md`

Users can create their own subdirectories as needed.

**See:** `docs/SIMPLIFY-DOCS-STRUCTURE-2025-10-28.md`

