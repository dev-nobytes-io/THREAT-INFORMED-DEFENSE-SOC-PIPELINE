# 07 — Detection Testing

> **MITRE SOC Strategy Addressed:**
> - Strategy 10: Measure Performance to Improve Performance

---

## Purpose

Detection testing closes the feedback loop. Without systematic testing, the SOC operates on assumptions about detection coverage. This module uses **Atomic Red Team** and **Splunk's attack_range.py** to continuously validate that detections actually fire against real adversary techniques and that countermeasures actually block them.

---

## Testing Stack

| Tool | Role | Scope |
|---|---|---|
| **Atomic Red Team** | Individual technique execution on live or lab endpoints | Per-technique unit testing |
| **Splunk attack_range** | Full attack simulation lab with infrastructure provisioning | End-to-end scenario testing |

### How They Complement Each Other

```
Atomic Red Team                        attack_range.py
──────────────────                     ──────────────────
Granularity:  Single technique         Granularity:  Multi-step attack chain
Environment:  Existing endpoints       Environment:  Purpose-built cloud lab
              or lab systems                         (AWS/Azure provisioned)
Speed:        Fast (seconds per test)  Speed:        Slower (infra spin-up)
Use case:     Detection unit tests     Use case:     Purple team exercises,
              CI/CD validation                       full kill chain simulation
SIEM needed:  Yes (existing)           SIEM needed:  Splunk (built-in)
```

---

## Atomic Red Team Integration

### What is Atomic Red Team?

Atomic Red Team provides small, discrete test procedures ("atomics") for individual ATT&CK techniques. Each atomic test is:

- Mapped to a specific ATT&CK technique ID
- Self-contained (can run independently)
- Documented with prerequisites, commands, and cleanup steps

### Atomic Testing Workflow

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ 1. SELECT    │───▶│ 2. EXECUTE   │───▶│ 3. VALIDATE  │───▶│ 4. RECORD    │
│              │    │              │    │              │    │              │
│ Pick tech-   │    │ Run atomic   │    │ Check SIEM:  │    │ Update       │
│ niques from  │    │ test on      │    │ Did alert    │    │ DeTTECT      │
│ Module 03    │    │ target host  │    │ fire?        │    │ scores       │
│ threat       │    │              │    │ Right fields?│    │ (Module 04)  │
│ profile      │    │              │    │ Right        │    │              │
│              │    │              │    │ severity?    │    │              │
└──────────────┘    └──────────────┘    └──────────────┘    └──────────────┘
       │                                       │
       │                                       ▼
       │                                ┌──────────────┐
       │                                │ 5. REMEDIATE │
       │                                │              │
       │                                │ Detection    │
       │                                │ failed?      │
       │                                │ → Module 04  │
       │                                │   fix rule   │
       │                                │              │
       │                                │ Countermeas. │
       │                                │ bypassed?    │
       │                                │ → Module 06  │
       │                                │   harden     │
       └────────────────────────────────┘──────────────┘
                    REPEAT CYCLE
```

### Running Atomic Tests

#### Using Invoke-AtomicRedTeam (PowerShell)

```powershell
# Install
Install-Module -Name invoke-atomicredteam -Scope CurrentUser

# List available tests for a technique
Invoke-AtomicTest T1059.001 -ShowDetailsBrief

# Execute a specific test
Invoke-AtomicTest T1059.001 -TestNumbers 1

# Execute all tests for a technique
Invoke-AtomicTest T1059.001

# Run prerequisites check before executing
Invoke-AtomicTest T1059.001 -CheckPrereqs

# Get prerequisites
Invoke-AtomicTest T1059.001 -GetPrereqs

# Cleanup after testing
Invoke-AtomicTest T1059.001 -Cleanup
```

#### Using atomic-operator (Python — cross-platform)

```bash
# Install
pip install atomic-operator

# Run a technique
atomic-operator run --techniques T1059.001

