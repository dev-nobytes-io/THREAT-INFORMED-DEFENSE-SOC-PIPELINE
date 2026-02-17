# Threat-Informed Defense SOC Pipeline

A structured, open-framework pipeline for building and maturing a Security Operations Center (SOC) using threat-informed defense principles. This project maps directly to the **MITRE 11 Strategies of a World-Class Cybersecurity Operations Center** and integrates leading open-source and government frameworks at every stage.

> **Core Philosophy:** Threat-informed defense is an everlasting baseline hunt with detection engineering CI/CD. Every detection rule is a codified hunt hypothesis running continuously. The pipeline never completes — it cycles.

---

## Pipeline Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    THREAT-INFORMED DEFENSE SOC PIPELINE                      │
│                    M3TID Continuous Hunt Cycle                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────┐   ┌──────────────┐   ┌──────────────┐   ┌─────────────┐  │
│  │ 01           │   │ 02           │   │ 03           │   │ 04          │  │
│  │ GOVERNANCE   │──▶│ DATA         │──▶│ THREAT       │──▶│ DETECTION   │  │
│  │ (Authority)  │   │ DOCUMENTATION│   │ INTELLIGENCE │   │ ENGINEERING │  │
│  │              │   │ (Foundation) │   │ (Direction)  │   │ (Codify)    │  │
│  │ NIST CSF 2.0 │   │ OSSEM        │   │ MITRE ATT&CK │   │ DeTTECT    │  │
│  │ ASD CSF      │   │ TH Playbook  │   │ M3TID        │   │ MITRE CAR   │  │
│  │ DoDCWF 8140  │   │ Pre-Hunt     │   │              │   │ Sigma       │  │
│  │ CIISec       │   │ Data Mgmt    │   │              │   │ CI/CD       │  │
│  └─────────────┘   └──────────────┘   └──────────────┘   └──────┬──────┘  │
│                                                                   │         │
│         ┌────────────────────────────────────────────────────────┘         │
│         ▼                                                                   │
│  ┌─────────────┐   ┌──────────────┐   ┌──────────────┐                     │
│  │ 05           │   │ 06           │   │ 07           │                     │
│  │ INCIDENT     │──▶│ COUNTER-     │──▶│ DETECTION    │ ──┐                │
│  │ RESPONSE     │   │ MEASURES     │   │ TESTING      │   │                │
│  │ (Activate)   │   │ (Harden)     │   │ (Validate)   │   │                │
│  │              │   │              │   │              │   │  Continuous   │
│  │ RE&CT        │   │ MITRE D3FEND │   │ Atomic RT    │   │  Feedback    │
│  │              │   │              │   │ attack_range │   │  Loop        │
│  └─────────────┘   └──────────────┘   └──────────────┘   │                │
│                                                            │                │
│         ┌──────────────────────────────────────────────────┘                │
│         ▼                                                                   │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                    EVERLASTING BASELINE HUNT                         │    │
│  │   Testing → Gaps → Hypotheses → Analytics → Detections → Testing   │    │
│  │         ◀───── M3TID cycle ─────▶  SOC-CMM maturity ─────▶         │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## MITRE 11 Strategies Mapping

Each pipeline module maps to one or more of the MITRE 11 Strategies for a World-Class SOC:

| # | MITRE SOC Strategy | Pipeline Module(s) | Primary Frameworks |
|---|---|---|---|
| 1 | **Know What You Are Protecting and Why** | [01-Governance](01-Governance/) | NIST CSF 2.0 (ID.AM, GV), DoDCWF 8140 |
| 2 | **Give the SOC the Authority to Do Its Job** | [01-Governance](01-Governance/) | NIST CSF 2.0 (GV), ASD Cyber Skills Framework |
| 3 | **Build a SOC Structure to Match Your Organizational Needs** | [01-Governance](01-Governance/) | ASD CSF, DoDCWF 8140, CIISec |
| 4 | **Hire AND Grow Quality Staff** | [01-Governance](01-Governance/) | ASD CSF, DoDCWF 8140, CIISec Skills Framework |
| 5 | **Prioritize Incident Response** | [05-Incident-Response](05-Incident-Response/) | RE&CT Framework |
| 6 | **Illuminate Adversaries with Cyber Threat Intelligence** | [03-Threat-Intelligence](03-Threat-Intelligence/) | MITRE ATT&CK, M3TID |
| 7 | **Select and Collect the Right Data** | [02-Data-Documentation](02-Data-Documentation/) | OSSEM (DD, CDM, DM), Threat Hunters Playbook |
| 8 | **Leverage Tools to Support Analyst Workflow** | [04-Detection-Engineering](04-Detection-Engineering/) | DeTTECT, MITRE CAR, Sigma |
| 9 | **Communicate Clearly, Collaborate Often, Share Generously** | [01-Governance](01-Governance/), All Modules | NIST CSF 2.0 (GV.RR), M3TID (Focused Sharing) |
| 10 | **Measure Performance to Improve Performance** | [07-Detection-Testing](07-Detection-Testing/) | Atomic Red Team, attack_range, SOC-CMM |
| 11 | **Turn up the Dial Incrementally** | [references/](references/) | SOC-CMM, Maturity Model |

