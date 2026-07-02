# pilot-honda — Unified Lifecycle Phase Map

This document maps BMad Method's 4-phase lifecycle to GStack's sprint cycle, creating a single unified model that pilot-honda uses for all routing decisions.

---

## The Unified Lifecycle

```
Phase 0:   SPARK             → Raw idea, no documentation
Phase 1:   ANALYSIS          → Research, brainstorming, idea validation
Phase 2:   PLANNING          → PRD, UX design, requirements definition
Phase 2.5: DESIGN ENG.       → Visual direction, design system, image references [frontend only]
Phase 3:   SOLUTIONING       → Architecture, epics, stories, readiness check
Phase 4:   IMPLEMENTATION    → Code, tests, reviews, QA, visual polish
Phase 5:   SHIPPING          → PR, deploy, production verification
Phase 6:   REFLECTION        → Retrospective, documentation, learnings
```

---

## Tri-Validation Pairs (Cross-Plugin Checks)

The core power of using all three plugins: BMad creates, Taste designs, GStack validates.

### Dual-Validation (BMad ⇄ GStack)
| BMad Creates (Execute) | GStack Validates | Purpose |
|------------------------|-----------------|---------|
| `bmad-forge-idea` | `/office-hours` | Reframe and pressure-test the idea |
| `bmad-product-brief` | `/plan-ceo-review` | Strategic stress-test of the concept |
| `bmad-prd` | `/plan-ceo-review` | Strategic alignment of requirements |
| `bmad-architecture` | `/plan-eng-review` | Lock architecture, find gaps and edge cases |
| `bmad-dev-story` (code) | `/review` | Diff-level production bug detection |
| `bmad-code-review` (story) | `/review` | Complementary: acceptance criteria vs code quality |
| `bmad-qa-generate-e2e-tests` | `/qa` | Run generated tests in a real browser |
| `bmad-retrospective` | `/retro` | Epic-scoped lessons vs time-scoped metrics |

### Tri-Validation (BMad → Taste → GStack)
| BMad Creates | Taste Designs | GStack Validates | Purpose |
|-------------|---------------|-----------------|----------|
| `bmad-ux` (wireframes) | `taste-design-system` (aesthetics) | `/plan-design-review` (audit 0-10) | Full design pipeline |
| `bmad-product-brief` | `taste-brandkit` (brand identity) | `/plan-ceo-review` | Brand-aligned product vision |
| `bmad-dev-story` (code) | `taste-apply` (anti-slop polish) | `/review` + `/design-review` | Premium code quality |
| — | `taste-redesign` (visual audit + fix) | `/qa` (browser verify) | Existing project upgrades |

> **Note:** Tri-validation is the full-power mode. Dual-validation is used for non-frontend or backend-only projects. Airplan Mode runs these pairs automatically.

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
| BMad | bmad-ux | `"Run bmad-ux"` | UX design guidance (wireframes, flows) |
| BMad | bmad-agent-pm | `"Invoke the PM agent (John)"` | Jobs-to-be-Done product management |
| BMad | bmad-agent-ux-designer | `"Invoke the UX Designer (Sally)"` | Empathy-driven UX design |
| GStack | /plan-design-review | `"Run /plan-design-review"` | Rate each UX design dimension 0-10 |
| GStack | /plan-devex-review | `"Run /plan-devex-review"` | Developer experience review |
| GStack | /design-consultation | `"Run /design-consultation"` | Build a design system from scratch |

**Evidence of completion:** Files matching `*prd*`, `*requirements*`, `*ux*` in planning artifacts folder.

**Required gate (BMad):** PRD must exist before proceeding to Phase 2.5 or Phase 3.

**Transition trigger:** When PRD is validated and UX design (if applicable) is complete.

---

### Phase 2.5: DESIGN ENGINEERING (Visual direction & design system)

> **Conditional phase:** Only runs when `has_frontend: true`. Skip entirely for backend-only, CLI, or API-only projects.

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| Taste | taste-design-system | `"Run taste-design-system"` | Full anti-slop visual system (brief inference → dials → system) |
| Taste | taste-stitch | `"Run taste-stitch"` | Generate semantic DESIGN.md for AI-consistent design |
| Taste | taste-brandkit | `"Run taste-brandkit"` | Brand identity — logo, palette, typography mockups |
| Taste | taste-soft | `"Run taste-soft"` | Premium soft UI archetype (agency / luxury feel) |
| Taste | taste-minimalist | `"Run taste-minimalist"` | Editorial / Notion-Linear archetype |
| Taste | taste-brutalist | `"Run taste-brutalist"` | Swiss-print / raw mechanical archetype |
| Taste | taste-image-first | `"Run taste-image-first"` | Image-First workflow — generate visuals, analyze, then code |
| Taste | taste-imagegen-web | `"Run taste-imagegen-web"` | Web section mockup image generation (no code) |
| Taste | taste-imagegen-mobile | `"Run taste-imagegen-mobile"` | Mobile screen mockup generation (platform: iOS/Android/Neutral) |
| GStack | /plan-design-review | `"Run /plan-design-review"` | Validate design system choices, score 0-10 |

