# SOC Metrics & Self-Assessment Templates

> KPIs, KRIs, and operational metrics aligned to SOC-CMM domains and pipeline modules
>
> **Principle:** Measure what matters. Every metric should drive a decision. If a metric doesn't change behaviour, stop measuring it.

---

## How to Use This Document

1. **Select metrics** relevant to your current maturity level — don't try to measure everything at Level 2
2. **Establish baselines** before setting targets — you need to know where you are before deciding where to go
3. **Review metrics at the cadence specified** — stale metrics are worse than no metrics
4. **Use the self-assessment scorecard** quarterly to track SOC-CMM domain progression

---

## Metric Types

| Type | Abbreviation | Purpose | Audience |
|---|---|---|---|
| **Key Performance Indicator** | KPI | Measures operational effectiveness — "How well are we doing?" | SOC Manager, CISO |
| **Key Risk Indicator** | KRI | Measures risk exposure — "How exposed are we?" | CISO, Risk Committee |
| **Operational Metric** | OPS | Measures day-to-day operational health — "Is the machine running?" | SOC Team, Shift Leads |

---

## SOC-CMM Domain: Business (Module 01 — Governance)

### KPIs

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **SOC Budget Utilisation** | (Actual spend / Approved budget) x 100 | Finance system | > 80% | > 90% | 95-105% | Monthly |
| **Governance Reporting Cadence** | Number of governance reports delivered on schedule / Total due | Report tracker | 4/year | 12/year (monthly) | 12/year + ad-hoc | Quarterly |
| **Stakeholder Satisfaction** | Survey score from key stakeholders (1-5 scale) | Stakeholder survey | N/A | >= 3.0 | >= 4.0 | Quarterly |
| **Compliance Coverage** | (Regulatory requirements mapped to SOC controls) / (Total regulatory requirements) | GRC tool / spreadsheet | > 50% | > 80% | > 95% | Quarterly |

### KRIs

| Metric | Definition | Data Source | Threshold | Frequency |
|---|---|---|---|---|
| **Unfunded SOC Requirements** | Count of identified SOC needs without budget allocation | Requirements tracker | < 5 critical items | Quarterly |
| **Governance Gap Score** | SOC-CMM Business domain score delta (target - current) | SOC-CMM assessment | < 1.0 gap | Quarterly |

---

## SOC-CMM Domain: People (Workforce Roles)

### KPIs

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Staffing Fill Rate** | (Filled SOC positions / Approved positions) x 100 | HR system | > 70% | > 85% | > 95% | Monthly |
| **Analyst Turnover Rate** | (Analysts departed in 12 months / Average headcount) x 100 | HR system | < 30% | < 20% | < 15% | Quarterly |
| **Training Hours per Analyst** | Average training hours completed per analyst per quarter | Training tracker | > 10 hrs/qtr | > 20 hrs/qtr | > 40 hrs/qtr | Quarterly |
| **Certification Completion Rate** | (Analysts with role-required certs) / (Total analysts requiring certs) | Certification tracker | > 30% | > 60% | > 80% | Quarterly |
| **Cross-Training Coverage** | (Analysts trained in >= 2 pipeline modules) / (Total analysts) | Training tracker | N/A | > 30% | > 60% | Quarterly |
| **CIISec Specialism Coverage** | (SOC roles with CIISec specialism assessment) / (Total SOC roles) | Workforce assessment | N/A | > 50% | > 80% | Annually |

### KRIs

| Metric | Definition | Data Source | Threshold | Frequency |
|---|---|---|---|---|
| **Single Points of Failure** | Number of critical capabilities dependent on one person | Skill matrix | 0 | Quarterly |
| **Burnout Risk** | Analysts averaging > 50 hrs/week over rolling 4-week period | Time tracking | 0 | Monthly |
| **Shift Coverage Gaps** | Hours/week without minimum analyst staffing | Scheduling system | 0 hrs | Weekly |

---

## SOC-CMM Domain: Process (Modules 03, 04, 05, 07)

