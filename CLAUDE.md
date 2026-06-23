# Techneering

> Open-source spec-driven Claude Code skills plugin.

## Plugin Identity

| Field | Value | Notes |
|------|---|------|
| Repository | `github.com/devnomad-byte/techneering` | Open-source repo |
| plugin.json name | `tn` | Claude Code identifier |
| settings registration | `"tn@techneering": true` | Enable at project or global scope |
| Skill call prefix | `tn:` | Prefix for all skills |

## Skill Inventory

| Skill | Call | Type | Description |
|------|--------|------|------|
| Compass | `tn:compass` | Gateway | Auto-injected on session start, selects development path |
| Scout | `tn:scout` | Flow | Standalone exploration mode, pure thinking, writes no files |
| Draft | `tn:draft` | Flow | Generate proposal + specs + design + tasks |
| Forge | `tn:forge` | Flow | Entry point: chains blueprint → assemble |
| Audit | `tn:audit` | Flow | Dual-layer verification: spec compliance + requirement tracing |
| Vault | `tn:vault` | Flow | delta merge → move into archive/ |
| Spark | `tn:spark` | Process | Socratic questioning + project style detection + framework recommendation |
| Blueprint | `tn:blueprint` | Process | Break into bite-sized tasks |
| Diagnose | `tn:diagnose` | Rigid | 4 phases: root cause → pattern → hypothesis → fix |
| Gate | `tn:gate` | Rigid | Iron law: run command → read output → claim → show menu |
| Redgreen | `tn:redgreen` | Rigid | Red-green-refactor cycle |
| Assemble | `tn:assemble` | Execution | Subagent dispatch + two-stage review + parallel scheduling |
| Inspect | `tn:inspect` | Execution | Triggered after menu option 1: global review for consistency, architecture, security |
| Isolate | `tn:isolate` | Execution | Create isolated workspace + verify test baseline |
| Ship | `tn:ship` | Wrap-up | Triggered by inspect or menu options 2/3: 4 options |
| Craft | `tn:craft` | Domain | Auto-triggered when a frontend task is detected |

## Development Paths

| Path | Use case | Flow |
|------|---------|------|
| Complete | Large changes | spark → draft → forge → gate → user chooses next flow |
| Lightweight | Medium changes | draft → forge → gate → user chooses next flow |
| Fix | Urgent bugs | diagnose → redgreen → gate → user chooses next flow |
| Direct | Trivial changes | direct execution → gate |

**After gate passes, present a menu for the user to choose:**
1. Full verification (audit → inspect → ship → vault) — production code
2. Standard flow (audit → ship → vault) — internal projects
3. Quick commit (ship → vault) — small changes/prototypes
4. Verify only (audit) — just check, don't commit

## Skill Transitions

After each skill completes, its Next Step points to the next skill. The full transition rules are in the `compass` gateway.

## Standard Skill Structure

All skills follow a unified structure: frontmatter → core principle → steps → red flags → quick reference → Next Step → guardrails.

Rigid skills (redgreen/diagnose/gate) must have a red-flags list. All skills must have Next Step + guardrails.

## config.yaml Core Fields

On the first draft, `techneering/config.yaml` is generated with two core sections:

| Field | Purpose | Source |
|------|------|------|
| `coding_conventions` | Code style constraints (naming, structure, patterns, formatting) | spark detection / simplified detection / framework defaults |
| `quality_standards` | External quality standards (Alibaba Taishan, PEP 8, etc.) | Recommended after spark detects the tech stack |

**quality_standards structure:**
```yaml
quality_standards:
  sources: ["ali-taishan-java", "clean-code"]  # standard sources (stackable)
  custom_rules: ["no System.out"]              # project-specific rules (highest priority)
  severity: strict                             # strict / normal / relaxed
```

**Standard source options:** Java → ali-taishan-java, Python → ali-taishan-python + pep8, Go → gofmt, frontend → user's choice.

**Full-flow injection:** draft writes → assemble implementer/reviewer read and enforce → inspect cross-checks globally.
