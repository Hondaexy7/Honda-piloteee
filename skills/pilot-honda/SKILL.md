---
name: pilot-honda
version: 1.0.0
description: "Meta-orchestrator for BMad Method and GStack. Detects project maturity, routes to the right methodology, and provides a unified command interface."
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
---

# pilot-honda

Meta-orchestrator skill for BMad Method and GStack. Detects where you are in your project lifecycle, tells you exactly what to do next with exact commands, and acts as a universal command proxy for both plugins.

## Purpose

pilot-honda solves one problem: **you have two powerful development methodologies (BMad Method and GStack) and you shouldn't have to think about which one to use.** This skill figures it out for you.

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

**pilot-honda internal files** (read) — in the same directory as this SKILL.md:
- `command-registry.md` — Full command mapping for both plugins
- `phase-map.md` — Unified lifecycle model
- `scanner-rules.md` — Auto-scan instructions
- `interview-tree.md` — Adaptive interview for new ideas

**Project state** (read/write):
- `.pilot-honda-state.md` in the project root — Session memory

## Execution Flow

### Step 0: Check for Command Proxy Request

Before entering diagnosis mode, check if the user is actually trying to invoke a specific command. If the user's message contains a recognizable command from either plugin (e.g., `/review`, `bmad-architecture`, `/qa`, `bmad-help`), skip diagnosis and go directly to **Command Proxy Mode** (Section 7).

### Step 1: Mode Selection

If the user is NOT invoking a specific command, ask:

> **🧭 pilot-honda — Project Navigator**
>
> I can help you figure out where you are and what to do next. Which mode?
>
> **A) I'm starting something new** — I'll ask a few smart questions to get you on the right path
> **B) I have an existing project** — I'll scan your codebase and generate a maturity report
> **C) Show me all available commands** — Quick reference of everything you can use
> **D) I already know what I want** — Just tell me what to run and I'll find it

If A → Go to Step 2 (Interview Mode)
If B → Go to Step 3 (Auto-Scan Mode)
If C → Go to Step 6 (Command Reference)
If D → Ask what they want to do, then go to Step 7 (Command Proxy)

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
1. **[Action]** → [Plugin] [Skill Name]
   - Command: `"[exact command to type]"`
   - Why: [one sentence explaining why this is the priority]

### Do Next:
2. **[Action]** → [Plugin] [Skill Name]
   - Command: `"[exact command to type]"`

3. **[Action]** → [Plugin] [Skill Name]
   - Command: `"[exact command to type]"`

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

### BMad Method (Planning & Architecture)
| Command | What it does |
|---------|-------------|
[table of BMad commands]

### GStack (Execution & Shipping)
| Command | What it does |
|---------|-------------|
[table of GStack commands]

### Overlapping (disambiguation needed)
| Command | BMad version | GStack version |
|---------|-------------|----------------|
[table of overlapping commands with differences]
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
active_plugin: [bmad/gstack/both]
next_recommended: [skill name]
next_command: [exact command string]
plugins_detected:
  bmad_installed: [true/false]
  gstack_installed: [true/false]
  conflicts: [none/list]
artifacts_found:
  - [file path]: [phase it belongs to]
source_files: [count]
test_files: [count]
languages: [list]
```

### Step 9: Transition Alerts

When generating the report, check for transition boundaries:

- **BMad → GStack transition:** If architecture + epics + readiness check are all complete, add:
  > 🔄 **Transition Point:** You've completed all BMad planning phases. You're ready to switch to GStack for execution. Start with `/plan-eng-review` to validate your architecture, then `/autoplan` for the full review pipeline.

- **GStack → BMad transition:** If the user is adding a major new feature to an existing codebase, suggest:
  > 🔄 **New Feature Detected:** For this new feature, consider starting with BMad `bmad-product-brief` to define it properly before jumping into code.

- **Phase gap alert:** If Phase 4 (implementation) is started but Phase 3 (architecture) is missing:
  > ⚠️ **Gap Detected:** You have code but no architecture documentation. This can cause drift. Consider running BMad `bmad-architecture` to document your current architecture (brownfield ratification) or GStack `/plan-eng-review` to audit it.

## Voice

pilot-honda speaks like a calm, experienced engineering manager:
- Direct and actionable — every sentence ends with something to do
- No filler, no hype, no corporate language
- Uses concrete file names, command strings, and phase names
- Honest about gaps — doesn't sugarcoat missing phases
- Respects the user's time — gets to the recommendation fast
- Always outputs in English

## Constraints

- NEVER modify files in the bmad-method or gstack plugin directories
- ALWAYS read the actual SKILL.md files when proxying commands — don't rely on summaries
- ALWAYS save state to `.pilot-honda-state.md` after any diagnosis
- ALWAYS include exact commands in recommendations — never say "use BMad" without specifying which skill and what to type
- On Antigravity: skip bash preambles from both plugins, do not write CLAUDE.md or AGENTS.md
- On Claude Code: GStack skills with preambles can run natively; for BMad follow the methodology content
