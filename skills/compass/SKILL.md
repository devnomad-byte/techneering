---
name: compass
description: Use when starting any conversation — establishes how to find and use Techneering skills, requiring skill invocation before ANY response including clarifying questions
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
(This is a hard instruction, not a suggestion — subagents go straight to their assigned task.)
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.
</EXTREMELY-IMPORTANT>

## Core Principle

```
INVOKE RELEVANT SKILLS BEFORE ANY RESPONSE OR ACTION.
Even a 1% chance means you should invoke the skill to check.
```

## Instruction Priority

Techneering skills override default system prompt behavior, but **user instructions always take precedence**:

1. **User's explicit instructions** (CLAUDE.md, direct requests) — highest priority
2. **Techneering skills** — override default system behavior where they conflict
3. **Default system prompt** — lowest priority

If CLAUDE.md says "don't use TDD" and a skill says "always use TDD," follow the user's instructions. The user is in control.

## How to Access Skills

Invoke the skill through your host's skill mechanism (the `Skill` tool on Claude Code; the skill/prompt loader on Codex). When you invoke a skill, its content is loaded and presented to you — follow it directly. Never read a skill file's raw contents manually as a substitute for invoking it.

## Skill Inventory

### Gateway

| Call | Trigger | Responsibility |
|--------|---------|------|
| `tn:compass` | Auto-injected on every session start | Identify applicable skills, select development path |

### Spec-Driven Flow Skills

| Call | Trigger | Responsibility |
|--------|---------|------|
| `tn:scout` | User wants to quickly explore ideas or investigate problems | Standalone exploration mode, produces no files |
| `tn:draft` | User raises a new requirement/change | Generate proposal + specs + design + tasks |
| `tn:forge` | User wants to implement an existing change | Implementation entry, chains tn:blueprint → tn:assemble |
| `tn:audit` | After implementation is complete | Dual-layer verification: code↔spec + code↔real requirements |
| `tn:vault` | Change complete, confirming archive | delta spec merge → move into archive/ |

### Process Skills

| Call | Trigger | Responsibility |
|--------|---------|------|
| `tn:spark` | Before new features or large changes | Socratic questioning → approach comparison → set direction |
| `tn:blueprint` | Have a clear spec, about to implement | Break into bite-sized tasks |
| `tn:diagnose` | Hit a bug, test failure | 4 phases: root cause → pattern → hypothesis → fix |
| `tn:gate` | About to claim work complete | Iron law: run command → read output → confirm → then claim |

### Execution Skills

| Call | Trigger | Responsibility |
|--------|---------|------|
| `tn:redgreen` | Implementing any feature or fix | Red-green-refactor cycle |
| `tn:assemble` | Have an implementation plan to execute | Subagent dispatch + per-task review + parallel scheduling |
| `tn:inspect` | After verify passes | Global review: cross-task consistency, architecture, security |
| `tn:isolate` | Need isolated feature development | Create isolated workspace → verify test baseline |

### Wrap-up Skill

| Call | Trigger | Responsibility |
|--------|---------|------|
| `tn:ship` | After review passes | Verify tests → present 4 options → execute |

### Domain Skill (auto-detected)

| Call | Trigger | Responsibility |
|--------|---------|------|
| `tn:craft` | Any frontend/UI/page/component/style work (see Frontend Detection Rules below) | Design thinking → aesthetic guidance → production-grade code |

### Frontend Detection Rules

When evaluating a user's request, check for these signals. If ANY match, invoke `tn:craft` during the implementation phase. Keywords are intentionally bilingual so Chinese user input is also detected:

| Signal type | Keyword examples |
|---------|-----------|
| Direct frontend | "页面/page", "组件/component", "UI", "界面/interface", "布局/layout", "样式/style", "前端/frontend" |
| Interactive elements | "按钮/button", "表单/form", "弹窗/dialog", "导航/nav", "菜单/menu", "表格/table", "列表/list", "卡片/card", "对话框/modal" |
| Visual | "配色/color", "主题/theme", "动画/animation", "响应式/responsive", "暗色模式/dark mode", "design" |
| Framework markers | "React", "Vue", "HTML", "CSS", "Tailwind", "Ant Design", "Element", "shadcn" |

**How triggering works:** Frontend detection is not a standalone path — it auto-stacks onto the implementation phase of an existing path:
- Complete path: spark → draft → forge (invoke tn:craft when frontend detected) → gate → ...
- Lightweight path: draft → forge (invoke tn:craft when frontend detected) → gate → ...
- During assemble execution: task involves frontend code → call tn:craft for aesthetic guidance first → then implement
- User says "make a page" directly → skip draft, go straight to tn:craft

## Development Path Selection

When a user presents a requirement, evaluate its scope and select the appropriate path:

```
User states a requirement
    │
    ▼
[tn:compass] evaluates change size
    │
    ├── Large change (new feature, cross-module, 3+ files) → Complete path
    ├── Medium change (single feature, 1-3 files) → Lightweight path
    ├── Urgent fix (production bug) → Fix path
    └── Trivial change (typo, config value) → Direct execution

Standalone entries (no path selection):
[tn:scout] always available, standalone exploration
```

