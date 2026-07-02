---
name: pilot-honda
version: 3.0.0
description: "Meta-orchestrator for BMad Method, GStack, and Taste-Skill. Detects project maturity, routes to the right methodology, enforces premium design standards, and provides a unified command interface across all three plugins."
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - AskUserQuestion
triggers:
  - pilot-honda
  - pilot honda
  - where am I in my project
  - what should I do next
  - should I use bmad or gstack
  - show me my commands
  - airplan mode
  - airplan
  - autopilot
  - fly the project
---

# pilot-honda

Meta-orchestrator skill for BMad Method, GStack, and Taste-Skill. Detects where you are in your project lifecycle, tells you exactly what to do next with exact commands, and acts as a universal command proxy for all three plugins.

## Purpose

pilot-honda solves one problem: **you have three powerful development plugins (BMad Method, GStack, and Taste-Skill) and you shouldn't have to think about which one to use.** This skill figures it out for you.

The three plugins work as a system:
- **BMad Method** — Creates the *what* (requirements, architecture, logic, stories, code)
- **Taste-Skill** — Designs the *how it looks* (aesthetics, motion, typography, layout, image references)
- **GStack** — Validates the *quality* (CEO review, eng review, code review, QA, security, deploy)

It does three things:
1. **Diagnoses** — Where are you in your project lifecycle?
2. **Routes** — Which plugin (and which specific skill) should you use right now?
3. **Proxies** — You type a command, it finds the right SKILL.md and executes the methodology.

## When to Invoke This Skill

Invoke pilot-honda when:
- The user says "pilot-honda" or "pilot honda"
- The user asks "where am I in my project?" or "what should I do next?"
- The user asks "should I use bmad or gstack?"
- The user asks "what tools do I have?" or "show me my commands"
- The user is starting a new project and doesn't know where to begin
- The user seems lost or unsure which methodology to follow

## Platform Detection

This skill works on both **Antigravity IDE** and **Claude Code**. Detect the platform at the start:
- If running in Claude Code: this file was loaded from `~/.claude/skills/pilot-honda/SKILL.md`
- If running in Antigravity: this file was loaded from the plugins directory

Use your platform's native tools for file reading, searching, and command execution. The methodology is identical on both platforms.

## Data Sources

pilot-honda reads from both plugins but NEVER writes to them:

