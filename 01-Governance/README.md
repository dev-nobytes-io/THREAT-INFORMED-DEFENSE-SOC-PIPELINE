# 01 — Governance

> **MITRE SOC Strategies Addressed:**
> - Strategy 1: Know What You Are Protecting and Why
> - Strategy 2: Give the SOC the Authority to Do Its Job
> - Strategy 3: Build a SOC Structure to Match Your Organizational Needs
> - Strategy 4: Hire AND Grow Quality Staff
> - Strategy 9: Communicate Clearly, Collaborate Often, Share Generously

---

## Purpose

Governance establishes the foundational authority, structure, staffing model, and compliance alignment for the SOC. Without governance, every downstream module (detection, response, testing) operates without mandate or measurable accountability.

This module integrates three governance frameworks:

| Framework | Scope | Role in Pipeline |
|---|---|---|
| **NIST CSF 2.0** | Enterprise-wide cybersecurity risk governance | SOC charter, risk alignment, functional categories |
| **ASD Cyber Skills Framework** | Workforce competency and skills taxonomy | Staff development, skill gap analysis, career paths |
| **DoD 8140 / DoDCWF** | Cyberspace workforce qualification | Role definitions, KSA requirements, certification mapping |

---

## NIST CSF 2.0 Integration

### CSF 2.0 Core Functions Mapped to SOC Operations

NIST CSF 2.0 introduced the **Govern (GV)** function as the overarching layer. This maps directly to SOC authority and charter.

```
┌──────────────────────────────────────────────────────────────┐
│                    NIST CSF 2.0 FUNCTIONS                    │
├──────────┬──────────┬──────────┬──────────┬──────────────────┤
│  GOVERN  │ IDENTIFY │ PROTECT  │ DETECT   │ RESPOND/RECOVER  │
│  (GV)    │ (ID)     │ (PR)     │ (DE)     │ (RS/RC)          │
├──────────┼──────────┼──────────┼──────────┼──────────────────┤
│ Module   │ Module   │ Module   │ Module   │ Module           │
│ 01       │ 01, 02   │ 06       │ 04       │ 05               │
│ Govern.  │ Data Doc │ Counter. │ Det.Eng  │ Inc. Response    │
└──────────┴──────────┴──────────┴──────────┴──────────────────┘
```

### Key CSF 2.0 Categories for SOC Governance

| CSF Category | ID | SOC Application |
|---|---|---|
| Organizational Context | GV.OC | Define what the SOC protects — mission-critical assets, data, services |
| Risk Management Strategy | GV.RM | Establish SOC risk appetite and prioritization criteria |
| Roles, Responsibilities, Authorities | GV.RR | SOC charter, escalation authority, decision rights |
| Policy | GV.PO | Detection policies, data retention, incident classification |
| Oversight | GV.OV | SOC metrics, board reporting, audit compliance |
| Supply Chain Risk Management | GV.SC | Third-party SOC integrations, MSSP governance |

### SOC Charter Template (CSF-Aligned)

Every SOC needs a formal charter. The following structure aligns with NIST CSF 2.0 GV categories:

```markdown
## SOC Charter

### 1. Mission Statement (GV.OC)
- Define the SOC's purpose in relation to organizational mission
- Identify crown jewel assets and services under protection
- Scope: networks, endpoints, cloud, OT/IoT boundaries

### 2. Authority & Mandate (GV.RR)
- Reporting structure (CISO, CIO, Board)
- Decision authority: containment, isolation, escalation
- Legal authority for monitoring, forensic acquisition
- Rules of engagement for active response

### 3. Scope of Operations (GV.OC, GV.PO)
- In-scope environments and data classifications
- Operating hours (business hours / 12x5 / 24x7)
- Tiered response model definition

### 4. Risk Alignment (GV.RM)
- Risk register integration points
- Threat profile linkage (→ Module 03)
- Risk-based detection prioritization criteria

### 5. Performance & Oversight (GV.OV)
- KPIs: MTTD, MTTR, detection coverage ratio
- Reporting cadence and audience
- Maturity assessment schedule (→ references/maturity-model.md)

### 6. Workforce Plan (GV.RR)
- Required roles (→ references/workforce-roles.md)
- Skill development pipeline
- Retention and burnout mitigation strategy
```

