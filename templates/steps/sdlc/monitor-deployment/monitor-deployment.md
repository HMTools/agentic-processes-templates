# Step: Monitor Deployment

**Category**: sdlc
**Used in**: deploy-to-testing

## Purpose

Poll the CD pipeline run status until completion (terminal state or timeout). Monitors deployment stages sequentially based on `target_environments` order. If an earlier environment (e.g. QA) fails, later environments (e.g. SB) are not attempted.

## Key Behavior

- **Polling**: Checks pipeline status at `cd.poll_interval_seconds` (default 30s) intervals
- **Timeout**: Enforces `cd.timeout_minutes` (default 20min) from trigger time
- **Sequential stages**: Monitors QA first, then SB — stops on first failure
- **Per-environment tracking**: Records individual status for each target environment
- **Progress reporting**: Outputs status at each poll interval with stage, elapsed time

## Substeps

0. **Get Run ID from Memory** — Retrieve cdPipelineRunId and config for polling
1. **Poll Pipeline Status** — Check status at intervals until terminal state or timeout
2. **Monitor Environment Stages** — Track per-environment deployment progress in order
3. **Report Progress** — Output status updates during monitoring
4. **Record Final Status** — Store cdPipelineStatus and cdEnvironmentStatuses in memory
