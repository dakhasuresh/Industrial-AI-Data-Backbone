# Industrial Data Backbone Framework

## What Is the Industrial Data Backbone?

The Industrial Data Backbone is the foundational data infrastructure that makes enterprise Industrial AI possible. It is not a single product or platform — it is an architectural pattern: a deliberate, structured approach to connecting, contextualizing, and governing operational data so that AI systems have the high-quality inputs they require.

Without an Industrial Data Backbone, AI initiatives in industrial settings consistently deliver one of three outcomes:

1. **Pilot purgatory** — models that work in controlled conditions but cannot scale to production
2. **Brittle point solutions** — narrow AI applications that cannot be extended or reused
3. **Data quality failures** — models trained on decontextualized, noisy, or incomplete data that produce unreliable outputs

The Industrial Data Backbone eliminates these failure modes by solving the data problem before the AI problem.

---

## Framework Structure

The Industrial Data Backbone Framework consists of five interdependent domains:

```mermaid
graph TB
    subgraph DOMAIN1["Domain 1: Data Acquisition"]
        DA1["Source System Inventory"]
        DA2["Protocol Integration Standards"]
        DA3["Edge Architecture Patterns"]
        DA4["Data Quality Baseline"]
    end

    subgraph DOMAIN2["Domain 2: Data Integration"]
        DI1["Unified Namespace Design"]
        DI2["Event Streaming Architecture"]
        DI3["API & Service Mesh"]
        DI4["Store & Forward Resilience"]
    end

    subgraph DOMAIN3["Domain 3: Data Contextualization"]
        DC1["ISA-95 Asset Hierarchy"]
        DC2["Semantic Models"]
        DC3["Knowledge Graph"]
        DC4["Digital Twin Registry"]
    end

    subgraph DOMAIN4["Domain 4: Data Governance"]
        DG1["Data Catalog"]
        DG2["Lineage Tracking"]
        DG3["Quality Management"]
        DG4["Access Control"]
    end

    subgraph DOMAIN5["Domain 5: Data Platform"]
        DP1["Lakehouse Architecture"]
        DP2["Time-Series Management"]
        DP3["Feature Engineering"]
        DP4["Model & Insight Serving"]
    end

    DOMAIN1 --> DOMAIN2 --> DOMAIN3 --> DOMAIN4 --> DOMAIN5
```

---

## Domain 1: Data Acquisition

### Purpose
Establish reliable, standards-based connectivity to every relevant operational data source, from field devices to enterprise systems.

### Key Activities

**Source System Inventory**

Before integrating any data, document what data exists, where it lives, and what protocols are available.

| Activity | Output | Priority |
|----------|--------|----------|
| OT system audit | Source system register | High |
| Protocol capability assessment | Protocol inventory | High |
| Data quality baseline assessment | Quality scorecard | Medium |
| Data volume and frequency profiling | Capacity requirements | Medium |
| Historian tag audit | Tag inventory and rationalization plan | High |

**Protocol Integration Standards**

| Protocol | Typical Source | Integration Pattern | Notes |
|----------|---------------|--------------------|----|
| OPC-UA | PLCs, DCS, SCADA | Native connector → UNS | Preferred modern standard |
| OPC-DA | Legacy PLCs, historians | OPC-DA to OPC-UA bridge | Requires gateway |
| Modbus TCP/RTU | Field devices, meters | Protocol adapter → OPC-UA | Very common in OT |
| EtherNet/IP | Allen-Bradley PLCs | EtherNet/IP adapter | Rockwell ecosystem |
| Profinet | Siemens PLCs | Profinet adapter | Siemens ecosystem |
| MQTT / Sparkplug B | Modern IIoT devices | Native UNS ingestion | Preferred for new deployments |
| REST API | MES, ERP, CMMS | REST → Event bridge | IT systems |
| Database polling | Legacy historians | JDBC/ODBC connector | Last resort |

**Edge Architecture Pattern**

