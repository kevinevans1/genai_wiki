# Proposal: AI-Powered Infrastructure Documentation Agent

An Azure AI Foundry agent that automatically discovers, documents, and maintains infrastructure wikis.

---

## 🎯 Executive Summary

This proposal outlines an **AI Documentation Agent** built on Azure AI Foundry that replicates and automates the wiki generation process demonstrated in this repository. The agent would continuously discover Azure infrastructure, generate documentation, and keep it synchronized with actual deployments.

---

## 🏗️ Proposed Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                              🤖 AI DOCUMENTATION AGENT                               │
│                              (Azure AI Foundry Agent)                                │
└─────────────────────────────────────────────────────────────────────────────────────┘
                                         │
                    ┌────────────────────┼────────────────────┐
                    ▼                    ▼                    ▼
┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────────┐
│   📡 AZURE CONNECTIONS  │ │   📂 GITHUB CONNECTION  │ │   💬 USER INTERFACES        │
├─────────────────────────┤ ├─────────────────────────┤ ├─────────────────────────────┤
│ • Azure Resource Graph  │ │ • Repository Access     │ │ • Teams Bot                 │
│ • Management APIs       │ │ • Pull Request Creation │ │ • Slack Integration         │
│ • Azure Monitor         │ │ • Wiki Updates          │ │ • Web Chat                  │
│ • Cost Management       │ │ • Issue Tracking        │ │ • CLI Tool                  │
│ • Service Health        │ │ • Actions Triggers      │ │ • VS Code Extension         │
└─────────────────────────┘ └─────────────────────────┘ └─────────────────────────────┘
                    │                    │                    │
                    └────────────────────┼────────────────────┘
                                         ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                           ⚙️ AGENT CAPABILITIES                                      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   Discovery     │  │  Documentation  │  │   Compliance    │  │   Alerting      │ │
│  │   & Inventory   │  │   Generation    │  │   & Drift       │  │   & Updates     │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────────┘
                                         │
                                         ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                              📊 OUTPUTS                                              │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  • Architecture Diagrams (auto-generated)                                            │
│  • Resource Inventories (live-synced)                                                │
│  • Integration Documentation (code samples)                                          │
│  • Runbooks (operational procedures)                                                 │
│  • Cost Reports (with optimization recommendations)                                  │
│  • Compliance Reports (policy violations, drift detection)                           │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔌 Required Connections & Access

### 1. Azure Connections

| Connection | Purpose | Required Permissions |
|------------|---------|---------------------|
| **Azure Resource Graph** | Query all resources across subscriptions | `Reader` on subscriptions |
| **Azure Management APIs** | Get detailed resource configurations | `Reader` on resource groups |
| **Azure Monitor** | Retrieve metrics, logs, alerts | `Monitoring Reader` |
| **Cost Management** | Cost analysis and recommendations | `Cost Management Reader` |
| **Service Health** | Incident and maintenance info | `Reader` |
| **Azure Policy** | Compliance status | `Policy Insights Data Reader` |

**Authentication**: Managed Identity (recommended) or Service Principal

```
┌─────────────────────────────────────────────────────────┐
│                  AZURE ACCESS MODEL                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│   ┌─────────────────┐      ┌─────────────────────────┐  │
│   │  Agent Identity │─────▶│  Azure RBAC Roles       │  │
│   │  (Managed ID)   │      │  • Reader               │  │
│   └─────────────────┘      │  • Monitoring Reader    │  │
│                            │  • Cost Mgmt Reader     │  │
│                            └─────────────────────────┘  │
│                                       │                  │
│                                       ▼                  │
│   ┌─────────────────────────────────────────────────┐   │
│   │              Target Subscriptions                │   │
│   │  • Sub 1: Production                             │   │
│   │  • Sub 2: Development                            │   │
│   │  • Sub 3: Staging                                │   │
│   └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 2. GitHub Connection

| Connection | Purpose | Required Permissions |
|------------|---------|---------------------|
| **Repository Access** | Read/write wiki files | `Contents: Write` |
| **Pull Requests** | Create PRs for doc updates | `Pull Requests: Write` |
| **Issues** | Track documentation gaps | `Issues: Write` |
| **Actions** | Trigger workflows | `Actions: Write` |
| **Wikis** | Direct wiki updates | `Wiki: Write` |

**Authentication**: GitHub App (recommended) or Personal Access Token

```
┌─────────────────────────────────────────────────────────┐
│                 GITHUB ACCESS MODEL                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│   ┌─────────────────┐      ┌─────────────────────────┐  │
│   │   GitHub App    │─────▶│  Repository Permissions │  │
│   │  (Installed)    │      │  • Contents: Write      │  │
│   └─────────────────┘      │  • PRs: Write           │  │
│                            │  • Issues: Write        │  │
│                            │  • Actions: Trigger     │  │
│                            └─────────────────────────┘  │
│                                       │                  │
│                                       ▼                  │
│   ┌─────────────────────────────────────────────────┐   │
│   │              Target Repositories                 │   │
│   │  • org/infra-wiki                                │   │
│   │  • org/platform-docs                             │   │
│   │  • org/runbooks                                  │   │
│   └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 3. Optional Integrations

