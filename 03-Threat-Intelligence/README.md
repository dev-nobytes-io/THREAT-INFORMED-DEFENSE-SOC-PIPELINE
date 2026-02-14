# 03 — Threat Intelligence

> **MITRE SOC Strategy Addressed:**
> - Strategy 6: Illuminate Adversaries with Cyber Threat Intelligence

---

## Purpose

Threat intelligence transforms raw threat data into actionable context that drives every other pipeline module. In this pipeline, **MITRE ATT&CK** is the central intelligence framework — it provides the common language for describing adversary behavior and the structure for prioritizing detection, response, and countermeasure efforts.

---

## MITRE ATT&CK as the Intelligence Backbone

### ATT&CK Structure

```
ATT&CK Enterprise Matrix
├── Tactics (14)          WHY an adversary acts (the goal)
│   ├── Reconnaissance
│   ├── Resource Development
│   ├── Initial Access
│   ├── Execution
│   ├── Persistence
│   ├── Privilege Escalation
│   ├── Defense Evasion
│   ├── Credential Access
│   ├── Discovery
│   ├── Lateral Movement
│   ├── Collection
│   ├── Command and Control
│   ├── Exfiltration
│   └── Impact
│
├── Techniques (~200)     HOW an adversary achieves a tactic
│   └── Sub-techniques    Specific implementations of a technique
│
├── Groups (~140)         WHO — named threat actor clusters
│
├── Software (~680)       WHAT tools/malware adversaries use
│
└── Data Sources          WHERE to look in your telemetry
```

### How ATT&CK Feeds Each Pipeline Module

| Pipeline Module | ATT&CK Usage |
|---|---|
| 01-Governance | Risk-based prioritization of techniques to defend against |
| 02-Data Documentation | ATT&CK Data Sources → identify required telemetry |
| 04-Detection Engineering | Techniques → analytics targets; DeTTECT coverage scoring |
| 05-Incident Response | Techniques → RE&CT response action mapping |
| 06-Countermeasures | Techniques → D3FEND defensive technique mapping |
| 07-Detection Testing | Techniques → Atomic Red Team test selection |

---

## Threat Profiling Process

### Step 1: Identify Relevant Threat Actors

Start by identifying which ATT&CK Groups are relevant to your organization based on:

- **Industry vertical** (e.g., healthcare, finance, defense, energy)
- **Geographic region** (nation-state targeting patterns)
- **Asset type** (cloud-heavy, OT/ICS, traditional enterprise)
- **Historical incidents** (what has actually hit your organization or sector)

```markdown
## Threat Profile: [Organization Name]

### Sector: [e.g., Financial Services]
### Region: [e.g., North America]

### Priority Threat Groups:

| Group | ATT&CK ID | Motivation | Relevance |
|---|---|---|---|
| APT38 | G0082 | Financial theft | Direct sector targeting |
| FIN7 | G0046 | Financial theft | Active campaigns against sector |
| APT29 | G0016 | Espionage | Supply chain risk |
| Wizard Spider | G0102 | Ransomware | Cross-sector targeting |
```

### Step 2: Extract Technique Sets

For each priority threat group, pull their known techniques from ATT&CK:

```
Group: APT38 (G0082)
├── T1566.001 - Spearphishing Attachment
├── T1059.001 - PowerShell
├── T1053.005 - Scheduled Task
├── T1071.001 - Web Protocols (C2)
├── T1486     - Data Encrypted for Impact
├── T1560.001 - Archive via Utility
└── ... (full list from ATT&CK)
```

### Step 3: Build a Composite Threat Profile

Merge technique lists from all priority groups into a single composite heat map. Techniques used by multiple relevant groups receive higher priority:

| Technique | T-ID | APT38 | FIN7 | APT29 | Wizard Spider | Count | Priority |
|---|---|---|---|---|---|---|---|
| Spearphishing Attachment | T1566.001 | X | X | X | X | 4 | Critical |
| PowerShell | T1059.001 | X | X | X | X | 4 | Critical |
| Scheduled Task | T1053.005 | X | X | | X | 3 | High |
| Valid Accounts | T1078 | | X | X | X | 3 | High |
| Web Protocols (C2) | T1071.001 | X | X | X | | 3 | High |
| OS Credential Dumping | T1003 | X | | X | X | 3 | High |
| Remote Services | T1021 | | X | X | X | 3 | High |
| Ingress Tool Transfer | T1105 | X | X | | | 2 | Medium |

### Step 4: ATT&CK Navigator Layer

Export the composite threat profile as an ATT&CK Navigator layer for visualization and sharing:

