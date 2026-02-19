# M3TID & The Continuous Hunt

> **Core Philosophy:** Threat-informed defense is an everlasting baseline hunt with detection engineering CI/CD.

---

## What is M3TID?

**M3TID** — the Methodology for Threat-Informed Defense — is the structured approach developed by MITRE Engenuity's [Center for Threat-Informed Defense (CTID)](https://ctid.mitre-engenuity.org/) for applying threat intelligence to defensive operations. It operationalises the three pillars of threat-informed defense:

| Pillar | Description | Pipeline Implementation |
|---|---|---|
| **Cyber Threat Intelligence** | Understand adversary behaviour through ATT&CK-mapped intelligence | Module 03 (Threat Intelligence) |
| **Defensive Engagement** | Continuously test and validate defenses against realistic adversary behaviour | Module 07 (Detection Testing), Module 04 (Detection Engineering) |
| **Focused Sharing** | Collaborate with the community to collectively raise the bar | Module 01 (Governance — MITRE Strategy 9), external sharing |

### M3TID Cycle

```mermaid
flowchart TD
    A["1. ANALYSE THREATS\nATT&CK-mapped threat profile"]
    B["2. ASSESS DEFENSES\nMap current detection &\nprevention coverage"]
    C["3. IDENTIFY GAPS\nCoverage gaps\nData gaps\nProcess gaps"]
    D["3b. TEST DEFENSES\nAtomic RT, attack_range,\npurple team"]
    E["4. IMPROVE DEFENSES\nNew detections\nNew playbooks\nNew counters\nUpdated hunts"]
    F["5. SHARE FINDINGS\nCommunity, ISACs, peers"]

    A -->|"Threat intelligence identifies\nadversary behaviours relevant\nto your environment"| B
    B --> C
    C -->|"Gaps drive testing,\nhunting, and engineering"| D
    D --> E
    E --> F
    F -->|"This cycle never stops"| A
```

---

## The Everlasting Baseline Hunt

### The Core Insight

Every detection rule running in your SIEM is a **codified hunt hypothesis executing continuously**. A Sigma rule that looks for `powershell.exe` spawned by `winword.exe` was once a threat hunter's hypothesis about adversary behaviour. Now it runs 24/7, hunting for that pattern in every event that flows through the SIEM.

This means:

- **Threat hunting and detection engineering are the same discipline at different stages of maturity**
- A hunt hypothesis that validates becomes a detection rule
- A detection rule that fires becomes an incident
- An incident that resolves feeds new hypotheses

The pipeline is not a sequence of one-time activities. It is **an everlasting hunt** — the threat landscape changes, your environment changes, your data changes, and your understanding deepens. The cycle never completes.

```mermaid
flowchart TD
    subgraph EXPLICIT["EXPLICIT HUNTING (Human-driven)"]
        E1["Analyst forms hypothesis\nfrom CTI + data analysis"]
        E2["Analyst runs queries\nagainst collected data"]
        E3["Analyst finds evidence\n(or doesn't)"]
        E4["Findings feed back:\n- New detections → Module 04\n- Updated threat profile → Mod 03\n- Data gaps identified → Mod 02\n- New hypotheses formed"]
        E1 --> E2 --> E3 --> E4
    end

    subgraph IMPLICIT["IMPLICIT HUNTING (Automated — Detection CI/CD)"]
        I1["Validated hypothesis becomes\nSigma rule in detection repo"]
        I2["Rule runs continuously in SIEM\nagainst all incoming events"]
        I3["Alert fires when pattern matches"]
        I4["Incident response handles alert"]
        I5["Lessons learned feed back:\n- Detection tuning\n- New hypotheses\n- Threat profile updates"]
        I1 --> I2 --> I3 --> I4 --> I5
    end

    E1 -.-> I1
    E2 -.-> I2
    E3 -.-> I3
    E4 -->|"CYCLE"| I5
    I5 -->|"CYCLE"| E1

    style EXPLICIT fill:#f0f4ff,stroke:#335
    style IMPLICIT fill:#f4fff0,stroke:#353
```

