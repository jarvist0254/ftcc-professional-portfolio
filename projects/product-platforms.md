# Product Platforms — The Eight Applications Ecosystem
*A suite of eight local-first Windows desktop applications published on the Microsoft Store, paired with perspective web landing pages on jehorizon.com.*

**Status:** Deployed and operated — All eight applications published or staged across the Microsoft Store ecosystem with dedicated public landing pages on `jehorizon.com` and `*.jehorizon.com`; comprehensive 433-case Fix_eight quality audit completed (October 2026).

---

## Overview

Between early 2026 and Fall 2026, J.E. Herizon LLC (Thomas Jarvis) shifted from early cloud SaaS experiments to build, package, and distribute a cohesive portfolio of eight local-first desktop applications targeting Windows 10/11 and professional operators. Each application adheres to a strict architectural philosophy:
- **Local-First Execution:** Sensitive records and core workflows run entirely on the operator's machine without mandatory cloud subscriptions.
- **Embedded Persistence:** Data is stored in local embedded engines (SQLite, encrypted JSON, or structured files) with platform-native encryption (Windows DPAPI).
- **Zero Telemetry Leakage:** Operating system analytics, keystrokes, and customer data never leave the local environment.
- **Unified Brand & Store Integration:** Each application features a dedicated public landing page on `jehorizon.com` and a registered listing in the Microsoft Store catalog.

---

## The Eight Applications: Architecture & Perspective Landing Pages

```
+----------------------------------------------------------------------------------------------------+
|                                    JE HORIZON APPLICATION ECOSYSTEM                                |
+------------------+-------------------+--------------------+-------------------+--------------------+
| ContractorDesk   | Wittlet / Worlds  | PyRe GPU           | OmniHorizon CRM   | FilePerch          |
| Dispatch & CRM   | Desktop Companion | Script Workbench   | Small Business ERP| Archive Workbench  |
+------------------+-------------------+--------------------+-------------------+--------------------+
| ScopeStamp       | RevLatch          | CutoverCheck       | Herizon Linux     | The Harness        |
| Evidence Timestamp| Review Ledger    | Move Verifier      | Sovereign OS      | Compute Chassis    |
+------------------+-------------------+--------------------+-------------------+--------------------+
```

