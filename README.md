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
├── BRIEF-BUILDER-v1.0.md                  # Formats brainstorm into PRD
├── GENERATOR.md                            # Generates project structure
├── FLIGHT-PLAN-COMMANDS.md                # Ongoing operations
├── templates/                              # Templates for generation
│   ├── flight-plan-current.md.template
│   ├── flight-plan-requirements.md.template
│   ├── flight-plan-implementation.md.template
│   └── cursor-rule.mdc.template
└── examples/
    ├── portfolio-site-brainstorm.md       # Raw brainstorm example
    └── portfolio-site-prd-v1.md           # Generated PRD example
```

---

## 📁 Complete Directory Structure

**After cloning and initial generation:**

```
your-workspace/
└── flight-plan-solution/              # Cloned from GitHub
    ├── README.md                      # This file
    ├── BRIEF-BUILDER-v1.0.md         # Format brainstorm → PRD
    ├── GENERATOR.md                   # Initial project generation
    ├── FLIGHT-PLAN-COMMANDS.md       # Ongoing operations
    │
    ├── templates/                     # Templates for generation
    │   ├── flight-plan-current.md.template
    │   ├── flight-plan-requirements.md.template
    │   ├── flight-plan-implementation.md.template
    │   └── cursor-rule.mdc.template
    │
    ├── examples/                      # Reference examples
    │   └── portfolio-site-prd-v1.md
    │
    ├── solution-prd-v1.md            # YOUR formatted PRD (you add this)
    │
    ├── ai-refs/                       # Cross-project coordination (generated)
    │   ├── solution-overview.md       # All projects status
    │   ├── notes.md                   # Your personal notes
    │   └── cursor.md                  # AI working memory
    │
    └── [generated projects]/          # Your projects (generated)
        │
        ├── project-a/
        │   ├── .flight-plan/
        │   │   ├── current.md         # Progress tracking
        │   │   ├── requirements.md    # WHAT to build
        │   │   ├── implementation.md  # HOW to build
        │   │   ├── history/
        │   │   ├── prompts/
        │   │   └── decisions/
        │   ├── docs/
        │   │   ├── third-party/
        │   │   ├── snippets/
        │   │   ├── research/
        │   │   └── logs/
        │   ├── .cursor/
        │   │   └── rules/
        │   │       └── flight-plan.mdc
        │   ├── src/                   # Your code
        │   └── README.md
        │
        └── project-b/
            └── [same structure]
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

Give any AI your brainstorm notes + `BRIEF-BUILDER-v1.0.md`:

```
Use BRIEF-BUILDER-v1.0.md to format my brainstorm.

Here are my notes:
[paste your brainstorm]
```

AI generates: `solution-prd-v1.md` (structured, versioned, with TODOs)

### 3. Generate Project Structure

Drop `solution-prd-v1.md` into this directory, then:

**In Cursor:**
```
Open flight-plan-solution/ in Cursor
Say: "Read GENERATOR.md and create the project structure"
```

**Or CLI:**
```
cd flight-plan-solution/
[Run your preferred AI CLI tool]
"Read GENERATOR.md and generate structure"
```

AI will:
- Ask clarifying questions if needed
- Generate all project directories
- Create `.flight-plan/` tracking in each
- Set up Cursor integration
- Create AI reference files

**Done!** Start building.

---

## 📋 What Gets Generated

For each project in your PRD:

```
your-project/
├── .flight-plan/               # Progress tracking
│   ├── current.md              # Current phase & tasks
│   ├── requirements.md         # WHAT to build (tech-agnostic)
│   ├── implementation.md       # HOW to build (tech-specific)
│   ├── history/                # Milestones
│   ├── prompts/                # What worked
│   └── decisions/              # Key choices
├── docs/                       # Reference materials
│   ├── third-party/            # External specs
│   ├── snippets/               # Code examples
│   └── research/               # Background
├── .cursor/
│   └── rules/
│       └── flight-plan.mdc     # IDE integration
└── src/                        # Your code
```

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

## 🔌 Cursor Integration

The `.cursor/rules/flight-plan.mdc` file automatically integrates Flight Plan with Cursor IDE:

**What it does:**
1. Auto-reads PRD on first interaction in each project
2. Loads current phase and tasks from `.flight-plan/current.md`
3. Checks requirements and implementation files
4. Updates progress as you work

**Setup:** None needed! Just open the project in Cursor.

The `alwaysApply: true` setting means Cursor automatically uses these rules without you needing to do anything. It will know about your PRD, current phase, blockers, and requirements every time you work in the project.

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
