# Step: Load Deploy Config

**Category**: sdlc
**Used in**: deploy-to-testing

## Purpose

Read `.ai/deploy-config.json` cd section, validate required fields (ado_project, cd.pipeline_definition_id, cd.target_environments), auto-discover from ADO if missing, and present to user for approval. Shares the deploy-config.json file with verify-ci-pipeline (which owns the ci section).

## Config: `.ai/deploy-config.json` — cd section

```json
{
  "ado_project": "Payoneer",
  "cd": {
    "pipeline_definition_id": 67890,
    "pipeline_name": "my-service-CD",
    "target_environments": ["QA", "SB"],
    "timeout_minutes": 20,
    "poll_interval_seconds": 30,
    "source_branch": null
  }
}
```

| Field | Required | Description | Default |
|-------|----------|-------------|---------|
| `ado_project` | Yes | ADO project name (shared with CI) | — |
| `cd.pipeline_definition_id` | Yes | CD pipeline definition ID to trigger | — |
| `cd.target_environments` | Yes | Ordered array of deployment targets | — |
| `cd.pipeline_name` | No | Pipeline name (for display/matching) | — |
| `cd.timeout_minutes` | No | Max wait time for pipeline | 20 |
| `cd.poll_interval_seconds` | No | Polling interval | 30 |
| `cd.source_branch` | No | Source branch override | current branch |

**Payoneer standard**: `target_environments` controls "highest environment" — `["QA"]` for QA only, `["QA", "SB"]` for QA then sandbox.

**Auto-discovery**: If the config file is missing or has null required fields, the step auto-discovers by listing pipeline definitions from ADO. Discovered values are presented to the user for confirmation.

## Key Behavior

- **Memory-aware**: Checks memory first — CI step may have already loaded shared fields (ado_project)
- **Config-driven**: Reads `.ai/deploy-config.json` cd section
- **Auto-discovery**: Falls back to ADO pipeline listing if config is missing, with user approval
- **Retry-aware**: If cdRetryIteration exists in memory, preserves config from previous attempt

## Substeps

0. **Check Memory for Existing Config** — Reuse deployConfig from CI step if available
1. **Read Deploy Config File** — Read `.ai/deploy-config.json` and extract cd section
2. **Validate CD Section** — Ensure required fields present, apply defaults
3. **Auto-Discover if Missing** — (conditional) Query ADO for pipeline definitions
4. **Present Config for Approval** — Show resolved config for user confirmation
5. **Store Config in Memory** — Persist deployConfig (cd section) and cdRetryIteration
