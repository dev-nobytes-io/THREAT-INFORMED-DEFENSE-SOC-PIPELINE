# 05 — Incident Response

> **MITRE SOC Strategy Addressed:**
> - Strategy 5: Prioritize Incident Response

---

## Purpose

Incident response is the SOC's core operational function. This module uses the **RE&CT (Response Actions & Countermeasures Techniques)** framework to create structured, ATT&CK-aligned response playbooks. RE&CT provides a systematic taxonomy of response actions — the defensive counterpart to ATT&CK's offensive technique taxonomy.

---

## RE&CT Framework Overview

### What is RE&CT?

RE&CT maps response actions to a structured lifecycle, similar to how ATT&CK maps adversary actions. It answers: *"When we detect technique T1059.001, what specific response actions should we take?"*

### RE&CT Response Stages

```
┌─────────────────────────────────────────────────────────────────┐
│                    RE&CT RESPONSE STAGES                         │
├───────────┬───────────┬───────────┬───────────┬─────────────────┤
│PREPARATION│IDENTIFICA-│CONTAINMENT│ERADICATION│RECOVERY /       │
│           │TION       │           │           │LESSONS LEARNED  │
├───────────┼───────────┼───────────┼───────────┼─────────────────┤
│RA.PREP    │RA.IDENT   │RA.CONT   │RA.ERAD    │RA.RECV/RA.LL   │
│           │           │           │           │                 │
│Inventory  │List hosts │Block IP   │Remove     │Restore from     │
│Train staff│Get logs   │Block      │ malware   │ backup          │
│Set up     │Analyze    │ domain    │Reset      │Unblock          │
│ tools     │ events    │Isolate    │ accounts  │ services        │
│Practice   │Check IoCs │ host      │Patch vuln │Report findings  │
│ playbooks │Identify   │Disable    │Revoke     │Update playbooks │
│           │ accounts  │ account   │ certs     │                 │
└───────────┴───────────┴───────────┴───────────┴─────────────────┘
```

### RE&CT Response Action Categories

| Category | Code | Description | Example Actions |
|---|---|---|---|
| **Preparation** | RA.PREP | Pre-incident readiness | Practice playbooks, maintain toolkits, access management |
| **Identification** | RA.IDENT | Detect and confirm the incident | List processes, get logs, analyze network traffic, check IoCs |
| **Containment** | RA.CONT | Limit adversary movement | Block IP/domain, isolate host, disable account, quarantine file |
| **Eradication** | RA.ERAD | Remove adversary presence | Delete malware, remove persistence, reset credentials |
| **Recovery** | RA.RECV | Restore normal operations | Restore from backup, unblock services, re-enable accounts |
| **Lessons Learned** | RA.LL | Post-incident improvement | Write report, update detections, revise playbooks |

---

## ATT&CK-to-RE&CT Playbook Mapping

### Playbook Architecture

Each response playbook maps an ATT&CK technique to specific RE&CT actions:

```
ATT&CK Technique ──▶ Detection Alert ──▶ RE&CT Response Playbook
                                              │
                                              ├── Identification Actions
                                              ├── Containment Actions
                                              ├── Eradication Actions
                                              ├── Recovery Actions
                                              └── Lessons Learned Actions
```

### Playbook Template

