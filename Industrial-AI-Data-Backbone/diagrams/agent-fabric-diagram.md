# Agent Fabric Architecture — Diagram Reference

> Visual reference for the Industrial Agent Fabric: multi-agent orchestration, human-in-the-loop workflows, and edge-to-cloud agent topology.

---

## 1. Agent Fabric — Full Topology

```mermaid
graph TB
    subgraph EDGE["Edge Layer — Plant Floor"]
        EA1[⚙️ Edge Maintenance Agent]
        EA2[🔋 Edge Energy Agent]
        EA3[🔍 Edge Quality Agent]
        EKG[Edge Knowledge Cache]
        EA1 & EA2 & EA3 <--> EKG
    end

    subgraph OT_NET["OT Network — Site Level"]
        SCADA[SCADA / DCS]
        HIST[Historian]
        MES[MES]
        SCADA & HIST & MES --> EA1
        SCADA & HIST --> EA2
        MES --> EA3
    end

    subgraph CLOUD["Cloud — Enterprise Agent Fabric"]

        subgraph ORCH["Agent Orchestrator"]
            PLANNER[🧠 Planning Engine]
            ROUTER[📡 Task Router]
            MEMORY[💾 Shared Memory Store]
            PLANNER <--> ROUTER
            PLANNER <--> MEMORY
        end

        subgraph AGENTS["Domain Agents"]
            MA[🔧 Maintenance Agent]
            PA[🏭 Production Agent]
            QA[✅ Quality Agent]
            ENA[⚡ Energy Agent]
            SA[🛡️ OT Security Agent]
        end

        subgraph TOOLS["Agent Tools & Services"]
            IKG[Industrial Knowledge Graph]
            RAG[RAG / Document Store]
            CMMS_API[CMMS API]
            ERP_API[ERP API]
            HIST_API[Historian API]
            WO[Work Order System]
        end

        subgraph HITL["Human-in-the-Loop"]
            APPROVE[👤 Approval Workflow]
            DASHBOARD[📊 Agent Dashboard]
            ALERT[🔔 Escalation Engine]
        end

        ORCH --> AGENTS
        AGENTS <--> TOOLS
        AGENTS --> HITL
        HITL --> ORCH
    end

    subgraph UNS["Unified Namespace — MQTT Broker"]
        BROKER[MQTT / Sparkplug B Broker]
    end

    EDGE <--> UNS
    UNS <--> CLOUD

    classDef edge fill:#1a3a4a,color:#e0f7fa,stroke:#00acc1
    classDef cloud fill:#1a2a3a,color:#e8eaf6,stroke:#5c6bc0
    classDef orch fill:#2a1a3a,color:#f3e5f5,stroke:#9c27b0
    classDef hitl fill:#3a2a1a,color:#fff3e0,stroke:#ff9800
    classDef uns fill:#1a3a2a,color:#e8f5e9,stroke:#43a047

    class EA1,EA2,EA3,EKG edge
    class MA,PA,QA,ENA,SA cloud
    class PLANNER,ROUTER,MEMORY orch
    class APPROVE,DASHBOARD,ALERT hitl
    class BROKER uns
```

---

## 2. Agent Orchestration — Request Lifecycle

```mermaid
sequenceDiagram
    participant SENSOR as 🏭 OT Sensor / Event
    participant UNS as 📡 Unified Namespace
    participant ORCH as 🧠 Orchestrator
    participant AGENT as 🔧 Domain Agent
    participant IKG as 🗂️ Knowledge Graph
    participant HITL as 👤 Human Approver
    participant WO as 📋 Work Order System

    SENSOR->>UNS: Publish anomaly (Sparkplug B)
    UNS->>ORCH: Event forwarded via subscription
    ORCH->>ORCH: Classify event type & severity
    ORCH->>AGENT: Assign to Maintenance Agent

    AGENT->>IKG: Query asset context & failure modes
    IKG-->>AGENT: Return asset hierarchy + RCM data
    AGENT->>AGENT: Run RCA — identify root cause

    alt Autonomous Action (Low Risk)
        AGENT->>WO: Create preventive work order
        WO-->>AGENT: Work order confirmed
        AGENT->>ORCH: Report action taken
    else Requires Approval (Medium / High Risk)
        AGENT->>HITL: Submit recommendation + evidence
        HITL->>HITL: Review recommendation
        alt Approved
            HITL->>AGENT: Approve action
            AGENT->>WO: Create corrective work order
        else Rejected / Modified
            HITL->>AGENT: Return with instructions
            AGENT->>ORCH: Re-plan with constraints
        end
    end

    ORCH->>ORCH: Update shared memory / learnings
```