```mermaid
graph TB
    subgraph FIELD["Field Level"]
        SENSOR["Sensors / Actuators"]
        PLC["PLCs / RTUs"]
        DRIVE["Drives / Meters"]
    end

    subgraph EDGE["Edge Node (Per Plant Area)"]
        COLLECT["Data Collector\n(OPC-UA Client, Modbus Master)"]
        NORM["Normalizer\n(EU Conversion, Tag Mapping)"]
        FILTER["Filter & Compress\n(Deadband, RLE)"]
        LOCAL_AI["Local AI Inference\n(Anomaly Detection)"]
        BUFF["Store & Forward Buffer"]
        MQTT_PUB["MQTT Publisher\n(Sparkplug B)"]
    end

    subgraph CONNECTIVITY["Network"]
        OT_NET["OT Network\n(Isolated)"]
        DMZ["Industrial DMZ"]
        IT_NET["IT Network"]
    end

    SENSOR --> PLC --> OT_NET
    DRIVE --> OT_NET
    OT_NET --> COLLECT
    COLLECT --> NORM --> FILTER --> LOCAL_AI
    LOCAL_AI --> BUFF --> MQTT_PUB
    MQTT_PUB --> DMZ --> IT_NET
```

---

## Domain 2: Data Integration

### Purpose
Create a single, canonical, real-time namespace where all operational data is available to any authorized consumer without point-to-point integration.

### Unified Namespace Design Principles

1. **Single source of truth** — each data point has exactly one authoritative publisher in the namespace
2. **Decoupled producers and consumers** — producers do not know who consumes their data and vice versa
3. **ISA-95 aligned topic structure** — topic taxonomy mirrors the enterprise asset hierarchy
4. **Schema governance** — data schemas are registered, versioned, and enforced
5. **Bi-directional** — the UNS supports both data publication (OT→IT) and command/setpoint flows (IT→OT) with appropriate security controls

### Event Streaming Architecture

```mermaid
graph LR
    subgraph INGRESS["Ingress"]
        MQTT["MQTT Broker\n(HiveMQ / EMQX)"]
        KAFKA_BRIDGE["Kafka Bridge"]
    end

    subgraph STREAMING["Stream Processing"]
        KAFKA["Apache Kafka /\nAzure Event Hubs"]
        FLINK["Apache Flink /\nAzure Stream Analytics"]
        ENRICHMENT["Contextualization\nStream"]
    end

    subgraph PERSISTENCE["Persistence"]
        HOT["Hot Path\n(ADX / InfluxDB)"]
        WARM["Warm Path\n(Data Lake - Parquet)"]
        COLD["Cold Path\n(Archive)"]
    end

    MQTT --> KAFKA_BRIDGE --> KAFKA
    KAFKA --> FLINK
    FLINK --> ENRICHMENT
    ENRICHMENT --> HOT
    ENRICHMENT --> WARM
    WARM --> COLD
```

**Data velocity tiers:**

| Tier | Frequency | Technology | Retention |
|------|-----------|-----------|-----------|
| Hot (real-time) | < 1 second | ADX, InfluxDB, Redis | 7–90 days |
| Warm (operational) | 1s – 1min | Data Lakehouse (Parquet) | 1–3 years |
| Cold (historical) | > 1 min or batched | Object storage (ADLS, S3) | 7–20 years |

---

## Domain 3: Data Contextualization

### Purpose
Transform raw operational data into information — adding the who, what, where, when, and why that AI models and human users need to extract value.

### Contextualization Model

```mermaid
graph LR
    RAW_DATA["Raw Data Stream\n{tag: 'TI_101', value: 284.6}"]

    subgraph CTX_PIPELINE["Contextualization Pipeline"]
        direction TB
        HIER["1. Hierarchy Resolver\nMaps tag to ISA-95 path"]
        ASSET["2. Asset Resolver\nMaps to equipment record"]
        PARAM["3. Parameter Resolver\nMaps to process parameter"]
        PROC["4. Process State Resolver\nLinks to current product/recipe/shift"]
        KG_LOOKUP["5. Knowledge Graph Lookup\nEnriches with relationships"]
    end

    RICH_DATA["Enriched Data Record\n{\n  tag: 'TI_101',\n  value: 284.6, unit: '°C',\n  parameter: 'Feed Temperature',\n  asset_id: 'REACTOR-03',\n  asset_type: 'CSTR Reactor',\n  isa95: 'Acme/Site1/ReactArea/Train2/REACTOR-03',\n  criticality: 'Critical',\n  normal_range: [270, 295],\n  product: 'Product X',\n  shift: 'Night',\n  work_order: 'WO-20240312-001'\n}"]

    RAW_DATA --> CTX_PIPELINE --> RICH_DATA
```