### KPIs

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Playbook Coverage** | (Techniques with RE&CT playbooks) / (Priority techniques in threat profile) x 100 | Playbook repo | > 30% | > 70% | > 90% | Monthly |
| **Detection Rule Count** | Total active detection rules (Sigma + SIEM-native) in production | Detection repo / SIEM | Baseline | +20% YoY | +10% YoY (quality > quantity) | Monthly |
| **Detection CI/CD Pipeline Uptime** | (Successful detection deployments / Total attempted) x 100 | CI/CD system | N/A | > 80% | > 95% | Per sprint |
| **Threat Profile Currency** | Days since last threat profile update (Module 03) | Threat profile repo | < 180 days | < 90 days | < 30 days | Monthly |
| **Hunt Hypothesis Completion Rate** | (Hunts completed / Hunts planned) per quarter | Hunt tracker | N/A | > 50% | > 80% | Quarterly |
| **Hunt-to-Detection Conversion** | (Hunt findings converted to production detections) / (Total actionable findings) x 100 | Detection repo | N/A | > 30% | > 60% | Quarterly |
| **Post-Incident Review Rate** | (Incidents with completed PIR) / (Total incidents at Medium+ severity) x 100 | Case management | > 30% | > 80% | 100% | Monthly |
| **Lessons Learned Implementation** | (PIR action items completed) / (Total PIR action items) x 100 | Action tracker | N/A | > 50% | > 80% | Quarterly |

### KRIs

| Metric | Definition | Data Source | Threshold | Frequency |
|---|---|---|---|---|
| **Stale Detections** | Detection rules not reviewed or tested in > 180 days | Detection repo metadata | < 10% of rule base | Monthly |
| **Playbook Staleness** | Playbooks not updated in > 365 days | Playbook repo metadata | < 20% of playbooks | Quarterly |
| **Process Documentation Gap** | (Core SOC processes without documented SOPs) / (Total core processes) | Documentation audit | < 20% | Quarterly |

---

## SOC-CMM Domain: Technology (Modules 02, 04, 07)

### KPIs

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Log Source Coverage** | (Data sources actively ingested) / (Data sources required by threat profile) x 100 | Module 02 inventory vs SIEM | > 50% | > 75% | > 90% | Monthly |
| **CDM Normalisation Rate** | (Log sources normalised to CDM) / (Total ingested sources) x 100 | SIEM parsing config | > 20% | > 60% | > 85% | Monthly |
| **Data Quality Score (Avg)** | Average of 5 OSSEM quality dimensions across priority sources (0-5) | Module 02 quality assessment | >= 2.0 | >= 3.0 | >= 4.0 | Quarterly |
| **SIEM Ingestion Health** | (Hours with no data gaps) / (Total hours) x 100 | SIEM monitoring | > 90% | > 95% | > 99% | Daily |
| **EDR Coverage** | (Endpoints with EDR deployed) / (Total managed endpoints) x 100 | EDR console | > 50% | > 80% | > 95% | Monthly |
| **SOAR Automation Rate** | (Incidents with at least 1 automated action) / (Total incidents) x 100 | SOAR platform | N/A | > 20% | > 50% | Monthly |
| **Tool Integration Score** | (Tool pairs with API integration) / (Total planned integrations) x 100 | Integration map | > 30% | > 60% | > 80% | Quarterly |

### KRIs

| Metric | Definition | Data Source | Threshold | Frequency |
|---|---|---|---|---|
| **Data Source Blindness** | Critical ATT&CK data sources with 0% collection | DeTTECT data_sources.yaml | 0 critical blind spots | Monthly |
| **SIEM License Consumption** | Current daily ingestion volume vs licensed/budgeted capacity | SIEM admin | < 80% capacity | Weekly |
| **Unmonitored Network Segments** | Network segments with no NSM or log collection | Network inventory | 0 critical segments | Quarterly |

---

## SOC-CMM Domain: Services (All Modules)

