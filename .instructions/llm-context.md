# LLM Context Instructions

This file provides context for Large Language Models (LLMs) when working with this repository.

## Repository Purpose

This repository is an **Engineering Wiki** documenting the **Real-Time Audio Voice Agent** infrastructure deployed on Azure. It serves as the single source of truth for:

- Architecture documentation
- Resource inventory
- Integration patterns
- Operational runbooks
- Dependency mapping

## Project Context

### What This System Does

The Real-Time Audio Voice Agent enables natural voice conversations with AI:

1. **Users speak** into a web interface or call a phone number
2. **Audio is streamed** in real-time to Azure Container Apps
3. **Azure AI Services** transcribe speech to text
4. **GPT models** generate intelligent responses
5. **Text-to-Speech** converts responses to natural speech
6. **Audio is played back** to the user

### Key Technologies

- **Compute**: Azure Container Apps (serverless containers)
- **AI**: Azure AI Foundry (Speech, GPT-4, TTS)
- **Communication**: Azure Communication Services (Voice, Email)
- **Infrastructure**: Terraform via Azure Developer CLI (azd)
- **Security**: Managed Identities, Key Vault

## When Modifying This Wiki

### Documentation Standards

1. **Use Mermaid diagrams** for all architecture visualizations
2. **Include code examples** in Python (backend) or JavaScript (frontend)
3. **Keep runbooks actionable** with copy-paste commands
4. **Update inventory** when resources change
5. **Document decisions** in Architecture Decision Records (ADRs)

### File Organization

```
/docs
  /architecture     - How the system works
  /resources        - What's deployed
  /integrations     - How services connect
  /runbooks         - How to operate
  /dependencies     - What depends on what
```

### Code Examples

When writing code examples:
- Use `DefaultAzureCredential` for authentication
- Include error handling patterns
- Show both sync and async patterns where applicable
- Reference actual resource names from this deployment

## Azure Resource Reference

### Key Resources

| Resource | Name | Purpose |
|----------|------|---------|
| Resource Group | rg-artagent-voice-agent-dev | Container for all resources |
| AI Hub | artagent<suffix>aif | AI services endpoint |
| Communication Services | acs-artagent-voice-agent-dev-<suffix> | Voice/calling |
| Container Apps Env | cae-artagent-voice-agent-dev-<suffix> | App hosting |
| Key Vault | kv-<suffix> | Secrets management |
| App Configuration | appconfig-voice-agent-dev-<suffix> | Runtime config |

### Naming Convention

Resources follow the pattern:
- `{type}-{project}-{environment}-{suffix}` or
- `{type}{suffix}` (for resources with character restrictions)

Where:
- `type`: Resource type abbreviation (rg, kv, st, cr, etc.)
- `project`: artagent or voice-agent
- `environment`: voice-agent-dev
- `suffix`: <suffix> (unique deployment identifier)

## Common Tasks

### Adding New Documentation

1. Determine the appropriate directory
2. Create markdown file with clear heading structure
3. Include diagrams where helpful
4. Add links to related documents
5. Update README.md if needed

### Updating Resource Inventory

1. Query Azure Resource Graph for current state
2. Update [inventory.md](docs/resources/inventory.md)
3. Update architecture diagrams if topology changed
4. Note changes in commit message

### Creating Runbooks

1. Start with the goal/outcome
2. List prerequisites
3. Provide step-by-step commands
4. Include verification steps
5. Document rollback procedures
6. Add troubleshooting section

## Diagram Conventions

### Mermaid Diagram Types

- **graph TB/LR**: Architecture and component diagrams
- **sequenceDiagram**: Data flows and interactions
- **flowchart**: Decision trees and processes

### Color Coding (in descriptions)

- 🖥️ Compute resources
- 🤖 AI services
- 📞 Communication services
- 🔐 Security resources
- 📊 Observability
- ⚙️ Configuration

## Sensitive Information

### Never Include

- Connection strings
- API keys or secrets
- Personal identifiable information
- Internal IP addresses
- Actual phone numbers

### Always Mask

- Use `<placeholder>` for sensitive values
- Reference Key Vault for secrets
- Use environment variables for configuration