```markdown
# Response Playbook: [ATT&CK Technique Name]

## Metadata
- **ATT&CK Technique:** T[xxxx.xxx] - [Name]
- **Tactic:** [Tactic Name]
- **Severity:** Critical / High / Medium / Low
- **Detection Source:** [Sigma rule ID, SIEM alert name]
- **Last Updated:** [Date]
- **Owner:** [SOC role]

---

## Stage 1: Identification (RA.IDENT)

| # | Action | RE&CT ID | Tool | Expected Output |
|---|---|---|---|---|
| 1 | List running processes on affected host | RA1101 | EDR console | Process tree |
| 2 | Get process command line arguments | RA1102 | EDR / Sysmon logs | Full cmdline |
| 3 | List network connections from host | RA1103 | EDR / netstat | Connection table |
| 4 | Check command line against known IoCs | RA1104 | TIP / SIEM lookup | Match / no match |
| 5 | Identify executing user account | RA1105 | SIEM / AD query | Account details |
| 6 | Check for lateral movement from host | RA1106 | SIEM query | Related alerts |

### Decision Point
- If IoC confirmed → Proceed to Containment
- If false positive → Document and close alert, tune detection
- If unclear → Escalate to Tier 2/3 for deeper analysis

---

## Stage 2: Containment (RA.CONT)

| # | Action | RE&CT ID | Tool | Authorization |
|---|---|---|---|---|
| 1 | Isolate affected host from network | RA2101 | EDR network isolation | Tier 2+ |
| 2 | Block identified C2 IP/domain | RA2201 | Firewall / DNS sinkhole | Tier 2+ |
| 3 | Disable compromised user account | RA2301 | Active Directory | Tier 2+ with manager approval |
| 4 | Quarantine identified malicious file | RA2401 | EDR quarantine | Tier 1+ |

---

## Stage 3: Eradication (RA.ERAD)

| # | Action | RE&CT ID | Tool | Verification |
|---|---|---|---|---|
| 1 | Remove malicious scheduled task/persistence | RA3101 | EDR / GPO | Confirm removal |
| 2 | Delete dropped files/tools | RA3201 | EDR remote shell | File system scan |
| 3 | Reset compromised credentials | RA3301 | AD / PAM | Credential rotation confirmed |
| 4 | Patch exploited vulnerability (if applicable) | RA3401 | Patch management | Scan verification |

---

## Stage 4: Recovery (RA.RECV)

| # | Action | RE&CT ID | Tool | Verification |
|---|---|---|---|---|
| 1 | Restore host from clean backup if needed | RA4101 | Backup system | System integrity check |
| 2 | Re-enable user account with new credentials | RA4201 | AD | User confirms access |
| 3 | Remove network isolation | RA4301 | EDR | Connectivity test |
| 4 | Unblock IPs/domains if false positive | RA4401 | Firewall | N/A |

---

## Stage 5: Lessons Learned (RA.LL)

| # | Action | RE&CT ID | Responsible |
|---|---|---|---|
| 1 | Document full incident timeline | RA5101 | IR Lead |
| 2 | Update detection rule (reduce FP / improve fidelity) | RA5201 | Detection Engineer |
| 3 | Update this playbook based on findings | RA5301 | IR Lead |
| 4 | Brief SOC team on new TTPs observed | RA5401 | CTI Analyst |
| 5 | Update threat profile (Module 03) if new technique observed | RA5501 | CTI Analyst |
```

---

## Example Playbooks

### Playbook: T1059.001 — PowerShell Execution

```
TRIGGER: SIEM alert "Suspicious PowerShell Invocation" fires

IDENTIFICATION:
  1. Pull Sysmon EventID 1 for the process (Image, CommandLine, ParentImage)
  2. Decode any Base64-encoded commands
  3. Check decoded content for known malicious patterns:
     - Invoke-Mimikatz, Invoke-WebRequest to unknown domains
     - AMSI bypass patterns
     - Credential harvesting commands
  4. Check parent process — is it expected? (Explorer, cmd, scheduled task)
  5. Query user account — is this a service account or interactive user?
  6. Check for related Sysmon 3 (network) events from the same process

DECISION: Malicious? → Containment | Benign? → Close + tune | Unclear? → Escalate

CONTAINMENT:
  1. Isolate host via EDR
  2. Block any identified C2 domains/IPs at firewall and DNS
  3. Disable user account if credential theft suspected

ERADICATION:
  1. Kill the PowerShell process
  2. Remove any persistence mechanisms (scheduled tasks, registry keys)
  3. Delete downloaded payloads
  4. Force credential reset for the affected user

RECOVERY:
  1. Scan host with updated signatures
  2. Remove network isolation after clean scan
  3. Re-enable user account with new credentials
  4. Monitor for 24 hours for reinfection

LESSONS LEARNED:
  1. Was the detection timely? Update MTTD metrics
  2. Were any sub-techniques not covered? → Update Module 04
  3. New IoCs discovered? → Feed back to Module 03
```