# Run with specific test
atomic-operator run --techniques T1059.001 --test-guids f66e691e-d90a-4c45-aca0-c85f2f0e9b32
```

### Test Result Documentation Template

```markdown
## Atomic Test Result: T1059.001 — PowerShell

### Test Execution
- **Date:** 2024-01-20
- **Tester:** [Name]
- **Environment:** [Lab / Production-equivalent]
- **Host:** [hostname]
- **Atomic Test #:** 1 — "Mimikatz - Invoke-Mimikatz"
- **Test GUID:** f66e691e-d90a-4c45-aca0-c85f2f0e9b32

### Detection Validation

| Check | Result | Notes |
|---|---|---|
| SIEM alert triggered? | YES / NO | Alert name: [x] |
| Correct technique ID in alert? | YES / NO | |
| Correct severity? | YES / NO | Expected: High, Got: [x] |
| Key fields populated? | YES / NO | CommandLine, ParentImage, User |
| Time to alert (MTTD)? | [seconds] | From execution to SIEM alert |
| False positive potential? | LOW / MED / HIGH | |

### Countermeasure Validation (if applicable)

| Check | Result | Notes |
|---|---|---|
| Execution blocked? | YES / NO | By: [AppLocker / WDAC / EDR] |
| Block alert generated? | YES / NO | |

### Outcome
- [ ] PASS — Detection fired correctly
- [ ] PARTIAL — Detection fired but missing data/context
- [ ] FAIL — No detection
- [ ] BLOCKED — Countermeasure prevented execution (Module 06 success)

### Action Items
- [ ] [Describe any fixes needed for detection or countermeasure]
```

---

## Splunk attack_range.py Integration

### What is attack_range?

Splunk's attack_range provisions a complete attack simulation environment in AWS or locally (via Vagrant). It creates:

- Windows domain controllers and workstations
- Linux servers
- Splunk instance with pre-configured data inputs
- Kali Linux attack host
- Pre-configured with Sysmon, Windows Event Forwarding, etc.

### attack_range Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    attack_range Lab                       │
│                                                          │
│  ┌──────────┐   ┌──────────┐   ┌──────────────────────┐│
│  │  Kali    │   │ Windows  │   │  Splunk Enterprise   ││
│  │  Attack  │──▶│ Domain   │──▶│  (log collection)    ││
│  │  Host    │   │ (DC+WS)  │   │                      ││
│  └──────────┘   └──────────┘   │  Pre-configured:     ││
│                  ┌──────────┐   │  - Sysmon logs       ││
│                  │  Linux   │──▶│  - WinEventLog       ││
│                  │  Server  │   │  - Zeek logs         ││
│                  └──────────┘   │  - Detection rules   ││
│                                 └──────────────────────┘│
└─────────────────────────────────────────────────────────┘
```

### Setting Up attack_range

```bash
# Clone the repository
git clone https://github.com/splunk/attack_range.git
cd attack_range

# Install dependencies
pip install -r requirements.txt

# Configure (edit attack_range.yml)
# Key settings:
#   - Cloud provider (AWS/Azure) or local (Vagrant)
#   - Number of Windows/Linux hosts
#   - Splunk version
#   - Sysmon configuration

# Build the range
python attack_range.py build

# Run a specific ATT&CK simulation
python attack_range.py simulate -st T1059.001 -t attack-range-windows-domain-controller

# Run Atomic Red Team test within the range
python attack_range.py simulate -e atomic_red_team -st T1059.001

# Search Splunk for detection results
python attack_range.py search -s "index=main sourcetype=sysmon EventCode=1 powershell"

# Dump logs for offline analysis
python attack_range.py dump -dn attack-range-domain-controller

# Tear down the range
python attack_range.py destroy
```

### attack_range Testing Scenarios

#### Scenario 1: Single Technique Validation

```bash
# Simulate T1059.001 (PowerShell) on a Windows target
python attack_range.py simulate -st T1059.001 \
  -t attack-range-windows-domain-controller

# Check Splunk for detection
python attack_range.py search \
  -s "index=main sourcetype=sysmon EventCode=1 Image=*powershell* | table _time Image CommandLine User"
```

