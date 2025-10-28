# SpecKit Setup Instructions for AI

**Version:** 2.0  
**Last Updated:** 2025-10-24  
**Source:** https://raw.githubusercontent.com/github/spec-kit/refs/heads/main/README.md

---

## ⚠️ CRITICAL: THESE ARE THE OFFICIAL SPECKIT INSTRUCTIONS

**This document contains the ACTUAL SpecKit installation steps from the official README.**

Any AI executing `flight-plan setup-speckit` MUST:
1. Read this ENTIRE file before proceeding
2. Use ONLY the instructions from the SpecKit README (included below)
3. NEVER assume, invent, or modify these instructions
4. If unsure, fetch latest from: https://raw.githubusercontent.com/github/spec-kit/refs/heads/main/README.md

---

## Official SpecKit Installation (from README)

**Source:** https://github.com/github/spec-kit/blob/main/README.md

### Step 1: Install `uv` (Prerequisite)

SpecKit requires `uv` (Python package installer).

**Check if installed:**
```bash
uv --version
```

**If not installed:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**To upgrade existing installation:**
```bash
uv self update
```

---

### Step 2: Install SpecKit

SpecKit provides TWO installation options:

#### Option 1: Persistent Installation (Recommended)

Install once and use everywhere:

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

Then use the tool directly:
```bash
specify init
specify check
```

**To upgrade SpecKit:**
```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

**Benefits of persistent installation:**
- Tool stays installed and available in PATH
- No need to create shell aliases
- Better tool management with `uv tool list`, `uv tool upgrade`, `uv tool uninstall`
- Cleaner shell configuration

#### Option 2: One-time Usage

Run directly without installing:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

---

### Step 3: Initialize SpecKit in Your Project

From your project directory:

```bash
specify init <PROJECT_NAME> --ai <agent>
```

**Supported AI agents:**
- `claude` - Claude Code
- `copilot` - GitHub Copilot
- `cursor-agent` - Cursor
- `gemini` - Gemini CLI
- `windsurf` - Windsurf
- `qwen` - Qwen Code
- `opencode` - opencode
- `codex` - Codex CLI
- `kilocode` - Kilo Code
- `auggie` - Auggie CLI
- `codebuddy` - CodeBuddy CLI
- `roo` - Roo Code
- `amp` - Amp

**Additional options:**
- `--here` - Initialize in current directory
- `--force` - Force merge/overwrite in current directory
- `--no-git` - Skip git repository initialization
- `--script ps` - Use PowerShell scripts (Windows)

**Example:**
```bash
# Initialize with Cursor
specify init my-project --ai cursor-agent

# Initialize in current directory
specify init . --ai claude

# Or use --here flag
specify init --here --ai copilot
```

---

### What SpecKit Creates

When you run `specify init`, SpecKit automatically creates:

```
project/
├── CLAUDE.md                  # (or agent-specific file)
├── memory/
│   └── constitution.md        # Project principles
├── scripts/
│   ├── check-prerequisites.sh
│   ├── common.sh
│   ├── create-new-feature.sh
│   ├── setup-plan.sh
│   └── update-claude-md.sh
├── specs/                     # Feature specifications
│   └── 001-feature-name/
│       ├── contracts/
│       ├── data-model.md
│       ├── plan.md
│       ├── quickstart.md
│       ├── research.md
│       └── spec.md
└── templates/
    ├── CLAUDE-template.md
    ├── plan-template.md
    ├── spec-template.md
    └── tasks-template.md