### 1. ContractorDesk / Line
- **Functional Scope:** Complete operations desk for trade contractors (HVAC, plumbing, electrical, roofing). Provides estimate calculation, automated quoting, work order management, invoicing, offline dispatch, and 10DLC TCR-compliant automated SMS lead qualification.
- **Architecture:** Next.js operator desk, FastAPI telephony backend, Twilio/Vapi webhooks, and an Expo/EAS Android mobile companion.
- **Perspective Web Landing Page:** [https://line.jehorizon.com/](https://line.jehorizon.com/) and [https://jehorizon.com/line/](https://jehorizon.com/line/)
- **Microsoft Store Listing:** [ContractorDesk on Microsoft Store](https://apps.microsoft.com/detail/9NZQV4GSJTJH) (`9NZQV4GSJTJH`)

### 2. Wittlet & Wittlet Worlds
- **Functional Scope:** Lightweight procedural desktop companion and room simulation. Offers interactive character dialogues, customizable virtual spaces, and reactive desktop presence with an ultra-lightweight memory footprint.
- **Architecture:** Native Windows desktop client, procedural generative heuristics, local state serialization, and zero cloud dependencies.
- **Perspective Web Landing Page:** [https://wittlet.jehorizon.com/](https://wittlet.jehorizon.com/) and [https://jehorizon.com/wittlet/](https://jehorizon.com/wittlet/) · [https://jehorizon.com/wittlet-worlds/](https://jehorizon.com/wittlet-worlds/)
- **Microsoft Store Listing:** [Wittlet on Microsoft Store](https://apps.microsoft.com/detail/9PD8F625136L) (`9PD8F625136L`)

### 3. PyRe GPU
- **Functional Scope:** Windows developer workbench for executing, queuing, and profiling Python scripts with optional GPU acceleration. Enforces CPU/GPU resource bounds, logs execution lifecycles, and captures run receipts.
- **Architecture:** Python 3.11+, PySide6 GUI, Vulkan/OpenCL shader probes, and isolated subprocess execution.
- **Perspective Web Landing Page:** [https://jehorizon.com/pyre/](https://jehorizon.com/pyre/)
- **Microsoft Store Listing:** [PyRe GPU on Microsoft Store](https://apps.microsoft.com/detail/9MZPG0LSZS31) (`9MZPG0LSZS31`)

### 4. OmniHorizon AI CRM
- **Functional Scope:** Unified local-first CRM, ERP, and quoting workspace for small businesses. Features client relationship ledgers, inventory tracking, invoice generation, and on-device natural language search over business records.
- **Architecture:** Embedded SQLite persistence, local sentence embeddings for retrieval, lightweight webview GUI with HTML5/CSS print engines, and Windows DPAPI credential protection.
- **Perspective Web Landing Page:** [https://jehorizon.com/omnihorizon/](https://jehorizon.com/omnihorizon/)
- **Microsoft Store Listing:** [OmniHorizon AI CRM on Microsoft Store](https://apps.microsoft.com/detail/9NJSG0QJ7WCF) (`9NJSG0QJ7WCF`)

### 5. FilePerch
- **Functional Scope:** High-throughput desktop file staging, search, and integrity verification workbench. Rapidly locates files across multi-terabyte disk arrays, calculates recursive cryptographic digests, and plans deduplication before execution.
- **Architecture:** High-concurrency multithreaded file crawler, SHA-256 hash streaming pipeline, and disk staging queue.
- **Perspective Web Landing Page:** [https://fileperch.jehorizon.com/](https://fileperch.jehorizon.com/) and [https://jehorizon.com/fileperch/](https://jehorizon.com/fileperch/)
- **Microsoft Store Listing:** [FilePerch on Microsoft Store](https://apps.microsoft.com/detail/9NQXV7HD0MMX) (`9NQXV7HD0MMX`)

### 6. ScopeStamp
- **Functional Scope:** Digital evidentiary timestamping and scope-verification workbench. Enables field operators and contractors to document change orders, extra site work, and photographic proof with cryptographic tamper evidence and verifiable receipts.
- **Architecture:** Local cryptographic hashing (SHA-256/HMAC), EXIF metadata extraction, PDF audit package generation, and tamper-evident append-only ledger.
- **Perspective Web Landing Page:** [https://jehorizon.com/scopestamp/](https://jehorizon.com/scopestamp/)
- **Microsoft Store Listing:** [ScopeStamp on Microsoft Store](https://apps.microsoft.com/detail/9NGKPNG4KC03) (`9NGKPNG4KC03`)

### 7. RevLatch
- **Functional Scope:** Engineering revision diffing and decision-auditing workbench. Allows engineers, architects, and managers to compare drawing revisions, track critical-region changes, and preserve an unalterable sign-off ledger across review cycles.
- **Architecture:** Python/PySide6 desktop client, SQLite decision ledger, raster/vector diff engine, and content-fingerprinted change tracking.
- **Perspective Web Landing Page:** [https://jehorizon.com/revlatch/](https://jehorizon.com/revlatch/)
- **Microsoft Store Listing:** [RevLatch on Microsoft Store](https://apps.microsoft.com/detail/9MTLXPDK0LLR) (`9MTLXPDK0LLR`)

### 8. CutoverCheck
- **Functional Scope:** Pre-flight and post-flight PC migration verification tool. Captures a read-only recursive SHA-256 baseline before a system migration, compares files on the destination, and exports a standalone, portable HTML audit certificate.
- **Architecture:** .NET 8, C#, WinUI 3 / Windows App SDK, high-speed streaming SHA-256 calculation, and atomic file writers.
- **Perspective Web Landing Page:** [https://jehorizon.com/cutovercheck/](https://jehorizon.com/cutovercheck/)
- **Microsoft Store Listing:** [CutoverCheck on Microsoft Store](https://apps.microsoft.com/detail/9MWGM28H0KZC) (`9MWGM28H0KZC`)

---

## Technical Decisions & Infrastructure Discipline

1. **Static Edge Delivery for Marketing Surfaces:** Marketing pages do not change per visitor; running origin servers adds unnecessary recurring cost. Frontends are deployed globally via Cloudflare Pages with zero-config TLS, custom subdomains, and instant edge routing.
2. **Local-First Architecture over SaaS Hosting:** Rather than hosting multi-tenant databases with high operational liability, core business workflows run on user workstations, reducing server costs to near-zero while offering absolute data privacy.
3. **Microsoft Store Distribution:** Delivering software as packaged MSIX / Windows desktop applications via the Microsoft Store provides verified code signatures, clean OS isolation, and trusted distribution.
4. **Fix_eight Quality Audit:** Before wide commercial availability, all eight applications were subjected to a 433-case automated quality audit evaluating offline resilience, credential hygiene, and file atomicity (see [Fix_eight Audit](fix-eight-audit.md)).

---

## Related Work

- **[Fix_eight Audit & Remediation](fix-eight-audit.md)** — The 433-case multi-agent quality audit across all eight applications.
- **[Herizon Linux](herizon-linux.md)** — Sovereign operating system for local AI.
- **[The Harness & J-Link](the-harness-and-jlink.md)** — Multi-GPU compute chassis and host-staged DMA software fabric.
- **[Technical Decision Records](technical-decision-records.md)** — Architectural decision records for local persistence and desktop integration.