### Detection & Monitoring (Module 04)

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Detection Coverage Ratio** | (Techniques with DeTTECT score >= 3) / (Total priority techniques) x 100 | DeTTECT | > 20% | > 50% | > 80% | Monthly |
| **Mean Time to Detect (MTTD)** | Avg time from adversary action to first SIEM alert | SIEM + test results | Baseline | < 15 min | < 5 min | Per test cycle |
| **Alert Volume** | Total alerts per day/week (monitor for trending) | SIEM | Baseline | Trending down (tuning) | Stable (optimised) | Daily |
| **True Positive Rate** | (True positive alerts) / (Total alerts triaged) x 100 | Case management | > 20% | > 40% | > 60% | Weekly |
| **False Positive Rate per Rule** | (FP alerts from rule) / (Total alerts from rule) x 100 | SIEM analytics | < 80% per rule | < 50% per rule | < 20% per rule | Monthly |
| **Alert Backlog** | Alerts awaiting triage at end of shift | SIEM queue | Not growing | < 4 hrs old | < 1 hr old | Per shift |

### Incident Response (Module 05)

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Mean Time to Respond (MTTR)** | Avg time from alert to first containment action | Case management | Baseline | < 4 hrs (High) | < 1 hr (High) | Monthly |
| **Mean Time to Contain (MTTC)** | Avg time from first response to full containment | Case management | Baseline | < 24 hrs | < 8 hrs | Monthly |
| **Mean Time to Recover (MTTRec)** | Avg time from containment to restored operations | Case management | Baseline | < 72 hrs | < 24 hrs | Monthly |
| **Incident Escalation Accuracy** | (Correctly escalated / Total escalated) x 100 | Case review | > 50% | > 70% | > 85% | Monthly |
| **Incidents per Analyst** | Monthly incident load per analyst (monitor for burnout) | Case management | Trending awareness | < 15 Medium+/month | < 10 Medium+/month | Monthly |

### Threat Intelligence (Module 03)

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Intel Products Produced** | Number of CTI products delivered per quarter | CTI tracker | > 1/qtr | > 4/qtr | > 8/qtr | Quarterly |
| **Intel Actionability Rate** | (Intel products that led to detection/hunt action) / (Total intel products) x 100 | CTI + Detection trackers | N/A | > 40% | > 70% | Quarterly |
| **Indicator Freshness** | Median age of active IoCs in TIP | TIP | N/A | < 90 days median | < 30 days median | Monthly |
| **Threat Profile Technique Count** | Number of ATT&CK techniques in composite threat profile | Threat profile | > 20 | > 40 | 50+ (prioritised) | Quarterly |

### Threat Hunting (M3TID Continuous Hunt)

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Hunts Conducted** | Number of structured hunts executed per quarter | Hunt log | N/A | >= 4/qtr | >= 8/qtr | Quarterly |
| **Hunt Finding Rate** | (Hunts with actionable findings) / (Total hunts) x 100 | Hunt log | N/A | > 20% | > 40% | Quarterly |
| **Hunt Hypothesis Sources** | Breakdown of hypothesis types: intelligence / situational / analytics-driven | Hunt log | N/A | All 3 types used | Balanced mix | Quarterly |
| **Dwell Time Discovered** | Longest adversary dwell time found during hunts | Hunt reports | N/A | Measured | Decreasing trend | Quarterly |

### Detection Testing (Module 07)

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Test Pass Rate** | (Atomic tests PASS) / (Total atomic tests run) x 100 | Test results | Baseline | > 60% | > 80% | Per test cycle |
| **Test Coverage** | (Priority techniques tested this quarter) / (Total priority techniques) x 100 | Test tracker | > 10% | > 50% | > 80% | Quarterly |
| **Countermeasure Block Rate** | (Techniques blocked by D3FEND controls) / (Techniques tested against controls) x 100 | Test results | N/A | Measured | Increasing trend | Per test cycle |
| **Detection Regression Rate** | (Previously passing tests now failing) / (Total re-tested) x 100 | CI/CD pipeline | N/A | Measured | < 5% | Per sprint |
| **Time to Remediate Failed Detection** | Avg time from test FAIL to updated detection deployed | Detection repo + test tracker | N/A | < 2 sprints | < 1 sprint | Per sprint |
| **Purple Team Exercise Cadence** | Purple team exercises conducted per year | Exercise log | N/A | >= 4/year | >= 4/year + ad-hoc | Annually |

