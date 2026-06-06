# Oil & Gas Use Cases

## Overview

The oil and gas sector operates some of the most complex, high-value, and high-consequence industrial assets in the world. From upstream production to downstream refining, AI has transformational potential — but only when built on a foundation of reliable, contextual, and secure operational data. The Industrial Data Backbone is that foundation.

---

## Production Optimization

### Business Problem

Hydrocarbon production is an intrinsically complex optimization problem. Each well has a unique production profile that degrades over time. Lift systems (ESP, rod pump, gas lift) must be tuned to maximize production while managing energy consumption, artificial lift costs, and equipment wear. Traditional approaches rely on periodic engineer reviews — too slow and too infrequent to capture the real-time opportunity.

AI-driven production optimization continuously monitors well and reservoir conditions, identifies underperformance, and recommends or autonomously implements operational changes to maximize production.

### Business Value

| Metric | Typical Improvement |
|--------|-------------------|
| Production uplift | 3–8% additional production |
| Energy cost reduction (ESPs) | 10–20% |
| Artificial lift optimization | 15–25% reduction in lift cost per barrel |
| Well intervention cost reduction | 20–30% (better timing, better targeting) |
| Engineer productivity | 3–5× (AI handles routine monitoring) |

### Data Requirements

| Source | Parameters | Frequency |
|--------|-----------|-----------|
| Wellhead sensors | Tubing/casing pressure, temperature, flow rate | 1–60 second intervals |
| ESP controllers | Motor current, frequency, torque, vibration | 1–60 second intervals |
| SCADA | Separator conditions, pipeline pressure, valve states | 1-minute intervals |
| Metering | Oil, water, gas rates per well | Hourly / daily test |
| LIMS | Well fluid composition, water cut | Per test |
| Reservoir model | Reservoir pressure, drive mechanism | Monthly / updated by simulation |
| Downhole gauges | Downhole pressure, temperature (where installed) | 1–30 minute intervals |

### Architecture Pattern

```mermaid
graph TB
    subgraph WELL_LEVEL["Well-Level Monitoring"]
        WELLHEAD["Wellhead SCADA\n(RTU / PLC)"]
        ESP_CTRL["ESP Controller\n(Variable Speed Drive)"]
        DOWNHOLE["Downhole Gauge\n(Optional)"]
    end

    subgraph FIELD_EDGE["Field Edge ()"]
        WELL_MODEL["Well Performance\nModel (per well)"]
        INFLOW["Inflow Performance\nCurve Tracking"]
        LIFT_EFFICIENCY["Lift Efficiency\nCalculation"]
    end

    subgraph PROD_AI["Production AI"]
        ANOMALY_WELL["Well Anomaly\nDetection"]
        DEFERMENT["Deferment Analysis\n(Lost production by cause)"]
        OPTIM["Optimization Engine\n(Setpoint recommendations)"]
        FORECAST_PROD["Production Forecasting\n(Well and field level)"]
    end

    subgraph AGENT["Production Agent"]
        MONITOR_AG["Continuous\nMonitoring Loop"]
        RECOMMEND["Setpoint\nRecommendation"]
        AUTO_ADJ["Autonomous\nAdjustment (supervised)"]
        ENG_ALERT["Engineer Alert\n(Exceptions only)"]
    end

    WELL_LEVEL --> FIELD_EDGE --> PROD_AI --> AGENT
```

### Use Case: ESP Surveillance and Optimization

**Problem:** Electric submersible pumps (ESPs) are the primary artificial lift method for high-rate wells. Each ESP must be operated within its optimal performance envelope — too little speed under-produces the well; too much speed risks pump-off (gas ingestion), overheating, and premature failure. ESP failures cost $500K–$2M+ in intervention costs and lost production.

**AI Solution:**
1. Collect ESP operating data (current, frequency, torque, vibration, motor temperature) at 1-minute intervals via VSD controllers
2. Build a digital model of each ESP's performance curve (head vs. flow at current fluid properties)
3. Train an anomaly detection model on each ESP to detect deviation from normal operating signature
4. Develop a pump-off prediction model (predict gas interference 30–60 minutes before it causes a trip)
5. Optimize frequency setpoints to maximize production while staying within safe operating envelope

**Detection examples:**
- Gradual efficiency decline: ESP performance curve shifting left → worn wear rings or rotor damage → recommend workover before failure
- Intermittent gas: Irregular current signature with transient underload events → recommend speed reduction and gas handling strategy
- Bearing wear: Vibration signature change at specific frequency → recommend ESP replacement at next scheduled intervention

---

## Integrity Management

### Business Problem