| Integration | Purpose | Use Case |
|-------------|---------|----------|
| **Microsoft Teams** | Notifications, chat interface | "What changed in prod last week?" |
| **Slack** | Notifications, chat interface | Same as Teams |
| **Jira/Azure DevOps** | Work item creation | Auto-create tasks for doc gaps |
| **ServiceNow** | CMDB sync | Keep CMDB aligned with reality |
| **Confluence** | Alternative wiki target | For Atlassian shops |

---

## 🛠️ Agent Components

### Core Agent (Azure AI Foundry)

```
┌─────────────────────────────────────────────────────────────────┐
│                    AI FOUNDRY AGENT STRUCTURE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                      ORCHESTRATOR                         │   │
│  │                    (GPT-4o / GPT-4)                       │   │
│  │  • Understands user queries                               │   │
│  │  • Plans multi-step actions                               │   │
│  │  • Synthesizes documentation                              │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│         ┌────────────────────┼────────────────────┐             │
│         ▼                    ▼                    ▼             │
│  ┌────────────┐       ┌────────────┐       ┌────────────┐       │
│  │   Azure    │       │   GitHub   │       │  Diagram   │       │
│  │   Tools    │       │   Tools    │       │  Generator │       │
│  ├────────────┤       ├────────────┤       ├────────────┤       │
│  │• Query ARG │       │• Read files│       │• ASCII art │       │
│  │• Get config│       │• Write docs│       │• Mermaid   │       │
│  │• List subs │       │• Create PR │       │• PlantUML  │       │
│  │• Get costs │       │• Add issues│       │• D2         │      │
│  └────────────┘       └────────────┘       └────────────┘       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Tool Definitions

| Tool | Description | Input | Output |
|------|-------------|-------|--------|
| `query_azure_resources` | Query Resource Graph | KQL-like intent | Resource list |
| `get_resource_details` | Get full resource config | Resource ID | JSON config |
| `list_subscriptions` | List accessible subs | - | Sub list |
| `get_cost_data` | Cost by resource/group | Scope, timeframe | Cost breakdown |
| `read_github_file` | Read existing docs | Repo, path | File contents |
| `write_github_file` | Create/update docs | Repo, path, content | Commit SHA |
| `create_pull_request` | Open PR for review | Repo, branch, title | PR URL |
| `generate_diagram` | Create architecture diagram | Resources, style | Diagram text |

---

## 📋 Use Cases

### 1. Initial Documentation Generation
```
User: "Document all resources in rg-production-app"
Agent: 
  1. Queries Azure Resource Graph for all resources
  2. Gets detailed configuration for each resource
  3. Identifies dependencies and integrations
  4. Generates wiki structure with diagrams
  5. Creates PR to documentation repository
```

### 2. Continuous Sync (Scheduled)
```
Schedule: Daily at 2 AM
Agent:
  1. Queries current Azure state
  2. Compares with existing documentation
  3. Identifies drift/changes
  4. Updates affected documentation
  5. Creates PR or direct commit
  6. Posts summary to Teams/Slack
```

### 3. On-Demand Queries
```
User: "What changed in production last week?"
Agent:
  1. Queries Azure Activity Log
  2. Identifies resource changes
  3. Summarizes additions, deletions, modifications
  4. Highlights security-relevant changes