### Contextualization Data Requirements

To contextualize OT data, the following master data must be maintained:

| Data Domain | Source System | Key Fields | Quality Requirement |
|-------------|--------------|------------|---------------------|
| Asset Master | CMMS / EAM | Asset ID, type, area, criticality, parent | Complete, no orphans |
| Tag Metadata | Historian / DCS | Tag name, description, EU, ranges | >95% complete |
| ISA-95 Hierarchy | Manual / ERP | Enterprise > Site > Area > Line > Unit | Fully defined |
| Process Parameters | MES / DCS | Parameter name, type, limits | Linked to assets |
| Product/Recipe | MES / LIMS | Product code, recipe ID, parameters | Linked to process |
| Failure Mode Library | CMMS | Failure code, mode, cause | RCM-based |

---

## Domain 4: Data Governance

### Purpose
Ensure industrial data is trustworthy, discoverable, and managed with appropriate access controls throughout its lifecycle.

### Industrial Data Governance Framework

```mermaid
graph TB
    subgraph PEOPLE["People & Process"]
        CDO["Data Owner\n(per domain)"]
        STEWARD["Data Steward\n(per system)"]
        CONSUMER["Data Consumer\n(AI engineers, operators)"]
    end

    subgraph POLICY["Policies"]
        QUALITY["Data Quality Policy"]
        ACCESS["Data Access Policy"]
        RETENTION["Data Retention Policy"]
        LINEAGE["Data Lineage Policy"]
    end

    subgraph TOOLING["Tooling"]
        CATALOG["Data Catalog\n(Microsoft Purview)"]
        LINEAGE_TOOL["Lineage Tracking"]
        QUALITY_MON["Quality Monitoring"]
        RBAC["Role-Based Access Control"]
    end

    PEOPLE --> POLICY --> TOOLING
```

### Data Quality Dimensions for Industrial AI

| Dimension | Definition | Target | Measurement |
|-----------|-----------|--------|-------------|
| Completeness | % of expected tags reporting values | > 98% | Missing value rate |
| Accuracy | Deviation from calibrated reference | < 0.5% error | Calibration comparison |
| Timeliness | Age of data when consumed | < 5s for real-time | Latency measurement |
| Consistency | Same value across redundant sources | 100% | Cross-source comparison |
| Validity | Values within defined engineering ranges | > 99% | Out-of-range rate |
| Uniqueness | No duplicate records | 100% | Duplicate scan |

---

## Domain 5: Data Platform

### Purpose
Provide a scalable, performant, and cost-effective data storage and serving infrastructure for industrial AI workloads.

### Lakehouse Architecture for Industrial AI

```mermaid
graph TB
    subgraph LANDING["Landing Zone (Bronze)"]
        RAW_TS["Raw Time-Series\n(Parquet / Delta)"]
        RAW_EVENTS["Raw Events\n(JSON / Parquet)"]
        RAW_RECORDS["Transactional Records\n(CSV / JSON)"]
    end

    subgraph CURATED["Curated Zone (Silver)"]
        CLEAN_TS["Cleaned & Contextualized\nTime-Series"]
        ASSET_DATA["Asset-Aligned\nOperational Data"]
        INCIDENTS["Incident & Event\nData"]
    end

    subgraph ANALYTICS["Analytics Zone (Gold)"]
        FEAT["Feature Store\n(AI Training Features)"]
        AGG["Aggregated KPIs\n(OEE, Uptime, Quality)"]
        SERVE["Serving Layer\n(Dashboard, API)"]
    end

    subgraph SPECIALISED["Specialized Stores"]
        TSDB["Time-Series DB\n(ADX / InfluxDB)"]
        GRAPH["Graph DB\n(Neo4j / CosmosDB)"]
        VECTOR["Vector Store\n(AI Search)"]
    end

    LANDING --> CURATED --> ANALYTICS
    CURATED --> SPECIALISED
    ANALYTICS --> SPECIALISED
```

