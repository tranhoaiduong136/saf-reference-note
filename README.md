# OpenSAF Documentation (v5.26.02)

Welcome to the documentation repository for **OpenSAF Release 5.26.02**. This repository contains the Programmer's Reference guides converted from the original OpenOffice/LibreOffice (`.odt`) specification documents into developer-friendly Markdown (`.md`).

---

## 📖 Background

### What is OpenSAF?
**OpenSAF** is an open-source, carrier-grade middleware platform providing high availability (HA) consistent with the **Service Availability Forum (SAF)** Application Interface Specification (AIS). It delivers resilient cluster infrastructure, fault detection, automatic failover, redundancy models (2N, N+M, N-way, N-way-active), and distributed service coordination for mission-critical enterprise and telecommunications applications.

### About this Documentation Set
The OpenSAF documentation was originally authored in OpenDocument Text (`.odt`) format as comprehensive Programmer's Reference (PR) guides. This repository organizes both the source documents and their modern Markdown representations to improve readability, version control, and searchability.

---

## 📂 Repository Structure

```text
opensaf-documentation-5.26.02/
├── README.md                      # Project overview and roadmap
├── LICENSE                        # Centralized GNU LGPL v2.1 license text
├── GLOSSARY.md                    # Centralized OpenSAF & SAF AIS glossary and conventions
├── modified_markdown_version/     # Optimized Markdown with license, artifacts & front matter pruned (Phases 1-3)
│   ├── GLOSSARY.md                # Local copy of the centralized glossary
│   ├── images/                    # Extracted architecture diagrams referenced across documents
│   │   ├── amf_subparts.jpg
│   │   ├── cpsv_subparts.jpg
│   │   ├── edsv_subparts.jpg
│   │   ├── glsv_subparts.png
│   │   ├── mqsv_subparts.png
│   │   ├── overview_mds_components.jpg
│   │   ├── plmsv_architecture.jpg
│   │   └── smfsv_upgrade_flow.png
│   ├── OpenSAF_AMF_PR.md
│   ├── OpenSAF_CLMSv_PR.md
│   ├── OpenSAF_CPSv_PR.md
│   ├── ...
│   └── OpenSAF_SMFSv_PR.md
├── markdown_version/              # Raw converted GitHub Flavored Markdown (.md) documents
│   ├── OpenSAF_AMF_PR.md
│   ├── OpenSAF_CLMSv_PR.md
│   ├── OpenSAF_CPSv_PR.md
│   ├── ...
│   └── OpenSAF_SMFSv_PR.md
└── odt_version/                   # Original OpenDocument Text (.odt) source files
    ├── OpenSAF_AMF_PR.odt
    ├── OpenSAF_CLMSv_PR.odt
    ├── OpenSAF_CPSv_PR.odt
    ├── ...
    └── OpenSAF_SMFSv_PR.odt
```

---

## 🛠️ OpenSAF Services & Documentation Catalog

The repository covers 13 core OpenSAF Programmer's Reference documents:

| Document | Service Name | Description | Clean Markdown | Raw Markdown | ODT Source |
| :--- | :--- | :--- | :---: | :---: | :---: |
| **OpenSAF_Overview_PR** | OpenSAF Architecture Overview | General architectural guide, node directors, agents, SAF compliance, and subsystem interaction. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_Overview_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_Overview_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_Overview_PR.odt) |
| **OpenSAF_AMF_PR** | Availability Management Framework (AMF) | Workload lifecycle management, service units (SU), service groups (SG), health monitoring, and recovery policies. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_AMF_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_AMF_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_AMF_PR.odt) |
| **OpenSAF_IMMSv_PR** | Information Model Management (IMM) | Distributed cluster configuration database, object management (OM), object implementers (OI), and runtime attributes. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_IMMSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_IMMSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_IMMSv_PR.odt) |
| **OpenSAF_CLMSv_PR** | Cluster Membership Service (CLM) | Node discovery, dynamic cluster membership tracking, and node state change notifications. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_CLMSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_CLMSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_CLMSv_PR.odt) |
| **OpenSAF_CPSv_PR** | Checkpoint Service (CPSv) | Synchronous and asynchronous runtime state replication between active and standby components. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_CPSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_CPSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_CPSv_PR.odt) |
| **OpenSAF_EDSv_PR** | Event Distribution Service (EDSv) | Publish-subscribe event mechanism for distributed notifications and state updates across cluster nodes. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_EDSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_EDSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_EDSv_PR.odt) |
| **OpenSAF_GLSv_PR** | Global Lock Service (GLSv) | Distributed mutual exclusion and lock management across multiple cluster nodes and processes. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_GLSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_GLSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_GLSv_PR.odt) |
| **OpenSAF_LOGSv_PR** | Log Service (LOGSv) | Centralized, high-throughput cluster-wide logging facility conforming to the SAF AIS specification. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_LOGSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_LOGSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_LOGSv_PR.odt) |
| **OpenSAF_MQSv_PR** | Message Queue Service (MQSv) | Priority-based, persistent and non-persistent message queuing across nodes. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_MQSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_MQSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_MQSv_PR.odt) |
| **OpenSAF_NTFSv_PR** | Notification Service (NTFSv) | Standardized alarm and event reporting (ITU-T X.733 compliant alarms, security alarms, state change alarms). | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_NTFSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_NTFSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_NTFSv_PR.odt) |
| **OpenSAF_PLMSv_PR** | Platform Management Service (PLMSv) | Hardware and platform monitoring, FRU (Field Replaceable Unit) state management, and HPI bridging. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_PLMSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_PLMSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_PLMSv_PR.odt) |
| **OpenSAF_SMFSv_PR** | Software Management Framework (SMFSv) | Automated non-disruptive and rolling software upgrades and patch management across cluster nodes. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_SMFSv_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_SMFSv_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_SMFSv_PR.odt) |
| **OpenSAF_Extensions_PR** | OpenSAF Extensions | Vendor and OpenSAF-specific API additions and extensions beyond the standard SAF AIS specification. | [clean md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/OpenSAF_Extensions_PR.md) | [raw md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/OpenSAF_Extensions_PR.md) | [odt](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/OpenSAF_Extensions_PR.odt) |

