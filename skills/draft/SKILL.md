---
name: draft
description: Propose a new change with all artifacts generated in one step. Use when the user wants to describe what they want to build and get a complete proposal with design, specs, and tasks ready for implementation.
---

# Draft a Change

Create a complete Techneering change — generate all specification artifacts in one step. Read project context and spark conclusions (if any), then produce: proposal → specs → design → tasks.

## Core Principle

```
UNDERSTAND BEFORE BUILDING.
Do NOT proceed without understanding what the user wants to build.
```

## Steps

### Step 1: Understand the Request

If the user's request is unclear, ask what they want to build. Derive a kebab-case name (e.g., "add user authentication" → `add-user-auth`).

If spark conclusions exist in the conversation, read them as input.

### Step 2: Initialize Project Workspace (First Time Only)

Check if `techneering/` directory exists at the project root. If not, create:

```
techneering/
  ├── config.yaml        # project context and rules
  ├── specs/             # main specification docs
  ├── changes/           # active changes
  └── archive/           # archived changes
```

Generate `config.yaml`:
```yaml
schema: spec-driven

context: |
  # TODO: fill in project context (tech stack, framework, database, deployment environment, etc.)

coding_conventions:
  naming:
    files: ""              # kebab-case / PascalCase / camelCase / snake_case
    classes: ""            # PascalCase (Java/TS class) / snake_case (Ruby/Python)
    functions: ""          # camelCase / snake_case / PascalCase
    constants: ""          # UPPER_SNAKE / camelCase / PascalCase
    variables: ""          # camelCase / snake_case
  structure:
    pattern: ""            # feature-based / layered / mvc / modular / monorepo / hexagonal
    source_dir: ""         # src/ / lib/ / app/ / internal/
    test_dir: ""           # tests/ / __tests__/ / test/ / src/test/
  patterns:
    error_handling: ""     # exceptions / Result type / error codes / try-catch
    dependency_injection: "" # constructor / annotations / manual / module system
    api_style: ""          # REST / GraphQL / gRPC / message-queue
    data_access: ""        # ORM / query-builder / raw-sql / repository-pattern
    imports: ""            # alias / relative / absolute / package-based
  formatting:
    indent: ""             # 2 spaces / 4 spaces / tab
    quotes: ""             # single / double
    semicolons: ""         # true / false
    trailing_comma: ""     # es5 / all / none
    max_line_length: ""    # 80 / 100 / 120 / none

quality_standards:
  # standard sources (stackable, ordered by priority)
  sources:
    - ""                   # primary standard source (see options table below)

  # custom rules (override or supplement the sources)
  custom_rules: []

  # review severity level
  severity: "strict"       # strict (Critical must fix) / normal (Important should fix) / relaxed (inform only)

rules:
  proposal: []
  specs: []
  design: []
  tasks: []
```

**Standard source options table (by tech stack):**

| Tech stack | Recommended source | Notes |
|--------|-----------|------|
| Java | `ali-taishan-java` | Alibaba Java Development Manual (Taishan edition) |
| Java | `google-java` | Google Java Style Guide, common for international projects |
| Python | `ali-taishan-python` | Alibaba Python Development Manual |
| Python | `pep8` | PEP 8 Style Guide (can stack with ali-taishan-python) |
| Go | `gofmt` | Go official formatting style (mandatory) |
| JavaScript | `standardjs` | No-semicolon style, concise |
| JavaScript | `airbnb-js` | Airbnb JS Style Guide, widely used |
| TypeScript | `google-js` | Google JS/TS Style Guide |
| Vue | `vue-official` | Vue official style guide |
| React | `react-airbnb` | Airbnb React/JSX Style Guide |
| General | `clean-code` | Clean Code principles (can stack with any) |
| General | `solid` | SOLID design principles |

**Filling coding_conventions:**
- If spark conclusions contain convention analysis → fill from spark output
- If no spark (lightweight path) → run simplified detection: scan project structure, sample 2-3 files, check for config files (.editorconfig, .prettierrc, eslint)
- If new project with no code → leave empty, recommend framework defaults during design.md generation

**Filling quality_standards:**
- Detect tech stack from project files:
  - `pom.xml` / `build.gradle` → Java → recommend `ali-taishan-java`
  - `requirements.txt` / `pyproject.toml` / `setup.py` → Python → recommend `ali-taishan-python` + `pep8`
  - `go.mod` → Go → recommend `gofmt`
  - `package.json` → check dependencies:
    - Has `vue` → prompt user to choose (`vue-official` recommended)
    - Has `react` → prompt user to choose (`react-airbnb` recommended)
    - Has `typescript` → prompt user to choose (`google-js` recommended)
    - Otherwise → prompt user to choose (`airbnb-js` or `standardjs`)
  - No detected stack → leave empty, prompt user
- If spark conclusions contain quality_standards choice → fill from spark output
- Always ask user to confirm the recommended sources before writing config.yaml

### Step 3: Create Change Directory

Create:
```
techneering/changes/<name>/
  ├── .tn.yaml     # { schema: spec-driven, created: YYYY-MM-DD }
  ├── proposal.md
  ├── specs/
  ├── design.md
  └── tasks.md
```

If the change already exists, ask: continue it or create a new one?

### Step 4: Generate Artifacts in Dependency Order

Read `techneering/config.yaml` for context and rules. Generate artifacts following the dependency graph:

```
proposal → specs (depends on proposal)
         → design (depends on proposal)
         → tasks (depends on specs + design)
```

#### proposal.md
Sections: Why + What Changes + Capabilities (New/Modified) + Impact.
Focus on "why" not "how".

#### specs/*.md
One spec file per capability. Use delta operations:
- **ADDED Requirements**: New capabilities
- **MODIFIED Requirements**: Changed behavior (full updated content)
- **REMOVED Requirements**: Deprecated features (include Reason + Migration)
- **RENAMED Requirements**: Name changes (FROM:/TO: format)

Each requirement: `### Requirement: <name>` with SHALL/MUST language.
Each scenario: `#### Scenario: <name>` with WHEN/THEN format.

#### design.md
Sections: Context + Goals/Non-Goals + Decisions + Risks/Trade-offs.
Focus on architecture and approach.

#### tasks.md
Checkbox format: `- [ ] X.Y Task description`.
Group under numbered headings. Each task completable in one session.

### Step 5: Show Summary

```
## Change Created: <name>

### Artifacts
- proposal.md — [brief summary]
- specs/<capability>.md — [N requirements]
- design.md — [key decisions]
- tasks.md — [N tasks in M groups]

### What's Next
Ready to implement? Run `tn:forge` to start.
```

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Output | 4 artifacts under `techneering/changes/<name>/` |
| Dependency order | proposal → specs + design → tasks |
| First-time use | Auto-creates `techneering/` directory + config.yaml |
| Path placeholder | Always use `techneering/changes/<name>/` |
| Duplicate change | Detect existing change, ask continue or new |

## Next Step

When done:
- Normal case → invoke `tn:forge` to start implementation
- User wants to isolate the workspace first → invoke `tn:isolate`, then `tn:forge`
- User wants to keep refining the proposal → stay in draft, edit artifacts
- User requests stop → end the current flow

## Guardrails

- Create ALL artifacts needed for implementation
- Always read dependency artifacts before creating a new one
- Context and rules from config.yaml are constraints for YOU, not content for the files
- If context is critically unclear, ask the user — but prefer making reasonable decisions
- If a change with that name already exists, ask if user wants to continue it or create a new one
- Verify each artifact file exists after writing before proceeding to next
- Unified path placeholder: always use `techneering/changes/<name>/`