```

### 4. Compliance Reporting
```
User: "Are all resources tagged properly?"
Agent:
  1. Queries all resources
  2. Checks against tagging policy
  3. Generates compliance report
  4. Creates issues for violations
```

---

## 🔒 Security Considerations

### Principle of Least Privilege

```
┌─────────────────────────────────────────────────────────────────┐
│                    SECURITY BOUNDARIES                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    READ-ONLY AZURE                       │    │
│  │  • Cannot modify resources                               │    │
│  │  • Cannot access data plane (blobs, secrets, etc.)       │    │
│  │  • Only control plane metadata                           │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    SCOPED GITHUB                         │    │
│  │  • Only specific documentation repositories              │    │
│  │  • Cannot access source code repositories                │    │
│  │  • PR-based workflow for review                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    AUDIT LOGGING                         │    │
│  │  • All agent actions logged                              │    │
│  │  • Queryable in Log Analytics                            │    │
│  │  • Alerts on anomalous behavior                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Data Handling
- **No secrets extracted** - Agent reads metadata only, never secret values
- **No PII logging** - User identifiers masked in logs
- **Retention policies** - Agent memory cleared after session

---

## 💰 Cost Estimate

### Azure AI Foundry

| Component | Unit | Est. Monthly Cost |
|-----------|------|-------------------|
| GPT-4o tokens | ~500K tokens/month | $15-25 |
| Agent hosting | Serverless | $0 (pay per use) |
| Storage | <1 GB | <$1 |

### Supporting Services

| Service | Purpose | Est. Monthly Cost |
|---------|---------|-------------------|
| Log Analytics | Agent telemetry | $5-10 |
| Key Vault | Credential storage | <$1 |
| App Configuration | Agent settings | $0 (free tier) |

**Total Estimated Cost: $20-40/month** for moderate usage

---

## 🚀 Implementation Phases

### Phase 1: MVP (2-3 weeks)
- [ ] Create AI Foundry agent with Azure tools
- [ ] Implement resource discovery
- [ ] Generate basic documentation (inventory, diagrams)
- [ ] Manual trigger via API/chat

### Phase 2: GitHub Integration (2 weeks)
- [ ] Add GitHub tools (read/write/PR)
- [ ] Implement wiki sync workflow
- [ ] PR-based review process
- [ ] Basic scheduling

### Phase 3: Advanced Features (3-4 weeks)
- [ ] Drift detection
- [ ] Cost reporting
- [ ] Compliance checks
- [ ] Teams/Slack integration
- [ ] Natural language queries

### Phase 4: Enterprise (4+ weeks)
- [ ] Multi-tenant support
- [ ] Custom templates
- [ ] CMDB integration
- [ ] Advanced analytics

---

## 📊 Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Documentation coverage | 100% of resources | Resources with docs / Total resources |
| Documentation freshness | <24 hours stale | Time since last sync |
| User adoption | 80% of teams | Teams using agent / Total teams |
| Time saved | 10 hours/week | Survey + before/after comparison |
| Accuracy | 99% | Spot checks, user feedback |

---

## 🤔 Open Questions for Customer Discussion

1. **Scope**: Which subscriptions/resource groups should be in scope?
2. **Frequency**: Real-time sync vs. daily batch updates?
3. **Review Process**: Direct commits or PR-based approval?
4. **Wiki Platform**: GitHub Wiki, Confluence, SharePoint, or custom?
5. **Notification Preferences**: Teams, Slack, email, or all?
6. **Compliance Requirements**: Any specific standards (SOC2, HIPAA, etc.)?
7. **Existing Documentation**: Migration or fresh start?
8. **Template Customization**: Standard format or custom branding?

---

## 📎 Related Resources

- [Azure AI Foundry Documentation](https://learn.microsoft.com/azure/ai-studio/)
- [Azure Resource Graph](https://learn.microsoft.com/azure/governance/resource-graph/)
- [GitHub Apps](https://docs.github.com/apps)
- [This Wiki - Prompt Journey](PROMPT_JOURNEY.md)

---

*Proposal prepared for customer discussion - February 2026*
