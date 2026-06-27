# pilot-honda — Unified Lifecycle Phase Map

This document maps BMad Method's 4-phase lifecycle to GStack's sprint cycle, creating a single unified model that pilot-honda uses for all routing decisions.

---

## The Unified Lifecycle

```
Phase 0: SPARK          → Raw idea, no documentation
Phase 1: ANALYSIS       → Research, brainstorming, idea validation
Phase 2: PLANNING       → PRD, UX design, requirements definition
Phase 3: SOLUTIONING    → Architecture, epics, stories, readiness check
Phase 4: IMPLEMENTATION → Code, tests, reviews, QA
Phase 5: SHIPPING       → PR, deploy, production verification
Phase 6: REFLECTION     → Retrospective, documentation, learnings
```

---

## Phase-to-Plugin Mapping

### Phase 0: SPARK (No artifacts exist)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-brainstorming | `"Run bmad-brainstorming"` | Expert-guided brainstorming techniques |
| BMad | bmad-forge-idea | `"Run bmad-forge-idea"` | Pressure-test and harden an idea |
| GStack | /office-hours | `"Run /office-hours"` | Six forcing questions to reframe the product |
| GStack | /spec | `"Run /spec"` | Turn vague intent into a precise spec |

**Recommendation:** Start with BMad `bmad-forge-idea` to validate the idea, then GStack `/office-hours` to reframe it for implementation.

---

### Phase 1: ANALYSIS (Research and validation)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-product-brief | `"Run bmad-product-brief"` | Nail down your product concept |
| BMad | bmad-prfaq | `"Run bmad-prfaq"` | Working Backwards PRFAQ challenge |
| BMad | bmad-market-research | `"Run bmad-market-research"` | Market analysis and competitive landscape |
| BMad | bmad-domain-research | `"Run bmad-domain-research"` | Industry domain deep dive |
| BMad | bmad-technical-research | `"Run bmad-technical-research"` | Technical feasibility and approaches |
| BMad | bmad-agent-analyst | `"Invoke the Analyst agent (Mary)"` | Strategic analysis grounded in evidence |
| GStack | /plan-ceo-review | `"Run /plan-ceo-review"` | Rethink the problem, find the 10-star product |

**Evidence of completion:** Files matching `*brief*`, `*prfaq*`, `*research*` in planning artifacts folder.

**Transition trigger:** When a product brief or PRFAQ document exists and is validated.

---

### Phase 2: PLANNING (Requirements and design)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-prd | `"Run bmad-prd"` | Create/edit/validate PRD (required gate) |
| BMad | bmad-ux | `"Run bmad-ux"` | UX design guidance |
| BMad | bmad-agent-pm | `"Invoke the PM agent (John)"` | Jobs-to-be-Done product management |
| BMad | bmad-agent-ux-designer | `"Invoke the UX Designer (Sally)"` | Empathy-driven UX design |
| GStack | /plan-design-review | `"Run /plan-design-review"` | Rate each design dimension 0-10 |
| GStack | /plan-devex-review | `"Run /plan-devex-review"` | Developer experience review |
| GStack | /design-consultation | `"Run /design-consultation"` | Build a design system from scratch |

**Evidence of completion:** Files matching `*prd*`, `*requirements*`, `*ux*` in planning artifacts folder.

**Required gate (BMad):** PRD must exist before proceeding to Phase 3.

**Transition trigger:** When PRD is validated and UX design (if applicable) is complete.

---

### Phase 3: SOLUTIONING (Architecture and breakdown)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-architecture | `"Run bmad-architecture"` | Design system architecture (required gate) |
| BMad | bmad-create-epics-and-stories | `"Run bmad-create-epics-and-stories"` | Break work into epics (required gate) |
| BMad | bmad-check-implementation-readiness | `"Run bmad-check-implementation-readiness"` | Verify alignment (required gate) |
| BMad | bmad-agent-architect | `"Invoke the Architect (Winston)"` | Trade-off driven architecture decisions |
| GStack | /plan-eng-review | `"Run /plan-eng-review"` | Lock architecture, data flow, edge cases |
| GStack | /autoplan | `"Run /autoplan"` | CEO → design → eng review pipeline |

**Evidence of completion:** Files matching `*architecture*`, `*epic*`, `*readiness*`.

**Required gates (BMad):** Architecture → Epics/Stories → Readiness Check. All three must complete.

**Transition trigger:** When implementation readiness check passes. This is the handoff point from BMad-dominant to GStack-dominant.

---

