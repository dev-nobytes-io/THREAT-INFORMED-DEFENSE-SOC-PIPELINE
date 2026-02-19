# Threat-Informed Defense SOC Pipeline

A structured, open-framework pipeline for building and maturing a Security Operations Center (SOC) using threat-informed defense principles. This project maps directly to the **MITRE 11 Strategies of a World-Class Cybersecurity Operations Center** and integrates leading open-source and government frameworks at every stage.

> **Core Philosophy:** Threat-informed defense is an everlasting baseline hunt with detection engineering CI/CD. Every detection rule is a codified hunt hypothesis running continuously. The pipeline never completes — it cycles.

---

## Pipeline Architecture

```mermaid
flowchart TD
    subgraph Pipeline["THREAT-INFORMED DEFENSE SOC PIPELINE — M3TID Continuous Hunt Cycle"]
        direction TB
        M01["<b>01 GOVERNANCE</b><br/><i>Authority</i><br/>NIST CSF 2.0 · ASD · DoDCWF · CIISec"]
        M02["<b>02 DATA DOCUMENTATION</b><br/><i>Foundation</i><br/>OSSEM DD, CDM, DM · TH Playbook"]
        M03["<b>03 THREAT INTELLIGENCE</b><br/><i>Direction</i><br/>MITRE ATT&CK · M3TID"]
        M04["<b>04 DETECTION ENGINEERING</b><br/><i>Codify</i><br/>DeTTECT · CAR · Sigma · CI/CD"]
        M05["<b>05 INCIDENT RESPONSE</b><br/><i>Activate</i><br/>RE&CT Framework"]
        M06["<b>06 COUNTERMEASURES</b><br/><i>Harden</i><br/>MITRE D3FEND"]
        M07["<b>07 DETECTION TESTING</b><br/><i>Validate</i><br/>Atomic RT · attack_range"]
        M08["<b>08 FORENSICS & DFIR</b><br/><i>Evidence</i><br/>Velociraptor · Volatility 3 · Plaso"]

        M01 -->|"Charter, scope, assets"| M02
        M02 -->|"Inventory, CDM, DM, quality"| M04
        M03 -->|"Data source reqs"| M02
        M02 -->|"Inventory, CDM, DM"| M03
        M03 -->|"Prioritised technique list"| M04
        M03 -->|"Threat profile"| M07
        M04 -->|"Sigma rules"| M07
        M07 -->|"Scores, gaps"| M04
        M04 -->|"Alerts"| M05
        M04 -->|"Detection gaps"| M06
        M07 -->|"Gap tickets"| M06
        M06 -->|"Countermeasure status"| M05
        M05 -->|"Acquisition requests"| M08
        M08 -->|"Findings → 02, 03, 04, 07"| M03
        M08 -->|"Reports, readiness"| M01
        M07 -->|"Coverage dashboard, KPIs"| M01
        M05 -->|"Incident reports, lessons"| M01

        subgraph Hunt["EVERLASTING BASELINE HUNT"]
            Cycle["Test → Gaps → Hypotheses → Analytics → Detections → Test<br/>◀── M3TID cycle ──▶ SOC-CMM maturity ──▶"]
        end

        M07 --> Hunt
        Hunt --> M03
    end
```

---

## Module Data Flow

Every module in the pipeline has explicit **inputs** (what it consumes) and **outputs** (what it produces). The pipeline is not a linear sequence — it is a **directed graph with feedback loops**. The table below summarises every inter-module data flow:

| From | To | What Flows | Direction |
|---|---|---|---|
| **01 Governance** | 02, 03, 05, 08 | SOC charter, crown jewel list, risk context, legal authority | Forward |
| **02 Data Documentation** | 03, 04, 05, 07, 08 | Data source inventory, CDM mappings, DM relationships, quality scores | Forward |
| **03 Threat Intelligence** | 02, 04, 05, 06, 07, 08 | Prioritised technique list, Navigator layers, data source requirements, IOCs | Forward + Backward |
| **04 Detection Engineering** | 01, 05, 06, 07 | Detection alerts, Sigma rules, DeTTECT layers, gap analysis | Forward |
| **05 Incident Response** | 01, 03, 04, 08 | Incident reports, lessons learned, detection gaps, acquisition requests | Feedback |
| **06 Countermeasures** | 01, 05, 07 | D3FEND mappings, countermeasure status, defence-in-depth map | Forward |
| **07 Detection Testing** | 01, 03, 04, 06 | Test results, validated DeTTECT scores, coverage dashboard, gap tickets | Feedback |
| **08 Forensics & DFIR** | 01, 02, 03, 04, 07 | Investigation reports, ATT&CK maps, IOCs, detection gaps, data source gaps | Feedback |

### Key Feedback Loops

