# Data Flow Documentation

This document details the data flows within the Real-Time Audio Voice Agent system.

## Overview

The system processes three primary types of data:
1. **Audio Data** - Real-time voice streams
2. **Text Data** - Transcripts and AI responses
3. **Telemetry Data** - Logs, metrics, and traces

---

## Audio Data Flow

### Web-Based Voice Interaction

```mermaid
sequenceDiagram
    autonumber
    participant Browser
    participant Frontend as Frontend<br/>(Container App)
    participant Backend as Backend<br/>(Container App)
    participant AI as Azure AI<br/>Foundry
    participant Storage

    Note over Browser: User speaks
    Browser->>Browser: Capture audio (WebAudio API)
    Browser->>Frontend: Audio chunks (WebSocket)
    Frontend->>Backend: Forward audio stream (WebSocket)
    
    Backend->>AI: Send audio for STT
    AI-->>Backend: Text transcript
    
    Backend->>AI: Generate AI response
    AI-->>Backend: Response text
    
    Backend->>AI: Synthesize speech (TTS)
    AI-->>Backend: Audio stream
    
    Backend->>Frontend: Audio response (WebSocket)
    Frontend->>Browser: Play audio
    
    opt Recording Enabled
        Backend->>Storage: Store audio recording
        Backend->>Storage: Store transcript
    end
```

### Audio Processing Pipeline

```mermaid
graph LR
    subgraph "Input Processing"
        A[Raw Audio<br/>16-bit PCM] --> B[Noise Reduction]
        B --> C[Voice Activity<br/>Detection]
        C --> D[Chunking<br/>20ms frames]
    end
    
    subgraph "AI Processing"
        D --> E[Speech-to-Text]
        E --> F[NLU Processing]
        F --> G[Response Generation]
        G --> H[Text-to-Speech]
    end
    
    subgraph "Output Processing"
        H --> I[Audio Encoding]
        I --> J[Streaming<br/>to Client]
    end
```

### Audio Format Specifications

| Stage | Format | Sample Rate | Channels |
|-------|--------|-------------|----------|
| Browser Capture | PCM | 16000 Hz | Mono |
| WebSocket Transport | PCM/Opus | 16000 Hz | Mono |
| STT Input | PCM | 16000 Hz | Mono |
| TTS Output | MP3/PCM | 24000 Hz | Mono |
| Storage | WAV/MP3 | 16000 Hz | Mono |

---

## Phone Call Data Flow

```mermaid
sequenceDiagram
    autonumber
    participant Phone as Phone/PSTN
    participant ACS as Azure Comm<br/>Services
    participant Backend as Backend<br/>(Container App)
    participant AI as Azure AI<br/>Foundry
    participant Storage

    Phone->>ACS: Incoming call
    ACS->>Backend: IncomingCall event (webhook)
    Backend->>ACS: Answer call
    ACS-->>Backend: Call connected
    
    loop Conversation
        ACS->>Backend: Audio stream (Media WebSocket)
        Backend->>AI: STT + AI Processing
        AI-->>Backend: Response audio
        Backend->>ACS: Play audio
        ACS->>Phone: Audio to caller
    end
    
    Note over Phone,ACS: Call ends
    ACS->>Backend: CallDisconnected event
    
    opt Recording Enabled
        Backend->>Storage: Store recording
        Backend->>Storage: Store transcript
    end
```

---

## Text Data Flow

### Configuration Data

```mermaid
graph LR
    subgraph "Configuration Sources"
        AC[App Configuration<br/>appconfig-voice-agent-dev]
        KV[Key Vault<br/>kv-<suffix>]
        ENV[Environment<br/>Variables]
    end
    
    subgraph "Applications"
        FE[Frontend]
        BE[Backend]
    end
    
    AC --> |"Non-sensitive settings"| FE
    AC --> |"Non-sensitive settings"| BE
    KV --> |"Secrets & Keys"| BE
    KV --> |"Secrets & Keys"| FE
    ENV --> |"Container-level config"| FE
    ENV --> |"Container-level config"| BE
```

### Configuration Hierarchy

```yaml
# Priority (highest to lowest)
1. Environment Variables (container-level)
2. Key Vault Secrets (for sensitive data)
3. App Configuration (for runtime settings)
4. Default values (in code)
```

---

## Telemetry Data Flow

```mermaid
graph TB
    subgraph "Application Layer"
        FE[Frontend]
        BE[Backend]
        CHAT[WebChat]
    end
    
    subgraph "Collection"
        SDK[OpenTelemetry SDK]
    end
    
    subgraph "Ingestion"
        AI[Application Insights<br/>ai-<suffix>]
    end
    
    subgraph "Storage & Analysis"
        LOG[Log Analytics<br/>log-<suffix>]
        DASH[Dashboards]
        ALERTS[Alerts]
    end
    
    FE --> SDK
    BE --> SDK
    CHAT --> SDK
    SDK --> AI
    AI --> LOG
    LOG --> DASH
    LOG --> ALERTS
```

### Telemetry Types

| Type | Source | Destination | Retention |
|------|--------|-------------|-----------|
| Requests | All apps | Application Insights | 90 days |
| Dependencies | All apps | Application Insights | 90 days |
| Exceptions | All apps | Application Insights | 90 days |
| Traces | All apps | Application Insights | 90 days |
| Custom Metrics | Backend | Application Insights | 90 days |
| Container Logs | Container Apps | Log Analytics | 30 days |

---

## Data Storage Patterns

### Blob Storage Organization

```
st<suffix>/
├── audio-recordings/
│   ├── 2026/
│   │   ├── 02/
│   │   │   ├── 03/
│   │   │   │   ├── {call-id}.wav
│   │   │   │   └── {call-id}.json  (metadata)
├── transcripts/
│   ├── 2026/
│   │   ├── 02/
│   │   │   ├── 03/
│   │   │   │   └── {call-id}.json
└── exports/
    └── {export-id}/
```

### Data Lifecycle

```mermaid
graph LR
    A[Hot Storage<br/>0-7 days] --> B[Cool Storage<br/>7-30 days]
    B --> C[Archive Storage<br/>30-365 days]
    C --> D[Deletion<br/>>365 days]
```

---

## Security & Compliance

### Data Classification

| Data Type | Classification | Encryption | Retention |
|-----------|---------------|------------|-----------|
| Audio recordings | Confidential | At-rest + In-transit | 90 days |
| Transcripts | Confidential | At-rest + In-transit | 90 days |
| Configuration | Internal | At-rest | N/A |
| Secrets | Restricted | HSM-backed | N/A |
| Telemetry | Internal | At-rest + In-transit | 90 days |

### Encryption

```mermaid
graph TB
    subgraph "In Transit"
        TLS[TLS 1.2+]
    end
    
    subgraph "At Rest"
        SSE[Storage Service<br/>Encryption]
        KV_ENC[Key Vault<br/>HSM]
    end
    
    subgraph "Application"
        MI[Managed Identity<br/>No credentials in code]
    end
```

---

## Data Privacy

### PII Handling

| Data Element | Treatment |
|--------------|-----------|
| Voice audio | Stored encrypted, access-controlled |
| Phone numbers | Masked in logs |
| Transcripts | May contain PII, encrypted storage |
| IP addresses | Anonymized in telemetry |

### Data Subject Rights

For GDPR/privacy compliance:
- **Right to Access**: Query transcripts and recordings by user ID
- **Right to Deletion**: Delete audio and transcripts by call ID
- **Right to Portability**: Export data in standard formats
