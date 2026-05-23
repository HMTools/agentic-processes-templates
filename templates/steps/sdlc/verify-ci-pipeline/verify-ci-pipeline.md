# Step: Verify CI Pipeline

**Category**: sdlc
**Used in**: implement-work-item

## Purpose

Check CI/CD pipeline status after git push. If the pipeline fails, extract failure details and loop back to the implementation step for targeted fixes. Supports configurable max retries.

## Key Behavior

- **Pipeline polling**: Waits for the CI pipeline to reach a terminal state before checking
- **Failure extraction**: Pulls build logs, error messages, and failing test names for actionable context
- **Loop support**: If CI fails and retries < max, loops back to Execute Implementation with failure details
- **Retry tracking**: Tracks retry iteration count and respects `maxIterations` from template parameter

## Failure Categories

| Category | Description | Action |
|----------|-------------|--------|
| Build error | Compilation/build failure | Loop back — code fix needed |
| Test failure | Unit/integration test fails | Loop back — code fix needed |
| Infrastructure | Agent/environment issue | May retry without code change |
| Timeout | Pipeline exceeded time limit | Investigate — may be code or infra |

## Substeps

1. **Identify CI pipeline** — Find the relevant pipeline for current repo/branch
2. **Wait for completion** — Poll until pipeline reaches terminal state
3. **Evaluate result** — Check pass/fail and categorize failure type
4. **Extract failure context** — (if failed) Pull logs, errors, failing tests
5. **Make retry decision** — Pass → proceed; fail → loop back or proceed if cap reached

## Retry Decision Matrix

| Pipeline Status | Retries < Max | Action |
|-----------------|---------------|--------|
| Passed | — | Proceed to Continuous Improvement |
| Failed | Yes | Loop back to Execute Implementation |
| Failed | No | Log warning, proceed to Continuous Improvement |