### Forensics (Module 08)

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **Forensic Readiness Score** | (Pre-collection checklist items completed) / (Total items) x 100 | Readiness checklist | > 30% | > 70% | > 90% | Quarterly |
| **Evidence Acquisition Time** | Avg time from "acquire evidence" decision to evidence preserved | Case management | Baseline | < 4 hrs | < 1 hr | Per incident |
| **Chain of Custody Compliance** | (Cases with complete CoC records) / (Cases requiring CoC) x 100 | Case management | > 50% | > 90% | 100% | Monthly |
| **Forensic Feedback Rate** | (Forensic cases generating detection/intel updates) / (Total forensic cases) x 100 | Case management + detection repo | N/A | > 40% | > 70% | Quarterly |

---

## Countermeasures (Module 06)

| Metric | Definition | Data Source | Level 2 Target | Level 3 Target | Level 4 Target | Frequency |
|---|---|---|---|---|---|---|
| **D3FEND Mapping Coverage** | (ATT&CK techniques with D3FEND countermeasures mapped) / (Priority techniques with detection score < 3) x 100 | D3FEND mapping | N/A | > 50% | > 80% | Quarterly |
| **Countermeasure Deployment Rate** | (Mapped countermeasures deployed) / (Total mapped countermeasures) x 100 | Deployment tracker | N/A | > 40% | > 70% | Quarterly |
| **Hardening Compliance** | (Hosts meeting CIS/hardening baseline) / (Total hosts) x 100 | Hardening scanner | > 40% | > 70% | > 90% | Monthly |

---

## Executive Dashboard Summary

For governance reporting (Module 01), distil metrics into an executive-level dashboard:

```markdown
## SOC Executive Dashboard — [Month Year]

### Overall Health

| Indicator | Status | Trend | Notes |
|---|---|---|---|
| Detection Coverage | XX% | ▲/▼/► | [brief context] |
| MTTD (avg) | XX min | ▲/▼/► | |
| MTTR (avg) | XX hrs | ▲/▼/► | |
| Test Pass Rate | XX% | ▲/▼/► | |
| SOC-CMM Overall | X.X / 5.0 | ▲/▼/► | |

### Key Metrics

| Category | Metric | Current | Target | Status |
|---|---|---|---|---|
| Detection | Coverage Ratio | XX% | XX% | On/Off Track |
| Detection | FP Rate (avg) | XX% | < XX% | On/Off Track |
| Response | MTTR (High sev) | XX hrs | < XX hrs | On/Off Track |
| Testing | Pass Rate | XX% | > XX% | On/Off Track |
| People | Staffing Fill | XX% | > XX% | On/Off Track |
| Data | Source Coverage | XX% | > XX% | On/Off Track |

### SOC-CMM Domain Scores

| Domain | Current | Target | Delta |
|---|---|---|---|
| Business | X.X | X.X | +/-X.X |
| People | X.X | X.X | +/-X.X |
| Process | X.X | X.X | +/-X.X |
| Technology | X.X | X.X | +/-X.X |
| Services | X.X | X.X | +/-X.X |

### Top Risks

1. [Risk description] — Mitigation: [action]
2. [Risk description] — Mitigation: [action]
3. [Risk description] — Mitigation: [action]

### Quarterly Priorities

1. [Priority from SOC-CMM gap analysis]
2. [Priority from detection gap analysis]
3. [Priority from staffing/training gap]
```

---

## SOC-CMM Self-Assessment Scorecard

Use this scorecard quarterly. For each aspect, assign the level that best describes your current state based on the criteria in [soc-cmm.md](soc-cmm.md).

### Assessment Instructions

