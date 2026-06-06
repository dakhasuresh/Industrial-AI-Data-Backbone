# ISA-95 Contextualization Model

> *Based on architectural principles by **Suresh Dakha** ([@dakhasuresh](https://github.com/dakhasuresh)), HCLTech — ISA/IEC 62443 Expert, ISA Senior Member.*

## Introduction to ISA-95

ISA-95 (also published as IEC 62264) is the international standard for integrating enterprise and control systems. For Industrial AI, ISA-95 provides the canonical framework for contextualizing operational data by establishing a common language and hierarchy that bridges OT and IT systems.

The value of ISA-95 in Industrial AI is not compliance — it is **context**. When every data point is mapped to a standardized hierarchy, AI models gain the spatial, temporal, and process context they need to produce actionable, explainable, and reliable outputs.

---

## ISA-95 Functional Hierarchy

```mermaid
graph TB
    L4["Level 4 — Business Planning & Logistics\nERP, Business Intelligence, Corporate Systems\nTime horizon: months to years"]
    L3["Level 3 — Manufacturing Operations Management\nMES, LIMS, WMS, Quality Systems\nTime horizon: days to shifts"]
    L2["Level 2 — Supervisory Control\nSCADA, DCS, HMI\nTime horizon: minutes to hours"]
    L1["Level 1 — Basic Control\nPLCs, RTUs, Servo Drives\nTime horizon: seconds"]
    L0["Level 0 — Physical Process\nSensors, Actuators, Field Instruments\nTime horizon: milliseconds"]

    L4 --> L3 --> L2 --> L1 --> L0

    style L4 fill:#1a5276,color:#fff
    style L3 fill:#1a6336,color:#fff
    style L2 fill:#4a235a,color:#fff
    style L1 fill:#784212,color:#fff
    style L0 fill:#212f3c,color:#fff
```

---

## Equipment Hierarchy Model

The ISA-95 equipment hierarchy defines how physical assets are organized for data contextualization purposes. Every sensor reading, alarm, or event must be traceable to a node in this hierarchy.

```mermaid
graph TB
    ENT["🏢 Enterprise\n(Acme Corporation)"]
    SITE1["🏭 Site\n(Sheffield UK Plant)"]
    SITE2["🏭 Site\n(Munich DE Plant)"]
    
    AREA1["⚙️ Area\n(Furnace Area)"]
    AREA2["⚙️ Area\n(Rolling Area)"]
    AREA3["⚙️ Area\n(Quality Lab)"]

    LINE1["🔄 Production Unit\n(Rolling Line 3)"]
    LINE2["🔄 Production Unit\n(Rolling Line 4)"]

    UNIT1["🔧 Work Unit\n(Furnace 01)"]
    UNIT2["🔧 Work Unit\n(Mill Stand A)"]
    UNIT3["🔧 Work Unit\n(Coiler 01)"]

    TAG1["📊 Control Module\n(Temperature Zone A)"]
    TAG2["📊 Control Module\n(Speed Drive)"]

    ENT --> SITE1
    ENT --> SITE2
    SITE1 --> AREA1
    SITE1 --> AREA2
    SITE1 --> AREA3
    AREA2 --> LINE1
    AREA2 --> LINE2
    LINE1 --> UNIT1
    LINE1 --> UNIT2
    LINE1 --> UNIT3
    UNIT1 --> TAG1
    UNIT2 --> TAG2
```

### Equipment Hierarchy Definitions

| Level | ISA-95 Name | Description | Examples |
|-------|------------|-------------|---------|
| Enterprise | Enterprise | The top-level legal and operational entity | `Acme Corporation` |
| Site | Site | A geographic location | `Sheffield UK`, `Munich DE` |
| Area | Area | A functional or physical zone within a site | `Furnace Area`, `Packaging Area` |
| Production Unit | Production Unit | A group of equipment with a common function | `Rolling Line 3`, `Reactor Train A` |
| Work Unit | Work Unit | An individual piece of equipment | `Furnace 01`, `Pump P-101` |
| Control Module | Control Module | A logical control element | `Temperature Zone A`, `Flow Loop PIC-101` |

---

## Operations Hierarchy Model

Alongside the equipment hierarchy, ISA-95 defines the operations hierarchy — the context of *what is being produced* and *how*.

```mermaid
graph TB
    PROD_SCHED["Production Schedule\n(Planned Production Plan)"]
    PROD_REQ["Production Request\n(Work Order)"]
    PROD_RESP["Production Response\n(Actual Production Result)"]
    
    BATCH["Batch\n(WO-2024-0312-001)"]
    SEGMENT["Production Segment\n(Melting → Casting → Rolling)"]
    STEP["Segment Response\n(Actual Step Data)"]

    PROD_SCHED --> PROD_REQ --> BATCH --> SEGMENT --> STEP
    BATCH --> PROD_RESP
```

### Combining Equipment + Operations Context

The power of ISA-95 contextualization comes from combining both hierarchies:

```
{equipment_path} + {operation_context} = Fully Contextualized Data Point

Example:
  Equipment:    Acme/Sheffield/FurnaceArea/Line3/Furnace01/Temperature_Zone_A
  Operations:   WorkOrder: WO-2024-0312, Product: Grade-A-Steel, Phase: Heat-Soak
  Shift:        Day Shift, Operator: J.Smith
  Result:       Temperature_Zone_A = 284.6°C @ 2024-03-12 14:22:01
                → During Grade-A-Steel production, Soak phase, Day Shift
```

---

## ISA-95 Data Models

### Part 2: Object Model Attributes

ISA-95 Part 2 defines the attribute model for equipment objects — the metadata that must be maintained for each node in the hierarchy.

**Equipment Entity Attributes:**

```json
{
  "equipment_id": "FURNACE-01",
  "description": "Rotary Hearth Furnace 1",
  "equipment_class": ["Furnace", "HeatTreatment"],
  "location": {
    "enterprise": "Acme_Corp",
    "site": "Sheffield_UK",
    "area": "Furnace_Area",
    "production_unit": "Rolling_Line_3"
  },
  "properties": {
    "manufacturer": "SMS Group",
    "model": "RHF-500",
    "install_date": "2018-06-15",
    "design_capacity": "500 t/day",
    "fuel_type": "Natural Gas",
    "criticality": "Critical",
    "maintenance_strategy": "PdM"
  },
  "capabilities": [
    { "capability": "MaxTemperature", "value": 1350, "unit": "°C" },
    { "capability": "MaxThroughput", "value": 500, "unit": "t/day" }
  ]
}
```

---

## Tag-to-ISA-95 Mapping Process

One of the most important — and most commonly neglected — activities in building an Industrial Data Backbone is **tag mapping**: the process of linking every historian tag, OPC-UA node, or MQTT topic to a node in the ISA-95 hierarchy.

### Tag Mapping Workflow

```mermaid
graph LR
    EXTRACT["1. Extract\nTag List from\nHistorian / DCS"]
    MATCH["2. Auto-Match\nRule-based matching\nto asset hierarchy"]
    REVIEW["3. Human Review\nEngineer validates\nunmatched tags"]
    ENRICH["4. Enrich\nAdd parameter type,\nunit, ranges"]
    PUBLISH["5. Publish\nRegister in\nData Catalog"]
    GOVERN["6. Govern\nOngoing change\nmanagement"]

    EXTRACT --> MATCH --> REVIEW --> ENRICH --> PUBLISH --> GOVERN
```

### Tag Mapping Data Structure

```json
{
  "source_tag": "TI_FUR01_ZA",
  "source_system": "OSIsoft PI",
  "isa95_path": "Acme_Corp/Sheffield_UK/Furnace_Area/Rolling_Line_3/Furnace_01",
  "control_module": "Temperature_Zone_A",
  "parameter_type": "ProcessVariable",
  "parameter_category": "Temperature",
  "unit_of_measure": "°C",
  "engineering_low": 0.0,
  "engineering_high": 1400.0,
  "normal_low": 270.0,
  "normal_high": 295.0,
  "alarm_low": 250.0,
  "alarm_high": 310.0,
  "sample_rate_seconds": 1,
  "data_type": "Float",
  "is_ai_relevant": true,
  "ai_use_cases": ["PredictiveMaintenance", "EnergyOptimization", "ProcessOptimization"]
}
```

---

## ISA-95 and the Unified Namespace

The ISA-95 hierarchy directly defines the UNS topic taxonomy. This alignment ensures that every message in the UNS carries its full operational context in the topic path itself.

```mermaid
graph LR
    ISA95["ISA-95 Hierarchy\nEnterprise/Site/Area/Line/Unit/Parameter"]
    UNS["UNS Topic\nspBv1.0/Enterprise/Site/Area/Line/Device/Tag"]
    CATALOG["Data Catalog Entry\n{equipment_id, parameter, unit, ranges, AI use cases}"]

    ISA95 -->|"Direct mapping\n(same hierarchy)"| UNS
    ISA95 -->|"Asset attributes"| CATALOG
    UNS -->|"Topic = key"| CATALOG
```

---

## ISA-95 Parts Relevant to Industrial AI

| Part | Title | Relevance to AI |
|------|-------|----------------|
| Part 1 | Models and Terminology | Equipment hierarchy — the backbone of all contextualization |
| Part 2 | Object Model Attributes | Asset attribute model — feeds the Digital Twin |
| Part 3 | Activity Models | Process activity context for production AI |
| Part 4 | Object & Attributes of Manufacturing Operations | MES integration for production and quality AI |
| Part 5 | Business-to-Manufacturing Transactions | ERP/MES transaction patterns for scheduling AI |

---

## ISA-95 Implementation Checklist

### Equipment Hierarchy

- [ ] Enterprise and site structure defined
- [ ] All production areas defined and named consistently
- [ ] All production units mapped to areas
- [ ] All work units (equipment) mapped to production units
- [ ] Equipment attributes populated (manufacturer, model, criticality)
- [ ] Hierarchy stored in CMMS / EAM as system of record

### Tag Mapping

- [ ] Complete tag inventory extracted from all historians and DCS
- [ ] Tag-to-ISA-95 path mapping completed (>95% coverage)
- [ ] Parameter type and category defined for all AI-relevant tags
- [ ] Engineering units validated for all tags
- [ ] Normal and alarm ranges defined
- [ ] Mapping registered in Data Catalog

### Operations Context

- [ ] Product hierarchy defined (product family / grade / SKU)
- [ ] Recipe library linked to equipment capabilities
- [ ] Work order integration from ERP/MES to contextualization pipeline
- [ ] Shift schedule integration to contextualization pipeline

---

## Common ISA-95 Mistakes to Avoid

| Mistake | Impact | Correction |
|---------|--------|-----------|
| Inconsistent naming across sites | AI models cannot learn cross-site patterns | Standardize names in master ISA-95 hierarchy |
| Equipment in wrong area | Misrouted alerts and incorrect context | Annual hierarchy audit against physical plant layout |
| Missing intermediate levels | Broken hierarchy traversal queries | Enforce hierarchy completeness in CMMS |
| Not linking tags to hierarchy | Decontextualized data, AI model failures | Make tag mapping a gate in data onboarding process |
| Hierarchy only in ERP | Disconnected from OT data | Publish hierarchy to Data Catalog and UNS Schema Registry |

---

## Related Documents

- [Unified Namespace Guide](unified-namespace-guide.md)
- [Industrial Knowledge Graph](industrial-knowledge-graph.md)
- [Industrial Data Backbone Framework](industrial-data-backbone-framework.md)
