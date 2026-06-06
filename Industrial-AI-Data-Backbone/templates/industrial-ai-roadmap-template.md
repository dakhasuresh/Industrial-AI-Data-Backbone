# Industrial AI Programme Roadmap Template

> **Usage:** Copy this template into your organisation's documentation system. Replace all `[bracketed]` placeholders with your specific details. This template supports the Industrial AI Maturity Model — from data collection through to agentic operations.

---

## Programme Identity

| Field | Value |
|---|---|
| **Programme Name** | [e.g., Industrial AI Transformation — Phase 1] |
| **Organisation** | [Company Name] |
| **Business Unit / Site** | [Division / Plant / Region] |
| **Programme Sponsor** | [Executive Name, Title] |
| **Programme Manager** | [Name, Title] |
| **Lead Architect** | [Name, Title] |
| **Baseline Date** | [DD/MM/YYYY] |
| **Target Completion** | [DD/MM/YYYY] |
| **Total Budget (Indicative)** | [£ / $ / €] |
| **Current Maturity Level** | [Level 1–5] |
| **Target Maturity Level** | [Level 1–5] |

---

## Executive Summary

> *[2–3 paragraph summary of the programme. Describe the business drivers, strategic objectives, and expected outcomes. Write for a Board or Executive Committee audience.]*

**Business Drivers:**
- [Driver 1: e.g., reduce unplanned downtime by 30%]
- [Driver 2: e.g., improve OEE from 72% to 85%]
- [Driver 3: e.g., reduce energy cost per unit by 15%]

**Strategic Alignment:**
- [Link to corporate strategy 1]
- [Link to corporate strategy 2]

---

## Current State Assessment

### Data Maturity Assessment

| Domain | Current Score (1–5) | Target Score (1–5) | Gap | Priority |
|---|---|---|---|---|
| Data Acquisition | | | | |
| Data Integration | | | | |
| Data Quality | | | | |
| Data Contextualization | | | | |
| Data Governance | | | | |
| Analytics Capability | | | | |
| AI/ML Capability | | | | |
| OT Security Posture | | | | |

### Key Pain Points

1. [Pain Point 1 — e.g., data silos between OT and IT systems]
2. [Pain Point 2 — e.g., no standardised asset naming/hierarchy]
3. [Pain Point 3 — e.g., manual quality inspection processes]
4. [Pain Point 4]
5. [Pain Point 5]

### Existing Systems Inventory

| System | Category | Vendor | Version | Integration Status | Data Quality |
|---|---|---|---|---|---|
| [System Name] | [PLC/SCADA/MES/ERP/etc.] | [Vendor] | [Version] | [Connected/Isolated/Planned] | [Good/Fair/Poor] |
| | | | | | |
| | | | | | |

---

## Programme Vision

### Target Architecture

> *[Describe the target state architecture in 1–2 paragraphs. Reference the iEdgeX Reference Architecture or your chosen framework.]*

### Success Metrics (KPIs)

| KPI | Baseline | 12-Month Target | 24-Month Target | 36-Month Target |
|---|---|---|---|---|
| Overall Equipment Effectiveness (OEE) | [%] | [%] | [%] | [%] |
| Unplanned Downtime (hrs/month) | | | | |
| Mean Time Between Failure (MTBF) | | | | |
| Energy Intensity (kWh/unit) | | | | |
| First Pass Quality Rate | [%] | [%] | [%] | [%] |
| AI-driven Work Orders (%) | [%] | [%] | [%] | [%] |
| Data Availability Score | [%] | [%] | [%] | [%] |
| [Custom KPI] | | | | |

---

## Programme Roadmap

### Phase Structure Overview

```
Phase 1: Data Foundation          [Months 1–6]    Maturity: L1 → L2
Phase 2: Contextualization         [Months 4–12]   Maturity: L2 → L3
Phase 3: Analytics & Prediction    [Months 10–18]  Maturity: L3 → L4
Phase 4: Agentic Operations        [Months 16–30]  Maturity: L4 → L5
```

---

### Phase 1: Data Foundation

**Objective:** Establish reliable, integrated data collection from OT sources.

