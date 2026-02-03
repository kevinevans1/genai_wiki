# Real-Time Audio Voice Agent - Infrastructure Wiki

[![Azure](https://img.shields.io/badge/Azure-Deployed-0078D4?logo=microsoftazure)](https://portal.azure.com)
[![Terraform](https://img.shields.io/badge/IaC-Terraform-623CE4?logo=terraform)](https://www.terraform.io/)
[![AI Powered](https://img.shields.io/badge/AI-Azure%20AI%20Services-FF6F00?logo=openai)](https://azure.microsoft.com/en-us/products/ai-services)

## 📋 Overview

This wiki documents the **Real-Time Audio Voice Agent** (`gbb-ai-audio-agent`) infrastructure deployed in Azure. The solution provides real-time voice interaction capabilities powered by Azure AI Services and Azure Communication Services.

### What This Architecture Does

The Real-Time Audio Voice Agent enables:
- 🎤 **Real-time voice conversations** with AI-powered agents
- 🔊 **Speech-to-text and text-to-speech** processing
- 📞 **Voice calling capabilities** via Azure Communication Services
- 📧 **Email integration** for notifications and follow-ups
- 💬 **Web chat interface** as an alternative interaction channel

## 🏗️ Architecture Diagram

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
│ ┌───────────────────┐ │  │ ┌───────────────────┐ │  │ ┌─────────┐ ┌───────────┐ │
│ │  AI Foundry Hub   │ │  │ │   Communication   │ │  │ │Key Vault│ │App Config │ │
│ │ artagenthffwg8l2  │ │  │ │     Services      │ │  │ └─────────┘ └───────────┘ │
│ └─────────┬─────────┘ │  │ └─────────┬─────────┘ │  │ ┌─────────┐ ┌───────────┐ │
│           ▼           │  │           ▼           │  │ │Container│ │  Storage  │ │
│ ┌───────────────────┐ │  │ ┌───────────────────┐ │  │ │Registry │ │  Account  │ │
│ │    AI Project     │ │  │ │  Email Services   │ │  │ └─────────┘ └───────────┘ │
│ │  artagent-aif-proj│ │  │ │ AzureManagedDomain│ │  │                           │
│ └───────────────────┘ │  │ └───────────────────┘ │  │                           │
└───────────────────────┘  └───────────────────────┘  └───────────────────────────┘
                                       │
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                            📊 OBSERVABILITY                                      │
│  ┌─────────────────────────────────┐    ┌─────────────────────────────────┐     │
│  │     Application Insights        │───▶│       Log Analytics             │     │
│  │         ai-hffwg8l2             │    │        log-hffwg8l2             │     │
│  └─────────────────────────────────┘    └─────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Component Overview

| Layer | Components | Purpose |
|-------|------------|---------|
| **Clients** | Web Browser, Phone/PSTN | User entry points |
| **Compute** | Frontend, Backend, WebChat | Container Apps workloads |
| **AI** | AI Foundry Hub, AI Project | Speech & language processing |
| **Communication** | ACS, Email | Voice calling & notifications |
| **Platform** | Key Vault, App Config, Registry, Storage | Supporting infrastructure |
| **Observability** | App Insights, Log Analytics | Monitoring & logging |

## 📁 Wiki Structure

| Directory | Description |
|-----------|-------------|
| [docs/architecture](docs/architecture/) | Architecture diagrams, ADRs, and design documents |
| [docs/resources](docs/resources/) | Detailed Azure resource documentation |
| [docs/integrations](docs/integrations/) | Service integration guides and patterns |
| [docs/runbooks](docs/runbooks/) | Operational procedures and troubleshooting |
| [docs/dependencies](docs/dependencies/) | Runtime and service dependencies |

## 🚀 Quick Links

- [Resource Inventory](docs/resources/inventory.md)
- [Architecture Overview](docs/architecture/overview.md)
- [Integration Patterns](docs/integrations/README.md)
- [Dependency Map](docs/dependencies/README.md)
- [Deployment Guide](docs/runbooks/deployment.md)

## 📊 Environment Details

| Property | Value |
|----------|-------|
| **Subscription** | AI_Foundry (`094336d1-8e03-42a4-95dc-1085ed02d8d5`) |
| **Resource Group** | `rg-artagent-voice-agent-dev` |
| **Environment** | `voice-agent-dev` |
| **Primary Region** | East US 2 |
| **Deployment Tool** | Terraform via Azure Developer CLI (azd) |
| **Deployed By** | kevinevans1 |

## 🔐 Security

- All services use **Managed Identities** for authentication
- Secrets stored in **Azure Key Vault**
- Network isolation via **Container Apps Environment**
- Tagged with `SecurityControl: Ignore` (dev environment)

---

*This documentation is maintained using GitHub Copilot SDK. Last updated: February 2026*
demo repo for generating documentation using Gen AI.