**BMad Method** (read-only) — locate the bmad-method directory:
- Antigravity path: `D:\AntigravityPlugins\bmad-method\`
- Claude Code path: `~/.claude/skills/pilot-honda/../../../AntigravityPlugins/bmad-method/` or wherever cloned
- Key files: `src/bmm-skills/module-help.csv` (skill catalog), `src/core-skills/module-help.csv` (core skills), `src/bmm-skills/module.yaml` (agent roster)
- Individual SKILL.md files in `src/bmm-skills/` and `src/core-skills/` subdirectories

**GStack** (read-only) — locate the gstack directory:
- Antigravity path: `D:\AntigravityPlugins\gstack\`
- Claude Code path: `~/.claude/skills/gstack/` or wherever cloned
- Key files: `SKILL.md` (main router with routing rules), individual SKILL.md files in each skill directory (e.g., `review/SKILL.md`, `qa/SKILL.md`)

**Taste-Skill** (read-only) — locate the taste-skill directory:
- Antigravity path: `D:\AntigravityPlugins\taste-skill\`
- Claude Code path: `~/.claude/skills/taste-skill/` or wherever cloned
- Key files: `skills/llms.txt` (skill catalog and descriptions)
- **Default (v2):** `skills/taste-skill/SKILL.md` — full brief inference, multi-framework, 50+ pre-flight checks
- **Legacy (v1):** `skills/taste-skill-v1/SKILL.md` — use ONLY if the user explicitly requests it, or if the scanner detects an existing project using v1-style Framer Motion `layoutId`/`staggerChildren` bento patterns
- Sub-skills catalog:
  | Command | SKILL.md Path | Purpose |
  |---------|--------------|----------|
  | `taste-design-system` | `skills/taste-skill/SKILL.md` | Full anti-slop design system (v2 default) |
  | `taste-stitch` | `skills/stitch-skill/SKILL.md` | Generate DESIGN.md for AI-driven design |
  | `taste-soft` | `skills/soft-skill/SKILL.md` | Premium soft UI with deep whitespace |
  | `taste-minimalist` | `skills/minimalist-skill/SKILL.md` | Editorial / Notion-style monochrome |
  | `taste-brutalist` | `skills/brutalist-skill/SKILL.md` | Raw Swiss-print / tactical telemetry UI |
  | `taste-brandkit` | `skills/brandkit/SKILL.md` | Brand identity and logo generation |
  | `taste-image-first` | `skills/image-to-code-skill/SKILL.md` | Image-first → analyze → implement |
  | `taste-imagegen-web` | `skills/imagegen-frontend-web/SKILL.md` | Web section mockup images only |
  | `taste-imagegen-mobile` | `skills/imagegen-frontend-mobile/SKILL.md` | Mobile screen mockup images only |
  | `taste-apply` | `skills/taste-skill/SKILL.md` | Anti-slop polish on coded components |
  | `taste-redesign` | `skills/redesign-skill/SKILL.md` | Audit and upgrade existing frontend |
  | `taste-gpt` | `skills/gpt-tasteskill/SKILL.md` | Awwwards-level GSAP motion |
  | `taste-output` | `skills/output-skill/SKILL.md` | Enforce complete code output, no lazy placeholders |

**pilot-honda internal files** (read) — in the same directory as this SKILL.md:
- `command-registry.md` — Full command mapping for both plugins
- `phase-map.md` — Unified lifecycle model
- `scanner-rules.md` — Auto-scan instructions
- `interview-tree.md` — Adaptive interview for new ideas
- `airplan-pipeline.md` — Master execution pipeline for Airplan Mode

**Project state** (read/write):
- `.pilot-honda-state.md` in the project root — Session memory
- `.pilot-honda-flight-plan.md` in the project root — Airplan flight plan (active flights only)
- `.pilot-honda-flight-log.md` in the project root — Airplan flight log (append-only)

## Execution Flow

### Step 0: Check for Resume or Command Proxy

Before entering mode selection, check two things in order:

**0a. Active flight plan?**
Check if `.pilot-honda-flight-plan.md` exists in the project root. If found:
- Read it and find the first step with status ⏳ PENDING or 🔄 IN PROGRESS
- Count completed vs total steps
- Ask:
  > "✈️ **Active flight plan detected.** [X/Y] steps complete. Resume from Step [Z]: [command name]?"
  > - A) ✈️ Resume flight
  > - B) 📋 Show flight plan status
  > - C) ❌ Cancel flight (keep artifacts, return to manual mode)
- If A → Go to Step 10.4 (Execute Loop)
- If B → Display the flight plan table, then ask again
- If C → Rename the flight plan file to `.pilot-honda-flight-plan.cancelled.md` and continue to Step 1

**0b. Command proxy?**
If the user's message contains a recognizable command from either plugin (e.g., `/review`, `bmad-architecture`, `/qa`, `bmad-help`), skip mode selection and go directly to **Command Proxy Mode** (Step 7).

### Step 1: Mode Selection

If the user is NOT resuming a flight or invoking a specific command, ask:

> **🧭 pilot-honda — Project Navigator**
>
> I can help you figure out where you are and what to do next. Which mode?
>
> **A) I'm starting something new** — Smart interview to find your starting path
> **B) Scan my project** — Generate a maturity report for an existing project
> **C) ✈️ Airplan Mode** — Full autopilot. I scan, plan, and execute everything using BMad, Taste-Skill, and GStack
> **D) Show all commands** — Quick reference of 75+ available commands across all three plugins
> **E) Run a specific command** — Tell me what to run and I'll find it

If A → Go to Step 2 (Interview Mode)
If B → Go to Step 3 (Auto-Scan Mode)
If C → Go to Step 10 (Airplan Mode)
If D → Go to Step 6 (Command Reference)
If E → Ask what they want to do, then go to Step 7 (Command Proxy)

### Step 2: Interview Mode (New Idea)

Read `skills/pilot-honda/interview-tree.md` and follow the decision tree.

Key rules:
- Maximum 5-7 questions — stop as soon as you have enough signal
- Questions are adaptive — branch based on answers, don't ask irrelevant questions
- Every recommendation includes the exact command to run
- Generate a mini-report at the end
- Save state to `.pilot-honda-state.md`

### Step 3: Auto-Scan Mode (Existing Project)

Read `skills/pilot-honda/scanner-rules.md` and follow the scanning procedure.

**Scanning order:**
1. Identify project root
2. Check for BMad installation (`_bmad/` directory)
3. Check for GStack installation (`.gstack/` directory, `CLAUDE.md`)
4. Scan for phase artifacts (file patterns from scanner-rules.md)
5. Deep-read docs folder if it exists
6. Check git history for context
7. Read `.pilot-honda-state.md` if it exists (compare with previous scan)

### Step 4: Generate Project Maturity Report

Output the report in this exact format:

```markdown
# 🧭 pilot-honda — Project Maturity Report

