# Tool Integration Matrix

> How all frameworks and tools in this pipeline interconnect
>
> **Organising Frameworks:** SOC-CMM (maturity assessment), M3TID (continuous hunt cycle)

---

## Integration Map

```
                           TOOL INTEGRATION FLOW

 ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
 │NIST CSF  │     │  OSSEM   │     │  ATT&CK  │     │ DeTTECT  │
 │  2.0     │     │          │     │          │     │          │
 │(Govern)  │     │(Data CIM)│     │(Threat)  │     │(Coverage)│
 └────┬─────┘     └────┬─────┘     └────┬─────┘     └────┬─────┘
      │                │                │                │
      │    Data        │   Technique    │   Score        │
      │    fields      │   IDs          │   data         │
      │                ▼                ▼                ▼
      │           ┌──────────────────────────────────────────┐
      │           │              MITRE CAR                    │
      │           │    (Analytics linked to ATT&CK +          │
      │           │     OSSEM data model)                     │
      └──────────▶│                                           │
                  └──────────────────┬───────────────────────┘
                                     │
                        Sigma rules  │  Detection logic
                                     ▼
                  ┌──────────────────────────────────────────┐
                  │           SIGMA RULES                     │
                  │    (Detection-as-code format)              │
                  └──────────────────┬───────────────────────┘
                                     │
                    Deploy to SIEM   │   Validate
                          ┌──────────┴──────────┐
                          ▼                     ▼
                  ┌──────────────┐     ┌──────────────┐
                  │    SIEM      │     │ ATOMIC RED   │
                  │  (Runtime)   │     │ TEAM         │
                  └──────────────┘     │(Test cases)  │
                          │            └──────┬───────┘
                          │                   │
                  Alert triggers       Test execution
                          │                   │
                          ▼                   ▼
                  ┌──────────────┐     ┌──────────────┐
                  │    RE&CT     │     │attack_range  │
                  │  (Response   │     │(Test infra)  │
                  │   actions)   │     └──────┬───────┘
                  └──────────────┘            │
                                              │
                                     Test results feed back
                                              │
                  ┌──────────────┐            │
                  │  D3FEND      │◀───────────┘
                  │(Counter-     │   Failed detections →
                  │ measures)    │   deploy countermeasures
                  └──────────────┘
```

---

## Pairwise Integration Details

### ATT&CK ↔ OSSEM (DD + CDM + DM)

| Integration Point | Direction | Mechanism |
|---|---|---|
| Data Sources | ATT&CK → OSSEM-DD | ATT&CK Data Sources (DS) specify what telemetry is needed; OSSEM-DD provides per-event field documentation |
| Data Components | ATT&CK → OSSEM-DM | ATT&CK Data Components (e.g., "Process Creation") map to OSSEM-DM entity relationships (e.g., `Process created Process`) |
| Field Normalisation | OSSEM-CDM → Analytics | OSSEM-CDM `{prefix}_{attribute}` naming convention provides standardised field names for all detection queries |
| Entity Relationships | OSSEM-DM → ATT&CK | OSSEM-DM relationship definitions were adopted into ATT&CK as the Data Component concept |
| Technique Coverage | OSSEM-DM → ATT&CK | `techniques_to_events_mapping.yaml` traces each ATT&CK technique through Data Source → Data Component → Relationship → Security Event |
| Data Quality | OSSEM-CDM → ATT&CK | CDM normalisation quality determines which techniques are reliably detectable |

**Practical use:** When Module 03 identifies T1059.001 as priority:
1. Look up ATT&CK Data Source DS0009 (Process) → Data Component "Process Creation"
2. Map to OSSEM-DM relationship: `Process created Process`
3. Identify required events: Sysmon Event 1, Security 4688
4. Map to OSSEM-CDM fields: `process_name`, `process_command_line`, `process_parent_name`
5. Verify fields are documented (OSSEM-DD) and quality-scored (Module 02) at ≥ 4

---

### OSSEM-CDM ↔ OSSEM-DM

| Integration Point | Direction | Mechanism |
|---|---|---|
| Entity Definitions | CDM → DM | DM relationships reference CDM entity types (process, file, user, etc.) |
| Field Names | CDM → DM → Analytics | CDM `{prefix}_{attribute}` names are the fields used in relationship-based analytics |
| Schema Tables | CDM → SIEM | CDM schema tables define the normalised tables that DM relationships query against |

---

### OSSEM-CDM ↔ SIEM Platforms

| Integration Point | Direction | Mechanism |
|---|---|---|
| Splunk CIM | CDM → Splunk | CDM field names map to Splunk CIM via `props.conf` field aliases |
| Elastic ECS | CDM → Elastic | Elastic Common Schema aligns with CDM entity/attribute structure |
| Microsoft Sentinel ASIM | CDM → Sentinel | Advanced Security Information Model natively aligns with OSSEM CDM |
| Chronicle UDM | CDM → Chronicle | Unified Data Model field mappings from CDM conventions |

