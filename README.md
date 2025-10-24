# Flight Plan Solution System

**Version:** 1.0  
**Status:** Production Ready

A structured approach for initializing multi-project solutions using AI assistance and the Flight Plan methodology.

---

## 🎯 What This Is

A 3-phase workflow for taking messy brainstorms and turning them into organized, AI-ready project structures.

**Phase 1:** Brainstorm freely (any AI, any tool)  
**Phase 2:** Format with Brief Builder → `solution-prd-v1.md`  
**Phase 3:** Generate structure with Generator → Complete project setup

---

## 📦 What's Included

```
flight-plan-solution/
├── README.md                               # This file
├── BRIEF-BUILDER-v1.1.md                  # Formats brainstorm into PRD
├── GENERATOR.md                            # Generates project structure
├── FLIGHT-PLAN-INIT.md                    # Initial setup command
├── FLIGHT-PLAN-COMMANDS.md                # Ongoing operations
├── FLIGHT-PLAN-PHASES.md                  # 8 phases explained
├── templates/                              # Templates for generation
│   ├── project-prd.md.template
│   ├── solution-rules.md.template
│   ├── project-rules.md.template
│   ├── constitution.md.template (Spec-Kit)
│   ├── flight-plan-current.md.template
│   └── cursor-rule.mdc.template
└── examples/
    ├── portfolio-site-brainstorm.md       # Raw brainstorm example
    └── portfolio-site-prd-v1.md           # Generated PRD example
```

---

## 📁 Complete Directory Structure

**Your intended workflow:**

```
MyApp/                                  # Your solution root directory
│
├── flight-plan-solution/               # Flight Plan tooling (copy from repo)
│   ├── README.md                       # This file
│   ├── BRIEF-BUILDER-v1.1.md          # Format brainstorm → PRD
│   ├── GENERATOR.md                    # Initial project generation
│   ├── FLIGHT-PLAN-COMMANDS.md        # Ongoing operations
│   ├── FLIGHT-PLAN-PHASES.md          # 8 phases explained
│   │
│   ├── templates/                      # Templates for generation
│   │   ├── flight-plan-current.md.template
│   │   ├── flight-plan-requirements.md.template
│   │   ├── flight-plan-implementation.md.template
│   │   └── cursor-rule.mdc.template
│   │
│   ├── examples/                       # Reference examples
│   │   ├── portfolio-site-brainstorm.md
│   │   └── portfolio-site-prd-v1.md
│   │
│   ├── solution-prd-v1.md             # YOUR formatted PRD (you add this)
│   │
│   └── ai-refs/                        # Cross-project coordination (generated)
│       ├── solution-overview.md        # All projects status
│       ├── notes.md                    # Your personal notes
│       └── cursor.md                   # AI working memory
│
├── project-a/                          # Generated project (sibling to flight-plan-solution)
│   ├── project-prd.md                  # What to build (single source of truth)
│   ├── project-rules.md                # How AI should work (inherits from solution-rules)
│   ├── .flight-plan/
│   │   ├── current.md                  # Progress tracking (phase, status)
│   │   └── history/
│   ├── .cursor/
│   │   └── rules/
│   │       └── flight-plan.mdc         # Points to project-rules.md
│   ├── CLAUDE.md (optional)            # Points to project-rules.md
│   ├── docs/                           # Reference materials
│   ├── src/                            # Your code
│   └── README.md
│
└── project-b/                          # Another generated project
    └── [same structure as project-a]
```

---

## 🚀 Quick Start

### 1. Brainstorm Your Solution

Use any AI (Claude, Gemini, ChatGPT, etc.) to explore ideas freely:
- What problem you're solving
- Project components needed
- Technology choices
- Architecture ideas

**No structure required** - just capture your thinking.

### 2. Format into PRD

Give any AI your brainstorm notes + `BRIEF-BUILDER-v1.1.md`:

```
Use BRIEF-BUILDER-v1.1.md to format my brainstorm.

Here are my notes:
[paste your brainstorm]
```

AI generates: `solution-prd-v1.md` (structured, versioned, with TODOs)

**💡 Tip: Rich PRDs for Better AI Handover**

Brief Builder v1.1 includes **Section 11: Design Rationale & Context**, which is critical for IDE AI handover.

**Why it matters:**
- ✅ AI understands WHY decisions were made (not just WHAT)
- ✅ AI knows alternatives considered and rejected
- ✅ AI has context from your conversation
- ✅ No "cold start" when AI opens your project

**During brainstorming, capture:**
- "We chose X **because** [reason]"
- "Considered Y but rejected due to [concern]"
- "Realized [insight] which changed our approach"
- Technical concepts that needed explanation

