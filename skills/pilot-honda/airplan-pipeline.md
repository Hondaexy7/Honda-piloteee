# ✈️ Airplan Execution Pipeline

The master execution pipeline for Airplan Mode. Defines the exact order, dependencies, dual-validation pairs, gate checkpoints, and loop logic for flying a project from idea to shipped product.

---

## How to Read This Pipeline

- **Execute** — Run the skill to produce new artifacts. The primary action step.
- **Validate** — Run the skill to review/validate the output of a prior Execute step. This is the **dual-validation** pattern — BMad creates, GStack validates, and vice versa.
- **Execute (Required)** — A BMad required gate. Cannot be skipped even if partial artifacts exist.
- **Depends On** — This step cannot start until the dependency is ✅ complete.
- **Gate** — A mandatory pause where the user reviews progress before the next phase.

---

## Skip Logic

When the scanner detects existing artifacts:
1. **Skip** Execute steps for phases that are already complete
2. **Keep** Validate steps — always run cross-plugin validation even if the artifact exists
3. **Keep** Execute (Required) steps — re-run them if artifacts look incomplete

Example: Architecture doc exists → skip `bmad-architecture` (step 3.1) → still run `/plan-eng-review` (step 3.2)

The scanner produces a skip list in this format:
```
skip_steps:
  - 0.1 (forge-idea output found at docs/forge.md)
  - 2.1 (PRD found at planning-artifacts/prd.md)
validate_still:
  - 3.2 /plan-eng-review (architecture exists but was never eng-reviewed)
```

---

## Dual-Validation Pairs

Every Execute step from one plugin is paired with a Validate step from another:

| BMad Creates (Execute) | GStack Validates | Purpose |
|------------------------|-----------------|---------|
| bmad-forge-idea | /office-hours | Reframe and pressure-test the idea |
| bmad-product-brief | /plan-ceo-review | Strategic stress-test |
| bmad-ux | taste-design-system + /plan-design-review | Full design pipeline (tri-validate) |
| bmad-architecture | /plan-eng-review | Lock architecture, find gaps |
| bmad-dev-story (code) | taste-apply + /review | Aesthetics + diff-level review (tri-validate) |
| bmad-code-review (story) | /review | Complementary: acceptance vs diff |
| bmad-qa-generate-e2e-tests | /qa | Run tests in real browser |
| bmad-retrospective | /retro | Epic-scoped vs time-scoped |

---

## Full Pipeline

### Phase 0 → 1: SPARK & ANALYSIS

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 0.1 | `bmad-forge-idea` | BMad | Execute | — |
| 0.2 | `/office-hours` | GStack | Validate | 0.1 |
| 1.1 | `bmad-product-brief` | BMad | Execute | 0.2 |
| 1.2 | `/plan-ceo-review` | GStack | Validate | 1.1 |
| 1.3 | `bmad-market-research` | BMad | Execute (Optional) | 1.1 |
| 1.4 | `bmad-domain-research` | BMad | Execute (Optional) | 1.1 |
| 1.5 | `bmad-technical-research` | BMad | Execute (Optional) | 1.1 |

**After Step 1.2:** Ask user if they want research steps (1.3–1.5). If no → skip to Gate 1.

---

### 🚦 GATE 1: Planning Readiness

**Pause and present:**
- Product brief summary
- CEO review feedback
- Research findings (if any)
- Decision: Proceed to PRD, or iterate on the idea?

---

### Phase 2: PLANNING

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 2.1 | `bmad-prd` | BMad | Execute (Required) | Gate 1 |
| 2.2 | `bmad-ux` | BMad | Execute | 2.1 |
| 2.3 | `/plan-design-review` | GStack | Validate | 2.2 |
| 2.4 | `/design-consultation` | GStack | Execute (Optional) | 2.3 |

**After Step 2.1:** Check if project has a UI component. If no UI → skip 2.2, 2.3, 2.4, and entire Phase 2.5. Jump to Phase 3.

---

### Phase 2.5: DESIGN ENGINEERING

> **Conditional phase:** Skip entirely if `has_frontend: false`. Only runs for projects with a user-facing UI.

