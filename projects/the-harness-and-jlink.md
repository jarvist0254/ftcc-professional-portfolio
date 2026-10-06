# The Harness & J-Link Compute Fabric
*Engineered 4-bay GPU compute chassis, FreeCAD parametric BRep, and cross-vendor host-staged DMA software fabric.*

**Status:** Engineered system & research instrument — Parametric CAD Revision 4 verified body-clear; cross-vendor P2P limitations formally analyzed and proven; ThunderEP host-staged DMA bridge verified across NVIDIA discrete and AMD integrated/discrete adapters.

---

## Overview

Consumer artificial intelligence and scientific computing hardware is artificially segmented by proprietary accelerator ecosystems. Builders seeking to aggregate inexpensive consumer GPUs across vendors face mechanical packaging constraints and driver-level peer communication barriers.

**The Harness** addresses both challenges through unified mechanical and systems engineering:
- **Chassis Engineering:** A 3D-printable, high-density computing enclosure adapting an AM5 host (AMD Ryzen 7 7700 on an MSI PRO X870-P WIFI motherboard with PCIe x4/x4/x4/x4 bifurcation) to support up to four external 8 GB GPU accelerators while preserving an independent host/display GPU (NVIDIA RTX 5070 Ti).
- **The Cross-Vendor P2P Verdict:** Formal research (`docs/10_JLINK_CROSS_VENDOR_P2P_VERDICT_AND_PLAN.md`) establishing that **direct cross-vendor dedicated-VRAM peer-to-peer (P2P) access does not exist on consumer hardware on Windows or Linux as of 2026**.
- **J-Link Host-Staged DMA Fabric:** An optimized software fabric implementing ThunderEP-style asynchronous copy queues over Direct3D 12 cross-adapter shared heaps and Vulkan staging buffers, achieving exact CPU-reference parity and detecting thirteen corruption controls.

Source Repository: [https://github.com/jarvist0254/the-harness](https://github.com/jarvist0254/the-harness)

---

## Mechanical CAD Architecture (Revision 4)

Modeled parametrically in FreeCAD (`cad/`):
- **Unified Continuous Solid Core:** Resolves prior structural collision defects by nesting upright load posts entirely within the inter-card gap volume ending below card surface D.
- **Envelope Specifications:** 
  - Compact card configuration: 188 × 200 × 150 mm
  - Extended card configuration: 188 × 200 × 199 mm
- **Bifurcation Topology:** Because AMD Zen architectures (`family >= 0x17`) pass root-complex allow-lists and IOMMU ACS checks without external PCIe switches, the design routes four discrete Gen4 x4 links directly from CPU lanes through shielded riser ribbons.

---

## Numerical Verification & Sensitivity Proof

Authoritative receipt: `records/root_continuation_20261004/FP32_FRESH_NUMERICAL_ACCEPTANCE_20261004T1910Z.json`

| Benchmark Domain | Configuration | Verified Result |
|---|---|---|
| **Vulkan F32 Whole Model** | NVIDIA discrete + AMD APU (both device orders) | **PASS**: Exact CPU-reference parity across **151,936 logits** per order. |
| **Matrix Slice Tests** | Heterogeneous dual-device split | **PASS**: 12,288 comparisons passed per device. |
| **Residual MLP Layers** | 8 unique resident layers across NVIDIA & AMD | **PASS**: Maximum final floating-point error ≈ **8.35e-6**. |
| **AMD HIP Native Kernel** | AMD gfx1036 architecture | **PASS**: 1 MiB affine kernel verified against host baseline. |
| **D3D12 Cross-Adapter** | Shared cross-adapter system memory heaps | **PASS**: 256 KiB cross-adapter transfer verified with corruption controls. |
| **Corruption Sensitivity** | 13 negative controls (OpenCL & Vulkan) | **PASS**: All 13 injected corruptions detected; non-zero exit codes. |

---

## The Cross-Vendor Architecture

```
                    +---------------------------+
                    |    System Host Memory     |
                    | (Pinned Direct3D 12 SHM / |
                    |   Vulkan Staging Buffer)  |
                    +-------------+-------------+
                                  ^
                 DMA Copy Queue   |   DMA Copy Queue
                 (Async Engine)   |   (Async Engine)
                                  v
+------------------------+                 +------------------------+
|       Device 0         |                 |        Device 1        |
| (NVIDIA Discrete GPU)  |                 | (AMD Radeon Discrete)  |
+------------------------+                 +------------------------+
```

Rather than attempting unsupported direct VRAM P2P copies or suffering unoptimized driver fallbacks, J-Link treats host staging as a documented, high-throughput first-class path:
1. **Single-Hop Crossings:** Avoids ring-relay serialization. Every device-to-device boundary crosses exactly once via host memory.
2. **Dedicated Copy Queues:** Moves tensors via hardware DMA copy engines, keeping compute shaders focused exclusively on inference.
3. **Capacity Planner:** Allocates layer blocks across 4x 8 GB cards (32,768 MiB gross capacity, 26,624 MiB post-reserve allocatable).

---

## Related Work

- **[Herizon Linux](herizon-linux.md)** — Sovereign operating system for on-device local AI.
- **[Home Lab and Infrastructure](infrastructure-networking.md)** — High-performance local networking and storage tiering.
- **[Technical Decision Records](technical-decision-records.md)** — TDR-14: Host-staged DMA bridging over unsupported consumer P2P.