1. For each aspect, select the **highest level where ALL criteria are met** (not some — all)
2. Use **evidence, not aspiration** — if you can't point to evidence, the level isn't earned
3. Record the evidence reference — this builds accountability and enables trend tracking
4. A half-level (e.g., 2.5) is acceptable when some but not all criteria at the next level are met

### Domain 1 — Business

| Aspect | Level 1 Criteria (Initial) | Level 2 Criteria (Repeatable) | Level 3 Criteria (Defined) | Level 4 Criteria (Managed) | Current | Evidence |
|---|---|---|---|---|---|---|
| **Governance** | SOC exists; no charter | Charter drafted; basic authority | Charter ratified; executive sponsor; authority documented | Charter reviewed annually; authority exercised regularly; governance drives decisions | | |
| **Funding & Budget** | No dedicated budget | Initial budget allocated | Multi-year budget; aligned to strategy | Budget tied to measurable outcomes; ROI tracked | | |
| **Strategy & Roadmap** | No strategy | Ad-hoc planning | Documented strategy with maturity targets | Strategy reviewed quarterly; metrics-driven adjustments | | |
| **Business Alignment** | SOC disconnected from business | Basic awareness of business context | SOC priorities reflect business risk; crown jewels identified | Risk-based prioritisation drives all SOC activities | | |
| **Stakeholder Management** | No stakeholder communication | Ad-hoc reporting | Regular reporting cadence; stakeholder register maintained | Stakeholder satisfaction measured; feedback loop active | | |
| **Regulatory Compliance** | No compliance awareness | Basic compliance requirements known | Requirements mapped to SOC controls; audit-ready | Compliance evidence automated; no audit findings | | |
| **Risk Management** | No risk integration | Basic risk awareness | SOC integrated into enterprise risk; risk-based detection priority | Risk quantified; SOC reduces measurable risk | | |
| **Domain Average** | | | | | **X.X** | |

### Domain 2 — People

| Aspect | Level 1 Criteria | Level 2 Criteria | Level 3 Criteria | Level 4 Criteria | Current | Evidence |
|---|---|---|---|---|---|---|
| **Staffing & Recruitment** | Part-time SOC staff | Dedicated headcount; basic roles | Roles mapped to DoDCWF/ASD/CIISec; recruitment pipeline | Retention strategy; competitive compensation; talent pipeline | | |
| **Training & Education** | Self-directed only | Basic training budget | Structured program aligned to roles and modules | Skill gap analysis drives training; effectiveness measured | | |
| **Certifications** | No cert requirements | Some certs encouraged | Certs required per role (DoD 8140); funded | Cert completion tracked; advanced certs pursued | | |
| **Career Progression** | No career paths | Informal progression | Documented career paths; specialist + management tracks | Progression criteria measured; mentorship program active | | |
| **Knowledge Management** | Tribal knowledge only | Some documentation | Knowledge base maintained; runbooks current; templates used | Knowledge sharing systematic; cross-training active | | |
| **Awareness & Culture** | No security culture | Basic awareness | SOC team aware of business context; culture of learning | Culture measured; innovation time allocated | | |
| **Collaboration** | Siloed operations | Some cross-team interaction | Regular collaboration with IT, dev, legal; peer engagement | ISAC active participation; community contribution | | |
| **Domain Average** | | | | | **X.X** | |

### Domain 3 — Process

