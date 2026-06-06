# Edge-to-Cloud Architecture Diagram

## Overview

This document provides the complete visual reference for the Industrial Edge-to-Cloud AI architecture — from field sensors to enterprise intelligence.

---

## Full Edge-to-Cloud Architecture

```mermaid
graph TB
    classDef field fill:#1c2833,color:#eee,stroke:#2e4057
    classDef edge fill:#1a3a4a,color:#eee,stroke:#2980b9
    classDef uns fill:#1a4035,color:#eee,stroke:#27ae60
    classDef context fill:#3d1a4a,color:#eee,stroke:#8e44ad
    classDef platform fill:#1a2a5a,color:#eee,stroke:#2980b9
    classDef ai fill:#1a4a2a,color:#eee,stroke:#27ae60
    classDef agent fill:#4a2a1a,color:#eee,stroke:#e67e22
    classDef security fill:#2a1a1a,color:#eee,stroke:#e74c3c

    subgraph FIELD["🔩 Field Level — Industrial Data Sources"]
        PLC1["PLC\n(Siemens S7-1500)"]:::field
        PLC2["PLC\n(Allen-Bradley 5000)"]:::field
        DCS["DCS\n(Honeywell Experion)"]:::field
        HIST["Process Historian\n(OSIsoft PI / Aveva)"]:::field
        MES["MES\n(SAP ME / Aveva MES)"]:::field
        CMMS["CMMS / EAM\n(SAP PM / IBM Maximo)"]:::field
        IOT["IIoT Sensors\n(Vibration, Temperature, Flow)"]:::field
        LIMS["LIMS\n(LabWare / STARLIMS)"]:::field
    end

    subgraph EDGE["⚙️ Edge Layer — Protocol Translation & Local Analytics"]
        GW_A["Industrial Edge Node A\n(Area 1)\nOPC-UA + Sparkplug B"]:::edge
        GW_B["Industrial Edge Node B\n(Area 2)\nModbus → OPC-UA"]:::edge
        GW_C["Industrial Edge Node C\n(Utilities)\nBACnet + Modbus"]:::edge
        EDGE_AI["Edge AI Runtime\n(Anomaly Detection + Local Rules)"]:::edge
        STORE_FWD["Store & Forward\n(72hr buffer)"]:::edge
    end

    subgraph NET["🔒 Industrial DMZ — Secure Boundary (IEC 62443)"]
        FIREWALL_OT["OT Firewall\n(Zone 2 → DMZ)"]:::security
        FIREWALL_IT["IT Firewall\n(DMZ → Zone 3)"]:::security
        JUMP["Privileged Jump Server\n(Remote Access)"]:::security
    end

    subgraph UNS_LAYER["🔗 Unified Namespace — Single Source of Truth"]
        BROKER["MQTT Broker Cluster\n(HiveMQ HA 3-node)\nSparkplug B"]:::uns
        SCHEMA["Schema Registry\n(Avro / JSON Schema)"]:::uns
        CATALOG_LIVE["Live Data Catalog\n(Topic Directory)"]:::uns
        KAFKA["Event Streaming\n(Azure Event Hubs / Kafka)"]:::uns
    end

    subgraph CTX["🧠 Contextualization Engine — Data Becomes Information"]
        ISA95_PIPE["ISA-95 Mapping\nPipeline"]:::context
        ASSET_MASTER["Asset Master\nResolution (CMMS)"]:::context
        PROC_STATE["Process State\nResolver (MES/ERP)"]:::context
        KG_ENRICH["Knowledge Graph\nEnricher (Neo4j)"]:::context
    end

    subgraph PLATFORM["☁️ Enterprise Data Platform"]
        BRONZE["Data Lakehouse\nBronze Layer\n(Raw / Delta)"]:::platform
        SILVER["Data Lakehouse\nSilver Layer\n(Contextualized)"]:::platform
        GOLD["Data Lakehouse\nGold Layer\n(Feature-ready)"]:::platform
        ADX["Azure Data Explorer\n(Time-Series / Hot)"]:::platform
        GRAPH_DB["Neo4j\n(Knowledge Graph)"]:::platform
        FEAT_STORE["Feature Store\n(ML Features)"]:::platform
        VECTOR_STORE["Vector Store\n(Azure AI Search)"]:::platform
    end

    subgraph AI_LAYER["🤖 AI & Analytics Layer"]
        PDM["Predictive\nMaintenance"]:::ai
        QUAL_AI["Quality\nPrediction"]:::ai
        ENERGY_AI["Energy\nOptimization"]:::ai
        ANOM_AI["Anomaly\nDetection"]:::ai
        RCA_AI["Root Cause\nAnalysis"]:::ai
        LLM_RAG["Industrial LLM\n+ RAG Engine"]:::ai
    end

    subgraph AGENTS["🦾 Agent Fabric — Autonomous Industrial Intelligence"]
        ORCH_AGENT["Agent Orchestrator\n(Azure AI Agents)"]:::agent
        MAINT_AG["Maintenance\nAgent"]:::agent
        PROD_AG["Production\nAgent"]:::agent
        QUAL_AG["Quality\nAgent"]:::agent
        ENERGY_AG["Energy\nAgent"]:::agent
        SEC_AG["OT Security\nAgent"]:::agent
        HITL_GATE["Human-in-the-Loop\nApproval Gate"]:::agent
    end

    subgraph BIZ["📊 Business Intelligence & Applications"]
        EXEC_DASH["Executive\nDashboard (Power BI)"]
        OPS_INTEL["Operational\nIntelligence"]
        MOBILE_OPS["Mobile Operations\n(Field Worker App)"]
        CMMS_WO["CMMS Work Order\nIntegration"]
        ERP_INT["ERP / SAP\nIntegration"]
    end

    FIELD --> EDGE
    EDGE --> NET
    NET --> UNS_LAYER
    UNS_LAYER --> CTX
    CTX --> PLATFORM
    PLATFORM --> AI_LAYER
    AI_LAYER --> AGENTS
    AGENTS --> HITL_GATE
    HITL_GATE --> BIZ

    CMMS --> ASSET_MASTER
    MES --> PROC_STATE
    LIMS --> SILVER
```

