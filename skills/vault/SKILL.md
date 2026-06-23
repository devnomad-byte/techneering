---
name: vault
description: Archive a completed change — delta spec merge and move to archive. Use when the user wants to finalize and archive a completed change.
---

# Archive a Change

Merge delta specs into main specs and move the change to archive. The final step in the Techneering workflow.

## Core Principle

```
NO ARCHIVE WITHOUT CONFIRMATION.
Every artifact checked. Every delta applied. Every conflict reported.
```

## Steps

### Step 1: Select the Change

Same selection logic as tn:forge — auto-detect or ask user.

Always announce: "Archiving change: <name>"

### Step 2: Pre-Archive Checks

**Artifact completion:** Check all artifacts exist (proposal, specs, design, tasks).

**Task completion:** Read `techneering/changes/<name>/tasks.md`, count `- [ ]` (incomplete) vs `- [x]` (complete).
- If incomplete tasks: warn user, confirm before proceeding

### Step 3: Delta Spec Merge

If delta specs exist at `techneering/changes/<name>/specs/`:

#### 3a. Read Delta Specs

For each delta spec file, parse the delta blocks:
- `## ADDED Requirements`
- `## MODIFIED Requirements`
- `## REMOVED Requirements`
- `## RENAMED Requirements`

#### 3b. Apply Deltas to Main Specs

Execute operations in this strict order: **RENAMED → REMOVED → MODIFIED → ADDED**

- **ADDED:** Append requirement blocks to the end of the corresponding chapter in the main spec
- **MODIFIED:** Replace the matching requirement block in the main spec (match by `### Requirement: <name>`)
- **REMOVED:** Delete the matching requirement block from the main spec
- **RENAMED:** Rename the requirement block header in the main spec (FROM → TO)

#### 3c. Handle Edge Cases

- MODIFIED/REMOVED target not found in main spec → Skip, log warning
- ADDED target chapter doesn't exist → Create chapter, then append
- Conflict (same requirement targeted by multiple deltas) → Stop merge, report to user

#### 3d. Post-Merge Verification

- Confirm main spec structure is intact
- No orphaned references
- No duplicate requirement names

If no delta specs exist, skip merge step.

### Step 4: Perform Archive

```bash
mkdir -p techneering/archive
mv techneering/changes/<name> techneering/archive/YYYY-MM-DD-<name>
```

If target archive already exists, report error and suggest renaming.

### Step 5: Display Summary

```
## Archive Complete

**Change:** <change-name>
**Archived to:** techneering/archive/YYYY-MM-DD-<name>/
**Specs:** Synced to main specs (or "No delta specs" or "Sync skipped")

All artifacts complete. All tasks complete.
```

## Red Flags

| Thought | Reality |
|---------|---------|
| "Can archive even if not finished" | Incomplete tasks must be warned and confirmed |
| "Delta merge errors don't matter" | Merge conflicts corrupt the main spec — must stop |
| "Archive is just moving files" | Archive = delta merge + verification + move, all required |
| "Skip verification to speed up" | Unverified main spec may be structurally broken |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Core operation | delta spec merge → move into archive/ |
| Merge order | RENAMED → REMOVED → MODIFIED → ADDED |
| Pre-checks | artifact completeness + task completion |
| Exception handling | target missing → skip + warn / conflict → stop + report |
| Path placeholder | Always use `techneering/changes/<name>/` |

## Next Step

When done:
- Normal case → done, Techneering workflow complete
- Merge conflict found during archive → pause, report to user for manual handling
- User requests stop → end the current flow

## Guardrails

- Always confirm with user if incomplete artifacts or tasks exist
- Preserve .tn.yaml when moving to archive
- Don't block archive on warnings — inform and confirm
- Show clear summary of what happened
- If delta spec merge encounters conflicts, stop and ask user
- Unified path placeholder: always use `techneering/changes/<name>/`
