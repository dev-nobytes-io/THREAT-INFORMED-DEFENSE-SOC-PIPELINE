# Data Standardisation

> **Foundation:** [OSSEM Common Data Model (CDM)](https://github.com/OTRF/OSSEM-CDM)
> **Source Methodology:** [Threat Hunters Playbook — Pre-Hunt: Data Standardisation](https://threathunterplaybook.com/pre-hunt/data_standardization.html)

---

## Why Standardise?

Without standardisation, the same concept has different names across data sources:

| Concept | Sysmon | Windows Security | Zeek | CloudTrail |
|---|---|---|---|---|
| Process name | `Image` | `NewProcessName` | — | — |
| Source IP | `SourceIp` | `IpAddress` | `id.orig_h` | `sourceIPAddress` |
| User account | `User` | `SubjectUserName` | `uid` | `userIdentity.userName` |
| Destination port | `DestinationPort` | — | `id.resp_p` | — |

This means every analytic must account for every field variant — or it misses data. Standardisation solves this by normalising all sources to a single naming convention before analytics are written.

> *"A common schema helps hunters to correlate data from diverse data sources, and avoid writing long queries trying to hit every possible name assigned to a field that provides the same information across several data sources."*
> — Threat Hunters Playbook

---

## OSSEM Common Data Model (CDM) Architecture

The CDM provides the standardisation layer. It operates across three levels:

```
┌─────────────────────────────────────────────────────────────┐
│  Level 3: SCHEMA TABLES                                     │
│  Composite schemas grouping multiple entities                │
│  e.g., network_session, process_creation, authentication     │
│                                                              │
│  Tables aggregate entities into normalised event categories  │
├─────────────────────────────────────────────────────────────┤
│  Level 2: FIELD NAMING CONVENTION                            │
│  {prefix}_{attribute} pattern                                │
│  e.g., process_name, process_parent_id, src_ip_addr          │
│                                                              │
│  Deterministic: entity + prefix + attribute = field name     │
├─────────────────────────────────────────────────────────────┤
│  Level 1: SCHEMA ENTITIES                                    │
│  37 atomic entity definitions in YAML                        │
│  e.g., process, file, user, network, registry, dns           │
│                                                              │
│  Each entity defines attributes + valid prefixes             │
└─────────────────────────────────────────────────────────────┘
```

---

## Level 1 — Schema Entities

Entities are the atomic building blocks. Each is defined in YAML with attributes, prefixes, and extension relationships.

### The 37 CDM Entities

| Category | Entities |
|---|---|
| **Endpoint** | `process`, `file`, `registry`, `module`, `service`, `pipe`, `audit_policy` |
| **Network** | `network`, `dns`, `http`, `tls`, `url`, `port`, `ip`, `mac`, `user_agent` |
| **Identity** | `user`, `group`, `logon`, `kerberos` |
| **Directional** | `source`, `source_nat`, `destination`, `destination_nat`, `target` |
| **Metadata** | `event`, `etl`, `meta`, `device`, `cloud`, `geo`, `rule`, `alert` |
| **Security** | `hash`, `threat`, `x509_and_certificates` |
| **Generic** | `any` |

### Entity YAML Structure

Each entity is defined in a YAML file specifying its prefixes and attributes:

```yaml
name: process
prefix:
  - process
  - process_parent
id: C9573023-9A39-4C94-88BD-B911E3C800A6
description: >
  Event fields used to define metadata about processes in a system.
  Isolated memory address space that is used to run a program.
extends_entities:
  - source
  - target
attributes:
  - name: id
    type: integer
    description: Process ID used by the operating system to identify the process
    sample_value: 4756
  - name: guid
    type: string
    description: Process global unique identifier across operating systems
    sample_value: "{A98268C1-9C2E-5ACD-0000-0010396CAB00}"
  - name: name
    type: string
    description: Name of the process derived from the Image file or executable
    sample_value: conhost.exe
  - name: command_line
    type: string
    description: Command arguments that were passed to the process
    sample_value: "??\C:\WINDOWS\system32\conhost.exe 0xffffffff -ForceV1"
  - name: integrity_level
    type: string
    description: Integrity label assigned to a process
    sample_value: Medium
```

### Entity Extension

Entities can extend other entities to inherit their prefixes. This creates composite field names:

```
process entity (prefix: process, process_parent)
    extends → source entity (prefix: src)
    extends → target entity (prefix: target)

Result: the process attributes can be prefixed as:
  - process_name           (direct)
  - process_parent_name    (direct)
  - src_process_name       (via source extension)
  - target_process_name    (via target extension)
```

This enables directional analytics — distinguishing the source process from the target process in a single event without ambiguity.

---

## Level 2 — Field Naming Convention

The CDM enforces a deterministic naming pattern:

```
{prefix}_{attribute}
```

### Naming Rules

1. **All lowercase**, underscore-separated
2. **Prefix** comes from the entity definition (can be multi-level for extensions)
3. **Attribute** is the entity's field name
4. **No abbreviations** in attributes (except widely accepted: `ip`, `mac`, `dns`, `url`)
5. **Descriptions must be generic** — they must make sense across all valid prefixes

### Field Name Examples

| Entity | Prefix | Attribute | Normalised Field Name |
|---|---|---|---|
| process | `process` | `name` | `process_name` |
| process | `process_parent` | `name` | `process_parent_name` |
| process | `process_parent` | `command_line` | `process_parent_command_line` |
| ip | `src_ip` | `addr` | `src_ip_addr` |
| ip | `dst_ip` | `addr` | `dst_ip_addr` |
| port | `src` | `port` | `src_port` |
| port | `dst` | `port` | `dst_port` |
| user | `user` | `name` | `user_name` |
| file | `file` | `name` | `file_name` |
| file | `file` | `path` | `file_path` |
| hash | `hash` | `md5` | `hash_md5` |
| hash | `hash` | `sha256` | `hash_sha256` |
| registry | `registry_key` | `path` | `registry_key_path` |
| registry | `registry_key` | `value_name` | `registry_key_value_name` |

### Raw-to-CIM Mapping Template

Use this template to document how each raw data source maps to CDM field names:

```markdown
## Source: [Sysmon Event 1 — Process Creation]

| Raw Field Name | CDM Entity | CDM Field Name | Notes |
|---|---|---|---|
| Image | process | process_name | Full path; extract filename only for `process_name` |
| CommandLine | process | process_command_line | — |
| ProcessId | process | process_id | — |
| ProcessGuid | process | process_guid | — |
| ParentImage | process | process_parent_name | — |
| ParentProcessId | process | process_parent_id | — |
| ParentCommandLine | process | process_parent_command_line | — |
| User | user | user_name | Format: DOMAIN\username |
| IntegrityLevel | process | process_integrity_level | — |
| Hashes | hash | hash_md5, hash_sha256, hash_imphash | Split by algorithm |
| UtcTime | event | event_date_creation | Convert to ISO 8601 UTC |
```

---

## Level 3 — Schema Tables

Tables group multiple entities into composite normalised schemas for entire event categories. This enables cross-source correlation within a single table definition.

### Example: Process Creation Table

```yaml
name: process_creation
description: Events related to process creation across platforms
entities:
  - entity: event
    prefix: event
  - entity: process
    prefix: process
  - entity: process
    prefix: process_parent
  - entity: user
    prefix: user
  - entity: hash
    prefix: hash
  - entity: file
    prefix: file
```

This table produces a normalised schema that unifies:
- Sysmon Event 1
- Windows Security Event 4688
- Linux auditd `execve` syscall
- macOS Endpoint Security `es_event_exec_t`
- EDR process creation telemetry

All use the same field names (`process_name`, `process_parent_name`, `user_name`, etc.), enabling a single analytic to query across all sources.

### Example: Network Session Table

```yaml
name: network_session
description: Events related to network connections across platforms
entities:
  - entity: event
    prefix: event
  - entity: process
    prefix: process
  - entity: ip
    prefix: src_ip
  - entity: ip
    prefix: dst_ip
  - entity: port
    prefix: src
  - entity: port
    prefix: dst
  - entity: network
    prefix: network
  - entity: dns
    prefix: dns
  - entity: http
    prefix: http
  - entity: tls
    prefix: tls
  - entity: url
    prefix: url
  - entity: user_agent
    prefix: user_agent
```

This unifies:
- Sysmon Event 3 (Network Connection)
- Zeek `conn.log`, `dns.log`, `http.log`, `ssl.log`
- Windows Firewall logs
- AWS VPC Flow Logs
- Palo Alto / Fortinet firewall logs

---

## Implementation in This Pipeline

### Step 1 — Select Priority Data Sources

Use the data source inventory from the main module README. Start with the highest-value sources:

1. Process creation events (Sysmon 1 / Security 4688)
2. Network connections (Sysmon 3 / Zeek / Firewall)
3. Authentication events (Security 4624/4625)
4. DNS queries (Sysmon 22 / Zeek dns.log)
5. File creation events (Sysmon 11)

### Step 2 — Create Data Dictionaries (per-source)

Document every field for each event log using the OSSEM-DD YAML format. Reference the [OSSEM-DD repository](https://github.com/OTRF/OSSEM-DD) for existing dictionaries.

### Step 3 — Map to CDM Entities

For each data dictionary entry, map raw fields to CDM entity attributes using the `{prefix}_{attribute}` convention. Use the Raw-to-CIM Mapping Template above.

### Step 4 — Apply in SIEM Parsing

Configure your SIEM to normalise at ingest time or at search time:

| SIEM | Normalisation Method |
|---|---|
| **Splunk** | `props.conf` field aliases + CIM add-on |
| **Elastic/ELK** | Logstash filters or Elasticsearch ingest pipelines using ECS (aligned to CDM) |
| **Microsoft Sentinel** | ASIM (Advanced Security Information Model) parsers — native CDM alignment |
| **Chronicle (Google SecOps)** | UDM (Unified Data Model) field mappings |

### Step 5 — Validate Normalisation

For each mapped source, verify:

- [ ] All CDM fields are populated (no nulls where data exists)
- [ ] Field values match expected formats (IPs are valid, timestamps are UTC ISO 8601)
- [ ] Cross-source queries return consistent results (e.g., `process_name = "cmd.exe"` matches across Sysmon and Security logs)
- [ ] Entity prefixes correctly distinguish directionality (src vs dst, process vs process_parent)

---

## Incremental Adoption Strategy

The Threat Hunters Playbook advises: *"Depending on your priorities and the resources allocated to your team, you can either start your own CIM based on all the data sources available at once, or gradually create it from each data source used as you build analytics."*

Recommended approach for this pipeline:

```
Phase 1: Standardise process creation events (Sysmon 1 + Security 4688)
         → Enables 60%+ of endpoint detection analytics

Phase 2: Standardise network events (Sysmon 3, Zeek, Firewall)
         → Enables network-layer detection and correlation

Phase 3: Standardise authentication events (4624/4625, Azure AD, CloudTrail)
         → Enables identity-based analytics and lateral movement detection

Phase 4: Standardise remaining event categories
         → Registry, file, DNS, cloud API, email

Each phase: Document DD → Map to CDM → Configure SIEM parsing → Validate
```

---

## Connection to Other Pipeline Modules

| Module | How Standardisation Feeds It |
|---|---|
| **03 — Threat Intelligence** | Standardised fields enable ATT&CK data source mapping |
| **04 — Detection Engineering** | Analytics written against CDM fields work across all normalised sources |
| **05 — Incident Response** | Investigators query using consistent field names regardless of source |
| **07 — Detection Testing** | Security Datasets (Mordor) use CDM-aligned field names for validation |

---

## References

- [OSSEM Common Data Model (CDM)](https://github.com/OTRF/OSSEM-CDM)
- [OSSEM CDM Entity Structure Guidelines](https://ossemproject.com/cdm/guidelines/entity_structure.html)
- [OSSEM CDM Table Structure Guidelines](https://ossemproject.com/cdm/guidelines/table_structure.html)
- [OSSEM Data Dictionaries (DD)](https://github.com/OTRF/OSSEM-DD)
- [Threat Hunters Playbook — Data Standardisation](https://threathunterplaybook.com/pre-hunt/data_standardization.html)
- [Microsoft Sentinel ASIM (CDM-aligned)](https://learn.microsoft.com/en-us/azure/sentinel/normalization)
- [Elastic Common Schema (ECS)](https://www.elastic.co/guide/en/ecs/current/index.html)
