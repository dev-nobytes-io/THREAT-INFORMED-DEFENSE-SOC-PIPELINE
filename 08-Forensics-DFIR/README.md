# 08 — Forensics & Digital Forensics Incident Response (DFIR)

> **MITRE SOC Strategy Addressed:**
> - Strategy 5: Prioritize Incident Response (forensic evidence underpins every investigation)
> - Strategy 7: Select and Collect the Right Data (forensic acquisition is the ultimate data collection)
> - Strategy 10: Measure Performance to Improve Performance (forensic findings validate or invalidate detection hypotheses)

> **Continuous Hunt Role:** **Evidence** — provides ground-truth artefacts that confirm adversary presence, validate detection hypotheses, and generate new hunt leads. Forensic findings feed directly back into the M3TID cycle as high-confidence intelligence.

---

## Purpose

Forensics transforms suspected incidents into confirmed, evidence-backed findings. While Module 05 (Incident Response) defines *what to do* when an alert fires, this module defines *how to collect, preserve, and analyse digital evidence* so that findings are defensible, reproducible, and actionable. Forensic capability is what separates a SOC that triages alerts from a SOC that truly understands adversary operations.

---

## SOC-CMM Alignment

| SOC-CMM Domain | Aspect | How This Module Contributes |
|---|---|---|
| **Services** | Forensics & Investigation | Primary mapping — defines the forensic service catalogue |
| **Services** | Incident Response | Forensics provides the evidence that drives IR decisions |
| **Process** | Procedures & Documentation | Chain of custody, acquisition SOPs, analysis playbooks |
| **Process** | Continuous Improvement | Forensic findings update detections (Module 04) and threat profiles (Module 03) |
| **Technology** | Testing Infrastructure | Forensic lab, analysis workstations, tool validation |
| **People** | Training & Education | DFIR specialist skills (CIISec C1, DoDCWF IN-FOR-002) |

---

## Capability Areas