**Duration:** [Month X – Month Y]
**Budget:** [£/$ amount]
**Maturity Progression:** Level 1 → Level 2

#### Workstreams

| Workstream | Description | Owner | Duration | Dependencies |
|---|---|---|---|---|
| OT Connectivity | Deploy OPC-UA / MQTT adapters to priority assets | [Name] | [Weeks] | Network access, PLC firmware |
| UNS Deployment | Install and configure MQTT broker + Sparkplug B | [Name] | [Weeks] | OT connectivity |
| Edge Infrastructure | Deploy edge compute nodes at priority sites | [Name] | [Weeks] | Network, power |
| Data Historian | Connect/upgrade historian for all priority tags | [Name] | [Weeks] | OT connectivity |
| Asset Register | Cleanse and standardise asset naming | [Name] | [Weeks] | Stakeholder alignment |
| OT Security Baseline | Zone/conduit model, IEC 62443 gap closure | [Name] | [Weeks] | Security assessment |

#### Phase 1 Deliverables

- [ ] OT connectivity established for [N] priority assets
- [ ] MQTT/Sparkplug B broker operational
- [ ] [N] data tags flowing into historian / data lake
- [ ] Edge nodes deployed at [N] sites
- [ ] Asset register baseline in place
- [ ] IEC 62443 Zone & Conduit model documented
- [ ] Data quality baseline report

#### Phase 1 Exit Criteria

- [ ] ≥ [N] assets producing time-series data continuously
- [ ] Data latency ≤ [X] seconds for priority tags
- [ ] Data availability ≥ [X]% over 30-day period
- [ ] Security baseline assessment completed

---

### Phase 2: Contextualization

**Objective:** Transform raw data into contextualised, semantically-rich information aligned to ISA-95.

**Duration:** [Month X – Month Y]
**Budget:** [£/$ amount]
**Maturity Progression:** Level 2 → Level 3

#### Workstreams

| Workstream | Description | Owner | Duration | Dependencies |
|---|---|---|---|---|
| ISA-95 Hierarchy | Map all assets to ISA-95 equipment hierarchy | [Name] | [Weeks] | Asset register |
| Data Lakehouse | Deploy Bronze/Silver/Gold lakehouse architecture | [Name] | [Weeks] | Cloud tenant, data ingestion |
| Knowledge Graph | Build initial industrial knowledge graph | [Name] | [Weeks] | ISA-95 hierarchy, CMMS data |
| Data Governance | Implement data quality rules and lineage | [Name] | [Weeks] | Lakehouse |
| Master Data | ERP/CMMS/MES master data cleansing | [Name] | [Weeks] | System access |
| Digital Twin (Basic) | Asset digital twin pilot — [asset/line] | [Name] | [Weeks] | ISA-95 mapping |

#### Phase 2 Deliverables

- [ ] ISA-95 hierarchy documented and loaded into knowledge graph
- [ ] Data lakehouse (Bronze/Silver/Gold) operational
- [ ] [N] critical assets modelled in knowledge graph
- [ ] Data quality score ≥ [X]% for priority data domains
- [ ] Data lineage documented for critical data flows
- [ ] Initial digital twin pilot operational

#### Phase 2 Exit Criteria

- [ ] All priority assets mapped to ISA-95 hierarchy
- [ ] Silver-layer data quality ≥ [X]%
- [ ] Knowledge graph populated with ≥ [N] assets, [N] relationships
- [ ] Data governance policy approved and active

---

### Phase 3: Analytics & Prediction

**Objective:** Deploy predictive analytics and AI models to drive measurable operational improvement.

**Duration:** [Month X – Month Y]
**Budget:** [£/$ amount]
**Maturity Progression:** Level 3 → Level 4

#### Use Case Pipeline

| Priority | Use Case | Business Value | Data Readiness | Complexity | Recommended Start |
|---|---|---|---|---|---|
| 1 | [e.g., Pump Failure Prediction] | High | [Red/Amber/Green] | Medium | Month [X] |
| 2 | [e.g., Energy Optimisation] | High | | | |
| 3 | [e.g., Quality Anomaly Detection] | Medium | | | |
| 4 | [e.g., Production Scheduling AI] | Medium | | | |
| 5 | [e.g., Root Cause Analysis] | High | | | |