---

## Module Index

| Module | Continuous Hunt Role | Key Frameworks & Tools |
|---|---|---|
| [01-Governance](01-Governance/) | **Authority** — mandate, budget, staffing for continuous operations | NIST CSF 2.0, ASD CSF, DoDCWF 8140, CIISec |
| [02-Data-Documentation](02-Data-Documentation/) | **Foundation** — data management disciplines that make data huntable | OSSEM (DD, CDM, DM), Threat Hunters Playbook Pre-Hunt |
| [03-Threat-Intelligence](03-Threat-Intelligence/) | **Direction** — CTI-driven hypothesis generation; threat profile focus | MITRE ATT&CK, M3TID |
| [04-Detection-Engineering](04-Detection-Engineering/) | **Codification** — promotes hunt findings into permanent automated detections | DeTTECT, MITRE CAR, Sigma, Detection CI/CD |
| [05-Incident-Response](05-Incident-Response/) | **Activation** — executes when a codified hunt (detection) fires | RE&CT Framework |
| [06-Countermeasures](06-Countermeasures/) | **Hardening** — prevents techniques that can't be hunted/detected reliably | MITRE D3FEND |
| [07-Detection-Testing](07-Detection-Testing/) | **Validation** — verifies codified hunts (detections) still work | Atomic Red Team, attack_range |

---

## The Continuous Hunt Cycle

This pipeline operates as an **everlasting baseline hunt** — not a linear build-once process:

```
         M3TID: Analyse Threats (Module 03)
                    │
                    ▼
         Pre-Hunt: Data Management (Module 02)
         Pre-Hunt: Hypothesis Generation (Module 03 → CTI)
         Pre-Hunt: Analytics Development (Module 04)
                    │
              ┌─────┴─────┐
              ▼           ▼
        EXPLICIT       IMPLICIT
        HUNT           HUNT
        (Analyst)      (Detection CI/CD)
              │           │
              ▼           ▼
        FINDINGS      ALERTS ──▶ INCIDENTS (Module 05)
              │           │
              └─────┬─────┘
                    ▼
         M3TID: Assess Defenses (Module 07 Testing)
                    │
                    ▼
         M3TID: Identify Gaps (DeTTECT + Data Gaps)
                    │
                    ▼
         M3TID: Improve (Module 04 + 05 + 06)
                    │
                    ▼
         M3TID: Share (Community, ISACs)
                    │
                    └──────▶ REPEAT (the cycle never stops)
```

For the full methodology, see [references/m3tid-continuous-hunt.md](references/m3tid-continuous-hunt.md).

---

## Maturity Assessment (SOC-CMM)

This pipeline uses the **SOC Capability Maturity Model (SOC-CMM)** as its primary maturity framework, providing holistic assessment across five domains:

| SOC-CMM Domain | What It Assesses | Primary Pipeline Module |
|---|---|---|
| **Business** | Governance, budget, strategy, stakeholders, compliance, risk | 01 Governance |
| **People** | Staffing, training, careers, knowledge, collaboration | Workforce Roles (DoDCWF, ASD, CIISec) |
| **Process** | Procedures, use cases, incidents, automation, metrics | 03, 04, 05, 07 |
| **Technology** | SIEM, EDR, NSM, SOAR, TIP, testing infrastructure | 02, 04, 07 |
| **Services** | Monitoring, IR, CTI, hunting, forensics, testing | All modules |

**Target Level 3 across all domains before pushing any single domain to Level 4+.**

For the full SOC-CMM reference, see [references/soc-cmm.md](references/soc-cmm.md).
For maturity criteria per pipeline module, see [references/maturity-model.md](references/maturity-model.md).

---

## Getting Started

### Prerequisites

- Familiarity with MITRE ATT&CK Navigator
- Access to a SIEM/log aggregation platform
- Python 3.8+ (for DeTTECT, attack_range tooling)
- Git (for version-controlled detection-as-code)

