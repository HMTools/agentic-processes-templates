# agentic-processes-templates

Official template library for the [agentic-processes](https://github.com/HMTools/agentic-processes) framework.

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

This repository is pre-configured as the `official` marketplace out of the box, so no setup is required:

1. Open the **Marketplace** section in the UI
2. Click **Refresh** to fetch the template catalog
3. Browse and install the templates you need

To add it manually (e.g. under a different name or priority), use **Add Marketplace** in the Marketplace Sources section with URL `https://github.com/HMTools/agentic-processes-templates.git`.

### Multi-Marketplace Configuration

The framework supports multiple marketplaces. Each marketplace is refreshed independently. Configure additional marketplaces via the UI Settings or in `~/.claude/agentic-processes/config/marketplaces.json`.

## Related Repositories

- **[sdlc-process-templates](https://github.com/HMTools/sdlc-process-templates)** -- SDLC process templates (work item implementation, test planning, deployment, PR review, etc.)

## License

See [agentic-processes](https://github.com/HMTools/agentic-processes) for license information.