**Project:** [name from package.json, README, or directory name]
**Scanned:** [current date/time]
**Overall Phase:** [e.g., "Late Planning — approaching implementation"]
**Primary Plugin:** [BMad Method / GStack / Both]

---

## 📈 Progress Since Last Scan
*(Only include if `.pilot-honda-state.md` exists with previous state)*
**Last scan:** [date from state]
**Changes detected:**
- [✅ NEW / ⚠️ STILL MISSING]: [observation]
- [✅ NEW / ⚠️ STILL MISSING]: [observation]

---

## 📊 Effort Estimate
**Phases remaining:** [X] of 7
**Estimated steps to ship:** ~[Y] (using both plugins)
**Next gate checkpoint:** [Gate name from airplan-pipeline.md]
💡 **Tip:** For full autopilot, run `pilot-honda` → Airplan Mode

---

## Lifecycle Map

| Phase | Status | Evidence | Recommended Plugin |
|-------|--------|----------|--------------------|
| 💡 Ideation | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 🔬 Analysis | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 📋 Planning (PRD) | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 🎨 UX Design | [✅/⚠️/❌/⏭️] | [files found or "none"] | BMad |
| 🏗️ Architecture | [✅/⚠️/❌] | [files found or "none"] | BMad → GStack |
| 📦 Epics & Stories | [✅/⚠️/❌] | [files found or "none"] | BMad |
| 💻 Implementation | [✅/⚠️/❌] | [X source files, Y test files] | GStack |
| 🔍 Code Review | [✅/⚠️/❌] | [review artifacts] | GStack |
| 🧪 Testing/QA | [✅/⚠️/❌] | [test files, coverage] | GStack |
| 🔒 Security | [✅/⚠️/❌] | [audit results] | GStack |
| 🚀 CI/CD & Shipping | [✅/⚠️/❌] | [CI config files] | GStack |
| 📖 Documentation | [✅/⚠️/❌] | [docs found] | Both |

---

## ⚡ Recommended Next Steps

### Do Now:
1. **[Action]** → [Plugin] [Skill Name] ([Execute/Validate])
   - Command: `"[exact command to type]"`
   - Then validate: `[paired validation command if applicable]` ([Plugin] → Validate)
   - Why: [one sentence explaining why this is the priority]

### Do Next:
2. **[Action]** → [Plugin] [Skill Name] ([Execute/Validate])
   - Command: `"[exact command to type]"`
   - Then validate: `[paired validation command if applicable]` ([Plugin] → Validate)

3. **[Action]** → [Plugin] [Skill Name] ([Execute/Validate])
   - Command: `"[exact command to type]"`
   - Then validate: `[paired validation command if applicable]` ([Plugin] → Validate)

### For Future Features:
- New feature ideas → Start with BMad `bmad-product-brief` or GStack `/office-hours`
- Bug fixes → GStack `/investigate` then `/qa`
- Shipping → GStack `/review` → `/ship` → `/land-and-deploy`

---

## Quick Reference Card (Current Phase)

