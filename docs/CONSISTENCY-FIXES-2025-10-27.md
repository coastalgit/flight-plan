# Consistency Fixes - 2025-10-27

## Problem

README contained operational-looking data that confused AI agents about where to find actual execution instructions.

**Issues Identified:**

1. **README had "PRD Version Tracking" section** with example history
   - Made README look like operational data
   - AI might think it needs to update README
   - Actual version history belongs in `framework/ai-refs/solution-overview.md`

2. **AI not picking up on Open Questions from PRD**
   - Unclear where to extract open questions from
   - No explicit instruction to read PRD Section 8
   - AI models like Claude Code were missing this

3. **README treated as execution reference**
   - No clear indication that README is informational only
   - AI agents reading README for execution logic instead of FLIGHT-PLAN-INIT.md

## Solutions Implemented

### 1. Added Clear AI Warning to README

**Added at top of README after title:**

```markdown
## ⚠️ FOR AI AGENTS

**This README is INFORMATIONAL ONLY.**

When executing Flight Plan commands:
- ✅ **Read:** `framework/FLIGHT-PLAN-INIT.md` for `init` execution logic
- ✅ **Read:** `framework/FLIGHT-PLAN-COMMANDS.md` for all command execution
- ✅ **Read:** `solution-prd-v*.md` for actual project requirements
- ❌ **DO NOT** treat README content as execution instructions

**This README explains the system. Execution files contain the actual logic.**
```

**Purpose:**
- Makes it crystal clear README is documentation, not instructions
- Directs AI to correct files for execution
- Prevents confusion about where to find operational logic

### 2. Removed Operational Data from README

**Removed:**
- Entire "PRD Version Tracking" section (26 lines)
- Example version history that looked operational

**Why:**
- Version history is generated in `framework/ai-refs/solution-overview.md`
- Example made it look like README contained operational data
- README should be purely informational about the system

### 3. Made Open Questions Extraction Explicit

**Added to `framework/FLIGHT-PLAN-INIT.md` STEP 4:**

```markdown
**From the PRD, extract:**
- Section 1: Solution name
- Section 3: Project list (names, types, descriptions)
- Section 4: Architecture and dependencies
- Section 5: Technology decisions
- Section 6: Development tools and MCP servers
- **Section 8: OPEN QUESTIONS** ← These become blockers if unresolved!
- Section 9: Technical specifications per project

**CRITICAL:** Open Questions from Section 8 MUST be shown in preview. 
If PRD has no Section 8 or it's empty, say "No open questions."
```

**Purpose:**
- Explicitly tells AI WHERE to find open questions
- Lists ALL sections AI should extract from PRD
- Makes it impossible to miss Section 8

### 4. Updated Preview Template

**Changed preview section title:**

**Before:**
```
📋 OPEN QUESTIONS FROM PRD
```

**After:**
```
📋 OPEN QUESTIONS FROM PRD (Section 8)
```

**Added explicit handling:**
```
[IF PRD Section 8 has questions, list them:]
• question-1: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]

[IF PRD Section 8 is empty or missing:]
✅ No open questions found in PRD.
```

**Purpose:**
- Makes it explicit that questions come from Section 8
- Provides fallback message if no questions
- AI can't miss this now

## Files Updated

### 1. `README.md` (Solution Root)
- ✅ Added "FOR AI AGENTS" warning at top
- ✅ Removed "PRD Version Tracking" section (operational data)
- ✅ Clarified README is informational only

### 1.5. `framework/GENERATOR.md` (Project README Template)
- ✅ Added "FOR AI AGENTS" warning to generated project READMEs
- ✅ Directs AI to `docs/project-rules.md` for context
- ✅ Makes it crystal clear: README = info, project-rules.md = execution

### 2. `framework/FLIGHT-PLAN-INIT.md`
- ✅ Added explicit PRD extraction instructions in STEP 4
- ✅ Listed all PRD sections to extract
- ✅ Made Section 8 (Open Questions) extraction CRITICAL
- ✅ Updated preview template to reference "Section 8" explicitly
- ✅ Added conditional handling for when no questions exist
- ✅ Marked README as "INFORMATIONAL ONLY" in context loading

## Project README Content Review

**What Project README Contains (INFORMATIONAL ONLY):**
- ✅ Project overview and purpose
- ✅ Tech stack from PRD
- ✅ Quick start / setup instructions
- ✅ Documentation file locations (with full paths)
- ✅ Flight Plan command reference (what commands exist)
- ✅ Architecture overview
- ✅ Dependencies list

**What Project README Does NOT Contain (No Operational Data):**
- ❌ No status tracking (that's in `.flight-plan/current.md`)
- ❌ No phase history (that's in `.flight-plan/history/`)
- ❌ No version tracking (that's in Git)
- ❌ No AI execution instructions (that's in `docs/project-rules.md`)

**AI Warning Added:**
```markdown
## ⚠️ FOR AI AGENTS

**This README is INFORMATIONAL ONLY.**

When working in this project:
- ✅ Read: `docs/project-rules.md` for all project context
- ✅ Read: `docs/project-prd.md` for specifications
- ✅ Read: `.flight-plan/current.md` for current status
- ✅ Read: `.flight-plan/FLIGHT-PLAN-COMMANDS.md` when user runs commands
- ❌ DO NOT treat README as execution instructions
```

## Result

### Before:
- ❌ AI confused about where to find execution logic
- ❌ README looked operational (version history in solution)
- ❌ Open questions extraction not explicit
- ❌ Different AI models had inconsistent behavior
- ❌ Project READMEs had no AI warning

### After:
- ✅ README clearly marked as informational only (both solution AND project)
- ✅ AI directed to correct execution files
- ✅ Open questions extraction is explicit and mandatory
- ✅ Consistent behavior across all AI models
- ✅ Project READMEs have same warning as solution README

## Testing

When AI runs `flight-plan init`:

1. **Sees warning in README:**
   - "This README is INFORMATIONAL ONLY"
   - "Read framework/FLIGHT-PLAN-INIT.md for execution"

2. **Reads FLIGHT-PLAN-INIT.md STEP 4:**
   - Explicitly told to read PRD Section 8 for open questions
   - Can't miss this instruction

3. **Shows preview with Section 8 reference:**
   - "OPEN QUESTIONS FROM PRD (Section 8)"
   - Lists questions or says "No open questions"

4. **Result:**
   - Consistent behavior across all AI models
   - Open questions always extracted
   - No confusion about operational vs informational data

## Benefits

✅ **Clear Separation:**
- README = Information about the system
- FLIGHT-PLAN-INIT.md = Execution instructions
- solution-prd-v*.md = Actual requirements

✅ **Explicit Instructions:**
- AI knows exactly where to find open questions
- No ambiguity about PRD sections
- Fallback handling for missing sections

✅ **Consistent Behavior:**
- Works with Claude Desktop, Claude Code, ChatGPT, Gemini
- All AI models follow same path
- No model-specific issues

✅ **Better UX:**
- Users see open questions in preview
- Clear recommendation to resolve during preview
- No surprise blockers later

---

**Change Date:** 2025-10-27
**Impact:** Solution-level documentation and init workflow
**Breaking:** No - only improves clarity
**Testing:** Verified with multiple AI models

