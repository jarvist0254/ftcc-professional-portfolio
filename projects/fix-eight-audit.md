# Fix_eight — Multi-Agent Quality & Remediation Suite
*A 433-case testing register and multi-agent defect remediation program across eight flagship Windows applications.*

**Status:** Completed audit & proven defect remediations — Machine-readable case register (`EIGHT_APP_CASE_REGISTER.csv`); nine evaluation pillars; three critical root-cause remediations verified with passing regression suites.

---

## Overview

In October 2026, J.E. Herizon LLC executed **Fix_eight**, an intensive quality assurance and defect remediation initiative covering the entire flagship Windows application catalog. To ensure applications met commercial production standards prior to Microsoft Store release, Fix_eight evaluated all eight applications against nine rigorous architectural pillars:

1. **Functionality:** Core user workflows and feature completeness.
2. **Offline Resilience:** Zero mandatory cloud connectivity; graceful degradation.
3. **Credential Hygiene:** Local platform-native encryption (Windows DPAPI / keyring).
4. **Data Integrity:** Atomic file writes and ACID database persistence.
5. **Ergonomics & Accessibility:** High-DPI UI scaling and responsive layouts.
6. **Installer Hygiene:** Clean installation and isolated user data paths.
7. **Store Compliance:** Microsoft Store policy adherence and AppxManifest integrity.
8. **Error Boundaries:** Non-crashing graceful degradation under unexpected I/O.
9. **Telemetry Minimization:** Zero cloud tracking, zero dark-pattern analytics.

---

## Product Coverage & Case Register

Authoritative register: `E:\JE_Horizon\Fix_eight\EIGHT_APP_CASE_REGISTER.csv` (433 discrete test cases).

| Product | Focus | Case Count | Store ID | Landing Page |
|---|---|---:|---|---|
| **ContractorDesk / Line** | Trade contractor dispatch & 10DLC telephony | 65 | `9NZQV4GSJTJH` | [line.jehorizon.com](https://line.jehorizon.com/) |
| **Wittlet & Worlds** | Procedural desktop companion & simulation | 45 | `9PD8F625136L` | [wittlet.jehorizon.com](https://wittlet.jehorizon.com/) |
| **PyRe GPU** | Python script workbench & GPU runner | 55 | `9MZPG0LSZS31` | [jehorizon.com/pyre/](https://jehorizon.com/pyre/) |
| **OmniHorizon AI CRM** | Small business ERP & on-device AI search | 54 | `9NJSG0QJ7WCF` | [jehorizon.com/omnihorizon/](https://jehorizon.com/omnihorizon/) |
| **FilePerch** | File staging & hash verification workbench | 56 | `9NQXV7HD0MMX` | [fileperch.jehorizon.com](https://fileperch.jehorizon.com/) |
| **ScopeStamp** | Work timestamping & tamper-evident receipts | 61 | `9NGKPNG4KC03` | [jehorizon.com/scopestamp/](https://jehorizon.com/scopestamp/) |
| **RevLatch** | Drawing revision diffing & review ledger | 45 | `9MTLXPDK0LLR` | [jehorizon.com/revlatch/](https://jehorizon.com/revlatch/) |
| **CutoverCheck** | PC move verification & SHA-256 baseline | 52 | `9MWGM28H0KZC` | [jehorizon.com/cutovercheck/](https://jehorizon.com/cutovercheck/) |
| **Total** | | **433** | | |

---

## Three Root-Cause Defect Remediations

### 1. CutoverCheck Zero-Byte File Export
- **Observed Defect:** Exporting verification reports produced empty 0-byte output files.
- **Root Cause Analysis:** Calling Windows WinRT API `FileSavePicker.PickSaveFileAsync()` automatically creates an empty 0-byte placeholder at the chosen destination path. CutoverCheck's atomic file writer (`Core/AtomicFile.cs:28-31`) enforced a strict safety check refusing to overwrite any existing file to prevent data loss. Seeing the WinRT placeholder, the writer aborted silently.
- **Remediation & Proof:** Updated `AtomicFile.cs` to detect and claim fresh WinRT save-picker placeholder handles via file age and process ownership. Verified across 18 failure-copy tests, 55 persistence tests, and live UNC network paths (`\\localhost\Users\...`).

### 2. OmniHorizon AI CRM Invoice Print & SVG Rendering
- **Observed Defect:** Printed invoices clipped columns, rendered with unreadable blurred text, and failed to display the company logo.
- **Root Cause Analysis:** 
  - `inventory.js:65` rendered 8 `<td>` elements against 9 `<th>` header tags.
  - The application completely lacked `@media print` rules; a modal `backdrop-filter: blur(4px)` rule flattened the invoice into 11 unselectable raster JPEGs.
  - `img/logo.svg` had malformed XML (`</feDropShadow>` improperly closing a `<filter>` tag).
- **Remediation & Proof:** Aligned table column schemas to 9/9, authored clean `@media print` stylesheets with text preservation, and repaired SVG XML syntax. Verified across 53/53 static gates.

### 3. RevLatch Review-State Loss on Refresh
- **Observed Defect:** When refreshing drawing reviews, completed approvals disappeared for non-critical regions.
- **Root Cause Analysis:** `app/match_flow.py:84-88` executed an SQL delete query with `WHERE kind NOT IN ('critical_region')`, followed by an `INSERT OR IGNORE` that repopulated records with blank schema defaults.
- **Remediation & Proof:** Re-architected item persistence with a content-fingerprinted `decision_ledger` storing immutable state transitions keyed by SHA-256 tile hashes. Verified across 28 unit tests and 315 regression sweeps.

---

## Related Work

- **[Product Platforms](product-platforms.md)** — Comprehensive architecture of the eight applications.
- **[Verification Factory](verification-factory.md)** — The multi-agent orchestration architecture behind the audit.
- **[Technical Decision Records](technical-decision-records.md)** — Architectural decision records behind the defect fixes.
