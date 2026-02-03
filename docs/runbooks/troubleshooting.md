# Troubleshooting Guide

This runbook provides troubleshooting procedures for common issues in the Real-Time Audio Voice Agent.

## Quick Diagnostics

### Health Check Script

```bash
#!/bin/bash
# Quick health check for all services

RG="rg-artagent-voice-agent-dev"

echo "=== Container Apps Status ==="
az containerapp list -g $RG --query "[].{Name:name, Status:properties.runningStatus}" -o table

echo ""
echo "=== Recent Errors (App Insights) ==="
# Check Application Insights for recent errors
az monitor app-insights query \
  --app ai-hffwg8l2 \
  --resource-group $RG \
  --analytics-query "exceptions | where timestamp > ago(1h) | summarize count() by type"
```

---

## Issue Categories

### 1. Container App Issues

#### Symptom: Container App Won't Start

**Diagnosis:**
```bash
# Check container app status
az containerapp show \
  --name artagent-backend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --query "properties.runningStatus"

# Check logs for errors
az containerapp logs show \
  --name artagent-backend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --type system
```

**Common Causes & Solutions:**

| Cause | Solution |
|-------|----------|
| Image pull failure | Verify ACR credentials and image exists |
| Missing environment variables | Check App Configuration bindings |
| Managed Identity not assigned | Verify identity assignment in Container App |
| Health probe failing | Check `/health` endpoint implementation |

#### Symptom: Container App Scaling Issues

**Diagnosis:**
```bash
# Check current replica count
az containerapp revision list \
  --name artagent-backend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --query "[].{Name:name, Replicas:properties.replicas, Active:properties.active}" -o table
```

---

### 2. AI Services Issues

#### Symptom: Speech-to-Text Not Working

**Diagnosis:**
```bash
# Check AI service status
az cognitiveservices account show \
  --name artagenthffwg8l2aif \
  --resource-group rg-artagent-voice-agent-dev \
  --query "{name:name, state:properties.provisioningState, endpoint:properties.endpoint}"
```

**Common Causes & Solutions:**

| Cause | Solution |
|-------|----------|
| Quota exceeded | Check usage in Azure Portal, request increase |
| Region unavailable | Implement fallback to secondary region |
| Model not deployed | Deploy required model in AI Project |
| Invalid credentials | Verify Managed Identity has correct RBAC |

#### Symptom: Slow AI Responses

**Check Latency:**
```kusto
// Application Insights query
dependencies
| where name contains "cognitiveservices"
| where timestamp > ago(1h)
| summarize avg(duration), percentile(duration, 95), percentile(duration, 99) by bin(timestamp, 5m)
| render timechart
```

---

### 3. Communication Services Issues

#### Symptom: Voice Calls Failing

**Diagnosis:**
```bash
# Check ACS resource
az communication show \
  --name acs-artagent-voice-agent-dev-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev
```

**Common Causes & Solutions:**

| Cause | Solution |
|-------|----------|
| No phone numbers | Purchase phone numbers in ACS |
| Webhook not configured | Verify callback URL in ACS events |
| Audio stream timeout | Check WebSocket connection handling |
| PSTN connectivity | Verify calling plan and country availability |

#### Symptom: Email Sending Failures

**Check Email Service:**
```bash
# Verify email domain
az communication email domain show \
  --domain-name AzureManagedDomain \
  --email-service-name email-artagent-voice-agent-dev-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev
```

---

### 4. Connectivity Issues

#### Symptom: Services Can't Connect to Key Vault

**Diagnosis:**
```bash
# Check Key Vault access policies
az keyvault show \
  --name kv-hffwg8l2 \
  --query "properties.accessPolicies"

# Or check RBAC assignments
az role assignment list \
  --scope "/subscriptions/094336d1-8e03-42a4-95dc-1085ed02d8d5/resourceGroups/rg-artagent-voice-agent-dev/providers/Microsoft.KeyVault/vaults/kv-hffwg8l2" \
  --output table
```

**Solution:**
```bash
# Grant access to Managed Identity
az role assignment create \
  --role "Key Vault Secrets User" \
  --assignee <managed-identity-object-id> \
  --scope "/subscriptions/094336d1-8e03-42a4-95dc-1085ed02d8d5/resourceGroups/rg-artagent-voice-agent-dev/providers/Microsoft.KeyVault/vaults/kv-hffwg8l2"
```

---

### 5. Observability Issues

#### Symptom: No Telemetry in Application Insights

**Diagnosis:**
```bash
# Check Application Insights resource
az monitor app-insights component show \
  --app ai-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --query "{name:name, connectionString:connectionString}"
```

**Common Causes:**
- Incorrect connection string in application
- Application Insights SDK not initialized
- Telemetry being sampled out

#### Check Log Analytics Ingestion

```kusto
// Check data ingestion
Usage
| where TimeGenerated > ago(1h)
| summarize TotalMB = sum(Quantity) by DataType
| order by TotalMB desc
```

---

## Log Analysis

### Container App Logs

```bash
# Stream live logs
az containerapp logs show \
  --name artagent-backend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --follow

# Get logs from specific time
az containerapp logs show \
  --name artagent-backend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --since 1h
```

### Application Insights Queries

```kusto
// Find failed requests
requests
| where timestamp > ago(1h)
| where success == false
| project timestamp, name, resultCode, duration, customDimensions

// Trace a specific operation
traces
| where timestamp > ago(1h)
| where operation_Id == "<operation-id>"
| order by timestamp asc

// Check dependency failures
dependencies
| where timestamp > ago(1h)
| where success == false
| summarize count() by name, resultCode
| order by count_ desc
```

---

## Escalation Procedures

### Level 1: Self-Service

1. Check this troubleshooting guide
2. Review Application Insights dashboards
3. Check Azure Service Health

### Level 2: Team Support

1. Create incident in team channel
2. Include:
   - Error messages
   - Timestamps
   - Affected services
   - Steps already taken

### Level 3: Azure Support

1. Open Azure Support ticket for:
   - Platform-level issues
   - Quota increases
   - Service outages

---

## Emergency Procedures

### Full Service Restart

```bash
# Restart all container apps
az containerapp revision restart \
  --name artagent-backend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --revision $(az containerapp revision list --name artagent-backend-hffwg8l2 -g rg-artagent-voice-agent-dev --query "[?properties.active].name" -o tsv)

az containerapp revision restart \
  --name artagent-frontend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --revision $(az containerapp revision list --name artagent-frontend-hffwg8l2 -g rg-artagent-voice-agent-dev --query "[?properties.active].name" -o tsv)
```

### Disable Voice Calling (Circuit Breaker)

```bash
# Scale backend to 0 replicas
az containerapp update \
  --name artagent-backend-hffwg8l2 \
  --resource-group rg-artagent-voice-agent-dev \
  --min-replicas 0 \
  --max-replicas 0
```
