# Industrial AI Readiness Assessment Template

> **Purpose:** Rapidly assess organisational and technical readiness to deploy Industrial AI. Identify blockers before committing to implementation.
>
> **Completion time:** 2–4 hours with cross-functional team (OT, IT, Operations, Leadership)
>
> **Output:** Readiness Score, RAG status per dimension, and a prioritised action plan

---

## Assessment Details

| Field | Value |
|---|---|
| **Organisation** | [Company Name] |
| **Assessment Scope** | [Site / Division / Enterprise] |
| **Use Case(s) Being Evaluated** | [e.g., Predictive Maintenance — Rotating Equipment] |
| **Assessment Date** | [DD/MM/YYYY] |
| **Facilitated By** | [Name, Title] |
| **Attendees** | [Names and Roles] |

---

## Part A: Business Readiness

### A1. Strategic Alignment

| Question | Response | Score (1–5) |
|---|---|---|
| Is there a named Executive Sponsor with budget authority for Industrial AI? | Yes / Partial / No | |
| Is Industrial AI explicitly referenced in the corporate or operational strategy? | Yes / Partial / No | |
| Are the target business outcomes and KPIs for Industrial AI defined? | Yes / Partial / No | |
| Has a business case been prepared with quantified ROI estimates? | Yes / Partial / No | |
| Is there board-level awareness and support for the programme? | Yes / Partial / No | |

**Section Score:** [Sum / 25]

**Notes / Observations:**
> *[Free text — record key observations, gaps, blockers]*

---

### A2. Organisational Capability

| Question | Response | Score (1–5) |
|---|---|---|
| Does the organisation have in-house OT/IT integration expertise? | Yes / Partial / No | |
| Is there a dedicated data engineering or data platform team? | Yes / Partial / No | |
| Are data scientists or AI/ML engineers available (in-house or contracted)? | Yes / Partial / No | |
| Is there an OT security specialist or team? | Yes / Partial / No | |
| Has the organisation previously delivered a digital / OT-IT integration project? | Yes / Partial / No | |
| Is there a change management capability to support operational adoption? | Yes / Partial / No | |

**Section Score:** [Sum / 30]

**Notes / Observations:**
> *[Free text]*

---

### A3. Operations Engagement

| Question | Response | Score (1–5) |
|---|---|---|
| Are operations leaders actively engaged and supportive of Industrial AI? | Yes / Partial / No | |
| Have front-line operators and maintenance technicians been consulted on use cases? | Yes / Partial / No | |
| Is there a history of technology adoption on the plant floor? | Yes / Partial / No | |
| Are there champions / super-users identified in operations? | Yes / Partial / No | |
| Is there a tolerance for AI-assisted (not fully autonomous) recommendations initially? | Yes / Partial / No | |

**Section Score:** [Sum / 25]

**Notes / Observations:**
> *[Free text]*

---

## Part B: Data Readiness

### B1. Data Availability

*For the target use case(s), assess data availability:*

| Data Source | Available? | Quality (1–5) | Accessible? | Latency | Notes |
|---|---|---|---|---|---|
| [e.g., Vibration sensors — Pump A] | Yes/No/Partial | | Yes/No | Real-time/Batch | |
| [e.g., Temperature — Bearing] | | | | | |
| [e.g., Historian — process data] | | | | | |
| [e.g., CMMS — maintenance history] | | | | | |
| [e.g., ERP — production schedule] | | | | | |
| [e.g., Lab / LIMS] | | | | | |
| [Custom source] | | | | | |

**Data Availability Score:** [Average of quality scores across required sources / 5]

---

### B2. Data History & Volume

| Question | Response | Score (1–5) |
|---|---|---|
| Is there ≥ 12 months of historical operational data for the target use case? | Yes / Partial / No | |
| Does historical data include labelled failure events or process outcomes? | Yes / Partial / No | |
| Is the data volume sufficient for model training (rule of thumb: ≥ 1,000 relevant events for supervised models)? | Yes / Partial / No | |
| Are data timestamps accurate and aligned across sources? | Yes / Partial / No | |
| Is historical data archived and retrievable, or lost/inaccessible? | Yes / Partial / No | |

