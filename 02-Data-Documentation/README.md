# 02 — Data Documentation

> **MITRE SOC Strategy Addressed:**
> - Strategy 7: Select and Collect the Right Data

---

## Purpose

Data documentation is the prerequisite for every detection, hunt, and investigation in the SOC. If you cannot describe what your data looks like, you cannot write reliable analytics against it. This module uses two complementary projects to create a rigorous data foundation:

| Project | Role |
|---|---|
| **OSSEM (Open Source Security Events Metadata)** | Standardized schema and taxonomy for security event data |
| **Threat Hunters Playbook** | Data-driven hunt hypotheses linked to specific data sources and ATT&CK techniques |

---

## OSSEM Integration

### What is OSSEM?

OSSEM provides a common information model (CIM) and data dictionaries for security events across platforms (Windows, Linux, macOS, cloud). It answers the question: *"What fields does this event actually contain, and what do they mean?"*

### OSSEM Components Used in This Pipeline

```
OSSEM Project
├── Common Information Model (CIM)     → Normalize field names across sources
├── Data Dictionaries (DD)             → Per-source field documentation
│   ├── Windows (Security, Sysmon, PowerShell)
│   ├── Linux (Auditd, Syslog)
│   ├── Cloud (AWS CloudTrail, Azure AD, GCP)
│   └── Network (Zeek, Suricata, Firewall)
├── Detection Data Model (DDM)         → Map data to ATT&CK data sources
└── OSSEM Attack API                   → ATT&CK technique ↔ data source relationships
```

### Data Source Inventory Template

Before writing any detection, catalog what data you actually have. Use this template per source:

```markdown
## Data Source: [Source Name]

### Metadata
- **Platform:** Windows / Linux / Cloud / Network
- **Log Source:** Sysmon / Windows Security / Auditd / CloudTrail / etc.
- **Collection Method:** Agent / WEF / Syslog / API
- **SIEM Index:** [index name]
- **Retention Period:** [days]
- **Volume:** [estimated EPS]

### OSSEM Mapping
- **OSSEM Data Dictionary:** [link to OSSEM DD entry]
- **CIM Entity:** [process, file, network, registry, user, etc.]
- **Key Fields:**

| OSSEM CIM Field | Raw Field Name | Description |
|---|---|---|
| src_ip_addr | SourceAddress | Source IP of the connection |
| dst_ip_addr | DestinationAddress | Destination IP |
| process_name | Image | Full path of the executing process |
| process_command_line | CommandLine | Full command line arguments |
| user_name | SubjectUserName | Account that performed the action |

### ATT&CK Data Source Mapping
- **ATT&CK Data Source:** [e.g., DS0009 - Process]
- **Data Component:** [e.g., Process Creation]
- **Techniques Coverable:** [T1059, T1053, T1547, ...]

### Quality Assessment
- [ ] Events arriving consistently (no gaps > 15 min)
- [ ] Key fields are populated (not null/empty)
- [ ] Timestamps are normalized to UTC
- [ ] Field parsing validated against OSSEM CIM
- [ ] Volume baseline established
```

### Data Quality Scoring

Use this rubric to assess each data source before writing detections against it:

| Score | Level | Criteria |
|---|---|---|
| **5** | Production-Ready | Consistent ingestion, CIM-normalized, validated fields, baselined volume |
| **4** | Reliable | Consistent ingestion, most fields parsed, minor gaps |
| **3** | Usable | Ingested but field mapping incomplete or occasional gaps |
| **2** | Partial | Intermittent collection, significant field issues |
| **1** | Experimental | Ad-hoc collection, untested parsing |
| **0** | Not Collected | Data source identified but not onboarded |

**Rule:** Only write production detections against data scored 4 or 5. Score 3 data can support hunting hypotheses. Below 3, prioritize data onboarding before detection work.

---

## Threat Hunters Playbook Integration

### What is the Threat Hunters Playbook?

The Threat Hunters Playbook by the Open Threat Research (OTR) community provides Jupyter notebook-based hunt playbooks. Each playbook documents:

- The **ATT&CK technique** being hunted
- The **hypothesis** (what adversary behavior looks like in data)
- The **required data sources** (mapped to OSSEM)
- The **analytics** (queries in multiple SIEM languages)
- **Validation datasets** (pre-built datasets from Mordor/Security Datasets project)

### How It Feeds the Pipeline

```
Threat Hunters Playbook
        │
        ├──▶ Module 02 (This Module)
        │    - Required data sources per technique
        │    - Data quality requirements
        │    - Field-level requirements for analytics
        │
        ├──▶ Module 03 (Threat Intelligence)
        │    - ATT&CK technique prioritization
        │    - Adversary behavior patterns
        │
        ├──▶ Module 04 (Detection Engineering)
        │    - Pre-built analytics as detection starting points
        │    - Query templates (Sigma, KQL, SPL)
        │
        └──▶ Module 07 (Detection Testing)
             - Mordor/Security Datasets for offline testing
             - Simulation procedures for live testing
```