### Platform Technology Selection Guide

| Requirement | Recommended Technology | Rationale |
|-------------|----------------------|-----------|
| Enterprise Microsoft environment | Microsoft Fabric | Native Entra ID, Power BI, unified governance |
| Multi-cloud / vendor neutral | Snowflake + dbt | Strong partner ecosystem, SQL-first |
| High-frequency time-series | Azure Data Explorer | Optimized for industrial telemetry at scale |
| Open source preference | Apache Iceberg + Spark | Fully open, portable, no vendor lock-in |
| Real-time analytics | ksqlDB / Flink SQL | Stream-first analytics |

---

## Implementation Sequence

The Industrial Data Backbone is built in a deliberate sequence. Skipping steps leads to the fragmentation and complexity that the framework is designed to avoid.

```mermaid
gantt
    title Industrial Data Backbone Implementation Roadmap
    dateFormat  YYYY-MM
    axisFormat  %b %Y

    section Phase 1 — Foundation
    OT System Audit & Source Inventory   :p1a, 2024-01, 6w
    Edge Node Architecture Design        :p1b, after p1a, 4w
    Protocol Integration Pilots          :p1c, after p1b, 8w

    section Phase 2 — Integration
    Unified Namespace Deployment         :p2a, after p1c, 6w
    ISA-95 Hierarchy Definition          :p2b, after p1c, 8w
    Data Platform Foundation             :p2c, after p2a, 6w

    section Phase 3 — Contextualization
    Asset Master Data Quality            :p3a, after p2b, 8w
    Contextualization Pipeline Build     :p3b, after p2b, 10w
    Knowledge Graph Foundation           :p3c, after p3b, 8w

    section Phase 4 — Analytics & AI
    Feature Engineering                  :p4a, after p3b, 8w
    First AI Use Cases (PdM, Quality)    :p4b, after p4a, 12w
    AI Governance Framework              :p4c, after p4b, 6w

    section Phase 5 — Agentic Operations
    Agent Framework Design               :p5a, after p4b, 8w
    Pilot Agents Deployment              :p5b, after p5a, 12w
    Full Agentic Operations              :p5c, after p5b, 16w
```

---

## Common Implementation Pitfalls

| Pitfall | Symptom | Prevention |
|---------|---------|-----------|
| Starting with AI before data | Model accuracy issues, pilot failures | Enforce data readiness gates before AI projects |
| Tag mapping without context | AI models with no business meaning | Build ISA-95 hierarchy before any integration |
| Skipping the UNS | Proliferating point-to-point integrations | Make UNS adoption a platform standard |
| Ignoring data quality | Garbage in, garbage out for AI models | Instrument quality metrics from day one |
| Vendor lock-in in the data layer | Cannot change platforms later | Use open formats (Parquet, Delta, Arrow) |
| No OT security design | Cyber incident during or after deployment | IEC 62443 review at every architecture phase |
| Big bang implementation | Costs overrun, value delayed | Phase-gate implementation with quick wins |

---

## Related Documents

- [Industrial AI Reference Architecture](industrial-ai-reference-architecture.md)
- [Unified Namespace Guide](unified-namespace-guide.md)
- [ISA-95 Contextualization Model](isa95-contextualization-model.md)
- [Industrial AI Maturity Model](industrial-ai-maturity-model.md)

---

## Attribution

The four architectural principles documented in this framework — protocol interoperability, Unified Namespace as data contract, IEC 62443 cyber-governance from day one, and layered data processing (Bronze/Silver/Gold) — are drawn from practitioner experience and published work by **Suresh Dakha** ([@dakhasuresh](https://github.com/dakhasuresh), HCLTech, 2026): *Why Enterprise Industrial AI Requires a Data Backbone, Not More Models.* Dakha holds ISA/IEC 62443 Expert certification and is an ISA Senior Member with delivery experience across UK gas distribution and global automotive manufacturing.
