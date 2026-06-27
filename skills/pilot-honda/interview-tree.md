# pilot-honda — Adaptive Interview Tree

Instructions for Mode A (new idea). This is a decision-tree interview, not a flat survey. Each question branches based on the user's answer. Maximum 5-7 questions to reach a recommendation.

---

## Entry Point

Ask the user:

> **"Tell me about what you want to build. Are you starting from scratch, or do you have some work done already?"**

Options:
- **A) I have a raw idea — haven't written anything down yet** → Go to Q1
- **B) I have some notes or a rough concept** → Go to Q2
- **C) I have a detailed plan/PRD but haven't started coding** → Go to Q4
- **D) I have code but need to organize my process** → Switch to Mode B (Auto-Scan)

---

## Q1: Raw Idea (Phase 0 — SPARK)

> **"How clear is your idea right now?"**

Options:
- **A) Very vague — I know the problem but not the solution** → Recommend:
  - Start with BMad `bmad-brainstorming` to explore the space
  - Then use BMad `bmad-forge-idea` to pressure-test and harden it
  - Commands: `"Run bmad-brainstorming"` then `"Run bmad-forge-idea"`
  - **STOP. Generate mini-report and save state.**

- **B) I know roughly what I want to build** → Go to Q2

- **C) I know exactly what I want — I just need to write it down** → Go to Q3

---

## Q2: Rough Concept (Phase 0-1 — SPARK to ANALYSIS)

> **"Do you want to validate this idea rigorously, or are you confident and want to move fast?"**

Options:
- **A) Let me validate it first — challenge my assumptions** → Recommend:
  - Use BMad `bmad-prfaq` for the Working Backwards PRFAQ gauntlet
  - This will stress-test your concept before you invest time
  - Command: `"Run bmad-prfaq"`
  - Alternative: GStack `/office-hours` for a faster reframing session
  - **STOP. Generate mini-report and save state.**

- **B) I'm confident — just help me document it properly** → Recommend:
  - Use BMad `bmad-product-brief` for a gentle, guided product brief
  - Command: `"Run bmad-product-brief"`
  - **STOP. Generate mini-report and save state.**

- **C) I want a quick reframe before diving in** → Recommend:
  - Use GStack `/office-hours` — six forcing questions to reframe the product
  - Command: `"Run /office-hours"`
  - **STOP. Generate mini-report and save state.**

---

## Q3: Clear Idea, Needs Documentation (Phase 1 — ANALYSIS)

> **"What kind of documentation do you need?"**

Options:
- **A) A product brief (what am I building and why)** → Recommend:
  - BMad `bmad-product-brief` or `bmad-prfaq`
  - Command: `"Run bmad-product-brief"`

- **B) Full requirements (PRD with features, constraints, success metrics)** → Go to Q4

- **C) A technical spec (how will it work)** → Recommend:
  - BMad `bmad-spec` for a lean spec, or go straight to GStack `/spec`
  - Command: `"Run bmad-spec"` or `"Run /spec"`

---

## Q4: Has Requirements/Plan (Phase 2-3 — PLANNING to SOLUTIONING)

> **"Do you have a technical architecture plan (how the system is structured, what technologies, data flows)?"**

Options:
- **A) No architecture yet** → Recommend:
  - Use BMad's Architect agent (Winston) via `bmad-architecture`
  - This will create the system spine: invariants, data flows, technology choices
  - Command: `"Run bmad-architecture"`
  - After that: `"Run bmad-create-epics-and-stories"` to break it into implementable pieces
  - Then validate with: `"Run bmad-check-implementation-readiness"`
  - **STOP. Generate mini-report and save state.**

- **B) Yes, I have an architecture plan** → Go to Q5

- **C) I have a rough idea but nothing formal** → Recommend:
  - Start with BMad `bmad-architecture` to formalize it
  - Then validate with GStack `/plan-eng-review` for a second opinion
  - Commands: `"Run bmad-architecture"` then `"Run /plan-eng-review"`
  - **STOP. Generate mini-report and save state.**

---

## Q5: Has Architecture (Phase 3-4 — Ready for Implementation)

> **"Are you ready to start coding, or do you need to break the work into smaller pieces first?"**

Options:
- **A) I need to break it into epics and stories** → Recommend:
  - BMad `bmad-create-epics-and-stories` to create the work breakdown
  - Then `bmad-check-implementation-readiness` to verify everything is aligned
  - Then `bmad-sprint-planning` to plan the first sprint
  - Commands in order:
    1. `"Run bmad-create-epics-and-stories"`
    2. `"Run bmad-check-implementation-readiness"`
    3. `"Run bmad-sprint-planning"`
  - **STOP. Generate mini-report and save state.**

- **B) I have epics/stories — ready to code** → Go to Q6

- **C) I want to skip the breakdown and just start building** → Recommend:
  - Use GStack `/autoplan` for a fast all-in-one review pipeline
  - Then start coding with GStack as your primary tool
  - Command: `"Run /autoplan"`
  - **STOP. Generate mini-report and save state.**

---

## Q6: Ready to Build (Phase 4 — IMPLEMENTATION)

> **"What's your priority right now?"**

Options:
- **A) Write the code** → Recommend:
  - BMad `bmad-dev-story` for structured story-by-story development
  - Or BMad `bmad-quick-dev` for a faster intent→code flow
  - Commands: `"Run bmad-dev-story"` or `"Run bmad-quick-dev"`

- **B) Review existing code** → Recommend:
  - GStack `/review` for diff-level code review
  - Command: `"Run /review"`

- **C) Test what I've built** → Recommend:
  - GStack `/qa` to test in a real browser and fix bugs
  - Command: `"Run /qa"`

- **D) Ship it** → Recommend:
  - GStack `/review` first, then `/ship` to push PR
  - Commands: `"Run /review"` then `"Run /ship"`

---

## Mini-Report Format

After reaching a recommendation, generate a brief report:

```markdown
## 🧭 pilot-honda — Your Starting Point

**Current Phase:** [Phase name]
**Active Plugin:** [BMad Method / GStack / Both]

### Your Path Forward:
1. **Now:** [Exact command] — [Why]
2. **Next:** [Exact command] — [Why]
3. **After that:** [Exact command] — [Why]

### For Future Reference:
- When you finish planning → Switch to GStack for execution
- For new features later → Start with BMad `bmad-product-brief` or GStack `/office-hours`
- For bug fixes → GStack `/investigate`
- To check where you are anytime → Run `pilot-honda` again
```

Save this to `.pilot-honda-state.md` in the project root.