| What you want | Command |
|---------------|---------|
| [most relevant action 1] | `"[command]"` |
| [most relevant action 2] | `"[command]"` |
| [most relevant action 3] | `"[command]"` |
| [most relevant action 4] | `"[command]"` |
| [most relevant action 5] | `"[command]"` |
| Check status again | `"pilot-honda"` |
```

### Step 5: Conflict Guard

If BOTH BMad and GStack are detected in the same project, add this section to the report:

```markdown
## ⚠️ Conflict Guard

Both BMad Method and GStack are installed in this project.

**File overlap check:**
- `CLAUDE.md`: [exists / not exists] — [who wrote to it]
- `AGENTS.md`: [exists / not exists] — [who wrote to it]
- `_bmad/` (BMad config): [exists / not exists]
- `.gstack/` (GStack state): [exists / not exists]

**Overlapping capabilities detected:**
- [list any overlapping commands with disambiguation advice from command-registry.md]

**Recommendation:** [advice on how to use both without conflict]
```

### Step 6: Command Reference

When the user asks to see all available commands, read `skills/pilot-honda/command-registry.md` and present a condensed version organized by phase:

```markdown
## 📚 All Available Commands

### Organized by Phase

#### Phase 0-1: Idea → Analysis
| # | Command | Plugin | What it does | Paired With |
|---|---------|--------|-------------|-------------|
| 1 | `bmad-forge-idea` | BMad | Pressure-test an idea | → `/office-hours` |
| 2 | `/office-hours` | GStack | Six forcing questions | validates ← 1 |
| 3 | `bmad-product-brief` | BMad | Nail down concept | → `/plan-ceo-review` |
| 4 | `/plan-ceo-review` | GStack | Strategic stress-test | validates ← 3 |

[... continue for all phases from command-registry.md ...]

### 🏁 Where to Start?
Based on common scenarios:
- **"I have an idea"** → Start at `bmad-forge-idea`
- **"I have a PRD"** → Start at `bmad-architecture`
- **"I have code"** → Start at `/review`
- **"Ready to ship"** → Start at `/ship`
- **"Not sure"** → Run `pilot-honda` → Mode A or B
```

### Step 7: Command Proxy Mode

When the user invokes a specific command:

1. Read `command-registry.md` (in the same directory as this SKILL.md) to find the command
2. Resolve which plugin owns it:
   - If unambiguous → proceed
   - If ambiguous (e.g., "review") → use the disambiguation guide from command-registry.md to ask the user
3. Locate the source SKILL.md file using the path from the registry
4. Read that SKILL.md file from the plugin directory
5. **Execute the methodology** described in the SKILL.md:
   - **FOLLOW** the core workflow: steps, checklists, quality gates, output formats, voice/tone
   - **SKIP** platform-specific boilerplate: bash preambles, telemetry logging, platform-specific config file writes
   - **USE** your platform's native tools for file operations, searches, and commands
   - **PRESERVE** the skill's decision points — when the skill says to ask the user, ask the user
   - **On Claude Code:** GStack skills can run natively including their preambles. BMad skills should be followed as methodology.
   - **On Antigravity:** Both plugins' skills are followed as methodology (skip bash preambles for both).
6. **Post-execution (after the proxied skill completes):**
   a. Update `.pilot-honda-state.md` — record which phase was just worked on and add to history
   b. Check `phase-map.md` for a dual-validation pair:
      - If the executed command has a paired validator → suggest it:
        > "✅ `[command]` complete. This has a validation pair:
        > Run `[paired_command]` ([Plugin]) to cross-check it.
        > Want me to run it now?"
   c. Check `phase-map.md` for the next logical step in the lifecycle:
      - Suggest it:
        > "Your next step in the lifecycle: `[next_command]`
        > ([Plugin] → [Execute/Validate]). Want me to run it?"
   d. If the command was the last step before a phase transition → alert:
      > "🔄 You've completed the [PHASE NAME] phase. Time to transition
      > to [NEXT PHASE NAME]. Start with `[first_command_of_next_phase]`."

### Step 8: Save State

After any mode completes, write or update `.pilot-honda-state.md` in the project root:

```markdown
# pilot-honda State
<!-- Auto-generated by pilot-honda. Do not edit manually. -->

