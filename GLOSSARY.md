# OpenSAF & SAF AIS Documentation Glossary

This document provides a centralized glossary of all abbreviations, acronyms, and terminology used across the **OpenSAF Programmer's Reference** documentation and the **Service Availability Forum (SAF) Application Interface Specification (AIS)** standards.

It also defines the standardized typographic conventions and documentation feedback channels.

---

## 📖 Abbreviations and Acronyms

| Abbreviation | Expansion / Term | OpenSAF Subsystem / Category | Description |
| :--- | :--- | :--- | :--- |
| **AIS** | Application Interface Specification | SAF Standards | Standardized API specifications published by the Service Availability Forum. |
| **AMF** | Availability Management Framework | AMF Service | Workload and lifecycle manager coordinating redundant resources to eliminate single points of failure. |
| **AMFA** | AMF Agent | AMF Service | Client library linked into managed applications to interface with AMF. |
| **AMFD** | AMF Director | AMF Service | Central cluster-wide director process implementing AMF coordination and decision logic. |
| **AMFND** | AMF Node Director | AMF Service | Node-local daemon responsible for managing components and service units on a specific node. |
| **AMF-OI** | AMF Object Implementer | AMF Service | IMM Object Implementer that manages AMF configuration and runtime objects in IMM. |
| **AMFWD** | AMF Watchdog | AMF Service | Watchdog process monitoring AMF node daemon responsiveness. |
| **API** | Application Programming Interface | General | Standard software interface definitions. |
| **AvA** | Availability Agent | AMF Service | Alternate designation for the AMF client agent library. |
| **CCB** | Configuration Change Bundle | IMM Service | Atomic transaction bundle for modifying configuration objects within IMM. |
| **CKPT** | Checkpoint Service | CPSv Service | Standard SAF abbreviation for the Checkpoint Service (AIS SAI-AIS-CKPT). |
| **CLI** | Command Line Interface | General | Terminal-based administrative utilities (e.g. `immadm`, `amf-adm`). |
| **CLM** | Cluster Membership Service | CLMSv Service | SAF service tracking cluster membership, nodes, and cluster-wide state changes. |
| **CLMA** | Cluster Membership Agent | CLMSv Service | Client library for applications to interact with CLMSv and receive membership callbacks. |
| **CLMD** | CLM Director | CLMSv Service | Central cluster director tracking active and configured nodes in the cluster. |
| **CLMNA** | CLM Node Agent | CLMSv Service | Node-level CLM agent process reporting local node status. |
| **CLMND** | CLM Node Director | CLMSv Service | Node-local daemon managing node membership state on individual cluster nodes. |
| **CLMS** | Cluster Membership Server | CLMSv Service | Server component implementing cluster membership database and notifications. |
| **CLMSv** | Cluster Membership Service | CLMSv Service | Full service name for OpenSAF Cluster Membership Service. |
| **CPA** | Checkpoint Agent | CPSv Service | Client library linked to applications for opening, writing, and reading checkpoints. |
| **CPD** | Checkpoint Director | CPSv Service | Central coordinator managing checkpoint replicas, metadata, and retention timers. |
| **CPND** | Checkpoint Node Director | CPSv Service | Local node daemon managing checkpoint data replicas stored in node memory. |
| **CPSv** | Checkpoint Service | CPSv Service | Facility for replicating runtime state across nodes to protect against failovers. |
| **DN** | Distinguished Name | IMM Service | Unique hierarchical naming string identifying an object in the IMM database. |
| **DTSv** | Distributed Tracing Service | Core Services | OpenSAF distributed logging and software tracing infrastructure. |
| **EDA** | Event Distribution Agent | EDSv Service | Client library for publishing and subscribing to event channels. |
| **EDS** | Event Distribution Server | EDSv Service | Central event distribution server routing events between publishers and subscribers. |
| **EDSv** | Event Distribution Service | EDSv Service | Publish-subscribe event mechanism based on SAI-AIS-EVT specification. |
| **EE** | Execution Environment | PLMSv Service | Logical environment (OS, hypervisor, or container) where software runs. |
| **EVT** | Event Service | EDSv Service | Standard SAF AIS designation for the Event Distribution Service. |
| **FRU** | Field Replaceable Unit | PLMSv Service | Physical hardware module that can be replaced in the field without taking down the chassis. |
| **GLA** | Global Locking Agent | GLSv Service | Client library providing distributed mutexes and locks to applications. |
| **GLD** | Global Locking Director | GLSv Service | Central coordinator for cluster-wide lock resource allocation. |
| **GLND** | Global Locking Node Director | GLSv Service | Node-local manager for lock tracking and client interaction. |
| **GLSv** | Global Lock Service | GLSv Service | Distributed mutual exclusion and lock management service (SAI-AIS-LCK). |
| **HE** | Hardware Element | PLMSv Service | Physical hardware component mapped into the PLM information model. |
| **HPI** | Hardware Platform Interface | PLMSv Service | Open specification (SA Forum) for monitoring hardware health, sensors, and inventory. |
| **HRB** | HPI Request Broker | PLMSv Service | Broker routing hardware queries between PLM and underlying HPI implementations. |
| **HSM** | HPI Session Manager | PLMSv Service | Session tracking layer interfacing with hardware platform daemons. |
| **IM** | Information Model | IMM Service | Hierarchical schema and object definition representing cluster configuration and state. |
| **IMCN** | Information Model Change Notifier | Extensions | OpenSAF extension generating NTF notifications upon IMM object lifecycle events. |
| **IMM** | Information Model Management | IMM Service | Distributed in-memory object database for cluster configuration and runtime attributes. |
| **IMMA** | IMM Agent | IMM Service | Client library providing IMM OM (Object Management) and OI (Object Implementer) APIs. |
| **IMMCLI** | IMM Command Line Interface | IMM Service | Suite of CLI commands (`immcfg`, `immadm`, `immlist`, `immfind`, `immxml-dump`). |
| **IMMD** | IMM Director | IMM Service | Central coordinator for IMM database synchronization and coordinator election. |
| **IMMND** | IMM Node Director | IMM Service | Local daemon maintaining a shared-memory replica of the IMM database on each node. |
| **IMMSv** | Information Model Management Service | IMM Service | Full service designation for OpenSAF IMM. |
| **LCK** | Locking Service | GLSv Service | SAF AIS standard acronym for the Global Lock Service (SAI-AIS-LCK). |
| **LEAP** | Layered Environment for Accelerated Portability | Core Services | Portability abstraction layer insulating OpenSAF from underlying operating systems. |
| **LGA** | Log Agent | LOGSv Service | Client library providing log stream operations (`saLogWriteLog`, etc.). |
| **LGS** | Log Server | LOGSv Service | High-throughput logging server writing log records and handling log file rotation. |
| **LOG-OI** | Log Object Implementer | LOGSv Service | IMM Object Implementer managing configuration of log streams and log servers. |
| **LOGSv** | Log Service | LOGSv Service | Cluster-wide logging service conforming to SAI-AIS-LOG specification. |
| **MBCA** | Message Based Checkpoint Agent | CPSv / Core | Agent managing message-based runtime checkpoint replication. |
| **MBCSv** | Message Based Checkpoint Service | CPSv / Core | Optimized internal protocol using message passing for 2N server state synchronization. |
| **MDS** / **MDSv** | Message Distribution Service | Core Services | Cluster-wide IPC and message routing substrate connecting all OpenSAF agents and directors. |
| **MQA** | Message Queue Agent | MQSv Service | Client library providing priority message queue operations to applications. |
| **MQD** | Message Queue Director | MQSv Service | Central coordinator managing cluster message queue groups and policies. |
| **MQND** | Message Queue Node Director | MQSv Service | Node-local daemon managing local message queues and buffers. |
| **MQSv** | Message Queue Service | MQSv Service | Priority-based, persistent and non-persistent queuing service (SAI-AIS-MSG). |
| **MSG** | Messaging Service | MQSv Service | Standard SAF AIS abbreviation for the Message Queue Service. |
| **NTF** | Notification Service | NTFSv Service | Standard SAF AIS abbreviation for the Notification Service (SAI-AIS-NTF). |
| **NTFA** | Notification Agent | NTFSv Service | Client library for sending alarms and subscribing to notification streams. |
| **NTFD** / **NTFS** | Notification Server | NTFSv Service | Central server managing notification subscriber lists and alarm delivery. |
| **NTF-OI** | Notification Object Implementer | NTFSv Service | IMM Object Implementer managing notification configuration objects. |
| **NTFSv** | Notification Service | NTFSv Service | Standardized ITU-T X.733 compliant alarm and notification service. |
| **OI** | Object Implementer | IMM Service | Application or service process authorized to create, validate, and update IMM runtime objects. |
| **OM** | Object Manager | IMM Service | Client reading or administrating IMM objects via the standard IMM OM API. |
| **PBE** | Persistent Back End | IMM Service | On-disk database engine (SQLite) persisting IMM configuration objects across cluster reboots. |
| **PLM** | Platform Management Service | PLMSv Service | Service managing hardware FRUs, execution environments, and HPI mapping. |
| **PLMA** | Platform Management Agent | PLMSv Service | Client library for interacting with hardware platform management. |
| **PLMC** | Platform Management Coordinator | PLMSv Service | Central coordinator evaluating hardware inventory and matching rules. |
| **PLMS** | Platform Management Server | PLMSv Service | Server managing hardware states and mapping to the SA Forum Information Model. |
| **PLMSv** | Platform Management Service | PLMSv Service | Full service name for OpenSAF Platform Management Service (SAI-AIS-PLM). |
| **RDN** | Relative Distinguished Name | IMM Service | Single attribute-value pair forming one component of an object Distinguished Name. |
| **SAF** | Service Availability Forum | SAF Standards | Industry consortium defining open HA middleware and platform interface standards. |
| **SG** | Service Group | AMF Service | Set of Service Units (SUs) grouped under an AMF redundancy model (2N, N+M, N-Way, etc.). |
| **SI** | Service Instance | AMF Service | Logical workload aggregation allocated to Active or Standby assignments by AMF. |
| **SMF** | Software Management Framework | SMFSv Service | Service orchestrating non-disruptive rolling upgrades across cluster nodes. |
| **SMFA** | SMF Agent | SMFSv Service | Client agent executing node-local upgrade procedures and command callbacks. |
| **SMFD** | SMF Director | SMFSv Service | Central director parsing upgrade campaigns and controlling cluster upgrade workflows. |
| **SMFND** | SMF Node Director | SMFSv Service | Node-local manager executing campaign steps on specific nodes. |
| **SMH** | System Manager Host | SMFSv Service | Node or external host initiating software upgrade campaigns. |
| **SNMP** | Simple Network Management Protocol | Management | Standard network management protocol used for external monitoring and alarms. |
| **SU** | Service Unit | AMF Service | Collection of software components running on a node and treated as a single unit of failover. |
| **XML** | Extensible Markup Language | General | Schema format used for IMM data dumps (`imm.xml`) and SMF upgrade campaigns. |

