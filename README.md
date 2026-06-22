# agentic-process-templates

Official template library for the [agentic-processes](https://github.com/user/agentic-processes) framework.

## Overview

This repository contains the **core infrastructure** process templates for the agentic-processes framework. Domain-specific templates (e.g., SDLC workflows) are maintained in separate repositories. Step definitions live as subdirectories within each process template.

## Structure

```
templates/
  processes/               # Process templates (full workflow definitions)
    infrastructure/        # Framework/tooling infrastructure workflows
      create-process-template/
        create-process-template.json
        plan-and-design-template/   # Step subdirectory
        create-template-file/       # Step subdirectory
        validate-process-steps-exist/  # Step subdirectory
      ...
```

## Usage

Add this repository as a marketplace in the UI Settings:

1. Open the **Marketplace** section in the UI Settings
2. Click **Add Marketplace**
3. Enter name: `official`, URL: `https://github.com/user/agentic-process-templates.git`, priority: `100`
4. Click **Refresh** to fetch the template catalog
5. Browse and install the templates you need

### Multi-Marketplace Configuration

The framework supports multiple marketplaces. Each marketplace is refreshed independently. Configure additional marketplaces via the UI Settings or in `~/.claude/agentic-processes/config/marketplaces.json`.

## Related Repositories

- **[sdlc-process-templates](https://github.com/user/sdlc-process-templates)** -- SDLC process templates (work item implementation, test planning, deployment, PR review, etc.)

## License

See [agentic-processes](https://github.com/user/agentic-processes) for license information.
