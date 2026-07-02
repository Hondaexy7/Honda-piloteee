# pilot-honda — Project Scanner Rules

Instructions for auto-scan mode (Mode B). When the user invokes pilot-honda on an existing project, follow these rules to analyze the codebase and generate a Project Maturity Report.

---

## Scanning Procedure

### Step 1: Identify the Project Root

The project root is the user's current workspace directory. Verify it contains at least one of:
- A `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, or similar manifest
- A `.git/` directory
- Source code files
- A `README.md`

If none of these exist, ask the user to confirm the project location.

### Step 2: Check for Plugin Installations

**BMad Method detection:**
- Look for `_bmad/` directory → BMad is installed in this project
- If found, read `_bmad/_config/bmad-help.csv` for BMad's own completion tracking
- Read `_bmad/_config/config.yaml` for project settings (output folders, language, etc.)
- Check configured output folders: `planning_artifacts`, `implementation_artifacts`, `project_knowledge`

**GStack detection:**
- Look for `.gstack/` directory → GStack state exists
- Look for `CLAUDE.md` with `## gstack` or `## Skill routing` sections
- If `.gstack/projects/*/timeline.jsonl` exists, read it for recent skill history
- If `.gstack/projects/*/learnings.jsonl` exists, note learned patterns

**Taste-Skill detection:**
- Look for `DESIGN.md` in project root or `docs/` folder → Taste design system is active
- Look for design reference images (`*.webp`, `*.png`) in `design/`, `mockups/`, or `design-ref/` folders
- Scan CSS/component files for taste best-practice signatures:
  - `min-h-[100dvh]` or `min-height: 100dvh` usage → Taste viewport rule followed
  - Font imports: `Geist`, `Outfit`, `Satoshi`, `Cabinet Grotesk`, `Fraunces`, `Instrument Serif` → Premium typography applied
  - `cubic-bezier` transitions (non-linear easing) → Motion rules followed
  - `staggerChildren` + `layoutId` from Framer Motion → **v1 bento pattern detected**
- Scan `package.json` for mobile platform dependencies:
  - `react-native`, `expo`, `@react-native-*` → Mobile (React Native)
  - `flutter` → Mobile (Flutter)
  - Swift/Kotlin file extensions in source → Mobile (iOS/Android)
  - If mobile detected: set `platform: mobile`, auto-route image generation to `taste-imagegen-mobile`
- If 0 taste signals found but project has a frontend: set `design_debt_detected: null` (unknown)
- If taste signatures found: set `taste_installed: true`
- If v1 pattern detected (Framer `staggerChildren` + `layoutId` bento): set `v1_patterns_detected: true`

**Both BMad and GStack installed:** If both are detected, flag this in the conflict guard section of the report.

### Step 3: Scan for Phase Artifacts

Scan the project directory for evidence of each lifecycle phase. Use the patterns below. Score each phase as:

- ✅ **Complete** — Required artifacts exist and appear substantive (>100 lines or multiple files)
- ⚠️ **Partial** — Some artifacts exist but are incomplete, or only optional items found
- ❌ **Missing** — No artifacts found for this phase
- ⏭️ **Skipped** — Phase not applicable (e.g., no UI = skip UX design)

---

### Phase 0: SPARK (Ideation)

**Scan for:**
```
**/brainstorm*          → Brainstorming output
**/ideation*            → Ideation documents
**/concept*             → Concept documents
**/forge*               → BMad forge-idea output
```

**Score:**
- Any file found → ✅ Complete (ideation happened)
- No files but README exists with project description → ⚠️ Partial
- Nothing → ❌ Missing

---

### Phase 1: ANALYSIS (Research & Validation)

**Scan for:**
```
**/brief*               → Product brief
**/product-brief*       → BMad product brief
**/prfaq*               → PRFAQ document
**/research*            → Research documents
**/market*research*     → Market research
**/domain*research*     → Domain research
**/technical*research*  → Technical research
**/competitive*         → Competitive analysis
```

**Score:**
- Product brief or PRFAQ exists → ✅ Complete
- Only research documents → ⚠️ Partial (research done but no brief)
- Nothing → ❌ Missing

---

### Phase 2: PLANNING (Requirements & UX)

**Scan for:**
```
**/prd*                 → Product Requirements Document
**/requirements*        → Requirements specification
**/PRD*                 → Case-insensitive PRD
**/user-stories*        → User stories (non-epic)
**/ux*                  → UX design documents
**/wireframe*           → Wireframes
**/mockup*              → Mockups
**/design-spec*         → Design specifications
**/ui-spec*             → UI specifications
**/figma*               → Figma references
```