**Example:**
```markdown
## 11. DESIGN RATIONALE & CONTEXT

### Decision Context
- Chose PostgreSQL because ACID transactions critical for payments
- React selected for team expertise (3 devs, 2+ years experience)

### Alternatives Considered
- MongoDB: Rejected due to financial data requirements
- Vue: Training time would delay launch

### Key Insights
- Realized admin tools need separate UI after discussing user types
- Payments are core feature → drove database decision early
```

**Result:** IDE AI reads Section 11 and immediately understands your reasoning.

### 3. Set Up Your Solution Directory

```bash
# Create your solution root directory
mkdir MyApp
cd MyApp

# Copy Flight Plan tooling into a subdirectory
cp -r /path/to/flight-plan-repo/ ./flight-plan-solution/

# Copy your PRD into the flight-plan-solution directory
cp solution-prd-v1.md ./flight-plan-solution/
```

### 4. Generate Project Structure

**In Cursor (or any AI):**
```
Open MyApp/flight-plan-solution/ in Cursor

# Step 1: Preview (see what will be created)
"flight-plan init"

AI: [Shows detailed preview with tech stack, projects, dependencies, etc.]

# Step 2: Ask questions (optional)
"What database is the backend using?"

AI: "PostgreSQL from PRD Section 3..."

# Step 3: Apply (actually create the projects)
"flight-plan init apply"

AI: ✅ Generated 3 projects in ../
    ✅ Created solution-rules.md
    ✅ Created ai-refs/
```

**Two-step pattern:**
1. **Preview** - `flight-plan init` shows what will be created (read-only)
2. **Apply** - `flight-plan init apply` actually creates projects (writes files)

**Why two steps?**
- Preview is safe - explore without commitment
- Discuss and refine before generating
- Clear separation between planning and execution

**Done!** Your projects are now in `MyApp/` alongside `flight-plan-solution/`.

**Next steps:**
```bash
cd MyApp/backend-api/
"flight-plan status"
```

The AI will guide you through:
- Reviewing project requirements
- Resolving open questions  
- Test results (automatically shown in Phase 5-6)
- Optional SpecKit setup (for feature-level development)
- Moving through Flight Plan phases naturally

---

## 📋 What Gets Generated

For each project in your PRD (created in parent directory `MyApp/`):

```
MyApp/your-project/
├── project-prd.md              # What to build (single source of truth)
├── project-rules.md            # How AI should work (inherits solution-rules.md)
├── .flight-plan/
│   ├── current.md              # Current phase, status, activity
│   ├── config.json             # Project configuration (SpecKit, etc.)
│   └── history/                # Milestones
├── .cursor/rules/
│   └── flight-plan.mdc         # Points to project-rules.md
├── memory/                     # SpecKit constitution (if enabled)
│   └── constitution.md         # References Flight Plan files
├── specs/                      # SpecKit feature specs (if enabled)
├── docs/                       # Reference materials
└── src/                        # Your code
```

**Structure highlights:**
- `project-prd.md` - Single source of truth (Git tracks history)
- `project-rules.md` - AI integration layer (inherits from solution)
- `.flight-plan/current.md` - Light status tracking (phase, blockers)
- `.flight-plan/config.json` - Project configuration (decisions remembered)
- `memory/constitution.md` - SpecKit integration (optional, references Flight Plan)
- Minimal AI pointer files (just reference project-rules.md)

---

## 🎨 AI-Agnostic Design

**Works with any AI:**
- Claude Desktop
- ChatGPT
- Gemini  
- Cursor
- Claude Code (CLI)
- Any tool that reads markdown

**No vendor lock-in.** Just markdown files and instructions.

---

## 🔌 AI Integration

**Works with any AI IDE/tool:**

Each project gets a `project-rules.md` that tells the AI:
- Where to find project specs (project-prd.md)
- What solution-wide tools are available (solution-rules.md)
- Current phase and status (.flight-plan/current.md)
- How to work in this project

**AI-specific pointer files:**
- `.cursor/rules/flight-plan.mdc` → points to project-rules.md (auto-loaded)
- `CLAUDE.md` → points to project-rules.md (for Claude Desktop)
- `GEMINI.md` → points to project-rules.md (for Gemini)

**Setup:** None needed! Just open the project. AI auto-reads project-rules.md.

The AI will:
1. Check your current phase
2. Apply phase-appropriate standards
3. Use available tools (MCP servers from solution-rules.md)
4. Guide you through Flight Plan methodology naturally

---

## 🎯 Optional: Spec-Kit Integration

### What is Spec-Kit?

