# No Proactive SpecKit Suggestions - 2025-10-28

**Date:** 2025-10-28  
**Issue:** AI asking "Want me to kick off a first feature spec?" after SpecKit setup  
**Solution:** Explicit instructions to STOP and not make proactive suggestions

---

## The Problem

**User Report:**
> "Why does it say something like 'Want me to kick off a first feature spec (e.g., "Tasks CRUD API")?' when using spec kit? This happened after constitution.md created but before /specify.specify"

**What was happening:**
After SpecKit setup completed successfully and showed the "NEXT STEPS" message, the AI was improvising by asking:
- "Want me to kick off a first feature spec?"
- "Shall we start with the Tasks CRUD API?"
- Other proactive suggestions

**Root cause:**
The instructions showed what SpecKit commands were available, but didn't explicitly tell AI to STOP there. AI was being "helpful" and going beyond instructions.

---

## The Instructions Said

**In FLIGHT-PLAN-SPECKIT-SETUP.md (lines 501-520):**
```markdown
🚀 NEXT STEPS:

Use SpecKit commands for feature development:
  /speckit.specify       - Create feature specification
  /speckit.plan          - Create implementation plan
  ...

SpecKit now has full context from your Flight Plan:
✓ Current phase and standards
✓ Tech stack and constraints
✓ Quality requirements
```

**This is INFORMATIONAL** - just telling the user what's available.

**But AI interpreted it as:** "I should help them get started!"

---

## The Fix

### 1. Added Explicit STOP Instruction

**File:** `framework/FLIGHT-PLAN-SPECKIT-SETUP.md` (lines 522-533)

**Added after the success message:**
```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️  FOR AI: STOP HERE

DO NOT offer to:
- "kick off a first feature spec"
- "start with a feature"
- "create a spec for X"
- ANY proactive suggestions

The user will use SpecKit commands when THEY decide to.
Your job is DONE after showing the success message above.
```

### 2. Added Reminder in Project Commands

**File:** `framework/templates/project-commands.md.template` (lines 162, 169)

**In SpecKit setup flow (line 162):**
```markdown
│  └─ STOP after showing success message (DO NOT offer to create specs)
```

**After the flow (line 169):**
```markdown
**CRITICAL:** After SpecKit setup completes, DO NOT ask "Want me to kick off 
a first feature spec?" or similar. The user will use SpecKit commands when 
they're ready.
```

---

## Why This Matters

**User Experience:**
- ✅ User sees what tools are available
- ✅ User decides when to use them
- ❌ AI doesn't pressure or suggest next steps
- ❌ AI doesn't assume user wants to start immediately

**User Control:**
- User might want to review the constitution
- User might want to plan their approach
- User might want to work on something else first
- User will use SpecKit commands when THEY'RE ready

**AI Behavior:**
- Show information
- Don't make suggestions
- Don't be "helpful" beyond instructions
- Stop when told to stop

---

## Expected Behavior Now

**After SpecKit setup completes:**

```
✅ SpecKit configured successfully!

Updated files:
✓ .specify/memory/constitution.md (generated from Flight Plan context)
✓ .flight-plan/config.json (tracking updated)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 NEXT STEPS:

Use SpecKit commands for feature development:

  /speckit.specify       - Create feature specification
  /speckit.plan          - Create implementation plan
  /speckit.tasks         - Break down into tasks
  /speckit.implement     - Execute implementation

Optional enhancement commands:
  /speckit.clarify       - Ask clarifying questions (before /speckit.plan)
  /speckit.checklist     - Generate quality checklist (after /speckit.plan)
  /speckit.analyze       - Check consistency (before /speckit.implement)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SpecKit now has full context from your Flight Plan:
✓ Current phase and standards
✓ Tech stack and constraints
✓ Quality requirements

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**THEN STOP.** No questions, no suggestions, no offers.

**User will type `/speckit.specify` when ready.**

---

## Related Pattern

This is the same issue we fixed with:
- AI explaining its reasoning ("Now I have all the information I need...")
- AI showing status order (SpecKit before status)

**Pattern:** AI being "helpful" beyond instructions

**Solution:** Explicit "STOP" and "DO NOT" instructions

---

## Files Changed

1. **`framework/FLIGHT-PLAN-SPECKIT-SETUP.md`**
   - Lines 522-533: Added "⚠️ FOR AI: STOP HERE" section
   - Lists what NOT to offer
   - Clear end point

2. **`framework/templates/project-commands.md.template`**
   - Line 162: Added STOP instruction in flow
   - Line 169: Added CRITICAL reminder after flow
   - Reinforces no proactive suggestions

---

**Change Date:** 2025-10-28  
**Severity:** Medium (UX issue, not functional)  
**Impact:** AI stops being pushy after SpecKit setup  
**Breaking:** No