#### Scenario 2: Kill Chain Simulation

```bash
# Simulate a multi-step attack chain
# Step 1: Initial Access via spearphishing (T1566.001)
python attack_range.py simulate -st T1566.001 -t attack-range-windows-client

# Step 2: Execution via PowerShell (T1059.001)
python attack_range.py simulate -st T1059.001 -t attack-range-windows-client

# Step 3: Credential dumping (T1003.001)
python attack_range.py simulate -st T1003.001 -t attack-range-windows-domain-controller

# Step 4: Lateral movement (T1021.002)
python attack_range.py simulate -st T1021.002 -t attack-range-windows-domain-controller

# Validate: Did the full chain get detected? Any gaps?
```

#### Scenario 3: Purple Team Exercise

```
Purple Team Exercise Plan
─────────────────────────
Objective: Validate detection of [Threat Group] TTPs

1. PRE-EXERCISE
   - Build attack_range environment
   - Deploy current Sigma rules to Splunk
   - Brief red team on approved technique scope
   - Brief blue team that exercise is occurring

2. EXECUTION
   - Red team executes techniques from threat profile (Module 03)
   - Blue team monitors SIEM in real-time
   - Document: which alerts fired, which were missed

3. POST-EXERCISE
   - Compare red team activity log vs. blue team detections
   - Calculate detection coverage percentage
   - Identify gaps → feed back to Module 04
   - Update DeTTECT scores with validated results

4. REPORTING
   - ATT&CK Navigator layer showing tested techniques
   - Green = detected, Red = missed, Yellow = partial
   - Action items for each gap
```

---

## Detection Coverage Metrics

### Key Performance Indicators

| Metric | Definition | Target |
|---|---|---|
| **Detection Coverage Ratio** | (Techniques with score >= 3) / (Total priority techniques) | > 80% |
| **Mean Time to Detect (MTTD)** | Average time from atomic execution to SIEM alert | < 5 minutes |
| **False Positive Rate** | (False alerts) / (Total alerts) per detection rule | < 10% |
| **Test Pass Rate** | (Atomic tests with PASS result) / (Total atomic tests run) | > 75% |
| **Countermeasure Block Rate** | (Techniques blocked by D3FEND controls) / (Techniques tested) | Increasing trend |
| **Time to Remediate Detection** | Time from FAIL result to updated detection deployed | < 1 sprint |

### Coverage Dashboard

```
Detection Coverage Report — [Date]
══════════════════════════════════════════════════════════
Threat Profile Techniques:        45
Techniques Tested This Cycle:     30
──────────────────────────────────────────────────────────
PASS (detected correctly):        22  ████████████████████ 73%
PARTIAL (detected, gaps exist):    4  ████                 13%
FAIL (not detected):               2  ██                    7%
BLOCKED (countermeasure worked):   2  ██                    7%
──────────────────────────────────────────────────────────
Coverage Score:                   87% (PASS + BLOCKED) / Tested
──────────────────────────────────────────────────────────

Action Items:
- FAIL: T1055.001 (Process Injection - DLL) → Module 04: write detection
- FAIL: T1218.011 (Rundll32) → Module 06: deploy AppLocker rule
- PARTIAL: T1053.005 (Sched Task) → Module 04: cover COM-based creation
```

---

## Testing Cadence

| Test Type | Frequency | Scope |
|---|---|---|
| **Atomic Smoke Tests** | Weekly (automated) | Top 10 critical techniques |
| **Sprint Validation** | Per detection sprint | Newly deployed detections |
| **Full Coverage Scan** | Monthly | All priority techniques |
| **Purple Team Exercise** | Quarterly | Full kill chain scenarios |
| **Countermeasure Validation** | After each deployment | All active countermeasures |

### Automation: CI/CD Detection Testing

