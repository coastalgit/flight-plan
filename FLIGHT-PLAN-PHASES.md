# Flight Plan Methodology - 8 Phases

**Version:** 1.0  
**Last Updated:** 2025-10-22

---

## Overview

The Flight Plan methodology structures AI-assisted development into 8 progressive phases. Each phase has specific objectives and transitions naturally to the next.

---

## Phase 1: Define the Mission

**Objective:** Establish WHY the project exists and WHAT success looks like.

**Activities:**
- Document problem statement
- Define user stories
- Establish success metrics
- Identify stakeholders
- Clarify scope (what's in, what's out)

**Outputs:**
- project-prd.md Section 2 (Requirements) completed
- Success criteria defined
- Open questions identified

**Move to Phase 2 when:** Problem and requirements are clear and documented.

---

## Phase 2: Build Context

**Objective:** Gather all information needed to design and build effectively.

**Activities:**
- Research similar solutions
- Collect API documentation
- Gather design assets
- Document technical constraints
- Review reference implementations
- Identify required tools and services

**Outputs:**
- docs/third-party/ populated with API specs
- docs/research/ contains background materials
- docs/snippets/ has relevant code examples
- project-prd.md Section 3 (Implementation) updated with research findings

**Move to Phase 3 when:** All necessary context and reference materials are gathered.

---

## Phase 3: Design System

**Objective:** Plan HOW to build before writing production code.

**Activities:**
- Design architecture (components, data flow, integrations)
- Plan database schema
- Define API contracts
- Sketch UI/UX wireframes
- Choose technology stack
- Design error handling approach
- Plan deployment strategy

**Outputs:**
- Architecture diagrams
- API specifications
- Database schema
- Technology decisions documented
- project-prd.md Section 3 (Implementation) fully detailed

**Move to Phase 4 when:** Design is complete and ready for validation.

---

## Phase 4: Validate Design

**Objective:** Verify the design works BEFORE implementing.

**Activities:**
- Review design with stakeholders
- Create proof-of-concept prototypes
- Test critical assumptions
- Validate API contracts
- Check performance feasibility
- Identify design flaws early

**Outputs:**
- Validated architecture
- Working prototypes
- Design revisions documented
- Green light to build
- Updated requirements or implementation if needed

**Move to Phase 5 when:** Design is validated and stakeholders approve.

---

## Phase 5: Build Code

**Objective:** Implement the validated design.

**Activities:**
- Write production code
- Implement features per requirements
- Follow architecture from Phase 3
- Write unit tests
- Handle errors gracefully
- Review and refactor code
- Document code decisions

**Outputs:**
- Working codebase
- Tests passing
- Features implemented
- Code reviewed
- decisions/ folder populated

**Move to Phase 6 when:** Core features are complete and tested.

---

## Phase 6: Automate & Integrate

**Objective:** Make the system production-ready through automation and integration.

**Activities:**
- Set up CI/CD pipeline
- Configure deployment automation
- Integrate with external services
- Set up monitoring and logging
- Configure alerts
- Automate testing
- Document deployment process

**Outputs:**
- Automated deployment pipeline
- Integrations working
- Monitoring in place
- Deployment documentation
- Production environment configured

**Move to Phase 7 when:** System is deployed and operational.

---

## Phase 7: Reflect & Capture

**Objective:** Learn from the project and capture knowledge for future use.

**Activities:**
- Document lessons learned
- Capture successful prompts/patterns
- Note what worked and what didn't
- Update project documentation
- Create troubleshooting guides
- Document gotchas and edge cases

**Outputs:**
- prompts/successful.md populated
- prompts/failed.md populated with lessons
- decisions/ documented
- Updated README with learnings
- Troubleshooting guide

**Move to Phase 8 when:** Knowledge is captured and documented.

---

## Phase 8: Meta Layer

**Objective:** Improve the development process itself based on experience.

**Activities:**
- Review entire project workflow
- Identify process improvements
- Update templates and patterns
- Refine AI prompting strategies
- Improve documentation approach
- Share learnings with team

**Outputs:**
- Process improvements documented
- Templates updated
- Best practices captured
- Team knowledge shared
- Meta-learnings for future projects

**Project Complete:** System is built, deployed, documented, and learnings captured.

---

## Phase Transitions

**How to know when to move forward:**
- ✅ Phase objectives achieved
- ✅ Key outputs created
- ✅ No blocking issues
- ✅ Stakeholder approval (if needed)

**It's OK to:**
- Move back to previous phases if needed
- Iterate within a phase
- Skip phases that don't apply
- Customize phase activities for your project

---

## Tracking Progress

Update `.flight-plan/current.md` as you progress:
- Mark current phase
- Check off completed tasks
- Note blockers
- Document phase transitions

Use `flight-plan status` command to see progress across all projects.

---

## Example Phase Flow

```
Portfolio Website Project:

Week 1:
Day 1-2: Phase 1 (Define Mission)
         → project-prd.md Section 2 completed
         
Day 3:   Phase 2 (Build Context)
         → Gathered design inspiration, API docs
         
Day 4:   Phase 3 (Design System)
         → Sketched layout, chose tech stack
         
Day 4:   Phase 4 (Validate Design)
         → Quick prototype, got feedback
         
Day 5-6: Phase 5 (Build Code)
         → Implemented HTML/CSS/JS
         
Day 6:   Phase 6 (Automate & Integrate)
         → Set up Netlify deployment
         
Day 7:   Phase 7 (Reflect & Capture)
         → Documented learnings, successful approaches
         
Day 7:   Phase 8 (Meta Layer)
         → Updated workflow for next project
```

---

**Flight Plan Phases Version:** 1.0  
**Last Updated:** 2025-10-22

*These phases provide structure without being rigid. Adapt to your needs.*
