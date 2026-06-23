---
name: inspect
description: Use after tn:audit passes, when completing major features, or before merging — dispatches a global cross-task code review covering consistency, architecture, and security (distinct from tn:assemble's per-task review)
---

# Global Review

Dispatch a code-reviewer subagent to review the **whole change** as one body of work, not task-by-task. Where `tn:assemble` reviewed each task in isolation (spec compliance + code quality), inspect catches problems that only surface when you look at the change end-to-end: **cross-task consistency, architectural integrity, and security**.

## Core Principle

```
REVIEW THE WHOLE, NOT THE PARTS.
Each task passed review alone — but do they fit together?
The reviewer is a safety net, not a rubber stamp.
```

### Why inspect ≠ assemble's per-task review

| | tn:assemble review | tn:inspect |
|---|---|---|
| Scope | one task | the entire change, all tasks together |
| Questions | did this task match its spec? | do the tasks fit? is the whole sound? |
| Catches | per-task defects | emergent issues: inconsistency, arch drift, security holes |

Inspect is not a re-run of assemble's reviews. It looks at what those reviews **could not see** — the gaps between tasks.

## Steps

### Phase 1: Request Review

#### Step 1: Get Git SHAs

Use the **merge-base** of the change, not `HEAD~1` (a change often spans many commits, so `HEAD~1` misses everything but the last):

```bash
BASE_SHA=$(git merge-base HEAD origin/main)  # whole change from branch point
HEAD_SHA=$(git rev-parse HEAD)
```

#### Step 2: Dispatch Code-Reviewer Subagent

Dispatch a reviewer subagent using the template at `./code-reviewer-prompt.md`. The template is host-neutral — see its header for how to dispatch on Claude Code (Agent tool) vs Codex (subagent). Fill the placeholders, then send the full prompt block to the reviewer.

**Placeholders to fill:**
- `WHAT_WAS_IMPLEMENTED` — What the whole change built (the proposal's "what")
- `PLAN_OR_REQUIREMENTS` — Reference proposal.md + the full tasks.md (all tasks, not one)
- `BASE_SHA` — Merge-base of the change
- `HEAD_SHA` — Current HEAD
- `DESCRIPTION` — Brief summary of the change as a whole
- `CODING_CONVENTIONS` — From `techneering/config.yaml` coding_conventions section (if exists)
- `QUALITY_STANDARDS` — From `techneering/config.yaml` quality_standards section (if exists)

#### Step 3: Receive Feedback

The reviewer returns:
- **Strengths:** What was done well
- **Issues:** Critical / Important / Minor
- **Assessment:** Approved / Needs fixes

### Phase 2: Process Feedback

**The Response Pattern:**

```
WHEN receiving code review feedback:

1. READ: Complete feedback without reacting
2. UNDERSTAND: Restate requirement in own words (or ask)
3. VERIFY: Check against codebase reality
4. EVALUATE: Technically sound for THIS codebase?
5. RESPOND: Technical acknowledgment or reasoned pushback
6. IMPLEMENT: One item at a time, test each
```

**Forbidden Responses:**
- "You're absolutely right!" / "Great point!" / "Excellent feedback!"
- "Let me implement that now" (before verification)
- Agreeing without understanding the issue

**Correct Responses:**
- Restate the technical requirement
- Ask clarifying questions
- Push back with technical reasoning if wrong
- Just start working (actions > words)

#### Step 4: Act on Feedback

- Fix Critical issues immediately
- Fix Important issues before proceeding
- Note Minor issues for later
- Push back if reviewer is wrong (with reasoning)

## Red Flags

| Thought | Reality |
|---------|---------|
| "assemble already reviewed each task" | Per-task review can't see cross-task gaps — that's inspect's whole point |
| "Review passed, can skip it" | Review is a quality gate, not a formality |
| "The reviewer is always right" | The reviewer may lack full context — must verify independently |
| "Merge directly without review" | Unreviewed code is a source of technical debt |
| "Fix Critical issues later" | Critical = must fix immediately, no exceptions |
| "Feedback too long, won't read" | Every item must be read, understood, verified, responded to |

## Common Rationalizations

| Excuse | Reality |
|---------|---------|
| "assemble's reviews covered it" | assemble reviewed each task alone; inspect reviews the assembled whole |
| "Tight deadline, skip review" | Time saved by skipping returns doubled in bug costs |
| "Just a small change, no review needed" | Small changes introduce big bugs all the time |
| "Already ran tests" | Tests verify behavior, review verifies design and integration |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Scope | The whole change (merge-base → HEAD), not one task |
| Focus | Cross-task consistency · architecture · security |
| Difference from assemble | assemble = per-task; inspect = whole-change |
| Two phases | Request review → receive and process feedback |
| Feedback levels | Critical (must fix) / Important (should fix) / Minor (optional) |
| Processing pattern | read → understand → verify → evaluate → respond → implement |
| Push back allowed | Breaks functionality / lacks context / YAGNI / technically wrong |

## Next Step

When done:
- Review passed (Approved) → invoke `tn:ship` for branch integration decision
- Review found Critical issues → fix and re-submit for review
- Review found architecture issues → roll back to `tn:forge` to redo affected tasks
- User requests stop → end the current flow

## Guardrails

- Review runs after `tn:audit` passes and before `tn:ship`
- inspect is a **whole-change** review — do not collapse it into a per-task re-check
- Never skip review for any reason
- Never blindly accept all feedback — verify each point independently
- Never merge without review approval
- Use merge-base, not `HEAD~1`, so the review covers the entire change
- Push back when: suggestion breaks functionality / reviewer lacks context / violates YAGNI / technically incorrect
- Acknowledge correct feedback with technical descriptions, not praise
- Fix one issue at a time, test each fix