---

## 3. Multi-Agent Collaboration Pattern

```mermaid
graph LR
    subgraph EVENT["Triggering Event"]
        EVT[⚠️ Production Deviation Detected]
    end

    subgraph COLLAB["Collaborative Agent Response"]
        PA[🏭 Production Agent<br/>Analyses throughput impact]
        QA[✅ Quality Agent<br/>Checks product spec compliance]
        MA[🔧 Maintenance Agent<br/>Assesses equipment health]
        ENA[⚡ Energy Agent<br/>Evaluates energy impact]
    end

    subgraph SYNTHESIS["Orchestrator Synthesis"]
        ORCH[🧠 Orchestrator<br/>Aggregates agent findings]
        REC[📋 Unified Recommendation]
    end

    subgraph ACTION["Coordinated Action"]
        ADJUST[⚙️ Process Adjustment]
        INSPECT[🔍 Targeted Inspection]
        REPORT[📊 Executive Report]
    end

    EVT --> PA & QA & MA & ENA
    PA & QA & MA & ENA --> ORCH
    ORCH --> REC
    REC --> ADJUST & INSPECT & REPORT
```

---

## 4. Human-in-the-Loop Decision Matrix

```mermaid
graph TD
    START([Agent Recommendation Ready]) --> ASSESS{Risk Assessment}

    ASSESS -->|Low Risk<br/>Routine / Reversible| AUTO[✅ Autonomous Execution<br/>Logged + Audited]
    ASSESS -->|Medium Risk<br/>Cost / Schedule Impact| NOTIFY[📧 Notification + 4hr Window<br/>Operator Can Override]
    ASSESS -->|High Risk<br/>Safety / Production Impact| APPROVE[🔐 Mandatory Approval<br/>Supervisor Sign-off Required]
    ASSESS -->|Critical Risk<br/>Safety / Environmental| ESCALATE[🚨 Escalation<br/>Multi-level Authorization]

    AUTO --> LOG[📝 Audit Trail]
    NOTIFY --> LOG
    APPROVE --> LOG
    ESCALATE --> LOG

    LOG --> LEARN[🧠 Agent Learning Loop]

    classDef auto fill:#1b5e20,color:#fff,stroke:#2e7d32
    classDef notify fill:#e65100,color:#fff,stroke:#bf360c
    classDef approve fill:#b71c1c,color:#fff,stroke:#7f0000
    classDef escalate fill:#4a148c,color:#fff,stroke:#311b92
    classDef log fill:#263238,color:#cfd8dc,stroke:#546e7a

    class AUTO auto
    class NOTIFY notify
    class APPROVE approve
    class ESCALATE escalate
    class LOG,LEARN log
```

---

## 5. Edge Agent Architecture

```mermaid
graph TB
    subgraph PLANT["Plant Floor"]
        PLC[PLC / DCS]
        SENSOR[IoT Sensors]
        HIST_LOCAL[Local Historian]
    end

    subgraph EDGE_NODE["Edge Node — Industrial PC / Gateway"]
        subgraph EDGE_AGENT["Edge Agent Runtime"]
            INFER[🤖 Local Inference Engine<br/>ONNX / TensorRT]
            CTX[📐 Context Engine<br/>ISA-95 Mapping]
            RULES[📋 Rules Engine<br/>Local Decision Logic]
            CACHE[💾 Edge Knowledge Cache]
        end

        subgraph EDGE_CONN["Connectivity"]
            OPC[OPC-UA Server]
            MQTT_E[MQTT Client]
            MODBUS[Modbus / Protocol Adapters]
        end

        MODBUS --> CTX
        OPC --> CTX
        CTX --> INFER
        INFER --> RULES
        RULES --> CACHE
        CACHE --> MQTT_E
    end

    subgraph CLOUD_FABRIC["Cloud Agent Fabric"]
        SYNC[🔄 Model Sync Service]
        CENTRAL[🧠 Central Orchestrator]
        MONITOR[📊 Fleet Monitor]
    end

    PLC & SENSOR & HIST_LOCAL --> MODBUS & OPC
    MQTT_E <-->|TLS / Sparkplug B| SYNC
    SYNC <--> CENTRAL
    CENTRAL --> MONITOR

    classDef plant fill:#1a3a1a,color:#c8e6c9,stroke:#388e3c
    classDef edge fill:#1a2a3a,color:#bbdefb,stroke:#1976d2
    classDef cloud fill:#2a1a3a,color:#e1bee7,stroke:#7b1fa2

    class PLC,SENSOR,HIST_LOCAL plant
    class INFER,CTX,RULES,CACHE,OPC,MQTT_E,MODBUS edge
    class SYNC,CENTRAL,MONITOR cloud
```