---

## ASD Cyber Skills Framework Integration

The Australian Signals Directorate Cyber Skills Framework provides a competency-based model for cybersecurity workforce development. It complements DoDCWF by focusing on observable skills rather than certifications alone.

### SOC-Relevant Skill Streams

| ASD Skill Stream | SOC Application | Pipeline Module Link |
|---|---|---|
| **Cyber Threat Intelligence** | Threat analyst roles, adversary profiling | Module 03 |
| **Cyber Security Operations** | SOC analyst tiers (L1/L2/L3), monitoring | Modules 02, 04 |
| **Incident Response** | IR team leads, forensic analysts | Module 05 |
| **Vulnerability Assessment** | Proactive defense, attack surface management | Module 06 |
| **Cyber Security Architecture** | Detection infrastructure, SIEM design | Module 04 |
| **Cyber Security Governance** | SOC management, compliance, reporting | Module 01 |

### Skills Progression Model

```
                    ASD Cyber Skills Levels

Level 1: Foundation     → SOC Tier 1 Analyst (Alert Triage)
Level 2: Intermediate   → SOC Tier 2 Analyst (Investigation)
Level 3: Advanced       → SOC Tier 3 / Hunt Team Lead
Level 4: Expert         → Detection Engineer / IR Lead
Level 5: Principal      → SOC Manager / CISO Advisory
```

### Skill Gap Analysis Process

1. **Map current SOC roles** to ASD skill streams
2. **Assess each staff member** against the ASD level descriptors
3. **Identify gaps** between current and required skill levels
4. **Build training plans** that align with pipeline modules:
   - Data fluency gaps → Module 02 (OSSEM training)
   - Detection gaps → Module 04 (DeTTECT, CAR workshops)
   - Threat intel gaps → Module 03 (ATT&CK training)
   - IR gaps → Module 05 (RE&CT tabletop exercises)

---

## DoD 8140 / DoDCWF Integration

DoD Directive 8140 established the Cyberspace Workforce Framework (DoDCWF), which defines work roles, KSAs (Knowledge, Skills, Abilities), and qualification requirements for cyberspace positions.

### SOC Work Roles from DoDCWF

| DoDCWF Work Role | NICE ID | SOC Position | Key KSAs |
|---|---|---|---|
| Cyber Defense Analyst | PR-CDA-001 | SOC Analyst (Tier 1/2) | Network protocols, IDS/IPS, log analysis, triage |
| Cyber Defense Incident Responder | PR-CIR-001 | Incident Responder | Forensics, malware analysis, containment procedures |
| Cyber Defense Infrastructure Support | PR-INF-001 | SOC Engineer | SIEM administration, sensor deployment, tuning |
| All-Source Analyst | AN-ASA-001 | Threat Intel Analyst | ATT&CK mapping, adversary TTPs, intelligence cycle |
| Threat/Warning Analyst | AN-TWA-001 | Threat Hunter | Hypothesis-driven hunting, data analytics, anomaly detection |
| Cyber Defense Forensics Analyst | IN-FOR-002 | Forensic Analyst | Disk/memory forensics, timeline analysis, evidence handling |
| Cyber Operations Planner | CO-OPL-001 | Purple Team Lead | Adversary emulation planning, exercise coordination |
| Cyber Workforce Manager | OV-MGT-001 | SOC Manager | Resource planning, metrics, governance |
| Security Control Assessor | SP-RSK-002 | Compliance Analyst | Control validation, audit support, framework mapping |

### DoDCWF Qualification Matrix