```yaml
# Example: GitHub Actions workflow for detection testing
name: Detection Validation

on:
  push:
    paths:
      - 'detections/sigma/**'
  schedule:
    - cron: '0 6 * * 1'  # Weekly Monday 6 AM

jobs:
  validate-detections:
    runs-on: self-hosted  # On a test endpoint with Atomic RT installed
    steps:
      - name: Checkout detections repo
        uses: actions/checkout@v4

      - name: Deploy updated Sigma rules to test SIEM
        run: |
          sigma convert -t splunk detections/sigma/ -o splunk_rules/
          # Deploy to test SIEM instance

      - name: Run Atomic tests for modified techniques
        run: |
          # Parse modified Sigma rules for ATT&CK technique IDs
          # Run corresponding Atomic tests
          Invoke-AtomicTest $TECHNIQUE_ID -TimeoutSeconds 120

      - name: Validate detections fired
        run: |
          # Query test SIEM for alerts
          # Compare expected vs. actual
          # Generate pass/fail report

      - name: Report results
        run: |
          # Update DeTTECT scores
          # Post results to SOC channel
          # Create issues for failures
```

---

## Inputs

| Input | Source Module | Description |
|---|---|---|
| Detection Analytics (Sigma Rules) | [04 Detection Engineering](../04-Detection-Engineering/) | Deployed detection rules to validate via Atomic testing |
| DeTTECT Coverage Layers | [04 Detection Engineering](../04-Detection-Engineering/) | Current detection scores to validate and update |
| Prioritised Technique List | [03 Threat Intelligence](../03-Threat-Intelligence/) | ATT&CK techniques determining test scope and priority |
| ATT&CK Navigator Layers | [03 Threat Intelligence](../03-Threat-Intelligence/) | Threat profile overlays for gap visualisation |
| Countermeasure Deployments | [06 Countermeasures](../06-Countermeasures/) | D3FEND controls to validate via block-rate testing |
| Data Source Inventory | [02 Data Documentation](../02-Data-Documentation/) | Telemetry availability determining what is testable |

---

## Outputs

| Output | Consumers | Description |
|---|---|---|
| Test Results Reports | [04 Detection Eng](../04-Detection-Engineering/), [01 Governance](../01-Governance/) | Per-technique pass/fail with evidence |
| Updated DeTTECT Scores | [04 Detection Engineering](../04-Detection-Engineering/) | Validated detection quality scores |
| Coverage Dashboard | [01 Governance](../01-Governance/), CISO | Overall detection coverage metrics and KPIs |
| Gap Remediation Tickets | [04 Detection Eng](../04-Detection-Engineering/), [06 Countermeasures](../06-Countermeasures/) | Action items for failed detection or countermeasure tests |
| Purple Team Reports | All modules | Full exercise findings with ATT&CK mapping |
| Feedback to Threat Profile | [03 Threat Intelligence](../03-Threat-Intelligence/) | Validated detection scores informing threat profile re-prioritisation |

---

## Implementation Checklist

- [ ] Install Atomic Red Team (Invoke-AtomicRedTeam or atomic-operator)
- [ ] Identify safe testing environment (lab or approved production subset)
- [ ] Run initial Atomic tests for top 10 Critical-tier techniques
- [ ] Document results using the test result template
- [ ] Update DeTTECT scores in Module 04 based on actual test outcomes
- [ ] Deploy attack_range for scenario-based testing
- [ ] Conduct first purple team exercise using attack_range
- [ ] Establish KPI baselines (coverage ratio, MTTD, FP rate)
- [ ] Set up automated weekly smoke tests (CI/CD pipeline)
- [ ] Create coverage dashboard for Governance reporting (Module 01)
- [ ] Schedule monthly full coverage scans
- [ ] Schedule quarterly purple team exercises

---

## References

- [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
- [Invoke-AtomicRedTeam](https://github.com/redcanaryco/invoke-atomicredteam)
- [atomic-operator](https://github.com/swimlane/atomic-operator)
- [Splunk attack_range](https://github.com/splunk/attack_range)
- [attack_range Documentation](https://github.com/splunk/attack_range/wiki)
- [MITRE 11 Strategies — Strategy 10](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
