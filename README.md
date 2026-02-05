<div align="center">

```
   ██████╗ ██████╗ ██████╗ ███████╗    ████████╗ ██████╗ 
  ██╔════╝██╔═══██╗██╔══██╗██╔════╝    ╚══██╔══╝██╔═══██╗
  ██║     ██║   ██║██║  ██║█████╗         ██║   ██║   ██║
  ██║     ██║   ██║██║  ██║██╔══╝         ██║   ██║   ██║
  ╚██████╗╚██████╔╝██████╔╝███████╗       ██║   ╚██████╔╝
   ╚═════╝ ╚═════╝ ╚═════╝ ╚══════╝       ╚═╝    ╚═════╝ 

   ██████╗██╗      ██████╗ ██╗   ██╗██████╗ 
  ██╔════╝██║     ██╔═══██╗██║   ██║██╔══██╗
  ██║     ██║     ██║   ██║██║   ██║██║  ██║
  ██║     ██║     ██║   ██║██║   ██║██║  ██║
  ╚██████╗███████╗╚██████╔╝╚██████╔╝██████╔╝
   ╚═════╝╚══════╝ ╚═════╝  ╚═════╝ ╚═════╝ 
```

</div>

# Building an AI-Powered Infrastructure Wiki with GitHub Copilot

