# Industrial Data Platform Assessment Template

> **Purpose:** Evaluate the current state of your industrial data platform and identify gaps against the Industrial AI Data Backbone reference architecture.
>
> **How to use:** Complete each section with your team. Use the scoring guidance provided. Output a Gap Analysis and prioritised recommendations.
>
> **Assessment Team:** OT Architect, IT Architect, Data Engineer, OT/OT Security Lead, Operations Lead

---

## Assessment Details

| Field | Value |
|---|---|
| **Organisation** | [Company Name] |
| **Site / Division** | [Site / Business Unit] |
| **Assessment Date** | [DD/MM/YYYY] |
| **Assessment Lead** | [Name, Title] |
| **Review Date** | [DD/MM/YYYY — typically 12 months] |

---

## Scoring Guide

| Score | Label | Description |
|---|---|---|
| **0** | Not Present | Capability does not exist |
| **1** | Informal | Ad hoc, undocumented, individual-dependent |
| **2** | Defined** | Documented but inconsistently applied |
| **3** | Managed** | Consistently applied, monitored |
| **4** | Optimised** | Continuously improved, measured against KPIs |
| **5** | Advanced** | Industry-leading, automated, self-improving |

---

## Domain 1: Data Acquisition & Connectivity

*Assesses the ability to reliably collect data from OT and industrial sources.*

### 1.1 OT Connectivity

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| PLC / DCS connectivity (OPC-UA or native) | | | | |
| SCADA system data extraction | | | | |
| Historian data collection (all key assets) | | | | |
| IoT / sensor data ingestion | | | | |
| Protocol support breadth (Modbus, Profinet, EtherNet/IP) | | | | |
| Real-time data streaming (< 1 second latency capability) | | | | |
| Historian tag coverage (% of critical assets) | | | | |

**Sub-domain Score:** [Sum / 35] = [%]

### 1.2 Edge Infrastructure

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Edge compute nodes deployed at OT sites | | | | |
| Edge data buffering / store-and-forward | | | | |
| Edge data preprocessing / filtering | | | | |
| Edge node management (remote update capability) | | | | |
| Edge node redundancy / resilience | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 1.3 Data Ingestion Quality

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Data completeness monitoring | | | | |
| Timestamp accuracy and alignment | | | | |
| Signal quality / bad value handling | | | | |
| Ingestion pipeline monitoring and alerting | | | | |
| Data volume scalability | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

**Domain 1 Total Score:** [Sum] / 85 = **[%]**

---

## Domain 2: Data Integration & Unification

*Assesses the ability to break data silos and create a unified data layer.*

### 2.1 Unified Namespace (UNS)

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| MQTT broker deployed and operational | | | | |
| Sparkplug B or structured payload standard adopted | | | | |
| All OT sources publishing to UNS | | | | |
| IT systems (ERP, CMMS) integrated with UNS | | | | |
| UNS topic hierarchy documented and governed | | | | |
| UNS security (authentication, authorisation, TLS) | | | | |
| UNS monitoring and health dashboards | | | | |

**Sub-domain Score:** [Sum / 35] = [%]

### 2.2 IT/OT Integration

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| ERP data accessible to OT analytics | | | | |
| CMMS data integrated with asset data | | | | |
| MES data connected to real-time OT data | | | | |
| Lab / LIMS data integrated | | | | |
| Cross-system data join capability | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 2.3 Data Pipeline Architecture

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Real-time streaming pipeline (e.g., Kafka, Event Hubs) | | | | |
| Batch ingestion pipeline | | | | |
| Pipeline orchestration and scheduling | | | | |
| Pipeline failure recovery and retry | | | | |
| Data lineage tracking | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

**Domain 2 Total Score:** [Sum] / 85 = **[%]**

---

## Domain 3: Data Contextualization

*Assesses the ability to transform raw data into contextualised, semantically meaningful information.*

