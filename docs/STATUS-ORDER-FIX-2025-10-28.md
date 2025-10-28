# Status Display Order Fix - 2025-10-28

**Date:** 2025-10-28  
**Issue:** SpecKit prompt shown before status, user has no context  
**Solution:** Reorder steps to show status FIRST, then prompt for SpecKit

---

## The Problem

When user runs `flight-plan status` for the first time in a project:

**Wrong Order (Original):**
1. Read files
2. Check SpecKit → **Prompt user immediately**
3. Show status
4. Provide guidance

**Result:** User is asked about SpecKit before seeing what project this is or what phase they're in. No context!

---

## User Report

Testing with two different AI implementations revealed the issue:

**Cursor Agent (Good UX):**
- Showed status report FIRST
- THEN asked about SpecKit

**Claude Code Extension (Bad UX):**
- Did NOT show status first
- Just asked about SpecKit
- AI explained its reasoning out loud: "Now I have all the information I need. According to the instructions, I should first check if SpecKit needs to be prompted..."

The instructions were technically correct but in the wrong order for good UX.

---

## The Solution

**Correct Order (Fixed):**
1. Read files
2. **Show status FIRST** → User gets context
3. Check SpecKit → Prompt if needed
4. Provide guidance

**Why this is better:**
- User sees: "🎯 Project: backend-api / 📍 Phase: 2"
- User has context about what project and phase
- THEN gets asked about tooling
- Can make informed decision

---

## Files Changed

### `framework/templates/project-commands.md.template`

**Changes Made:**

**1. Swapped Steps 2 and 3 (lines 136-163):**
- Step 2: Now "Show current status FIRST (give user context)"
- Step 3: Now "Check SpecKit (ONLY if not prompted before)"

**2. Added explicit instruction (line 146):**
```markdown
**IMPORTANT:** Show the status immediately in your response. Don't explain what you're about to do, just do it.
```

**3. Added clarity to Step 3 (line 150):**
```markdown
**After showing status,** check if SpecKit prompt is needed:
```

**4. Added skip condition (line 163):**
```markdown
**If config.json shows speckit_prompted: true** → Skip this step, continue to step 4
```

**5. Updated example outputs (lines 189-254):**
- Added "Example Output (First Time - With SpecKit Prompt)"
- Added "Example Output (Subsequent Times - No SpecKit Prompt)"
- Shows status at top, SpecKit prompt in middle

**6. Added anti-explanation rule (lines 49-51):**
```markdown
- ❌ **DO NOT explain what you're about to do** - just do it!

**CRITICAL:** Don't say things like "Now I have all the information I need" 
or "Let me show you the status". Just show the status. Users want results, 
not explanations of your process.
```

---

## New Flow

```
User: "flight-plan status"
↓
AI reads: .flight-plan/current.md, config.json, docs/project-prd.md
↓
AI IMMEDIATELY outputs:
```
🎯 Project: backend-api
📍 Phase: 2 - Research & Context
📊 Status: In Progress

Last Activity:
- Gathered API documentation

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Would you like to enable Spec-Kit? (yes/no)
[explanation of SpecKit]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Phase 2 Objectives:
[guidance]
```
```

**NO explanations like:**
- ❌ "Now I have all the information I need"
- ❌ "According to the instructions, I should..."
- ❌ "Let me show you the status"

**JUST output the status!**

---

## Benefits

✅ **Better UX**
- User immediately sees project context
- Can make informed decision about SpecKit

✅ **Consistent behavior**
- Both Cursor agent and Claude Code will now show status first

✅ **Cleaner output**
- No AI "thinking out loud"
- Just results

✅ **Logical flow**
- Context before questions
- Information before decisions

---

## Testing

When user runs `flight-plan status` in a new project:

**Expected first time:**
```
🎯 Project: [name]
📍 Phase: [N]
📊 Status: [status]

[SpecKit prompt]

[Phase guidance]
```

**Expected subsequent times:**
```
🎯 Project: [name]
📍 Phase: [N]
📊 Status: [status]

[Phase guidance]
[NO SpecKit prompt]
```

---

**Change Date:** 2025-10-28  
**Severity:** High (affects first-run UX for all projects)  
**Impact:** Better user experience, clearer context  
**Breaking:** No (just reorders output)