Pipelines, pressure vessels, storage tanks, and structural assets in oil and gas are subject to degradation mechanisms (corrosion, erosion, fatigue, cracking) that can lead to loss of containment — with severe safety, environmental, and business consequences. Regulatory frameworks (API 570, API 510, PSSR) mandate inspection programs, but traditional time-based inspection is inefficient and often misses developing threats.

AI-driven integrity management uses continuous monitoring data, historical inspection records, and process conditions to predict degradation rates and optimize inspection and intervention timing.

### Business Value

| Metric | Typical Improvement |
|--------|-------------------|
| Inspection cost reduction | 20–35% (risk-based interval optimization) |
| Unplanned releases / leaks | –40–60% |
| Regulatory compliance confidence | Significantly improved with audit-ready data |
| Remaining life accuracy | ±5 years vs. ±15 years with traditional methods |
| Inspection planning efficiency | +40% (AI-prioritized inspection lists) |

### Integrity Data Model

```mermaid
graph LR
    subgraph PROCESS["Process Data"]
        CORR_PARAM["Corrosivity Parameters\n(H₂S, CO₂, pH, velocity, temperature)"]
        FLOW["Flow Conditions\n(Flow rate, phase fractions)"]
        PRESS["Pressure Cycles\n(Fatigue damage tracking)"]
    end

    subgraph INSPECTION["Inspection Data"]
        UT["Ultrasonic Testing\n(Wall thickness measurements)"]
        CIS["Cathodic Protection\n(CIPS / DCVG survey)"]
        ILI["In-Line Inspection\n(Smart pig data)"]
        VISUAL["Visual Inspection\n(Corrosion grade, coating condition)"]
    end

    subgraph INTEGRITY_AI["Integrity AI Models"]
        CORR_RATE["Corrosion Rate Model\n(Norsok M-506, ML hybrid)"]
        REMAINING_WT["Remaining Wall\nThickness Prediction"]
        FAILURE_PROB["Failure Probability\nCalculation (PoF)"]
        RISK_RANK["Risk Ranking\n(PoF × CoF)"]
        INSPECTION_OPT["Inspection Interval\nOptimization"]
    end

    PROCESS --> CORR_RATE
    INSPECTION --> REMAINING_WT
    CORR_RATE & REMAINING_WT --> FAILURE_PROB --> RISK_RANK --> INSPECTION_OPT
```

### Risk-Based Inspection Workflow

```mermaid
graph LR
    DATA["Continuous data\n(process + inspection)"]
    RL_UPDATE["Real-time risk level\nupdate (per asset)"]
    RISK_MATRIX["Risk matrix\n(API 580 framework)"]
    PRIORITIZE["AI-prioritized\ninspection list"]
    SCHEDULE["Optimized inspection\nschedule (cost + risk)"]
    EXECUTE["Inspection execution\n(technician + report)"]
    FEEDBACK["Results feedback\n(model calibration)"]

    DATA --> RL_UPDATE --> RISK_MATRIX --> PRIORITIZE --> SCHEDULE --> EXECUTE --> FEEDBACK --> DATA
```

---

## Predictive Inspection

### Business Problem

Physical inspection of oil and gas assets is expensive, hazardous (working at height, confined spaces, hazardous atmospheres), and disruptive. Many inspections are routine with no findings — but the schedule cannot be safely relaxed without risk data to justify it. AI-driven predictive inspection identifies which assets genuinely require attention and what the inspector should look for — transforming inspection from a compliance exercise to a targeted, high-value activity.

### Business Value

| Metric | Typical Improvement |
|--------|-------------------|
| Inspection efficiency | +50% (fewer zero-finding inspections) |
| Inspector preparation time | –40% (AI pre-brief with likely findings) |
| Finding detection rate | +25% (targeted inspections find more) |
| Defect-to-repair time | –30% (earlier detection) |
| Inspector safety | Improved (fewer unnecessary entries to hazardous areas) |

### Predictive Inspection Architecture

