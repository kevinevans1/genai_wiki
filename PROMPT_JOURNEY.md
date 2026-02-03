# Prompt Journey: Building This Wiki with GitHub Copilot

This document captures the prompt engineering process used to create this infrastructure wiki using GitHub Copilot and the GitHub Copilot SDK.

---

## 🎯 Objective

Demonstrate how GitHub Copilot can be used as an engineering assistant to automatically document Azure infrastructure by querying live resources and generating comprehensive documentation.

---

## 📝 Prompt Sequence

### Step 1: Verify Azure Access

**Prompt:**
```
do we have access to azure if so do a lookup on the subscription and give me the resource groups
```

**What Copilot Did:**
- Called Azure MCP tools to list subscriptions
- Retrieved 3 subscriptions: AI_Foundry, AI, Platform_Engineering_Lab
- Listed all resource groups across subscriptions (22 total)

**Output:** Subscription and resource group inventory tables

---

### Step 2: Explore Subscription Contents

**Prompt:**
```
whats in the sub ai foundry ?
```

**What Copilot Did:**
- Queried Azure Resource Graph for all resources in AI_Foundry subscription
- Returned 70 resources (truncated to 10 in response)
- Identified key workloads: AlbertaForge AI project, voice agent infrastructure, AKS cluster

**Output:** Resource summary with highlights of AI services, AKS, and voice agent components

---

### Step 3: Deep Dive into Resource Group

**Prompt:**
```
whats in the rg-artagent-voice-agent-dev
```

**What Copilot Did:**
- Queried Azure Resource Graph for specific resource group
- Retrieved 21 resources with details (type, location, purpose)
- Identified the Real-Time Audio Voice Agent architecture

**Output:** Detailed resource table with architecture summary

---

### Step 4: Generate Complete Documentation

**Prompt:**
```
ok awesome lets use that resource group for context basically i demonstrate that we can use 
github copilot and github copilot sdk to be engineering call wiki and document the resource 
group the integrations, the dependencies, the runtimes and generate diagrams and full suit 
of documentation for the resource group and give an overview of what the architecture does, 
i want this repo and wiki to structured with best practices with llm instruction file
```

**What Copilot Did:**
- Queried Azure for complete resource inventory (multiple queries to get all 21 resources)
- Created full wiki directory structure:
  ```
  docs/
  ├── architecture/
  │   ├── overview.md
  │   └── data-flow.md
  ├── resources/
  │   └── inventory.md
  ├── integrations/
  │   ├── README.md
  │   ├── azure-ai.md
  │   └── communication-services.md
  ├── dependencies/
  │   └── README.md
  └── runbooks/
      ├── deployment.md
      └── troubleshooting.md
  ```
- Created LLM instruction files:
  - `.github/copilot-instructions.md` - GitHub Copilot context
  - `.instructions/llm-context.md` - General LLM context
- Generated Mermaid diagrams for architecture visualization
- Included code samples for Azure SDK integrations
- Documented all service dependencies and data flows

**Output:** 12 documentation files with comprehensive wiki structure

---

### Step 5: Diagram Style Iteration

**Prompt:**
```
use boxes for the architecture diagram
```

**What Copilot Did:**
- Updated Mermaid diagram to use `block-beta` format with grid layout

**Prompt:**
```
we need line boxes the current state does not help the reader
```

**What Copilot Did:**
- Changed to `flowchart TB` with `subgraph` for bordered sections
- Added color-coded styling for each layer

**Prompt:**
```
so no mermaid diagrams at all i can still see some
```

**What Copilot Did:**
- Replaced Mermaid with ASCII box-drawing diagram using Unicode characters
- Created clear visual hierarchy with boxes, arrows, and labels

**Output:** Plain-text ASCII architecture diagram

---

### Step 6: Document the Process

**Prompt:**
```
now create a prompt readme file, edited of course with prompt steps on how we got here
```

**What Copilot Did:**
- Created this document (PROMPT_JOURNEY.md)
- Captured all prompts in sequence
- Documented what actions Copilot took at each step
- Included lessons learned and best practices

---

## 🔑 Key Techniques Used

### 1. Progressive Discovery
Started broad (subscription level) and narrowed down to specific resource group.

### 2. Natural Language Queries
Used conversational prompts rather than technical commands:
- ❌ "Execute az group list --subscription..."
- ✅ "whats in the sub ai foundry?"

### 3. Context Building
Each prompt built on previous context, allowing Copilot to maintain understanding of the target system.

### 4. Iterative Refinement
Refined outputs through follow-up prompts:
- Initial diagram → Box format → Line boxes → ASCII art

### 5. Comprehensive Scope
Single prompt requested full documentation suite:
- Architecture diagrams
- Resource inventory
- Integration guides
- Runbooks
- Dependencies
- LLM instructions

---

## 📊 Results Summary

| Metric | Value |
|--------|-------|
| Total Prompts | 6 |
| Files Created | 13 |
| Azure Queries | 6 |
| Documentation Pages | 10 |
| Code Samples | 15+ |
| Diagrams | 8 |

---

## 💡 Lessons Learned

### What Worked Well

1. **Azure MCP Integration** - Copilot seamlessly queried live Azure resources
2. **Structured Output** - Requested "wiki structure with best practices" yielded organized docs
3. **LLM Instructions** - Asking for "llm instruction file" created proper context files
4. **Iterative Refinement** - Quick feedback loop for diagram style changes

### Tips for Replication

1. **Be Specific About Scope** - "document the resource group, integrations, dependencies, runtimes"
2. **Request Best Practices** - "structured with best practices" guides output quality
3. **Include Meta-Documentation** - Ask for LLM context files to help future AI interactions
4. **Iterate on Visuals** - Diagrams often need refinement; use follow-up prompts

---

## 🚀 Try It Yourself

To replicate this process for your own Azure infrastructure:

```
1. "do we have access to azure? list my subscriptions and resource groups"

2. "what resources are in [your-resource-group]?"

3. "create a wiki documenting [resource-group] with architecture diagrams, 
    resource inventory, integration guides, dependencies, runbooks, 
    and LLM instruction files following best practices"

4. [Iterate on specific sections as needed]
```

---

## 📚 Files Generated in This Session

| File | Purpose |
|------|---------|
| `README.md` | Main wiki homepage |
| `PROMPT_JOURNEY.md` | This file - prompt documentation |
| `.github/copilot-instructions.md` | GitHub Copilot context |
| `.instructions/llm-context.md` | General LLM instructions |
| `docs/architecture/overview.md` | Architecture documentation |
| `docs/architecture/data-flow.md` | Data flow diagrams |
| `docs/resources/inventory.md` | Azure resource inventory |
| `docs/integrations/README.md` | Integration patterns |
| `docs/integrations/azure-ai.md` | Azure AI integration guide |
| `docs/integrations/communication-services.md` | ACS integration guide |
| `docs/dependencies/README.md` | Dependency mapping |
| `docs/runbooks/deployment.md` | Deployment procedures |
| `docs/runbooks/troubleshooting.md` | Troubleshooting guide |

---

*Generated using GitHub Copilot with Claude Opus 4.5 - February 2026*
