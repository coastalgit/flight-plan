# NOT a CLI Tool Fix - 2025-10-27

## Problem

**Critical Issue:** Claude Code (and potentially other AI agents) was treating "flight-plan" as if it were a command-line tool that needs to be installed.

**Observed Behavior (Multiple Attempts):**
1. AI checked if "flight-plan" command is installed
2. AI searched for npm packages
3. AI tried to run shell commands
4. AI looked for executables with `which flight-plan`
5. **Even after warnings:** AI read README and executed `npx` commands!

**User Reports:**
> "This is getting worse! This time claude code checks to see if 'flight-plan' is installed, then goes to see if it an NPM package!"

> "FFS! This time it reads readme and wades straight in with an npx command! CLARIFY!"

**Root Cause - Multiple Issues:**
1. AI pattern-matches "flight-plan X" to CLI tools like `npm install`, `git commit`
2. README mentions npm/npx for OTHER tools (SpecKit), AI confuses this with Flight Plan itself
3. Warnings weren't strong enough or prominent enough
4. README was read first (informational) but AI treated it as execution instructions

**Root Cause:**
Instructions said things like:
```
"flight-plan init"         ← Preview project structure
"flight-plan init apply"   ← Generate projects
```

AI models interpreted "flight-plan" as a command-line tool name, similar to `npm`, `git`, `docker`, etc.

**Impact:**
- Wasted time checking for non-existent installations
- Confusion about how to proceed
- AI trying to install packages
- Execution blocked by failed prerequisite checks

## Solution

### Iteration 1: Initial Warnings (Not Strong Enough)
Added "CRITICAL: FOR AI AGENTS" sections explaining Flight Plan is not a CLI tool.
**Result:** AI still executed npx commands after reading README.

### Iteration 2: MAXIMUM STRENGTH Warnings (Still Had Issues)
Added **🚨 CRITICAL - STOP AND READ 🚨** sections with:
- Explicit "DO NOT EXECUTE SHELL COMMANDS" headline
- "ABSOLUTELY DO NOT" section
**Result:** Still too negative, didn't explain WHAT it IS clearly enough.

### Iteration 3: POSITIVE APPROACH (Final Solution)
User feedback: *"I think this is an incorrect approach - you tell it is NOT a CLI tool, but not WHAT IT IS! Also, devs might be running this tool from a CLI OR an IDE agent UI!"*

**Complete rewrite to focus on WHAT Flight Plan IS and HOW it works:**

**Key Changes:**
1. **Positive framing:** "Flight Plan IS" instead of "is NOT"
2. **Multi-interface support:** Explicitly mentions CLI, IDE chat, desktop apps
3. **Clear workflow:** User types → AI reads → AI performs
4. **Concrete examples:** Shows exact flow for commands
5. **Role clarity:** "Instructions for you (AI)" vs "program to install"
6. **Distinction clarity:** Flight Plan uses file ops, other tools (SpecKit) use shell

### 1. Updated `README.md` - Positive Approach

**Replaced negative warnings with positive explanation:**

```markdown
## 💡 FOR AI AGENTS: HOW FLIGHT PLAN WORKS

**Flight Plan is an AI-driven development methodology.**

### What Flight Plan IS:
Flight Plan is a **framework of markdown files** that provide step-by-step 
instructions for you (the AI agent) to follow. Whether the user is interacting 
with you through:
- A terminal/CLI (e.g., Cursor terminal, ChatGPT CLI)
- An IDE agent UI (e.g., Cursor chat, Claude Code chat)
- A chat interface (e.g., Claude Desktop, ChatGPT)

...the process is the same.

### How It Works:

**User types:** "flight-plan init"
**You (AI) do:**
1. Read framework/FLIGHT-PLAN-COMMANDS.md (understand what this command does)
2. Read framework/FLIGHT-PLAN-INIT.md (get detailed execution steps)
3. Use your **native file operations** to perform actions:
   - Read files (to understand requirements)
   - Create files (to generate project structure)
   - Write files (to create configurations)
4. **No shell commands** - you do the work directly

### The Key Concept:
- **"flight-plan X"** = Natural language instruction to you (the AI)
- **You interpret and execute** using your file read/write capabilities
- **No external program** to install or run
- **No shell commands** for Flight Plan itself

### What About npm/npx/uv Commands in This README?
- Those are for **other tools** like SpecKit (optional integration)
- **Not for Flight Plan itself**
- Flight Plan will tell you **when** to guide the user to install those
- You show the user the command, they run it in their terminal
```