```

**⚠️ IMPORTANT: Flight Plan does NOT create these directories!**
- SpecKit creates `memory/` and `memory/constitution.md`
- SpecKit creates `specs/` directory structure
- SpecKit creates `scripts/` and `templates/`
- Flight Plan only needs to REFERENCE these files

---

### Step 4: SpecKit Commands

After initialization, use these commands in your AI assistant:

```bash
/speckit.constitution  # Create project principles
/speckit.specify       # Create feature specification
/speckit.plan          # Create implementation plan
/speckit.tasks         # Break down into tasks
/speckit.implement     # Execute implementation
```

---

## Flight Plan Integration

Flight Plan's role is to:
1. Guide user through SpecKit installation
2. Let SpecKit create its own structure
3. Reference SpecKit's files from Flight Plan context

**Flight Plan does NOT:**
- ❌ Create `memory/` directory
- ❌ Create `specs/` directory
- ❌ Create `constitution.md` file
- ❌ Modify SpecKit's structure

**Flight Plan DOES:**
- ✅ Check if user has `uv` installed
- ✅ Show SpecKit installation commands
- ✅ Guide user to run `specify init`
- ✅ Update `.flight-plan/config.json` to track SpecKit status
- ✅ Reference SpecKit's files in documentation

---

## How to Execute `flight-plan setup-speckit`

### STEP 1: Check Prerequisites

**Verify context:**
1. ✅ In a project directory (has `project-prd.md`)
2. ✅ `.flight-plan/` directory exists

**Check if SpecKit already initialized:**
```bash
# Check for SpecKit directories
ls memory/ specs/ 2>/dev/null
```

If SpecKit already configured:
```
⚠️  SpecKit already initialized

Found: memory/constitution.md and specs/ directory

SpecKit is already set up in this project.
To reinitialize, remove the SpecKit directories first or use `specify init --force --here`
```
**STOP. Do not proceed.**

---

### STEP 2: Check `uv` Installation

**Ask user to check if `uv` is installed:**

```bash
uv --version
```

**If not installed, provide instructions:**
```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**If installed but outdated:**
```bash
# Upgrade uv
uv self update
```

---

### STEP 3: Present Official SpecKit Installation to User

**Show user the actual SpecKit installation from the official README:**

```
📖 SpecKit Setup for [project-name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🌱 WHAT IS SPECKIT?

Build high-quality software faster with Spec-Driven Development.
An open source toolkit that allows you to focus on product scenarios 
and predictable outcomes instead of vibe coding.

Source: https://github.com/github/spec-kit

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📥 INSTALLATION STEPS:

Step 1: Install SpecKit (Recommended Method)

   uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

Step 2: Initialize SpecKit in this project

   cd [project-directory]
   specify init . --ai [your-ai-agent] --here

   Available AI agents:
   - claude          (Claude Code)
   - copilot         (GitHub Copilot)
   - cursor-agent    (Cursor)
   - gemini          (Gemini CLI)
   - windsurf        (Windsurf)
   - and more (see Step 3 in Official Installation section above)

   Example:
   specify init . --ai cursor-agent --here

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📚 TO UPGRADE SPECKIT LATER:

   uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 YOUR PROJECT CONTEXT (from Flight Plan):

Project: [project-name]
Current Phase: Phase [N] from .flight-plan/current.md
Tech Stack: [from project-prd.md]
Quality Standards: [from project-prd.md]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

What SpecKit will create:
- memory/constitution.md (project principles)
- specs/ (feature specifications)
- scripts/ (helper scripts)
- templates/ (spec templates)

You can reference Flight Plan files from your SpecKit constitution:
- ../project-prd.md (requirements & tech stack)
- ../.flight-plan/current.md (current phase)
- ../../flight-plan-solution/FLIGHT-PLAN-PHASES.md (phase standards)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Please run the installation commands above.
Type 'done' when SpecKit is initialized.
```

**Wait for user to complete installation.**

---

### STEP 4: Update Flight Plan Tracking

**After user confirms SpecKit is installed:**

1. **Verify SpecKit created its structure:**
   ```bash
   ls memory/constitution.md specs/ 2>/dev/null
   ```
   
   If files exist, proceed. If not, user needs to run `specify init` again.

2. **Update `.flight-plan/config.json`:**
   ```json
   {
     "project_name": "[project-name]",
     "generated_date": "[existing]",
     "prd_version": "[existing]",
     "speckit_enabled": true,
     "speckit_prompted": true,
     "speckit_initialized_date": "[current date YYYY-MM-DD HH:MM UTC]",
     "current_phase": [existing],
     "last_updated": "[current date]"
   }
   ```