**Score:**
- PRD exists and is substantive (>50 lines) → ✅ Complete
- PRD exists but is thin (<50 lines) → ⚠️ Partial
- UX docs exist → Note separately (UX sub-phase complete)
- No PRD → ❌ Missing (this is a required gate for BMad)

---

### Phase 2.5: DESIGN ENGINEERING (Visual Direction & System)

> **Conditional phase:** Only score this phase if the project has a frontend component.

**Scan for:**
```
**/DESIGN.md              → Taste design system document
**/design-system*         → Design system files
**/style-guide*           → Style guide
**/brand*                 → Brand kit / identity files
**/mockup*                → Design mockups
**/design-ref*            → Design reference images
design/**                 → Design assets folder
mockups/**                → Mockups folder
```

**Score:**
- `DESIGN.md` exists and is substantive (>30 lines with colors, typography, component rules) → ✅ Complete
- Brand kit or mockup images exist but no `DESIGN.md` → ⚠️ Partial
- Premium fonts or `min-h-[100dvh]` detected in code but no `DESIGN.md` → ⚠️ Partial (implicit design, undocumented)
- Frontend exists but no design signals found → ❌ Missing (recommend `taste-design-system`)
- No frontend → ⏭️ Skipped

---

### Phase 3: SOLUTIONING (Architecture & Breakdown)

**Scan for:**
```
**/architecture*        → Architecture document
**/ARCHITECTURE.md      → Standard architecture file
**/arch*                → Architecture references
**/system-design*       → System design
**/data-model*          → Data model documents
**/api-spec*            → API specifications
**/epic*                → Epic definitions
**/stor*                → Story definitions (careful: not "store")
**/sprint*              → Sprint-related artifacts
**/implementation-readiness* → BMad readiness check
**/tech-spec*           → Technical specification
```

**Score:**
- Architecture + Epics + Stories all exist → ✅ Complete
- Architecture exists but no epics/stories → ⚠️ Partial
- Only epics without architecture → ⚠️ Partial (unusual — flag this)
- Nothing → ❌ Missing

---

### Phase 4: IMPLEMENTATION (Code & Tests)

**Scan for source code:**
```
**/*.ts, **/*.tsx       → TypeScript
**/*.js, **/*.jsx       → JavaScript
**/*.py                 → Python
**/*.rs                 → Rust
**/*.go                 → Go
**/*.java               → Java
**/*.rb                 → Ruby
**/*.swift              → Swift
**/*.cs                 → C#
**/*.cpp, **/*.c        → C/C++
```

**Scan for tests:**
```
**/test/**              → Test directory
**/tests/**             → Test directory
**/spec/**              → Spec directory (RSpec style)
**/__tests__/**         → Jest-style tests
**/*.test.*             → Test files
**/*.spec.*             → Spec files
**/jest.config*         → Jest configuration
**/vitest.config*       → Vitest configuration
**/pytest.ini           → Pytest configuration
**/cypress/**           → Cypress E2E tests
**/playwright/**        → Playwright tests
```

**Scan for review artifacts:**
```
**/review*              → Code review documents
**/REVIEW.md            → Review report
**/.gstack/projects/*/reviews* → GStack review history
```

**Score:**
- Source code exists + tests exist + reviews done → ✅ Complete
- Source code exists + tests exist but no reviews → ⚠️ Partial
- Source code exists but no tests → ⚠️ Partial (flag: "code exists but untested")
- No source code → ❌ Not started

**Metrics to report:**
- Number of source files
- Number of test files
- Approximate test-to-source ratio
- Languages detected
- **Design debt signals detected** (see Design Debt Detection section below)

---

### Phase 5: SHIPPING (Deploy & CI/CD)

**Scan for:**
```
.github/workflows/**   → GitHub Actions
.gitlab-ci.yml          → GitLab CI
Jenkinsfile             → Jenkins
.circleci/**            → CircleCI
Dockerfile              → Docker
docker-compose*         → Docker Compose
vercel.json             → Vercel
netlify.toml            → Netlify
fly.toml                → Fly.io
render.yaml             → Render
railway.json            → Railway
```

**Scan for git state:**
```
git log --oneline -5    → Recent commits
git branch -r           → Remote branches
git tag                 → Release tags
```

