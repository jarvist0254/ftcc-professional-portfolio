# Projects

Nine pages covering cybersecurity, systems engineering, local-first AI, distributed systems,
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

- **[Local-Inference Desktop Platform](local-inference-platform.md)** — a desktop application
  running its core model inference locally on CPU, with no hosted model API in that path and
  optional external features disclosed separately. *Implemented.*
- **[Product Platforms](product-platforms.md)** — several independently built product surfaces, the
  API and integration work behind them, and a documented platform trade-off argued on workload
  rather than preference. *Mixed; per-product status stated.*
- **[Machine-Learning Model Publication](machine-learning-models.md)** — two openly published model
  bases, local-first design constraints, and the reasoning behind what was deliberately withheld.
  *Publicly demonstrated in part.*

## Distributed systems and research

- **[Distributed Systems](distributed-systems.md)** — a network designed to settle payment only
  after independent validators re-check a worker's computation. *Implemented; not independently
  audited.*
- **[Quantitative Research System](quantitative-research.md)** — a walk-forward instrument built to
  invalidate its own findings, including the formal retraction of results that failed review.
  *Research instrument.*

## Engineering judgment

- **[Technical Decision Records](technical-decision-records.md)** — thirteen decisions, each with
  the alternatives rejected, what the evidence showed, and where nothing was measured. The best
  single page for a technical interviewer.

---

Supporting material behind these pages is deliberately withheld from this repository. See the
[evidence index](../evidence/README.md).