### 3.1 Asset Hierarchy & Naming

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| ISA-95 equipment hierarchy documented | | | | |
| Consistent asset naming convention adopted | | | | |
| Tag naming aligned to asset hierarchy | | | | |
| Asset register complete and maintained | | | | |
| Equipment taxonomy standardised | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 3.2 Semantic Enrichment

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Unit of measure standardisation | | | | |
| Process variable classification (pressure, temp, flow, etc.) | | | | |
| Operational state / mode tagging | | | | |
| Production context (product, recipe, shift) linked to data | | | | |
| Alarm and event contextualisation | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 3.3 Industrial Knowledge Graph

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Knowledge graph deployed | | | | |
| Asset relationships modelled | | | | |
| Failure modes and causes modelled (RCM/FMEA data) | | | | |
| Knowledge graph connected to live operational data | | | | |
| Knowledge graph query capability for AI models | | | | |
| Knowledge graph maintained and versioned | | | | |

**Sub-domain Score:** [Sum / 30] = [%]

### 3.4 Data Lakehouse Architecture

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Bronze layer (raw ingestion) operational | | | | |
| Silver layer (cleansed, standardised) operational | | | | |
| Gold layer (business-ready, contextualised) operational | | | | |
| Delta Lake / Iceberg or equivalent format adopted | | | | |
| Time-travel / data versioning capability | | | | |
| Query performance meets analytical SLAs | | | | |

**Sub-domain Score:** [Sum / 30] = [%]

**Domain 3 Total Score:** [Sum] / 110 = **[%]**

---

## Domain 4: Data Governance

*Assesses the policies, controls, and processes that ensure data is trusted, compliant, and well-managed.*

### 4.1 Data Quality Management

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Data quality rules defined and enforced | | | | |
| Data quality monitoring dashboards | | | | |
| Data quality SLAs defined per data domain | | | | |
| Data quality issue resolution process | | | | |
| Data profiling performed regularly | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 4.2 Master Data Management

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Equipment master data managed centrally | | | | |
| Tag master data aligned across systems | | | | |
| Product / material master data consistent | | | | |
| Supplier / vendor master data integrated | | | | |

**Sub-domain Score:** [Sum / 20] = [%]

### 4.3 Data Catalogue & Discovery

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Data catalogue tool deployed | | | | |
| Key data assets catalogued with metadata | | | | |
| Data ownership assigned per domain | | | | |
| Data search / discovery capability for analysts | | | | |
| Data definitions / business glossary maintained | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 4.4 Compliance & Retention

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Data retention policies defined and enforced | | | | |
| Regulatory compliance requirements mapped | | | | |
| Audit trail for data changes | | | | |
| Data sovereignty / residency requirements met | | | | |

**Sub-domain Score:** [Sum / 20] = [%]

**Domain 4 Total Score:** [Sum] / 90 = **[%]**

---

## Domain 5: Analytics & AI Platform

*Assesses the maturity of the analytics and AI model development, deployment, and operations capability.*

### 5.1 Analytics Capability

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Self-service analytics for operations teams | | | | |
| Real-time operational dashboards | | | | |
| Historical trend analysis capability | | | | |
| Cross-site benchmarking and analytics | | | | |
| Root cause analysis tools | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 5.2 MLOps & AI Development

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| ML experiment tracking and versioning | | | | |
| Model registry and governance | | | | |
| Automated model training pipelines | | | | |
| Model deployment / serving infrastructure | | | | |
| Model performance monitoring | | | | |
| Model drift detection and retraining | | | | |

**Sub-domain Score:** [Sum / 30] = [%]

### 5.3 AI Models in Production

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Predictive maintenance models deployed | | | | |
| Energy optimisation models deployed | | | | |
| Quality analytics models deployed | | | | |
| Process optimisation models deployed | | | | |
| Anomaly detection models deployed | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 5.4 Agentic AI

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| AI agent framework deployed | | | | |
| Domain agents operational (maintenance, quality, energy) | | | | |
| Agent orchestration capability | | | | |
| Human-in-the-loop workflows implemented | | | | |
| Agent audit trail and explainability | | | | |
| Agent security controls (identity, scope limits) | | | | |

