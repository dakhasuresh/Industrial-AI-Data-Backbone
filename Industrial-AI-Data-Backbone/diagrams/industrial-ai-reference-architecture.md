# Industrial AI Reference Architecture Diagrams

## Diagram Index

This document contains all primary architecture diagrams for the Industrial AI Reference Architecture. Each diagram is designed to be embedded in presentations, reports, and documentation.

---

## Diagram 1: Full Stack Architecture (Layers View)

```mermaid
graph TB
    subgraph LAYER7["Layer 7 — Business Value"]
        BV1["Executive Dashboards"]
        BV2["Operational Intelligence"]
        BV3["Decision Support"]
        BV4["Autonomous Actions"]
    end

    subgraph LAYER6["Layer 6 — Agentic AI"]
        AG1["Maintenance Agent"]
        AG2["Production Agent"]
        AG3["Quality Agent"]
        AG4["Energy Agent"]
        AG5["Security Agent"]
        ORCH["Orchestrator + HITL"]
    end

    subgraph LAYER5["Layer 5 — AI & Analytics"]
        AI1["Predictive Maintenance"]
        AI2["Quality Analytics"]
        AI3["Energy Optimization"]
        AI4["Root Cause Analysis"]
        AI5["Industrial LLM / RAG"]
    end

    subgraph LAYER4["Layer 4 — Enterprise Data Platform"]
        DP1["Data Lakehouse\n(Bronze/Silver/Gold)"]
        DP2["Time-Series DB\n(ADX/InfluxDB)"]
        DP3["Knowledge Graph\n(Neo4j)"]
        DP4["Feature Store"]
    end

    subgraph LAYER3["Layer 3 — Data Contextualization"]
        C1["ISA-95 Hierarchy\nMapper"]
        C2["Asset Master\nResolver"]
        C3["Process State\nResolver"]
        C4["Knowledge Graph\nEnricher"]
    end

    subgraph LAYER2["Layer 2 — Unified Namespace"]
        UNS["Industrial MQTT Broker\n(Sparkplug B)\nISA-95 Topic Taxonomy"]
    end

    subgraph LAYER1["Layer 1 — Edge Integration"]
        E1["OPC-UA\nGateway"]
        E2["Protocol\nAdapters"]
        E3["Edge AI\nNode"]
        E4["Store &\nForward"]
    end

    subgraph LAYER0["Layer 0 — Industrial Data Sources"]
        S1["PLCs / RTUs"]
        S2["SCADA / DCS"]
        S3["Historians"]
        S4["MES / ERP"]
        S5["CMMS / EAM"]
        S6["IoT Sensors"]
        S7["LIMS / Lab"]
    end

    LAYER0 --> LAYER1 --> LAYER2 --> LAYER3 --> LAYER4 --> LAYER5 --> LAYER6 --> LAYER7
```

---

## Diagram 2: Data Contextualization Flow

```mermaid
flowchart LR
    A["🔩 Raw Tag Value\nTI_FUR01_ZA = 284.6\n@ 2024-03-12 14:22:01"]

    B["🏷️ Tag Resolution\n→ asset_id: FURNACE-01\n→ parameter: Temperature Zone A\n→ unit: °C"]

    C["🏭 Hierarchy Resolution\n→ Enterprise: Acme Corp\n→ Site: Sheffield UK\n→ Area: Furnace Area\n→ Line: Rolling Line 3\n→ Unit: Furnace 01"]

    D["⚙️ Asset Enrichment\n→ type: Rotary Hearth Furnace\n→ criticality: Critical\n→ manufacturer: SMS Group\n→ normal range: 270–295°C"]

    E["📦 Process State\n→ work_order: WO-2024-0312\n→ product: Grade A Steel\n→ shift: Day Shift\n→ operator: J.Smith"]

    F["🧠 Knowledge Graph\n→ failure_modes: Refractory Wear\n→ precursor_rank: 3 of 7\n→ similar events: 2 in last 12mo"]

    G["✅ AI-Ready Record\nFull context assembled\nReady for model inference\nReady for agent reasoning"]

    A --> B --> C --> D --> E --> F --> G
```

