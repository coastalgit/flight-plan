# SpecKit AI Commands Clarification Fix

**Date:** 2025-10-29  
**Issue:** AI agents might try to run `/speckit.*` commands in terminal instead of processing them as AI chat commands  
**Status:** ✅ Resolved

---

## Problem

SpecKit commands like `/speckit.specify`, `/speckit.plan`, etc. are **Cursor AI chat commands**, not terminal commands. However, without explicit clarification, AI agents might mistakenly:

1. ❌ Try to run them in terminal using `run_terminal_cmd`
2. ❌ Treat them as shell commands
3. ❌ Show errors when trying to execute them

This would confuse users and break the SpecKit workflow.

---

## Root Cause

### Ambiguity in Documentation

Previous documentation mentioned `/speckit.*` commands without explicitly stating they are AI chat commands:

**Before:**
```markdown
Commands: `/speckit.specify`, `/speckit.plan`, `/speckit.tasks`, `/speckit.implement`
```

**Problem:** Could be interpreted as either:
- Shell commands (wrong)
- AI chat commands (correct)

---

## Solution

Added **explicit warnings and clarifications** in all relevant files:

### Pattern Used:

```markdown
**⚠️ CRITICAL: SpecKit commands are Cursor AI chat commands, NOT terminal commands!**
- ✅ Type `/speckit.specify` in **Cursor chat** (AI command)
- ❌ Do NOT run in terminal
- ❌ Do NOT use `run_terminal_cmd` or shell execution
- ✅ AI processes these directly like "flight-plan status"
```

---

## Files Updated

### 1. `framework/FLIGHT-PLAN-SPECKIT-SETUP.md`

**Lines 40-44:** Added critical warning in "What is SpecKit?" section
```markdown
**⚠️ CRITICAL: These are Cursor AI chat commands, NOT terminal commands!**
- ✅ Type `/speckit.specify` in **Cursor chat** (AI command)
- ❌ Do NOT run in terminal
- ❌ Do NOT use `run_terminal_cmd` or shell execution
- ✅ AI processes these directly like natural language
```

**Lines 595-596:** Added warning in "NEXT STEPS" section
```markdown
⚠️ IMPORTANT: Type these in CURSOR CHAT, not terminal!
These are AI commands (like "flight-plan status"), not shell commands.
```

### 2. `framework/templates/project-commands.md.template`

**Lines 356-360:** Added critical warning in "SpecKit Integration" section
```markdown
**⚠️ CRITICAL: SpecKit commands are Cursor AI chat commands, NOT terminal commands!**
- ✅ Type `/speckit.specify` in **Cursor chat** (AI command)
- ❌ Do NOT run in terminal
- ❌ Do NOT use `run_terminal_cmd` or shell execution
- ✅ AI processes these directly like "flight-plan status"
```

**Impact:** This template is used to generate `.flight-plan/FLIGHT-PLAN-COMMANDS.md` in every project, so all new projects will have this warning.

### 3. `framework/FLIGHT-PLAN-COMMANDS.md`

**Lines 183-187:** Added critical warning in "What is Spec-Kit?" section
```markdown
**⚠️ CRITICAL: SpecKit commands are Cursor AI chat commands, NOT terminal commands!**
- ✅ Type `/speckit.specify` in **Cursor chat** (AI processes directly)
- ❌ Do NOT run in terminal (no `run_terminal_cmd`)
- ❌ Do NOT execute as shell commands
- ✅ Handle like "flight-plan status" - natural language AI commands
```

### 4. `README.md`

**Lines 460-462:** Added critical warning in "Spec-Kit Components" section
```markdown
**⚠️ CRITICAL: SpecKit commands are Cursor AI chat commands, NOT terminal commands!**
- ✅ Type `/speckit.specify` in **Cursor chat** (like "flight-plan status")
- ❌ Do NOT run in terminal
```

