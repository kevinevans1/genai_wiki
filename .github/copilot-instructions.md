# GitHub Copilot Instructions

This file provides context and instructions for GitHub Copilot when working with this repository.

## Project Overview

This repository documents the **Real-Time Audio Voice Agent** infrastructure deployed on Azure. The architecture enables real-time voice interactions powered by Azure AI Services and Azure Communication Services.

## Architecture Context

- **Project Name**: gbb-ai-audio-agent
- **Environment**: voice-agent-dev
- **Deployment Method**: Terraform via Azure Developer CLI (azd)
- **Region**: East US 2 (primary), Global (communication services)

## Key Components

1. **Frontend (rtaudio-client)**: Container App serving the web-based voice interface
2. **Backend (rtaudio-server)**: Container App handling real-time audio processing
3. **AI Services**: Azure AI Foundry for cognitive capabilities
4. **Communication**: Azure Communication Services for voice/calling

## When Generating Code

- Use Azure SDK for Python/JavaScript when interacting with Azure services
- Prefer managed identities over connection strings
- Reference App Configuration for runtime settings
- Use Key Vault references for secrets

## When Documenting

- Include Mermaid diagrams for architecture visualization
- Document all service dependencies and integrations
- Maintain the resource inventory in `/docs/resources/`
- Update runbooks when operational procedures change

## File Structure Conventions

```
/docs
  /architecture     - Architecture decision records and diagrams
  /resources        - Azure resource documentation
  /integrations     - Service integration guides
  /runbooks         - Operational procedures
/.github
  /copilot-instructions.md  - This file
```

## Azure Resource Naming Conventions

- Resource Group: `rg-{project}-{environment}`
- Container Apps: `{service}-{suffix}`
- Key Vault: `kv-{suffix}`
- Storage: `st{suffix}` (no hyphens)
- Container Registry: `cr{project}{suffix}`