---

### ATT&CK ↔ DeTTECT

| Integration Point | Direction | Mechanism |
|---|---|---|
| Technique IDs | ATT&CK → DeTTECT | DeTTECT YAML files reference ATT&CK technique IDs |
| Group Definitions | ATT&CK → DeTTECT | DeTTECT imports ATT&CK group technique lists for overlay analysis |
| Data Source Mapping | ATT&CK → DeTTECT | DeTTECT data_sources.yaml maps to ATT&CK data source framework |
| Navigator Layers | DeTTECT → ATT&CK Navigator | DeTTECT outputs .json layers viewable in ATT&CK Navigator |

---

### ATT&CK ↔ MITRE CAR

| Integration Point | Direction | Mechanism |
|---|---|---|
| Technique Mapping | ATT&CK → CAR | Every CAR analytic maps to one or more ATT&CK technique IDs |
| Data Model | OSSEM-adjacent → CAR | CAR uses its own data model (process, flow, file) similar to OSSEM CIM |
| Analytics | CAR → SIEM | CAR provides pseudocode + SIEM-native queries (SPL, KQL) |

---

### ATT&CK ↔ Atomic Red Team

| Integration Point | Direction | Mechanism |
|---|---|---|
| Technique IDs | ATT&CK → Atomic RT | Atomic tests organized by ATT&CK technique ID (T-code) |
| Test Procedures | Atomic RT → Testing | Each atomic test simulates one technique/sub-technique |
| Results | Testing → DeTTECT | Test pass/fail updates DeTTECT detection scores |

---

### ATT&CK ↔ RE&CT

| Integration Point | Direction | Mechanism |
|---|---|---|
| Technique Triggers | ATT&CK → RE&CT | Response playbooks triggered by ATT&CK technique detections |
| Response Actions | RE&CT → Playbooks | RE&CT action IDs provide structured response steps |
| Lessons Learned | RE&CT → ATT&CK profile | Incident findings update the threat profile (Module 03) |

---

### ATT&CK ↔ D3FEND

| Integration Point | Direction | Mechanism |
|---|---|---|
| Offensive Techniques | ATT&CK → D3FEND | D3FEND maps defensive techniques to ATT&CK offensive techniques |
| Countermeasure Selection | D3FEND → Hardening | D3FEND recommends specific defensive actions per technique |
| Digital Artifacts | D3FEND ↔ OSSEM | D3FEND digital artifacts connect to OSSEM data entities |

---

### DeTTECT ↔ Atomic Red Team

| Integration Point | Direction | Mechanism |
|---|---|---|
| Coverage Gaps | DeTTECT → Atomic RT | Low DeTTECT scores identify which techniques need testing |
| Score Validation | Atomic RT → DeTTECT | Test results validate or correct DeTTECT scores |
| Continuous Loop | Bidirectional | Test → Score → Improve → Re-test |

---

### Atomic Red Team ↔ attack_range

| Integration Point | Direction | Mechanism |
|---|---|---|
| Test Execution | Atomic RT → attack_range | attack_range can invoke Atomic tests via `simulate` command |
| Infrastructure | attack_range → Atomic RT | attack_range provides the lab environment for safe test execution |
| Log Collection | attack_range → Splunk | attack_range auto-ships logs to built-in Splunk instance |

---

### SOC-CMM ↔ All Pipeline Modules

| Integration Point | Direction | Mechanism |
|---|---|---|
| Maturity Assessment | SOC-CMM → All modules | Five-domain assessment (Business, People, Process, Technology, Services) identifies gaps across the entire pipeline |
| Improvement Prioritisation | SOC-CMM → Roadmap | Domain scores determine which pipeline modules need investment; Business and People gaps addressed before Technology |
| Service Evaluation | SOC-CMM Services → Modules 03-07 | Services domain aspects map directly to pipeline module outputs (monitoring, IR, CTI, hunting, testing) |
| People Evaluation | SOC-CMM People → Workforce Roles | People domain aspects map to DoDCWF/ASD/CIISec three-framework workforce assessment |
| Quarterly Review | SOC-CMM ↔ All modules | SOC-CMM assessment drives quarterly maturity review and sprint planning |

---

### M3TID ↔ Pipeline Modules