---

## Pre-Hunt Activities (Full Methodology)

The Threat Hunters Playbook defines six pre-hunt activities that must be established before any hunt can succeed. The first four (data management) are covered in Module 02. The remaining two (hypothesis generation and analytics development) bridge Modules 03 and 04.

### All Six Pre-Hunt Activities

```mermaid
flowchart TD
    subgraph DM["DATA MANAGEMENT (Module 02)"]
        D1["1. Data Documentation → OSSEM-DD data dictionaries"]
        D2["2. Data Standardisation → OSSEM-CDM #123;prefix#125;_#123;attribute#125;"]
        D3["3. Data Modelling → OSSEM-DM entity relationships"]
        D4["4. Data Quality → Completeness, consistency, time"]
        D1 --- D2 --- D3 --- D4
    end

    subgraph HG["HYPOTHESIS GENERATION (Module 03)"]
        H["5. Hypothesis Generation → Intelligence,\nsituational, or analytics-driven hypotheses"]
    end

    subgraph AD["ANALYTICS DEVELOPMENT (Module 04)"]
        A["6. Analytics Development → Queries to test\nhypothesis; promote to production detections"]
    end

    EX["HUNT EXECUTION\nDETECTION DEPLOYMENT\nCONTINUOUS TESTING"]

    DM --> HG --> AD --> EX
```

---

## Pre-Hunt 5 — Hypothesis Generation

A hunt hypothesis is a testable statement about adversary behaviour in your environment. Without a hypothesis, a hunt is just a fishing expedition.

### Three Hypothesis Types

| Type | Source | Trigger | Example |
|---|---|---|---|
| **Intelligence-Driven** | CTI reports, ATT&CK group profiles, threat advisories, ISAC alerts | New threat group targets your sector; new technique published; incident at peer organisation | *"APT29 has been observed using T1059.001 with encoded PowerShell commands against organisations in our sector. If they target us, we should observe encoded PowerShell execution from abnormal parent processes."* |
| **Situational-Awareness-Driven** | Environmental knowledge, new data sources, infrastructure changes, anomalous baselines | New cloud environment deployed; new application onboarded; baseline deviation detected; vulnerability announced | *"We recently migrated to Azure AD. Adversaries targeting cloud environments use T1078.004 (Cloud Accounts). We should hunt for anomalous sign-in patterns from unfamiliar locations."* |
| **Analytics-Driven** | Data analysis, statistical baselines, ML anomaly outputs, stack counting | Unusual process frequency; new process names; statistical outlier in authentication patterns | *"Stack counting of parent-child process relationships reveals an unusual combination: outlook.exe → cmd.exe → certutil.exe. This matches T1105 (Ingress Tool Transfer) and warrants investigation."* |

### Hypothesis Format

Every hypothesis should be documented in a standard format:

```markdown
## Hunt Hypothesis: [ID]

### Hypothesis Statement
If [adversary / threat type] targets [asset / system] using [ATT&CK technique],
we should observe [specific data pattern] in [data source].

### Type
[Intelligence-Driven / Situational / Analytics-Driven]

### ATT&CK Mapping
- **Technique:** T[xxxx.xxx] — [Name]
- **Tactic:** [Tactic Name]
- **Data Sources:** [ATT&CK DS IDs]
- **Data Components:** [Specific components]

### Entity Relationships Required (OSSEM-DM)
| Source | Relationship | Target |
|---|---|---|
| Process | created | Process |
| Process | connected to | IP |

### Required Data (from Module 02)
- **Events:** [Sysmon 1, Security 4688, etc.]
- **CDM Fields:** [process_name, process_parent_name, process_command_line, etc.]
- **Data Quality Score:** [Current score from Module 02]
- **Hunt-Readiness:** [Ready / Not Ready — why]

### Analytics (initial queries)
[SQL / KQL / SPL queries to test the hypothesis]

### Expected Outcomes
- **If confirmed:** → Escalate to IR (Module 05); create detection rule (Module 04)
- **If not confirmed:** → Document negative finding; adjust hypothesis or data requirements
- **Either way:** → Update threat profile (Module 03); document findings

### Priority
[Critical / High / Medium / Low] — based on Module 03 threat profile priority

### Source
[CTI report link / Environmental trigger / Data analysis output]
```