**Section Score:** [Sum / 25]

---

### B3. Data Quality

| Dimension | Assessment | Score (1–5) |
|---|---|---|
| **Completeness** — What % of expected data points are present? | [%] | |
| **Accuracy** — Is data representative of actual physical reality? | [Assessment] | |
| **Consistency** — Is data consistent across systems / historians? | [Assessment] | |
| **Timeliness** — Is data delivered within acceptable latency? | [Assessment] | |
| **Uniqueness** — Are there duplicated tags or conflicting values? | [Assessment] | |

**Section Score:** [Sum / 25]

---

### B4. Data Contextualization Readiness

| Question | Response | Score (1–5) |
|---|---|---|
| Is there a documented asset hierarchy for the target area? | Yes / Partial / No | |
| Are tag names structured and meaningful (not just raw PLC addresses)? | Yes / Partial / No | |
| Is there a mapping between tags and physical equipment? | Yes / Partial / No | |
| Is ISA-95 or a consistent naming convention applied? | Yes / Partial / No | |
| Is there an industrial knowledge graph or equipment ontology? | Yes / Partial / No | |

**Section Score:** [Sum / 25]

---

## Part C: Technology Readiness

### C1. OT Connectivity

| Question | Response | Score (1–5) |
|---|---|---|
| Can data be extracted from target assets (OPC-UA, MQTT, or native protocol)? | Yes / Partial / No | |
| Is real-time data accessible (not just batch exports)? | Yes / Partial / No | |
| Is there an MQTT broker or Unified Namespace infrastructure? | Yes / Partial / No | |
| Are edge compute nodes in place or planned? | Yes / Partial / No | |
| Are there OT network constraints that would prevent data flow? | Yes / Partial / No | |

**Section Score:** [Sum / 25]

---

### C2. Cloud / Platform Infrastructure

| Question | Response | Score (1–5) |
|---|---|---|
| Is there an approved cloud provider / data platform for this programme? | Yes / Partial / No | |
| Is a data lakehouse or equivalent platform in place? | Yes / Partial / No | |
| Is there sufficient compute for model training (GPU / large CPU)? | Yes / Partial / No | |
| Are MLOps tools in place (experiment tracking, model registry, serving)? | Yes / Partial / No | |
| Is there a vector store or knowledge graph for AI retrieval? | Yes / Partial / No | |

**Section Score:** [Sum / 25]

---

### C3. OT Security Posture

| Question | Response | Score (1–5) |
|---|---|---|
| Is OT network segmentation (IT/OT separation) in place? | Yes / Partial / No | |
| Has an IEC 62443 or equivalent security assessment been completed? | Yes / Partial / No | |
| Is there a formal approval process for new IT/OT integrations? | Yes / Partial / No | |
| Is remote access to OT systems controlled and monitored? | Yes / Partial / No | |
| Are there documented security requirements for data platform connections? | Yes / Partial / No | |

**Section Score:** [Sum / 25]

---

## Part D: Use Case Readiness

*Complete this section for each target use case.*

### Use Case: [Name — e.g., Pump Failure Prediction]

| Question | Response | Score (1–5) |
|---|---|---|
| Is the problem statement clearly defined and agreed? | Yes / Partial / No | |
| Is the expected business outcome quantified (e.g., £X cost avoided)? | Yes / Partial / No | |
| Is there a subject matter expert available (e.g., reliability engineer)? | Yes / Partial / No | |
| Are the input data requirements identified and available? | Yes / Partial / No | |
| Is there labelled training data (historical failures, outcomes)? | Yes / Partial / No | |
| Is a success metric (accuracy / F1 / ROC-AUC) defined and agreed? | Yes / Partial / No | |
| Is there a clear integration path for model outputs? (dashboard / alert / API) | Yes / Partial / No | |
| Is there a process for operators to act on AI recommendations? | Yes / Partial / No | |
| Is there an agreed approach to model validation and sign-off? | Yes / Partial / No | |
| Is the risk of incorrect predictions understood and acceptable? | Yes / Partial / No | |

