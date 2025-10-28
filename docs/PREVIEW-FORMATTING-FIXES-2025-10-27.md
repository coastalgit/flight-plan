# Preview Formatting Fixes - 2025-10-27

## Problem

When AI outputs the `flight-plan init` preview, the text runs together without proper formatting:
- Technology decisions displayed as continuous paragraphs
- No blank lines between list items
- No proper spacing between sections
- Hard to read and scan

**User Report:**
> "When technology decision is spat out from an 'init', it is not formatting nicely i.e. with carriage returns etc. You will need to explicitly indicate readable formatting throughout etc - whether carriage returns, bullets, paragraph breaks etc"

**Root Cause:**
- Template had vague instructions like "[List key tech decisions and reasoning:]"
- No explicit formatting rules
- AI models interpret formatting differently
- Some AI output everything as continuous text

## Solution

### 1. Added Global Formatting Standards

Added **"Output Formatting Standards"** section to `framework/FLIGHT-PLAN-COMMANDS.md` at the beginning (right after Overview).

This section applies to **ALL** Flight Plan command outputs:
- Preview outputs (`init`, `sync`)
- Status outputs (`status` command)
- Apply confirmations
- Error messages
- All AI responses

### 2. Added Specific Rules to Init Preview

Added explicit formatting rules to `framework/FLIGHT-PLAN-INIT.md` STEP 5 (SHOW PREVIEW) with section-specific examples.

### 1. Added Critical Formatting Rules Header

**Added before preview template:**
```markdown
**CRITICAL FORMATTING RULES:**
- ✅ Use blank lines between sections for readability
- ✅ Use bullet points (•) with space after for lists
- ✅ Use line breaks after each list item
- ✅ Indent nested content with 2 spaces
- ✅ Keep each decision/question on separate lines
- ✅ Add blank line after each project in PROJECT DETAILS
- ❌ DO NOT run text together in continuous paragraphs
- ❌ DO NOT omit line breaks between items
```

**Purpose:**
- Sets explicit expectations for ALL output formatting
- Positive (✅) and negative (❌) rules for clarity
- Covers all common formatting scenarios

### 2. Updated TECHNOLOGY DECISIONS Section

**Before:**
```markdown
🔧 TECHNOLOGY DECISIONS (from PRD)

[List key tech decisions and reasoning:]
• [Decision 1]: [Reason from PRD]
• [Decision 2]: [Reason from PRD]
```

**After:**
```markdown
🔧 TECHNOLOGY DECISIONS (from PRD)

[Extract from PRD Section 5 and Section 11 (Design Rationale)]
[Format as bullet list with blank line after each decision:]

• [Decision 1 name]: [Brief reason from PRD]

• [Decision 2 name]: [Brief reason from PRD]

• [Decision 3 name]: [Brief reason from PRD]

[Example:]
• Database Choice (PostgreSQL): Chosen for ACID compliance and JSON support

• Authentication (JWT): Stateless auth for microservices, easier scaling

• Hosting (Vercel): Zero-config deployments, built-in CDN, serverless functions
```

**Changes:**
- ✅ Explicit instruction: "with blank line after each decision"
- ✅ Shows multiple items in template
- ✅ Provides concrete examples with proper spacing
- ✅ References specific PRD sections to extract from

### 3. Updated OPEN QUESTIONS Section

**Before:**
```markdown
[IF PRD Section 8 has questions, list them:]
• question-1: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]
• question-2: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]
```

**After:**
```markdown
[IF PRD Section 8 has questions, format with blank lines between each:]

• question-1: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]

• question-2: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]

• question-3: [Question text from PRD] (Priority: High/Medium/Low)
  → Will block: [project-name]
```

**Changes:**
- ✅ Explicit instruction: "with blank lines between each"
- ✅ Shows 3 items (not just 2) to reinforce pattern
- ✅ Blank lines in template show exact spacing

### 4. Updated PROJECT DETAILS Section

**Before:**
```markdown
📦 PROJECT DETAILS

[For each project, show:]

• **[project-name]**
  Type: [Frontend/Backend/Functions/etc from PRD]
  Purpose: [one-line from PRD]
  ...

[Repeat for each project]
```

**After:**
```markdown
📦 PROJECT DETAILS

[For each project, format with blank line after each project block:]

• **[project-name]**
  Type: [Frontend/Backend/Functions/etc from PRD]
  Purpose: [one-line from PRD]
  ...

• **[project-2-name]**
  Type: [Frontend/Backend/Functions/etc from PRD]
  Purpose: [one-line from PRD]
  ...

[Continue with blank line between each project...]
```

**Changes:**
- ✅ Explicit instruction: "with blank line after each project block"
- ✅ Shows 2 full project examples
- ✅ Demonstrates exact spacing between projects
- ✅ Clear continuation instruction

### 5. Updated ARCHITECTURE Section

