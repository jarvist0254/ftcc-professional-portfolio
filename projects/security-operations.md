# Security Operations Practice
*Graded security-operations coursework, plus a self-built SOC tooling prototype.*
**Status:** Coursework and Prototype — two distinct strands at different maturity levels; see below.

## Problem
Security operations work comes down to three linked disciplines: knowing who should have access to what, reconstructing what happened during an intrusion, and finding the gaps in how data is protected before someone else does. I wanted graded practice in all three, and I wanted to test a separate idea: whether small, purpose-built language models wired into a lightweight telemetry pipeline could take some manual load off triage work, without a large hosted model answering every query.

This entry covers two distinct strands, not one project at one maturity level — the coursework is finished and graded; the tooling prototype is not.

## What I built
**Coursework (three graded artifacts, instructor-assigned scenarios, not live incidents):**
- A security-governance analysis: a role-based access-control review across 26 user accounts, covering off-boarding gaps, shared-account handling, and logging deficiencies, with a remediation plan.
- An incident-response case study: a phishing-to-remote-access-trojan intrusion worked through an 8-event timeline, correlating intrusion-detection alerts, command-and-control traffic, and lateral movement.
- A data-security posture assessment: 9 identified deficiencies (weak transport encryption, insecure remote access exposure, and similar) with a remediation plan.

**Prototype (self-directed, partially implemented):** a SIEM/SOC platform concept built around a central correlation engine that ingests telemetry and scores it for analyst review, paired with a lightweight endpoint-telemetry agent collecting process, network, registry, and memory signals. Detection and analysis logic is split across six small, task-specific models covering triage, summarization, rule authoring, prioritization, playbooks, and log normalization. Connector templates map the engine's output onto eight common SIEM and log platforms' ingestion formats.

## Technical decisions
**Six small task-specific models instead of one general model.** Each model has a narrow, evaluable job, cost and hardware requirements stay predictable, and a bad output in one task doesn't degrade another. The rejected alternative — one large general-purpose model handling every task — is harder to evaluate and harder to run cheaply at the edge.

**Connector templates against a common event schema, not bespoke per-platform integrations.** Normalizing to one internal schema first means adding a ninth platform is a new template, not a new pipeline. The cost: templates are drafted against vendor documentation, not proven against a running instance.

**Endpoint telemetry scoped to what distinguishes intrusion behavior — process lineage, network egress, persistence points — rather than collecting everything.** Broad collection buries an analyst in volume; the agent targets a smaller signal set chosen for discriminative value.

**Detection logic embedded in models rather than as portable rules.** A deliberate trade-off, not a clear win. A Sigma or KQL rule is something another analyst can read and tune line by line; a model's output approximating that rule is not. I chose it to test whether model-based triage was viable at all, accepting the loss of portability a standalone rule file would have kept.

## Publicly demonstrated / evidence available
**Publicly verifiable now:** nothing in this entry links to a public artifact — no live deployment, no public repository, no sanitized diagram is published for this project yet. That absence is stated here deliberately rather than left implicit.

**Available only on request, not independently verifiable from this repository:** the three coursework artifacts exist as submitted, graded written deliverables — access-control review, incident-response case study, posture assessment — available for review on request to a legitimate reviewer, never as a linkable public document. The prototype exists as design documentation, a model training pipeline, a model manifest, and a connector-template registry, also available on request. No public deployment link exists for the prototype.

## Outcome
The coursework built the analyst's decision process: reasoning about access governance, reconstructing an intrusion timeline from alert data, and turning a posture gap into a remediation plan, against frameworks including NIST SP 800-53, the CIS Controls, and ISO/IEC 27001. The prototype attempted to automate the tedious parts of that same process — triage, correlation, first-draft detection content — with small, self-hosted models instead of a vendor API.

## Honest scope boundary
This is coursework and a prototype, not professional experience. I have not worked a live incident, and I have not deployed or administered any commercial SIEM or EDR product. The prototype has never processed live traffic and is partially implemented. The connector templates are unvalidated against any live instance of the platforms they target. Detection logic lives inside the models, not as portable Sigma or KQL rules another analyst could independently read or tune. I have no threat-hunting or offensive-security experience, and the prototype has not been security-reviewed.

## Related work

- [Verification factory](verification-factory.md) — separation of duties applied to automated task claims, the same control principle behind the access-governance review here.
- [Machine learning models](machine-learning-models.md) — the small, task-specific model approach used by the SOC prototype's triage and correlation logic.
- [Technical decision records](technical-decision-records.md) — the fuller record of judgment calls behind this and other projects, including ones that did not make this page.