#### Workstreams

| Workstream | Description | Owner | Duration | Dependencies |
|---|---|---|---|---|
| MLOps Platform | Deploy model training, registry, and serving | [Name] | [Weeks] | Data lakehouse |
| Use Case 1 | [Description] | [Name] | [Weeks] | Data readiness |
| Use Case 2 | [Description] | [Name] | [Weeks] | Data readiness |
| Use Case 3 | [Description] | [Name] | [Weeks] | Data readiness |
| Model Monitoring | Drift detection and model performance tracking | [Name] | [Weeks] | Models deployed |
| Operator Dashboards | Operational AI dashboards | [Name] | [Weeks] | Model outputs |

#### Phase 3 Deliverables

- [ ] MLOps platform operational
- [ ] [N] predictive models in production
- [ ] Model performance dashboards live
- [ ] [KPI improvement 1] achieved
- [ ] [KPI improvement 2] achieved
- [ ] Operator training completed

#### Phase 3 Exit Criteria

- [ ] ≥ [N] models in production with monitored performance
- [ ] Model accuracy ≥ [X]% for priority use cases
- [ ] [KPI target] demonstrated over ≥ 90 days

---

### Phase 4: Agentic Operations

**Objective:** Deploy autonomous AI agents capable of closed-loop industrial operations with appropriate human oversight.

**Duration:** [Month X – Month Y]
**Budget:** [£/$ amount]
**Maturity Progression:** Level 4 → Level 5

#### Agent Pipeline

| Agent | Capability | Autonomy Level | HITL Requirement | Integration |
|---|---|---|---|---|
| [Maintenance Agent] | Autonomous work order creation | Supervised | Medium-risk approval | CMMS, Historian |
| [Production Agent] | Process parameter optimisation | Supervised | High-risk approval | MES, DCS |
| [Energy Agent] | Demand optimisation | Semi-autonomous | Notification only | Energy SCADA |
| [Quality Agent] | Defect detection + disposition | Supervised | Mandatory for reject | MES, Lab |
| [Security Agent] | OT anomaly detection + response | Supervised | Always | SIEM, Firewall |

#### Workstreams

| Workstream | Description | Owner | Duration | Dependencies |
|---|---|---|---|---|
| Agent Framework | Deploy agent orchestration platform | [Name] | [Weeks] | MLOps, Knowledge Graph |
| Agent 1 Pilot | [Agent name] — limited scope pilot | [Name] | [Weeks] | Tools integration |
| HITL Workflow | Human-in-the-loop approval system | [Name] | [Weeks] | Agent framework |
| Agent Security | Agent identity, audit, guardrails | [Name] | [Weeks] | Security baseline |
| Agent Rollout | Scale approved agents across sites | [Name] | [Weeks] | Pilot success |
| Change Management | Operator and supervisor training | [Name] | [Weeks] | Ongoing |

#### Phase 4 Deliverables

- [ ] Agent orchestration platform deployed
- [ ] [Agent 1] pilot completed with measurable outcome
- [ ] HITL approval workflow operational
- [ ] Agent audit log and security controls in place
- [ ] [N] agents in production across [N] sites
- [ ] Change management programme completed

---

## Programme Governance

### Steering Committee

| Role | Name | Organisation | Meeting Frequency |
|---|---|---|---|
| Executive Sponsor | [Name] | [Org] | Monthly |
| Programme Director | [Name] | [Org] | Bi-weekly |
| OT Architect | [Name] | [Org] | Bi-weekly |
| IT Architect | [Name] | [Org] | Bi-weekly |
| Operations Lead | [Name] | [Org] | Monthly |
| Security Lead | [Name] | [Org] | Monthly |
| System Integrator | [Name] | [Partner] | Bi-weekly |

### Decision Authority Matrix (RACI)

| Decision | Sponsor | Programme Mgr | Architect | Operations | IT/OT Security |
|---|---|---|---|---|---|
| Architecture decisions | A | R | R | C | C |
| Technology selection | A | C | R | I | C |
| Budget approval | A | R | I | I | I |
| Go/No-Go phase gates | A | R | C | C | C |
| Agent autonomy levels | A | C | C | R | C |
| Security waivers | A | I | I | I | R |