[![Azure](https://img.shields.io/badge/Azure-Deployed-0078D4?logo=microsoftazure)](https://portal.azure.com)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-Powered-000?logo=github)](https://github.com/features/copilot)
[![MCP](https://img.shields.io/badge/MCP-Azure%20Tools-blue)](https://modelcontextprotocol.io/)
[![Discord](https://img.shields.io/badge/Join%20the%20Discord-5865F2?logo=discord&logoColor=white)](https://discord.gg/vwfwq2EpXJ)
[![Podcast](https://img.shields.io/badge/Listen%20to%20Podcast-1DB954?logo=spotify&logoColor=white)](https://open.spotify.com/show/1iOZfFVamUk7CJPOvtU00v)

> **TL;DR**: This blog demonstrates how to use GitHub Copilot (Opus 4.5) with Azure MCP servers to automatically generate comprehensive infrastructure documentation from live Azure resources—turning natural language prompts into a full wiki in minutes.

---

## 📖 What Is This?

This repository showcases a practical approach to **automating infrastructure documentation** using generative AI. Instead of manually documenting Azure resources, we use GitHub Copilot to query live infrastructure and generate:

- Architecture diagrams
- Resource inventories
- Integration guides
- Operational runbooks
- Dependency maps

**The result?** A complete infrastructure wiki generated through 6 conversational prompts.

---

## 🎯 Who Is This For?

### Developers
- Learn how AI assistants can accelerate documentation tasks
- Understand patterns for integrating GitHub Copilot into your workflow
- See practical examples of prompt engineering for infrastructure tasks

### Platform Engineers
- Discover how to auto-generate documentation for Azure landing zones
- Explore patterns for maintaining living documentation
- Learn RBAC requirements for AI-assisted infrastructure queries

### DevOps / SRE Teams
- Automate runbook generation from existing infrastructure
- Keep documentation synchronized with actual deployments
- Reduce documentation debt with AI assistance

---

## 🛠️ Prerequisites

Before you can replicate this workflow, ensure you have the following set up:

### Option 1: GitHub Codespaces (Recommended)

The fastest way to get started—no local setup required:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/your-org/genai_wiki?quickstart=1)

**What's included:**
- Ubuntu minimal base image
- Azure CLI pre-installed
- GitHub Copilot + Azure MCP extensions
- 2 vCPU / 4GB RAM / 16GB storage (Codespaces Lite)

After launch, just authenticate:
```bash
az login --use-device-code
```

### Option 2: Local Development

### Tools Required

| Tool | Purpose | macOS | Windows |
|------|---------|-------|---------|
| **VS Code** | Development environment | `brew install --cask visual-studio-code` | `winget install Microsoft.VisualStudioCode` |
| **GitHub Copilot** | AI assistant (with Chat enabled) | [Subscribe](https://github.com/features/copilot) | [Subscribe](https://github.com/features/copilot) |
| **Azure CLI** | Azure authentication | `brew install azure-cli` | `winget install Microsoft.AzureCLI` |
| **Azure MCP Extension** | Copilot-to-Azure integration | Install via VS Code Extensions | Install via VS Code Extensions |

#### Quick Install (Copy & Paste)

**macOS (Homebrew):**
```bash
brew install --cask visual-studio-code
brew install azure-cli
```

**Windows (winget):**
```powershell
winget install Microsoft.VisualStudioCode
winget install Microsoft.AzureCLI
```

### MCP Servers Used

This workflow leverages the **Model Context Protocol (MCP)** to connect GitHub Copilot with Azure services:

| MCP Server | Capabilities | Used For |
|------------|--------------|----------|
| **Azure Resource Graph** | Query resources across subscriptions | Discovering and inventorying resources |
| **Azure Resource Manager** | List subscriptions, resource groups | Navigation and scoping |
| **Azure CLI Integration** | Execute az commands | Authentication and advanced queries |

### Azure RBAC Requirements

To run the prompts in this guide, your Azure identity needs the following permissions:

| Role | Scope | Purpose |
|------|-------|---------|
| **Reader** | Subscription or Resource Group | Query resource metadata and configurations |
| **Resource Graph Reader** | Subscription | Execute Azure Resource Graph queries |

> **Minimum Required**: `Reader` role at the resource group level is sufficient for documentation generation.

#### Assigning RBAC (if needed)

```bash
# Assign Reader role at resource group scope
az role assignment create \
  --assignee <your-user-or-service-principal-id> \
  --role "Reader" \
  --scope /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>
```

### Authentication Setup

```bash
# Login to Azure CLI
az login

# Verify your subscription access
az account list --output table

# Set your target subscription
az account set --subscription "<subscription-name-or-id>"
```

---

## 🚀 The Prompt Journey

We generated this entire wiki using just **6 natural language prompts**. See the complete prompt sequence, what GitHub Copilot did at each step, and lessons learned:

### 📄 [View the Full Prompt Journey →](PROMPT_JOURNEY.md)

**Highlights:**
- Progressive discovery from subscription → resource group → detailed docs
- Iterative refinement of architecture diagrams
- Single prompt that generated 12 documentation files

---

## 📚 Generated Documentation

The following wiki was auto-generated by GitHub Copilot from the `rg-artagent-voice-agent-dev` resource group:

| Section | Description |
|---------|-------------|
| [Architecture Overview](docs/architecture/overview.md) | System design and component relationships |
| [Data Flow](docs/architecture/data-flow.md) | How data moves through the system |
| [Resource Inventory](docs/resources/inventory.md) | Complete Azure resource listing |
| [Integration Guides](docs/integrations/README.md) | Service integration patterns |
| [Azure AI Integration](docs/integrations/azure-ai.md) | AI Services configuration |
| [Communication Services](docs/integrations/communication-services.md) | ACS setup and usage |
| [Dependencies](docs/dependencies/README.md) | Runtime and service dependencies |
| [Deployment Runbook](docs/runbooks/deployment.md) | Deployment procedures |
| [Troubleshooting Guide](docs/runbooks/troubleshooting.md) | Common issues and solutions |

---

## 🏗️ The Documented Architecture

This wiki documents a **Real-Time Audio Voice Agent** infrastructure—a fictional scenario showcasing typical Azure AI workloads:

### What the Architecture Does

The Real-Time Audio Voice Agent enables:
- 🎤 **Real-time voice conversations** with AI-powered agents
- 🔊 **Speech-to-text and text-to-speech** processing
- 📞 **Voice calling capabilities** via Azure Communication Services
- 📧 **Email integration** for notifications and follow-ups
- 💬 **Web chat interface** as an alternative interaction channel

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              👤 CLIENT LAYER                                     │
│  ┌─────────────────────────────────┐    ┌─────────────────────────────────┐     │
│  │       🌐 Web Browser            │    │       📞 Phone / PSTN           │     │
│  └─────────────────────────────────┘    └─────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────────────────────┘
                    │                                      │
                    ▼                                      ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                     📦 AZURE CONTAINER APPS ENVIRONMENT                          │
│  ┌─────────────────────┐  ┌─────────────────────┐  ┌─────────────────────┐      │
│  │      Frontend       │  │      Backend        │  │      WebChat        │      │
│  │   (rtaudio-client)  │─▶│   (rtaudio-server)  │◀─│      (Demo UI)      │      │
│  └─────────────────────┘  └─────────────────────┘  └─────────────────────┘      │
└─────────────────────────────────────────────────────────────────────────────────┘
                                       │
          ┌────────────────────────────┼────────────────────────────┐
          ▼                            ▼                            ▼
┌───────────────────────┐  ┌───────────────────────┐  ┌───────────────────────────┐
│  🤖 AZURE AI SERVICES │  │ 📡 COMMUNICATION SVC  │  │   ⚙️ PLATFORM SERVICES    │
├───────────────────────┤  ├───────────────────────┤  ├───────────────────────────┤
│ • AI Foundry Hub      │  │ • Communication Svc   │  │ • Key Vault               │
│ • AI Project          │  │ • Email Services      │  │ • App Configuration       │
│ • Speech Services     │  │ • PSTN Integration    │  │ • Container Registry      │
└───────────────────────┘  └───────────────────────┘  └───────────────────────────┘
```

> 📖 **Detailed diagrams available in** [docs/architecture/overview.md](docs/architecture/overview.md)

---

## 🌐 Network Architecture

This section details the networking configuration discovered by querying the live Azure resources.

### Network Topology Summary

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| **VNet Integration** | ❌ Not configured | Container Apps Environment uses public networking |
| **Private Endpoints** | ❌ None deployed | All services use public endpoints |
| **NSGs** | ❌ Not applicable | No custom VNet = no NSG requirements |
| **Subnets** | ❌ Not applicable | Managed by Azure Container Apps |

> ⚠️ **Development Environment**: This architecture uses public networking suitable for dev/test. Production deployments should implement VNet integration and private endpoints.

### Service Endpoints & Access

#### Container Apps (Compute Layer)

| Service | FQDN | Port | External |
|---------|------|------|----------|
| **Frontend** | `<frontend-app>.<environment-id>.eastus2.azurecontainerapps.io` | 8080 | ✅ Yes |
| **Backend** | `<backend-app>.<environment-id>.eastus2.azurecontainerapps.io` | 8000 | ✅ Yes |
| **WebChat** | `<webchat-app>.<environment-id>.eastus2.azurecontainerapps.io` | 3001 | ✅ Yes |

#### Container Apps Environment

| Property | Value |
|----------|-------|
| **Static IP** | `<static-ip-address>` |
| **Default Domain** | `<environment-id>.eastus2.azurecontainerapps.io` |
| **Public Network Access** | Enabled |
| **VNet Configuration** | None (Azure-managed networking) |
| **Zone Redundancy** | Disabled |
| **mTLS** | Disabled |

#### Data Services Network Configuration

| Service | Type | Public Access | Private Endpoints | Network Rules |
|---------|------|---------------|-------------------|---------------|
| **Key Vault** | `kv-<suffix>` | Enabled | None | RBAC Authorization enabled |
| **Storage Account** | `st<suffix>` | Enabled | None | Bypass: AzureServices, Default: Allow |
| **AI Services** | `<project><suffix>aif` | Enabled | None | None |
| **Redis Enterprise** | `redis<suffix>` | Enabled | None | TLS 1.2 minimum |
| **Cosmos DB (Mongo)** | `cosmos-cluster-<suffix>` | Enabled | None | None |

### Redis Enterprise Details

| Property | Value |
|----------|-------|
| **Host** | `<redis-name>.eastus2.redis.azure.net` |
| **Port** | 10000 |
| **SKU** | MemoryOptimized_M10 |
| **Redis Version** | 7.4 |
| **High Availability** | Enabled |
| **Redundancy** | Zone Redundant (ZR) |
| **TLS Version** | 1.2+ |
| **Access Keys Auth** | Disabled (Entra ID) |

### Cosmos DB Mongo Cluster Details

| Property | Value |
|----------|-------|
| **Connection String** | `mongodb+srv://<user>:<password>@<cluster-name>.mongocluster.cosmos.azure.com/` |
| **Server Version** | 8.0 |
| **SKU** | M30 |
| **Storage** | 128 GB |
| **High Availability** | Disabled |
| **Replication Role** | Primary |

---

## 💡 Key Takeaways

### What We Learned

1. **Natural Language Works** — Conversational prompts like "what's in this resource group?" are effective for infrastructure discovery

2. **Progressive Discovery** — Start broad (subscription) and narrow down (resource group → specific resources)

3. **Iterative Refinement** — Diagrams and documentation improve through follow-up prompts

4. **Context Matters** — Providing LLM instruction files (`.github/copilot-instructions.md`) improves future AI interactions

### Limitations to Consider

- AI-generated docs should be reviewed for accuracy
- Sensitive information (keys, connection strings) must be filtered
- Complex architectures may need multiple prompt iterations
- Real-time data means docs can become stale

---

## 🔄 Try It Yourself

### Quick Start

1. **Clone this repo** to see the generated output
2. **Review the [Prompt Journey](PROMPT_JOURNEY.md)** to understand the process
3. **Set up prerequisites** (see above)
4. **Run these prompts** against your own Azure infrastructure:

```
1. "Do we have access to Azure? List my subscriptions and resource groups."

2. "What resources are in [your-resource-group]?"

3. "Create a wiki documenting [resource-group] with architecture diagrams, 
    resource inventory, integration guides, dependencies, runbooks, 
    and LLM instruction files following best practices."

4. [Iterate on specific sections as needed]
```

---

## 📊 Results Summary

| Metric | Value |
|--------|-------|
| Total Prompts | 6 |
| Files Generated | 13 |
| Azure Queries | 6 |
| Documentation Pages | 10 |
| Time to Generate | ~15 minutes |

---

## 🔐 Security Considerations

When using AI to document infrastructure:

- ✅ **Reader-only access** is sufficient—no write permissions needed
- ✅ **No secrets exposed** — AI queries metadata, not secret values
- ✅ **Audit trail** — All queries go through Azure Resource Graph
- ⚠️ **Review outputs** — Ensure no sensitive resource names are shared publicly

---

## 📁 Repository Structure

```
├── README.md                           # This file (blog/guide)
├── PROMPT_JOURNEY.md                   # Detailed prompt documentation
├── .github/
│   └── copilot-instructions.md         # GitHub Copilot context file
└── docs/                               # Generated wiki documentation
    ├── architecture/
    ├── resources/
    ├── integrations/
    ├── dependencies/
    └── runbooks/
```

---

## 🤝 Contributing

Found this useful? Have improvements?

- ⭐ **Star this repo** if you found it helpful
- 🐛 **Open an issue** for questions or suggestions
- 🔀 **Submit a PR** with improvements to the prompts or documentation

---

## 📜 License

This project is provided as-is for educational purposes. The documented infrastructure is a fictional scenario for demonstration.

---

*Built with GitHub Copilot (Claude Opus 4.5) + Azure MCP Servers — February 2026*
