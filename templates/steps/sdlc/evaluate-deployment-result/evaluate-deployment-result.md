# Step: Evaluate Deployment Result

**Category**: sdlc
**Used in**: deploy-to-testing

## Purpose

Evaluate the CD pipeline outcome, extract failure details if deployment failed, and decide whether to proceed, retry (loop back to implementation), or report that the retry cap has been reached. Failure details are stored in memory so the implementation step can target the fix on retry.

## Key Behavior

- **Result evaluation**: Checks cdPipelineStatus from memory (succeeded/failed/canceled/timeout)
- **Failure extraction**: Reads build logs and extracts actionable error details
- **Failure categorization**: Classifies into deployment config, startup, health check, environment, infrastructure, or timeout
- **Retry management**: Tracks cdRetryIteration against maxDeploymentRetries
- **Actionable context**: On retry, stores failure details so implementation step can target the fix

## Failure Categories

| Category | Description | Action |
|----------|-------------|--------|
| deployment_config_error | Pipeline config or parameter issues | Loop back — config fix needed |
| application_startup_failure | App fails to start in environment | Loop back — code fix needed |
| health_check_failure | App starts but health checks fail | Loop back — code/config fix needed |
| environment_issue | Target environment unhealthy | May retry without code change |
| infrastructure_failure | Agent/network/platform issues | May retry without code change |
| timeout | Pipeline exceeded time limit | Investigate — may be code or infra |

## Decision Matrix

| Pipeline Status | Retries < Max | Action |
|-----------------|---------------|--------|
| Passed | — | Proceed (cdDecision: passed) |
| Failed | Yes | Loop back to implementation (cdDecision: retry) |
| Failed | No | Log warning, proceed (cdDecision: cap-reached) |

## Substeps

0. **Check Deployment Status** — Read cdPipelineStatus and cdEnvironmentStatuses from memory
1. **Extract Failure Details** — (conditional) Read build logs, extract errors and affected environments
2. **Categorize Failure** — (conditional) Classify failure type for appropriate handling
3. **Check Retry Count** — (conditional) Compare cdRetryIteration against maxDeploymentRetries
4. **Make Decision and Store Results** — Set cdDecision and store all details in memory