```
Work Role               │ Required Cert (DoD 8570/8140)  │ Training Path
────────────────────────┼────────────────────────────────┼─────────────────────
Cyber Defense Analyst   │ CySA+, GCIA, CEH              │ Module 02 → 04
Incident Responder      │ GCIH, ECIH                     │ Module 05
Threat Intel Analyst    │ GCTI, CTIA                     │ Module 03
SOC Engineer            │ CASP+, CISSP                   │ Module 02 → 04
Forensic Analyst        │ GCFE, EnCE, CFCE               │ Module 05
Threat Hunter           │ GCFA, GREM, OSCP               │ Module 03 → 04
SOC Manager             │ CISSP, CISM                    │ Module 01
```

---

## Inputs

| Input | Source Module | Description |
|---|---|---|
| Incident Reports (ATT&CK-mapped) | [05 Incident Response](../05-Incident-Response/) | Post-incident findings for risk register updates and governance reporting |
| Lessons Learned | [05 Incident Response](../05-Incident-Response/) | Process improvement recommendations from incident reviews |
| Coverage Dashboard & KPIs | [07 Detection Testing](../07-Detection-Testing/) | Detection coverage metrics, MTTD, test pass rates for executive reporting |
| Detection Gap Analysis | [04 Detection Engineering](../04-Detection-Engineering/) | Uncovered technique list for risk-based investment decisions |
| SOC-CMM Assessment Scores | [references/soc-cmm.md](../references/soc-cmm.md) | Five-domain maturity scores driving strategy and budget |
| Forensic Readiness Reports | [08 Forensics & DFIR](../08-Forensics-DFIR/) | Evidence collection readiness status and improvement needs |
| Data Gap Analysis | [02 Data Documentation](../02-Data-Documentation/) | Missing data sources mapped to ATT&CK techniques requiring investment |
| Intelligence Products | [03 Threat Intelligence](../03-Threat-Intelligence/) | Threat landscape updates for stakeholder communication |
| Metric Trends | [references/metrics.md](../references/metrics.md) | KPI/KRI trends for executive dashboard and board reporting |

---

## Outputs

This module produces the following artifacts that feed downstream modules:

| Output | Consumers | Description |
|---|---|---|
| SOC Charter | All modules | Formal authority, scope, and mandate document |
| Asset Inventory (Crown Jewels) | Module 02 (Data), Module 03 (Intel) | Prioritized list of what the SOC protects |
| Risk Register Extract | Module 03 (Intel), Module 04 (Detection) | Organizational risk context for detection prioritization |
| Workforce Role Map | Module 01 internal, HR | Current roles mapped to DoDCWF + ASD with gap analysis |
| Training Plan | All modules | Skill development plan aligned to pipeline modules |
| KPI Dashboard Definition | Module 07 (Testing) | Metrics definitions for MTTD, MTTR, coverage ratio |

---

## Implementation Checklist

- [ ] Draft SOC charter using CSF 2.0 GV category structure
- [ ] Obtain executive sign-off on SOC authority and escalation rights
- [ ] Complete asset inventory (GV.OC) — identify crown jewels
- [ ] Map all SOC roles to DoDCWF work roles
- [ ] Assess staff against ASD Cyber Skills Framework levels
- [ ] Identify skill gaps and create training roadmap
- [ ] Define SOC KPIs (MTTD, MTTR, detection coverage %)
- [ ] Establish reporting cadence (weekly ops, monthly exec, quarterly board)
- [ ] Document data retention and monitoring policies (GV.PO)
- [ ] Schedule first maturity assessment (→ references/maturity-model.md)

---

## References

- [NIST CSF 2.0](https://www.nist.gov/cyberframework)
- [ASD Cyber Skills Framework](https://www.cyber.gov.au/)
- [DoD Directive 8140 / DoDCWF](https://public.cyber.mil/cw/dcwf/)
- [NICE Cybersecurity Workforce Framework (SP 800-181r1)](https://www.nist.gov/itl/applied-cybersecurity/nice/nice-framework-resource-center)
- [MITRE 11 Strategies — Strategies 1-4, 9](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
