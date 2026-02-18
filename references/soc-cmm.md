# SOC-CMM — SOC Capability Maturity Model

> **Framework:** [SOC-CMM](https://www.soc-cmm.com) by Rob van Os
> **Role:** Primary maturity assessment framework for this pipeline

---

## Overview

The SOC Capability Maturity Model (SOC-CMM) is a self-assessment framework designed specifically for Security Operations Centers. Unlike generic maturity models (CMMI, C2M2), SOC-CMM evaluates the **five domains that determine SOC effectiveness** — Business, People, Process, Technology, and Services — providing a holistic view of operational maturity rather than measuring technical capability in isolation.

SOC-CMM is the primary maturity framework for this pipeline because it:

1. **Covers the full SOC scope** — not just detections, but governance, workforce, processes, and services
2. **Aligns naturally to the pipeline modules** — each SOC-CMM domain maps to one or more pipeline modules
3. **Provides actionable assessment criteria** — specific, measurable aspects within each domain
4. **Supports incremental improvement** — aligns with MITRE Strategy 11 (Turn up the Dial)
5. **Is purpose-built for SOCs** — developed from real-world SOC operations, not abstracted from generic frameworks

---

## SOC-CMM Domain Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        SOC CAPABILITY MATURITY MODEL                     │
│                                                                          │
│   ┌─────────────────────────────────────────────────────────────────┐   │
│   │                        BUSINESS                                  │   │
│   │  Governance · Budget · Strategy · Stakeholders · Compliance      │   │
│   │  Risk Management · Business Alignment                            │   │
│   └──────────────────────────┬──────────────────────────────────────┘   │
│                              │                                           │
│   ┌──────────────┐   ┌──────┴──────┐   ┌──────────────┐                │
│   │    PEOPLE     │   │   PROCESS   │   │  TECHNOLOGY   │                │
│   │              │   │             │   │              │                │
│   │ Staffing     │   │ Procedures  │   │ SIEM         │                │
│   │ Training     │   │ Use Cases   │   │ Log Mgmt     │                │
│   │ Careers      │   │ Incidents   │   │ SOAR         │                │
│   │ Knowledge    │   │ Analytics   │   │ Endpoint     │                │
│   │ Awareness    │   │ Automation  │   │ Network      │                │
│   │ Collaboration│   │ Change Mgmt │   │ TIP          │                │
│   └──────┬───────┘   └──────┬──────┘   └──────┬───────┘                │
│          │                  │                  │                         │
│   ┌──────┴──────────────────┴──────────────────┴───────┐                │
│   │                      SERVICES                        │                │
│   │  Monitoring · Incident Response · Threat Intel       │                │
│   │  Threat Hunting · Vulnerability Mgmt · Forensics     │                │
│   │  Advisory & Reporting                                │                │
│   └──────────────────────────────────────────────────────┘                │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Maturity Levels

SOC-CMM uses a 6-level maturity scale (0–5), based on the Capability Maturity Model:

| Level | Name | Description | Characteristics |
|---|---|---|---|
| **0** | **Non-Existent** | Capability does not exist | No awareness, no processes, no tools |
| **1** | **Initial** | Ad-hoc and reactive | Dependent on individual initiative; no documentation; firefighting mode |
| **2** | **Repeatable** | Basic processes established | Key activities can be repeated; some documentation; basic tools in place |
| **3** | **Defined** | Standardised and proactive | Documented procedures followed consistently; roles defined; threat-informed |
| **4** | **Managed** | Measured and controlled | Quantitative metrics drive decisions; SLAs met; continuous monitoring of performance |
| **5** | **Optimising** | Continuous improvement | Automated feedback loops; innovation; predictive capabilities; industry-leading |

### Level Progression Philosophy

```
Level 0 → 1:  Stand up the SOC. Get something running.
Level 1 → 2:  Stabilise. Make it repeatable. Document what you do.
Level 2 → 3:  Standardise and align to frameworks. THIS IS WHERE THIS PIPELINE
              DELIVERS MAXIMUM VALUE. Most of the pipeline's frameworks, templates,
              and workflows are designed to bring a SOC from Level 2 to Level 3.
Level 3 → 4:  Measure everything. Let data drive improvement.
Level 4 → 5:  Automate the feedback loop. Innovate. Lead the industry.
```

---

## Domain 1 — Business

The Business domain evaluates whether the SOC has organisational authority, strategic alignment, adequate funding, and compliance integration. A technically excellent SOC without business alignment is a SOC at risk of defunding.

### Aspects

| Aspect | Description | Pipeline Mapping |
|---|---|---|
| **Governance** | SOC charter, mandate, authority, reporting structure, executive sponsorship | Module 01 (NIST CSF 2.0 GV) |
| **Funding & Budget** | Dedicated SOC budget, multi-year planning, ROI justification, cost tracking | Module 01 (Governance) |
| **Strategy & Roadmap** | SOC strategy aligned to business strategy, documented roadmap, maturity targets | references/maturity-model.md |
| **Business Alignment** | SOC priorities reflect business risk; crown jewel protection; business impact assessment | Module 01 (NIST CSF ID.AM), Module 03 (threat profiling) |
| **Stakeholder Management** | Regular communication with executives, IT, legal, HR, business units; reporting cadence | Module 01 (NIST CSF GV.RR) |
| **Regulatory Compliance** | Compliance requirements identified; SOC activities map to regulatory obligations; audit readiness | Module 01 (NIST CSF GV.PO) |
| **Risk Management** | SOC integrated into enterprise risk management; risk-based detection prioritisation | Module 03 (ATT&CK threat profiling) |

### Business Domain Maturity Criteria

| Level | What It Looks Like |
|---|---|
| 0 | No SOC function exists; security is incidental |
| 1 | SOC exists informally; no charter; no dedicated budget; reactive to incidents only |
| 2 | Charter drafted; initial budget allocated; basic reporting to management; ad-hoc stakeholder communication |
| 3 | Charter ratified with executive authority; multi-year budget; strategy document with maturity targets; regular stakeholder reporting; compliance requirements mapped; risk-based prioritisation active |
| 4 | Budget tied to measurable outcomes (MTTD/MTTR improvement, coverage %); strategy reviewed quarterly; stakeholder satisfaction measured; compliance evidence automated; SOC integrated into ERM |
| 5 | Predictive budget modelling based on threat landscape; SOC strategy drives business security investment; real-time compliance dashboards; SOC is a recognised business enabler |

---

## Domain 2 — People

The People domain evaluates workforce capability, development, and sustainability. The most advanced technology is useless without skilled, motivated staff.

### Aspects

| Aspect | Description | Pipeline Mapping |
|---|---|---|
| **Staffing & Recruitment** | Adequate headcount, defined roles, recruitment pipeline, retention strategy | references/workforce-roles.md (DoDCWF + ASD CSF + CIISec) |
| **Training & Education** | Structured training program, budget, alignment to role requirements, hands-on labs | All modules (training paths per role) |
| **Certifications** | Certification requirements defined per role; funded by organisation; tracked | references/workforce-roles.md (DoD 8140 certs) |
| **Career Progression** | Defined career paths; progression criteria documented; specialist and management tracks | references/workforce-roles.md (career paths) |
| **Knowledge Management** | Runbooks, wikis, lessons learned; institutional knowledge captured; knowledge sharing | All modules (playbooks, templates, procedures) |
| **Awareness & Culture** | Security culture; SOC team awareness of business context; cross-team awareness programs | Module 01 (Governance) |
| **Collaboration** | Internal SOC collaboration; cross-team (IT, dev, legal); external (ISACs, peers) | Module 01 (MITRE Strategy 9) |

### People Domain Maturity Criteria

| Level | What It Looks Like |
|---|---|
| 0 | No dedicated security operations staff |
| 1 | Staff assigned to SOC duties part-time; no role definitions; training is self-directed; tribal knowledge |
| 2 | Dedicated SOC staff; roles defined (tiers); basic training budget; some documentation of procedures |
| 3 | Roles mapped to DoDCWF work roles and ASD/CIISec skill streams; structured training program aligned to pipeline modules; career paths documented; knowledge base maintained; regular collaboration with peers |
| 4 | Skill gap analysis drives training investment; certification completion tracked; retention metrics monitored; cross-training across specialisms; active ISAC participation; knowledge sharing is systematic |
| 5 | Predictive workforce planning; talent pipeline from education partnerships; innovation time allocated; staff contribute to open-source/community; SOC is an employer of choice |

### CIISec Skills Framework Integration

The [Chartered Institute of Information Security (CIISec) Skills Framework](https://www.ciisec.org/skills-framework) provides **specialism-based capability differentiation** that complements the ASD Cyber Skills Framework. Where ASD defines skill streams and levels, CIISec defines **specific knowledge and skill areas within each specialism**, enabling more granular workforce assessment.

#### CIISec Specialisms Relevant to SOC Operations

| CIISec Specialism | SOC Application | ASD Equivalent Stream | Differentiation CIISec Adds |
|---|---|---|---|
| **A1 — Threat Intelligence & Assessment** | CTI analysts, threat profiling, adversary tracking | Cyber Threat Intelligence | Adds strategic vs operational vs tactical intel distinction; geopolitical context assessment |
| **A4 — Incident Management** | IR leads, incident commanders, forensic analysts | Incident Response | Adds crisis management, communication protocols, legal coordination, evidence handling chain |
| **A5 — Security Operations** | SOC analysts (all tiers), monitoring, triage | Cyber Security Operations | Adds shift management, alert fatigue mitigation, analyst workflow optimisation |
| **B1 — Secure Operations & Service Delivery** | SOC service management, SLAs, operational resilience | Cyber Security Governance | Adds ITIL alignment, service catalogue, operational metrics, capacity planning |
| **B2 — Vulnerability Assessment** | Purple team, attack surface management, proactive testing | Vulnerability Assessment | Adds risk-based vulnerability prioritisation, exploit probability scoring |
| **C1 — Threat Detection & Digital Forensics** | Detection engineering, hunt operations, DFIR | Cyber Security Operations + Incident Response | Adds detection development lifecycle, evidence preservation, forensic tool validation |
| **C2 — Cyber Security Research** | Detection research, adversary emulation, tool development | N/A (gap in ASD) | **Unique to CIISec** — covers research methodology, proof-of-concept development, academic collaboration |
| **D1 — Governance, Risk & Compliance** | SOC governance, compliance mapping, risk integration | Cyber Security Governance | Adds GRC tool expertise, regulatory interpretation, control effectiveness measurement |
| **E1 — Learning, Training & Awareness** | SOC training program design, knowledge transfer | N/A (gap in ASD) | **Unique to CIISec** — covers adult learning theory, competency assessment design, training effectiveness metrics |

#### Three-Framework Workforce Assessment

Using CIISec alongside ASD and DoDCWF provides a **three-dimensional workforce assessment**:

```
┌──────────────────────────────────────────────────────────────────┐
│                THREE-FRAMEWORK WORKFORCE ASSESSMENT               │
│                                                                   │
│  DoDCWF 8140          ASD Cyber Skills        CIISec Skills       │
│  ─────────────        ───────────────         ─────────────       │
│  WHAT role?           HOW skilled?            HOW DEEPLY skilled? │
│                                                                   │
│  Work Role ID    →    Skill Stream    →    Specialism Knowledge   │
│  (PR-CDA-001)        (Cyber Sec Ops)      (A5: SOC Workflow      │
│                                            Optimisation,          │
│  KSA               → Level (1-5)     →     Alert Fatigue         │
│  Requirements                               Mitigation,          │
│                                             Shift Handover        │
│  Certification   →   Progression     →     Protocols)            │
│  Requirements        Criteria                                     │
│                                                                   │
│  DEFINES the role    MEASURES the      DIFFERENTIATES the        │
│                      competency level   depth within each level   │
└──────────────────────────────────────────────────────────────────┘
```

**Example — Detection Engineer:**

| Dimension | Framework | Assessment |
|---|---|---|
| **Role Definition** | DoDCWF PR-INF-001 | Cyber Defense Infrastructure Support — KSAs for SIEM, sensor deployment, detection |
| **Competency Level** | ASD Cyber Security Architecture Level 3-4 | Can design and implement detection infrastructure independently |
| **Specialism Depth** | CIISec C1 (Threat Detection & Digital Forensics) | Has the detection development lifecycle knowledge: hypothesis → analytic → test → deploy → tune. Understands OSSEM-CDM normalisation. Can build CI/CD detection pipelines. |
| **Specialism Depth** | CIISec C2 (Cyber Security Research) | Can research novel adversary techniques, develop proof-of-concept detections, contribute to community Sigma rules. |

---

## Domain 3 — Process

The Process domain evaluates whether the SOC has documented, repeatable, and continuously improving operational processes.

### Aspects

| Aspect | Description | Pipeline Mapping |
|---|---|---|
| **Procedures & Documentation** | Standard operating procedures documented, accessible, current; runbooks maintained | All modules (checklists, templates, playbooks) |
| **Use Case Management** | Detection use cases linked to threats; lifecycle managed (create, test, deploy, retire) | Module 04 (Detection Engineering) |
| **Incident Management** | Incident lifecycle process; severity classification; escalation procedures; post-incident review | Module 05 (RE&CT) |
| **Threat Intelligence Process** | CTI collection, processing, analysis, dissemination, feedback cycle | Module 03 (ATT&CK profiling) |
| **Analytics & Metrics** | KPIs defined; dashboards operational; metrics drive decisions | Module 07 (Testing KPIs), Module 01 (Governance reporting) |
| **Automation** | Repetitive tasks automated; playbook automation (SOAR); detection deployment automation (CI/CD) | Module 04 (detection-as-code CI/CD), Module 05 (SOAR) |
| **Change Management** | Changes to detections, infrastructure, and processes are controlled and documented | Module 04 (detection-as-code repo) |
| **Continuous Improvement** | Lessons learned captured and actioned; process reviews scheduled; feedback loops operational | Module 07 (continuous feedback loop) |

### Process Domain Maturity Criteria

| Level | What It Looks Like |
|---|---|
| 0 | No documented processes; everything is ad-hoc |
| 1 | Some processes exist in people's heads; incident response is reactive and unstructured |
| 2 | Key processes documented (incident handling, escalation); basic use case list exists; some metrics tracked |
| 3 | SOPs for all core activities; use cases managed with lifecycle (linked to ATT&CK); RE&CT playbooks for priority techniques; CTI intelligence cycle operational; detection-as-code repo with review process; metrics dashboard with MTTD/MTTR/coverage |
| 4 | SOAR automates routine containment; detection CI/CD pipeline deploys validated rules; metrics analysed for trends; change management enforced; post-incident reviews systematically update detections, playbooks, and threat profiles |
| 5 | Predictive analytics identify emerging gaps; automated continuous improvement loops; processes self-adapt based on threat landscape changes; innovation pipeline for new SOC processes |

---

## Domain 4 — Technology

The Technology domain evaluates the tools, platforms, and technical infrastructure supporting SOC operations.

### Aspects

| Aspect | Description | Pipeline Mapping |
|---|---|---|
| **SIEM / Log Management** | Central log platform; ingestion coverage; parsing and normalisation; retention | Module 02 (OSSEM-CDM normalisation) |
| **Log Source Coverage** | Breadth and depth of log sources collected; gap analysis against requirements | Module 02 (data source inventory) |
| **Detection Platform** | Detection rule engine; correlation capability; custom analytic development | Module 04 (Sigma, DeTTECT, CAR) |
| **Endpoint Detection & Response** | EDR deployment coverage; capability (collect, detect, respond, isolate) | Module 04, 05 (detection + containment) |
| **Network Security Monitoring** | NSM tools (Zeek, Suricata); PCAP capability; encrypted traffic handling | Module 02 (network data sources) |
| **Security Orchestration & Automation** | SOAR platform; playbook automation; API integrations | Module 05 (automated response) |
| **Threat Intelligence Platform** | TIP for feed management, indicator lifecycle, ATT&CK integration | Module 03 (CTI operations) |
| **Ticketing & Case Management** | Incident tracking; workflow management; audit trail; metrics extraction | Module 05 (incident management) |
| **Testing & Simulation Infrastructure** | Lab environment for adversary simulation and detection testing | Module 07 (Atomic RT, attack_range) |
| **Tool Integration** | Tools interoperate via APIs; data flows between platforms; single-pane-of-glass capability | references/tool-integration-matrix.md |

### Technology Domain Maturity Criteria

| Level | What It Looks Like |
|---|---|
| 0 | No dedicated security monitoring tools |
| 1 | Basic SIEM or log aggregation; default vendor rules; no EDR; manual processes |
| 2 | SIEM with custom log sources; EDR deployed to critical assets; basic ticketing; some tool integration |
| 3 | SIEM with OSSEM-CDM normalisation; detection-as-code (Sigma); EDR with custom rules; DeTTECT coverage scoring; Atomic Red Team for testing; NSM operational; TIP managing feeds; tools integrated via APIs |
| 4 | SOAR automating response playbooks; CI/CD detection deployment; attack_range for scenario testing; comprehensive log coverage with quality monitoring; tool telemetry feeding metrics dashboards |
| 5 | AI/ML augmenting detection; automated detection generation from CTI; self-tuning analytics; full kill chain visibility; predictive tooling identifying gaps before they're exploited |

---

## Domain 5 — Services

The Services domain evaluates the operational services the SOC delivers. This is what the SOC **does** — the outputs that justify its existence.

### Aspects

| Aspect | Description | Pipeline Mapping |
|---|---|---|
| **Security Monitoring & Detection** | Real-time monitoring; alert triage; correlation; escalation | Module 04 (Detection Engineering) |
| **Incident Response** | Incident handling lifecycle; containment, eradication, recovery; post-incident review | Module 05 (RE&CT) |
| **Threat Intelligence** | CTI production; threat profiling; indicator management; intel sharing | Module 03 (ATT&CK profiling) |
| **Threat Hunting** | Proactive, hypothesis-driven threat discovery; hunt operations; hunt-to-detection pipeline | M3TID continuous hunt methodology |
| **Vulnerability Management** | Vulnerability awareness; risk-based prioritisation; coordination with patch management | Module 06 (D3FEND countermeasures) |
| **Digital Forensics & Investigation** | Forensic acquisition; analysis; evidence handling; legal support | Module 05 (IR forensic actions) |
| **Advisory & Reporting** | Security advisories; executive reporting; compliance evidence; lessons learned dissemination | Module 01 (Governance reporting) |
| **Detection Testing & Validation** | Continuous validation of detection coverage; purple teaming; adversary simulation | Module 07 (Atomic RT, attack_range) |

### Services Domain Maturity Criteria

| Level | What It Looks Like |
|---|---|
| 0 | No SOC services delivered |
| 1 | Basic monitoring during business hours; reactive incident response; no hunting; ad-hoc reporting |
| 2 | Monitoring with documented escalation; incident response with basic playbooks; threat intel consumed (not produced); occasional ad-hoc hunts; regular status reporting |
| 3 | 24x7 monitoring (or risk-justified hours); RE&CT playbooks for priority techniques; CTI products produced and consumed; structured hunt program with hypotheses from threat profile; detection testing monthly; vulnerability awareness integrated; forensic capability available; regular executive and compliance reporting |
| 4 | High-fidelity detections with low FP rates; automated containment for high-confidence alerts; CTI drives detection priorities automatically; hunt findings systematically converted to detections; continuous detection validation (CI/CD); purple team exercises quarterly; metrics-driven service improvement |
| 5 | Predictive detection anticipates emerging threats; automated incident response for routine scenarios; intelligence sharing with community; continuous hunt operations integrated with detection engineering; real-time coverage dashboards; SOC services benchmarked against industry peers |

---

## SOC-CMM ↔ Pipeline Module Mapping

| SOC-CMM Domain | SOC-CMM Aspect | Primary Pipeline Module | Supporting Modules |
|---|---|---|---|
| **Business** | Governance | 01 Governance | — |
| Business | Budget & Funding | 01 Governance | — |
| Business | Strategy & Roadmap | Maturity Model | 01 Governance |
| Business | Business Alignment | 01 Governance | 03 Threat Intel |
| Business | Stakeholder Management | 01 Governance | — |
| Business | Regulatory Compliance | 01 Governance | — |
| Business | Risk Management | 03 Threat Intel | 01 Governance |
| **People** | Staffing & Recruitment | Workforce Roles | 01 Governance |
| People | Training & Education | Workforce Roles | All modules |
| People | Certifications | Workforce Roles | — |
| People | Career Progression | Workforce Roles | — |
| People | Knowledge Management | All modules | — |
| People | Awareness & Culture | 01 Governance | — |
| People | Collaboration | 01 Governance | 03 Threat Intel |
| **Process** | Procedures & Documentation | All modules | — |
| Process | Use Case Management | 04 Detection Eng | 03 Threat Intel |
| Process | Incident Management | 05 Incident Response | — |
| Process | Threat Intel Process | 03 Threat Intel | — |
| Process | Analytics & Metrics | 07 Detection Testing | 01 Governance |
| Process | Automation | 04 Detection Eng, 05 IR | 07 Testing |
| Process | Change Management | 04 Detection Eng | — |
| Process | Continuous Improvement | 07 Detection Testing | All modules |
| **Technology** | SIEM / Log Management | 02 Data Documentation | 04 Detection Eng |
| Technology | Log Source Coverage | 02 Data Documentation | — |
| Technology | Detection Platform | 04 Detection Eng | — |
| Technology | EDR | 04 Detection Eng | 05 IR |
| Technology | NSM | 02 Data Documentation | 04 Detection Eng |
| Technology | SOAR | 05 Incident Response | — |
| Technology | TIP | 03 Threat Intel | — |
| Technology | Ticketing & Case Mgmt | 05 Incident Response | — |
| Technology | Testing Infrastructure | 07 Detection Testing | — |
| Technology | Tool Integration | Tool Integration Matrix | — |
| **Services** | Monitoring & Detection | 04 Detection Eng | 02 Data Doc |
| Services | Incident Response | 05 Incident Response | — |
| Services | Threat Intelligence | 03 Threat Intel | — |
| Services | Threat Hunting | M3TID Continuous Hunt | 02, 03, 04 |
| Services | Vulnerability Management | 06 Countermeasures | — |
| Services | Forensics & Investigation | 08 Forensics & DFIR | 05 Incident Response |
| Services | Advisory & Reporting | 01 Governance | 07 Testing |
| Services | Detection Testing | 07 Detection Testing | — |

---

## Self-Assessment Process

### Step 1 — Assess Each Aspect

For each aspect within each domain, assign a maturity level (0–5) based on the criteria above. Use evidence, not aspiration.

### Step 2 — Calculate Domain Scores

Domain score = average of all aspect scores within that domain, rounded to one decimal.

### Step 3 — Identify Gaps

For each domain, compare current score to target score. The delta is your improvement backlog.

### Step 4 — Prioritise Improvements

Use this prioritisation logic:

1. **Business domain gaps first** — without governance and funding, nothing else sustains
2. **People gaps second** — without skilled staff, tools and processes are underutilised
3. **Process gaps third** — standardise before automating
4. **Technology gaps fourth** — tools serve processes, not the reverse
5. **Services gaps fifth** — services improve as underlying domains mature

### Step 5 — Map to Pipeline Modules

Use the SOC-CMM ↔ Pipeline Module Mapping table above to identify which pipeline modules address each gap.

---

## Assessment Template

```markdown
## SOC-CMM Assessment — [Date]

### Assessor: [Name / Team]
### Method: [Self-assessment / Peer review / External audit]
### Assessment Period: [Q1 2025 / Annual / etc.]

---

### Domain 1 — Business

| Aspect | Current (0-5) | Target (0-5) | Gap | Priority Actions |
|---|---|---|---|---|
| Governance | | | | |
| Funding & Budget | | | | |
| Strategy & Roadmap | | | | |
| Business Alignment | | | | |
| Stakeholder Management | | | | |
| Regulatory Compliance | | | | |
| Risk Management | | | | |
| **Domain Average** | **X.X** | **X.X** | | |

### Domain 2 — People

| Aspect | Current (0-5) | Target (0-5) | Gap | Priority Actions |
|---|---|---|---|---|
| Staffing & Recruitment | | | | |
| Training & Education | | | | |
| Certifications | | | | |
| Career Progression | | | | |
| Knowledge Management | | | | |
| Awareness & Culture | | | | |
| Collaboration | | | | |
| **Domain Average** | **X.X** | **X.X** | | |

### Domain 3 — Process

| Aspect | Current (0-5) | Target (0-5) | Gap | Priority Actions |
|---|---|---|---|---|
| Procedures & Documentation | | | | |
| Use Case Management | | | | |
| Incident Management | | | | |
| Threat Intelligence Process | | | | |
| Analytics & Metrics | | | | |
| Automation | | | | |
| Change Management | | | | |
| Continuous Improvement | | | | |
| **Domain Average** | **X.X** | **X.X** | | |

### Domain 4 — Technology

| Aspect | Current (0-5) | Target (0-5) | Gap | Priority Actions |
|---|---|---|---|---|
| SIEM / Log Management | | | | |
| Log Source Coverage | | | | |
| Detection Platform | | | | |
| EDR | | | | |
| NSM | | | | |
| SOAR | | | | |
| TIP | | | | |
| Ticketing & Case Management | | | | |
| Testing Infrastructure | | | | |
| Tool Integration | | | | |
| **Domain Average** | **X.X** | **X.X** | | |

### Domain 5 — Services

| Aspect | Current (0-5) | Target (0-5) | Gap | Priority Actions |
|---|---|---|---|---|
| Monitoring & Detection | | | | |
| Incident Response | | | | |
| Threat Intelligence | | | | |
| Threat Hunting | | | | |
| Vulnerability Management | | | | |
| Forensics & Investigation | | | | |
| Advisory & Reporting | | | | |
| Detection Testing & Validation | | | | |
| **Domain Average** | **X.X** | **X.X** | | |

---

### Summary

| Domain | Current | Target | Gap |
|---|---|---|---|
| Business | X.X | X.X | X.X |
| People | X.X | X.X | X.X |
| Process | X.X | X.X | X.X |
| Technology | X.X | X.X | X.X |
| Services | X.X | X.X | X.X |
| **Overall** | **X.X** | **X.X** | **X.X** |

### SOC-CMM Radar Chart Data

Plot the five domain scores on a radar/spider chart for visual gap identification.

### Next Assessment: [Date — recommended quarterly]
### Assessment Review: [Who reviews and approves the assessment]
```

---

## SOC-CMM Improvement Roadmap Alignment

### Phase 1: Foundation (Target Level 2 across all domains)

| Domain | Key Actions | Pipeline Modules |
|---|---|---|
| Business | Draft SOC charter; secure initial budget; identify stakeholders | 01 Governance |
| People | Define roles; hire core team; start training | Workforce Roles |
| Process | Document incident handling; create initial use cases | 05 IR, 04 Detection |
| Technology | Deploy SIEM; begin log collection; deploy EDR to critical assets | 02 Data Doc |
| Services | Establish monitoring during business hours; basic IR capability | 04, 05 |

### Phase 2: Standardise (Target Level 3 — Pipeline delivers maximum value here)

| Domain | Key Actions | Pipeline Modules |
|---|---|---|
| Business | Ratify charter; align to NIST CSF; risk-based priorities; regular reporting | 01 Governance |
| People | Map roles to DoDCWF/ASD/CIISec; structured training; career paths; knowledge base | Workforce Roles |
| Process | OSSEM-CDM standardisation; ATT&CK threat profile; RE&CT playbooks; detection-as-code; CTI cycle | 02, 03, 04, 05 |
| Technology | SIEM normalised (OSSEM-CDM); DeTTECT coverage; Sigma rules; Atomic RT testing; TIP operational | 02, 04, 07 |
| Services | 24x7 monitoring; structured hunts; CTI products; monthly detection testing; D3FEND countermeasures | All modules |

### Phase 3: Measure (Target Level 4)

| Domain | Key Actions | Pipeline Modules |
|---|---|---|
| Business | Budget tied to outcomes; strategy reviews; compliance automated; ERM integration | 01 |
| People | Skill gap analysis drives investment; retention metrics; cross-training; ISAC participation | Workforce Roles |
| Process | SOAR automation; CI/CD detections; trend analysis; systematic post-incident improvement | 04, 05, 07 |
| Technology | Full log coverage; quality monitoring; automated testing; SOAR integrated; metrics dashboards | All |
| Services | Low FP detections; automated containment; hunt-to-detection pipeline; purple team quarterly | All |

### Phase 4: Optimise (Target Level 5)

| Domain | Key Actions | Pipeline Modules |
|---|---|---|
| Business | Predictive budgeting; SOC as business enabler; real-time compliance | 01 |
| People | Predictive workforce planning; community contribution; innovation time | Workforce Roles |
| Process | Self-adapting processes; predictive gap analysis; automated improvement loops | All |
| Technology | AI/ML augmentation; automated detection generation; self-tuning analytics | 04, 07 |
| Services | Predictive detection; continuous hunting; industry benchmarking; real-time dashboards | All |

---

## References

- [SOC-CMM](https://www.soc-cmm.com) — Rob van Os
- [SOC-CMM Self-Assessment Tool](https://www.soc-cmm.com/downloads/)
- [CIISec Skills Framework](https://www.ciisec.org/skills-framework)
- [MITRE 11 Strategies — Strategy 11: Turn up the Dial](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
- [ASD Cyber Skills Framework](https://www.cyber.gov.au/)
- [DoD 8140 / DoDCWF](https://public.cyber.mil/cw/dcwf/)