### Hypothesis Backlog

Maintain a prioritised backlog of hunt hypotheses, sourced from:

| Source | Feeding Framework | How Hypotheses Are Generated |
|---|---|---|
| Module 03 threat profile | ATT&CK | Priority techniques without detection coverage → "Can we find evidence of T[xxxx] in our data?" |
| Module 04 DeTTECT gaps | DeTTECT | Low-scoring techniques → "Let's hunt for T[xxxx] to understand what the data looks like before writing a detection" |
| Module 05 incident findings | RE&CT Lessons Learned | Incident revealed technique not in profile → "Is this technique active elsewhere in our environment?" |
| Module 07 test results | Atomic Red Team | Technique that bypassed detection → "Can we find the bypass pattern in historical data?" |
| CTI feeds | External | New threat advisory → "Is this adversary already in our environment?" |
| Environmental changes | Internal | New system deployed → "What does the attack surface look like for this new asset?" |
| Data analysis | SIEM / notebooks | Statistical anomaly → "What is this unusual pattern — benign or malicious?" |

---

## Pre-Hunt 6 — Analytics Development

Analytics development bridges the gap between a hunt hypothesis and a production detection. It is the process of creating, testing, and refining the queries that test a hypothesis.

### The Analytics Maturity Spectrum

```
HUNT QUERY                VALIDATED ANALYTIC         PRODUCTION DETECTION
(one-time, manual)   →    (tested, documented)   →   (automated, CI/CD deployed)

Ad-hoc SQL in            Jupyter notebook with       Sigma rule in detection-
a SIEM search bar        documented logic,           as-code repo, validated
                         tested against Security     by Atomic RT, deployed
                         Datasets (Mordor),          via CI/CD to SIEM,
                         peer-reviewed               scored in DeTTECT
```

### Analytics Development Workflow

```mermaid
flowchart LR
    S1["1. HYPOTHESISE\nForm the hunt hypothesis\n(Pre-Hunt 5)"]
    S2["2. EXPLORE\nExplore data using\nad-hoc queries in the SIEM"]
    S3["3. DEVELOP\nBuild a structured analytic\n(Jupyter notebook or\nSigma rule draft)"]
    S4["4. VALIDATE\nTest analytic against\nSecurity Datasets (Mordor)\nand Atomic RT"]
    S5["5. PROMOTE\nConvert validated analytic\nto Sigma rule format;\npeer review"]
    S6["6. DEPLOY\nDeploy to SIEM via CI/CD\npipeline; update DeTTECT\nscore; monitor FPs"]
    S7["7. FEEDBACK\nUpdate DeTTECT scores,\nModule 03 threat profile,\nModule 02 data quality"]

    S1 --> S2 --> S3 --> S4 --> S5 --> S6 --> S7
    S7 -->|"The detection now runs continuously —\nan automated, everlasting instance\nof the original hunt hypothesis"| S1
```

### From Hunt to Detection: The Promotion Criteria

A hunt analytic should be promoted to a production detection when:

| Criterion | Threshold | How to Verify |
|---|---|---|
| **True Positive Rate** | Fires on known-bad data (Security Datasets / Atomic RT) | Test against validation datasets |
| **False Positive Rate** | < 10% on production data baseline (2-week observation) | Run as alert-only (no action) for 2 weeks |
| **Data Dependency** | Required data sources scored ≥ 4 (Module 02 quality rubric) | Check data quality scorecards |
| **ATT&CK Mapping** | Mapped to specific technique and sub-technique | Verify in Sigma rule `tags` field |
| **Peer Review** | Reviewed by at least one other analyst/engineer | Code review in detection repo |
| **Documentation** | Hypothesis, logic, expected FPs, and tuning guidance documented | Sigma rule `description` + linked notebook |

