<div align="center">
  <h1>🧭 pilot-honda</h1>
  <p><strong>A Meta-Orchestrator Skill for BMad Method & GStack</strong></p>
  <p>One skill to rule them all. Detect project maturity, route to the right methodology, and proxy commands seamlessly across platforms.</p>
</div>

---

## 📑 Table of Contents
- [What is pilot-honda?](#-what-is-pilot-honda)
- [Key Features](#-key-features)
- [Installation](#-installation)
  - [For Antigravity IDE](#1-for-antigravity-ide)
  - [For Claude Code](#2-for-claude-code)
- [How to Use](#-how-to-use)
  - [Project Navigator (Diagnosis)](#1-project-navigator-diagnosis)
  - [Universal Command Proxy](#2-universal-command-proxy)
- [Under the Hood: The Unified Lifecycle](#-under-the-hood-the-unified-lifecycle)
- [Architecture & File Structure](#-architecture--file-structure)
- [Updating](#-updating)
- [License](#-license)

---

## 🤔 What is pilot-honda?

You have two powerful development methodologies: [BMad Method](https://github.com/bmad-code-org/bmad-method) (incredible for agile planning and architecture) and [GStack](https://github.com/garrytan/gstack) (incredible for sprint execution, reviews, and shipping). 

**pilot-honda solves one problem: you shouldn't have to think about which one to use.**

This custom skill sits above both plugins, acting as a single intelligent entry point. It figures out where you are in your project lifecycle and routes you to the exact command you need from the right methodology.

It works natively in both **Antigravity IDE** and **Claude Code**.

---

## ✨ Key Features

- **🧠 Adaptive Diagnosis:** Tell it about a new idea, and it runs a smart, branching interview (max 5-7 questions) to find your starting point.
- **🔍 Auto-Scanner:** Point it at an existing project, and it scans your artifacts (PRDs, tests, code, CI/CD) to generate a detailed Project Maturity Report.
- **🔀 Universal Command Proxy:** Type any command from either plugin (e.g., `/review`, `bmad-architecture`, `/qa`) and pilot-honda will execute the correct methodology natively.
- **💾 Session Memory:** Remembers your project state between sessions via a `.pilot-honda-state.md` file, tracking your progress automatically.
- **🛡️ Conflict Guard:** Safely manages environments where both BMad and GStack are installed, preventing overlapping actions.
- **🌐 Cross-Platform:** 100% compatible with both Antigravity IDE and Claude Code from a single source of truth.

---

## 🚀 Installation

pilot-honda is designed to be cloned once and symlinked to your AI assistants.

**Prerequisites:** You must have [BMad Method](https://github.com/bmad-code-org/bmad-method) and [GStack](https://github.com/garrytan/gstack) installed on your machine.

First, clone this repository:
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

Restart Antigravity IDE, and the plugin will load automatically.

### 2. For Claude Code

Create a junction link from the Claude Code skills directory to the `skills/pilot-honda` folder inside the repository (Claude Code requires the SKILL.md to be at the root of the linked folder):

**(Windows PowerShell):**
```powershell
$claudePath = "$env:USERPROFILE\.claude\skills"
New-Item -Path $claudePath -ItemType Directory -Force
New-Item -ItemType Junction -Path "$claudePath\pilot-honda" -Target "C:\path\to\your\cloned\pilot-honda\skills\pilot-honda" -Force
```

Restart Claude Code. You will now see `/pilot-honda` when you type `/`.

---

## 🛠️ How to Use

### 1. Project Navigator (Diagnosis)

When you're lost or starting out, ask pilot-honda for directions:
- **In Antigravity:** Type `"pilot-honda"` or ask *"what should I do next?"*
- **In Claude Code:** Type `/pilot-honda`

It will offer you modes:
- **Start something new:** A quick interview to route your raw idea.
- **Scan existing project:** A deep analysis of your codebase maturity.
- **Show all commands:** A cheat sheet of the 60+ commands across both methodologies.

### 2. Universal Command Proxy

You don't need to invoke pilot-honda directly if you know what you want. Just type the command!

Examples:
- `"Run /office-hours"` → Executes GStack's product reframing
- `"Invoke bmad-architecture"` → Executes BMad's system design agent
- `"Run /review"` → Executes GStack's diff-level staff engineer review
- `"Run bmad-code-review"` → Executes BMad's story-level acceptance criteria review

pilot-honda will intercept the request, look up the command in its registry, locate the specific `SKILL.md` in your local BMad or GStack installation, and execute the exact methodology.

---

## 🗺️ Under the Hood: The Unified Lifecycle

pilot-honda seamlessly maps BMad's 4-phase agile model to GStack's sprint cycle:

| Phase | Description | Recommended Methodology |
|-------|-------------|-------------------------|
| **Phase 0: SPARK** | Raw idea, no documentation | BMad (brainstorming) / GStack (`/office-hours`) |
| **Phase 1: ANALYSIS** | Research, validation, briefs | BMad (PRFAQ, product-brief) |
| **Phase 2: PLANNING** | PRD, UX design, requirements | BMad (PRD) / GStack (`/plan-design-review`) |
| **Phase 3: SOLUTIONING** | Architecture, epics, stories | BMad (Architecture) → GStack (`/plan-eng-review`) |
| **Phase 4: IMPLEMENTATION**| Code, tests, reviews, QA | Both (BMad for story execution, GStack for QA/review) |
| **Phase 5: SHIPPING** | PR, deploy, verification | GStack (`/ship`, `/land-and-deploy`) |
| **Phase 6: REFLECTION** | Retrospectives, documentation | Both (BMad retros, GStack `/retro`) |

---

## 🏗️ Architecture & File Structure

```text
pilot-honda/
├── plugin.json                          # Antigravity plugin manifest
├── README.md                            # This comprehensive guide
└── skills/
    └── pilot-honda/
        ├── SKILL.md                     # Core brain (Claude Code & Antigravity logic)
        ├── command-registry.md          # Maps all 60+ commands to source plugins
        ├── phase-map.md                 # Unified Phase 0-6 lifecycle model
        ├── scanner-rules.md             # Heuristics for auto-detecting maturity
        └── interview-tree.md            # Adaptive decision tree for new ideas
```

The core `SKILL.md` contains YAML frontmatter for Claude Code slash-command registration, but relies on platform-agnostic heuristics to function perfectly in Antigravity as well.

---

## 🔄 Updating

Because pilot-honda is a meta-orchestrator, it reads the *current* state of your BMad and GStack installations. 

To update the core methodologies, just pull their repos:
```bash
cd path/to/bmad-method && git pull
cd path/to/gstack && git pull
```
pilot-honda will automatically pick up their changes.

To update pilot-honda itself:
```bash
cd path/to/pilot-honda && git pull
```

---

## 📜 License

MIT License. See `LICENSE` for more information.