**Lines 496-497:** Added inline clarification in setup steps
```markdown
- `/speckit.specify from docs/project-spec.md` ← Type in Cursor chat, not terminal!
- `/speckit.plan from docs/project-plan.md` ← Type in Cursor chat, not terminal!
```

---

## Command Comparison

### AI Chat Commands (NOT terminal commands)

**Flight Plan Commands:**
```
flight-plan init
flight-plan status
flight-plan sync
flight-plan prd refresh
flight-plan note "text"
```

**SpecKit Commands:**
```
/speckit.specify
/speckit.plan
/speckit.tasks
/speckit.implement
/speckit.clarify
/speckit.checklist
/speckit.analyze
```

**How to use:**
- ✅ Type directly in Cursor chat
- ✅ AI processes them using file read/write capabilities
- ✅ No shell execution needed

### Actual Terminal Commands

**Installation Commands:**
```bash
# These ARE terminal commands
uv --version
uvx --version
npx -y @modelcontextprotocol/server-firebase
```

**How to use:**
- ✅ User runs in terminal
- ✅ AI shows command to user
- ✅ AI waits for user to execute

---

## AI Agent Guidelines

### When User Types `/speckit.specify`:

**✅ CORRECT Response:**
```
AI reads the command as an instruction
AI processes it directly (like "flight-plan status")
AI uses file operations to create specs
AI shows output to user
```

**❌ WRONG Response:**
```
AI tries: run_terminal_cmd("/speckit.specify")
Result: Command not found
Error: This breaks the workflow
```

### Key Principle:

**Commands starting with:**
- `flight-plan` → AI chat command (process directly)
- `/speckit.` → AI chat command (process directly)
- `uv`, `npm`, `npx` → Terminal command (show to user, wait for execution)

---

## Testing

### Verify Correct Behavior:

**Test 1: User types `/speckit.specify`**
```
Expected: AI processes command, creates spec file
Wrong: AI tries to run in terminal
```

**Test 2: User types `flight-plan status`**
```
Expected: AI reads files, shows status
Wrong: AI tries to run in terminal
```

**Test 3: User needs to install SpecKit**
```
Expected: AI shows `uvx ...` command, waits for user
Wrong: AI tries to run installation command
```

---

## Related Documentation

- `framework/FLIGHT-PLAN-SPECKIT-SETUP.md` - SpecKit setup guide (updated)
- `framework/templates/project-commands.md.template` - Project commands template (updated)
- `framework/FLIGHT-PLAN-COMMANDS.md` - Solution-level commands (updated)
- `README.md` - Main documentation (updated)

---

## Impact

### Positive Changes:
- ✅ **Clarity:** AI agents now understand command types
- ✅ **Prevention:** Stops terminal execution attempts
- ✅ **Consistency:** Same pattern across all files
- ✅ **User Experience:** No confusing errors

### Coverage:
- ✅ All SpecKit command mentions updated
- ✅ Both solution-level and project-level files
- ✅ Template will propagate to new projects
- ✅ README clarifies for users

---

## Future Enhancements

### Potential Improvements:
1. **Command Registry:** Explicit list of AI commands vs terminal commands
2. **Validation:** Tool to check AI isn't using `run_terminal_cmd` for AI commands
3. **More Patterns:** Identify other command types that need clarification
4. **Agent Training:** Examples of correct command handling

---

**Fix Date:** 2025-10-29  
**Reporter:** User  
**Fixed By:** AI Assistant  
**Status:** ✅ Complete - All SpecKit command mentions clarified  

---

## Summary

**Before Fix:**
- ❌ Ambiguous documentation
- ❌ AI might try terminal execution
- ❌ Potential for user confusion

**After Fix:**
- ✅ Explicit warnings in 4 files
- ✅ Clear distinction: AI commands vs terminal commands
- ✅ Consistent pattern across all documentation
- ✅ Templates propagate fix to new projects

**Key Takeaway:** `/speckit.*` commands are AI chat commands (like "flight-plan status"), NOT terminal commands. AI should process them directly, not execute them in a shell.

