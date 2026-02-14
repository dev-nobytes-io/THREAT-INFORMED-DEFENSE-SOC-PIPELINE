# Threat-Informed Defense SOC Pipeline

A structured, open-framework pipeline for building and maturing a Security Operations Center (SOC) using threat-informed defense principles. This project maps directly to the **MITRE 11 Strategies of a World-Class Cybersecurity Operations Center** and integrates leading open-source and government frameworks at every stage.

---

## Pipeline Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    THREAT-INFORMED DEFENSE SOC PIPELINE                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────┐   ┌──────────────┐   ┌──────────────┐   ┌─────────────┐  │
│  │ 01           │   │ 02           │   │ 03           │   │ 04          │  │
│  │ GOVERNANCE   │──▶│ DATA         │──▶│ THREAT       │──▶│ DETECTION   │  │
│  │              │   │ DOCUMENTATION│   │ INTELLIGENCE │   │ ENGINEERING │  │
│  │ NIST CSF 2.0 │   │ OSSEM        │   │ MITRE ATT&CK │   │ DeTTECT    │  │
│  │ ASD CSF      │   │ TH Playbook  │   │              │   │ MITRE CAR   │  │
│  │ DoDCWF 8140  │   │              │   │              │   │ Atomic RT   │  │
│  └─────────────┘   └──────────────┘   └──────────────┘   └──────┬──────┘  │
│                                                                   │         │
│         ┌────────────────────────────────────────────────────────┘         │
│         ▼                                                                   │
│  ┌─────────────┐   ┌──────────────┐   ┌──────────────┐                     │
│  │ 05           │   │ 06           │   │ 07           │                     │
│  │ INCIDENT     │──▶│ COUNTER-     │──▶│ DETECTION    │ ──┐                │
│  │ RESPONSE     │   │ MEASURES     │   │ TESTING      │   │                │
│  │              │   │              │   │              │   │                │
│  │ RE&CT        │   │ MITRE D3FEND │   │ Atomic RT    │   │  Continuous   │
│  │              │   │              │   │ attack_range │   │  Feedback     │
│  └─────────────┘   └──────────────┘   └──────────────┘   │  Loop         │
│                                                            │                │
│         ┌──────────────────────────────────────────────────┘                │
│         ▼                                                                   │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                    CONTINUOUS IMPROVEMENT CYCLE                      │    │
│  │   Testing results feed back into Detection Engineering & Intel      │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## MITRE 11 Strategies Mapping

Each pipeline module maps to one or more of the MITRE 11 Strategies for a World-Class SOC. The table below shows how this pipeline provides comprehensive coverage.

| # | MITRE SOC Strategy | Pipeline Module(s) | Primary Frameworks |
|---|---|---|---|
| 1 | **Know What You Are Protecting and Why** | [01-Governance](01-Governance/) | NIST CSF 2.0 (ID.AM, GV), DoDCWF 8140 |
| 2 | **Give the SOC the Authority to Do Its Job** | [01-Governance](01-Governance/) | NIST CSF 2.0 (GV), ASD Cyber Skills Framework |
| 3 | **Build a SOC Structure to Match Your Organizational Needs** | [01-Governance](01-Governance/) | ASD CSF, DoDCWF 8140 Work Roles |
| 4 | **Hire AND Grow Quality Staff** | [01-Governance](01-Governance/) | ASD Cyber Skills Framework, DoDCWF 8140 |
| 5 | **Prioritize Incident Response** | [05-Incident-Response](05-Incident-Response/) | RE&CT Framework |
| 6 | **Illuminate Adversaries with Cyber Threat Intelligence** | [03-Threat-Intelligence](03-Threat-Intelligence/) | MITRE ATT&CK |
| 7 | **Select and Collect the Right Data** | [02-Data-Documentation](02-Data-Documentation/) | OSSEM, Threat Hunters Playbook |
| 8 | **Leverage Tools to Support Analyst Workflow** | [04-Detection-Engineering](04-Detection-Engineering/) | DeTTECT, MITRE CAR |
| 9 | **Communicate Clearly, Collaborate Often, Share Generously** | [01-Governance](01-Governance/), All Modules | NIST CSF 2.0 (GV.RR) |
| 10 | **Measure Performance to Improve Performance** | [07-Detection-Testing](07-Detection-Testing/) | Atomic Red Team, attack_range.py |
| 11 | **Turn up the Dial Incrementally** | [references/](references/) | Maturity Model across all modules |