### Phase 4: IMPLEMENTATION (Code, test, review)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-sprint-planning | `"Run bmad-sprint-planning"` | Plan the sprint (required gate) |
| BMad | bmad-create-story | `"Run bmad-create-story"` | Prepare stories for development |
| BMad | bmad-dev-story | `"Run bmad-dev-story"` | Execute story implementation |
| BMad | bmad-code-review | `"Run bmad-code-review"` | Story-level code review |
| BMad | bmad-agent-dev | `"Invoke the Developer (Amelia)"` | Test-first, precise implementation |
| BMad | bmad-qa-generate-e2e-tests | `"Run bmad-qa-generate-e2e-tests"` | Generate automated tests |
| GStack | /review | `"Run /review"` | Diff-level staff engineer review |
| GStack | /investigate | `"Run /investigate"` | Root-cause debugging |
| GStack | /qa | `"Run /qa"` | Test in real browser, fix bugs |
| GStack | /qa-only | `"Run /qa-only"` | Report bugs without fixing |
| GStack | /design-review | `"Run /design-review"` | Visual polish audit + fixes |
| GStack | /devex-review | `"Run /devex-review"` | Live developer experience audit |
| GStack | /cso | `"Run /cso"` | OWASP + STRIDE security audit |
| GStack | /codex | `"Run /codex"` | Cross-model second opinion |
| GStack | /benchmark | `"Run /benchmark"` | Page load and Core Web Vitals |

**Key difference between BMad and GStack in this phase:**
- BMad's `bmad-code-review` is story-level — validates against acceptance criteria
- GStack's `/review` is diff-level — finds production bugs in the actual code changes
- **Both are complementary, not conflicting.** Use BMad's for story validation, GStack's for code quality.

**Evidence of completion:** Source code files exist, test files exist, review artifacts exist.

---

### Phase 5: SHIPPING (Deploy and verify)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| GStack | /ship | `"Run /ship"` | Run tests, push, open PR |
| GStack | /land-and-deploy | `"Run /land-and-deploy"` | Merge PR, deploy, verify production |
| GStack | /canary | `"Run /canary"` | Post-deploy monitoring |
| GStack | /setup-deploy | `"Run /setup-deploy"` | One-time deploy configuration |

**BMad has no shipping skills.** GStack owns this phase entirely.

---

### Phase 6: REFLECTION (Learn and document)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-retrospective | `"Run bmad-retrospective"` | Epic-level retrospective |
| BMad | bmad-checkpoint-preview | `"Run bmad-checkpoint-preview"` | Guided walkthrough of changes |
| GStack | /retro | `"Run /retro"` | Team-aware weekly retro with metrics |
| GStack | /document-release | `"Run /document-release"` | Update all docs to match shipped code |
| GStack | /document-generate | `"Run /document-generate"` | Generate missing docs from scratch |
| GStack | /learn | `"Run /learn"` | Manage cross-session learnings |

**Key difference:**
- BMad's `bmad-retrospective` is epic-scoped and focuses on lessons learned
- GStack's `/retro` is time-scoped (weekly) and includes per-person breakdowns and metrics

---

### Anytime Skills (Available regardless of phase)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-help | `"Run bmad-help"` | Where am I? What's next? |
| BMad | bmad-quick-dev | `"Run bmad-quick-dev"` | Intent-in, code-out unified flow |
| BMad | bmad-spec | `"Run bmad-spec"` | Distill any intent into a spec |
| BMad | bmad-correct-course | `"Run bmad-correct-course"` | Navigate significant changes |
| BMad | bmad-party-mode | `"Run bmad-party-mode"` | Multi-agent discussion |
| BMad | bmad-agent-tech-writer | `"Invoke the Tech Writer (Paige)"` | Documentation, diagrams, validation |
| BMad | bmad-document-project | `"Run bmad-document-project"` | Analyze project for documentation |
| BMad | bmad-generate-project-context | `"Run bmad-generate-project-context"` | Generate LLM-optimized project context |
| GStack | /investigate | `"Run /investigate"` | Systematic debugging |
| GStack | /browse | `"Run /browse"` | Real browser interaction |
| GStack | /careful | `"Run /careful"` | Safety guardrails |
| GStack | /freeze | `"Run /freeze"` | Lock edits to one directory |
| GStack | /guard | `"Run /guard"` | Full safety mode |
| GStack | /make-pdf | `"Run /make-pdf"` | Markdown → publication-quality doc |
| GStack | /diagram | `"Run /diagram"` | English → editable diagram |