```
FORENSIC CAPABILITY STACK

┌─────────────────────────────────────────────────────────────────────┐
│                        CHAIN OF CUSTODY                              │
│   Evidence integrity underpins everything — break it, lose it all    │
├────────────┬────────────┬────────────┬────────────┬─────────────────┤
│ ACQUISITION│  MEMORY    │   DISK     │  NETWORK   │   TIMELINE      │
│            │  FORENSICS │  FORENSICS │  FORENSICS │   ANALYSIS      │
│ Live +     │ Process    │ File sys   │ PCAP       │ Super-timeline  │
│ dead       │ analysis   │ analysis   │ analysis   │ generation      │
│ imaging    │ Injected   │ Artefact   │ Flow       │ Artefact        │
│ Triage     │ code       │ carving    │ analysis   │ correlation     │
│ collection │ Malware    │ Deleted    │ DNS        │ Temporal        │
│            │ in memory  │ file       │ forensics  │ pattern         │
│            │            │ recovery   │ TLS/SSL    │ identification  │
│            │            │            │ inspection │                 │
├────────────┴────────────┴────────────┴────────────┴─────────────────┤
│                       ARTEFACT PARSING                               │
│   Windows artefacts (Registry, Event Logs, Prefetch, SRUM, Amcache, │
│   ShimCache, MFT, USN Journal, Shellbags, LNK, JumpLists)          │
│   Linux artefacts (auth.log, wtmp/btmp, .bash_history, cron,        │
│   systemd journals, /proc, auditd)                                  │
│   macOS artefacts (FSEvents, Spotlight, KnowledgeC, Unified Log)    │
├─────────────────────────────────────────────────────────────────────┤
│                       REPORTING & HANDOFF                            │
│   Findings → Module 03 (intel), Module 04 (detections), legal       │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Tool Options

This module is **tool-agnostic** — the pipeline specifies capabilities, not products. The tables below present curated open-source options for each capability area. See [references/tool-integration-matrix.md](../references/tool-integration-matrix.md) for the full cross-module tool options matrix.

### Forensic Acquisition

| Tool | Platform | Capability | Best For |
|---|---|---|---|
| **dc3dd** | Linux | Bit-for-bit disk imaging with on-the-fly hashing | Disk acquisition with integrity verification |
| **FTK Imager (free)** | Windows | Disk/memory imaging, logical/physical acquisition | Windows-native acquisition in the field |
| **AVML** (Azure Virtual Machine Live) | Linux | Memory acquisition for Linux hosts and cloud VMs | Cloud and Linux memory collection |
| **LiME** (Linux Memory Extractor) | Linux | Loadable kernel module for memory acquisition | Linux memory forensics, IR triage |
| **Velociraptor** | Cross-platform | Agent-based live acquisition, triage collection, hunt | Enterprise-scale remote triage and acquisition |
| **GRR Rapid Response** | Cross-platform | Agent-based remote live forensics at scale | Large fleet triage and targeted collection |
| **CyLR** | Windows/Linux/macOS | Lightweight triage collection of key forensic artefacts | Rapid triage when full imaging is impractical |

**Choose based on:** Scale (single host vs fleet), environment (on-prem vs cloud), urgency (full image vs triage), legal requirements (bit-for-bit vs logical).

### Memory Forensics

| Tool | Platform | Capability | Best For |
|---|---|---|---|
| **Volatility 3** | Cross-platform | Open-source memory analysis framework; plugin architecture | Deep memory analysis — process trees, injected code, rootkits, network connections |
| **MemProcFS** | Cross-platform | Memory as a virtual file system; rapid triage | Fast triage — browse memory as a mounted file system |
| **Rekall** | Cross-platform | Memory analysis framework (Google) | Alternative to Volatility; good for automation |

**Choose based on:** Depth of analysis needed (Volatility 3 for deep, MemProcFS for rapid triage), automation requirements (Rekall for scripted pipelines).

### Disk Forensics

| Tool | Platform | Capability | Best For |
|---|---|---|---|
| **Autopsy / The Sleuth Kit** | Cross-platform | Full disk forensic analysis suite; file system analysis, keyword search, timeline | Primary disk forensic analysis workbench |
| **X-Ways Forensics** | Windows | Commercial-grade disk forensics (license required) | High-performance disk analysis for large cases |
| **DFIR-IRIS** | Web-based | Collaborative forensic case management and analysis | Team-based forensic investigation management |

### Network Forensics

| Tool | Platform | Capability | Best For |
|---|---|---|---|
| **Zeek** (formerly Bro) | Linux | Network traffic analysis, protocol logging, connection metadata | Continuous network monitoring and forensic log generation |
| **Wireshark / tshark** | Cross-platform | Deep packet inspection, protocol analysis, PCAP examination | Interactive packet-level forensic analysis |
| **NetworkMiner** | Cross-platform | Network forensic analysis, host identification, file extraction | PCAP analysis with automatic file and image extraction |
| **Arkime** (formerly Moloch) | Linux | Large-scale indexed PCAP capture and search | Enterprise PCAP retention and search at scale |

### Timeline Analysis

| Tool | Platform | Capability | Best For |
|---|---|---|---|
| **Plaso / log2timeline** | Cross-platform | Super-timeline generation from multiple artefact sources | Generating unified timelines from disk images, logs, artefacts |
| **Timesketch** | Web-based | Collaborative timeline analysis and annotation | Team-based timeline investigation and sharing |
| **DFIR-IRIS** | Web-based | Timeline view integrated into case management | Combining timeline analysis with case tracking |

### Artefact Parsing

| Tool | Platform | Artefacts Covered | Best For |
|---|---|---|---|
| **Eric Zimmerman Tools** | Windows | Registry, Prefetch, ShimCache, Amcache, LNK, JumpLists, MFT, SRUM, Shellbags | Windows artefact parsing — the standard toolkit |
| **Hayabusa** | Cross-platform | Windows Event Logs (EVTX), Sigma rule-based analysis | Rapid triage of Windows Event Logs using Sigma rules |
| **Chainsaw** | Cross-platform | Windows Event Logs, Sigma/custom rules, shimcache, prefetch | Fast log triage with built-in detection rules |
| **Velociraptor** | Cross-platform | Registry, Event Logs, MFT, Prefetch, SRUM, browser history + custom | Remote artefact collection and parsing at scale |
| **KAPE** (Kroll Artifact Parser) | Windows | Comprehensive Windows artefact collection and parsing | Targeted artefact collection with modular targets/modules |

### Malware Analysis (Triage-Level)

| Tool | Platform | Capability | Best For |
|---|---|---|---|
| **YARA** | Cross-platform | Pattern-matching rule engine for malware classification | Malware identification and classification |
| **Capa** | Cross-platform | Identifies capabilities in PE/ELF executables | Rapid capability assessment of suspicious binaries |
| **pe-sieve / HollowsHunter** | Windows | Detects hollowed/injected processes in live memory | Runtime malware triage on live systems |
| **FLOSS** | Cross-platform | Automatic string deobfuscation for malware analysis | Extracting obfuscated strings from malware samples |

---

## Forensic Investigation Workflow

### Standard Investigation Process

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ 1. IDENTIFY  │───▶│ 2. PRESERVE  │───▶│ 3. COLLECT   │───▶│ 4. ANALYSE   │
│              │    │              │    │              │    │              │
│ Scope the    │    │ Isolate      │    │ Acquire      │    │ Memory       │
│ incident     │    │ evidence     │    │ evidence     │    │ Disk         │
│ from IR      │    │ Chain of     │    │ Memory first │    │ Network      │
│ (Module 05)  │    │ custody      │    │ then disk    │    │ Artefacts    │
│              │    │ initiated    │    │ Log triage   │    │ Timeline     │
└──────────────┘    └──────────────┘    └──────────────┘    └──────┬───────┘
                                                                    │
       ┌────────────────────────────────────────────────────────────┘
       ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│ 5. CORRELATE │───▶│ 6. REPORT    │───▶│ 7. FEEDBACK  │
│              │    │              │    │              │
│ Build        │    │ Document     │    │ Update       │
│ timeline     │    │ findings     │    │ detections   │
│ Map to       │    │ ATT&CK map  │    │ (Module 04)  │
│ ATT&CK       │    │ Preserve     │    │ Update       │
│ techniques   │    │ for legal    │    │ threat       │
│              │    │              │    │ profile      │
│              │    │              │    │ (Module 03)  │
└──────────────┘    └──────────────┘    └──────────────┘
```