**Evidence of completion:** `DESIGN.md` exists, or design reference images in `design/` or `mockups/` folder.

**Aesthetic archetype routing:**
- No strong vibe signal → `taste-design-system` (reads the brief and decides)
- Explicitly "premium" / "agency" → `taste-soft`
- Explicitly "minimal" / "editorial" / "clean" → `taste-minimalist`
- Explicitly "bold" / "brutal" / "raw" → `taste-brutalist`
- Needs visual reference images first → `taste-imagegen-web` or `taste-imagegen-mobile`, then `taste-image-first`
- Mobile project (React Native / Flutter / Swift / Kotlin) → auto-route to `taste-imagegen-mobile`
- Need brand identity visuals → `taste-brandkit`

**Transition trigger:** DESIGN.md exists or design reference images approved. Then proceed to Phase 3.

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

### Phase 4: IMPLEMENTATION (Code, test, review, polish)

| Plugin | Skill | Command | Purpose |
|--------|-------|---------|---------|
| BMad | bmad-sprint-planning | `"Run bmad-sprint-planning"` | Plan the sprint (required gate) |
| BMad | bmad-create-story | `"Run bmad-create-story"` | Prepare stories for development |
| BMad | bmad-dev-story | `"Run bmad-dev-story"` | Execute story implementation |
| BMad | bmad-code-review | `"Run bmad-code-review"` | Story-level code review |
| BMad | bmad-agent-dev | `"Invoke the Developer (Amelia)"` | Test-first, precise implementation |
| BMad | bmad-qa-generate-e2e-tests | `"Run bmad-qa-generate-e2e-tests"` | Generate automated tests |
| Taste | taste-apply | `"Run taste-apply"` | Anti-slop polish — remove generic AI patterns from coded UI |
| Taste | taste-redesign | `"Run taste-redesign"` | Full visual audit and targeted upgrade of existing code |
| Taste | taste-gpt | `"Run taste-gpt"` | Awwwards-level GSAP motion and premium micro-interactions |
| Taste | taste-output | `"Run taste-output"` | Enforce complete code output — no lazy placeholders |
| GStack | /review | `"Run /review"` | Diff-level staff engineer review |
| GStack | /investigate | `"Run /investigate"` | Root-cause debugging |
| GStack | /qa | `"Run /qa"` | Test in real browser, fix bugs |
| GStack | /qa-only | `"Run /qa-only"` | Report bugs without fixing |
| GStack | /design-review | `"Run /design-review"` | Visual polish audit + fixes |
| GStack | /devex-review | `"Run /devex-review"` | Live developer experience audit |
| GStack | /cso | `"Run /cso"` | OWASP + STRIDE security audit |
| GStack | /codex | `"Run /codex"` | Cross-model second opinion |
| GStack | /benchmark | `"Run /benchmark"` | Page load and Core Web Vitals |

**Key differences in this phase:**
- BMad's `bmad-code-review` is story-level — validates against acceptance criteria
- GStack's `/review` is diff-level — finds production bugs in the actual code changes
- Taste's `taste-apply` is aesthetics-level — removes generic AI patterns from coded components
- **All three are complementary.** Use BMad for story validation, GStack for code quality, Taste for visual quality.

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
| Taste | taste-output | `"Run taste-output"` | Enforce complete code output — no lazy placeholders, no `// ...rest` comments |
| Taste | taste-redesign | `"Run taste-redesign"` | Visual audit & targeted upgrade of any existing frontend UI |
| GStack | /investigate | `"Run /investigate"` | Systematic debugging |
| GStack | /browse | `"Run /browse"` | Real browser interaction |
| GStack | /careful | `"Run /careful"` | Safety guardrails |
| GStack | /freeze | `"Run /freeze"` | Lock edits to one directory |
| GStack | /guard | `"Run /guard"` | Full safety mode |
| GStack | /make-pdf | `"Run /make-pdf"` | Markdown → publication-quality doc |
| GStack | /diagram | `"Run /diagram"` | English → editable diagram |