**Key improvements:** 
- Explains WHAT it IS (AI-driven framework)
- Explains HOW it works (concrete example)
- Mentions ALL interfaces (CLI, IDE, desktop)
- Positive framing ("you do X" instead of "don't do Y")

### 2. Updated `framework/FLIGHT-PLAN-COMMANDS.md`

**Replaced warning section with:**

```markdown
## 🚨 CRITICAL: FOR AI AGENTS - STOP AND READ 🚨

**FLIGHT PLAN IS NOT A COMMAND LINE TOOL. DO NOT EXECUTE SHELL COMMANDS.**

### ABSOLUTELY DO NOT:
- ❌ Run ANY shell commands for Flight Plan
- ❌ Execute npm/npx/pip/cargo/uv commands for Flight Plan
- ❌ Check if "flight-plan" is installed
- ❌ Search for packages or executables

### What Flight Plan IS:
- ✅ **Markdown files** (this file!) that you read
- ✅ **Instructions for AI** to follow step-by-step
- ✅ **No executable** - uses your native file operations

### How "flight-plan" commands work:
User types: "flight-plan init"
↓
You (AI) read: framework/FLIGHT-PLAN-INIT.md
↓
You (AI) perform: File operations described
↓
NO shell commands executed

### If you see mentions of npm/npx/uv in this file:
- ⚠️ Those are for **OTHER tools** (like SpecKit)
- ⚠️ NOT for Flight Plan itself
```

**Key changes:** 
- "STOP AND READ" in header
- "ABSOLUTELY DO NOT" section
- Visual workflow diagram
- Distinction between Flight Plan and other tools

### 3. Updated `framework/FLIGHT-PLAN-INIT.md`

**Replaced warning section with:**

```markdown
## 🚨 CRITICAL: FOR AI AGENTS - STOP AND READ 🚨

**"flight-plan" is NOT a shell command. It is NOT an npm package. It is NOT installed.**

**What "flight-plan X" means:**
- These are **natural language phrases** that the user types to the AI
- The AI reads this markdown file and **performs the actions directly**
- No shell commands are executed
- No package managers are involved

**When user types "flight-plan init":**
1. ✅ AI reads this file (FLIGHT-PLAN-COMMANDS.md)
2. ✅ AI finds the flight-plan init section
3. ✅ AI reads framework/FLIGHT-PLAN-INIT.md for execution logic
4. ✅ AI performs the actions described

**DO NOT:**
- ❌ Check if "flight-plan" is installed
- ❌ Look for npm/pip/cargo packages
- ❌ Try to run "flight-plan" as a shell command
- ❌ Execute which flight-plan or similar
- ❌ Search for executables

**Flight Plan is:**
- ✅ A set of markdown files with instructions
- ✅ A methodology executed by AI
- ✅ Read by AI, performed by AI
- ✅ No installation required
```

**Also updated Overview section:**
```markdown
**Initial Setup (First Time):**
User types: "flight-plan init"
AI action: Show preview of project structure

User types: "flight-plan init apply"
AI action: Generate projects
```

Changed from just showing the commands to explicitly stating "User types" and "AI action" to clarify the flow.

### 2. Updated `framework/FLIGHT-PLAN-INIT.md`

**Added at top of file (after header, before Quick Reference):**

```markdown
## ⚠️ CRITICAL: FOR AI AGENTS

**"flight-plan" is NOT a shell command. It is NOT an npm package. It is NOT installed.**

**What "flight-plan init" means:**
- User types this phrase to the AI (you)
- You read this file and perform the actions described
- No shell commands are executed
- No package managers are involved

**DO NOT:**
- ❌ Check if "flight-plan" is installed
- ❌ Look for npm/pip/cargo packages
- ❌ Try to run "flight-plan" as a shell command
- ❌ Execute which flight-plan or similar
- ❌ Search for executables

**Flight Plan is:**
- ✅ A set of markdown files (this file!)
- ✅ Instructions for AI to follow
- ✅ No installation required

**When user says "flight-plan init":**
1. ✅ Read this file (FLIGHT-PLAN-INIT.md)
2. ✅ Follow the steps below (STEP 0, STEP 1, etc.)
3. ✅ Perform the actions using your file reading/writing capabilities
```

### 3. Updated `README.md`

**Updated AI AGENTS warning section:**

```markdown
## ⚠️ FOR AI AGENTS

**This README is INFORMATIONAL ONLY.**

**"flight-plan" is NOT a shell command or installed package:**
- ❌ DO NOT check if "flight-plan" is installed
- ❌ DO NOT look for npm/pip/cargo packages
- ❌ DO NOT execute shell commands
- ✅ These are natural language phrases for you (the AI)
- ✅ You read markdown files and perform actions directly
```