---

## Data Flow Diagram

End-to-end data flow from sensor reading to business action:

```mermaid
sequenceDiagram
    participant SENSOR as 🔩 Sensor
    participant EDGE as ⚙️ Edge Node
    participant UNS as 🔗 UNS Broker
    participant CTX_E as 🧠 Context Engine
    participant PLATFORM as ☁️ Data Platform
    participant AI_M as 🤖 AI Model
    participant AGENT as 🦾 Agent
    participant HUMAN as 👤 Human
    participant CMMS as 📋 CMMS

    SENSOR->>EDGE: Raw value (100ms)
    EDGE->>EDGE: Normalize, filter, deadband check
    EDGE->>UNS: Sparkplug B MQTT (Contextualized topic)
    UNS->>CTX_E: Event stream subscription
    CTX_E->>CTX_E: ISA-95 lookup, asset enrichment, KG enrichment
    CTX_E->>PLATFORM: Enriched event to Data Lakehouse + ADX
    PLATFORM->>AI_M: Real-time features served from Feature Store
    AI_M->>AI_M: Inference: Failure probability = 87%
    AI_M->>AGENT: Alert: PUMP-07, bearing failure, 87%, 4 days
    AGENT->>AGENT: Query KG for repair history, check parts, check schedule
    AGENT->>HUMAN: Work order recommendation + evidence summary
    HUMAN->>AGENT: Approve (30 min window)
    AGENT->>CMMS: Create confirmed work order WO-2024-1142
    CMMS->>HUMAN: Technician notified with full AI brief
```

---

## Network Architecture Diagram