**Before:**
```markdown
🏗️  ARCHITECTURE (from PRD)

[Show high-level architecture if mentioned in PRD]
Example:
  User → portfolio-site → Netlify CDN
  Contact Form → Netlify Forms → Email
```

**After:**
```markdown
🏗️  ARCHITECTURE (from PRD)

[Show high-level architecture if mentioned in PRD]
[Use ASCII diagram with clear line breaks:]

[Example for simple architecture:]
  User → portfolio-site → Netlify CDN
  Contact Form → Netlify Forms → Email

[Example for complex architecture - use blank lines between tiers:]
  Client Layer:
    frontend (React)
  
  API Layer:
    backend-api (Express)
      ↓
    auth-service (JWT)
  
  Data Layer:
    PostgreSQL database
    Redis cache
```

**Changes:**
- ✅ Explicit instruction: "Use ASCII diagram with clear line breaks"
- ✅ Provides both simple and complex examples
- ✅ Shows tier separation with blank lines
- ✅ Demonstrates nested indentation

## Formatting Rules Summary

### For Lists:
```
• Item 1: Description

• Item 2: Description

• Item 3: Description
```
- ✅ Blank line after each item
- ✅ Bullet point (•) with space
- ✅ Colon and space before description

### For Sections:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 SECTION TITLE

Content here...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
- ✅ Blank line before and after separators
- ✅ Blank line after section title

### For Nested Content:
```
• **Project Name**
  Type: Frontend
  Purpose: User interface
  
  Technology Stack:
    Language: TypeScript
    Framework: React
```
- ✅ 2-space indent for nested items
- ✅ Blank line after nested sections

### For Multi-Line Items:
```
• question-1: Long question text here? (Priority: High)
  → Will block: project-name
  
  Additional context here...
```
- ✅ Indented continuation lines
- ✅ Blank line after complete item

## Benefits

### For Users:
✅ **Readable Output**
- Easy to scan preview
- Clear section separation
- Visual hierarchy obvious

✅ **Professional Appearance**
- Consistent formatting
- No wall of text
- Proper spacing throughout

### For AI:
✅ **Clear Instructions**
- Explicit formatting rules up front
- Examples show exact spacing
- Both positive and negative rules

✅ **Consistent Behavior**
- All AI models follow same rules
- No ambiguity about formatting
- Examples demonstrate expected output

## Testing

### Good Output Example:
```
🔧 TECHNOLOGY DECISIONS (from PRD)

• Database (PostgreSQL): ACID compliance needed for transactions

• Auth (JWT): Stateless tokens for microservices architecture

• Hosting (Vercel): Zero-config deployments with built-in CDN
```

### Bad Output Example (What We Fixed):
```
🔧 TECHNOLOGY DECISIONS (from PRD)
• Database (PostgreSQL): ACID compliance needed for transactions • Auth (JWT): Stateless tokens for microservices architecture • Hosting (Vercel): Zero-config deployments with built-in CDN
```

## Global vs. Specific Formatting Rules

### Global Rules (FLIGHT-PLAN-COMMANDS.md)
**Location:** Top of file, applies to ALL outputs

**Purpose:**
- Universal formatting standards
- Applies to all commands (init, sync, status, prd refresh)
- Consistent behavior across all outputs

**Content:**
- Blank line rules
- Symbol usage (•, +, ~, -, ✅, ⚠️, ❌)
- Indentation standards
- Line break requirements

### Specific Rules (FLIGHT-PLAN-INIT.md)
**Location:** STEP 5 (SHOW PREVIEW)

**Purpose:**
- Init preview-specific formatting
- Section-by-section examples
- Shows exact spacing for each template section

**Content:**
- How to format TECHNOLOGY DECISIONS
- How to format OPEN QUESTIONS
- How to format PROJECT DETAILS
- How to format ARCHITECTURE

### Why Both?

**Global rules provide:**
- Overall principles (use blank lines, no walls of text)
- Consistent standards across ALL commands
- Single source of truth for formatting

**Specific rules provide:**
- Concrete examples for complex sections
- Template-level guidance
- Shows exact output format

## Result

**Before:**
- ❌ Technology decisions ran together
- ❌ No spacing between items
- ❌ Hard to read and scan
- ❌ Inconsistent formatting across AI models
- ❌ No universal formatting standards

**After:**
- ✅ Global formatting standards apply to ALL outputs
- ✅ Clear spacing between all items
- ✅ Proper blank lines throughout
- ✅ Easy to read and scan
- ✅ Consistent formatting rules
- ✅ Professional appearance
- ✅ Examples demonstrate exact spacing
- ✅ Both universal rules AND specific examples

---

**Change Date:** 2025-10-27
**Impact:** All Flight Plan command outputs
**Breaking:** No - only improves formatting
**Benefit:** All outputs are readable, professional, and consistent
**Files Updated:** 
- `framework/FLIGHT-PLAN-COMMANDS.md` (global standards)
- `framework/FLIGHT-PLAN-INIT.md` (specific examples)