| Integration Point | Direction | Mechanism |
|---|---|---|
| Analyse Threats | M3TID → Module 03 | CTI-driven threat profiling produces ATT&CK-mapped composite threat profile |
| Assess Defenses | M3TID → Modules 04, 07 | DeTTECT coverage scoring and Atomic RT validation assess current defensive posture |
| Identify Gaps | M3TID → Modules 02, 04, 06 | Coverage gaps, data gaps, and process gaps identified through continuous assessment |
| Improve Defenses | M3TID → Modules 04, 05, 06 | New detections, playbooks, and countermeasures deployed to close gaps |
| Share Findings | M3TID → Module 01, External | Community sharing via ISACs, peer collaboration, open-source contributions |
| Pre-Hunt Data Mgmt | M3TID/THP → Module 02 | Four data management disciplines (DD, CDM, DM, Quality) provide the data foundation |
| Hypothesis Generation | M3TID/THP → Module 03 | Intelligence, situational, and analytics-driven hunt hypotheses generated from threat profile |
| Analytics Development | M3TID/THP → Module 04 | Hunt queries promoted to validated analytics then to production detections via CI/CD |
| Continuous Hunt | M3TID → All modules | The everlasting baseline hunt — explicit (analyst) and implicit (automated detections) running continuously |

---

## Data Flow Summary

| From | To | What Flows |
|---|---|---|
| NIST CSF 2.0 | All modules | Governance categories, risk context, authority |
| ASD CSF / DoDCWF | Module 01, HR | Workforce roles, skill requirements, training needs |
| OSSEM-DD | Module 02 | Per-event field documentation (data dictionaries) |
| OSSEM-CDM | Modules 02, 04 | Normalised field names (`{prefix}_{attribute}`), schema tables for SIEM parsing |
| OSSEM-DM | Modules 02, 03, 04, 05 | Entity relationships, ATT&CK data component mappings, techniques-to-events mapping |
| ATT&CK | Modules 03-07 | Technique IDs, group profiles, data sources, data components |
| Threat Hunters Playbook | Modules 02, 04 | Four data management disciplines, hunt hypotheses, analytics templates, data requirements |
| Security Datasets (Mordor) | Modules 02, 07 | Pre-recorded security events for CDM validation and detection testing |
| DeTTECT | Modules 04, 06, 07 | Coverage scores, gap analysis, Navigator layers |
| MITRE CAR | Module 04 | Pre-built analytics, query templates |
| Atomic Red Team | Modules 04, 07 | Test procedures, validation results |
| attack_range | Module 07 | Lab infrastructure, scenario simulation, Splunk data |
| RE&CT | Module 05 | Response action taxonomy, playbook structure |
| D3FEND | Module 06 | Defensive technique mappings, countermeasure specifications |
| SOC-CMM | All modules | Five-domain maturity assessment; improvement prioritisation; quarterly review cycle |
| M3TID / THP Pre-Hunt | Modules 02, 03, 04 | Continuous hunt cycle; six pre-hunt activities; hypothesis generation; hunt-to-detection pipeline |
| CIISec Skills Framework | Module 01, Workforce Roles | Specialism-based capability differentiation complementing ASD skill streams |

---

## Toolchain Installation Summary

| Tool | Install Method | Primary Use |
|---|---|---|
| DeTTECT | `pip install DeTTECT` | Coverage scoring and gap analysis |
| Sigma CLI | `pip install sigma-cli` | Detection-as-code conversion |
| Atomic Red Team (PS) | `Install-Module invoke-atomicredteam` | Windows technique testing |
| atomic-operator (Python) | `pip install atomic-operator` | Cross-platform technique testing |
| attack_range | `git clone` + `pip install -r requirements.txt` | Full lab provisioning |
| ATT&CK Navigator | Web-based or `git clone` | Technique visualization |
| OSSEM-DD | Reference ([GitHub](https://github.com/OTRF/OSSEM-DD)) | Per-event data dictionaries |
| OSSEM-CDM | Reference ([GitHub](https://github.com/OTRF/OSSEM-CDM)) | Common Data Model — entity schemas, field naming, schema tables |
| OSSEM-DM | Reference ([GitHub](https://github.com/OTRF/OSSEM-DM)) | Detection Model — entity relationships, ATT&CK data component mapping |
| Threat Hunters Playbook | Reference ([GitHub](https://github.com/OTRF/ThreatHunter-Playbook)) | Hunt playbooks, data management methodology, analytics templates |
| Security Datasets | Reference ([GitHub](https://github.com/OTRF/Security-Datasets)) | Pre-recorded security events for validation |
| SOC-CMM | Reference ([soc-cmm.com](https://www.soc-cmm.com)) | SOC maturity assessment — five-domain self-assessment framework |
| M3TID / CTID | Reference ([ctid.mitre-engenuity.org](https://ctid.mitre-engenuity.org/)) | Methodology for Threat-Informed Defense — continuous improvement cycle |
| CIISec Skills Framework | Reference ([ciisec.org](https://www.ciisec.org/skills-framework)) | Specialism-based cybersecurity workforce capability assessment |