**Platform detection (auto, before step 2.5.1):**
- Detect React Native / Flutter / Swift / Kotlin in dependencies or source files
- If mobile platform detected → auto-route image generation steps to `taste-imagegen-mobile`
- If web platform (or ambiguous) → route to `taste-imagegen-web`
- Store result in state as `platform: [web/mobile-ios/mobile-android/mobile-neutral]`

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 2.5.1 | `taste-design-system` | Taste | Execute | 2.1 (PRD) |
| 2.5.2 | `taste-brandkit` | Taste | Execute (Optional) | 2.5.1 |
| 2.5.3 | `taste-imagegen-web` OR `taste-imagegen-mobile` | Taste | Execute (Optional, image-gen required) | 2.5.1 |
| 2.5.4 | `taste-image-first` | Taste | Execute (Optional, if 2.5.3 ran) | 2.5.3 |
| 2.5.5 | `/plan-design-review` | GStack | Validate | 2.5.1 |

**Routing logic for 2.5.1 aesthetic selection:**
- User said "premium" / "agency" / "luxury" → run `taste-soft` instead
- User said "minimal" / "clean" / "editorial" → run `taste-minimalist` instead
- User said "brutalist" / "bold" / "raw" → run `taste-brutalist` instead
- No strong signal → run default `taste-design-system` (it reads the brief and decides)

**After Step 2.5.1:** Ask user:
- Need brand identity visuals? → run 2.5.2
- Need visual reference images before coding? → run 2.5.3 + 2.5.4
- No to both → skip to 2.5.5

---

### 🚦 GATE 1.5: Design Readiness (frontend projects only)

> Skip this gate entirely if `has_frontend: false`.

**Pause and present:**
- Design system summary (dials: VARIANCE / MOTION / DENSITY)
- DESIGN.md or design direction document
- Brand kit assets (if generated)
- Image references (if generated)
- GStack design review score (0-10 per dimension)
- Decision: Proceed to architecture, or refine the visual direction?

---

### Phase 3: SOLUTIONING

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 3.1 | `bmad-architecture` | BMad | Execute (Required) | 2.1 |
| 3.2 | `/plan-eng-review` | GStack | Validate | 3.1 |
| 3.3 | `bmad-create-epics-and-stories` | BMad | Execute (Required) | 3.1 |
| 3.4 | `bmad-check-implementation-readiness` | BMad | Execute (Required) | 3.3 |

---

### 🚦 GATE 2: Implementation Readiness

**This is the BMad → GStack transition point.**

**Pause and present:**
- Architecture summary
- Eng review feedback (pass/fail)
- Epics and stories created (count)
- Readiness check result
- Decision: Start coding, or fix planning gaps?

---

### Phase 4: IMPLEMENTATION

This phase contains a **story loop**. The pipeline repeats steps 4.2–4.10 for each story within each sprint.

**Sprint Setup:**

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 4.1 | `bmad-sprint-planning` | BMad | Execute (Required) | Gate 2 |

**Story Loop** (repeat for each story):

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 4.2 | `bmad-create-story` | BMad | Execute | 4.1 |
| 4.3 | `bmad-dev-story` | BMad | Execute | 4.2 |
| 4.3.5 | `taste-apply` | Taste | Execute (if UI story) | 4.3 |
| 4.4 | `bmad-code-review` | BMad | Validate (Story-level) | 4.3 |
| 4.5 | `/review` | GStack | Validate (Diff-level) | 4.3 |
| 4.6 | `bmad-qa-generate-e2e-tests` | BMad | Execute | 4.3 |
| 4.7 | `/qa` | GStack | Validate | 4.6 |

> **Step 4.3.5 routing:** Only run `taste-apply` if the story produces UI/frontend code. Skip for pure backend, API, or data stories.

**After all stories in sprint complete → check if more sprints:**
- Yes → loop back to 4.1 for next sprint
- No → continue to post-implementation validation

**Post-Implementation Validation** (run once after all sprints):

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 4.8 | `taste-redesign` | Taste | Validate (Visual audit) | All UI stories done |
| 4.9 | `/design-review` | GStack | Validate | All stories done |
| 4.10 | `/cso` | GStack | Validate | All stories done |
| 4.11 | `/benchmark` | GStack | Validate | All stories done |

---

### 🚦 GATE 3: Ship Readiness

**Pause and present:**
- Stories completed (count, pass/fail per story)
- Code review results (BMad story-level + GStack diff-level)
- Visual polish results (Taste `taste-redesign` visual audit)
- QA results
- Security audit results
- Performance benchmark results
- Decision: Ship it, or fix remaining issues?

---

