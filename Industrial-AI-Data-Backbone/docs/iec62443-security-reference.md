# IEC 62443 OT Cybersecurity Reference Architecture

> *Based on architectural principles by **Suresh Dakha** ([@dakhasuresh](https://github.com/dakhasuresh)), HCLTech — ISA/IEC 62443 Expert, ISA Senior Member.*

## Overview

IEC 62443 is the international standard for cybersecurity in industrial automation and control systems (IACS). It provides a risk-based framework for securing operational technology environments — PLCs, SCADA, DCS, and the networks that connect them.

For Industrial AI deployments, IEC 62443 is not optional. Connecting OT systems to AI platforms increases the attack surface of industrial environments. Every integration point between OT and IT must be designed with IEC 62443 principles from the outset.

---

## IEC 62443 Structure

```mermaid
graph TB
    subgraph SERIES["IEC 62443 Standard Series"]
        G1["62443-1-1: Terminology & Concepts"]
        G2["62443-1-2: Master Glossary"]
        G3["62443-1-3: System Security Compliance Metrics"]
        G4["62443-1-4: IACS Security Lifecycle"]
        
        P1["62443-2-1: Security Management System"]
        P2["62443-2-2: IACS Protection Rating"]
        P3["62443-2-3: Patch Management"]
        P4["62443-2-4: SP Requirements for IACS Service Providers"]

        T1["62443-3-2: Security Risk Assessment for System Design"]
        T2["62443-3-3: System Security Requirements & Security Levels"]

        C1["62443-4-1: Secure Product Development Lifecycle"]
        C2["62443-4-2: Technical Security Requirements for IACS Components"]
    end
```

The standards most directly applicable to Industrial AI architecture are:
- **62443-3-2** — Risk assessment methodology (zone and conduit model)
- **62443-3-3** — Security levels (SL 1–4) and system requirements
- **62443-4-1** — Secure development lifecycle (for AI systems built for industrial use)

---

## Zone and Conduit Model

The foundation of IEC 62443 architecture is the Zone and Conduit model. A **zone** is a logical or physical grouping of assets with a common security requirement. A **conduit** is a controlled communication path between zones.

```mermaid
graph TB
    subgraph Z4["Zone 4 — Enterprise IT / Cloud"]
        ERP["ERP"]
        AI_CLOUD["Cloud AI Platform"]
        CORP_NET["Corporate Network"]
    end

    subgraph Z3["Zone 3 — OT Site / Supervisory"]
        HIST["Process Historian"]
        MES["MES System"]
        ENGR["Engineering Workstation"]
    end

    subgraph DMZ["Industrial DMZ"]
        DATA_DIODE["Data Diode /\nUnidirectional Gateway"]
        JUMP["Jumpserver /\nRemote Access Gateway"]
        PROXY["Application Proxy\n(OPC-UA Server)"]
    end

    subgraph Z2["Zone 2 — Control System"]
        SCADA["SCADA / HMI"]
        DCS["DCS"]
        OPC_SERVER["OPC-UA Server"]
    end

    subgraph Z1["Zone 1 — Field Control"]
        PLC1["PLC (Line 1)"]
        PLC2["PLC (Line 2)"]
        SAFETY["Safety PLC (SIS)"]
    end

    subgraph Z0["Zone 0 — Field Devices"]
        SENSORS["Sensors / Actuators"]
        DRIVES["Drives / Meters"]
    end

    Z0 <-->|"Conduit: OT Fieldbus\n(Profinet, EIP)"| Z1
    Z1 <-->|"Conduit: OT LAN\n(Managed Switch)"| Z2
    Z2 -->|"Conduit: OPC-UA\n(Controlled, OT Firewall)"| DMZ
    DMZ -->|"Conduit: Historian Pull\n(IT Firewall)"| Z3
    Z3 -->|"Conduit: API / MQTT\n(Zero Trust)"| Z4
    JUMP -.->|"Remote Access\n(MFA, Privileged Session)"| Z2
```

---

## Security Level Framework

IEC 62443-3-3 defines four Security Levels (SL) that describe the capability required to resist attack by adversaries of increasing sophistication:

| Security Level | Threat Actor | Requirements |
|---------------|-------------|-------------|
| **SL 1** | Casual or unintentional violation | Basic security hygiene — default password changes, patching |
| **SL 2** | Intentional violation using simple means | Authentication, authorization, encrypted communications |
| **SL 3** | Intentional violation using sophisticated means | MFA, anomaly detection, integrity monitoring, segmentation |
| **SL 4** | State-sponsored, nation-state level attack | Highest assurance — specialized for critical national infrastructure |

### Recommended Security Levels by Zone

| Zone | Zone Description | Recommended SL | Rationale |
|------|-----------------|---------------|-----------|
| Zone 0 | Field devices | SL 1 | Legacy devices, limited capability |
| Zone 1 | Field control | SL 2 | Modern PLCs should support SL 2 |
| Zone 2 | Control system | SL 2–3 | Core OT systems; SL 3 for critical infrastructure |
| Industrial DMZ | DMZ | SL 3 | Highest exposure point; must be robust |
| Zone 3 | OT supervisory | SL 2 | Connected to both OT and IT |
| Zone 4 | Enterprise / Cloud | SL 2 (IT controls) | Standard IT security framework |
| Safety Systems | SIS | SL 3–4 | Safety instrumented systems are critical |

---

## Zero Trust for OT Environments

Zero Trust in OT is not identical to IT Zero Trust. The consequences of incorrect authorization in an OT environment can be physical — damaged equipment, process disruptions, or safety incidents. The following principles define an OT-appropriate Zero Trust model:

### Zero Trust Principles Adapted for OT

| Principle | IT Application | OT Adaptation |
|-----------|--------------|--------------|
| Verify explicitly | Authenticate every user/device | Every device has a unique identity; mTLS on all OT communications |
| Least privilege | Minimum permissions per role | Read-only by default for all IT connections to OT; writes require explicit approval per tag |
| Assume breach | Continuous monitoring | OT network traffic monitored for anomalies; assume adversary may already be in IT |
| Micro-segmentation | Network segments per application | Zones per IEC 62443; each PLC group in its own network segment |

### OT Identity Model

```mermaid
graph TB
    subgraph IDENTITIES["OT Identity Types"]
        HUMAN_OT["Human OT Users\n(Engineers, Operators)"]
        MACHINE_OT["Machine Identities\n(PLCs, Edge Nodes)"]
        SERVICE_OT["Service Identities\n(AI Agents, Applications)"]
    end

    subgraph IAM["Identity Governance"]
        PKI["PKI / Certificate Authority\n(Per-device certs)"]
        PAM["Privileged Access\nManagement (PAM)"]
        MFA_OT["OT-aware MFA\n(OTP / Hardware Token)"]
        RBAC_OT["OT Role-Based\nAccess Control"]
    end

    subgraph AUDIT["Audit & Compliance"]
        SESSION["Privileged Session\nRecording"]
        REVIEW["Quarterly Access\nReview"]
        CERT_MON["Certificate\nExpiry Monitoring"]
    end

    IDENTITIES --> IAM --> AUDIT
```

---

## Industrial AI Security Considerations

### AI-Specific OT Security Risks

| Risk | Description | Mitigation |
|------|-------------|-----------|
| Data poisoning | Adversary injects false sensor data to corrupt AI models | Data integrity checks; anomaly detection on training data |
| Model inversion | Adversary extracts process IP from AI model behavior | Model output obfuscation; access control on model APIs |
| Adversarial inputs | Manipulated sensor values cause incorrect AI recommendations | Input validation; adversarial training |
| Agent exploitation | Malicious instructions injected into agent reasoning | Agent sandboxing; input sanitization; human approval gates |
| Over-reliance on AI | Operators disable manual overrides based on AI trust | Mandatory human-in-the-loop for safety-critical decisions |
| Supply chain | Compromised AI model or library deployed to OT edge | Code signing; model provenance tracking; SBOM |

### Secure AI Deployment Pattern

```mermaid
graph LR
    subgraph UNTRUSTED["Untrusted Input Path"]
        OT_DATA["OT Sensor Data"]
        VALIDATION["Input Validation\n& Integrity Check"]
    end

    subgraph AI_SANDBOX["AI Sandbox Zone"]
        AI_MODEL["AI Model Inference\n(Isolated container)"]
        OUTPUT_FILTER["Output Filtering\n& Range Validation"]
    end

    subgraph ACTION_GATE["Action Authorization Gate"]
        POLICY["Action Policy Engine\n(Allow / Deny / Escalate)"]
        AUDIT_LOG["Immutable Audit Log"]
        HUMAN_GATE["Human Approval\n(If required)"]
    end

    subgraph TARGET["Target Systems"]
        CMMS["CMMS Write"]
        NOTIFY["Notification"]
        OT_CMD["OT Setpoint\n(Supervised only)"]
    end

    OT_DATA --> VALIDATION --> AI_MODEL --> OUTPUT_FILTER --> POLICY
    POLICY --> AUDIT_LOG
    POLICY -->|Low risk| CMMS
    POLICY -->|Notify only| NOTIFY
    POLICY -->|Human approval| HUMAN_GATE --> OT_CMD
```

---

## Remote Access Architecture

Secure remote access to OT environments is one of the highest-risk areas in industrial cybersecurity. The following architecture is recommended:

```mermaid
graph LR
    REMOTE["Remote User\n(Engineer / Vendor)"]
    MFA_GW["MFA Gateway\n(Azure AD / Okta)"]
    JUMP_SERVER["Privileged Jump Server\n(Bastion Host)"]
    PAM_TOOL["PAM Tool\n(CyberArk / BeyondTrust)"]
    DMZ_NET["Industrial DMZ"]
    OT_TARGET["OT Target System\n(DCS / PLC)"]
    SESSION_REC["Session Recording\n(Immutable)"]

    REMOTE -->|"VPN / ZTNA"| MFA_GW
    MFA_GW -->|"Authenticated session"| JUMP_SERVER
    JUMP_SERVER <--> PAM_TOOL
    PAM_TOOL -->|"Just-in-time access"| DMZ_NET
    DMZ_NET -->|"Controlled protocol\n(RDP/SSH only)"| OT_TARGET
    PAM_TOOL --> SESSION_REC
```

**Requirements:**
- No direct VPN to OT network — always via jump server
- MFA required for all remote access to OT
- Just-in-time (JIT) access with approval workflow
- Full session recording for all OT remote access
- Vendor access strictly time-limited and revoked immediately after task

---

## Patch Management for OT

OT patching is fundamentally different from IT patching. Production systems cannot be patched on the same schedule as enterprise IT without impacting operations.

### OT Patch Management Framework

| Priority | Vulnerability Type | Maximum Patch Window | Mitigation if Patch Delayed |
|----------|--------------------|---------------------|----------------------------|
| Critical | CVSS 9.0–10.0; remote code execution | 72 hours (with compensating controls) | Isolate affected system; increase monitoring |
| High | CVSS 7.0–8.9; exploitable remotely | Next planned maintenance window (< 30 days) | Firewall rule tightening; disable unused services |
| Medium | CVSS 4.0–6.9 | Quarterly patch cycle | Document and accept risk |
| Low | CVSS < 4.0 | Annual cycle | Accept risk |

**Key principle:** Unpatched OT systems are normal. The response to unpatched OT is compensating controls (network segmentation, monitoring), not emergency patching that could destabilize production systems.

---

## OT Security Monitoring

### What to Monitor in OT Environments

| Data Source | What to Collect | Detection Value |
|------------|-----------------|----------------|
| OT network traffic | NetFlow, SPAN/TAP traffic | Anomalous communications, new assets, lateral movement |
| PLC event logs | Program changes, login events | Unauthorized configuration changes |
| SCADA audit logs | User actions, setpoint changes | Insider threat, unauthorized operation |
| Historian data | Tag value behavior | Process anomalies, data manipulation |
| Firewall logs | DMZ traffic | Unauthorized access attempts, data exfiltration |
| Edge node logs | Agent actions, connectivity events | Edge compromise, agent anomaly |

### OT SIEM Integration

```mermaid
graph LR
    OT_LOGS["OT Log Sources\n(PLC, SCADA, Firewall)"]
    OT_COLLECTOR["OT Log Collector\n(Passive, Read-only)"]
    SIEM["OT SIEM\n(Splunk / Sentinel + OT content)"]
    OT_SOC["OT Security\nOperations Center"]
    PLAYBOOKS["Incident Response\nPlaybooks"]

    OT_LOGS --> OT_COLLECTOR --> SIEM --> OT_SOC --> PLAYBOOKS
```

**Important:** OT security monitoring is passive. Collectors must never send traffic back to OT systems. Use SPAN ports, TAPs, or syslog forwarding — never agent-based collection on PLCs or DCS controllers.

---

## IEC 62443 Compliance Checklist

### Zone and Conduit Design

- [ ] Security zones defined and documented for all OT areas
- [ ] Zone risk assessment completed per 62443-3-2
- [ ] Security levels assigned to all zones
- [ ] All conduits between zones documented with allowed protocols
- [ ] Industrial DMZ deployed between OT Zone 3 and IT Zone 4
- [ ] Data diode or unidirectional gateway for highest-risk conduits

### Access Control

- [ ] Unique identities for all OT users (no shared accounts)
- [ ] OT role-based access control implemented
- [ ] MFA for all remote OT access
- [ ] Privileged Access Management (PAM) deployed for OT
- [ ] Vendor access process documented and enforced

### Network Security

- [ ] OT network segmented from IT (no flat network)
- [ ] OT firewall rules documented and reviewed quarterly
- [ ] Wireless OT networks isolated and encrypted (WPA3)
- [ ] OT asset inventory complete and maintained

### Monitoring & Response

- [ ] OT network monitoring deployed (passive)
- [ ] OT SIEM with industrial-specific use cases
- [ ] OT incident response plan documented and exercised
- [ ] Vulnerability management process for OT assets

### Lifecycle

- [ ] OT security incorporated in change management process
- [ ] New integrations (including AI integrations) reviewed by OT security
- [ ] OT security training for engineers and operators
- [ ] Annual IEC 62443 compliance review

---

## Related Documents

- [Industrial AI Reference Architecture](industrial-ai-reference-architecture.md)
- [iEdgeX Reference Architecture](iedgex-reference-architecture.md)
- [Agent Fabric Architecture](agent-fabric-architecture.md)