**Use Case Readiness Score:** [Sum / 50]

---

## Readiness Summary Dashboard

### Dimension Scores

| Dimension | Max | Actual | % | RAG |
|---|---|---|---|---|
| **A1** Strategic Alignment | 25 | | | 🔴/🟡/🟢 |
| **A2** Organisational Capability | 30 | | | |
| **A3** Operations Engagement | 25 | | | |
| **B1** Data Availability | 25 | | | |
| **B2** Data History & Volume | 25 | | | |
| **B3** Data Quality | 25 | | | |
| **B4** Data Contextualization | 25 | | | |
| **C1** OT Connectivity | 25 | | | |
| **C2** Cloud / Platform | 25 | | | |
| **C3** OT Security | 25 | | | |
| **D** Use Case Readiness | 50 | | | |
| **Total** | **305** | | | |

*RAG: 🔴 < 40% | 🟡 40–69% | 🟢 ≥ 70%*

---

### Overall Readiness Verdict

| Score Range | Verdict | Recommended Action |
|---|---|---|
| **≥ 75%** | ✅ Ready to Proceed | Initiate programme with confidence |
| **55–74%** | 🟡 Proceed with Conditions | Address critical gaps before Phase 1 Gate |
| **35–54%** | 🟠 Foundational Work Required | 3–6 month foundation sprint before AI programme |
| **< 35%** | 🔴 Not Ready | 6–12 month prerequisite programme required |

**Overall Score:** [Total / 305] = **[%]** — **[Verdict]**

---

## Critical Blockers

> *List any issues that would prevent programme success regardless of overall score. These are hard blockers that must be resolved before proceeding.*

| # | Blocker | Dimension | Resolution Required | Owner | By When |
|---|---|---|---|---|---|
| 1 | [e.g., No OT network connectivity to target assets] | C1 | Network infrastructure project | [Name] | [Date] |
| 2 | | | | | |
| 3 | | | | | |

---

## Prioritised Pre-Requisites

| Priority | Action | Dimension | Effort | Owner | Target Date |
|---|---|---|---|---|---|
| 1 | [e.g., Appoint Executive Sponsor and define KPIs] | A1 | Low | [Name] | |
| 2 | [e.g., Complete IEC 62443 security assessment] | C3 | Medium | [Name] | |
| 3 | [e.g., Establish MQTT broker and test OT connectivity] | C1 | Medium | [Name] | |
| 4 | [e.g., Extract and profile 12 months historical data] | B2 | Low | [Name] | |
| 5 | [e.g., Map asset hierarchy for target area] | B4 | Low | [Name] | |
| 6 | | | | | |
| 7 | | | | | |

---

## Recommended Next Steps

Based on this assessment, the recommended next steps are:

1. **[Immediate — Week 1–2]:** [e.g., Convene steering committee and confirm Executive Sponsor]
2. **[Short-term — Month 1]:** [e.g., Complete OT security assessment and network review]
3. **[Short-term — Month 1–2]:** [e.g., Extract and profile historical data for target use case]
4. **[Medium-term — Month 2–3]:** [e.g., Deploy MQTT broker and validate OT connectivity]
5. **[Milestone]:** Target re-assessment date: [DD/MM/YYYY]

---

## Assessor Notes

> *[Free text for additional observations, caveats, or context not captured in the structured questions above.]*

---

## Sign-off

| Role | Name | Date |
|---|---|---|
| Assessment Lead | | |
| Operations Lead | | |
| OT Architect | | |
| IT Architect | | |
| Executive Sponsor | | |

---

*Template version: 1.0 | Industrial AI Data Backbone Reference Architecture*
*For use case guidance, see [/examples/](/examples/) | For maturity model, see [/docs/industrial-ai-maturity-model.md](/docs/industrial-ai-maturity-model.md)*