**[Spec-Kit](https://github.com/github/spec-kit)** is GitHub's toolkit for **Spec-Driven Development** - a structured approach to building individual features with AI agents.

**Key Differences:**
- **Flight Plan** → Project-level methodology (8 phases, PRDs, overall progress)
- **Spec-Kit** → Feature-level framework (individual feature specs, tasks, implementation)

**How They Work Together:**
```
Flight Plan: "We're in Phase 5, building the backend API"
              ↓ (provides context)
Spec-Kit:    "Let's implement user authentication feature"
              → /speckit.spec, /speckit.plan, /speckit.implement
```

**Spec-Kit Components:**
- `memory/constitution.md` - Project rules (references Flight Plan files)
- `specs/` - Feature specifications
- Commands: `/speckit.spec`, `/speckit.plan`, `/speckit.tasks`, `/speckit.implement`

### Enable Spec-Kit in Your Project

**When working in a project:**
Navigate to your project:
```
cd MyApp/your-project/
"flight-plan status"
```

AI will show your status and ask if you want SpecKit. If yes, it guides you through setup.

**What this does:**
1. ✅ Fetches **actual SpecKit README** from `github.com/github/spec-kit`
   - Installation instructions are at the TOP of the README
   - Shows you the REAL, current installation commands (uses `uv`/`uvx`)
2. 🔧 Checks prerequisites:
   - Asks you to check: `uv --version`
   - Provides uv install/upgrade commands if needed
3. 📖 Displays actual installation steps from SpecKit README (not assumed!)
4. ⏸️ Waits for you to install SpecKit
5. 🔧 Then creates `memory/constitution.md` with Flight Plan references:
   - PRD location: `project-prd.md`
   - Current phase: `.flight-plan/current.md`
   - Phase standards: `../flight-plan-solution/FLIGHT-PLAN-PHASES.md`
6. 📁 Creates `specs/` directory for feature specs
7. 💾 Saves decision to `.flight-plan/config.json`

Spec-Kit will automatically:
- Check your current Flight Plan phase
- Apply phase-appropriate standards
- Reference project-prd.md for constraints
- Use quality gates from your PRD
- Always work with latest SpecKit practices

**Learn more:** [github.com/github/spec-kit](https://github.com/github/spec-kit)

**No Spec-Kit?** No problem. Flight Plan works standalone.

---

## 📖 Examples

See `examples/` directory for:
- `portfolio-site-brainstorm.md` - Raw brainstorm notes
- `portfolio-site-prd-v1.md` - Complete PRD after Brief Builder formatting

Shows the complete flow: messy notes → structured PRD → ready for generation

---

## 🔄 Versioning

**PRDs are immutable:**
- First run: `solution-prd-v1.md`
- Re-run later: `solution-prd-v2.md`  
- Keeps full history

**Brief Builder versions:**
- Current: v1.0
- Future: v1.1 (minor improvements), v2.0 (major changes)

---

## 🤝 Contributing

This is part of the Flight Plan methodology. 

**To contribute:**
1. Fork the repository
2. Make improvements
3. Test with real projects
4. Submit PR with examples

**Versioning for contributions:**
- Bug fixes/clarifications: Increment minor (v1.0 → v1.1)
- New features/sections: Increment minor (v1.1 → v1.2)
- Breaking changes: Increment major (v1.x → v2.0)

---

## 📚 Flight Plan Methodology

This solution initializer is part of the broader Flight Plan methodology for AI-assisted development.

**8 Phases:**
1. Define the Mission
2. Build Context  
3. Design System
4. Validate Design
5. Build Code
6. Automate & Integrate
7. Reflect & Capture
8. Meta Layer

**Learn more:** [Link to your canonical Flight Plan repo]

---

## 💡 Use Cases

**Perfect for:**
- Multi-project solutions (frontend + backend + functions)
- Team collaboration with AI tools
- Personal projects with structure
- R&D experimentation that becomes production

**Workshop-ready:**
- Teach teams how to use AI effectively
- Demonstrate structured development
- Show AI tool integration

---

## 🐛 Troubleshooting

**Brief Builder adds details I didn't mention:**
- Check the "DO NOT INVENT" banner is being respected
- Verify AI is extracting, not creating
- File an issue with the specific example

**Generator asks too many questions:**
- Your PRD might be too sparse
- Add more detail in Phase 2 (formatting)
- Or answer questions - that's expected for minimal PRDs

**Generated structure doesn't match needs:**
- Modify templates/ directory
- Adjust GENERATOR.md instructions
- Submit PR with improvements

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

Free to use, modify, and distribute. Attribution appreciated but not required.

---

## 🙏 Credits

Created as part of the Flight Plan methodology for structured AI-assisted development.

**Version:** 1.0  
**Last Updated:** 2025-10-22

---

**Ready to start?** Run through Quick Start above, or check `examples/` for a reference implementation.