```mermaid
graph TB
    subgraph CONTINUOUS["Continuous Monitoring Data"]
        CORR_COUPONS["Corrosion Coupons\n(Weight loss data)"]
        ONLINE_UT["Online UT\n(Fixed monitoring points)"]
        ACOUSTIC_EM["Acoustic Emission\n(Crack detection)"]
        HYDROGEN_PROBES["Hydrogen Flux Probes\n(HIC / SOHIC detection)"]
    end

    subgraph HISTORICAL["Historical Inspection Data"]
        PAST_REPORTS["Past Inspection Reports\n(Digitized + structured)"]
        THICKNESS_DB["Thickness Measurement\nDatabase"]
        ANOMALY_DB["Historical Anomaly\nDatabase"]
    end

    subgraph AI_PREDICT["Predictive Inspection AI"]
        NLP_REPORTS["NLP Report Analysis\n(Extract findings from text reports)"]
        DEGRADATION_MOD["Degradation Modeling\n(Corrosion rate projection)"]
        PRIORITY_MODEL["Inspection Priority\nModel (what needs inspection now)"]
        FINDING_PRED["Finding Prediction\n(What will the inspector likely find)"]
    end

    subgraph INSPECTOR_SUPPORT["Inspector Decision Support"]
        BRIEF["AI Pre-inspection Brief\n(Likely findings, focus areas)"]
        ROUTE_OPT["Inspection Route\nOptimization"]
        MOBILE_GUIDE["Mobile Inspection\nGuidance (AR-ready)"]
        REPORT_ASSIST["AI Report\nGeneration Assistant"]
    end

    CONTINUOUS --> AI_PREDICT
    HISTORICAL --> NLP_REPORTS --> AI_PREDICT
    AI_PREDICT --> INSPECTOR_SUPPORT
```

### Use Case: Refinery Pressure Vessel Inspection Optimization

**Problem:** A mid-size refinery has 850 pressure vessels subject to API 510 inspection requirements. Current inspection schedule is time-based (4-year external, 8-year internal). Of 200 inspections performed annually, approximately 140 result in no significant findings — wasted cost and risk exposure from unnecessary vessel entry.

**AI Solution:**
1. Digitize 20 years of historical inspection reports using NLP to extract measurements, findings, and recommendations
2. Integrate with process data to calculate actual corrosion rates per vessel (not just nominal rates from materials database)
3. Build a degradation model per vessel class, accounting for actual operating conditions
4. Calculate current risk rank per vessel using API 580 methodology with AI-enhanced data inputs
5. Generate optimized inspection schedule: high-risk vessels inspected sooner, low-risk vessels safely deferred
6. Produce AI-generated pre-inspection brief for each vessel: expected findings, recommended techniques, historical photos

**Results:** 30% reduction in inspection volume while maintaining or improving defect detection rate; estimated £1.2M annual saving on a medium refinery; 2 fewer confined-space entries per week (significant safety benefit).

---

## Data Architecture for Oil & Gas

### SCADA / RTU Connectivity Pattern (Remote Assets)

Oil and gas assets are often in remote locations with challenging connectivity. The following pattern addresses this:

```mermaid
graph LR
    subgraph WELLSITE["Remote Wellsite"]
        RTU["RTU\n(Field PLC)"]
        SOLAR["Solar Power\n+ Battery"]
        SATELLITE["Satellite / Cellular\n(LTE-M / Iridium)"]
    end

    subgraph FIELD_OFFICE["Field Office / PoP"]
        EDGE_AGG["Edge Aggregator\n(Industrial Edge Node)"]
        LOCAL_HIST["Local Historian\n(72hr buffer)"]
    end

    subgraph CLOUD["Operations Centre"]
        SCADA_CLOUD["Cloud SCADA\n(Aveva / ICONICS)"]
        AI_PLATFORM["AI / Analytics\nPlatform"]
        PROD_CONTROL["Production\nControl Centre"]
    end

    RTU -->|"MQTT / Modbus\nover cellular"| SATELLITE
    SATELLITE -->|"Aggregated\ndata stream"| EDGE_AGG
    EDGE_AGG --> LOCAL_HIST
    EDGE_AGG -->|"Sparkplug B\nMQTT"| SCADA_CLOUD
    SCADA_CLOUD --> AI_PLATFORM
    AI_PLATFORM --> PROD_CONTROL
```

**Connectivity technologies for remote O&G assets:**

| Technology | Bandwidth | Latency | Cost | Best For |
|------------|-----------|---------|------|---------|
| 4G LTE / LTE-M | Medium | Low | Medium | Onshore well pads |
| Satellite (Starlink) | High | Medium | High | Remote offshore / arctic |
| Iridium SBD | Very low | High | Low | Emergency backup |
| Microwave / Radio | High | Very low | Medium | Gathering systems, pipelines |
| Fibre (where available) | Very high | Very low | High | Platforms, major facilities |

---

## Related Documents

- [Manufacturing Use Cases](manufacturing-use-cases.md)
- [Utility Use Cases](utility-use-cases.md)
- [Industrial AI Reference Architecture](../docs/industrial-ai-reference-architecture.md)
- [Industrial AI Reference Architecture](../docs/industrial-ai-reference-architecture.md)
