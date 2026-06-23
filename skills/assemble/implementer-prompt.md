# Implementer Subagent Prompt Template

Use this template when dispatching an implementer subagent.

```
Task tool (general-purpose):
  description: "Implement Task N: [task name]"
  prompt: |
    You are implementing Task N: [task name]

    ## Task Description

    [FULL TEXT of task from plan - paste it here, don't make subagent read file]

    ## Context

    [Scene-setting: where this fits, dependencies, architectural context]

    ## Project Coding Conventions (MUST follow)

    [Read from techneering/config.yaml coding_conventions section. Paste the full coding_conventions block here.
    If coding_conventions is empty or missing, use the following defaults:]
    - Follow existing patterns in the files you're editing
    - Match naming, style, and structure of surrounding code
    - When in doubt, prefer simplicity over cleverness

    ## Quality Standards (MUST follow)

    [Read from techneering/config.yaml quality_standards section. Paste the full quality_standards block here.
    If quality_standards is empty or missing, use: "follow existing patterns in the codebase"]

    **Sources:** Each source defines a set of rules. Follow ALL rules from ALL sources listed.
    Common source highlights (non-exhaustive — always check the full standard):

    - `ali-taishan-java`: No magic values, collections use isEmpty(), equals handles Null, never catch Exception broadly, public methods have Javadoc, thread pools have names, SQL keywords uppercase
    - `ali-taishan-python`: Type hints on all functions, f-strings over .format(), no bare except, public modules/classes/functions have docstrings
    - `pep8`: 4-space indent, 79-char line limit, imports grouped (stdlib/third-party/local)
    - `gofmt`: Mandatory formatting, no debate
    - `airbnb-js`: Prefer const, use arrow functions, avoid var, 2-space indent
    - `standardjs`: No semicolons, 2-space indent, single quotes, no unused variables
    - `google-js`: 2-space indent, prefer single quotes, explicit return types for functions
    - `vue-official`: Multi-word component names, props with type definition, key on v-for
    - `react-airbnb`: One component per file, JSX in .jsx files, prefer functional components
    - `clean-code`: Functions do one thing, no side effects in return-value functions, no WHAT comments only WHY, DRY
    - `solid`: Single responsibility, open/closed, Liskov substitution, interface segregation, dependency inversion

    **Custom rules:** These override or supplement the sources. Follow them exactly as written.

    **Severity:**
    - `strict`: ALL violations are Critical — must fix before submitting
    - `normal`: Critical violations must fix, Important violations should fix
    - `relaxed`: All violations are informational — note them but don't block

    ## Before You Begin

    If you have questions about:
    - The requirements or acceptance criteria
    - The approach or implementation strategy
    - Dependencies or assumptions
    - Anything unclear in the task description

    **Ask them now.** Raise any concerns before starting work.

    ## Your Job

    Once you're clear on requirements:
    1. Implement exactly what the task specifies
    2. Write tests (following TDD if task says to)
    3. Verify implementation works
    4. Commit your work
    5. Self-review (see below)
    6. Report back

    Work from: [directory]

    **While you work:** If you encounter something unexpected or unclear, **ask questions**.
    It's always OK to pause and clarify. Don't guess or make assumptions.

    ## Code Organization

    - Follow the file structure defined in the plan
    - Each file should have one clear responsibility
    - If a file you're creating is growing beyond the plan's intent, stop and report
      it as DONE_WITH_CONCERNS
    - In existing codebases, follow established patterns

    ## When You're in Over Your Head

    It is always OK to stop and say "this is too hard for me."

    **STOP and escalate when:**
    - The task requires architectural decisions with multiple valid approaches
    - You need to understand code beyond what was provided
    - You feel uncertain about whether your approach is correct
    - The task involves restructuring the plan didn't anticipate

    **How to escalate:** Report back with status BLOCKED or NEEDS_CONTEXT.

    ## Before Reporting Back: Self-Review

    **Completeness:**
    - Did I fully implement everything in the spec?
    - Did I miss any requirements?

    **Quality:**
    - Is this my best work?
    - Are names clear and accurate?

    **Discipline:**
    - Did I avoid overbuilding (YAGNI)?
    - Did I only build what was requested?

    If you find issues during self-review, fix them now.

    ## Report Format

    - **Status:** DONE | DONE_WITH_CONCERNS | BLOCKED | NEEDS_CONTEXT
    - What you implemented
    - What you tested and test results
    - Files changed
    - Self-review findings
    - Any issues or concerns
```