```mermaid
graph LR
    subgraph ZONE0["Zone 0 — Field Network"]
        FD["Field Devices\n(Sensors, Actuators, Drives)"]
        OT_FIELDBUS["OT Fieldbus\n(Profinet, EIP, Modbus)"]
    end

    subgraph ZONE1["Zone 1 — Control Network"]
        PLC_CLUSTER["PLC Cluster\n(per line or area)"]
        SAFETY_SYS["Safety PLC\n(SIL-rated, air-gapped)"]
        OT_LAN["OT LAN\n(Managed Layer 2)"]
    end

    subgraph ZONE2["Zone 2 — Supervisory Network"]
        SCADA_SERVER["SCADA / DCS\nServer"]
        HISTORIAN_SRV["Historian Server"]
        ENG_WS["Engineering\nWorkstation"]
        OPCUA_SRV["OPC-UA Server"]
    end

    subgraph DMZ["Industrial DMZ"]
        OT_FW["OT Firewall\n(e.g., Fortinet NGFW)"]
        JUMP_SERVER["Jump Server\n(PAM-managed)"]
        DATA_RELAY["Data Relay /\nProxy"]
        IT_FW["IT Firewall"]
    end

    subgraph ZONE3["Zone 3 — OT Supervisory / IT"]
        MQTT_BROKER["UNS Broker\n(HiveMQ)"]
        EDGE_NODES[" Edge\nNodes"]
        MES_SERVER["MES Server"]
        CMMS_SERVER["CMMS Server"]
    end

    subgraph CLOUD["Cloud / Zone 4"]
        CLOUD_PLATFORM["Azure / AWS\nCloud Platform"]
        AI_SERVICES["AI & Analytics\nServices"]
        CORP_NETWORK["Corporate\nNetwork"]
    end

    FD <-->|Fieldbus| PLC_CLUSTER
    PLC_CLUSTER <-->|OT LAN| SCADA_SERVER
    SCADA_SERVER <-->|OPC-UA| OPCUA_SRV
    OPCUA_SRV -->|Controlled| OT_FW
    OT_FW --> DATA_RELAY
    DATA_RELAY --> IT_FW
    IT_FW --> MQTT_BROKER
    MQTT_BROKER --> CLOUD_PLATFORM
    CLOUD_PLATFORM --> AI_SERVICES
    JUMP_SERVER -.->|PAM session\n(inbound only)| ZONE2

    style SAFETY_SYS fill:#7b0000,color:#fff
    style DMZ fill:#3d3d00,color:#fff
```

---

## Deployment Topology Options

### Option 1: Cloud-First (Azure)

```mermaid
graph LR
    EDGE_NODE["Industrial Edge Node\n(On-premises)"] -->|"MQTT/TLS → Event Hub"| AZURE_EH["Azure Event Hub"]
    AZURE_EH --> AZURE_STREAM["Azure Stream Analytics\n(Real-time processing)"]
    AZURE_STREAM --> ADX["Azure Data Explorer\n(Hot path)"]
    AZURE_STREAM --> ADLS["Azure Data Lake\nStorage Gen2 (Warm/Cold)"]
    ADLS --> FABRIC["Microsoft Fabric\n(Analytics + AI)"]
    ADX --> FABRIC
    FABRIC --> OPENAI["Azure OpenAI\n(LLM)"]
    FABRIC --> POWERBI["Power BI\n(Dashboards)"]
    FABRIC --> AI_AGENTS["Azure AI Agents\n(Agent Fabric)"]
```

### Option 2: On-Premises + Cloud Hybrid

```mermaid
graph LR
    EDGE[" Edge\n(Per area)"]
    ON_PREM_UNS["On-Premises UNS\n(HiveMQ)"]
    ON_PREM_DATA["On-Premises Data Platform\n(Historian + SQL Server)"]
    CLOUD_BRIDGE["Selective Cloud Sync\n(Aggregated / anonymized)"]
    CLOUD_AI["Cloud AI Training\n& Advanced Analytics"]
    EDGE_AI["Edge AI Inference\n(Models deployed to edge)"]

    EDGE --> ON_PREM_UNS --> ON_PREM_DATA
    ON_PREM_DATA --> CLOUD_BRIDGE --> CLOUD_AI
    CLOUD_AI -->|Model updates| EDGE_AI
    EDGE_AI --> EDGE
```

---

## Related Documents

- [Industrial AI Reference Architecture](../docs/industrial-ai-reference-architecture.md)
- [Industrial AI Reference Architecture](../docs/industrial-ai-reference-architecture.md)
- [Agent Fabric Diagram](agent-fabric-diagram.md)
