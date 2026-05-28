# agentic-process-templates

Official template library for the [agentic-processes](https://github.com/user/agentic-processes) framework.

## Overview

This repository contains the **core infrastructure** process templates and shared step templates for the agentic-processes framework. Domain-specific templates (e.g., SDLC workflows) are maintained in separate repositories.

## Structure

```
templates/
  processes/               # Process templates (full workflow definitions)
    infrastructure/        # Framework/tooling infrastructure workflows
  steps/                   # Step templates (individual step definitions)
    common/                # Common/shared steps (apply-changes, etc.)
    guideline/             # Guideline management steps
    investigation/         # Investigation steps (identify-files, review-verify-document)
    learning/              # Learning and improvement steps
    planning/              # Planning and design steps (design-implementation-plan, understand-context)
    template/              # Template management steps
```

## Usage

Add this repository as a template source in your agentic-processes configuration:

```bash
python scripts/template_manager.py add-source \
  --name official \
  --url https://github.com/user/agentic-process-templates.git \
  --priority 100
```

Then sync templates:

```bash
python scripts/template_manager.py sync
```

### Multi-Source Configuration

The framework supports multiple template sources. Each source is synced independently and merged by priority (higher priority wins on conflicts). Configure additional sources in `~/.claude/agentic-processes/config/template-sources.json`.

## Related Repositories

- **[sdlc-process-templates](https://github.com/user/sdlc-process-templates)** -- SDLC process and step templates (work item implementation, test planning, deployment, PR review, etc.)

## License

See [agentic-processes](https://github.com/user/agentic-processes) for license information.
