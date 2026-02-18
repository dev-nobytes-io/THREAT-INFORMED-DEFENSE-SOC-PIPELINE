# SOC Maturity Model

> Aligned to **SOC-CMM** (SOC Capability Maturity Model) and MITRE Strategy 11: Turn up the Dial Incrementally

This maturity model uses **SOC-CMM** as its primary assessment framework, mapping SOC-CMM's five domains (Business, People, Process, Technology, Services) to the seven pipeline modules. It provides a structured path from initial capability to world-class operations.

For the full SOC-CMM framework reference — including all domain aspects, assessment templates, and improvement roadmaps — see [soc-cmm.md](soc-cmm.md).

---

## Maturity Levels (SOC-CMM)

| Level | Name | Description |
|---|---|---|
| **0** | **Non-Existent** | No formal SOC capability |
| **1** | **Initial** | Ad-hoc, reactive operations; dependent on individual initiative |
| **2** | **Repeatable** | Basic processes established; key activities can be repeated |
| **3** | **Defined** | Standardised and proactive; documented procedures followed consistently |
| **4** | **Managed** | Measured and controlled; quantitative metrics drive decisions |
| **5** | **Optimising** | Continuous improvement; automated feedback loops; predictive capability |

---

## Per-Module Maturity Criteria

Each pipeline module is assessed against the relevant SOC-CMM domains. The criteria below integrate SOC-CMM aspects with the specific frameworks used in each module.

### 01 — Governance

**SOC-CMM Domains:** Business (primary), People (workforce planning)

| Level | Criteria |
|---|---|
| 1 | SOC exists informally; no charter; no dedicated budget; reactive staffing |
| 2 | SOC charter drafted; roles defined; basic reporting to management; initial budget allocated |
| 3 | Charter ratified with executive authority; DoDCWF role mapping; ASD/CIISec skill assessments; NIST CSF 2.0 alignment; multi-year budget; strategy document with maturity targets; regular stakeholder reporting; compliance requirements mapped |
| 4 | KPIs tracked monthly (MTTD, MTTR, coverage); workforce gap analysis drives hiring/training; budget tied to measurable outcomes; board-level reporting; SOC integrated into enterprise risk management |
| 5 | Predictive workforce planning; automated compliance reporting; risk-adaptive SOC structure; predictive budget modelling; SOC is a recognised business enabler |

### 02 — Data Documentation

**SOC-CMM Domains:** Technology (SIEM/Log Management, Log Source Coverage), Process (Procedures)

| Level | Criteria |
|---|---|
| 1 | Logs collected but not inventoried; no standard naming; raw vendor field names used in queries |
| 2 | Data source inventory exists; OSSEM-DD data dictionaries created for primary sources; basic field documentation |
| 3 | **Standardisation:** OSSEM-CDM `{prefix}_{attribute}` naming applied to all priority sources; SIEM parsing normalises at ingest. **Modelling:** Entity relationships documented for top 20 ATT&CK techniques; OSSEM-DM `Source→Verb→Target` patterns mapped. **Quality:** Data quality scored across completeness/consistency/timeliness; Threat Hunters Playbook data requirements mapped; hunt-readiness scores calculated |
| 4 | Data quality monitored continuously; CDM normalisation validated via cross-source queries; entity relationship gaps auto-detected; onboarding SLAs for new sources; techniques-to-events mapping maintained |
| 5 | Self-healing data pipelines; automated quality remediation; new data sources auto-standardised to CDM; entity relationships auto-discovered from telemetry; data source and relationship coverage auto-maps to ATT&CK |

### 03 — Threat Intelligence

**SOC-CMM Domains:** Services (Threat Intelligence), Process (Threat Intelligence Process)

