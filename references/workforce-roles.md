# Workforce Roles Reference

> Cross-mapping DoDCWF 8140 Work Roles to ASD Cyber Skills Framework Streams

This reference provides a unified view of SOC workforce requirements, mapping between the two governance frameworks used in Module 01.

---

## Role Mapping Matrix

| SOC Position | DoDCWF Work Role | DoDCWF ID | ASD Skill Stream | ASD Level | Pipeline Module Focus |
|---|---|---|---|---|---|
| SOC Tier 1 Analyst | Cyber Defense Analyst | PR-CDA-001 | Cyber Security Operations | 1-2 | 02, 04 |
| SOC Tier 2 Analyst | Cyber Defense Analyst | PR-CDA-001 | Cyber Security Operations | 2-3 | 02, 04, 05 |
| SOC Tier 3 / Senior Analyst | Cyber Defense Analyst | PR-CDA-001 | Cyber Security Operations | 3-4 | 03, 04, 05 |
| Detection Engineer | Cyber Defense Infrastructure Support | PR-INF-001 | Cyber Security Architecture | 3-4 | 02, 04, 07 |
| Threat Intelligence Analyst | All-Source Analyst | AN-ASA-001 | Cyber Threat Intelligence | 3-4 | 03 |
| Threat Hunter | Threat/Warning Analyst | AN-TWA-001 | Cyber Threat Intelligence | 4 | 02, 03, 04 |
| Incident Responder | Cyber Defense Incident Responder | PR-CIR-001 | Incident Response | 3-4 | 05 |
| Forensic Analyst | Cyber Defense Forensics Analyst | IN-FOR-002 | Incident Response | 4 | 05 |
| Purple Team Lead | Cyber Operations Planner | CO-OPL-001 | Vulnerability Assessment | 4-5 | 07 |
| SOC Engineer | Cyber Defense Infrastructure Support | PR-INF-001 | Cyber Security Architecture | 3-4 | 02, 04 |
| SOC Manager | Cyber Workforce Manager | OV-MGT-001 | Cyber Security Governance | 4-5 | 01 |
| Compliance Analyst | Security Control Assessor | SP-RSK-002 | Cyber Security Governance | 3-4 | 01 |

---

## Role Profiles

### SOC Tier 1 Analyst

```
Primary Function: Alert triage and initial investigation
DoDCWF: PR-CDA-001 | ASD: Cyber Security Operations Level 1-2

Key Responsibilities:
- Monitor SIEM alerts and triage based on severity
- Perform initial investigation using documented playbooks (Module 05)
- Escalate confirmed or unclear incidents to Tier 2
- Document all actions in case management system

Required KSAs (DoDCWF):
- K: Network protocols, common attack vectors, log sources
- S: SIEM query construction, basic log analysis, playbook execution
- A: Pattern recognition, attention to detail, clear communication

Pipeline Training Path:
- Module 02: Understand data sources and field meanings
- Module 04: Read and understand detection logic
- Module 05: Execute response playbooks (Stages 1-2)

Certifications (DoD 8140):
- CompTIA Security+
- CompTIA CySA+
- GIAC GSEC
```

### Detection Engineer

```
Primary Function: Build, test, and maintain detection analytics
DoDCWF: PR-INF-001 | ASD: Cyber Security Architecture Level 3-4

Key Responsibilities:
- Develop Sigma rules mapped to ATT&CK techniques (Module 04)
- Maintain DeTTECT coverage scoring (Module 04)
- Validate detections with Atomic Red Team (Module 07)
- Manage detection-as-code repository and CI/CD pipeline
- Tune detections to reduce false positives

Required KSAs (DoDCWF):
- K: ATT&CK framework, SIEM internals, log schemas (OSSEM), regex
- S: Query language (SPL/KQL/Sigma), scripting (Python/PowerShell), DeTTECT
- A: Analytical thinking, adversary perspective, systematic testing

Pipeline Training Path:
- Module 02: Deep OSSEM knowledge, data quality assessment
- Module 03: ATT&CK threat profile interpretation
- Module 04: DeTTECT, MITRE CAR, Sigma rule development
- Module 07: Atomic Red Team, attack_range validation

Certifications (DoD 8140):
- GIAC GCIA
- CompTIA CASP+
- SANS SEC555 / SEC599
```

### Threat Intelligence Analyst