**Score:**
- CI/CD config exists + recent deploys/tags → ✅ Complete
- CI/CD config exists but no tags/releases → ⚠️ Partial
- No CI/CD → ❌ Missing

---

### Phase 6: REFLECTION (Documentation & Learnings)

**Scan for:**
```
README.md               → Project README (assess quality/completeness)
CONTRIBUTING.md          → Contribution guide
CHANGELOG.md            → Change log
docs/**                 → Documentation directory
**/retrospective*       → Retrospective documents
**/retro*               → Retro documents
```

**Score:**
- README + docs folder + changelog exist → ✅ Complete
- README exists but thin or outdated → ⚠️ Partial
- No README → ❌ Missing

---

## Step 4: Deep Document Analysis

If a `docs/` directory exists (or BMad's configured `project_knowledge` folder), read key documents to understand:

1. **Project purpose** — What does this project do?
2. **Technical stack** — What technologies are used?
3. **Current status** — Are there TODOs, known issues, or roadmap items?
4. **Team context** — Solo developer or team? Any contribution guidelines?

Use this context to make the maturity report recommendations more specific and relevant.

---

## Design Debt Detection (Taste-Skill Auto-Trigger)

When scanning an existing project with a frontend, actively check for these **design debt signals** in source/CSS files. These are the most common "AI slop" patterns that `taste-redesign` can fix:

| Signal | What to look for | Weight |
|--------|-----------------|--------|
| Generic font | `font-family: Inter` or browser defaults without premium override | 1 |
| Viewport bug | `height: 100vh` or `h-screen` instead of `min-height: 100dvh` | 1 |
| Generic 3-col layout | `.grid-cols-3` with equal widths, or 3 identical `flex` children | 1 |
| Pure black background | `background: #000000` or `bg-black` | 1 |
| Generic shadows | `box-shadow: 0 4px 6px rgba(0,0,0,0.3)` or Tailwind `shadow-md` | 1 |
| Linear transitions | `transition: all linear` or `ease-in-out` exclusively | 1 |
| Thick icon libraries | `lucide-react` or `@fortawesome` as only icon source | 1 |
| AI purple gradient | `purple` + `blue` gradient or `from-purple-500 to-blue-500` | 1 |
| Generic names | "John Doe", "Jane Smith", "Acme", "Nexus" in content | 1 |
| Centered hero | Hero section with `text-center` and no asymmetry | 1 |

**Scoring:**
- 0–2 signals: No alert needed
- 3–5 signals: Add to report as a "Design Improvement Opportunity"
- 6+ signals: Flag prominently as **Design Debt Detected** and strongly recommend `taste-redesign`

**Report language for 3+ signals:**
> ⚠️ **Design Debt Detected** ([X] signals found). Your frontend uses several common AI design patterns that make it look generic. Running `taste-redesign` before adding new features is a high-impact, low-risk upgrade.
> 
> Signals detected: [list the specific signals found]
> 
> Command: `"Run taste-redesign"` (Taste → Execute)
> Then verify: `"Run /design-review"` (GStack → Validate)

**v1 Pattern Detection:**
If `staggerChildren` + `layoutId` from Framer Motion are both present in the same component tree (v1 bento pattern):
- Set `v1_patterns_detected: true` in state
- When the user runs any taste command, route to v1 instead of v2 and note:
  > "I detected v1-style Framer Motion bento patterns in your project. Using `taste-skill-v1` for compatibility. If you want to upgrade to v2, tell me and I'll guide the migration."

---

## Step 5: Generate the Report

See SKILL.md Section 4 for the report format. The report must include:

1. **Lifecycle Map** — Visual table of all phases with status (including Phase 2.5)
2. **Evidence** — Specific files that prove each phase's status
3. **Design State** — Taste-skill signals, design debt score, v1/v2 detection
4. **Recommended Next Steps** — Ordered by priority, with exact commands
5. **Conflict Guard** — If multiple plugins detected
6. **Future Feature Guidance** — Which plugin to use for planned features
7. **Quick Reference Card** — 3-5 most important commands for current phase

**Updated Lifecycle Map rows (add to report format):**

| Phase | Status | Evidence | Recommended Plugin |
|-------|--------|----------|--------------------|
| 💡 Ideation | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 🔬 Analysis | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 💻 Planning (PRD) | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 🎨 UX Design | [✅/⚠️/❌/⏭️] | [files found or "none"] | BMad |
| ✨ Design Engineering | [✅/⚠️/❌/⏭️] | [DESIGN.md / images / "none"] | Taste |
| 🏗️ Architecture | [✅/⚠️/❌] | [files found or "none"] | BMad → GStack |
| 📦 Epics & Stories | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 💻 Implementation | [✅/⚠️/❌] | [X source files, Y test files] | GStack |
| 🔍 Code Review | [✅/⚠️/❌] | [review artifacts] | GStack |
| 🧪 Testing/QA | [✅/⚠️/❌] | [test files, coverage] | GStack |
| 🔒 Security | [✅/⚠️/❌] | [audit results] | GStack |
| 🚀 CI/CD & Shipping | [✅/⚠️/❌] | [CI config files] | GStack |
| 📖 Documentation | [✅/⚠️/❌] | [docs found] | Both |

---

## Step 6: Save State

Write `.pilot-honda-state.md` to the project root with:
- Scan timestamp
- Phase scores
- Current recommended phase
- Active plugin recommendation
- List of detected artifacts

On subsequent runs, compare the new scan against the saved state to show progress:
> "Since your last scan (3 days ago): Architecture doc was added ✅, 12 new source files, still no tests."

---

## Step 7: Airplan Skip Logic (Airplan Mode Only)

When scanning for Airplan Mode (SKILL.md Step 10), the scanner must produce additional output beyond the normal phase scores. This tells the flight plan builder which pipeline steps to skip and which validation steps to keep.

### Produce a Skip List

Map each detected artifact to a specific pipeline step number from `airplan-pipeline.md`:

| Artifact Found | Pipeline Step to Skip | Reason |
|---------------|----------------------|--------|
| `**/forge*`, `**/brainstorm*` | 0.1 (bmad-forge-idea) | Ideation already done |
| `**/brief*`, `**/product-brief*` | 1.1 (bmad-product-brief) | Product brief exists |
| `**/research*` | 1.3, 1.4, 1.5 (research steps) | Research already done |
| `**/prd*`, `**/requirements*` | 2.1 (bmad-prd) | PRD exists |
| `**/ux*`, `**/wireframe*` | 2.2 (bmad-ux) | UX design exists |
| `**/DESIGN.md` | 2.5.1 (taste-design-system) | Design system exists |
| `**/brand*` images in design/ | 2.5.2 (taste-brandkit) | Brand kit exists |
| `**/mockup*`, `**/design-ref*` images | 2.5.3 + 2.5.4 (taste-imagegen + taste-image-first) | Design references exist |
| `**/architecture*`, `**/ARCHITECTURE.md` | 3.1 (bmad-architecture) | Architecture exists |
| `**/epic*` | 3.3 (bmad-create-epics-and-stories) | Epics exist |
| `**/readiness*` | 3.4 (bmad-check-implementation-readiness) | Readiness checked |
| Source code files exist | 4.3 (bmad-dev-story) — partially | Code started (sprint may be in progress) |

### Produce a Validate-Still List

Even when an Execute step is skipped, the corresponding validation step should still run (unless evidence shows it already ran):

| Execute Step Skipped | Keep This Validate Step | Unless... |
|---------------------|------------------------|-----------|
| 0.1 (bmad-forge-idea) | 0.2 (/office-hours) | Already CEO-reviewed |
| 1.1 (bmad-product-brief) | 1.2 (/plan-ceo-review) | CEO review artifacts found |
| 2.2 (bmad-ux) | 2.3 (/plan-design-review) | Design review artifacts found |
| 2.5.1 (taste-design-system) | 2.5.5 (/plan-design-review) | Design review already ran |
| 3.1 (bmad-architecture) | 3.2 (/plan-eng-review) | Eng review artifacts found |

### Output Format

```markdown
## Airplan Scanner Results

skip_steps:
  - 0.1 bmad-forge-idea (forge output found at docs/forge.md)
  - 1.1 bmad-product-brief (product brief found at planning-artifacts/brief.md)
  - 2.1 bmad-prd (PRD found at planning-artifacts/prd.md)
  - 3.1 bmad-architecture (architecture found at planning-artifacts/architecture.md)

validate_still:
  - 0.2 /office-hours (idea exists but never stress-tested via GStack)
  - 1.2 /plan-ceo-review (brief exists but never CEO-reviewed)
  - 3.2 /plan-eng-review (architecture exists but never eng-reviewed)

execute_from: 3.3 (first pending required step: bmad-create-epics-and-stories)
```

The flight plan builder (SKILL.md Step 10.2) reads this output to pre-populate the flight plan with the correct statuses.