| Aspect | Level 1 Criteria | Level 2 Criteria | Level 3 Criteria | Level 4 Criteria | Current | Evidence |
|---|---|---|---|---|---|---|
| **Procedures & Documentation** | No SOPs | Key processes documented | SOPs for all core activities; accessible; current | SOPs reviewed on schedule; compliance verified | | |
| **Use Case Management** | No use case tracking | Basic use case list | Use cases linked to ATT&CK; lifecycle managed (create→test→deploy→retire) | Use case metrics tracked; ROI per use case measured | | |
| **Incident Management** | Unstructured response | Escalation defined; basic playbooks | RE&CT playbooks for priority techniques; post-incident review | SOAR automation; systematic PIR → improvement loop | | |
| **Threat Intelligence Process** | Ad-hoc IOC consumption | Threat feeds received | CTI intelligence cycle operational; products produced; M3TID active | Intel drives detection priority automatically; community sharing | | |
| **Analytics & Metrics** | No metrics | Basic alert counts | KPIs tracked (MTTD, MTTR, coverage); dashboard operational | Trend analysis; metrics drive decisions; forecasting | | |
| **Automation** | All manual | Some scripts | Detection CI/CD; SOAR for routine containment | Automation ROI measured; self-service automation | | |
| **Change Management** | No change control | Ad-hoc changes | Detection-as-code repo with review process; change log | Change metrics tracked; rollback tested; zero-downtime deploys | | |
| **Continuous Improvement** | No feedback loops | Ad-hoc improvements | PIR → improvement loop; quarterly SOC-CMM review | Improvement velocity measured; predictive gap analysis | | |
| **Domain Average** | | | | | **X.X** | |

### Domain 4 — Technology

| Aspect | Level 1 Criteria | Level 2 Criteria | Level 3 Criteria | Level 4 Criteria | Current | Evidence |
|---|---|---|---|---|---|---|
| **SIEM / Log Management** | No SIEM | Basic log aggregation | SIEM with CDM normalisation; correlation; adequate retention | Quality monitoring; automated ingest health alerts | | |
| **Log Source Coverage** | Minimal logs | Some priority sources | Coverage mapped to ATT&CK data sources; gaps documented | Continuous coverage expansion; onboarding SLAs | | |
| **Detection Platform** | Vendor defaults only | Some custom rules | Sigma rules in production; DeTTECT scoring active; MITRE CAR reviewed | CI/CD deployment; ML-augmented detection; coverage > 80% | | |
| **EDR** | No EDR | Deployed to critical assets | Fleet-wide; custom rules; isolation capability | Automated response for high-confidence alerts | | |
| **NSM** | No NSM | Basic IDS | Zeek/Suricata operational; PCAP available for critical segments | Full network visibility; encrypted traffic inspection | | |
| **SOAR** | No automation | Basic scripts | SOAR operational; playbooks for routine actions | Advanced playbooks; cross-tool orchestration | | |
| **TIP** | No TIP | Basic feed consumption | TIP managing feeds; ATT&CK integration; indicator lifecycle | Automated intel → detection pipeline; sharing active | | |
| **Ticketing & Case Mgmt** | Email/spreadsheet | Basic ticketing | Security-focused case management; audit trail; metrics | Integrated with SOAR + SIEM; SLA tracking | | |
| **Testing Infrastructure** | No test env | Ad-hoc testing | Atomic RT + attack_range deployed; regular testing | CI/CD detection validation; continuous testing pipeline | | |
| **Tool Integration** | Siloed tools | Some manual integration | Key tools integrated via API; data flows documented | Full integration architecture; single-pane operations | | |
| **Domain Average** | | | | | **X.X** | |

### Domain 5 — Services

| Aspect | Level 1 Criteria | Level 2 Criteria | Level 3 Criteria | Level 4 Criteria | Current | Evidence |
|---|---|---|---|---|---|---|
| **Monitoring & Detection** | Reactive; business hours | Monitoring with escalation | 24x7 or risk-justified; tuned detections; low FP | High-fidelity; automated triage; ML-augmented | | |
| **Incident Response** | Ad-hoc response | Basic playbooks; escalation | RE&CT playbooks for priority TTPs; tabletops quarterly | Automated containment; MTTR targets met; predictive | | |
| **Threat Intelligence** | No CTI | Feeds consumed | CTI products produced; threat profile drives operations | Intel drives automation; community sharing active | | |
| **Threat Hunting** | No hunting | Ad-hoc hunts | Structured hunts from hypotheses; hunt-to-detection pipeline | Continuous hunt operations; all 3 hypothesis types active | | |
| **Vulnerability Mgmt** | No vuln awareness | Scan results received | Risk-based prioritisation; D3FEND countermeasures mapped | Countermeasures validated via testing; trend improvement | | |
| **Forensics & Investigation** | No capability | Basic disk imaging | Full acquisition (mem+disk+triage); CoC enforced; findings feed pipeline | Enterprise-scale; automated evidence collection; forensic-as-code | | |
| **Advisory & Reporting** | No reporting | Ad-hoc status updates | Regular executive reports; compliance evidence; actionable advisories | Dashboards; real-time reporting; predictive advisories | | |
| **Detection Testing** | No testing | Occasional Atomic tests | Monthly coverage scans; quarterly purple team; KPIs tracked | Continuous CI/CD validation; regressions caught automatically | | |
| **Domain Average** | | | | | **X.X** | |