**Sub-domain Score:** [Sum / 30] = [%]

**Domain 5 Total Score:** [Sum] / 110 = **[%]**

---

## Domain 6: OT Security

*Assesses the security posture of the industrial data platform, aligned to IEC 62443.*

### 6.1 Network Security

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| OT network segmentation (zones and conduits) | | | | |
| IT/OT DMZ implemented | | | | |
| Firewall rules reviewed and documented | | | | |
| Remote access controls (VPN / PAM) | | | | |
| Network traffic monitoring (OT) | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 6.2 Identity & Access Management

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| OT asset inventory up to date | | | | |
| Role-based access control for OT systems | | | | |
| Multi-factor authentication for privileged access | | | | |
| Service account / machine identity management | | | | |
| Privileged Access Management (PAM) deployed | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

### 6.3 AI & Data Platform Security

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| Data platform access controls (RBAC / ABAC) | | | | |
| Data encryption at rest and in transit | | | | |
| AI model access controls | | | | |
| Agent identity and permission management | | | | |
| Security monitoring for cloud data platform | | | | |
| Secrets management (API keys, credentials) | | | | |

**Sub-domain Score:** [Sum / 30] = [%]

### 6.4 Governance & Compliance

| Capability | Score (0–5) | Evidence / Notes | Target Score | Priority |
|---|---|---|---|---|
| IEC 62443 gap assessment completed | | | | |
| Security policies and procedures documented | | | | |
| Vulnerability management process | | | | |
| Incident response plan (OT) | | | | |
| Security awareness training for OT staff | | | | |

**Sub-domain Score:** [Sum / 25] = [%]

**Domain 6 Total Score:** [Sum] / 105 = **[%]**

---

## Assessment Summary

### Domain Scores

| Domain | Max Score | Actual Score | Percentage | RAG |
|---|---|---|---|---|
| 1. Data Acquisition & Connectivity | 85 | | | 🔴/🟡/🟢 |
| 2. Data Integration & Unification | 85 | | | |
| 3. Data Contextualization | 110 | | | |
| 4. Data Governance | 90 | | | |
| 5. Analytics & AI Platform | 110 | | | |
| 6. OT Security | 105 | | | |
| **Total** | **585** | | | |

*RAG: 🔴 < 40% | 🟡 40–69% | 🟢 ≥ 70%*

### Maturity Level Mapping

| Score Range | Maturity Level | Description |
|---|---|---|
| 0–20% | Level 1 — Data Collection | Isolated, informal data collection |
| 21–40% | Level 2 — Data Integration | Basic integration, limited governance |
| 41–60% | Level 3 — Data Contextualization | Structured, contextualised data platform |
| 61–80% | Level 4 — Predictive Analytics | AI models in production |
| 81–100% | Level 5 — Agentic Operations | Autonomous, self-improving operations |

**Current Maturity Level:** Level [ ] — [Description]

---

## Top 10 Prioritised Recommendations

| Priority | Recommendation | Domain | Effort | Impact | Timeline |
|---|---|---|---|---|---|
| 1 | [e.g., Deploy MQTT broker and UNS structure] | Integration | Medium | High | Q[X] |
| 2 | [e.g., Complete ISA-95 asset hierarchy mapping] | Contextualization | Low | High | Q[X] |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |
| 6 | | | | | |
| 7 | | | | | |
| 8 | | | | | |
| 9 | | | | | |
| 10 | | | | | |

---

## Assessment Sign-off

| Role | Name | Signature | Date |
|---|---|---|---|
| Assessment Lead | | | |
| OT Architect | | | |
| IT Architect | | | |
| Operations Lead | | | |
| Security Lead | | | |
| Executive Sponsor | | | |

---

*Template version: 1.0 | Industrial AI Data Backbone Reference Architecture*
*For scoring interpretation, see [/docs/industrial-ai-maturity-model.md](/docs/industrial-ai-maturity-model.md)*
