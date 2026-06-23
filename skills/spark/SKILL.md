---
name: spark
description: "You MUST use this before any creative work — creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation."
---

# Sparking Ideas Into Designs

Help turn ideas into fully formed designs through natural collaborative dialogue. Explore intent, refine requirements, compare approaches — then hand off to draft for formal documentation.

## Core Principle

```
NO IMPLEMENTATION WITHOUT A DESIGN.
Present a design and get user approval before writing any code.
```

## Steps

### Step 1: Explore Project Context

Check files, docs, recent commits. Understand the current state before asking questions.

**For existing projects (code already exists):**

#### 1a. Project Structure Analysis
- Scan directory structure, identify layering pattern (MVC / layered / feature-based / modular / monorepo)
- Identify file/directory naming convention (kebab-case / PascalCase / camelCase / snake_case)

#### 1b. Code Pattern Analysis
Sample 3-5 source files and identify:
- **Naming conventions:** files, classes, functions, constants, variables (camelCase / snake_case / PascalCase / UPPER_SNAKE)
- **Error handling:** exceptions / Result type / error codes / try-catch
- **Dependency injection:** constructor / annotations / manual / module system
- **API style:** REST / GraphQL / gRPC (if applicable)
- **Data access:** ORM / query-builder / raw-sql / repository pattern
- **Import style:** alias (@/) / relative / absolute / package-based

#### 1c. Formatting Style Analysis
- Check for config files: `.editorconfig`, `.prettierrc`, `.eslintrc`, `biome.json`
- If no config found, infer from code: indent (tab/space, 2/4), quotes (single/double), semicolons, trailing commas

#### 1d. Summarize Conventions
Produce a `coding_conventions` summary as part of spark output. This will be written to `techneering/config.yaml` by tn:draft.

#### 1e. Tech Stack Detection & Quality Standards Recommendation
Detect project tech stack from build files and recommend matching quality standards:

| Detected File | Tech Stack | Recommended Source |
|--------------|------------|-------------------|
| `pom.xml` / `build.gradle` | Java | `ali-taishan-java` |
| `requirements.txt` / `pyproject.toml` | Python | `ali-taishan-python` + `pep8` |
| `go.mod` | Go | `gofmt` |
| `package.json` + `vue` | Vue | `vue-official` |
| `package.json` + `react` | React | `react-airbnb` |
| `package.json` + `typescript` | TypeScript | `google-js` |
| `package.json` (other) | JavaScript | prompt user to choose |

Present the recommendation to the user and ask for confirmation. Include the full options table from tn:draft if the user wants to see alternatives.

**For new projects (empty or freshly initialized):**

#### 1a. Extract Requirements Keywords
From the user's description, identify technical requirements (e.g., "real-time", "SEO", "mobile-first", "high-performance", "admin dashboard").

#### 1b. Framework Research
Based on requirements, search for available frameworks. Present 2-3 options with:
- Maturity / ecosystem
- Learning curve
- Best-fit scenarios
- Your recommendation with reasoning

#### 1c. Recommend Standard Style
Based on the chosen framework, recommend a matching code style and project structure.

### Step 2: Assess Scope

If the request describes multiple independent subsystems, flag immediately and help decompose into sub-projects. Each sub-project gets its own spark → draft → forge cycle.

### Step 3: Web Research (on demand)

The model's training data has a cutoff. Before committing to a design, check whether the real world still agrees with you.

**Search when:**
- The request involves a framework, library, API, or tool that moves fast (e.g. "use the latest React patterns", "set up a build pipeline")
- It's a new project and you're recommending a stack — confirm the options are still current and mature
- The user names a specific version, package, or feature flag — verify it exists and behaves as assumed

**How:** Use the built-in `WebSearch` to find current docs, benchmarks, or community recommendations; use `WebFetch` on a trusted URL (official docs, the library's README) when you need the detail. Keep it lightweight — **1-3 searches** is usually enough to confirm a direction, not a full literature review.

**Skip the search when:**
- It's pure business logic, internal architecture, or a domain the user knows better than the web does
- The user has already specified the exact approach, version, or library
- You've searched this exact question recently in the same conversation

**Bring the findings back to the user** — cite what you found (a doc, a benchmark, a known issue) and explain how it shaped the proposed design. If the search contradicts the user's assumption, surface it politely rather than silently overriding.

### Step 4: Ask Clarifying Questions

- One question at a time
- Prefer multiple choice when possible
- Focus on: purpose, constraints, success criteria
- Don't follow a script — let questions emerge naturally

### Step 5: Propose 2-3 Approaches

- Present options with trade-offs
- Lead with your recommendation and explain why
- Cover architecture, data flow, error handling, testing

### Step 6: Present Design

- Scale each section to its complexity
- Ask after each section whether it looks right
- Cover: architecture, components, data flow, error handling, testing
- Be ready to go back and clarify if something doesn't make sense

### Step 7: Get User Approval

Design approved → transition to draft. Design rejected → revise and re-present.

## Red Flags

| Thought | Reality |
|---------|---------|
| "Just start implementing" | Implementation without design is vibe coding, not spec coding |
| "Too simple to need spark" | Even simple problems need confirmation that you understood them |
| "Ask all questions at once" | One question at a time lets you dig deeper |
| "Only one design option exists" | You must propose 2-3 approaches for comparison |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Role | Thinking phase, writes no files |
| Output | Conclusions stay in the conversation |
| Terminal state | Invoke tn:draft to document the conclusions |
| One question at a time | Never ask multiple questions at once |
| Web research | On demand — confirm fast-moving tech before committing, 1-3 searches max |
| YAGNI | Remove unnecessary features from all designs |

## Next Step

When done:
- Design approved → invoke `tn:draft` to turn conclusions into a formal proposal
- Design needs revision → keep revising until approved
- User requests stop → end the current flow

## Guardrails

- **Never implement** — Do not write code, scaffold projects, or take any implementation action
- **Never write files** — Spark produces thinking conclusions that stay in conversation
- **Never skip design approval** — Even "simple" projects need a design review
- **Never invoke other skills** — The ONLY skill to invoke after spark is `tn:draft`
- Do break systems into smaller units with clear purpose and interfaces
- Do propose 2-3 approaches before settling on one
- Do go back and clarify when something doesn't make sense