*R = Responsible, A = Accountable, C = Consulted, I = Informed*

### Phase Gate Criteria

| Gate | Phase Transition | Key Criteria |
|---|---|---|
| Gate 0 | Initiation → Phase 1 | Business case approved, budget allocated |
| Gate 1 | Phase 1 → Phase 2 | Data foundation exit criteria met |
| Gate 2 | Phase 2 → Phase 3 | Contextualization exit criteria met |
| Gate 3 | Phase 3 → Phase 4 | Analytics exit criteria met, safety review passed |
| Gate 4 | Phase 4 → BAU | All agents in production, ops team trained |

---

## Risk Register

| ID | Risk Description | Probability | Impact | Score | Mitigation | Owner |
|---|---|---|---|---|---|---|
| R01 | OT/IT stakeholder misalignment delays programme | Medium | High | High | Establish joint OT/IT governance from Day 1 | [Name] |
| R02 | Poor data quality prevents model performance | High | High | Critical | Data quality gate before Phase 3 | [Name] |
| R03 | OT security constraints limit connectivity | Medium | Medium | Medium | Early security assessment and design | [Name] |
| R04 | Vendor lock-in on key platform components | Low | High | Medium | Open standards (MQTT, OPC-UA, open APIs) | [Name] |
| R05 | Change resistance from operations teams | Medium | High | High | Change management from programme start | [Name] |
| R06 | Agent actions cause unintended consequences | Low | Critical | High | HITL controls, staged autonomy rollout | [Name] |
| R07 | [Custom Risk] | | | | | |

---

## Dependencies & Assumptions

### Key Dependencies

| Dependency | Description | Dependency Owner | Due Date |
|---|---|---|---|
| D01 | Network infrastructure upgrade (OT DMZ) | [Owner] | [Date] |
| D02 | Cloud tenant provisioning | [Owner] | [Date] |
| D03 | CMMS / ERP API access confirmed | [Owner] | [Date] |
| D04 | OT security assessment completed | [Owner] | [Date] |
| D05 | [Custom dependency] | | |

### Key Assumptions

- [ ] OT network connectivity can be established without major infrastructure investment
- [ ] Business stakeholders will dedicate [N] hours per week to programme activities
- [ ] Cloud egress costs are within approved budget envelope
- [ ] System integrator resource is available as planned
- [ ] [Custom assumption]

---

## Budget Summary

| Phase | Workstream | Internal Resource | External / Vendor | Technology | Total |
|---|---|---|---|---|---|
| Phase 1 | Data Foundation | [£] | [£] | [£] | [£] |
| Phase 2 | Contextualization | [£] | [£] | [£] | [£] |
| Phase 3 | Analytics | [£] | [£] | [£] | [£] |
| Phase 4 | Agentic Operations | [£] | [£] | [£] | [£] |
| Programme Mgmt | Governance / PMO | [£] | [£] | — | [£] |
| **Total** | | **[£]** | **[£]** | **[£]** | **[£]** |

### Technology Cost Categories (for reference)

| Category | Typical Range | Notes |
|---|---|---|
| Edge nodes (per site) | £5k–£20k | Depends on compute requirements |
| MQTT Broker (cloud) | £1k–£5k/month | HiveMQ, EMQX cloud, or Azure Event Grid |
| Data Lakehouse | £2k–£15k/month | Fabric, Databricks, or Snowflake |
| Knowledge Graph | £1k–£5k/month | Neo4j Aura, Azure Cosmos DB |
| MLOps Platform | £2k–£8k/month | Azure ML, Databricks, or open source |
| Agent Framework | £3k–£10k/month | Azure AI Foundry, LangGraph cloud |

---

## Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 0.1 | [Date] | [Name] | Initial draft |
| 0.2 | [Date] | [Name] | Stakeholder review |
| 1.0 | [Date] | [Name] | Approved for execution |

---

*Template version: 1.0 | Based on the Industrial AI Data Backbone Reference Architecture*
*For framework guidance, see [/docs/industrial-ai-maturity-model.md](/docs/industrial-ai-maturity-model.md)*
