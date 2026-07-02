# pilot-honda — Command Registry

This registry maps every available command from BMad Method, GStack, and Taste-Skill to its source SKILL.md file, phase, and prerequisites. When the user invokes a command, this file tells pilot-honda where to find the skill definition and how to execute it.

---

## How Command Resolution Works

1. User types a command (e.g., `/review`, `bmad-architecture`, or natural language like "review my code")
2. pilot-honda checks this registry for a match
3. If found: reads the source SKILL.md file from the plugin directory and follows its methodology
4. If not found: asks the user to clarify what they want

**Important:** When executing a skill from any plugin in Antigravity, skip any Claude Code-specific sections (bash preambles, `~/.claude/skills/` paths, CLAUDE.md writes, telemetry logging). Focus on the **methodology content** — the workflow steps, checklists, quality gates, and output formats.

**Taste-Skill v1/v2 rule:** Always default to v2 (`skills/taste-skill/SKILL.md`). Route to v1 (`skills/taste-skill-v1/SKILL.md`) only when the user explicitly requests it, or when the scanner detects v1-style Framer Motion `layoutId`/`staggerChildren` bento patterns in existing code.

---

## BMad Method Commands

Source: `D:\AntigravityPlugins\bmad-method\`

### Core Skills (available anytime)

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `bmad-help` | "bmad help", "what do I do next" | `src/core-skills/bmad-help/SKILL.md` | anytime |
| `bmad-brainstorming` | "brainstorm", "BSP" | `src/core-skills/bmad-brainstorming/SKILL.md` | anytime |
| `bmad-party-mode` | "party mode", "PM" | `src/core-skills/bmad-party-mode/SKILL.md` | anytime |
| `bmad-spec` | "spec", "SP" | `src/core-skills/bmad-spec/SKILL.md` | anytime |
| `bmad-forge-idea` | "forge idea", "FI" | `src/core-skills/bmad-forge-idea/SKILL.md` | anytime |
| `bmad-index-docs` | "index docs", "ID" | `src/core-skills/bmad-index-docs/SKILL.md` | anytime |
| `bmad-shard-doc` | "shard document", "SD" | `src/core-skills/bmad-shard-doc/SKILL.md` | anytime |
| `bmad-editorial-review-prose` | "editorial prose", "EP" | `src/core-skills/bmad-editorial-review-prose/SKILL.md` | anytime |
| `bmad-editorial-review-structure` | "editorial structure", "ES" | `src/core-skills/bmad-editorial-review-structure/SKILL.md` | anytime |
| `bmad-review-adversarial-general` | "adversarial review", "AR" | `src/core-skills/bmad-review-adversarial-general/SKILL.md` | anytime |
| `bmad-review-edge-case-hunter` | "edge case hunter", "ECH" | `src/core-skills/bmad-review-edge-case-hunter/SKILL.md` | anytime |
| `bmad-customize` | "customize bmad", "BC" | `src/core-skills/bmad-customize/SKILL.md` | anytime |

### Phase 1: Analysis

| Command | Aliases | Source SKILL.md | Prerequisites |
|---------|---------|----------------|---------------|
| `bmad-product-brief` | "create brief", "CB" | `src/bmm-skills/1-analysis/bmad-product-brief/SKILL.md` | None |
| `bmad-prfaq` | "PRFAQ challenge", "WB" | `src/bmm-skills/1-analysis/bmad-prfaq/SKILL.md` | None |
| `bmad-document-project` | "document project", "DP" | `src/bmm-skills/1-analysis/bmad-document-project/SKILL.md` | None |
| `bmad-generate-project-context` | "generate context", "GPC" | `src/bmm-skills/1-analysis/bmad-generate-project-context/SKILL.md` | None |
| `bmad-agent-analyst` | "analyst agent", "Mary" | `src/bmm-skills/1-analysis/bmad-agent-analyst/SKILL.md` | None |
| `bmad-agent-tech-writer` | "tech writer", "Paige" | `src/bmm-skills/1-analysis/bmad-agent-tech-writer/SKILL.md` | None |

### Phase 2: Planning

| Command | Aliases | Source SKILL.md | Prerequisites |
|---------|---------|----------------|---------------|
| `bmad-prd` | "PRD", "create PRD" | `src/bmm-skills/2-plan-workflows/bmad-prd/SKILL.md` | product-brief |
| `bmad-create-prd` | "create PRD" | `src/bmm-skills/2-plan-workflows/bmad-create-prd/SKILL.md` | product-brief |
| `bmad-edit-prd` | "edit PRD" | `src/bmm-skills/2-plan-workflows/bmad-edit-prd/SKILL.md` | bmad-prd |
| `bmad-validate-prd` | "validate PRD" | `src/bmm-skills/2-plan-workflows/bmad-validate-prd/SKILL.md` | bmad-prd |
| `bmad-ux` | "create UX", "CU" | `src/bmm-skills/2-plan-workflows/bmad-ux/SKILL.md` | bmad-prd |
| `bmad-agent-pm` | "PM agent", "John" | `src/bmm-skills/2-plan-workflows/bmad-agent-pm/SKILL.md` | None |
| `bmad-agent-ux-designer` | "UX designer", "Sally" | `src/bmm-skills/2-plan-workflows/bmad-agent-ux-designer/SKILL.md` | None |

### Phase 3: Solutioning

| Command | Aliases | Source SKILL.md | Prerequisites |
|---------|---------|----------------|---------------|
| `bmad-architecture` | "architecture", "CA" | `src/bmm-skills/3-solutioning/bmad-architecture/SKILL.md` | bmad-prd |
| `bmad-create-architecture` | "create architecture" | `src/bmm-skills/3-solutioning/bmad-create-architecture/SKILL.md` | bmad-prd |
| `bmad-create-epics-and-stories` | "create epics", "CE" | `src/bmm-skills/3-solutioning/bmad-create-epics-and-stories/SKILL.md` | bmad-architecture |
| `bmad-check-implementation-readiness` | "readiness check", "IR" | `src/bmm-skills/3-solutioning/bmad-check-implementation-readiness/SKILL.md` | bmad-create-epics-and-stories |
| `bmad-generate-project-context` | "project context", "GPC" | `src/bmm-skills/3-solutioning/bmad-generate-project-context/SKILL.md` | None |
| `bmad-agent-architect` | "architect agent", "Winston" | `src/bmm-skills/3-solutioning/bmad-agent-architect/SKILL.md` | None |

### Phase 4: Implementation

| Command | Aliases | Source SKILL.md | Prerequisites |
|---------|---------|----------------|---------------|
| `bmad-sprint-planning` | "sprint planning", "SP" | `src/bmm-skills/4-implementation/bmad-sprint-planning/SKILL.md` | readiness check |
| `bmad-sprint-status` | "sprint status", "SS" | `src/bmm-skills/4-implementation/bmad-sprint-status/SKILL.md` | sprint-planning |
| `bmad-create-story` | "create story", "CS" | `src/bmm-skills/4-implementation/bmad-create-story/SKILL.md` | sprint-planning |
| `bmad-dev-story` | "dev story", "DS" | `src/bmm-skills/4-implementation/bmad-dev-story/SKILL.md` | bmad-create-story |
| `bmad-dev-auto` | "dev auto" | `src/bmm-skills/4-implementation/bmad-dev-auto/SKILL.md` | bmad-create-story |
| `bmad-quick-dev` | "quick dev", "QQ" | `src/bmm-skills/4-implementation/bmad-quick-dev/SKILL.md` | None |
| `bmad-code-review` | "code review", "CR" | `src/bmm-skills/4-implementation/bmad-code-review/SKILL.md` | bmad-dev-story |
| `bmad-checkpoint-preview` | "checkpoint", "CK" | `src/bmm-skills/4-implementation/bmad-checkpoint-preview/SKILL.md` | None |
| `bmad-qa-generate-e2e-tests` | "QA tests", "QA" | `src/bmm-skills/4-implementation/bmad-qa-generate-e2e-tests/SKILL.md` | bmad-dev-story |
| `bmad-retrospective` | "retrospective", "ER" | `src/bmm-skills/4-implementation/bmad-retrospective/SKILL.md` | bmad-code-review |
| `bmad-correct-course` | "correct course", "CC" | `src/bmm-skills/4-implementation/bmad-correct-course/SKILL.md` | None |
| `bmad-agent-dev` | "developer agent", "Amelia" | `src/bmm-skills/4-implementation/bmad-agent-dev/SKILL.md` | None |

---

## GStack Commands

Source: `D:\AntigravityPlugins\gstack\`

### Think & Plan

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `/office-hours` | "office hours", "brainstorm with gstack" | `office-hours/SKILL.md` | Phase 0-1 |
| `/plan-ceo-review` | "ceo review", "strategic review" | `plan-ceo-review/SKILL.md` | Phase 1 |
| `/plan-eng-review` | "eng review", "architecture review" | `plan-eng-review/SKILL.md` | Phase 3 |
| `/plan-design-review` | "design review plan" | `plan-design-review/SKILL.md` | Phase 2 |
| `/plan-devex-review` | "devex review plan", "DX review" | `plan-devex-review/SKILL.md` | Phase 2 |
| `/autoplan` | "review everything", "auto plan" | `autoplan/SKILL.md` | Phase 3 |
| `/spec` | "spec", "write a spec" | `spec/SKILL.md` | Phase 0-1 |

### Build & Review

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `/review` | "code review", "review my changes" | `review/SKILL.md` | Phase 4 |
| `/investigate` | "debug", "why is this broken" | `investigate/SKILL.md` | Phase 4 / anytime |
| `/design-consultation` | "design system", "brand identity" | `design-consultation/SKILL.md` | Phase 2 |
| `/design-review` | "visual polish", "design audit" | `design-review/SKILL.md` | Phase 4 |
| `/design-shotgun` | "design options", "show me variants" | `design-shotgun/SKILL.md` | Phase 2-4 |
| `/design-html` | "mockup to HTML", "production HTML" | `design-html/SKILL.md` | Phase 4 |
| `/devex-review` | "live DX audit", "developer experience" | `devex-review/SKILL.md` | Phase 4 |
| `/codex` | "second opinion", "codex review" | `codex/SKILL.md` | Phase 4 |

### Test

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `/qa` | "QA", "test this site", "find bugs" | `qa/SKILL.md` | Phase 4 |
| `/qa-only` | "report bugs only" | `qa-only/SKILL.md` | Phase 4 |
| `/browse` | "open browser", "browse site" | `browse/SKILL.md` | anytime |
| `/benchmark` | "performance test", "page speed" | `benchmark/SKILL.md` | Phase 4 |
| `/cso` | "security audit", "OWASP" | `cso/SKILL.md` | Phase 4 |

### Ship

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `/ship` | "ship it", "push PR", "send it" | `ship/SKILL.md` | Phase 5 |
| `/land-and-deploy` | "merge and deploy", "land it" | `land-and-deploy/SKILL.md` | Phase 5 |
| `/canary` | "post-deploy monitor" | `canary/SKILL.md` | Phase 5 |
| `/setup-deploy` | "configure deploy" | `setup-deploy/SKILL.md` | Phase 5 (one-time) |

### Reflect & Document

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `/retro` | "retrospective", "how'd we do" | `retro/SKILL.md` | Phase 6 |
| `/document-release` | "update docs", "doc release" | `document-release/SKILL.md` | Phase 6 |
| `/document-generate` | "generate docs", "write docs" | `document-generate/SKILL.md` | Phase 6 |
| `/learn` | "show learnings", "what did we learn" | `learn/SKILL.md` | anytime |

### Safety & Utilities

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `/careful` | "be careful", "safety mode" | `careful/SKILL.md` | anytime |
| `/freeze` | "lock edits", "restrict editing" | `freeze/SKILL.md` | anytime |
| `/guard` | "full safety", "careful + freeze" | `guard/SKILL.md` | anytime |
| `/unfreeze` | "unlock edits" | `unfreeze/SKILL.md` | anytime |
| `/make-pdf` | "make PDF", "export document" | `make-pdf/SKILL.md` | anytime |
| `/diagram` | "make diagram", "draw diagram" | `diagram/SKILL.md` | anytime |
| `/gstack-upgrade` | "upgrade gstack" | `gstack-upgrade/SKILL.md` | anytime |

---

## Overlapping Commands — Disambiguation Guide

Some commands appear similar across both plugins. When the user types an ambiguous command, use this guide:

| User says | Could mean | Resolution |
|-----------|-----------|------------|
| "review" | BMad `bmad-code-review` OR GStack `/review` | Ask: "Story-level review (checks acceptance criteria) or diff-level review (finds production bugs)?" Default to GStack `/review` if code changes exist. |
| "brainstorm" | BMad `bmad-brainstorming` OR GStack `/office-hours` | Ask: "Structured brainstorming techniques (BMad) or product reframing with forcing questions (GStack)?" Default to BMad if no idea exists yet. |
| "spec" | BMad `bmad-spec` OR GStack `/spec` | Ask: "Planning spec for BMad workflow (generates SPEC.md) or executable spec for GStack pipeline?" Default to BMad if in planning phase. |
| "architecture" | BMad `bmad-architecture` OR GStack `/plan-eng-review` | BMad `bmad-architecture` creates it. GStack `/plan-eng-review` validates it. Route based on whether architecture already exists. |
| "QA" / "test" | BMad `bmad-qa-generate-e2e-tests` OR GStack `/qa` | BMad generates test code. GStack runs tests in a real browser. Ask: "Generate test code or run live QA?" |
| "retrospective" | BMad `bmad-retrospective` OR GStack `/retro` | BMad is epic-scoped. GStack is time-scoped (weekly). Ask: "End of epic retro or weekly retro?" |
| "design" | BMad `bmad-ux` OR GStack `/design-consultation` OR Taste `taste-design-system` | BMad = UX flows/wireframes. GStack = design system audit/review. Taste = premium visual aesthetic blueprint. Ask: "Functional UX layout, design system review, or premium visual styling?" Default to Taste `taste-design-system` if no design artifacts exist. |
| "redesign" | Taste `taste-redesign` OR BMad `bmad-ux` | Taste for visual upgrades to existing frontend (keep structure, fix aesthetics). BMad for full UX re-architecture. Ask: "Visual polish keeping existing structure, or full UX overhaul?" Default to Taste if code exists and design looks generic. |
| "mockup" | Taste `taste-imagegen-web` / `taste-imagegen-mobile` OR BMad `bmad-ux` | Taste generates visual reference images. BMad creates wireframe descriptions in text. Ask: "Visual reference image or UX wireframe document?" |
| "polish" / "make it look better" | Taste `taste-apply` OR GStack `/design-review` | Taste `taste-apply` actively rewrites code to remove generic patterns. GStack `/design-review` audits and reports issues. Default to Taste `taste-apply` for code changes, GStack `/design-review` for a report only. |
| "brand" / "logo" | Taste `taste-brandkit` OR BMad `bmad-product-brief` | Taste generates visual brand identity images. BMad documents the brand positioning in text. Ask: "Visual brand assets (logo, palette, typography mockups) or brand positioning document?" |

---

## Taste-Skill Commands

Source: `D:\AntigravityPlugins\taste-skill\`

**Scope boundary:** Taste-Skill is for landing pages, portfolios, marketing sites, and redesigns. It is NOT for dashboards, data tables, admin panels, multi-step product UIs, or CLI tools. If the brief is out of scope, skip taste-skill and say so explicitly.

### Phase 2.5: Design Engineering (Visual Direction & System)

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `taste-design-system` | "design system", "design taste", "anti-slop", "set up design" | `skills/taste-skill/SKILL.md` | Phase 2.5 |
| `taste-stitch` | "stitch design", "generate DESIGN.md", "design doc" | `skills/stitch-skill/SKILL.md` | Phase 2.5 |
| `taste-soft` | "soft design", "premium look", "expensive feel", "agency look" | `skills/soft-skill/SKILL.md` | Phase 2.5 |
| `taste-minimalist` | "minimalist", "editorial style", "notion style", "linear style", "clean" | `skills/minimalist-skill/SKILL.md` | Phase 2.5 |
| `taste-brutalist` | "brutalist", "raw design", "swiss", "tactical" | `skills/brutalist-skill/SKILL.md` | Phase 2.5 |
| `taste-brandkit` | "brand kit", "logo", "brand identity", "brand assets" | `skills/brandkit/SKILL.md` | Phase 2.5 |
| `taste-v1` | "taste v1", "old taste", "framer bento" | `skills/taste-skill-v1/SKILL.md` | Phase 2.5 (legacy only) |

### Phase 2.5: Image-First Design (requires image generation capability)

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `taste-image-first` | "image to code", "design reference", "visual reference", "image first" | `skills/image-to-code-skill/SKILL.md` | Phase 2.5 |
| `taste-imagegen-web` | "web mockup", "generate web design", "web sections" | `skills/imagegen-frontend-web/SKILL.md` | Phase 2.5 |
| `taste-imagegen-mobile` | "mobile mockup", "generate mobile design", "mobile screens", "app screens" | `skills/imagegen-frontend-mobile/SKILL.md` | Phase 2.5 |

**Image generation note:** If the current platform cannot generate images, offer `taste-stitch` (generates a semantic DESIGN.md) or `taste-design-system` (text-based design blueprint) as text-only alternatives.

### Phase 4: Implementation Polish (apply during/after coding)

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `taste-apply` | "apply taste", "make it premium", "remove generic", "polish UI" | `skills/taste-skill/SKILL.md` | Phase 4 |
| `taste-redesign` | "redesign", "upgrade design", "fix the UI", "visual audit" | `skills/redesign-skill/SKILL.md` | Phase 4 |
| `taste-gpt` | "awwwards", "GSAP motion", "premium motion", "cinematic" | `skills/gpt-tasteskill/SKILL.md` | Phase 4 |

### Anytime (universal quality enforcement)

| Command | Aliases | Source SKILL.md | Phase |
|---------|---------|----------------|-------|
| `taste-output` | "full output", "no lazy code", "no placeholders", "complete the code" | `skills/output-skill/SKILL.md` | anytime |