## Why This Happened

### AI Pattern Recognition

AI models are trained on:
- CLI tools: `npm install`, `git commit`, `docker run`
- Package managers: checking if tools are installed
- Shell commands: `which`, `command -v`, `npm list -g`

When AI sees:
```
"flight-plan init"
```

It pattern-matches to:
```
"npm install"     ← CLI tool
"git status"      ← CLI tool
"docker build"    ← CLI tool
```

And thinks: "I should check if this is installed!"

### How We Fixed It

**Explicit negatives:**
- ❌ NOT a shell command
- ❌ NOT an npm package
- ❌ NOT installed

**Explicit positives:**
- ✅ Natural language phrase
- ✅ Markdown file instructions
- ✅ AI reads and performs

**Clear workflow:**
1. User types phrase
2. AI reads file
3. AI performs action

## Result

### Before (WRONG):
```
User: "flight-plan init"

AI:
1. Checks if "flight-plan" is installed
2. Runs: which flight-plan
3. Searches npm: npm list -g | grep flight-plan
4. Error: "flight-plan not found, please install"
```

### After (CORRECT):
```
User: "flight-plan init"

AI:
1. Reads: framework/FLIGHT-PLAN-COMMANDS.md
2. Sees: "⚠️ CRITICAL: NOT a shell command"
3. Reads: framework/FLIGHT-PLAN-INIT.md
4. Performs: Actions described in the file
```

## Benefits

✅ **No False Prerequisites**
- AI doesn't check for installations
- No npm/pip/cargo searches
- No blocked execution

✅ **Clear Understanding**
- AI knows these are natural language phrases
- AI knows to read markdown files
- AI knows to perform actions directly

✅ **Consistent Behavior**
- Works same across Claude Desktop, Claude Code, ChatGPT, Gemini
- No model-specific installation checks
- Predictable execution flow

✅ **Faster Execution**
- No wasted time checking installations
- No confusion about prerequisites
- Direct action

## Files Updated

1. **`framework/FLIGHT-PLAN-COMMANDS.md`**
   - Added "CRITICAL: FOR AI AGENTS" section at top
   - Updated Overview to clarify "User types" vs "AI action"

2. **`framework/FLIGHT-PLAN-INIT.md`**
   - Added "CRITICAL: FOR AI AGENTS" section at top
   - Explains workflow: user types → AI reads → AI performs

3. **`README.md`**
   - Updated "FOR AI AGENTS" warning section
   - Added explicit "NOT a shell command" warnings

## Testing

### Test Case 1: User types "flight-plan init"
- ✅ AI should immediately read FLIGHT-PLAN-COMMANDS.md
- ✅ AI should NOT check if installed
- ✅ AI should proceed to read FLIGHT-PLAN-INIT.md

### Test Case 2: User types "flight-plan status"
- ✅ AI should read FLIGHT-PLAN-COMMANDS.md
- ✅ AI should NOT run shell commands
- ✅ AI should perform status check

### Test Case 3: Fresh AI session
- ✅ First command should work without checking installation
- ✅ AI should not search for packages
- ✅ AI should read markdown files directly

---

## Related: Output Formatting Fix (2025-10-28)

After fixing the "NOT a CLI tool" issue, discovered another critical problem:

**Issue:** CLI/IDE markdown renderers collapse blank lines
- Even when AI outputs perfectly formatted markdown
- Results in unreadable walls of text
- Took 6 attempts with Claude Code to diagnose

**Solution:** Always wrap formatted output in code blocks (triple backticks)
- Code blocks preserve blank lines and spacing
- Required for: previews, status reports, structured lists
- Not required for: normal conversation

**See:** `docs/OUTPUT-FORMATTING-FIX-2025-10-28.md`

This was added as "RULE #1" in all command execution files.

---

**Change Date:** 2025-10-27 (updated 2025-10-28)
**Impact:** ALL Flight Plan command execution
**Breaking:** No - only prevents incorrect behavior
**Benefit:** AI immediately executes commands without false prerequisite checks
**Critical:** YES - Without this, AI may not execute commands at all
**Files Updated:** 
- `framework/FLIGHT-PLAN-COMMANDS.md`
- `framework/FLIGHT-PLAN-INIT.md`
- `README.md`
- Added: `docs/OUTPUT-FORMATTING-FIX-2025-10-28.md` (2025-10-28)

