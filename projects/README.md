# Projects

Twelve pages covering cybersecurity, systems engineering, local-first AI, distributed systems,
software products, and research methodology. Every page follows the same structure and ends with an
honest scope boundary.

## Status labels

| Label | Meaning |
|---|---|
| **Deployed and operated** | Provisioned, deployed, and run on a real cloud or container platform — infrastructure that existed and served traffic, distinct from a build that only ran locally |
| **Publicly demonstrated** | A public repository, URL, release, or sanitized artifact in this repository supports it |
| **Implemented** | Built and tested, but not publicly deployed or independently verified |
| **Prototype** | Partially built or experimental |
| **Research instrument** | A methodology or measurement system, not proof of product performance |
| **Coursework** | An academic exercise, not professional production work |
| **Available on request** | Private supporting evidence exists and is deliberately withheld |

## Security and infrastructure

- **[Multi-Agent Verification Factory](verification-factory.md)** — a task pipeline in which a
  deterministic verifier, never the worker, decides whether work actually happened. Insufficient
  evidence fails closed. *Implemented.*
- **[Security Operations Practice](security-operations.md)** — graded coursework in access control,
  incident response, and posture assessment, plus a self-built SOC tooling prototype. Both strands
  labelled separately. *Coursework and Prototype.*
- **[Secure Home Lab and Compute Infrastructure](infrastructure-networking.md)** — a private
  device-authorized overlay in place of a forwarded public port, and compute routed by measured
  CPU/GPU crossover rather than assumption. *Implemented.*

## Software and AI

- **[Herizon Linux Sovereign OS](herizon-linux.md)** — a bootable, privacy-first Linux distribution built
  on Debian 13 (Trixie) for on-device AI (Qwen3-0.6B CPU backbone, 14 trained heads) and verified
  personal computing; first stable release (1.2 GA) pending. *Implemented and verified.*
- **[Product Platforms](product-platforms.md)** — eight commercial Windows desktop applications published on the
  Microsoft Store with dedicated perspective landing pages on `jehorizon.com` and public technical documentation repositories on GitHub.
  *Deployed and operated.*
- **[The Harness & J-Link Compute Fabric](the-harness-and-jlink.md)** ([GitHub](https://github.com/jarvist0254/the-harness)) — an engineered 4-bay GPU compute chassis
  (FreeCAD parametric solid BRep) and software fabric proving cross-vendor consumer P2P limits and implementing
  ThunderEP host-staged DMA. *Engineered system and research instrument.*
- **[Local-Inference Desktop Platform](local-inference-platform.md)** — a desktop application
  running its core model inference locally on CPU, with no hosted model API in that path and
  optional external features disclosed separately. *Implemented.*
- **[Machine-Learning Model Publication](machine-learning-models.md)** — twenty-three model repositories
  on Hugging Face with 16,041 all-time downloads (audited 2026-10-04), local-first constraints, and clear licensing boundaries.
  *Publicly demonstrated.*

## Distributed systems and research

- **[Distributed Systems](distributed-systems.md)** — a network designed to settle payment only
  after independent validators re-check a worker's computation. *Implemented; not independently
  audited.*
- **[Quantitative Research System](quantitative-research.md)** — a walk-forward instrument built to
  invalidate its own findings, including the formal retraction of results that failed review.
  *Research instrument.*

## Engineering judgment

- **[Technical Decision Records](technical-decision-records.md)** — seventeen decisions, each with
  the alternatives rejected, what the evidence showed, and where nothing was measured. The best
  single page for a technical interviewer.

---

Supporting material behind these pages is deliberately withheld from this repository. See the
[evidence index](../evidence/README.md).
