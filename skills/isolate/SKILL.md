---
name: isolate
description: Use when starting feature work that needs isolation from current workspace or before executing implementation plans — creates isolated git worktrees with smart directory selection and safety verification
---

# Using Git Worktrees

Create isolated workspaces sharing the same repository, allowing work on multiple branches simultaneously without switching.

## Core Principle

```
SYSTEMATIC DIRECTORY SELECTION + SAFETY VERIFICATION = RELIABLE ISOLATION.
Never create a worktree without verifying it's gitignored.
```

## Steps

### Step 1: Directory Selection

Follow this priority order:

1. **Check existing directories:**
   ```bash
   ls -d .worktrees 2>/dev/null     # Preferred (hidden)
   ls -d worktrees 2>/dev/null      # Alternative
   ```
   If found: Use that directory. `.worktrees` wins if both exist.

2. **Check CLAUDE.md:**
   ```bash
   grep -i "worktree.*director" CLAUDE.md 2>/dev/null
   ```
   If preference specified: Use it without asking.

3. **Ask user** (only if no directory exists and no preference):
   ```
   No worktree directory found. Where should I create worktrees?
   1. .worktrees/ (project-local, hidden)
   2. ~/.config/tn/worktrees/<project-name>/ (global location)
   ```

### Step 2: Safety Verification

**MUST verify directory is ignored before creating worktree:**

```bash
git check-ignore -q .worktrees 2>/dev/null
```

**If NOT ignored:** Add to .gitignore and commit.

### Step 3: Create Worktree

1. Detect project name: `project=$(basename "$(git rev-parse --show-toplevel)")`
2. Create worktree: `git worktree add "$path" -b "$BRANCH_NAME"`
3. Run project setup (auto-detect from package.json, Cargo.toml, etc.)
4. Verify clean baseline (run tests)
5. Record the baseline state (passing test count / suite output) so later `tn:gate` or `tn:audit` can compare against it
6. Report location

**If no test command exists:** degrade gracefully — record "no tests; baseline = clean checkout + setup completes" and note it in the report. Do not block isolation on a project that legitimately has no tests.

## Red Flags

| Thought | Reality |
|---------|---------|
| "Directory doesn't need gitignore" | Unignored worktrees pollute version control |
| "Skip baseline tests, just start" | Baseline test failure means environment issues — must resolve first |
| "Develop directly on the current branch" | Unisolated development pollutes the main branch |
| "Worktree is done once created" | Must verify the test baseline passes before starting work |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Directory priority | .worktrees → worktrees → CLAUDE.md → ask user |
| Safety check | Must verify gitignore |
| Baseline verification | Run tests after creation to ensure clean |
| Cleanup | Handled by tn:ship |

## Next Step

When done:
- Normal case → return to caller (usually `tn:assemble` or `tn:forge`), continue implementation flow
- Baseline tests fail → report failure reason, ask user whether to continue
- User requests stop → end the current flow

## Guardrails

- Never create a worktree without verifying the directory is gitignored
- If baseline tests fail, report and ask — don't proceed silently
- Follow existing directory conventions if they exist
- Announce at start: "I'm using the tn:isolate skill to set up an isolated workspace."
- Pairs with `tn:ship` for cleanup after work is complete
