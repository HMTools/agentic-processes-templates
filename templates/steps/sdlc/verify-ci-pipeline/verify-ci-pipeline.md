# Step: Verify CI Pipeline

**Category**: sdlc
**Used in**: implement-work-item

## Purpose

Check CI/CD pipeline status after git push. Loads pipeline configuration from `.ai/deploy-config.json` (with auto-discovery fallback). If the pipeline fails, extract failure details and loop back to the implementation step for targeted fixes. Supports configurable max retries.

## Config: `.ai/deploy-config.json`

Per-repo config file storing pipeline settings. Currently holds CI (build) config; will be extended with CD (deployment/release) in the future.

```json
{
  "ado_project": "Payoneer",
  "ci": {
    "pipeline_definition_id": 12345,
    "pipeline_name": "my-service-CI",
    "timeout_minutes": 15,
    "poll_interval_seconds": 30
  }
}
```

| Field | Required | Description | Default |
|-------|----------|-------------|---------|
| `ado_project` | Yes | ADO project name | — |
| `ci.pipeline_definition_id` | Yes | Pipeline definition ID to monitor | — |
| `ci.pipeline_name` | No | Pipeline name (for display/matching) | — |
| `ci.timeout_minutes` | No | Max wait time for pipeline | 15 |
| `ci.poll_interval_seconds` | No | Polling interval | 30 |

**Auto-discovery**: If the config file is missing or has null required fields, the step auto-discovers by listing pipeline definitions from ADO for the current repo. Discovered values are presented to the user for confirmation before use.

## Key Behavior

- **Config-driven**: Reads `.ai/deploy-config.json` to know which pipeline to check
- **Auto-discovery**: Falls back to ADO pipeline listing if config is missing, with user approval
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

0. **Load Deploy Config** — Read `.ai/deploy-config.json` or auto-discover from ADO (with user approval)
1. **Identify CI pipeline run** — Find the build triggered by the recent push
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
