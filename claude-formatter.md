# Claude Output Formatting Instructions

## CRITICAL: How to Output Formatted Text to Users

**PROBLEM:** When Claude outputs markdown text directly in chat, the rendering engine collapses blank lines and removes spacing, making output unreadable even when the source markdown is correct.

**SOLUTION:** Always wrap formatted output in code blocks using triple backticks.

---

## ✅ CORRECT METHOD

When outputting any structured information (previews, status reports, formatted lists, etc.):

Always wrap your output in triple backticks (code blocks).

This preserves:
- Blank lines between sections
- Blank lines after list items
- All spacing and formatting

Without code blocks, blank lines collapse and create walls of text.

---

## ❌ INCORRECT METHOD

**DO NOT** output formatted text directly without code blocks - the markdown renderer will collapse all blank lines even if they exist in your source.

---

## FORMATTING RULES (Inside Code Blocks)

### Blank Lines
- Blank line between major sections
- Blank line after each list item
- Blank line between project/item blocks

### List Items Format
```
• First item
  Details about first item

• Second item
  Details about second item

• Third item
  Details about third item
```

**NOT like this:**
```
• First item
  Details
• Second item
  Details
```

### Nested Content
Use 2 spaces for indentation:

```
• Main item

  Nested detail line 1

  Nested detail line 2

  Sub-section:
    - Sub-item 1
    - Sub-item 2
```

---

## THE SOLUTION IS SIMPLE

**Just wrap your output in triple backticks (code blocks).**

That's it. No files needed. No validation needed. Just use code blocks for formatted output.

---

## EXAMPLE: Flight Plan Preview Output

### ✅ CORRECT (with code block wrapper):

User wraps output in triple backticks, preserving all blank lines and spacing.

User sees properly spaced, readable output.

### ❌ INCORRECT (without code block wrapper):

User outputs markdown directly, blank lines collapse into walls of text.

Even though blank lines exist in source, rendering removes them.

---

## WHEN TO USE CODE BLOCKS

Use code blocks for:
- ✅ Flight Plan previews and status outputs
- ✅ Structured lists with multiple items
- ✅ Any output with required blank line spacing
- ✅ ASCII diagrams and flow charts
- ✅ Directory structure trees
- ✅ Any formatted reports

Do NOT use code blocks for:
- ❌ Normal conversational responses
- ❌ Simple one-line answers
- ❌ Explanations without formatting requirements

---

## SUMMARY

**Golden Rule:** When formatting matters, wrap output in triple backticks (code blocks).

This is the ONLY way to guarantee blank lines and spacing are preserved in the rendered UI.

---

**Created:** 2025-10-28
**Context:** Learned during Flight Plan implementation debugging
**Issue:** Claude repeatedly produced correct markdown source but rendering collapsed blank lines
**Solution:** Always use code block wrapper for formatted output
