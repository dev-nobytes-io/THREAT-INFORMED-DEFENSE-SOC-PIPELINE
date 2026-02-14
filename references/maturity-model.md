# SOC Maturity Model

> Aligned to MITRE Strategy 11: Turn up the Dial Incrementally

This maturity model maps across all seven pipeline modules, providing a structured path from initial capability to world-class operations.

---

## Maturity Levels

| Level | Name | Description |
|---|---|---|
| **0** | **Non-Existent** | No formal SOC capability |
| **1** | **Initial** | Ad-hoc, reactive operations with minimal process |
| **2** | **Managed** | Documented processes, basic tools deployed |
| **3** | **Defined** | Standardized procedures, threat-informed operations |
| **4** | **Measured** | Quantitative metrics drive continuous improvement |
| **5** | **Optimizing** | Automated feedback loops, predictive defense |

---

## Per-Module Maturity Criteria

### 01 — Governance

| Level | Criteria |
|---|---|
| 1 | SOC exists informally; no charter; reactive staffing |
| 2 | SOC charter drafted; roles defined; basic reporting |
| 3 | Charter ratified with executive authority; DoDCWF role mapping; ASD skill assessments; NIST CSF alignment |
| 4 | KPIs tracked monthly (MTTD, MTTR, coverage); workforce gap analysis drives hiring/training; board-level reporting |
| 5 | Predictive workforce planning; automated compliance reporting; risk-adaptive SOC structure changes |

### 02 — Data Documentation

| Level | Criteria |
|---|---|
| 1 | Logs collected but not inventoried; no standard naming |
| 2 | Data source inventory exists; basic field documentation |
| 3 | OSSEM CIM applied to all sources; data quality scored; Threat Hunters Playbook data requirements mapped |
| 4 | Data quality monitored continuously; gaps auto-detected; onboarding SLAs for new sources |
| 5 | Self-healing data pipelines; automated quality remediation; data source coverage auto-maps to ATT&CK |

### 03 — Threat Intelligence

| Level | Criteria |
|---|---|
| 1 | Ad-hoc IOC consumption; no formal threat profile |
| 2 | ATT&CK Navigator layer exists; basic threat group awareness |
| 3 | Composite threat profile built; prioritized technique list drives detection; regular CTI products |
| 4 | Threat profile updated automatically from multiple feeds; coverage gaps quantified; intel drives budget decisions |
| 5 | Predictive intelligence; automated technique trending; real-time threat profile updates drive detection pipeline |

### 04 — Detection Engineering

| Level | Criteria |
|---|---|
| 1 | Vendor default rules only; no custom analytics |
| 2 | Some custom Sigma rules; DeTTECT initial assessment done |
| 3 | Detection-as-code repo; CAR analytics integrated; DeTTECT coverage ≥ 50% of threat profile; sprint-based development |
| 4 | DeTTECT coverage ≥ 80%; CI/CD detection pipeline; FP rates tracked per rule; detection SLAs met |
| 5 | Automated detection generation from threat intel; ML-augmented analytics; near-100% priority technique coverage |

### 05 — Incident Response

| Level | Criteria |
|---|---|
| 1 | Unstructured response; no playbooks; tribal knowledge |
| 2 | Basic playbooks for common scenarios; escalation matrix defined |
| 3 | RE&CT-mapped playbooks for all Critical/High techniques; tabletops conducted quarterly |
| 4 | Playbooks integrated with SOAR; MTTR tracked and improving; lessons learned systematically fed back |
| 5 | Automated containment for high-confidence detections; adaptive playbooks; response time targets consistently met |

### 06 — Countermeasures

| Level | Criteria |
|---|---|
| 1 | Default OS/vendor configurations; no D3FEND mapping |
| 2 | Basic hardening applied; some D3FEND techniques identified |
| 3 | D3FEND mapping for all low-detection-score techniques; countermeasures deployed for top gaps; deception layer active |
| 4 | All countermeasures validated via testing (Module 07); block rates measured; defense-in-depth coverage mapped |
| 5 | Adaptive countermeasures auto-deploy based on threat activity; comprehensive deception layer; near-zero exploitable gaps |

### 07 — Detection Testing

| Level | Criteria |
|---|---|
| 1 | No formal testing; assume detections work |
| 2 | Occasional Atomic Red Team runs; ad-hoc validation |
| 3 | Monthly coverage scans; attack_range deployed; test results update DeTTECT; quarterly purple team |
| 4 | Weekly automated smoke tests; CI/CD detection validation; coverage dashboard drives decisions; KPIs baselined |
| 5 | Continuous automated testing pipeline; detection regressions caught in CI; predictive gap analysis; red team on staff |

---

## Maturity Assessment Template

```markdown
## SOC Maturity Assessment — [Date]

### Assessor: [Name]
### Assessment Method: [Self-assessment / Peer review / External audit]

| Module | Current Level | Target Level | Gap | Priority Actions |
|---|---|---|---|---|
| 01 Governance | [0-5] | [0-5] | [delta] | [specific actions] |
| 02 Data Documentation | [0-5] | [0-5] | [delta] | [specific actions] |
| 03 Threat Intelligence | [0-5] | [0-5] | [delta] | [specific actions] |
| 04 Detection Engineering | [0-5] | [0-5] | [delta] | [specific actions] |
| 05 Incident Response | [0-5] | [0-5] | [delta] | [specific actions] |
| 06 Countermeasures | [0-5] | [0-5] | [delta] | [specific actions] |
| 07 Detection Testing | [0-5] | [0-5] | [delta] | [specific actions] |

### Overall SOC Maturity: [Average / Lowest / Weighted]
### Next Assessment: [Date — recommended quarterly]
```

---

## Progression Strategy

**Do not attempt to jump levels.** Each level builds on the previous:

```
Level 1 → 2: Establish fundamentals. Document what exists.
Level 2 → 3: Standardize and align to frameworks. This is where the pipeline delivers most value.
Level 3 → 4: Measure everything. Let metrics drive improvement.
Level 4 → 5: Automate the feedback loop. Reduce human bottlenecks.
```

Most organizations should target Level 3 as the initial goal across all modules before pushing any single module to Level 4+.
