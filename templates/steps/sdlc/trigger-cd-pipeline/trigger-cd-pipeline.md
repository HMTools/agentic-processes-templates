# Step: Trigger CD Pipeline

**Category**: sdlc
**Used in**: deploy-to-testing

## Purpose

Trigger the CD/release pipeline using configuration loaded in the previous step. Unlike CI (which is auto-triggered by push), CD must be actively triggered via the ADO API. Passes source branch and environment parameters to the pipeline run.

## Key Behavior

- **Active trigger**: CD pipelines are triggered via API, not auto-triggered by push (key difference from CI)
- **Config-driven**: Uses deployConfig.cd from memory (loaded by load-deploy-config)
- **Branch resolution**: Uses cd.source_branch if set, otherwise detects current git branch
- **Failure handling**: Captures trigger failures with actionable error details

## Substeps

0. **Get Pipeline Config from Memory** — Read deployConfig.cd from memory
1. **Determine Source Branch** — Use cd.source_branch override or detect current git branch
2. **Trigger CD Pipeline** — Call mcp__azuredevops__pipelines_run with pipeline ID and branch
3. **Record Run ID and Status** — Store cdPipelineRunId and cdTriggerStatus in memory
4. **Handle Trigger Failures** — (conditional) Capture and categorize trigger errors
