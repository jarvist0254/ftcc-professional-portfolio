# Herizon Linux — Sovereign Operating System
*A bootable, privacy-first Linux distribution built for local AI models, grounded terminal synthesis, and audited personal computing.*

**Status:** Implemented and verified — Developer Live ISO (`1.1.9-dev`, 7.28 GB); BIOS and UEFI in-guest boots verified PASS (2026-10-06); approaching stable 1.2 general availability release.

---

## Overview

Modern desktop operating systems increasingly convert local workstations into telemetry conduits, streaming user activity and context to cloud AI providers. **Herizon Linux** is an independent, bootable Linux distribution designed to restore complete operator sovereignty:

- **Debian 13 (Trixie) amd64 Foundation:** Built on upstream Debian with Linux Kernel 6.x and a customized, low-latency Xfce 4.18 desktop environment and LightDM greeter.
- **Default AI-Off Architecture:** Machine intelligence is **off by default**. When idle or toggled off, zero inference processes run in the background, zero memory is consumed by model weights, and context buffers are wiped.
- **On-Device Machine Intelligence:** Underpinned by a quantised Qwen3-0.6B Q8_0 CPU backbone paired with 14 specialized neural heads across two backbones and a shared-encoder routing extension.
- **Grounded Terminal Synthesis & Command RAG:** Natural language shell assistance grounded in local manpages, binaries, and system state with typed execution proposals—never unconstrained autonomous shell execution.
- **Offline Application Packs:** Pre-caches 9 offline APT package archives containing 170 software selections (networking, forensics, security, development, and media) for instant utility in air-gapped environments.

Live Landing Page: [https://jehorizon.com/herizon-linux/](https://jehorizon.com/herizon-linux/)

---

## Technical Specifications & Verification Receipts

| Property | Value | Verification Reference |
|---|---|---|
| **Distribution Version** | `1.1.9-dev` (October 2026) | `CURRENT_BUILD.json` |
| **Live ISO Artifact** | `herizon-os-1.1.9-dev-amd64.iso` | `vm-lab/releases/1.1.9-dev/` |
| **File Size** | 7,275,921,408 bytes (7.28 GB decimal) | Filesystem receipt |
| **SHA-256 Digest** | `51ba3f1e9a1d4b7b648748291d0372a3b8c30770f9a4b4a6aa62af631d534e74` | Cryptographic receipt |
| **Guest Hypervisor Boot** | **PASS 2026-10-06** (BIOS + UEFI) | `GUEST_BOOT_20261006.md` |
| **Container Acceptance Tests** | **11 / 11 PASS** | `desktop_controller: PASS` |
| **Host Integration Tests** | **18 / 18 PASS** | `os_local_models` + `os_controller` |
| **Routing Suite Accuracy** | **94.76%** (199 / 210 correct) | 210 frozen test cases; 0 Qwen API calls |
| **Default Live Credentials** | `herizon` / `herizon` | Sudo passwordless enabled |

---

## Architectural Breakdown

```
+-------------------------------------------------------------------------+
|                         Herizon Desktop Surface                         |
|   Xfce 4.18 Desktop · LightDM Greeter · Native AI Chat & Status Bubble  |
+-------------------------------------------------------------------------+
|                 Local Intelligence Orchestration Layer                  |
|  - Qwen3-0.6B Q8_0 CPU Backbone (Lazy loaded, zero idle footprint)      |
|  - 14 Specialized Output Heads (Intent, Extraction, Validation, Refusal)|
|  - Grounded Command RAG (Typed execution proposals, inspectable diffs)   |
|  - Literal-Field Autofill & System Diagnostic Helpers                   |
+-------------------------------------------------------------------------+
|                    Offline Capability Packs (9 Packs)                   |
|  170 Curated Packages: Dev, Forensics, Networking, Media, Security, Ops |
+-------------------------------------------------------------------------+
|                  Upstream Core Foundation (amd64)                       |
|           Debian 13 (Trixie) · Linux Kernel 6.x · Live-Boot Core        |
+-------------------------------------------------------------------------+
```

### Subprocess Environment Remediation
During hypervisor qualification of earlier builds (1.1.0-dev through 1.1.1-dev), in-guest execution revealed 8/8 orchestration failures. Root-cause debugging proved that the background head-worker launched in a detached subprocess inheriting only default shell environment paths. Under the guest environment's alien working directory, the process terminated immediately with `ModuleNotFoundError: No module named 'li'`.

In build 1.1.9-dev, worker environment resolution was re-engineered to derive hermetically from package anchors in `li/os_local_models.py` and baked into the image manifest (`os_image/build_iso.sh`). Live in-guest tests confirmed active PID persistence and continuous communication.

---

## Roadmap to Stable 1.2

1. **Current Milestone (1.1.9-dev):** Verified live developer ISO, guest hypervisor boot, native chat bubble, and frozen routing benchmarks.
2. **Next Milestone (1.1.10-dev):** Chat latency budget optimization, interactive onboarding, and bare-metal USB flash boot certification.
3. **General Availability (Stable 1.2):** Persistent disk installer (Calamares), physical GPU passthrough support, signed repository updates, and public mirrors.

---

## Related Work

- **[Product Platforms](product-platforms.md)** — The desktop and web application ecosystem of JE Horizon.
- **[The Harness & J-Link](the-harness-and-jlink.md)** — The high-density hardware chassis and multi-GPU compute fabric.
- **[Technical Decision Records](technical-decision-records.md)** — Decision records behind Herizon Linux's default-off CPU inference architecture.