---

## 6. Agent Tool Integration Map

```mermaid
graph LR
    subgraph AGENT_LAYER["Agent Layer"]
        MA[Maintenance Agent]
        PA[Production Agent]
        QA[Quality Agent]
        ENA[Energy Agent]
        SA[Security Agent]
    end

    subgraph TOOLS["Tool Registry"]
        T1[📊 Historian Query Tool]
        T2[🗂️ Knowledge Graph Tool]
        T3[📋 CMMS Tool]
        T4[🏭 MES Tool]
        T5[📦 ERP Tool]
        T6[⚡ Energy SCADA Tool]
        T7[🛡️ SIEM Tool]
        T8[📄 Document RAG Tool]
        T9[🔔 Notification Tool]
        T10[✅ Work Order Tool]
    end

    MA --> T1 & T2 & T3 & T8 & T10
    PA --> T1 & T4 & T5 & T8
    QA --> T1 & T4 & T8 & T9
    ENA --> T1 & T6 & T8
    SA --> T7 & T2 & T9

    classDef agent fill:#1a2a4a,color:#e3f2fd,stroke:#1565c0
    classDef tool fill:#1a3a2a,color:#e8f5e9,stroke:#2e7d32

    class MA,PA,QA,ENA,SA agent
    class T1,T2,T3,T4,T5,T6,T7,T8,T9,T10 tool
```

---

## 7. Agent Fabric — Technology Stack

| Layer | Component | Technology Options |
|---|---|---|
| **Orchestration** | Agent Orchestrator | LangGraph, AutoGen, CrewAI, Azure AI Foundry |
| **LLM Backend** | Foundation Model | GPT-4o, Claude 3.5, Llama 3, Mistral |
| **Edge Inference** | Local Model Runtime | ONNX Runtime, TensorRT, Ollama |
| **Memory** | Short-term Memory | Redis, in-process state |
| **Memory** | Long-term Memory | PostgreSQL + pgvector, Pinecone, Weaviate |
| **Knowledge Graph** | Graph Database | Neo4j, Amazon Neptune, Azure Cosmos DB Gremlin |
| **RAG** | Vector Store + Retrieval | Azure AI Search, FAISS, Chroma |
| **Tool Execution** | API Gateway | FastAPI, Azure API Management |
| **Human-in-the-Loop** | Approval Workflow | Power Automate, Azure Logic Apps, custom portal |
| **Observability** | Agent Monitoring | LangSmith, Arize, Azure Monitor |
| **Security** | Agent Identity | Azure Managed Identity, OAuth 2.0, mTLS |

---

## 8. Agent Fabric — Security Architecture

```mermaid
graph TB
    subgraph IDENTITY["Identity & Access"]
        MI[Managed Identity<br/>per Agent]
        RBAC[Role-Based<br/>Tool Permissions]
        AUDIT[Immutable Audit Log<br/>All Agent Actions]
    end

    subgraph GUARDRAILS["Guardrails"]
        INPUT[Input Validation<br/>Prompt Injection Defence]
        OUTPUT[Output Validation<br/>Hallucination Checks]
        SCOPE[Action Scope Limits<br/>Write Restrictions]
    end

    subgraph ISOLATION["Isolation"]
        SANDBOX[Sandboxed Tool Execution]
        NETWORK[Network Segmentation<br/>Agent to OT Boundary]
        SECRET[Secret Management<br/>Azure Key Vault / HSM]
    end

    subgraph AGENT_CORE["Agent Core"]
        EXEC[Agent Execution]
    end

    IDENTITY --> EXEC
    GUARDRAILS --> EXEC
    EXEC --> ISOLATION

    classDef security fill:#1a1a3a,color:#e8eaf6,stroke:#5c6bc0
    class MI,RBAC,AUDIT,INPUT,OUTPUT,SCOPE,SANDBOX,NETWORK,SECRET security
```

---

*Diagrams use Mermaid syntax — render natively on GitHub, GitLab, and in VS Code with the Mermaid Preview extension.*

*For implementation guidance, see [Agent Fabric Architecture](/docs/agent-fabric-architecture.md).*