### Phase 5: SHIPPING

## Plugin Usage Summary
| Plugin | Steps Executed | Steps Validated |
|--------|---------------|----------------|
| BMad Method | [count] | [count] |
| Taste-Skill | [count] | [count] |
| GStack | [count] | [count] |

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 5.1 | `/ship` | GStack | Execute | Gate 3 |
| 5.2 | `/land-and-deploy` | GStack | Execute | 5.1 |
| 5.3 | `/canary` | GStack | Validate | 5.2 |

---

### Phase 6: REFLECTION

| Step | Command | Plugin | Type | Depends On |
|------|---------|--------|------|------------|
| 6.1 | `bmad-retrospective` | BMad | Execute | 5.1 |
| 6.2 | `/retro` | GStack | Execute | 5.1 |
| 6.3 | `/document-release` | GStack | Execute | 5.1 |
| 6.4 | `bmad-checkpoint-preview` | BMad | Execute | 5.1 |

---

## Flight Plan File Format

When Airplan generates the flight plan, it writes `.pilot-honda-flight-plan.md` to the project root:

```markdown
# ✈️ pilot-honda Flight Plan
<!-- Auto-generated by pilot-honda Airplan Mode. Do not edit manually. -->

**Project:** [name]
**Created:** [ISO timestamp]
**Status:** IN FLIGHT | PAUSED | COMPLETE
**Progress:** [X/Y steps complete]
**Current Step:** [step number and name]
**Active Plugin:** [BMad | GStack]

---

## Execution Tracker

| # | Step | Command | Plugin | Type | Status | Started | Completed |
|---|------|---------|--------|------|--------|---------|-----------|
| 0.1 | Forge Idea | bmad-forge-idea | BMad | Execute | ⏳ | — | — |
| 0.2 | Office Hours | /office-hours | GStack | Validate | ⏳ | — | — |
| 1.1 | Product Brief | bmad-product-brief | BMad | Execute | ⏳ | — | — |
| 2.1 | PRD | bmad-prd | BMad | Execute (Required) | ⏳ | — | — |
| 2.5.1 | Design System | taste-design-system | Taste | Execute | ⏳ | — | — |
| 2.5.5 | Design Review | /plan-design-review | GStack | Validate | ⏳ | — | — |
| ... | ... | ... | ... | ... | ... | ... | ... |

Status legend: ⏳ PENDING | 🔄 IN PROGRESS | ✅ COMPLETE | ⏭️ SKIPPED | ❌ FAILED | ⏸️ PAUSED
Active Plugin: BMad | GStack | Taste

---

## Gate Reviews

### 🚦 Gate 1: Planning Readiness
**Status:** PENDING
**Artifacts reviewed:** —
**Decision:** —

### 🚦 Gate 2: Implementation Readiness
**Status:** PENDING
**Artifacts reviewed:** —
**Decision:** —

### 🚦 Gate 3: Ship Readiness
**Status:** PENDING
**Artifacts reviewed:** —
**Decision:** —

---

## Sprint Tracker (Phase 4)

### Sprint 1
| Story | Create | Dev | BMad Review | GStack Review | Tests | QA |
|-------|--------|-----|-------------|---------------|-------|----|
| [story 1] | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |
| [story 2] | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |
```

---

## Flight Log File Format

Append-only log at `.pilot-honda-flight-log.md`:

```markdown
# ✈️ pilot-honda Flight Log

---

## [timestamp] Step 0.1: bmad-forge-idea (BMad → Execute)
**Status:** ✅ COMPLETE
**Duration:** ~15 min
**Artifacts created:**
- docs/forge-idea-output.md
**Summary:** [1-2 sentence summary of what happened]

---

## [timestamp] Step 0.2: /office-hours (GStack → Validate)
**Status:** ✅ COMPLETE
**Duration:** ~15 min
**Validation result:** PASSED
**Summary:** [1-2 sentence summary]
```

---

## Resume Logic

When pilot-honda starts and detects `.pilot-honda-flight-plan.md`:

1. Read the file
2. Find the first step with status ⏳ PENDING or 🔄 IN PROGRESS
3. Count completed vs total steps
4. Ask the user:
   > "✈️ Active flight plan detected. [X/Y] steps complete. Resume from Step [Z]: [command name]?"
   > - A) Resume flight
   > - B) Show flight plan status
   > - C) Cancel flight (keep artifacts)