---

## 📋 Execution Steps & Roadmap

### Step 1: Initial ODT to Markdown Conversion (Completed)
- [x] **Identify Pandoc conversion profile**: Standardize on GitHub Flavored Markdown (`pandoc <input.odt> -t gfm -o <output.md>`).
- [x] **Batch convert all documents**: Converted 13 `.odt` Programmer's Reference files to `.md`.
- [x] **Organize directory layout**:
  - [x] Move all original `.odt` files into [odt_version/](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/odt_version/).
  - [x] Move all generated `.md` files into [markdown_version/](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/markdown_version/).
- [x] **Verify corpus integrity**: Verified total 13 Markdown documents (~23,560 lines).

---

### Step 2: Content Reduction & Markdown Optimization

#### Phase 1: Strip Boilerplate License Text (~27.3% Size Reduction) (Completed)
- [x] **Extract centralized license**: Created root [LICENSE](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/LICENSE) file containing the full GNU LGPL v2.1 text.
- [x] **Create optimized workspace directory**: Created [modified_markdown_version/](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/) containing the pruned markdown files.
- [x] **Remove embedded license preambles**: Stripped ~494 lines of verbatim LGPL text from each of the 13 `.md` files (reduced from 23,560 lines to 17,137 lines, saving 6,423 lines / 27.3% total corpus size).
- [x] **Content preservation check**: Verified 100% byte-for-byte fidelity of all technical content, API specifications, and diagrams.

#### Phase 2: Eliminate Pandoc Conversion Artifacts (Completed)
- [x] **Strip raw HTML anchor tags**: Removed all 826 `<span id="anchor-..."></span>` elements generated during `.odt` parsing and converted internal anchor hyperlinks to standard Markdown links.
- [x] **Clean orphan image tags**: Extracted 8 core architecture diagrams into [modified_markdown_version/images/](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/images/), remapped diagram links, replaced table warning/info icons with standard Markdown badges (`**DANGER**`, `**WARNING**`, `**NOTE**`), and removed broken cover logos.
- [x] **Format normalization**: Removed Pandoc comments (`<!-- end list -->`), pruned empty headings, normalized whitespace linebreaks, and compressed consecutive empty lines across all 13 documents (corpus streamlined from 17,137 to 17,061 lines).

#### Phase 3: Prune Repetitive Front Matter & Metadata (Completed)
- [x] **Extract duplicate prefaces**: Pruned 1,020 lines of repetitive front matter boilerplate across all 13 documents (dead OpenOffice table of contents, broken list of tables/figures, chapter summaries with invalid anchor links, and redundant comments sections). Replaced with streamlined Markdown headers pointing to global documentation.
- [x] **Consolidate glossary**: Compiled all 85 unique abbreviations, acronyms, typographic conventions, and project feedback channels into a centralized [GLOSSARY.md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/GLOSSARY.md) (and [modified_markdown_version/GLOSSARY.md](file:///mnt/c/Users/admin/Desktop/opensaf-documentation-5.26.02/modified_markdown_version/GLOSSARY.md)), eliminating 13 duplicate abbreviation tables (corpus streamlined from 17,061 to 16,041 lines, saving 1,020 lines / 6.0% additional size reduction).

#### Phase 4: Modularization & Document Splitting
- [ ] **Split oversized documents**: Break down large monolithic files into manageable chapters:
  - [ ] `OpenSAF_IMMSv_PR.md` (5,575 lines) $\rightarrow$ `architecture.md`, `api_reference.md`, `configuration.md`
  - [ ] `OpenSAF_AMF_PR.md` (2,673 lines) $\rightarrow$ `architecture.md`, `api_reference.md`, `configuration.md`
- [ ] **Generate quick-reference cheatsheets**: Create concise tables for API signatures, return codes, and IMM class definitions for fast developer lookup and LLM context efficiency.

#### Phase 5: Verification & Quality Assurance
- [ ] **Automated linting**: Run `markdownlint` across all documents to ensure syntax compliance.
- [ ] **Content preservation check**: Validate that no functional API definitions or parameters were lost during the reduction process.
# saf-reference-note
