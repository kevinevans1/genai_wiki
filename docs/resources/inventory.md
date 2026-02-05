# Azure Resource Inventory

Complete inventory of resources in `rg-artagent-voice-agent-dev`.

## Summary

| Category | Count |
|----------|-------|
| Compute (Container Apps) | 3 |
| AI Services | 2 |
| Communication | 3 |
| Security & Identity | 3 |
| Configuration & Storage | 3 |
| Observability | 2 |
| Platform | 2 |
| **Total** | **18+** |

---

## 🖥️ Compute Resources

### Container Apps

| Resource Name | Service Name | Purpose |
|---------------|--------------|---------|
| `artagent-frontend-<suffix>` | rtaudio-client | Web-based voice interface frontend |
| `artagent-backend-<suffix>` | rtaudio-server | Real-time audio processing backend |
| `webchat-demo-<suffix>` | - | Web chat demonstration interface |

### Container App Environment

| Resource Name | Location | Purpose |
|---------------|----------|---------|
| `cae-artagent-voice-agent-dev-<suffix>` | eastus2 | Shared hosting environment for all Container Apps |

### Container Registry

| Resource Name | SKU | Purpose |
|---------------|-----|---------|
| `crartagent<suffix>` | Basic | Container image storage and management |

---

## 🤖 AI Services

### Azure AI Foundry

| Resource Name | Kind | SKU | Purpose |
|---------------|------|-----|---------|
| `artagent<suffix>aif` | AIServices | S0 | AI Foundry Hub - cognitive services endpoint |
| `artagent<suffix>aif/artagent-<suffix>-aif-proj` | AIServices | - | AI Project for model deployments |

**Capabilities**:
- Real-time speech-to-text
- Text-to-speech synthesis
- GPT model integration for conversational AI
- Audio processing and analysis

---

## 📞 Communication Services

| Resource Name | Type | Location | Purpose |
|---------------|------|----------|---------|
| `acs-artagent-voice-agent-dev-<suffix>` | Communication Services | global | Voice calling, PSTN connectivity |
| `email-artagent-voice-agent-dev-<suffix>` | Email Services | global | Email sending capabilities |
| `AzureManagedDomain` | Email Domain | global | Azure-managed email domain for sending |

---

## 🔐 Security & Identity

### Key Vault

| Resource Name | Location | Purpose |
|---------------|----------|---------|
| `kv-<suffix>` | eastus2 | Secrets, keys, and certificates management |

### Managed Identities

| Resource Name | Associated Service | Purpose |
|---------------|-------------------|---------|
| `artagent-backend-<suffix>` | Backend Container App | Service authentication |
| `artagent-frontend-<suffix>` | Frontend Container App | Service authentication |

---

## ⚙️ Configuration & Storage

### App Configuration

| Resource Name | SKU | Purpose |
|---------------|-----|---------|
| `appconfig-voice-agent-dev-<suffix>` | Standard | Centralized configuration management |

### Storage Account

| Resource Name | Kind | SKU | Purpose |
|---------------|------|-----|---------|
| `st<suffix>` | StorageV2 | Standard_LRS | Blob storage for audio files, logs |

---

## 📊 Observability

### Application Insights

| Resource Name | Kind | Purpose |
|---------------|------|---------|
| `ai-<suffix>` | web | Application performance monitoring, distributed tracing |

### Log Analytics Workspace

| Resource Name | Purpose |
|---------------|---------|
| `log-<suffix>` | Centralized log aggregation and querying |

---

## 🏷️ Resource Tags

All resources share consistent tagging:

```json
{
  "environment": "voice-agent-dev",
  "project": "gbb-ai-audio-agent",
  "hidden-title": "Real Time Audio voice-agent-dev",
  "azd-env-name": "voice-agent-dev",
  "deployment": "terraform",
  "deployed_by": "<deployed-by>",
  "SecurityControl": "Ignore"
}
```

---

## 📍 Location Summary

| Location | Resources |
|----------|-----------|
| East US 2 | Container Apps, AI Services, Key Vault, Storage, App Config, Observability |
| Global | Communication Services, Email Services |