### Order of Volatility

Always collect evidence in order of volatility — most volatile first:

| Priority | Source | Volatility | Tool Examples |
|---|---|---|---|
| 1 | **CPU registers, cache** | Seconds | Hardware debugger (rarely practical in SOC) |
| 2 | **Memory (RAM)** | Minutes | AVML, LiME, FTK Imager, Velociraptor |
| 3 | **Network connections** | Minutes | netstat, Velociraptor, Zeek (if running) |
| 4 | **Running processes** | Minutes | Velociraptor, GRR, Volatility (from memory dump) |
| 5 | **Disk (file system)** | Hours-Days | dc3dd, FTK Imager, KAPE (triage) |
| 6 | **Log files** | Days-Weeks | SIEM (already collected), Velociraptor, CyLR |
| 7 | **Archived/backup data** | Months-Years | Backup infrastructure |

---

## Chain of Custody

### Why It Matters

Chain of custody ensures evidence integrity from acquisition through analysis to potential legal proceedings. Without it, forensic findings may be challenged or inadmissible.

### Chain of Custody Record Template

```markdown
## Evidence Chain of Custody — Case [Case-ID]

### Evidence Item

| Field | Value |
|---|---|
| Evidence ID | [unique identifier] |
| Description | [what it is — e.g., "Memory dump from HOST-WS042"] |
| Source Host | [hostname / IP / asset ID] |
| Acquisition Method | [tool used — e.g., "AVML v0.14.0"] |
| Acquisition Date/Time | [UTC timestamp] |
| Acquired By | [name, role] |
| Hash (SHA-256) | [hash value] |
| Hash Verified | [YES/NO, by whom, when] |
| Storage Location | [path / evidence locker / secure share] |
| Original Size | [bytes] |

### Custody Log

| Date/Time (UTC) | Action | From | To | Purpose | Signature |
|---|---|---|---|---|---|
| [timestamp] | Acquired | [source host] | [analyst name] | Initial collection | [initials] |
| [timestamp] | Transferred | [analyst 1] | [analyst 2] | Memory analysis | [initials] |
| [timestamp] | Copied | [analyst 2] | [secure archive] | Long-term storage | [initials] |

### Integrity Verification Log

| Date/Time (UTC) | Verified By | Hash Match | Notes |
|---|---|---|---|
| [timestamp] | [name] | YES / NO | [any discrepancies] |
```

