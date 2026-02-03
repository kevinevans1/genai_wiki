# Architecture Overview

## Executive Summary

The Real-Time Audio Voice Agent is a cloud-native solution that enables real-time voice interactions with AI-powered conversational agents. Built on Azure's serverless container platform and AI services, it provides a scalable, secure, and cost-effective architecture for voice-based AI applications.

## Architecture Principles

1. **Serverless-First**: Container Apps provide automatic scaling and pay-per-use billing
2. **Zero-Trust Security**: Managed identities eliminate credential management
3. **Observability by Default**: Full telemetry pipeline from day one
4. **Infrastructure as Code**: Terraform ensures reproducible deployments
5. **Separation of Concerns**: Distinct frontend, backend, and AI layers

---

## High-Level Architecture

```mermaid
graph LR
    subgraph "User Channels"
        A[Web Browser] 
        B[Phone/PSTN]
    end

    subgraph "Ingress"
        C[Container Apps<br/>Ingress]
    end

    subgraph "Application"
        D[Frontend]
        E[Backend]
    end

    subgraph "AI"
        F[Azure AI<br/>Foundry]
    end

    subgraph "Communication"
        G[Azure<br/>Communication<br/>Services]
    end

    A --> C --> D --> E --> F
    B --> G --> E
    E --> G
```

---

## Component Architecture

### Frontend (rtaudio-client)

The frontend is a web application that provides the user interface for voice interactions.

```mermaid
graph TB
    subgraph "Frontend Container App"
        UI[Web UI]
        WS[WebSocket Client]
        AUDIO[Audio Capture]
    end
    
    UI --> AUDIO
    AUDIO --> WS
    WS --> |"Real-time audio stream"| BACKEND[Backend Service]
```

**Responsibilities**:
- Audio capture from browser microphone
- WebSocket connection management
- Real-time audio streaming to backend
- Playback of synthesized speech responses
- Visual feedback during conversations

**Technology Stack**:
- Modern JavaScript/TypeScript framework
- WebSocket for real-time communication
- Web Audio API for audio processing

---

### Backend (rtaudio-server)

The backend handles audio processing, AI orchestration, and communication services integration.

```mermaid
graph TB
    subgraph "Backend Container App"
        WS[WebSocket Server]
        AP[Audio Processor]
        AI[AI Orchestrator]
        ACS[ACS Integration]
    end
    
    CLIENT[Frontend] --> WS
    WS --> AP
    AP --> AI
    AI --> |"Speech/Text"| AZURE_AI[Azure AI Services]
    ACS --> AZURE_ACS[Azure Communication Services]
    
    AI --> ACS
```

**Responsibilities**:
- WebSocket server for client connections
- Audio stream processing and buffering
- Speech-to-text conversion via Azure AI
- Conversational AI orchestration
- Text-to-speech synthesis
- Voice calling via Azure Communication Services

---

### AI Layer

```mermaid
graph LR
    subgraph "Azure AI Foundry"
        HUB[AI Hub<br/>artagenthffwg8l2aif]
        PROJ[AI Project<br/>artagent-hffwg8l2-aif-proj]
        
        subgraph "Capabilities"
            STT[Speech-to-Text]
            TTS[Text-to-Speech]
            GPT[GPT Models]
        end
    end
    
    HUB --> PROJ
    PROJ --> STT
    PROJ --> TTS
    PROJ --> GPT
```

**Services Used**:
- **Speech-to-Text**: Real-time transcription of user speech
- **Text-to-Speech**: Natural voice synthesis for responses
- **GPT Models**: Conversational intelligence and response generation

---

## Data Flow

### Voice Conversation Flow

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant Backend
    participant AI as Azure AI
    participant ACS as Comm Services

    User->>Frontend: Speaks into microphone
    Frontend->>Backend: Stream audio via WebSocket
    Backend->>AI: Send audio for transcription
    AI-->>Backend: Return text transcript
    Backend->>AI: Generate AI response
    AI-->>Backend: Return response text
    Backend->>AI: Synthesize speech
    AI-->>Backend: Return audio stream
    Backend->>Frontend: Stream synthesized audio
    Frontend->>User: Play audio response
```

### Phone Call Flow

```mermaid
sequenceDiagram
    participant Phone as Phone/PSTN
    participant ACS as Azure Comm Services
    participant Backend
    participant AI as Azure AI

    Phone->>ACS: Incoming call
    ACS->>Backend: Call event webhook
    Backend->>ACS: Answer call
    ACS-->>Backend: Audio stream
    Backend->>AI: Process audio
    AI-->>Backend: AI response
    Backend->>ACS: Play audio
    ACS->>Phone: Audio to caller
```

---

## Security Architecture

```mermaid
graph TB
    subgraph "Identity"
        MI_FE[Frontend<br/>Managed Identity]
        MI_BE[Backend<br/>Managed Identity]
    end
    
    subgraph "Secrets"
        KV[Key Vault]
    end
    
    subgraph "Configuration"
        AC[App Configuration]
    end
    
    subgraph "Services"
        AI[AI Services]
        ACS[Comm Services]
        ST[Storage]
    end
    
    MI_FE --> KV
    MI_FE --> AC
    MI_BE --> KV
    MI_BE --> AC
    MI_BE --> AI
    MI_BE --> ACS
    MI_BE --> ST
```

**Security Features**:
- No secrets in code or environment variables
- All service-to-service auth via Managed Identity
- Key Vault for any external secrets/certificates
- App Configuration for non-sensitive settings

---

## Scalability

### Container Apps Scaling

```mermaid
graph LR
    subgraph "Auto-scaling Rules"
        HTTP[HTTP Requests]
        CPU[CPU Usage]
        MEM[Memory Usage]
        CUSTOM[Custom Metrics]
    end
    
    subgraph "Container Apps"
        R1[Replica 1]
        R2[Replica 2]
        RN[Replica N]
    end
    
    HTTP --> R1
    HTTP --> R2
    HTTP --> RN
```

**Scaling Configuration**:
- Min replicas: 0 (scale to zero when idle)
- Max replicas: Configurable based on load
- Scale triggers: HTTP concurrent requests, CPU, Memory

---

## Cost Optimization

| Component | Cost Driver | Optimization |
|-----------|-------------|--------------|
| Container Apps | vCPU-seconds, Memory-GB-seconds | Scale to zero, right-size containers |
| AI Services | API calls, audio minutes | Batch processing, caching |
| Communication Services | Minutes, phone numbers | Efficient call handling |
| Storage | GB stored, transactions | Lifecycle policies |