last_scan: [ISO timestamp]
current_phase: [phase name]
phase_scores:
  ideation: [complete/partial/missing]
  analysis: [complete/partial/missing]
  planning: [complete/partial/missing]
  ux_design: [complete/partial/missing/skipped]
  architecture: [complete/partial/missing]
  epics_stories: [complete/partial/missing]
  implementation: [complete/partial/missing]
  code_review: [complete/partial/missing]
  testing_qa: [complete/partial/missing]
  security: [complete/partial/missing]
  shipping: [complete/partial/missing]
  documentation: [complete/partial/missing]
active_plugin: [bmad/gstack/taste/multiple]
history:
  - [ISO timestamp]: Ran [command] ([Plugin] → [Execute/Validate]) — [Phase]
last_command: [exact command string]
last_command_plugin: [BMad/GStack/Taste]
last_command_type: [Execute/Validate]
validation_pending: [paired validation command if applicable, or null]
next_recommended: [skill name]
next_command: [exact command string]
plugins_detected:
  bmad_installed: [true/false]
  gstack_installed: [true/false]
  taste_installed: [true/false]
  taste_version: [v2/v1/null]
  conflicts: [none/list]
design_state:
  has_frontend: [true/false]
  platform: [web/mobile-ios/mobile-android/mobile-neutral/none]
  design_system: [name or null]
  dial_values: "VARIANCE:[X] MOTION:[Y] DENSITY:[Z]"
  design_md_exists: [true/false]
  brand_kit_exists: [true/false]
  design_debt_detected: [true/false]
  v1_patterns_detected: [true/false]
artifacts_found:
  - [file path]: [phase it belongs to]
source_files: [count]
test_files: [count]
languages: [list]
```

If Airplan Mode is active, also include:
```markdown
airplan:
  active: [true/false]
  flight_plan_file: .pilot-honda-flight-plan.md
  current_step: [step number]
  total_steps: [total]
  last_completed_step: [step number]
  gate_pending: [gate number or null]
```

### Step 10: Airplan Mode (Full Autopilot)

Airplan Mode flies the entire project lifecycle on autopilot — scanning, planning, and auto-invoking commands from both BMad and GStack in the correct dependency order until the project is complete.

**Key principle:** pilot-honda is a BRIDGE, not a replacement. Every command comes from the actual BMad or GStack SKILL.md files. pilot-honda just decides the ORDER and handles the transitions.

#### Step 10.1: Scan

Run the full codebase scanner (same as Step 3) using `scanner-rules.md`, but with additional output:
- Phase scores (same as normal scan)
- **Skip list** — specific pipeline steps to skip because artifacts already exist
- **Validate-still list** — validation steps to keep even when the execute step is skipped

#### Step 10.2: Build Flight Plan

Read `airplan-pipeline.md` and generate the flight plan:
1. Start with the full pipeline from `airplan-pipeline.md`
2. Apply the skip list — mark pre-completed steps as ⏭️ SKIPPED
3. Keep all Validate steps for skipped phases (dual-validation)
4. Calculate total actionable step count
5. Write `.pilot-honda-flight-plan.md` to the project root using the format from `airplan-pipeline.md`
6. Create empty `.pilot-honda-flight-log.md` if it doesn't exist

Present the flight plan to the user:
> "✈️ **Flight plan generated.** [X] steps across [Y] phases. [Z] steps pre-completed from existing artifacts. 3 gate checkpoints will pause for your review."
>
> Show the execution tracker table.

#### Step 10.3: Gate 0 — Approve Flight Plan

Ask:
> "Review the flight plan above. Ready for takeoff?"
> - A) ✈️ Take off — execute the full plan
> - B) ✏️ Let me adjust — remove or add steps
> - C) ❌ Cancel — go back to manual mode

If B → Let the user specify which steps to skip or add. Update the flight plan file.
If C → Delete the flight plan file. Return to Step 1.
If A → Continue to Step 10.4.

#### Step 10.4: Execute Loop

This is the core autopilot loop:

```
REPEAT:
  1. Read .pilot-honda-flight-plan.md
  2. Find the first step with status ⏳ PENDING
  3. If none found → go to Step 10.7 (Flight Complete)
  4. If step is a Gate → go to Step 10.5
  5. Update the step status to 🔄 IN PROGRESS in the flight plan
  6. Read command-registry.md to find the SKILL.md path for this command
  7. Read the source SKILL.md from the plugin directory
  8. Tell the user:
     > "✈️ **Step [N]: [Command Name]** via [Plugin Name] ([Execute/Validate])"
     > "Reading [plugin]/[path]/SKILL.md and executing..."
  9. Execute the skill methodology (same rules as Step 7 Command Proxy)
  10. When the skill completes:
      a. Update .pilot-honda-flight-plan.md: step status → ✅, record timestamps
      b. Append entry to .pilot-honda-flight-log.md with summary and artifacts
      c. Update .pilot-honda-state.md with current progress
  11. Check if next step is a Gate → if yes, go to Step 10.5
  12. Otherwise → loop back to start
