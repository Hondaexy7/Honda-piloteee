<div align="center">
  <h1>pilot-honda</h1>
  <p><strong>A Meta-Orchestrator for BMad Method, Taste-Skill, & GStack</strong></p>
  <p>A unified routing layer that detects project maturity and orchestrates three distinct AI development plugins across a single lifecycle.</p>
</div>

---

## Overview

You have three powerful AI development plugins:
- [BMad Method](https://github.com/bmad-code-org/bmad-method) (for architecture, product management, and agile execution)
- [Taste-Skill](https://github.com/Leonxlnx/taste-skill) (for premium frontend design systems and aesthetic direction)
- [GStack](https://github.com/garrytan/gstack) (for diff-level code review, QA, security, and shipping)

**pilot-honda** acts as a traffic controller sitting above all three. Instead of manually deciding which plugin's command to run, you interact with pilot-honda. It diagnoses your project state and routes your intent to the correct plugin based on a unified 8-phase lifecycle model.

It works natively in both **Antigravity IDE** and **Claude Code**.

---

## Core Mechanics

### Tri-Validation (Cross-Plugin Verification)
pilot-honda orchestrates the plugins so they cover each other's blind spots. The core workflow is based on **Tri-Validation**:

1. **BMad Creates:** Generates the PRD, wireframes, architecture, and baseline code.
2. **Taste Designs:** Establishes the visual system, generates reference images, and polishes the frontend code to remove generic AI patterns.
3. **GStack Validates:** Audits the architecture, reviews the code diffs for production bugs, runs QA in a browser, and handles deployment.

No single plugin does everything. pilot-honda ensures you never use BMad for QA, never use GStack for initial product briefs, and never use either for visual design when Taste-Skill is available.

### Features
- **Adaptive Diagnosis:** A branching interview that finds your starting point based on how much documentation or code you already have.
- **Codebase Scanner:** Scans your project for artifacts, CI/CD configs, and **Design Debt** to generate a Project Maturity Report.
- **✈️ Airplan Mode (Autopilot):** Scans the project and builds a dependency-ordered execution plan. It automatically invokes commands across all three plugins in sequence, pausing only at designated Gate checkpoints for your approval.
- **Universal Command Proxy:** Type any command from any of the three plugins (e.g., `/review`, `taste-design-system`, `bmad-architecture`) and pilot-honda will execute the correct methodology natively.
- **Session Memory:** Tracks your progress automatically via a `.pilot-honda-state.md` file.

---

## The Unified Lifecycle

pilot-honda maps the three plugins into an 8-phase execution model:

| Phase | Description | Routed Plugins |
|-------|-------------|-------------------------|
| **0. SPARK** | Raw idea, initial reframing | BMad → GStack |
| **1. ANALYSIS** | Research and product briefs | BMad → GStack |
| **2. PLANNING** | PRD and functional UX wireframes | BMad |
| **2.5. DESIGN ENG.** | Visual direction and design systems | Taste-Skill → GStack |
| **3. SOLUTIONING** | System architecture and epic breakdown | BMad → GStack |
| **4. IMPLEMENTATION**| Code execution, visual polish, and review | BMad → Taste-Skill → GStack |
| **5. SHIPPING** | PR, deployment, and live verification | GStack |
| **6. REFLECTION** | Retrospectives and documentation | BMad → GStack |

*(Note: Phase 2.5 is automatically skipped for backend-only, CLI, or API projects).*

---

## Installation

**Prerequisites:** You must have the [BMad Method](https://github.com/bmad-code-org/bmad-method), [GStack](https://github.com/garrytan/gstack), and [Taste-Skill](https://github.com/Leonxlnx/taste-skill) repositories cloned locally.

Clone this repository:
```bash
git clone https://github.com/Hondaexy7/Honda-piloteee.git pilot-honda
cd pilot-honda
```

### 1. For Antigravity IDE

Create a junction link from the Antigravity plugins directory to your cloned repository:

**(Windows PowerShell):**
```powershell
New-Item -ItemType Junction -Path "$env:USERPROFILE\.gemini\config\plugins\pilot-honda" -Target "C:\path\to\your\cloned\pilot-honda" -Force
```
Restart Antigravity IDE. The plugin will load automatically.

### 2. For Claude Code

Create a junction link from the Claude Code skills directory to the `skills/pilot-honda` folder inside the repository:

**(Windows PowerShell):**
```powershell
$claudePath = "$env:USERPROFILE\.claude\skills"
New-Item -Path $claudePath -ItemType Directory -Force
New-Item -ItemType Junction -Path "$claudePath\pilot-honda" -Target "C:\path\to\your\cloned\pilot-honda\skills\pilot-honda" -Force
```
Restart Claude Code. You will now see `/pilot-honda` available as a slash command.

---

## Usage Guide

To enter the main orchestrator menu, type:
- **In Antigravity:** `"pilot-honda"` or *"what should I do next?"*
- **In Claude Code:** `/pilot-honda`

You will be presented with five options:
- **A) Start something new:** Initiates the adaptive interview to route your project.
- **B) Scan existing project:** Analyzes your codebase and outputs a maturity report.
- **C) Airplan Mode:** Full autopilot across all three plugins.
- **D) Show all commands:** A phase-grouped reference of all 75+ available commands.
- **E) Run a specific command:** Direct execution proxy.

Alternatively, you can bypass the menu entirely by just typing the command you want (e.g., `"Run taste-redesign"` or `"Invoke bmad-prd"`). pilot-honda will automatically intercept it, locate the correct source file, and execute the methodology.

---

## Architecture

The orchestration logic is contained in markdown files, making it completely transparent and editable by the LLM:

```text
pilot-honda/
├── plugin.json                          # Antigravity plugin manifest
├── README.md                            # Documentation
└── skills/
    └── pilot-honda/
        ├── SKILL.md                     # Core routing logic (v3.0.0)
        ├── command-registry.md          # Maps all 75+ commands to source plugins
        ├── phase-map.md                 # Unified Phase 0-6 lifecycle definitions
        ├── scanner-rules.md             # Codebase analysis heuristics
        ├── interview-tree.md            # Adaptive branching for new projects
        └── airplan-pipeline.md          # Master execution pipeline for autopilot
```

---

## Updating

pilot-honda reads the *current* state of your BMad, Taste-Skill, and GStack installations. When those upstream plugins update their methodologies, you do not need to update pilot-honda. Just `git pull` those respective repositories, and pilot-honda will execute their newest instructions automatically.

To update the pilot-honda orchestration rules itself:
```bash
cd path/to/pilot-honda && git pull
```

---

## License

MIT License. See `LICENSE` for more information.
