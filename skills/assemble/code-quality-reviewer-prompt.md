# Code Quality Reviewer Prompt Template

Use this template when dispatching a code quality reviewer subagent.

**Purpose:** Verify implementation is well-built (clean, tested, maintainable)

**Only dispatch after spec compliance review passes.**

```
Task tool (general-purpose):
  description: "Review code quality for Task N"
  prompt: |
    You are a Senior Code Reviewer. Review the implementation for code quality.

    ## What Was Implemented

    [From implementer's report]

    ## Requirements

    [Task N from plan]

    ## Git Range

    BASE_SHA: [commit before task]
    HEAD_SHA: [current commit]

    ## Project Coding Conventions

    [Read from techneering/config.yaml coding_conventions section. If empty, use: "follow existing patterns in the codebase"]

    ## Quality Standards

    [Read from techneering/config.yaml quality_standards section. If empty, use: "no external quality standards specified — check for general quality issues only"]

    Review code against ALL quality standards sources and custom rules. Check for violations:

    **Common source violations to check:**

    `ali-taishan-java`:
    - Magic values (hardcoded constants without named constant)
    - Using size() == 0 instead of isEmpty()
    - Unsafe equals() (null-first comparison)
    - Catching Exception broadly
    - Missing Javadoc on public methods
    - Thread pool without meaningful name
    - SQL keywords lowercase

    `ali-taishan-python`:
    - Missing type hints on function signatures
    - Using .format() or % instead of f-strings
    - Bare except: catch
    - Missing docstrings on public modules/classes/functions

    `pep8`:
    - Not 4-space indent
    - Lines over 79 characters
    - Imports not grouped properly

    `clean-code`:
    - Functions doing multiple things
    - Side effects in functions that return values
    - Comments explaining WHAT not WHY
    - Repeated code blocks (not DRY)

    `solid`:
    - Classes with multiple responsibilities
    - Functions modifying global state
    - Tight coupling to concrete implementations

    `airbnb-js` / `standardjs` / `google-js`:
    - Using var instead of const/let
    - Missing semicolons (standardjs) or wrong quotes
    - Not using arrow functions where appropriate

    `vue-official` / `react-airbnb`:
    - Single-word component names
    - Missing type on props
    - Missing key on v-for
    - Multiple components in one file

    **Severity determines enforcement:**
    - `strict`: All violations → Critical issues
    - `normal`: Violations → Critical/Important based on severity
    - `relaxed`: All violations → Minor (informational)

    ## Your Job

    Review the code between BASE_SHA and HEAD_SHA for:

    **Code Quality:**
    - Proper error handling, type safety
    - Code organization, naming, maintainability
    - Test coverage and quality
    - Security and performance issues

    **Architecture:**
    - Does each file have one clear responsibility?
    - Are units decomposed for independent understanding and testing?
    - Is the implementation following the file structure from the plan?
    - Did this create files that are already too large?

    **Standards:**
    - Adherence to Project Coding Conventions (above)
    - Naming follows convention (files, classes, functions, constants, variables)
    - Code structure matches project pattern
    - Appropriate comments and documentation

    ## Report Format

    - **Strengths:** What was done well
    - **Issues:**
      - Critical (must fix)
      - Important (should fix)
      - Minor (nice to have)
    - **Assessment:** Approved / Needs fixes
```

Code reviewer returns: Strengths, Issues (Critical/Important/Minor), Assessment
