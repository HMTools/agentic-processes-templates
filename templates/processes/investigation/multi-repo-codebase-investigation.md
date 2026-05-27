# Process: {{researchQuestion}}

**Template**: multi-repo-codebase-investigation
**Status**: Not Started

## Description

Systematic investigation of multiple repositories to answer a specific research question. This template orchestrates per-repository investigation subprocesses, aggregates findings across repositories, and produces a comprehensive comparative report. Supports both remote repositories (with on-demand cloning and automatic cleanup) and local paths.

## Purpose & Usage

Use this template when you need to:
- Investigate how a feature or pattern is implemented across multiple microservices or codebases
- Compare implementations and practices across different repositories
- Gather metrics or data from multiple projects simultaneously
- Understand architectural decisions made independently across teams
- Analyze security, testing, or configuration practices across repositories

**Not suitable for**: Single-repository investigations, general code search without a focused research question, or tasks that do not require cross-repository comparison.

## Quick Reference

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `researchQuestion` | Yes | - | The specific question or investigation goal to answer across repositories |
| `repositories` | Yes | - | List of repository URLs or local paths to investigate (comma-separated) |
| `investigationScope` | No | - | Specific areas or patterns to focus on |
| `filePatterns` | No | - | File patterns to search within each repository |
| `excludePatterns` | No | - | Patterns to exclude from investigation |
| `outputFormat` | No | "markdown" | Desired output format for findings |

## Process Flow

```mermaid
graph TD
    A[Start] --> C[Understand Investigation Context]
    C --> D{Context Approved?}
    D -->|No| C
    D -->|Yes| E[Parse Repository List<br/>Lightweight parsing only]
    E --> F[Spawn Sub-Process for Each Repo]
    F --> G[Sub-Process 1:<br/>Clone → Investigate → Cleanup]
    F --> H[Sub-Process 2:<br/>Clone → Investigate → Cleanup]
    F --> I[Sub-Process N:<br/>Clone → Investigate → Cleanup]
    G --> J[SYNC POINT<br/>Aggregate and Synthesize]
    H --> J
    I --> J
    J --> K{Findings Approved?}
    K -->|No - Revise| J
    K -->|Yes| L[Create Investigation Report]
    L --> P[End]
```

## Steps

- [ ] Step 1: Understand Investigation Context (@step:planning/understand-context)
  - Output: Context documentation with research question, repositories, and scope
  - Approval: Required before proceeding

- [ ] Step 2: Parse Repository List (@step:multi-repo/parse-repository-list)
  - Output: `repositories-list.json` with each repo's name, source, type (remote/local), and tracking fields

- [ ] Step 3: Investigate Repositories - Subprocess Loop (sub-process spawning via subProcessTemplate metadata)
  - Sub-Process Template: `investigate-single-repo`
  - Output: Completed sub-processes for each repository; `repositories-list.json` updated with subprocess IDs and statuses
  - Note: Each subprocess handles clone (if remote), investigation, and cleanup autonomously

- [ ] Step 4: Aggregate and Synthesize Findings - SYNC POINT (@step:multi-repo/aggregate-and-synthesize-findings)
  - Output: `comparative-analysis.md` with cross-repository patterns, differences, and recommendations
  - Approval: Required before proceeding to report creation

- [ ] Step 5: Create Investigation Report (@step:investigation/create-research-report)
  - Output: `investigation-report.{md|json}` with executive summary, per-repository findings, comparative analysis, and recommendations