---

## Module Index

| Module | Purpose | Key Frameworks & Tools |
|---|---|---|
| [01-Governance](01-Governance/) | SOC charter, workforce planning, authority, compliance | NIST CSF 2.0, ASD Cyber Skills Framework, DoD 8140 / DoDCWF |
| [02-Data-Documentation](02-Data-Documentation/) | Data source inventory, log standardization, event taxonomy | OSSEM Project, Threat Hunters Playbook |
| [03-Threat-Intelligence](03-Threat-Intelligence/) | Threat modeling, adversary profiling, technique prioritization | MITRE ATT&CK |
| [04-Detection-Engineering](04-Detection-Engineering/) | Analytic development, coverage mapping, detection-as-code | DeTTECT, MITRE CAR, Atomic Red Team |
| [05-Incident-Response](05-Incident-Response/) | Response playbooks, action mapping, escalation procedures | RE&CT Framework |
| [06-Countermeasures](06-Countermeasures/) | Defensive technique mapping, hardening, active defense | MITRE D3FEND |
| [07-Detection-Testing](07-Detection-Testing/) | Adversary simulation, detection validation, purple teaming | Atomic Red Team, Splunk attack_range.py |

---

## Continuous Feedback Loop

This pipeline is not linear — it operates as a **continuous improvement cycle**:

```
Detection Testing (07) ──results──▶ Threat Intelligence (03)
                                          │
                                    coverage gaps
                                          │
                                          ▼
                                   Detection Engineering (04)
                                          │
                                     new analytics
                                          │
                                          ▼
                                   Detection Testing (07)
                                      [repeat]
```

1. **Test** detections against known adversary techniques (Atomic Red Team, attack_range)
2. **Identify** coverage gaps by comparing results to ATT&CK technique coverage
3. **Engineer** new detections or improve existing analytics (DeTTECT, MITRE CAR)
4. **Deploy** countermeasures for techniques that cannot be reliably detected (D3FEND)
5. **Update** incident response playbooks for newly detected technique patterns (RE&CT)
6. **Re-test** to validate improvements

---

## Getting Started

### Prerequisites

- Familiarity with MITRE ATT&CK Navigator
- Access to a SIEM/log aggregation platform
- Python 3.8+ (for DeTTECT, attack_range tooling)
- Git (for version-controlled detection-as-code)

### Recommended Progression

```
Phase 1: Foundation
├── 01-Governance    → Establish charter, roles, authority
├── 02-Data-Docs     → Inventory and standardize data sources
└── 03-Threat-Intel  → Build initial threat profile

Phase 2: Build
├── 04-Detection-Eng → Develop initial detection analytics
└── 05-IR            → Create response playbooks

Phase 3: Harden & Validate
├── 06-Countermeasures → Map and deploy defensive techniques
└── 07-Testing         → Validate detection coverage

Phase 4: Mature
└── Continuous cycle through all modules
```

---

## Framework Reference Links

| Framework | Source |
|---|---|
| MITRE 11 Strategies | [11 Strategies of a World-Class Cybersecurity Operations Center](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf) |
| NIST CSF 2.0 | [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework) |
| ASD Cyber Skills Framework | [Australian Signals Directorate CSF](https://www.cyber.gov.au/) |
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
├── README.md                          # This file
├── 01-Governance/
│   └── README.md                      # NIST CSF 2.0, ASD CSF, DoDCWF 8140
├── 02-Data-Documentation/
│   └── README.md                      # OSSEM, Threat Hunters Playbook
├── 03-Threat-Intelligence/
│   └── README.md                      # MITRE ATT&CK integration
├── 04-Detection-Engineering/
│   └── README.md                      # DeTTECT, MITRE CAR, Atomic Red Team
├── 05-Incident-Response/
│   └── README.md                      # RE&CT Framework
├── 06-Countermeasures/
│   └── README.md                      # MITRE D3FEND
├── 07-Detection-Testing/
│   └── README.md                      # Atomic Red Team, attack_range.py
└── references/
    ├── maturity-model.md              # SOC maturity progression
    ├── workforce-roles.md             # DoDCWF & ASD role mapping
    └── tool-integration-matrix.md     # Framework interoperability guide
```

---

## License

This project is released for educational and defensive cybersecurity purposes. Individual frameworks and tools referenced retain their own licensing terms.