---

## Detection Engineering CI/CD as Continuous Hunting

The detection-as-code CI/CD pipeline is the mechanism that transforms the continuous hunt into an automated, self-improving system.

```mermaid
flowchart LR
    COMMIT["<b>COMMIT</b><br/>New/modified<br/>Sigma rule"]
    BUILD["<b>BUILD</b><br/>Sigma compile<br/>to SIEM format"]
    TEST["<b>TEST</b><br/>Atomic Red Team<br/>validate detection"]
    DEPLOY["<b>DEPLOY</b><br/>Push to SIEM"]
    MONITOR["<b>MONITOR</b><br/>FP rate,<br/>alert volume"]
    SCORE["<b>SCORE</b><br/>DeTTECT score<br/>update"]
    REPORT["<b>REPORT</b><br/>Coverage metrics<br/>to Module 01"]
    FEEDBACK["<b>FEEDBACK</b><br/>Update threat<br/>profile"]

    COMMIT --> BUILD --> TEST --> DEPLOY
    DEPLOY --> MONITOR --> SCORE --> REPORT --> FEEDBACK
    FEEDBACK -->|"Cycle repeats"| COMMIT
```

> *Every detection deployed is a hunt hypothesis running permanently.*
> *Every test validates the hypothesis still works.*
> *Every alert is a hunt finding requiring triage.*
> *Every incident feeds new hypotheses.*
>
> ***THE CYCLE NEVER STOPS.***

---

## The Complete Continuous Hunt Cycle

Integrating M3TID, all six pre-hunt activities, detection engineering CI/CD, and incident response into a single continuous cycle:

```mermaid
flowchart TD
    ANALYSE["<b>M3TID: ANALYSE THREATS</b><br/>Module 03: ATT&CK threat profile"]
    DATA["<b>PRE-HUNT: DATA MANAGEMENT</b><br/>Module 02: DD → CDM → DM → Quality"]
    HYPO["<b>PRE-HUNT: HYPOTHESIS GENERATION</b><br/>Intelligence / Situational / Analytics"]
    ANALYTICS["<b>PRE-HUNT: ANALYTICS DEVELOPMENT</b><br/>Hunt queries → validated analytics"]
    EXPLICIT["<b>EXPLICIT HUNT</b><br/>Analyst-driven"]
    IMPLICIT["<b>IMPLICIT HUNT</b><br/>Detection CI/CD"]
    FINDINGS["FINDINGS"]
    ALERTS["ALERTS"]
    ASSESS["<b>M3TID: ASSESS DEFENSES</b><br/>Module 07: Testing<br/>+ DeTTECT scoring"]
    GAPS["<b>M3TID: IDENTIFY GAPS</b><br/>Coverage analysis<br/>+ data gaps + process gaps"]
    IMPROVE["<b>M3TID: IMPROVE DEFENSES</b><br/>Module 04: new detections<br/>Module 05: new playbooks<br/>Module 06: new countermeasures"]
    SHARE["<b>M3TID: SHARE FINDINGS</b><br/>Community, ISACs,<br/>internal teams"]

    ANALYSE --> DATA --> HYPO --> ANALYTICS
    ANALYTICS --> EXPLICIT
    ANALYTICS --> IMPLICIT
    EXPLICIT --> FINDINGS
    IMPLICIT --> ALERTS
    FINDINGS --> ASSESS
    ALERTS --> ASSESS
    ASSESS --> GAPS --> IMPROVE --> SHARE
    SHARE -->|"Continuous cycle"| ANALYSE
```

---

## Operationalising the Continuous Hunt

### Hunt Cadence