---

## Diagram 3: Industrial AI Maturity Progression

```mermaid
graph LR
    subgraph L1["Level 1\n📦 Data Collection"]
        l1a["Isolated systems"]
        l1b["Manual data access"]
        l1c["Excel-based analysis"]
    end

    subgraph L2["Level 2\n🔗 Data Integration"]
        l2a["Connected via UNS"]
        l2b["Real-time dashboards"]
        l2c["Basic KPI monitoring"]
    end

    subgraph L3["Level 3\n🧠 Contextualization"]
        l3a["ISA-95 hierarchy"]
        l3b["Knowledge graph"]
        l3c["Feature store live"]
    end

    subgraph L4["Level 4\n📈 Predictive Analytics"]
        l4a["PdM in production"]
        l4b["Quality AI live"]
        l4c["MLOps operational"]
    end

    subgraph L5["Level 5\n🤖 Agentic Operations"]
        l5a["Multi-agent system"]
        l5b["Autonomous actions"]
        l5c["Human-AI teaming"]
    end

    L1 -->|"OT connectivity\n& UNS deployment"| L2
    L2 -->|"ISA-95 mapping\n& knowledge graph"| L3
    L3 -->|"Feature engineering\n& ML pipeline"| L4
    L4 -->|"Agent framework\n& governance"| L5
```

---

## Diagram 4: Security Zone Architecture

```mermaid
graph TB
    subgraph Z0["🔩 Zone 0 — Process Layer\n(SL 1)"]
        FIELD_DEV["Sensors, Actuators, Drives\nOT Fieldbus (Profinet, EIP, Modbus)"]
    end

    subgraph Z1["⚙️ Zone 1 — Field Control\n(SL 2)"]
        PLC_RACK["PLC Clusters\nOT LAN (Managed Switches)"]
        SIS["Safety PLC (SIS)\n⚠️ Air-Gapped from all IT"]
    end

    subgraph Z2["🖥️ Zone 2 — Supervisory\n(SL 2–3)"]
        SCADA_H["SCADA / DCS / HMI"]
        OPCUA_PUB["OPC-UA Server"]
        HISTORIAN_S["Process Historian"]
    end

    subgraph DMZ_BOX["🔒 Industrial DMZ\n(Highest security — SL 3)"]
        OT_FW1["IEC 62443 Firewall\n(OT → DMZ)"]
        DATA_DIODE_BOX["Unidirectional Gateway\n(Data Diode — for highest risk)"]
        JUMP_B["Bastion Jump Server\n(PAM + MFA)"]
        IT_FW1["IEC 62443 Firewall\n(DMZ → IT)"]
    end

    subgraph Z3_BOX["🏢 Zone 3 — IT/OT Integration\n(SL 2)"]
        UNS_BROKER["UNS Broker\n(HiveMQ)"]
        MES_IT["MES / CMMS Systems"]
        EDGE_COMPUTE["iEdgeX Edge Nodes"]
    end

    subgraph Z4_BOX["☁️ Zone 4 — Enterprise/Cloud\n(IT Controls)"]
        CLOUD_PLAT["Cloud AI Platform"]
        CORP_NET["Corporate Network"]
        REMOTE["Remote Access\n(ZTNA)"]
    end

    Z0 <--> Z1
    Z1 <--> Z2
    Z2 --> OT_FW1
    OT_FW1 --> DATA_DIODE_BOX
    DATA_DIODE_BOX --> IT_FW1
    IT_FW1 --> Z3_BOX
    JUMP_B -.->|Controlled inbound| Z2
    Z3_BOX --> Z4_BOX

    style SIS fill:#7b0000,color:#fff,stroke:#ff0000
    style DMZ_BOX fill:#3d3000,color:#eee
```

---

## Diagram 5: Agent Orchestration Flow

