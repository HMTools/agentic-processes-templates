# Step: Self Code Review

**Category**: sdlc
**Used in**: implement-work-item

## Purpose

Perform an independent code review of implementation changes in an isolated subagent context. Reviews code against the implementation plan, codebase patterns, and quality standards. Supports iterative loop-back to the implementation step when issues are found.

## Key Behavior

- **Isolated context**: MUST run in a separate subagent window — does not share the implementation agent's context
- **Loop support**: If issues found and iterations < max, loops back to Execute Implementation with findings
- **Iteration tracking**: Tracks review iteration count and respects `maxIterations` from template parameter

## Review Criteria

| Priority | Category | Examples |
|----------|----------|----------|
| 1 | Bugs | Null refs, off-by-one, race conditions |
| 2 | Security | Injection, auth bypass, secrets in code |
| 3 | Logic errors | Wrong branching, missing cases |
| 4 | Pattern violations | Inconsistent with codebase conventions |
| 5 | Improvements | Better naming, simpler approach |

## Substeps

1. **Load review context** — Read implementation results and plan from memory, determine iteration number
2. **Review code changes** — Examine each changed file using git diff and file reads
3. **Compile findings** — Create structured list with file, location, severity, suggested fix
4. **Make loop decision** — Clean → proceed; needs-fixes → loop back or proceed if cap reached

## Loop Decision Matrix

| Verdict | Iteration < Max | Action |
|---------|-----------------|--------|
| Clean | — | Proceed to Run Tests |
| Needs fixes | Yes | Loop back to Execute Implementation |
| Needs fixes | No | Log warning, proceed to Run Tests |