| Level | Criteria |
|---|---|
| 1 | Ad-hoc IOC consumption; no formal threat profile |
| 2 | ATT&CK Navigator layer exists; basic threat group awareness; threat feeds received |
| 3 | Composite threat profile built from multiple groups; prioritised technique list drives detection and hunt operations; CTI intelligence cycle operational; regular CTI products produced; M3TID methodology active |
| 4 | Threat profile updated automatically from multiple feeds; coverage gaps quantified; intel drives budget and staffing decisions; community sharing active (ISACs) |
| 5 | Predictive intelligence; automated technique trending; real-time threat profile updates drive detection pipeline and hunt hypotheses; strategic intel influences business decisions |

### 04 — Detection Engineering

**SOC-CMM Domains:** Services (Monitoring & Detection), Process (Use Case Management, Automation), Technology (Detection Platform)

| Level | Criteria |
|---|---|
| 1 | Vendor default rules only; no custom analytics |
| 2 | Some custom Sigma rules; DeTTECT initial assessment done |
| 3 | Detection-as-code repository; CAR analytics integrated; DeTTECT coverage ≥ 50% of threat profile; sprint-based development; hunt findings routinely converted to production detections; pre-hunt analytics development methodology applied |
| 4 | DeTTECT coverage ≥ 80%; CI/CD detection pipeline deploys validated rules; FP rates tracked per rule; detection SLAs met; SOAR automation for high-confidence detections; hunt-to-detection pipeline formalised |
| 5 | Automated detection generation from threat intel; ML-augmented analytics; near-100% priority technique coverage; self-tuning analytics; detection effectiveness continuously measured |

### 05 — Incident Response

**SOC-CMM Domains:** Services (Incident Response, Forensics), Process (Incident Management)

| Level | Criteria |
|---|---|
| 1 | Unstructured response; no playbooks; tribal knowledge |
| 2 | Basic playbooks for common scenarios; escalation matrix defined |
| 3 | RE&CT-mapped playbooks for all Critical/High techniques; tabletops conducted quarterly; forensic capability available; post-incident reviews systematically update detections and threat profiles |
| 4 | Playbooks integrated with SOAR; MTTR tracked and improving; lessons learned systematically fed back to all modules; evidence handling meets legal standards |
| 5 | Automated containment for high-confidence detections; adaptive playbooks; response time targets consistently met; predictive response preparation based on threat landscape |

### 06 — Countermeasures

**SOC-CMM Domains:** Services (Vulnerability Management), Technology (Endpoint, Network)

| Level | Criteria |
|---|---|
| 1 | Default OS/vendor configurations; no D3FEND mapping |
| 2 | Basic hardening applied; some D3FEND techniques identified |
| 3 | D3FEND mapping for all low-detection-score techniques; countermeasures deployed for top gaps; deception layer active |
| 4 | All countermeasures validated via testing (Module 07); block rates measured; defence-in-depth coverage mapped |
| 5 | Adaptive countermeasures auto-deploy based on threat activity; comprehensive deception layer; near-zero exploitable gaps |

### 07 — Detection Testing

**SOC-CMM Domains:** Services (Detection Testing), Process (Analytics & Metrics, Continuous Improvement)

| Level | Criteria |
|---|---|
| 1 | No formal testing; assume detections work |
| 2 | Occasional Atomic Red Team runs; ad-hoc validation |
| 3 | Monthly coverage scans; attack_range deployed; test results update DeTTECT; quarterly purple team; KPIs tracked (coverage ratio, MTTD, FP rate) |
| 4 | Weekly automated smoke tests; CI/CD detection validation; coverage dashboard drives decisions; KPIs baselined and trending; continuous hunt validation |
| 5 | Continuous automated testing pipeline; detection regressions caught in CI; predictive gap analysis; red team on staff; testing integrated into every detection deployment |

### 08 — Forensics & DFIR

**SOC-CMM Domains:** Services (Forensics & Investigation), Process (Procedures), Technology (Testing Infrastructure)