---

## 📐 Typographic & Documentation Conventions

The OpenSAF documentation set uses the following standardized conventions:

| Convention | Meaning / Usage Context | Example |
| :--- | :--- | :--- |
| `0x00000000` | Hexadecimal numbers (digits 0–9, A–F) used for memory addresses, handles, and offsets. | `0x00000401` |
| `0b0000` | Binary numbers (digits 0 and 1). | `0b0010` |
| **Bold** | Emphasized technical terms, key parameters, or object types. | **saAmfSUAdminState** |
| `Screen / Monospace` | On-screen command output, source code identifiers, configuration keys, or file paths. | `/etc/opensaf/dtmd.conf` |
| **`Courier Bold`** | Explicit user input or shell commands to be typed by the operator. | **`immcfg -f imm.xml`** |
| *Italics* | Cross-document references, document titles, or file names. | *OpenSAF Overview Programmer's Reference* |
| `File > Exit` | Notation for navigating graphical or hierarchical menu paths. | `System > Configuration` |
| `<variable>` | Placeholder for a user-supplied variable, identifier, or node name. | `<node_name>`, `<CCB ID>` |
| `[parameter]` | Optional software parameters, buttons, or array index notation. | `[--force]`, `[timeout]` |
| `...` | Repeated sequence of items. | `node1, node2, ..., nodeN` |
| `. . .` | Omission of non-relevant lines or command output for brevity. | `. . .` |
| `a..b` | Integer ranges inclusive. | `0..4` (integers 0, 1, 2, 3, 4) |
| `\|` | Logical OR / alternative choices. | `lock \| unlock \| shutdown` |
| **`DANGER`** | Hazardous condition that could result in cluster loss or severe corruption. | Security / power alert |
| **`WARNING`** | Potential condition that may cause service interruption, failover, or error. | Failover trigger |
| **`NOTICE`** | Operational notice regarding configuration or storage damage. | Disk sync alert |
| **`NOTE`** | Helpful informational tip or implementation-specific detail. | Tip / Best practice |

---

## 📬 Document Comments & Feedback

OpenSAF is an open-source project hosted under the LGPL v2.1 license. Feedback, corrections, and contributions to this documentation set may be submitted to:

- **Mailing List / Email:** [`info@opensaf.org`](mailto:info@opensaf.org)
- **Project Repository & Bug Tracker:** [OpenSAF SourceForge Project](https://sourceforge.net/p/opensaf/)
- **Documentation Portal:** [OpenSAF Documentation](https://opensaf.sourceforge.io/)