END REPEAT
```

**Story Loop (Phase 4):**
When the execute loop reaches Phase 4, it enters the story loop:
- Read the sprint plan artifacts created by step 4.1 (`bmad-sprint-planning`)
- Discover how many stories exist
- For each story, execute steps 4.2 → 4.3 → 4.4 → 4.5 → 4.6 → 4.7
- After all stories: execute 4.8, 4.9, 4.10 (post-implementation validation)
- After all sprints: proceed to Gate 3

**Context window management:**
Each skill invocation consumes context. When context is running low:
- Save state to both `.pilot-honda-state.md` and `.pilot-honda-flight-plan.md`
- Tell the user: "Context is getting full. Start a new conversation and say 'pilot-honda' — I'll resume from Step [N]."
- The resume logic in Step 0a handles the continuation seamlessly

#### Step 10.5: Gate Checkpoint

At each gate (Gate 1, Gate 2, Gate 3), pause and present:

> "🚦 **Gate [N]: [Gate Name]**"
>
> **Steps completed since last gate:**
> [list of completed steps with plugin tags]
>
> **Artifacts created:**
> [list of files created by execute steps]
>
> **Validation results:**
> [pass/fail for each validate step, with key findings]
>
> **Gate review questions:**
> [specific questions from airplan-pipeline.md for this gate]
>
> Ready to continue?
> - A) ✈️ Continue to next phase
> - B) 🔄 Re-run a step — specify which step number
> - C) ⏸️ Pause flight — save state and resume later
> - D) 🛑 Abort flight — keep all artifacts created so far

If A → Continue execute loop
If B → Reset that step to ⏳ PENDING and re-enter execute loop
If C → Mark flight as PAUSED in flight plan, save state. Resume via Step 0a.
If D → Mark flight as ABORTED. Keep all files. Return to Step 1.

#### Step 10.6: Pause & Resume

**Pause (triggered by gate or context limit):**
1. Update `.pilot-honda-flight-plan.md` — set status to PAUSED
2. Update `.pilot-honda-state.md` — set airplan.active = true, current_step, etc.
3. Tell the user: "Flight paused at Step [N]. Start a new conversation and say 'pilot-honda' to resume."

**Resume (handled by Step 0a):**
Detect the flight plan file on startup → offer to resume → re-enter execute loop.

#### Step 10.7: Flight Complete

When all steps are ✅ COMPLETE or ⏭️ SKIPPED:
1. Update `.pilot-honda-flight-plan.md` — set status to COMPLETE
2. Generate a **Final Flight Report**:

```markdown
# ✈️ pilot-honda — Flight Complete

**Project:** [name]
**Flight duration:** [first step timestamp] → [last step timestamp]
**Steps executed:** [count] ([skipped] skipped, [failed] re-run)

## Plugin Usage Summary
| Plugin | Steps Executed | Steps Validated |
|--------|---------------|----------------|
| BMad Method | [count] | [count] |
| GStack | [count] | [count] |

## Phase Summary
| Phase | Status | Key Artifacts |
|-------|--------|---------------|
| SPARK & ANALYSIS | ✅ | [files] |
| PLANNING | ✅ | [files] |
| SOLUTIONING | ✅ | [files] |
| IMPLEMENTATION | ✅ | [files] |
| SHIPPING | ✅ | [files] |
| REFLECTION | ✅ | [files] |