---

## Integration with Pipeline Modules

### Module 05 (Incident Response) → Module 08 (Forensics)

```
RE&CT Stage                    Forensic Activity
─────────────────              ──────────────────
RA.IDENT (Identification)  →   Triage collection (Velociraptor/CyLR)
                                Memory acquisition (if warranted)
                                Initial artefact review

RA.CONT (Containment)     →   Preserve evidence BEFORE containment actions
                                Image before wipe, dump before isolate

RA.ERAD (Eradication)     →   Full disk imaging if persistence suspected
                                Verify eradication completeness via forensics

RA.LL (Lessons Learned)    →   Forensic report drives detection updates
                                ATT&CK technique map from forensic findings
                                New IoCs extracted → Module 03
```

### Module 04 (Detection Engineering) ← Module 08 (Forensics)

Forensic findings generate new detection opportunities:

| Forensic Finding | Detection Action |
|---|---|
| New persistence mechanism discovered | Write Sigma rule → Module 04 backlog |
| Undocumented lateral movement path | Map to ATT&CK, build detection, test (Module 07) |
| Novel data exfiltration channel | Create SIEM correlation rule |
| Adversary tool identified (YARA/Capa) | Deploy YARA rule to EDR; create file-based detection |
| Timeline gap (data source missing) | Update data source inventory (Module 02) |

### Module 03 (Threat Intelligence) ← Module 08 (Forensics)

| Forensic Output | Intelligence Action |
|---|---|
| ATT&CK technique map of incident | Update composite threat profile |
| Indicators of compromise (IoCs) | Ingest into TIP; share via STIX/TAXII |
| Adversary infrastructure details | Track in threat group profile |
| TTP patterns (behavioural) | Generate new hunt hypotheses (M3TID) |

### Module 07 (Detection Testing) ← Module 08 (Forensics)

| Forensic Output | Testing Action |
|---|---|
| Confirmed technique that detection missed | Atomic Red Team test → validate new detection |
| Confirmed technique that detection caught | Update DeTTECT score (validated) |
| Technique not in Atomic library | Write custom test procedure |

---

## Pre-Collection Readiness Checklist

Forensic readiness ensures the SOC can collect evidence *before* an incident demands it:

- [ ] **Acquisition toolkits staged** — USB/network share with imaging tools, pre-validated hashes
- [ ] **Memory acquisition tools deployed** — AVML/LiME packages available for all OS types
- [ ] **Triage agents deployed** — Velociraptor/GRR agents on critical assets (or deployment plan)
- [ ] **Secure evidence storage** — Write-once or access-controlled storage for evidence
- [ ] **Chain of custody templates** — Pre-printed or digital templates ready for use
- [ ] **Forensic workstations** — Dedicated analysis stations with tools installed and validated
- [ ] **Legal authority documented** — Authority to collect evidence defined in SOC charter (Module 01)
- [ ] **Evidence retention policy** — Defined retention periods aligned to legal/regulatory requirements
- [ ] **Acquisition SOPs documented** — Step-by-step procedures for each acquisition type
- [ ] **Tool validation records** — Each forensic tool validated against known-good data sets
- [ ] **Network forensic capture** — Zeek/Arkime running on critical network segments
- [ ] **Log retention verified** — SIEM retention covers expected investigation timelines (Module 02)

---

## Forensic Artefact Quick Reference