| Activity | Frequency | Owner | Output |
|---|---|---|---|
| **Hypothesis generation** from CTI | Weekly | Threat Intel Analyst | New hypotheses added to backlog |
| **Hunt execution** (structured) | Weekly (1-2 hunts) | Threat Hunter | Hunt reports; new detection candidates |
| **Analytics development** (hunt → detection) | Per hunt finding | Detection Engineer | Sigma rules promoted to detection repo |
| **Detection validation** (Atomic RT) | Per new detection + weekly smoke | Detection Engineer / CI/CD | Test results updating DeTTECT |
| **Coverage assessment** (DeTTECT) | Monthly | Detection Engineer | Updated Navigator layers |
| **Purple team exercise** | Quarterly | Purple Team Lead | Full kill chain validation report |
| **Threat profile review** | Quarterly | Threat Intel Analyst | Updated composite threat profile |
| **SOC-CMM assessment** | Quarterly | SOC Manager | Maturity scores across all domains |
| **M3TID cycle review** | Quarterly | SOC Manager + Team | Full cycle effectiveness review |

### Metrics That Matter

| Metric | What It Measures | Target |
|---|---|---|
| **Hypotheses generated per month** | Intelligence-to-action conversion rate | ≥ 4 |
| **Hunt-to-detection conversion rate** | % of hunts that produce a new production detection | ≥ 30% |
| **Mean time from hypothesis to production detection** | Speed of the hunt-to-detection pipeline | < 2 sprints |
| **Detection coverage (DeTTECT)** | % of priority techniques with detection score ≥ 3 | ≥ 80% |
| **Detection validation pass rate** | % of detections that fire correctly on Atomic RT | ≥ 90% |
| **False positive rate** | Alerts that are not true positives | < 10% per rule |
| **MTTD (Mean Time to Detect)** | Time from technique execution to alert | < 5 minutes |
| **MTTR (Mean Time to Respond)** | Time from alert to containment | Trending down |
| **Data quality score** | Average quality across priority data sources | ≥ 4.0 |
| **SOC-CMM score** | Overall maturity across all domains | Trending up |

---

## Pipeline Modules as Continuous Hunt Stages

Reframing the seven pipeline modules through the lens of the continuous hunt:

| Pipeline Module | Continuous Hunt Role | What It Provides |
|---|---|---|
| **01 Governance** | **Hunt Authority** | Mandate, budget, staffing, and stakeholder support for continuous operations |
| **02 Data Documentation** | **Hunt Foundation** | The data management disciplines (DD, CDM, DM, Quality) that make data huntable |
| **03 Threat Intelligence** | **Hunt Direction** | CTI-driven hypothesis generation; threat profile that focuses the hunt |
| **04 Detection Engineering** | **Hunt Codification** | The pipeline that promotes validated hunt findings into permanent, automated detections |
| **05 Incident Response** | **Hunt Activation** | When an automated detection (codified hunt) fires, IR executes the response |
| **06 Countermeasures** | **Hunt Hardening** | For techniques that can't be hunted/detected reliably, countermeasures prevent them |
| **07 Detection Testing** | **Hunt Validation** | Continuous verification that codified hunts (detections) still work |

---

## References

- [MITRE Engenuity Center for Threat-Informed Defense](https://ctid.mitre-engenuity.org/)
- [Threat-Informed Defense — MITRE](https://www.mitre.org/focus-areas/threat-informed-defense)
- [Threat Hunters Playbook — Pre-Hunt Methodology](https://threathunterplaybook.com/pre-hunt/)
- [Threat Hunters Playbook — Data Management](https://threathunterplaybook.com/pre-hunt/data_management.html)
- [Ready to Hunt? First, Show Me Your Data! — Roberto Rodriguez](https://posts.specterops.io/ready-to-hunt-first-show-me-your-data-a642c6b170d6)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [SOC-CMM](https://www.soc-cmm.com)
- [MITRE 11 Strategies of a World-Class SOC](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