```mermaid
flowchart TD
    TRIGGER["⚡ Trigger Event\nPdM Alert: Compressor C-301\n89% failure probability\nEstimated: 3 days to failure"]

    ORCH["🎯 Agent Orchestrator\nIntent: Maintenance + Production impact\nActivate: Maintenance Agent + Production Agent"]

    MAINT["🔧 Maintenance Agent\nQuery KG: failure history\nQuery CMMS: parts, technicians\nQuery Calendar: maintenance windows\n→ Plan: Wed 06:00, 8hr, £4,800"]

    PROD["🏭 Production Agent\nAssess: C-301 feeds Train A\nCheck: Can Train B cover?\nRisk: 4hr partial rate reduction\n→ Impact: Manageable, no customer impact"]

    COMBINE["📋 Orchestrator Compiles Recommendation\nMaintenance plan + Production impact\nRisk: Not acting = £180K lost production\nCost: £4,800 PM + production reduction\nRecommendation: Proceed Wednesday"]

    APPROVAL["👤 Human Approval Gate\nSent to: Maintenance Manager\nChannel: Mobile notification + dashboard\nWindow: 4 hours to approve/modify/reject"]

    APPROVED{"Approved?"}

    WO_CREATE["✅ Create Work Order\nWO-2024-1188 in CMMS\nParts reserved\nTechnician scheduled\nKnowledge brief generated"]

    DEFER["⏸️ Defer / Modify\nReschedule or adjust plan\nRe-run optimization\nLog reasoning"]

    AUDIT["📝 Audit Log\nAll agent reasoning logged\nApproval recorded\nActions timestamped\nImmutable trail"]

    TRIGGER --> ORCH
    ORCH --> MAINT & PROD
    MAINT & PROD --> COMBINE
    COMBINE --> APPROVAL
    APPROVAL --> APPROVED
    APPROVED -->|Yes| WO_CREATE
    APPROVED -->|No| DEFER
    WO_CREATE --> AUDIT
    DEFER --> AUDIT
```

---

## Diagram 6: iEdgeX Three-Pillar Model

```mermaid
graph TB
    subgraph P1["PILLAR 1\nOT/IT Connectivity & Data Integration"]
        direction LR
        P1A["Industrial Protocol\nSupport (OPC-UA, Modbus,\nSparkplug B, EIP, Profinet)"]
        P1B["iEdgeX Edge Nodes\n(Per area / zone)"]
        P1C["Unified Namespace\n(HiveMQ + Schema Registry)"]
        P1D["IEC 62443 Security\n(DMZ, mTLS, PKI)"]
    end

    subgraph P2["PILLAR 2\nIndustrial Data Foundation & Contextualization"]
        direction LR
        P2A["ISA-95 Context\nPipeline"]
        P2B["Industrial\nKnowledge Graph"]
        P2C["Data Lakehouse\n(Bronze/Silver/Gold)"]
        P2D["Feature Store\n& Data Governance"]
    end

    subgraph P3["PILLAR 3\nAI, Analytics & Agentic Intelligence"]
        direction LR
        P3A["Predictive Analytics\n(PdM, Quality, Energy)"]
        P3B["Industrial LLM\n+ RAG Engine"]
        P3C["Agent Fabric\n(Multi-agent + HITL)"]
        P3D["Business Applications\n& Dashboards"]
    end

    FIELD_ASSETS["🏭 Industrial Assets\n(OT Systems, Sensors, Historians)"]
    ENTERPRISE_VALUE["📊 Enterprise Value\n(Reduced Costs, Higher Throughput, Better Safety)"]

    FIELD_ASSETS --> P1 --> P2 --> P3 --> ENTERPRISE_VALUE
```

---

## Related Documents

- [Edge-to-Cloud Architecture](edge-to-cloud-architecture.md)
- [Agent Fabric Diagram](agent-fabric-diagram.md)
- [Industrial AI Reference Architecture](../docs/industrial-ai-reference-architecture.md)
