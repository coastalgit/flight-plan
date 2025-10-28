# Output Formatting Fix - Code Block Wrapper Required

**Date:** 2025-10-28  
**Issue:** CLI/IDE output formatting collapsed into unreadable walls of text  
**Solution:** Always wrap formatted output in code blocks

---

## The Problem

When AI outputs formatted text (previews, status reports, lists) directly to CLI or IDE chat:
- **The markdown renderer collapses ALL blank lines**
- Even if the source markdown has perfect formatting
- Results in walls of unreadable text
- Took 6 attempts with Claude Code to diagnose

### Example of the Problem:

**AI outputs this (with blank lines):**
```markdown
• Project 1
  Description here

• Project 2
  Description here
```

**User sees this (blank lines collapsed):**
```
• Project 1
  Description here
• Project 2
  Description here
```

---

## The Solution

**ALWAYS wrap formatted output in triple backticks (code blocks):**

````markdown
```
• Project 1
  Description here

• Project 2
  Description here
```
````

**Code blocks preserve:**
- ✅ Blank lines between sections
- ✅ Blank lines after list items
- ✅ All spacing and formatting
- ✅ Indentation

---

## Files Updated

### 1. `README.md`
Added new section after "Ready to start?":

**"🎨 CRITICAL: Output Formatting for CLI/IDE"**
- Explains why code blocks are needed
- Lists what to wrap (previews, status, etc.)
- Lists what NOT to wrap (normal conversation)

### 2. `framework/FLIGHT-PLAN-COMMANDS.md`
Updated "Output Formatting Standards" section:

**Added "🚨 RULE #1: ALWAYS Wrap Formatted Output in Code Blocks"**
- Moved to top of formatting standards
- Emphasized with 🚨 emoji
- Included quadruple backticks example
- Explained the "why" (blank line collapse)

### 3. `framework/FLIGHT-PLAN-INIT.md`
Updated STEP 5 (SHOW PREVIEW):

**Added "🚨 CRITICAL: ALWAYS WRAP PREVIEW IN CODE BLOCKS 🚨"**
- Placed immediately before formatting rules
- Mentioned "took 6 attempts to discover"
- Showed quadruple backticks wrapper
- Clear warning: "Without code block wrapper = Wall of unreadable text!"

---

## Reference File Created

**`claude-formatter.md`** (user-created)
- Documents the formatting issue in detail
- Explains the solution (code blocks)
- Provides examples of correct vs incorrect
- Lists when to use code blocks vs not

This file was created by the user after experiencing the issue firsthand with Claude Code in Cursor.

---

## Key Takeaway

**For CLI/IDE output:**
- Formatted/structured output → ALWAYS wrap in code blocks
- Normal conversation → NO code blocks

**This is the ONLY way to preserve spacing in CLI/IDE UIs.**

---

## Testing Note

This issue was discovered during iterative testing with Claude Code (Cursor extension). The AI repeatedly produced correctly formatted markdown source, but the UI rendering collapsed all blank lines, making output unreadable.

Solution was only discovered after user explicitly asked for formatting instructions after 6 failed attempts.

