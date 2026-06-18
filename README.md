<div align="center">

# ⚒️ Techneering

### Engineering, with ancient discipline.

**Write the spec. Strike while it's hot. Ship with confidence.**

[![license](https://img.shields.io/badge/license-MIT-black?style=flat-square)](LICENSE)
[![skills](https://img.shields.io/badge/skills-16-orange?style=flat-square)](#the-16-skills)
[![platform](https://img.shields.io/badge/platform-Win%20%7C%20mac%20%7C%20Linux-555?style=flat-square)](#requirements)
[![claude](https://img.shields.io/badge/Claude%20Code-plugin-blueviolet?style=flat-square)](#quick-start)

`tn:` · a Claude Code skills plugin

</div>

---

> **The spec is the contract. The harness is the discipline.** Techneering fuses the four layers AI tooling has evolved through — **Prompt** (writing each instruction well), **Context** (managing what the AI sees), **Harness** (constraining the agent's behavior), **Loop** (iterating to quality) — into a single harness that wraps the model and turns AI coding from guesswork into craft. Sixteen skills walk a change from idea, through spec and code, to a verified, archived release. The spec keeps the AI pointed at the right target; the harness keeps it from drifting off course. Every line of code traces back to a requirement; nothing ships on a hunch.

### AI tooling has evolved through four layers. Techneering was built for all of them.

```
Prompt  Engineering   →  how to write a single instruction well
                          └─ every SKILL.md is a carefully designed prompt
    ↓
Context Engineering   →  how to manage what the AI sees
                          └─ the workspace: files carry state, not chat memory
    ↓
Harness Engineering   →  how to constrain the agent's behavior
                          └─ the 16-skill scaffold (spec = contract, harness = discipline)
    ↓
Loop Engineering      →  how to get the AI to iterate to quality
                          └─ assemble → redgreen → gate → audit → inspect → ship → vault
```

Most tools stop at one layer. Techneering sits at the intersection of all four — including the parts loop systems usually skip: explicit **failure limits**, **generation ≠ evaluation**, and a **depth dial** so trivial changes skip the full ceremony. See [Design Philosophy](#design-philosophy) for the principles behind each layer.

---

## The 16 Skills

The names follow a **forge workshop**: the spec is raw material, the code is the forged product, and each skill is a station on the shop floor.

### 🧭 The Smithy — core flow

| Skill | Call | What it does |
|-------|------|--------------|
| **compass** | `tn:compass` | Gateway. Auto-loaded at session start — picks the right path |
| **scout** | `tn:scout` | Think out loud. Explore ideas, write no files |
| **draft** | `tn:draft` | Generate proposal + specs + design + tasks in one pass |
| **forge** | `tn:forge` | Implement. Chains blueprint → assemble internally |
| **audit** | `tn:audit` | Dual-layer check: code↔spec + code↔real requirements |
| **vault** | `tn:vault` | Merge delta specs into main specs, archive the change |

### 🧠 The Drawing Board — thinking & planning

| Skill | Call | What it does |
|-------|------|--------------|
| **spark** | `tn:spark` | Brainstorm design, detect coding style, recommend frameworks |
| **blueprint** | `tn:blueprint` | Break the spec into bite-sized, testable tasks |

### 🔨 The Anvil — execution

| Skill | Call | What it does |
|-------|------|--------------|
| **assemble** | `tn:assemble` | Dispatch one subagent per task, two-stage review each |
| **redgreen** | `tn:redgreen` | Test-driven: red (failing test) → green (minimal code) → refactor |
| **diagnose** | `tn:diagnose` | Systematic debugging: root cause before any fix |

### 🔍 The Inspection Bench — quality gates

| Skill | Call | What it does |
|-------|------|--------------|
| **gate** | `tn:gate` | Iron law: run the command, read the output, *then* claim it works |
| **inspect** | `tn:inspect` | Dispatch a code-reviewer subagent for global review |

### 📦 The Shipping Dock — utilities

| Skill | Call | What it does |
|-------|------|--------------|
| **isolate** | `tn:isolate` | Create an isolated git worktree, verify the test baseline |
| **ship** | `tn:ship` | Branch integration: merge / PR / keep / discard |
| **craft** | `tn:craft` | Frontend design guidance (auto-triggered on UI keywords) |

---

## Quick Start

```bash
# 1. Register the marketplace
claude plugin marketplace add https://github.com/devnomad-byte/techneering.git

# 2. Install — project-scoped is recommended
claude plugin install tn@techneering -s project

# 3. Start Claude Code and test the forge
claude
```

Then type <kbd>/tn:scout</kbd> to take it for a spin. On every session start, `tn:compass` is auto-injected — just say what you want to build and Techneering routes you to the right station.

---

## How It Works

Techneering picks one of four paths based on change size, then walks you through it station by station:

| Path | Flow |
|------|------|
| **Large change** | `spark → draft → forge → gate →` you choose verify depth |
| **Medium change** | `draft → forge → gate →` you choose verify depth |
| **Urgent bug** | `diagnose → redgreen → gate →` you choose verify depth |
| **Trivial tweak** | `direct edit → gate` *(optional)* |

**Always available:**
- `scout` — pure thinking, writes no files
- `craft` — auto-stacked when frontend work is detected

Inside `forge`, two skills chain automatically — **blueprint** breaks the work into tasks, **assemble** dispatches a fresh subagent per task with spec-compliance + code-quality review after each.

<details>
<summary><b>After the gate passes, pick how deep to verify</b></summary>

| Option | Flow | When |
|--------|------|------|
| **1** Full verification | `audit → inspect → ship → vault` | Production code |
| **2** Standard flow | `audit → ship → vault` | Routine features |
| **3** Quick commit | `ship → vault` | Prototypes, small changes |
| **4** Verify only | `audit` | Just check, don't commit |

</details>

Every skill writes its progress to disk. Close the terminal, come back tomorrow — Techneering scans the files and resumes exactly where you struck off.

---

## Design Philosophy

> **You drive the AI. Not the other way around.**

A model alone will drift — it guesses, over-builds, forgets decisions mid-conversation, and declares success without checking. Techneering wraps the model in a **harness** (Anthropic's term for the scaffolding around an agent) so its effort stays pointed where *you* aimed it.

| Principle | What it means |
|-----------|---------------|
| **You hold the reins** | Every step is a handoff *you* approve. AI proposes, you decide. Nothing ships without a human checkpoint. |
| **The spec is the contract** | Requirements live in files, not chat. Every line of code traces back to a `SHALL`/`MUST` — drift becomes visible, not invisible. |
| **Generation ≠ evaluation** | The agent that wrote the code never grades it. Separate reviewers (`tn:audit`, `tn:inspect`) check against spec and reality — models grade their own work too leniently to self-evaluate. |
| **Context is a file, not memory** | Each skill reads its inputs from disk and writes its outputs back. New session, new subagent, same source of truth — nothing lives only in someone's head. |
| **Simplest harness that works** | Four paths, not one. Trivial tweaks skip the full ceremony; only production code walks every gate. Complexity is added when the task earns it, never before. |

This pattern — decompose, hand off via artifacts, separate generation from evaluation — comes from [Anthropic's work on long-running agents](https://www.anthropic.com/engineering/harness-design-long-running-apps). Techneering applies it to **human-in-the-loop** development: the harness keeps the AI disciplined, you keep the harness pointed at the right target.

The four layers shown at the top aren't separate features — they're the same system seen from different angles. The loop is the latest layer ([a pattern Addy Osmani named in 2024](https://addyosmani.com/blog/loop-engineering/)): stop hand-prompting every step, design the system that does it. Techneering was built this way from the start — including the parts most loop systems skip: explicit **failure limits** (`tn:diagnose` stops after 3 failed fixes), **generation ≠ evaluation** (the writer never grades its own work), and a **depth dial** so trivial tweaks skip the full ceremony.

---

## The Workspace

Running `tn:draft` generates a self-contained workspace at your project root. It is the source of truth; nothing lives only in chat.

```
techneering/
├── config.yaml          # tech stack · coding style · quality rules
├── specs/               # authoritative module definitions
├── changes/
│   └── <name>/          # one directory per change
│       ├── .tn.yaml
│       ├── proposal.md  # why  — the motivation
│       ├── specs/       # what — SHALL/MUST · WHEN/THEN
│       ├── design.md    # how  — decisions & trade-offs
│       └── tasks.md     # todo — checkbox-tracked, resumable
└── archive/             # finished changes, fully preserved
```

`tn:audit` traces every line of code back to a requirement. `tn:vault` merges finished specs into the main `specs/`, so knowledge **compounds** instead of evaporating.

---

## Requirements

| Dependency | Why |
|------------|-----|
| **Claude Code CLI** (latest) | the host that runs the skills |
| **Git** 2.20+ | the `isolate` station needs worktrees |
| **Bash** 4.0+ | the session-start hook |

Runs on Windows 10/11 · macOS 12+ · Linux.

---

<div align="center">

<sub>Built with discipline. Released under <a href="LICENSE">MIT</a>.</sub>
<br>
<sub>★ Fork it · forge it · make it yours ★</sub>

</div>