3. **Show completion:**
   ```
   ✅ SpecKit initialized successfully!
   
   Flight Plan tracking updated in .flight-plan/config.json

   🚀 NEXT STEPS:

   1. Use SpecKit commands in your AI assistant:
      /speckit.constitution  # Define project principles
      /speckit.specify       # Create feature spec
      /speckit.plan          # Create implementation plan
      /speckit.tasks         # Break down tasks
      /speckit.implement     # Execute implementation

   2. Reference Flight Plan files in your SpecKit constitution:
      - project-prd.md (your requirements & tech stack)
      - .flight-plan/current.md (your current phase)
      - ../flight-plan-solution/FLIGHT-PLAN-PHASES.md (phase standards)

   3. SpecKit will respect your Flight Plan context automatically.

   📚 Resources:
      - SpecKit docs: https://github.com/github/spec-kit
      - Flight Plan phases: ../flight-plan-solution/FLIGHT-PLAN-PHASES.md
      - Your PRD: project-prd.md
   ```

---

## How SpecKit Uses Flight Plan Context

SpecKit creates its own `memory/constitution.md` during `specify init`. 

**After SpecKit is initialized, user can EDIT constitution.md to reference Flight Plan files:**

```markdown
## Flight Plan Integration

**Project Context:**
- ../project-prd.md - Requirements, tech stack, quality standards
- ../.flight-plan/current.md - Current phase, status, blockers  
- ../project-rules.md - Project rules, MCP servers, standards

**Phase Standards:**
- ../../flight-plan-solution/FLIGHT-PLAN-PHASES.md - Standards per phase (1-8)

Use these files to:
- Apply phase-appropriate standards during /speckit.spec
- Respect quality gates from PRD during /speckit.implement
- Reference tech stack constraints during /speckit.plan
```

**Note:** SpecKit creates constitution.md with its own template. Users can manually add Flight Plan references if desired.

---

## Error Handling

### If Web Fetch Fails

```
❌ Could not fetch SpecKit documentation

Tried: github.com/github/spec-kit
Error: [error message]

Please:
1. Check internet connection
2. Visit github.com/github/spec-kit manually
3. Follow installation instructions there
4. Then run: "flight-plan setup-speckit" again

Or provide installation instructions, and I'll configure Flight Plan integration.
```

### If SpecKit Already Installed

If user says "already installed":

```
✅ SpecKit already installed

Proceeding with Flight Plan integration only...

[Continue with STEP 5]
```

### If User Declines After Seeing Instructions

```
📝 Noted: SpecKit setup cancelled

You can run "flight-plan setup-speckit" anytime to try again.

Updated .flight-plan/config.json:
  "speckit_enabled": false,
  "speckit_prompted": true
```

---

## Testing Checklist

Before recommending to user, verify:

- [ ] Fetched actual SpecKit README
- [ ] Extracted installation commands (from top of README)
- [ ] Showed actual commands (not invented)
- [ ] Provided Flight Plan context
- [ ] Created constitution.md with correct file paths
- [ ] Created specs/ directory
- [ ] Updated config.json
- [ ] Showed next steps

---

## Important Notes

**DO:**
- ✅ Read this ENTIRE file first (contains official SpecKit README info)
- ✅ Use ACTUAL installation commands from official SpecKit README
- ✅ Let SpecKit create its own directories (memory/, specs/, etc.)
- ✅ Only update Flight Plan's config.json
- ✅ Wait for user to complete installation

**DON'T:**
- ❌ Assume or invent installation steps
- ❌ Create memory/ or specs/ directories (SpecKit does this)
- ❌ Create constitution.md file (SpecKit does this)
- ❌ Modify SpecKit's structure
- ❌ Proceed without user confirmation
- ❌ Use old/cached instructions

---

## Version Tracking

When you fetch SpecKit instructions:

```json
{
  "speckit_version": "fetched-2025-10-24",
  "speckit_readme_url": "https://github.com/github/spec-kit",
  "last_fetched": "2025-10-24 16:30 UTC"
}
```

This helps track which version of instructions was used.

---

## Summary

**The flow is:**
1. User wants SpecKit
2. Fetch github.com/github/spec-kit README (installation at top)
3. Show actual installation commands to user
4. User installs SpecKit
5. We configure Flight Plan integration (constitution.md with file refs)
6. Done!

**Key principle:** FETCH REAL INSTRUCTIONS, DON'T ASSUME.

---

**Version:** 1.0  
**Compatible with:** Flight Plan v1.0, SpecKit (latest)  
**Last Updated:** 2025-10-24

