# Process: Create Test Plan for Work Item {{workItemId}}

**Template**: create-test-plan
**Status**: Not Started

## Description

Create comprehensive ADO test plans for backend work items. Loads team config, fetches work item details, runs sniffing checklists with code cross-reference, generates test cases following guidelines, maps against existing coverage, and pushes approved test cases to Azure DevOps.

## Purpose & Usage

Use this template when you need to:
- Create a test plan for an ADO work item
- Generate test cases with code-informed sniffing
- Map new test cases against existing coverage (new/update/skip/obsolete)
- Push test cases to ADO with proper suite and linking

**Not suitable for**: Unit test creation (code-level), manual exploratory testing, or non-backend work items.

## Quick Reference

| Parameter | Required | Description |
|-----------|----------|-------------|
| `workItemId` | Yes | Azure DevOps work item ID |

## Process Flow

```mermaid
flowchart TD
    A[Start: Work Item ID] --> B[Step 0: Load & Validate Config]
    B --> B1{Config Confirmed?}
    B1 -->|No| B
    B1 -->|Yes| C[Step 1: Pull Work Item Details]
    C --> D[Step 2: Pull Sniffing Guidelines]
    D --> E[Step 3: Execute Sniffing]
    E --> F[Step 4: Q&A Session if needed]
    F --> G[Step 5: Generate Test Plan Context]
    G --> G1{Context Approved?}
    G1 -->|No| G
    G1 -->|Yes| H[Step 6: Get Test Case Guidelines]
    H --> I[Step 7: Generate Test Cases & Map Coverage]
    I --> I1{TCs Approved?}
    I1 -->|No| I
    I1 -->|Yes| J[Step 8: Push Test Plan to ADO]
    J --> K[Step 9: Report Results]
    K --> L[Step 10: Continuous Improvement]
    L --> M[Step 11: End Process Validation]
    M --> N[End: Test Plan Complete]
```

## Steps Summary

| Step | Name | Approval |
|------|------|----------|
| 0 | Load & Validate Config | Yes |
| 1 | Pull Work Item Details | No |
| 2 | Pull Sniffing Guidelines | No |
| 3 | Execute Sniffing | No |
| 4 | Q&A Session (if needed) | No |
| 5 | Generate Test Plan Context | Yes |
| 6 | Get Test Case Guidelines | No |
| 7 | Generate Test Cases & Map Coverage | Yes |
| 8 | Push Test Plan to ADO | No |
| 9 | Report Results | No |
| 10 | Continuous Improvement | Yes |
| 11 | End Process Validation | No |