### Recommended Progression

```
Phase 1: Foundation (SOC-CMM Level 1 → 2)
├── 01-Governance    → Establish charter, roles, authority
├── 02-Data-Docs     → Inventory and document data sources (OSSEM-DD)
└── 03-Threat-Intel  → Build initial ATT&CK threat profile

Phase 2: Standardise (SOC-CMM Level 2 → 3 — maximum pipeline value)
├── 02-Data-Docs     → Standardise (OSSEM-CDM) and model (OSSEM-DM)
├── 03-Threat-Intel  → Generate hunt hypotheses from threat profile (M3TID)
├── 04-Detection-Eng → Build detections, establish CI/CD pipeline
└── 05-IR            → Create RE&CT playbooks for priority techniques

Phase 3: Harden & Validate (SOC-CMM Level 3 solidified)
├── 06-Countermeasures → Map and deploy D3FEND defensive techniques
├── 07-Testing         → Validate detection coverage with Atomic RT
└── SOC-CMM Assessment → Baseline all five domains

Phase 4: Continuous Hunt (SOC-CMM Level 3 → 4+)
└── Everlasting cycle: Hunt → Detect → Respond → Test → Improve
```

---

## Framework Reference Links

| Framework | Source |
|---|---|
| MITRE 11 Strategies | [11 Strategies of a World-Class Cybersecurity Operations Center](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf) |
| SOC-CMM | [SOC Capability Maturity Model](https://www.soc-cmm.com) |
| M3TID / CTID | [MITRE Engenuity Center for Threat-Informed Defense](https://ctid.mitre-engenuity.org/) |
| NIST CSF 2.0 | [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework) |
| ASD Cyber Skills Framework | [Australian Signals Directorate CSF](https://www.cyber.gov.au/) |
| CIISec Skills Framework | [Chartered Institute of Information Security](https://www.ciisec.org/skills-framework) |
| DoD 8140 / DoDCWF | [DoD Cyberspace Workforce Framework](https://public.cyber.mil/cw/dcwf/) |
| OSSEM | [Open Source Security Events Metadata](https://github.com/OTRF/OSSEM) |
| Threat Hunters Playbook | [Threat Hunter Playbook](https://github.com/OTRF/ThreatHunter-Playbook) |
| MITRE ATT&CK | [ATT&CK Knowledge Base](https://attack.mitre.org/) |
| DeTTECT | [DeTT&CT Framework](https://github.com/rabobank-cdc/DeTTECT) |
| MITRE CAR | [Cyber Analytics Repository](https://car.mitre.org/) |
| Atomic Red Team | [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team) |
| RE&CT | [RE&CT Framework](https://github.com/atc-project/atc-react) |
| MITRE D3FEND | [D3FEND Knowledge Graph](https://d3fend.mitre.org/) |
| Splunk attack_range | [attack_range](https://github.com/splunk/attack_range) |

---

## Project Structure

```
THREAT-INFORMED-DEFENSE-SOC-PIPELINE/
├── README.md                              # This file
├── 01-Governance/
│   └── README.md                          # NIST CSF 2.0, ASD CSF, DoDCWF 8140, CIISec
├── 02-Data-Documentation/
│   ├── README.md                          # OSSEM, TH Playbook — 4 data management disciplines
│   ├── data-standardisation.md            # OSSEM-CDM field normalisation guide
│   └── data-modelling.md                  # OSSEM-DM entity relationship modelling guide
├── 03-Threat-Intelligence/
│   └── README.md                          # MITRE ATT&CK, M3TID threat profiling
├── 04-Detection-Engineering/
│   └── README.md                          # DeTTECT, MITRE CAR, Sigma, Detection CI/CD
├── 05-Incident-Response/
│   └── README.md                          # RE&CT Framework
├── 06-Countermeasures/
│   └── README.md                          # MITRE D3FEND
├── 07-Detection-Testing/
│   └── README.md                          # Atomic Red Team, attack_range
└── references/
    ├── soc-cmm.md                         # SOC-CMM — full framework, 5 domains, assessment
    ├── maturity-model.md                  # SOC-CMM-based maturity progression per module
    ├── m3tid-continuous-hunt.md            # M3TID + pre-hunt methodology + continuous hunt
    ├── workforce-roles.md                 # DoDCWF, ASD, CIISec role mapping
    └── tool-integration-matrix.md         # Framework interoperability guide
```

---

## License

This project is released for educational and defensive cybersecurity purposes. Individual frameworks and tools referenced retain their own licensing terms.
