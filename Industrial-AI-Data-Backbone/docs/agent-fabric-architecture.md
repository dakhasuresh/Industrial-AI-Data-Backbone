# Agent Fabric Architecture

> *Based on architectural principles by **Suresh Dakha** ([@dakhasuresh](https://github.com/dakhasuresh)), HCLTech — ISA/IEC 62443 Expert, ISA Senior Member.*

## Introduction

The Agent Fabric is the multi-agent AI layer of the Industrial Data Backbone — a structured, governed framework for deploying autonomous AI agents in industrial environments. It defines how agents are organized, how they communicate, how they are supervised, and how they safely interact with operational systems.

The term "fabric" is intentional. Like a physical fabric, the Agent Fabric is a woven structure of individual agents, each with a specific purpose, that together create a unified intelligence layer capable of acting across the full operational envelope of an industrial enterprise.

---

## Why Multi-Agent Architecture?

Industrial operations are inherently multi-domain. A refinery does not have one problem — it has simultaneous challenges in production optimization, equipment health, energy consumption, quality assurance, and safety. No single AI model can effectively reason across all these domains at once.

The multi-agent approach provides:

- **Domain specialization** — each agent is purpose-built for a specific operational domain
- **Parallel reasoning** — multiple agents can work on different problems simultaneously
- **Composable intelligence** — agents can collaborate on complex, cross-domain problems
- **Bounded autonomy** — each agent operates within defined, auditable boundaries
- **Graceful degradation** — failure of one agent does not compromise the entire AI layer

---

## Agent Fabric Architecture Diagram

```mermaid
graph TB
    subgraph TRIGGERS["Triggers & Inputs"]
        SENSOR_EVENTS["Sensor Events\n(UNS Stream)"]
        SCHEDULES["Scheduled Tasks\n(Cron / Calendar)"]
        USER_REQ["Human Requests\n(Natural Language)"]
        EXT_EVENTS["External Events\n(ERP, Market)"]
    end

    subgraph ORCHESTRATOR["Agent Orchestrator"]
        ROUTER["Request Router\n(Intent Classification)"]
        PLANNER["Task Planner\n(Multi-step decomposition)"]
        COORD["Agent Coordinator\n(Cross-agent collaboration)"]
        AUDIT["Audit & Compliance Log"]
        HITL["Human-in-the-Loop\nApproval Gate"]
    end

    subgraph AGENTS["Domain Agents"]
        MAINT_A["🔧 Maintenance Agent"]
        PROD_A["🏭 Production Agent"]
        QUAL_A["🔬 Quality Agent"]
        ENERGY_A["⚡ Energy Agent"]
        SEC_A["🔒 OT Security Agent"]
        SAFETY_A["⚠️ Safety Agent"]
    end

    subgraph TOOLS["Agent Tools & Capabilities"]
        KG_TOOL["Knowledge Graph\nQuery"]
        TS_TOOL["Time-Series\nQuery"]
        ML_TOOL["ML Model\nInference"]
        CMMS_TOOL["CMMS / ERP\nRead/Write"]
        SCADA_TOOL["SCADA / MES\nRead (Write: Supervised)"]
        DOCS_TOOL["Document & Manual\nRAG Search"]
        NOTIF_TOOL["Notification &\nAlerting"]
    end

    subgraph CONTEXT["Shared Context Layer"]
        KG["Industrial\nKnowledge Graph"]
        MEMSHORT["Short-term Memory\n(Session context)"]
        MEMLONG["Long-term Memory\n(Agent history)"]
        SHARED_STATE["Shared Operational\nState"]
    end

    TRIGGERS --> ORCHESTRATOR
    ORCHESTRATOR --> AGENTS
    AGENTS <--> TOOLS
    TOOLS <--> CONTEXT
    HITL -.->|Approval required| AGENTS
    AUDIT -.->|Logs all actions| AGENTS
```

---

## Agent Definitions

### Maintenance Agent

**Purpose:** Predict, plan, and coordinate maintenance activities to maximize asset reliability and minimize unplanned downtime.

```mermaid
graph LR
    subgraph INPUTS["Inputs"]
        SENSOR["Sensor anomalies\nfrom UNS"]
        PDM["PdM model\nalerts"]
        CMMS_DATA["Work order history\nfrom CMMS"]
        KG_DATA["Failure modes\nfrom Knowledge Graph"]
    end

    subgraph REASONING["Agent Reasoning (LLM + Tools)"]
        ASSESS["Assess failure\nprobability & impact"]
        PRIORITIZE["Prioritize against\nactive work orders"]
        PLAN["Generate maintenance\nplan & BOM"]
        RISK["Calculate risk of\ndeferred maintenance"]
    end

    subgraph ACTIONS["Actions"]
        CREATE_WO["Create/update\nwork order in CMMS"]
        RESERVE_PARTS["Reserve parts\nin inventory"]
        NOTIFY["Notify planner\n& technician"]
        APPROVE["Request human\napproval (if needed)"]
    end

    INPUTS --> REASONING --> ACTIONS
```

**Autonomy level:** 5b — agent creates work orders automatically; human confirms within 4 hours before scheduling execution.

**Key tools:**
- `query_knowledge_graph(asset_id)` — retrieve failure mode history
- `get_pdm_prediction(asset_id)` — retrieve current RUL and failure probability
- `create_work_order(cmms_payload)` — write to CMMS
- `check_parts_availability(part_numbers)` — check stock
- `get_technician_schedule(craft, site)` — check resource availability

---

### Production Agent

**Purpose:** Optimize production schedules, respond to unplanned events, and maximize throughput and on-time delivery.

**Trigger conditions:**
- Unplanned downtime > 15 minutes
- OEE deviation > 5% from plan
- Demand signal change from ERP
- Quality hold impacting WIP

**Autonomy level:** 5a/5b — recommendations auto-approved for schedule adjustments within ±4 hours; customer commitment changes require human approval.

---

### Quality Agent

**Purpose:** Monitor process conditions in real time to predict and prevent quality defects, and trigger corrective actions before non-conforming product is produced.

**Key capabilities:**
- Real-time SPC monitoring against control limits
- Multivariate process analysis (detect combinations of parameters that lead to defects)
- Recipe deviation detection
- Product hold recommendation with evidence chain

**Autonomy level:** 5b — product holds require supervisor approval within 30 minutes; recipe micro-adjustments within pre-approved ranges are autonomous.

---

### Energy Agent

**Purpose:** Minimize energy cost and consumption while respecting production constraints and regulatory requirements.

**Key capabilities:**
- Real-time energy demand monitoring by asset and area
- Demand response event management
- Load shifting optimization
- Peak demand prediction and pre-emptive action
- Carbon intensity optimization

**Autonomy level:** 5c — pre-approved load shedding actions (non-critical assets) are fully autonomous; grid-level demand response actions require senior approval.

---

### OT Security Agent

**Purpose:** Continuously monitor OT network behavior, detect anomalies, and recommend or take protective actions to prevent cyber incidents in industrial environments.

**Key capabilities:**
- Passive OT network traffic analysis (Purdue Model zones)
- Asset discovery and inventory reconciliation
- Anomaly detection (new assets, unexpected communications, protocol violations)
- Vulnerability identification (unpatched firmware, default credentials)
- Incident response playbook execution

**Autonomy level:** 5a/5b — recommendations for isolation of suspected compromised assets require SOC approval; logging and alerting is fully autonomous.

> **Important:** The OT Security Agent must never be configured with write access to control systems. Its intervention actions are limited to network segmentation and alerting. Physical process control remains with OT systems and human operators.

---

## Agent Orchestrator Design

The Agent Orchestrator is the central coordination component of the Agent Fabric. It is responsible for:

1. **Intent classification** — determining which agent(s) should handle a request or event
2. **Task decomposition** — breaking complex, cross-domain tasks into agent-specific sub-tasks
3. **Cross-agent coordination** — managing information sharing between collaborating agents
4. **Human-in-the-loop gates** — enforcing approval requirements before high-impact actions
5. **Audit logging** — maintaining an immutable record of all agent reasoning, decisions, and actions

### Orchestrator Workflow

```mermaid
sequenceDiagram
    participant EVENT as Event/Request
    participant ORCH as Orchestrator
    participant MAINT as Maintenance Agent
    participant PROD as Production Agent
    participant HUMAN as Human Approver
    participant CMMS as CMMS System

    EVENT->>ORCH: PdM alert: Compressor C-201, 87% failure probability, 5 days
    ORCH->>ORCH: Intent classification: Maintenance + Production impact
    
    par Parallel agent activation
        ORCH->>MAINT: Assess Compressor C-201, generate maintenance plan
        ORCH->>PROD: Assess production impact of C-201 outage
    end

    MAINT->>MAINT: Query KG: failure modes, history, parts
    MAINT->>MAINT: Query CMMS: available technicians, upcoming windows
    MAINT->>ORCH: Plan: 8hr PM window, Wed 06:00, parts available, total cost £4,200

    PROD->>PROD: Assess: C-201 feeds Reactor Train A (critical)
    PROD->>PROD: Check: Can production be maintained via Train B during outage?
    PROD->>ORCH: Impact: 4hr partial rate reduction, recoverable

    ORCH->>ORCH: Compile cross-agent recommendation
    ORCH->>HUMAN: Approval request: C-201 maintenance plan [Wed 06:00, 8hrs, £4,200]
    HUMAN->>ORCH: Approved
    ORCH->>CMMS: Create confirmed work order WO-2024-1142
    ORCH->>AUDIT: Log: all reasoning, approvals, actions taken
```

---

## Human-in-the-Loop Framework

Industrial AI agents must operate within a clear human oversight model. The following framework defines when human approval is required:

### Approval Matrix

| Action Category | Risk Level | Approval Requirement | Approval Window |
|----------------|-----------|---------------------|----------------|
| Read-only analysis and recommendations | None | No approval needed | N/A |
| Work order creation (planned maintenance) | Low | Planner notification; auto-approved after 4 hours | 4 hours |
| Work order creation (emergency) | Medium | Supervisor approval required | 30 minutes |
| Recipe parameter adjustment (within range) | Low | Operator notification; auto-approved after 15 min | 15 minutes |
| Product hold recommendation | Medium | Quality Manager approval | 30 minutes |
| Emergency shutdown recommendation | Critical | Shift Manager approval; operator executes | 5 minutes |
| Network isolation (OT security) | High | SOC Lead approval | 15 minutes |
| Capital expenditure > £10,000 | High | Engineering Manager approval | 24 hours |

### Human-in-the-Loop Interface

Human approvers interact with agent recommendations through:
- **Operations dashboard** — real-time recommendation queue with evidence summaries
- **Mobile notifications** — push alerts for time-sensitive approvals
- **Natural language interface** — "Tell me more" / "Approve" / "Defer" / "Reject + reason"
- **Audit trail** — every decision recorded with approver identity and timestamp

---

## Edge Agents vs Cloud Agents

The Agent Fabric operates at two levels, each with distinct characteristics:

### Edge Agents

| Characteristic | Description |
|---------------|-------------|
| Location | Deployed on industrial edge compute nodes (per plant / area) |
| Latency | < 100ms response for real-time control-adjacent decisions |
| Connectivity | Operate during cloud connectivity loss (store-and-forward) |
| Scope | Local asset group or production area |
| Examples | Local anomaly response, edge alarm management, local energy control |

### Cloud Agents

| Characteristic | Description |
|---------------|-------------|
| Location | Deployed on cloud AI platform (Azure, AWS, GCP) |
| Latency | 1–30 seconds acceptable for planning and optimization |
| Connectivity | Always-on; enterprise-wide visibility |
| Scope | Cross-site, fleet-wide, enterprise |
| Examples | Cross-site production optimization, enterprise maintenance scheduling, carbon accounting |

### Edge-Cloud Agent Coordination

```mermaid
graph TB
    subgraph EDGE["Edge Layer"]
        EDGE_A1["Edge Agent A\n(Plant Area 1)"]
        EDGE_A2["Edge Agent B\n(Plant Area 2)"]
        LOCAL_CONTEXT["Local Context\n(Edge KG + Cache)"]
    end

    subgraph CLOUD["Cloud Layer"]
        CLOUD_AGENT["Cloud Orchestrator\nAgent"]
        FLEET_KG["Fleet Knowledge\nGraph"]
        ENTERPRISE["Enterprise AI\nPlatform"]
    end

    EDGE_A1 <-->|"Events & Actions\n(MQTT / API)"| CLOUD_AGENT
    EDGE_A2 <-->|"Events & Actions\n(MQTT / API)"| CLOUD_AGENT
    CLOUD_AGENT <--> FLEET_KG
    CLOUD_AGENT <--> ENTERPRISE
    EDGE_A1 <--> LOCAL_CONTEXT
    EDGE_A2 <--> LOCAL_CONTEXT
    LOCAL_CONTEXT -.->|"Sync (hourly)"| FLEET_KG
```

---

## Agent Security Architecture

Agents in industrial environments require a rigorous security model:

| Control | Description |
|---------|-------------|
| Least privilege | Each agent has API access only to the systems it needs |
| Action sandboxing | Agents operate in a validated action space — no arbitrary code execution |
| Action signing | All agent-initiated system writes are cryptographically signed |
| Immutable audit trail | All agent reasoning and actions logged to tamper-evident store |
| Human kill switch | Any agent can be suspended instantly by authorized personnel |
| Adversarial input detection | Agent inputs monitored for prompt injection or data poisoning |
| Separate agent identity | Each agent has its own service identity (not shared credentials) |

---

## Agent Development Lifecycle

```mermaid
graph LR
    DESIGN["1. Design\nDefine purpose,\nboundaries, tools"]
    BUILD["2. Build\nImplement agent logic,\ntool integrations"]
    TEST["3. Test\nShadow mode,\nback-testing"]
    VALIDATE["4. Validate\nHuman review of\nagent decisions"]
    DEPLOY["5. Deploy\nPhased rollout\n(assisted → supervised → autonomous)"]
    MONITOR["6. Monitor\nDecision quality,\ndrift detection"]

    DESIGN --> BUILD --> TEST --> VALIDATE --> DEPLOY --> MONITOR --> DESIGN
```

**Shadow mode:** The agent runs in parallel with human decisions for 30–90 days, its recommendations logged and compared to actual human decisions. Agreement rate and accuracy are measured before live deployment.

---

## Agent Fabric Technology Stack

| Component | Recommended Technology | Purpose |
|-----------|----------------------|---------|
| Agent runtime | LangChain / LlamaIndex / AutoGen | Agent reasoning framework |
| LLM | Azure OpenAI (GPT-4o) | Language reasoning |
| Orchestration | Azure AI Agents / CrewAI | Multi-agent coordination |
| Short-term memory | Redis | Session and working memory |
| Long-term memory | PostgreSQL + pgvector | Agent history and learned patterns |
| Knowledge graph | Neo4j | Industrial KG access |
| Tool APIs | FastAPI + OpenAPI | Standardized tool interfaces |
| Audit log | Azure Monitor / Elasticsearch | Immutable action logging |
| Approval workflow | ServiceNow / Power Automate | Human-in-the-loop UI |

---

## Related Documents

- [Industrial Knowledge Graph](industrial-knowledge-graph.md)
- [Industrial AI Maturity Model](industrial-ai-maturity-model.md)
- [Industrial AI Reference Architecture](industrial-ai-reference-architecture.md)
- [IEC 62443 Security Reference](iec62443-security-reference.md)