### Assessment Summary

```markdown
## SOC-CMM Assessment Summary — [Date]

### Assessor: [Name / Team]
### Method: [Self / Peer / External]
### Period: [Q_ 20__]

| Domain | Current | Target | Gap | Priority Actions |
|---|---|---|---|---|
| Business | X.X | X.X | X.X | |
| People | X.X | X.X | X.X | |
| Process | X.X | X.X | X.X | |
| Technology | X.X | X.X | X.X | |
| Services | X.X | X.X | X.X | |
| **Overall** | **X.X** | **X.X** | **X.X** | |

### Lowest Domain: [Domain] at [X.X]
### Highest Domain: [Domain] at [X.X]
### Largest Gap: [Domain] — delta of [X.X]

### Improvement Priorities (next quarter)

1. [Priority 1 — from largest gap or Business/People domain]
2. [Priority 2]
3. [Priority 3]

### Next Assessment: [Date]
```

---

## Measurement Maturity Progression

Don't try to measure everything at once. The metrics you track should match your maturity:

### Level 1-2: Awareness Metrics

Focus on understanding what you have and establishing baselines:

- Alert volume (trending awareness)
- Incident count by severity
- Staffing fill rate
- Log source count
- Detection rule count

### Level 3: Effectiveness Metrics

Focus on whether the SOC is working as designed:

- MTTD, MTTR, MTTC
- Detection coverage ratio
- Test pass rate
- Playbook coverage
- Data quality scores
- Hunt hypothesis completion rate
- Forensic readiness score

### Level 4: Optimisation Metrics

Focus on continuous improvement and efficiency:

- False positive rate per rule
- Hunt-to-detection conversion rate
- Detection regression rate
- Automation rate (SOAR)
- Countermeasure block rate
- Time to remediate failed detection
- ROI per use case

### Level 5: Predictive Metrics

Focus on anticipation and innovation:

- Threat coverage forecasting
- Detection gap prediction
- Workforce planning models
- Risk reduction rate over time
- Industry benchmark comparison

---

## Metric Collection Sources

| Source | Metrics It Provides | Collection Method |
|---|---|---|
| **SIEM** | Alert volume, MTTD, FP rate, query performance | API / built-in dashboards |
| **Case Management** (TheHive, DFIR-IRIS) | MTTR, MTTC, incident counts, escalation accuracy | API / export |
| **DeTTECT** | Detection coverage, data source quality, gap analysis | YAML review |
| **Detection Repo** (Git) | Rule count, change frequency, review metrics | Git log analysis |
| **CI/CD Pipeline** | Deployment success, regression rate | Pipeline dashboards |
| **Test Results** | Pass rate, block rate, MTTD validation | Test tracker |
| **TIP** (MISP, OpenCTI) | Indicator counts, freshness, intel products | API / export |
| **SOAR** (Shuffle) | Automation rate, playbook execution counts | API / dashboards |
| **HR / Training Systems** | Staffing, turnover, training hours, certs | HR export |
| **SOC-CMM Assessment** | Domain scores, gap analysis | Quarterly assessment |

---

## References

- [SOC-CMM](https://www.soc-cmm.com) — Rob van Os
- [MITRE 11 Strategies — Strategy 10](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
- [SOC Metrics That Matter](https://www.sans.org/white-papers/) — SANS
- [NIST CSF 2.0 Measurement](https://www.nist.gov/cyberframework)