### Windows Critical Artefacts

| Artefact | Location | What It Reveals | Tool |
|---|---|---|---|
| **MFT** | `$MFT` (NTFS root) | All file metadata, timestamps, resident data | MFTECmd (EZ Tools) |
| **USN Journal** | `$UsnJrnl:$J` | File system change journal — create/modify/delete/rename | MFTECmd |
| **Prefetch** | `C:\Windows\Prefetch\` | Program execution history (last 8 run times) | PECmd (EZ Tools) |
| **Amcache** | `C:\Windows\appcompat\Programs\Amcache.hve` | Program execution with SHA-1 hashes | AmcacheParser (EZ Tools) |
| **ShimCache** | `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache` | Program execution evidence (existence, not necessarily execution) | AppCompatCacheParser |
| **SRUM** | `C:\Windows\System32\sru\SRUDB.dat` | Application resource usage, network usage per-app | SrumECmd (EZ Tools) |
| **Event Logs** | `C:\Windows\System32\winevt\Logs\` | Security, System, Sysmon, PowerShell, TaskScheduler | Hayabusa, Chainsaw, EvtxECmd |
| **Registry Hives** | `C:\Windows\System32\config\` | System config, user activity, persistence mechanisms | RECmd, Registry Explorer (EZ Tools) |
| **Shellbags** | `NTUSER.DAT`, `UsrClass.dat` | Folder access history (even deleted folders) | ShellBagsExplorer (EZ Tools) |
| **LNK Files** | `C:\Users\*\AppData\Roaming\Microsoft\Windows\Recent\` | File/folder access with timestamps and paths | LECmd (EZ Tools) |
| **JumpLists** | `C:\Users\*\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\` | Application-specific recent file access | JLECmd (EZ Tools) |
| **Browser History** | Per-browser profile paths | URL access, downloads, search terms | Hindsight (Chrome), KAPE modules |

### Linux Critical Artefacts

| Artefact | Location | What It Reveals | Tool |
|---|---|---|---|
| **auth.log / secure** | `/var/log/auth.log` or `/var/log/secure` | Authentication events, sudo usage, SSH logins | Plaso, manual review |
| **wtmp / btmp** | `/var/log/wtmp`, `/var/log/btmp` | Login history (successful / failed) | `last`, `lastb`, Plaso |
| **bash_history** | `~/.bash_history` | Command history per user | Manual review, Velociraptor |
| **crontab** | `/etc/crontab`, `/var/spool/cron/` | Scheduled tasks (persistence mechanism) | Manual review |
| **systemd journals** | `/var/log/journal/` | Service logs, boot events | `journalctl`, Plaso |
| **/proc** | `/proc/` | Running processes, network connections, loaded modules | Volatility 3 (from memory), live commands |
| **auditd logs** | `/var/log/audit/audit.log` | Detailed system call audit trail | `ausearch`, `aureport`, Plaso |
| **Filesystem timestamps** | `stat` / Plaso | File creation, modification, access, change (MACB) | Plaso, Autopsy |

---

## Maturity Progression

| Level | Forensic Capability |
|---|---|
| **0** | No forensic capability; evidence destroyed during incident response |
| **1** | Ad-hoc forensics; tools used inconsistently; no chain of custody; no dedicated analyst |
| **2** | Basic acquisition capability (disk imaging); some artefact parsing; incident-driven only; one trained analyst |
| **3** | Full acquisition capability (memory + disk + triage); chain of custody enforced; artefact parsing with standard tools; forensic findings feed back to Modules 03, 04, 07; pre-collection readiness achieved; forensic SOPs documented |
| **4** | Enterprise-scale triage (Velociraptor/GRR fleet-wide); automated artefact parsing and timeline generation; forensic case management (DFIR-IRIS); all findings systematically ATT&CK-mapped; tool validation program; legal coordination procedures tested |
| **5** | Predictive forensic readiness based on threat landscape; automated evidence collection triggered by high-confidence detections; forensic-as-code (automated analysis pipelines); community sharing of forensic intelligence; continuous tool validation |

---

## Workforce Roles

| Role | DoDCWF ID | ASD Stream | CIISec Specialism | Pipeline Focus |
|---|---|---|---|---|
| **Forensic Analyst** | IN-FOR-002 | Incident Response Level 4 | C1 — Threat Detection & Digital Forensics | Acquisition, analysis, reporting |
| **Incident Responder** (with DFIR skills) | PR-CIR-001 | Incident Response Level 3-4 | A4 — Incident Management | Triage collection, initial analysis |
| **Malware Analyst** | AN-EXP-001 | Cyber Threat Intelligence Level 4 | C2 — Cyber Security Research | Sample analysis, YARA rules, reverse engineering |
| **Threat Hunter** (forensic-enabled) | AN-TWA-001 | Cyber Threat Intelligence Level 4 | C1 — Threat Detection & Digital Forensics | Using forensic artefacts in proactive hunts |

---

## Outputs

| Output | Consumers | Description |
|---|---|---|
| Forensic Investigation Reports | Module 05 (IR), Module 01 (Governance), Legal | Evidence-backed incident findings with ATT&CK mapping |
| ATT&CK Technique Maps | Module 03 (Threat Intel) | Confirmed techniques from real incidents |
| New Indicators of Compromise | Module 03 (Threat Intel), Module 04 (Detection) | IoCs extracted from forensic analysis |
| Detection Gap Findings | Module 04 (Detection Engineering) | Techniques observed forensically that lacked detections |
| Data Source Gap Findings | Module 02 (Data Documentation) | Missing log sources discovered during investigation |
| Updated DeTTECT Scores | Module 04 (Detection Engineering) | Validated or corrected detection scores based on real incidents |
| Forensic Readiness Reports | Module 01 (Governance) | Pre-collection readiness status and improvement needs |

---

## Implementation Checklist

- [ ] Assess current forensic capability against maturity criteria above
- [ ] Select and deploy acquisition tools for all OS types in environment
- [ ] Deploy triage agent (Velociraptor or GRR) to critical asset subset
- [ ] Build forensic workstation with analysis toolkit (Autopsy, Volatility 3, EZ Tools, Plaso)
- [ ] Document chain of custody procedures and train all IR staff
- [ ] Create acquisition SOPs for memory, disk, and triage collection scenarios
- [ ] Validate all forensic tools against known-good data sets
- [ ] Establish secure evidence storage with access controls and audit logging
- [ ] Integrate forensic findings feedback loop into Modules 03, 04, and 07
- [ ] Conduct first forensic exercise using attack_range evidence (Module 07)
- [ ] Set up Timesketch or DFIR-IRIS for collaborative analysis
- [ ] Document legal authority for evidence collection in SOC charter (Module 01)
- [ ] Train at least one analyst to CIISec C1 specialism competency
- [ ] Establish forensic artefact parsing playbooks for Windows and Linux

---

## References

- [Volatility 3](https://github.com/volatilityfoundation/volatility3)
- [Autopsy / The Sleuth Kit](https://www.autopsy.com/)
- [Velociraptor](https://docs.velociraptor.app/)
- [GRR Rapid Response](https://github.com/google/grr)
- [Plaso / log2timeline](https://github.com/log2timeline/plaso)
- [Timesketch](https://github.com/google/timesketch)
- [DFIR-IRIS](https://github.com/dfir-iris/iris-web)
- [Eric Zimmerman Tools](https://ericzimmerman.github.io/)
- [Hayabusa](https://github.com/Yamato-Security/hayabusa)
- [Chainsaw](https://github.com/WithSecureLabs/chainsaw)
- [KAPE](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape)
- [Arkime](https://arkime.com/)
- [AVML](https://github.com/microsoft/avml)
- [LiME](https://github.com/504ensicsLabs/LiME)
- [YARA](https://github.com/VirusTotal/yara)
- [Capa](https://github.com/mandiant/capa)
- [MITRE 11 Strategies](https://www.mitre.org/sites/default/files/2022-04/11-strategies-of-a-world-class-cybersecurity-operations-center.pdf)