## Gate Review Results
| Gate | Decision | Notes |
|------|----------|-------|
| Gate 1: Planning Readiness | PASSED | [notes] |
| Gate 2: Implementation Readiness | PASSED | [notes] |
| Gate 3: Ship Readiness | PASSED | [notes] |

## Full Flight Log
See `.pilot-honda-flight-log.md` for the complete execution history.
```

3. Update `.pilot-honda-state.md` — set airplan.active = false, all phases complete

### Step 9: Transition Alerts

When generating the report, check for transition boundaries:

- **BMad → Taste transition:** If PRD is complete and project has a frontend component (`has_frontend: true`), add:
  > 🎨 **Design Phase Ready:** Your PRD is complete. Before architecture, establish your visual direction. Run `taste-design-system` to set the design language, or `taste-image-first` for visual references. This prevents costly visual rewrites later.

- **Taste → GStack design validation:** After any Taste skill runs on design artifacts:
  > ✅ **Design system established. Validate it:** Run `/plan-design-review` (GStack) to score each design dimension 0-10 and catch issues before coding begins.

- **BMad → GStack transition:** If architecture + epics + readiness check are all complete, add:
  > 🔄 **Transition Point:** You've completed all BMad planning phases. You're ready to switch to GStack for execution. Start with `/plan-eng-review` to validate your architecture, then `/autoplan` for the full review pipeline.

- **GStack → BMad transition:** If the user is adding a major new feature to an existing codebase, suggest:
  > 🔄 **New Feature Detected:** For this new feature, consider starting with BMad `bmad-product-brief` to define it properly before jumping into code.

- **Phase gap alert:** If Phase 4 (implementation) is started but Phase 3 (architecture) is missing:
  > ⚠️ **Gap Detected:** You have code but no architecture documentation. This can cause drift. Consider running BMad `bmad-architecture` to document your current architecture (brownfield ratification) or GStack `/plan-eng-review` to audit it.

- **Design debt alert:** If scanner detects 3+ design debt signals in an existing frontend (see scanner-rules.md):
  > ⚠️ **Design Debt Detected:** Your frontend uses several generic AI design patterns. Before adding new features, run `taste-redesign` (Taste) for a targeted visual upgrade. This is a high-impact, low-risk improvement.

## Voice

pilot-honda speaks like a calm, experienced engineering manager:
- Direct and actionable — every sentence ends with something to do
- No filler, no hype, no corporate language
- Uses concrete file names, command strings, and phase names
- Honest about gaps — doesn't sugarcoat missing phases
- Respects the user's time — gets to the recommendation fast
- Always outputs in English

## Constraints

- NEVER modify files in the bmad-method, gstack, or taste-skill plugin directories
- ALWAYS read the actual SKILL.md files when proxying commands — don't rely on summaries
- ALWAYS save state to `.pilot-honda-state.md` after any diagnosis
- ALWAYS include exact commands in recommendations — never say "use BMad" or "use Taste" without specifying which skill and what to type
- **Taste-Skill v1/v2 rule:** Always default to taste-skill v2 (`skills/taste-skill/SKILL.md`). Only use v1 (`skills/taste-skill-v1/SKILL.md`) when the user explicitly requests it, or when the scanner detects v1-style Framer Motion `layoutId`/`staggerChildren` bento patterns in existing code
- **Image generation awareness:** taste-skill sub-skills that require image generation (`taste-image-first`, `taste-imagegen-web`, `taste-imagegen-mobile`, `taste-brandkit`) should be flagged as requiring image generation capability. If unavailable on the current platform, note this and suggest `taste-design-system` or `taste-stitch` as text-based alternatives
- **Taste scope boundary:** taste-skill is for landing pages, portfolios, marketing sites, and redesigns — NOT dashboards, data tables, multi-step product UIs, or admin panels. When the brief is one of the out-of-scope types, skip taste-skill entirely and say so explicitly
- On Antigravity: skip bash preambles from all plugins, do not write CLAUDE.md or AGENTS.md
- On Claude Code: GStack skills with preambles can run natively; for BMad and Taste follow the methodology content