```
Primary Function: Produce actionable, ATT&CK-mapped intelligence
DoDCWF: AN-ASA-001 | ASD: Cyber Threat Intelligence Level 3-4

Key Responsibilities:
- Maintain composite threat profile (Module 03)
- Produce ATT&CK Navigator layers for detection engineering
- Author intelligence products (briefs, reports, technique deep dives)
- Process and analyze threat feeds
- Brief SOC on threat landscape changes

Required KSAs (DoDCWF):
- K: ATT&CK framework, geopolitical context, intelligence cycle, STIX/TAXII
- S: Threat group analysis, technique extraction, Navigator layer creation
- A: Critical thinking, synthesis of disparate sources, clear writing

Pipeline Training Path:
- Module 03: Full ATT&CK profiling workflow
- Module 02: Understanding data source implications of techniques
- Module 04: How intelligence drives detection priorities

Certifications (DoD 8140):
- GIAC GCTI
- CREST CRTIA
- CompTIA CTIA
```

### Threat Hunter

```
Primary Function: Hypothesis-driven proactive threat discovery
DoDCWF: AN-TWA-001 | ASD: Cyber Threat Intelligence Level 4

Key Responsibilities:
- Develop hunt hypotheses from threat profile (Module 03)
- Execute hunts using Threat Hunters Playbook methodology (Module 02)
- Discover new detection opportunities → feed to Detection Engineering
- Validate data source quality during hunts (Module 02)
- Contribute findings to threat intelligence (Module 03)

Required KSAs (DoDCWF):
- K: Deep ATT&CK knowledge, advanced query languages, statistics, attacker TTPs
- S: Hypothesis formation, large-scale data analysis, anomaly identification
- A: Creative thinking, persistence, adversarial mindset

Pipeline Training Path:
- Module 02: OSSEM data model mastery, Threat Hunters Playbook
- Module 03: Threat profiling for hunt hypothesis generation
- Module 04: Detection gap analysis to focus hunts
- Module 07: Atomic Red Team for generating ground-truth data

Certifications (DoD 8140):
- GIAC GCFA
- GIAC GREM
- OSCP (for adversary mindset)
```

---

## Minimum Staffing Models

### Small SOC (8x5)

| Role | Headcount | Notes |
|---|---|---|
| SOC Manager | 1 | Also handles governance/compliance |
| Tier 1 Analyst | 2 | Rotating shifts |
| Tier 2 Analyst | 1 | Senior analyst, also threat hunts |
| Detection Engineer | 1 | Part-time IR support |
| **Total** | **5** | |

### Medium SOC (16x5)

| Role | Headcount | Notes |
|---|---|---|
| SOC Manager | 1 | |
| Tier 1 Analyst | 4 | Two shifts |
| Tier 2 Analyst | 2 | Investigation and escalation |
| Detection Engineer | 2 | One focused on SIEM, one on EDR |
| Threat Intel Analyst | 1 | |
| Incident Responder | 1 | |
| **Total** | **11** | |

### Large SOC (24x7)

| Role | Headcount | Notes |
|---|---|---|
| SOC Manager | 1 | |
| Shift Leads | 4 | One per shift (4 shifts for 24x7) |
| Tier 1 Analyst | 8 | Two per shift |
| Tier 2 Analyst | 4 | One per shift |
| Tier 3 / Senior Analyst | 2 | Day shift, on-call rotation |
| Detection Engineer | 3 | Day shift |
| Threat Intel Analyst | 2 | Day shift |
| Threat Hunter | 2 | Day shift |
| Incident Responder | 2 | Day shift, on-call rotation |
| Forensic Analyst | 1 | Day shift, on-call |
| Purple Team Lead | 1 | Day shift |
| SOC Engineer | 2 | Day shift |
| Compliance Analyst | 1 | Day shift |
| **Total** | **33** | |

---

## Career Progression Paths

```
Entry                    Mid-Career                Senior / Leadership
─────────────────────────────────────────────────────────────────────

Tier 1 Analyst ────────▶ Tier 2 Analyst ─────────▶ Tier 3 / Senior
    │                        │                          │
    │                        ▼                          ▼
    │                   Detection Engineer ────────▶ Lead Detection Eng.
    │                        │
    ▼                        ▼
Tier 1 Analyst ────────▶ Threat Intel Analyst ───▶ CTI Lead
    │                        │
    │                        ▼
    │                   Threat Hunter ────────────▶ Hunt Team Lead
    │
    ▼
Tier 1 Analyst ────────▶ Incident Responder ─────▶ IR Lead
                             │
                             ▼
                        Forensic Analyst ─────────▶ DFIR Lead

All Senior Roles ──────────────────────────────────▶ SOC Manager
                                                        │
                                                        ▼
                                                     CISO / Director
```