### Playbook: T1486 — Data Encrypted for Impact (Ransomware)

```
TRIGGER: EDR alert for mass file encryption activity OR SIEM alert
         for rapid file modification events

SEVERITY: CRITICAL — Immediate escalation

IDENTIFICATION:
  1. Identify affected host(s) and scope of encryption
  2. Determine ransomware variant (ransom note, file extension, behavior)
  3. Map lateral movement — what other hosts are at risk?
  4. Identify initial access vector (email, RDP, exploit)
  5. Determine if data exfiltration occurred (double extortion check)

CONTAINMENT (IMMEDIATE — do not wait for full identification):
  1. Isolate ALL affected hosts from network
  2. Disable potentially compromised service accounts
  3. Block known C2 infrastructure at all egress points
  4. Shut down file share access to prevent spread
  5. Activate incident commander — engage legal, communications, management

ERADICATION:
  1. Image affected systems for forensic analysis
  2. Identify and remove all persistence mechanisms
  3. Reset ALL potentially compromised credentials (broad scope)
  4. Patch initial access vulnerability

RECOVERY:
  1. Restore from verified clean backups (test backup integrity first)
  2. Rebuild systems that cannot be verified as clean
  3. Gradually restore network connectivity with monitoring
  4. Validate data integrity post-restoration

LESSONS LEARNED:
  1. Full incident report with ATT&CK mapping of entire kill chain
  2. Gaps in detection or response? → Update Modules 04, 06, 07
  3. Backup process adequate? → Update recovery procedures
  4. Communication plan effective? → Update Module 01 governance
```

---

## Escalation Matrix

| Severity | Criteria | Response Time | Escalation Path |
|---|---|---|---|
| **Critical** | Active ransomware, data breach, APT intrusion | Immediate | SOC → IR Lead → CISO → Executive + Legal |
| **High** | Confirmed malware, lateral movement, credential theft | 15 minutes | SOC → IR Lead → SOC Manager |
| **Medium** | Suspicious activity confirmed, single host | 1 hour | SOC Tier 1 → Tier 2 |
| **Low** | Policy violation, minor anomaly | 4 hours | SOC Tier 1 handles, documents |

---

## Outputs

| Output | Consumers | Description |
|---|---|---|
| Response Playbooks | SOC Analysts (all tiers) | Step-by-step response procedures per technique |
| Escalation Matrix | All SOC staff | Severity-based escalation procedures |
| Incident Reports | Module 01 (Governance), Module 03 (Intel) | Post-incident findings mapped to ATT&CK |
| Playbook Updates | Module 04 (Detection) | Detection gaps identified during response |
| Lessons Learned | All modules | Process improvements from real incidents |

---

## Implementation Checklist

- [ ] Review RE&CT framework action categories and IDs
- [ ] Create playbook templates aligned to RE&CT stages
- [ ] Build playbooks for all Critical-tier techniques from Module 03
- [ ] Build playbooks for all High-tier techniques from Module 03
- [ ] Define escalation matrix with severity criteria and response times
- [ ] Map RE&CT containment actions to available SOC tools (EDR, firewall, AD)
- [ ] Conduct tabletop exercise using playbooks
- [ ] Establish post-incident review process (lessons learned → all modules)
- [ ] Integrate playbooks with SOAR platform (if available)
- [ ] Schedule quarterly playbook review and update cycle

---

## References

- [RE&CT Framework](https://github.com/atc-project/atc-react)
- [RE&CT Navigator](https://atc-project.github.io/react-navigator/)
- [ATC RE&CT Action Descriptions](https://atc-project.github.io/atc-react/)
- [MITRE 11 Strategies — Strategy 5](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