### Complete Path (Large Changes)

```
tn:spark → tn:draft → [tn:isolate] → tn:forge(blueprint→assemble) → tn:gate → [user chooses next flow]
```

### Lightweight Path (Medium Changes)

```
tn:draft → tn:forge(blueprint→assemble) → tn:gate → [user chooses next flow]
```

### Fix Path (Emergency Bugs)

```
tn:diagnose → tn:redgreen → tn:gate → [user chooses next flow]
```

### Post-Check Menu

After `tn:gate` passes, present the menu for user to choose:

| Option | Flow | Use case |
|------|------|---------|
| 1. Full verification | audit → inspect → ship → vault | Production code, critical features |
| 2. Standard flow | audit → ship → vault | Internal projects, routine features |
| 3. Quick commit | ship → vault | Small changes, prototypes |
| 4. Verify only | audit (don't commit) | Just check, don't commit |

### Direct Execution (Trivial Changes)

```
Direct edit → tn:gate (optional)
```

### Independent Explore Mode

```
tn:scout — always available, pure thinking, produces no files, enters no path
```

## Flow Transition Rules

Every skill has a Next Step section. When a skill completes, follow its transition:

| Current skill | Normal exit | Exception exit |
|---------|---------|---------|
| `tn:spark` | `tn:draft` | — |
| `tn:draft` | `tn:forge` | — |
| `tn:forge` | `tn:gate` (internally chains blueprint→assemble, ends at gate) | `tn:diagnose` (when implementation hits an issue) |
| `tn:blueprint` | `tn:assemble` (internal chain) | — |
| `tn:assemble` | `tn:gate` | `tn:diagnose` (when issues arise) |
| `tn:redgreen` | Return to caller | `tn:diagnose` (after 3 RED→GREEN attempts still failing) |
| `tn:gate` | Show menu for user choice (see Post-Check Menu) | Return to current skill (when tests fail) |
| `tn:audit` | Follow check menu choice (review / finish / end) | WARNING → fix → `tn:gate` → `tn:audit` (loop limit 3 rounds) / `tn:forge` (large deviation) / `tn:draft` (spec wrong) |
| `tn:inspect` | `tn:ship` | Back to `tn:forge` (Critical issues) |
| `tn:ship` | `tn:vault` | — |
| `tn:scout` | None (standalone mode) | `tn:draft` (when user decides to implement) |
| `tn:craft` | Return to caller (usually tn:assemble or direct implementation) | — |
| `tn:diagnose` | `tn:redgreen` → `tn:gate` | — |
| `tn:isolate` | Return to caller | — |
| `tn:vault` | End | — |

**How transitions work:**
- Each skill's SKILL.md contains a `## Next Step` section with condition branches
- After completing a skill, read its Next Step to determine what to invoke next
- This table is the authoritative reference — if a skill's Next Step conflicts with this table, this table wins
- Users can always override by explicitly invoking any skill

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (tn:spark, tn:diagnose) — these determine HOW to approach the task
2. **Implementation skills second** (tn:craft) — these guide execution

"Let's build X" → tn:spark first, then implementation skills.
"Fix this bug" → tn:diagnose first, then domain-specific skills.

## Skill Types

**Rigid** (tn:redgreen, tn:diagnose, tn:gate): Follow exactly. Don't adapt away discipline.

**Flexible** (tn:spark, tn:blueprint, tn:scout): Adapt principles to context.

**Flow** (tn:draft → tn:forge → tn:gate → tn:audit → tn:inspect → tn:ship → tn:vault): Sequential, each step has clear output.

The skill itself tells you which.

## Red Flags

These thoughts mean STOP — you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Total skills | 16 (gateway 1 + flow 5 + process 4 + execution 4 + wrap-up 1 + domain 1) |
| Path selection | Complete / Lightweight / Fix / Direct |
| Transition mechanism | Each skill's Next Step + the gateway Flow Transition Rules table |
| Skill types | Rigid (redgreen/diagnose/gate) / Flexible (spark/blueprint/scout) / Flow (draft→vault) |
| Source of truth | Disk files are the sole source of truth (`techneering/changes/<name>/`) |

## Cross-Session Resume

The skill system maintains no session state. All progress is inferred from disk files:

```
New session starts
    │
    ▼
User says "continue implementing XXX change"
    → scan techneering/changes/<name>/ directory
    → infer current progress from existing files:
      ├── only proposal.md → back to draft, continue
      ├── has proposal + specs + design + tasks → enter forge
      ├── tasks.md has unchecked checkboxes → continue implementation
      └── all checkboxes checked → enter tn:gate (show menu after passing)
```

Disk files are the sole source of truth. Every skill checks files first to determine progress.

## Next Step

This is the gateway skill, loaded at session start. It does not have a Next Step — it prepares you to invoke the correct skill based on the user's request.

## Guardrails

- Never skip skill check before responding to a user message
- Never read a skill file's raw contents manually as a substitute for invoking it — use your host's skill mechanism
- User instructions always override skill instructions
- When in doubt about which skill applies, invoke it to check — wrong invocations are harmless
- The Flow Transition Rules table is the authoritative reference for skill transitions
