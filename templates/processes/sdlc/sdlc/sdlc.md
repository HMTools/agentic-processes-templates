# Process: SDLC for Work Item {{workItemId}}

**Template**: sdlc
**Status**: Not Started

## Description

End-to-end SDLC orchestrator for Azure DevOps work items. Spawns sub-processes for each phase: planning, test planning, implementation, and test execution. Each sub-process is a native agentic-processes template with its own steps, approval gates, memory, and logging.

## Purpose & Usage

Use this template when you need to:
- Implement an ADO work item (PBI, Bug, Feature) with full SDLC coverage
- Follow a structured development workflow with planning, testing, and implementation phases
- Ensure consistent quality and traceability across the development process

**Not suitable for**: Quick bug fixes without test plans or documentation-only changes.

## Quick Reference

| Parameter | Required | Description |
|-----------|----------|-------------|
| `workItemId` | Yes | Azure DevOps work item ID |

## Process Flow

```mermaid
flowchart TD
    A[Start: ADO Work Item ID] --> B[Step 0: Spawn plan-work-item]

    B --> B1[Sub-Process: plan-work-item]
    B1 --> B2[Fetch WI → Analyze Code → Create Plan]
    B2 --> B3{Plan Approved?}
    B3 -->|No| B2
    B3 -->|Yes| C[Step 1: Spawn create-test-plan]

    C --> C1[Sub-Process: create-test-plan]
    C1 --> C2[Config → Sniffing → Context → Test Cases → Push ADO]
    C2 --> C3{Test Plan Approved?}
    C3 -->|No| C2
    C3 -->|Yes| D[Step 2: Spawn implement-work-item]

    D --> D1[Sub-Process: implement-work-item]
    D1 --> D2[Review Plan → Implement → Test → Prepare]
    D2 --> D3{Implementation Approved?}
    D3 -->|No| D2
    D3 -->|Yes| E[Step 3: Spawn run-test-suite]

    E --> E1[Sub-Process: run-test-suite]
    E1 --> E2[Setup → Fetch TCs → Execute → Report]
    E2 --> E3{Results Approved?}
    E3 -->|No| F[Fix & Re-run]
    F --> D
    E3 -->|Yes| G[Step 4: Continuous Improvement]

    G --> H[Step 5: End Process Validation]
    H --> I[End: Work Item Complete]
```

## Steps Summary

| Step | Name | Sub-Process | Approval |
|------|------|-------------|----------|
| 0 | Plan Work Item | `sdlc/plan-work-item` | In sub-process |
| 1 | Create Test Plan | `sdlc/create-test-plan` | In sub-process |
| 2 | Implement Work Item | `sdlc/implement-work-item` | In sub-process |
| 3 | Run Test Suite | `sdlc/run-test-suite` | In sub-process |
| 4 | Continuous Improvement | — | Yes |
| 5 | End Process Validation | — | No |
