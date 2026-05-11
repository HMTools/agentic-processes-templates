# agentic-process-templates

Official template library for the [agentic-processes](https://github.com/user/agentic-processes) framework.

## Overview

This repository contains reusable process templates and step templates that can be consumed by the agentic-processes framework via git-based template sources.

## Structure

```
templates/
  processes/               # Process templates (full workflow definitions)
    development/           # Software development workflows
    infrastructure/        # Framework/tooling infrastructure workflows
    investigation/         # Code investigation and analysis workflows
    review/                # Review and verification workflows
    testing/               # Testing workflows
  steps/                   # Step templates (individual step definitions)
    _components/           # Shared components used across steps
    api/                   # API layer steps
    common/                # Common/shared steps
    data/                  # Data layer steps
    documentation/         # Documentation steps
    external-services/     # External service integration steps
    guideline/             # Guideline management steps
    investigation/         # Investigation steps
    learning/              # Learning and improvement steps
    multi-repo/            # Multi-repository operation steps
    planning/              # Planning and design steps
    service/               # Service layer steps
    template/              # Template management steps
    testing/               # Testing steps
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

## License

See [agentic-processes](https://github.com/user/agentic-processes) for license information.