```json
{
    "name": "Composite Threat Profile - [Org Name]",
    "versions": {
        "attack": "14",
        "navigator": "4.9",
        "layer": "4.5"
    },
    "domain": "enterprise-attack",
    "description": "Priority techniques based on threat group analysis",
    "techniques": [
        {
            "techniqueID": "T1566.001",
            "tactic": "initial-access",
            "score": 100,
            "color": "#ff0000",
            "comment": "Used by APT38, FIN7, APT29, Wizard Spider"
        },
        {
            "techniqueID": "T1059.001",
            "tactic": "execution",
            "score": 100,
            "color": "#ff0000",
            "comment": "Used by APT38, FIN7, APT29, Wizard Spider"
        }
    ]
}
```

Save Navigator layers as `.json` files in this module directory for import into ATT&CK Navigator.

---

## Intelligence-Driven Prioritization

### The Prioritization Formula

Not all techniques are equally important. Use this multi-factor scoring to prioritize:

```
Priority Score = (Threat Frequency × 3) + (Impact × 2) + (Detection Feasibility × 1)

Where:
  Threat Frequency  = How many relevant groups use this technique (1-5)
  Impact            = Potential damage if technique succeeds (1-5)
  Detection Feasibility = How detectable given current data (1-5, inverted:
                          5 = easy to detect, 1 = very hard)
```

| Factor | Weight | Rationale |
|---|---|---|
| Threat Frequency | 3x | Techniques used by more groups are more likely to be encountered |
| Impact | 2x | High-impact techniques (ransomware, data theft) warrant more investment |
| Detection Feasibility | 1x | Factor in effort — easy wins matter, but don't ignore hard problems |

### Priority Tiers

| Tier | Score Range | Action |
|---|---|---|
| **Critical** | 25-30 | Immediate detection engineering + response playbook + countermeasure |
| **High** | 18-24 | Detection engineering + response playbook |
| **Medium** | 12-17 | Detection engineering (next sprint) |
| **Low** | 6-11 | Monitor via threat intel feeds, plan for future |

---

## CTI Operations Workflow

```
┌─────────────────┐
│  COLLECTION      │  Feeds: OSINT, ISACs, vendor reports, ATT&CK updates
└────────┬────────┘
         ▼
┌─────────────────┐
│  PROCESSING      │  Normalize to ATT&CK technique IDs
└────────┬────────┘  Map indicators to techniques where possible
         ▼
┌─────────────────┐
│  ANALYSIS        │  Update composite threat profile
└────────┬────────┘  Re-score priority techniques
         ▼
┌─────────────────┐
│  DISSEMINATION   │  Navigator layer → Detection Eng (Module 04)
└────────┬────────┘  Priority changes → Governance (Module 01)
         │           New techniques → IR Playbooks (Module 05)
         ▼
┌─────────────────┐
│  FEEDBACK        │  Detection test results (Module 07) feed back
└─────────────────┘  Incident findings update threat profile
```

### Intelligence Products

| Product | Audience | Cadence | Content |
|---|---|---|---|
| **ATT&CK Coverage Brief** | SOC Analysts, Detection Engineers | Monthly | Current detection coverage vs. threat profile |
| **Threat Landscape Update** | SOC Manager, CISO | Quarterly | Changes in threat actor activity, new techniques |
| **Technique Deep Dive** | Detection Engineers, Hunters | As needed | Detailed analysis of a single technique for analytics development |
| **Incident Intel Report** | All SOC, IR, Management | Per incident | ATT&CK-mapped findings from real incidents |

---

## Outputs

| Output | Consumers | Description |
|---|---|---|
| Composite Threat Profile | All modules | Prioritized list of ATT&CK techniques relevant to the organization |
| ATT&CK Navigator Layers | Module 04, 07 | Visual heat maps for coverage analysis |
| Prioritized Technique List | Module 04 (Detection), Module 05 (IR) | Ordered list driving detection and playbook development |
| Data Source Requirements | Module 02 (Data) | ATT&CK data sources needed for priority techniques |
| Intelligence Products | Module 01 (Governance) | Regular reporting for oversight and risk management |

---

## Implementation Checklist

- [ ] Identify threat actors relevant to organization (sector, region, assets)
- [ ] Pull technique lists for each group from ATT&CK
- [ ] Build composite threat profile with frequency counts
- [ ] Score and tier all techniques using the prioritization formula
- [ ] Generate ATT&CK Navigator layer (.json) for the threat profile
- [ ] Cross-reference with Module 02 data sources — identify intel-driven data gaps
- [ ] Establish CTI collection feeds (OSINT, ISACs, vendor)
- [ ] Define intelligence product cadence and distribution list
- [ ] Create feedback mechanism from Module 07 testing results
- [ ] Schedule quarterly threat profile reviews

---

## References

- [MITRE ATT&CK](https://attack.mitre.org/)
- [ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/)
- [ATT&CK Groups](https://attack.mitre.org/groups/)
- [ATT&CK Data Sources](https://attack.mitre.org/datasources/)
- [MITRE 11 Strategies — Strategy 6](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