### Playbook-Driven Data Gap Analysis

Use the Threat Hunters Playbook to identify what data your SOC is missing:

1. **Select priority ATT&CK techniques** from Module 03 threat profile
2. **Find corresponding playbooks** in the Threat Hunters Playbook
3. **Extract required data sources** from each playbook
4. **Compare against your data source inventory** (above)
5. **Score each gap**:

| Technique | Required Data Source | Current Score | Gap Action |
|---|---|---|---|
| T1059.001 (PowerShell) | Windows PowerShell Operational | 5 | None — fully operational |
| T1059.001 (PowerShell) | Script Block Logging | 0 | Enable via GPO, create OSSEM mapping |
| T1053.005 (Sched. Task) | Windows Security 4698 | 4 | Validate field parsing |
| T1071.001 (Web Protocols) | Zeek HTTP logs | 2 | Fix collection gaps, normalize fields |

---

## Data Documentation Workflow

### Step-by-Step Process

```
1. INVENTORY          2. DOCUMENT           3. NORMALIZE          4. VALIDATE
List all log      →   Map each source   →   Apply OSSEM CIM   →   Score quality
sources currently      to OSSEM Data         field naming to        per data source
collected              Dictionaries          SIEM parsing           rubric above
        │                    │                     │                     │
        ▼                    ▼                     ▼                     ▼
5. GAP ANALYSIS       6. PRIORITIZE         7. ONBOARD            8. BASELINE
Compare to TH     →   Rank gaps by     →   Deploy collection  →   Establish
Playbook required      ATT&CK technique      for new sources       volume/field
data sources           priority (Mod 03)                            baselines
```

### Data Source Priority Matrix

Cross-reference data source value against collection cost:

```
HIGH VALUE ─────────────────────────────────────┐
│                                               │
│  ★ Process Creation (Sysmon 1)                │
│  ★ PowerShell Script Block Logging            │
│  ★ DNS Query Logs                             │
│  ★ Authentication Events                       │
│  ★ Network Connection (Sysmon 3 / Zeek)       │
│  ★ File Creation (Sysmon 11)                  │
│                                               │
├───────────────────────────────────────────────┤
│  ■ Registry Modification (Sysmon 13)          │
│  ■ WMI Events (Sysmon 19-21)                  │
│  ■ Cloud API Calls (CloudTrail, AzureAD)      │
│  ■ Email Gateway Logs                         │
│                                               │
├───────────────────────────────────────────────┤
│  ○ Image Load (Sysmon 7)                      │
│  ○ Raw Named Pipe (Sysmon 17-18)              │
│  ○ Driver Load (Sysmon 6)                     │
│  ○ Full Packet Capture                        │
│                                               │
LOW VALUE ──────────────────────────────────────┘
     LOW COST ────────────────────── HIGH COST
```

Prioritize the upper-left quadrant: high detection value, lower collection cost.

---

## Outputs

| Output | Consumers | Description |
|---|---|---|
| Data Source Inventory | All modules | Complete catalog of available security data sources |
| OSSEM CIM Mapping | Module 04 (Detection) | Field normalization reference for analytics |
| Data Quality Scorecards | Module 04, 07 | Per-source quality ratings |
| Data Gap Analysis | Module 01 (Governance), Module 04 | Missing data sources mapped to ATT&CK techniques |
| Data Onboarding Backlog | SOC Engineering | Prioritized list of data sources to collect |

---

## Implementation Checklist

- [ ] Enumerate all currently collected log sources
- [ ] Create OSSEM-format data dictionary entry for each source
- [ ] Map raw field names to OSSEM CIM standard fields
- [ ] Score each data source using the quality rubric
- [ ] Download Threat Hunters Playbook and extract data requirements
- [ ] Run gap analysis: required data vs. collected data
- [ ] Prioritize gaps using the value/cost matrix
- [ ] Create onboarding tickets for high-priority missing sources
- [ ] Establish volume baselines for anomaly detection
- [ ] Schedule quarterly data quality reviews

---

## References

- [OSSEM Project](https://github.com/OTRF/OSSEM)
- [OSSEM Common Information Model](https://github.com/OTRF/OSSEM/tree/master/OSSEM-CIM)
- [Threat Hunters Playbook](https://github.com/OTRF/ThreatHunter-Playbook)
- [Security Datasets (Mordor)](https://github.com/OTRF/Security-Datasets)
- [ATT&CK Data Sources](https://attack.mitre.org/datasources/)
- [MITRE 11 Strategies — Strategy 7](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
