# Process: Implement Work Item {{workItemId}}

**Template**: implement-work-item
**Status**: Not Started

## Description

Implement an Azure DevOps work item based on an approved implementation plan. Reviews the plan, executes code changes following the plan's step order, runs tests to verify no regressions, and prepares clean commits for review.

## Purpose & Usage

Use this template when you need to:
- Implement code changes for an ADO work item based on an existing plan
- Follow a structured implementation workflow with test verification
- Prepare changes for code review with clear commit history

**Not suitable for**: Exploratory prototyping, planning (use plan-work-item), or test-only changes.

## Quick Reference

| Parameter | Required | Description |
|-----------|----------|-------------|
| `workItemId` | Yes | Azure DevOps work item ID |

## Process Flow

```mermaid
flowchart TD
    A[Start: Work Item ID + Plan] --> B[Step 0: Review Implementation Plan]
    B --> C[Step 1: Execute Implementation]
    C --> D{Implementation Approved?}
    D -->|No| E[Revise Implementation]
    E --> C
    D -->|Yes| F[Step 2: Run Tests]
    F --> G[Step 3: Prepare for Review]
    G --> H{Ready for Review?}
    H -->|No| I[Fix Issues]
    I --> G
    H -->|Yes| J[Step 4: Continuous Improvement]
    J --> K[Step 5: End Process Validation]
    K --> L[End: Implementation Complete]
```

## Steps Summary

| Step | Name | Approval |
|------|------|----------|
| 0 | Review Implementation Plan | No |
| 1 | Execute Implementation | Yes |
| 2 | Run Tests | No |
| 3 | Prepare for Review | Yes |
| 4 | Continuous Improvement | Yes |
| 5 | End Process Validation | No |