| Level | Criteria |
|---|---|
| 1 | No forensic capability; evidence destroyed during incident response; ad-hoc tool usage |
| 2 | Basic disk imaging capability; some artefact parsing; incident-driven only; one trained analyst |
| 3 | Full acquisition capability (memory + disk + triage); chain of custody enforced; artefact parsing with standard tools (EZ Tools, Hayabusa); forensic findings feed back to Modules 03, 04, 07; pre-collection readiness achieved; forensic SOPs documented |
| 4 | Enterprise-scale triage (Velociraptor/GRR fleet-wide); automated artefact parsing and timeline generation (Plaso, Timesketch); forensic case management (DFIR-IRIS); all findings systematically ATT&CK-mapped; tool validation program; legal coordination tested |
| 5 | Predictive forensic readiness based on threat landscape; automated evidence collection triggered by high-confidence detections; forensic-as-code (automated analysis pipelines); community sharing of forensic intelligence |

---

## SOC-CMM Domain Summary Assessment

This template provides the cross-cutting SOC-CMM view alongside the per-module view:

```markdown
## SOC Maturity Assessment — [Date]

### Assessor: [Name]
### Assessment Method: [Self-assessment / Peer review / External audit]

### Per-Module View

| Module | Current Level | Target Level | Gap | Priority Actions |
|---|---|---|---|---|
| 01 Governance | [0-5] | [0-5] | [delta] | [specific actions] |
| 02 Data Documentation | [0-5] | [0-5] | [delta] | [specific actions] |
| 03 Threat Intelligence | [0-5] | [0-5] | [delta] | [specific actions] |
| 04 Detection Engineering | [0-5] | [0-5] | [delta] | [specific actions] |
| 05 Incident Response | [0-5] | [0-5] | [delta] | [specific actions] |
| 06 Countermeasures | [0-5] | [0-5] | [delta] | [specific actions] |
| 07 Detection Testing | [0-5] | [0-5] | [delta] | [specific actions] |
| 08 Forensics & DFIR | [0-5] | [0-5] | [delta] | [specific actions] |

### SOC-CMM Domain View

| SOC-CMM Domain | Current Level | Target Level | Gap | Key Pipeline Modules |
|---|---|---|---|---|
| Business | [0-5] | [0-5] | [delta] | 01 |
| People | [0-5] | [0-5] | [delta] | 01, Workforce Roles |
| Process | [0-5] | [0-5] | [delta] | 03, 04, 05, 07, 08 |
| Technology | [0-5] | [0-5] | [delta] | 02, 04, 07 |
| Services | [0-5] | [0-5] | [delta] | All |

### Overall SOC Maturity: [Average / Lowest / Weighted]
### Next Assessment: [Date — recommended quarterly]
```

For the full SOC-CMM assessment template with per-aspect scoring, see [soc-cmm.md](soc-cmm.md).

---

## Progression Strategy

**Do not attempt to jump levels.** Each level builds on the previous:

```
Level 1 → 2: Establish fundamentals. Document what exists. Get repeatability.
Level 2 → 3: Standardise and align to frameworks. This is where the pipeline
             delivers maximum value — OSSEM, ATT&CK, DeTTECT, RE&CT, D3FEND,
             SOC-CMM, and M3TID all converge here.
Level 3 → 4: Measure everything. Let metrics drive improvement. Automate routine.
Level 4 → 5: Automate the feedback loop. Reduce human bottlenecks. Predict and adapt.
```

**Target Level 3 across all modules and all SOC-CMM domains before pushing any single area to Level 4+.** An unbalanced SOC (e.g., Level 4 Technology but Level 1 Process) is fragile.

---

## Dual Assessment Approach

This pipeline supports two complementary assessment perspectives:

| Perspective | Framework | View | When to Use |
|---|---|---|---|
| **Module View** | Pipeline-specific criteria | Vertical — per pipeline module (Governance, Data, Intel, Detection, IR, Countermeasures, Testing, Forensics) | For operational improvement planning — identifies which module needs investment next |
| **Domain View** | SOC-CMM | Horizontal — per SOC-CMM domain (Business, People, Process, Technology, Services) | For holistic capability assessment — ensures no domain is neglected |

**Both views should be assessed quarterly.** The module view drives operational sprint planning. The domain view drives strategic investment and organisational change.