```mermaid
flowchart LR
    subgraph L1["Loop 1 — Detection Improvement"]
        direction LR
        A04a["04 Detection"] -->|"rules"| A07a["07 Testing"] -->|"gaps"| A04a
    end
    subgraph L2["Loop 2 — Threat Profile Update"]
        direction LR
        A03b["03 Intel"] -->|"techniques"| A04b["04 Detection"] -->|"rules"| A07b["07 Testing"] -->|"scores"| A03b
    end
    subgraph L3["Loop 3 — Incident Learning"]
        direction LR
        A04c["04 Detection"] -->|"alerts"| A05c["05 IR"] -->|"requests"| A08c["08 Forensics"] -->|"findings"| A03c["03 Intel"] -->|"updated profile"| A04c
    end
    subgraph L4["Loop 4 — Hardening Cycle"]
        direction LR
        A04d["04 Detection"] -->|"gaps"| A06d["06 Countermeasures"] -->|"controls"| A07d["07 Testing"] -->|"results"| A06d
    end
    subgraph L5["Loop 5 — Data Completeness"]
        direction LR
        A02e["02 Data"] -->|"CDM/DM"| A04e["04 Detection"] -->|"rules"| A07e["07 Testing"] -->|"data gaps"| A02e
    end
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
| 5 | **Prioritize Incident Response** | [05-Incident-Response](05-Incident-Response/), [08-Forensics-DFIR](08-Forensics-DFIR/) | RE&CT Framework, DFIR Toolchain |
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
| [08-Forensics-DFIR](08-Forensics-DFIR/) | **Evidence** — ground-truth artefacts that confirm adversary presence and generate new hunt leads | Velociraptor, Volatility 3, Plaso, Autopsy |

---

## The Continuous Hunt Cycle

This pipeline operates as an **everlasting baseline hunt** — not a linear build-once process:

```mermaid
flowchart TD
    Analyse["<b>M3TID: Analyse Threats</b><br/>(Module 03)"]
    PreHunt["<b>Pre-Hunt</b><br/>Data Management (Module 02)<br/>Hypothesis Generation (Module 03 → CTI)<br/>Analytics Development (Module 04)"]
    Explicit["<b>EXPLICIT HUNT</b><br/>(Analyst)"]
    Implicit["<b>IMPLICIT HUNT</b><br/>(Detection CI/CD)"]
    Findings["Findings"]
    Alerts["Alerts"]
    Incidents["Incidents<br/>(Module 05)"]
    Assess["<b>M3TID: Assess Defenses</b><br/>(Module 07 Testing)"]
    Gaps["<b>M3TID: Identify Gaps</b><br/>(DeTTECT + Data Gaps)"]
    Improve["<b>M3TID: Improve</b><br/>(Module 04 + 05 + 06)"]
    Share["<b>M3TID: Share</b><br/>(Community, ISACs)"]
    Repeat(["REPEAT — the cycle never stops"])

    Analyse --> PreHunt
    PreHunt --> Explicit
    PreHunt --> Implicit
    Explicit --> Findings
    Implicit --> Alerts
    Alerts --> Incidents
    Findings --> Assess
    Alerts --> Assess
    Assess --> Gaps
    Gaps --> Improve
    Improve --> Share
    Share --> Repeat
    Repeat --> Analyse
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

```mermaid
flowchart TD
    subgraph P1["<b>Phase 1: Foundation</b> (SOC-CMM Level 1 → 2)"]
        direction LR
        P1A["01-Governance<br/>Establish charter, roles, authority"]
        P1B["02-Data-Docs<br/>Inventory and document data sources (OSSEM-DD)"]
        P1C["03-Threat-Intel<br/>Build initial ATT&CK threat profile"]
    end
    subgraph P2["<b>Phase 2: Standardise</b> (SOC-CMM Level 2 → 3 — maximum pipeline value)"]
        direction LR
        P2A["02-Data-Docs<br/>Standardise (OSSEM-CDM) and model (OSSEM-DM)"]
        P2B["03-Threat-Intel<br/>Generate hunt hypotheses from threat profile (M3TID)"]
        P2C["04-Detection-Eng<br/>Build detections, establish CI/CD pipeline"]
        P2D["05-IR<br/>Create RE&CT playbooks for priority techniques"]
    end
    subgraph P3["<b>Phase 3: Harden & Validate</b> (SOC-CMM Level 3 solidified)"]
        direction LR
        P3A["06-Countermeasures<br/>Map and deploy D3FEND defensive techniques"]
        P3B["07-Testing<br/>Validate detection coverage with Atomic RT"]
        P3C["08-Forensics<br/>Establish forensic readiness and DFIR capability"]
        P3D["SOC-CMM Assessment<br/>Baseline all five domains"]
    end
    subgraph P4["<b>Phase 4: Continuous Hunt</b> (SOC-CMM Level 3 → 4+)"]
        direction LR
        P4A(["Everlasting cycle:<br/>Hunt → Detect → Respond → Test → Improve"])
    end

    P1 --> P2 --> P3 --> P4
    P4 -.->|"cycle repeats"| P1
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
├── 08-Forensics-DFIR/
│   └── README.md                          # DFIR capability areas, tool options, artefact reference
└── references/
    ├── soc-cmm.md                         # SOC-CMM — full framework, 5 domains, assessment
    ├── maturity-model.md                  # SOC-CMM-based maturity progression per module
    ├── m3tid-continuous-hunt.md            # M3TID + pre-hunt methodology + continuous hunt
    ├── workforce-roles.md                 # DoDCWF, ASD, CIISec role mapping
    ├── tool-integration-matrix.md         # Tool options matrix — curated OSS tools per capability
    ├── metrics.md                         # KPIs, KRIs, SOC-CMM self-assessment scorecard
    └── agentic-soc.md                     # Human-AI operating model — 6 SOC agents
```

---

## License

This project is released for educational and defensive cybersecurity purposes. Individual frameworks and tools referenced retain their own licensing terms.
