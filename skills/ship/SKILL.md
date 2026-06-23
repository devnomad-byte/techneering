---
name: ship
description: Use when implementation is complete, all tests pass, and you need to decide how to integrate the work — guides completion by presenting structured options for merge, PR, or cleanup
---

# Finishing a Development Branch

Guide completion of development work. Verify tests, present clear options, execute the chosen workflow, clean up.

## Core Principle

```
VERIFY TESTS → PRESENT OPTIONS → EXECUTE CHOICE → CLEAN UP.
No merge without verification. No discard without confirmation.
```

## Steps

### Step 1: Verify Tests

**Before presenting options, verify tests pass:**

```bash
# Run project's test suite
npm test / cargo test / pytest / go test ./...
```

**If tests fail:** Stop. Report failures. Don't proceed.
**If tests pass:** Continue to Step 2.

### Step 2: Determine Base Branch

```bash
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

### Step 3: Present Options

Present exactly these 4 options:

```
Implementation complete. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

### Step 4: Execute Choice

#### Option 1: Merge Locally
```bash
git checkout <base-branch>
git pull
git merge <feature-branch>
<test command>
git branch -d <feature-branch>
```
Then: Cleanup worktree

#### Option 2: Push and Create PR
```bash
git push -u origin <feature-branch>
gh pr create --title "<title>" --body "<body>"
```
Then: Cleanup worktree

#### Option 3: Keep As-Is
Report: "Keeping branch <name>. Worktree preserved at <path>."
Don't cleanup worktree.

#### Option 4: Discard
**Confirm first:** Require typed "discard" confirmation.
Then: Delete branch + cleanup worktree.
Also: Ask user if they want to remove the change directory from `techneering/changes/`. If yes, move to `techneering/archive/YYYY-MM-DD-<name>-discarded/`.

### Step 5: Cleanup Worktree

For Options 1, 2, 4:
```bash
git worktree list | grep $(git branch --show-current)
# If in worktree:
git worktree remove <worktree-path>
```

## Red Flags

| Thought | Reality |
|---------|---------|
| "Tests passed before, no need to rerun" | Must rerun before merge — context may have changed |
| "Discard without confirmation" | Discard is irreversible — must have typed confirmation |
| "Force push is fine" | Force push may overwrite others' work unless the user explicitly asks |
| "Skip merge, push directly" | Unmerged branches may have hidden conflicts |
| "Not enough options" | Exactly 4 options — don't add or remove any |

## Common Rationalizations

| Excuse | Reality |
|---------|---------|
| "Tests should pass" | "Should" is not verification — run to confirm |
| "Small changes don't need a PR" | Small changes can still conflict — PR is a safety net |
| "Just force push" | Not allowed unless the user explicitly requests it |
| "Discard without confirmation" | Discard is irreversible — must confirm |
| "Worktree doesn't need cleanup" | Leftover worktrees cause confusion and disk bloat |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Precondition | All tests must pass |
| 4 options | Merge / PR / Keep / Discard |
| Confirmation | Discard requires typed "discard" |
| Worktree cleanup | Options 1/2/4 clean up, option 3 keeps |
| Forbidden | Unconfirmed discard / force push / skipping tests |

## Next Step

When done:
- Normal case (options 1 or 2) → invoke `tn:vault` to archive the change
- User chose keep (option 3) → end, wait for user follow-up
- User chose discard (option 4) → clean change directory (move to archive or delete) → end
- User requests stop → end the current flow

## Guardrails

- Never proceed with failing tests
- Never merge without verifying tests on result
- Never delete work without typed "discard" confirmation
- Never force-push without explicit user request
- Always present exactly 4 options
- Announce at start: "I'm using the tn:ship skill to complete this work."
