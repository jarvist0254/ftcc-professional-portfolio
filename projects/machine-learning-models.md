# Machine-Learning Model Publication

*Open model bases, local-first design constraints, and a deliberate line between what is published and what is not.*

**Status:** Mixed — the published architecture bases are Publicly demonstrated; production weights and a larger model in the same family are Implemented and unpublished; a set of task-specific security models is Prototype. See below for which claim applies to which part.

## Problem

"I published a model" can mean several different things — an architecture design, a trained checkpoint, a training corpus, or a production system built on all three — and treating them as one claim overstates what a reader can actually inspect. I wanted to publish the part of this work that stands on its own as design evidence, decide deliberately what to hold back, and be able to explain that decision rather than blur it into a single sentence.

## What I built

Two small language-model bases, built around a proprietary tokenizer and an original architecture rather than fine-tuned from a borrowed foundation model, published openly under a permissive open-source license. What is published is the architecture and an untrained, randomly initialized checkpoint that a reader can train themselves — not a finished, trained production model. A third, larger model in the same family has been announced but is not published; it does not exist as a public artifact today, and I state that precisely rather than letting "two models published" round up to "the model family is published."

Model size was not chosen for a benchmark leaderboard position. It was chosen to run at usable speed on ordinary consumer CPU, with no GPU or hosted inference service required — a constraint documented in more detail on the [local-inference platform page](local-inference-platform.md), where it shapes an entire application's runtime path. Each published base ships with a model card describing its architecture, tokenizer, and intended training procedure.

Separately, a set of small, task-specific models was built for a security-operations tooling prototype — one narrow model per task (triage, summarization, log normalization, and similar) rather than one general model doing everything. That work is labeled **Prototype**, is not published, and is described in more detail on the [security-operations page](security-operations.md).

## Technical decisions

**An original tokenizer and architecture, instead of fine-tuning a borrowed foundation model.** Fine-tuning an existing large model reaches a working demo faster, but it does not demonstrate the ability to design a model from first principles, and it inherits that foundation model's licensing and provenance. Building the tokenizer and architecture from scratch was slower, and was the point.

**Small, CPU-sized models, instead of a larger model that assumes GPU-hosted inference.** A bigger model is generally more capable on paper. It also fails the constraint that mattered here — running locally, on hardware a user already has, with no server dependency — so model family and parameter count were chosen against that constraint first, not against a leaderboard.

**Publishing the architecture and an untrained checkpoint, instead of the trained production weights.** These are different acts with different consequences. Publishing a design shows how something is built and lets someone else reproduce or critique it. Publishing trained production weights hands over the finished, differentiated asset itself — the product of real data and training investment — and I chose to keep that line rather than let "open architecture" quietly become "give away the product."

**Withholding training data and routing logic, instead of shipping a fully reproducible package.** A complete reproduction package looks more impressive on the surface. It would also require publishing data-provenance detail and the internal decision logic that selects which model handles which task — material that is either not mine to publish freely or is the part of the system that differentiates it from a generic reimplementation.

**Announcing the third, larger model rather than staying silent about it or publishing it early.** Saying nothing would read as evasive once the smaller two bases are already public; publishing a model before it is a real, testable artifact would misstate its status. Announcing it while it remains unavailable is the accurate middle position.

## Publicly demonstrated / evidence available

**Publicly verifiable now:** the two published model bases and their architecture and tokenizer design are public under an open-source license, and a reader can inspect the architecture directly. No current public source verifying download or adoption figures is cited on this page, so no such figure is stated here — that omission is deliberate, not an oversight, and it is more credible than a number I cannot currently source.

**Available only on request, not independently verifiable from this repository:** the model cards' full training-procedure documentation, the status and timeline of the announced third model, and the task-specific security-model prototype's design and evaluation notes are available to a legitimate reviewer on request.

## Outcome

The published bases demonstrate original architecture and tokenizer design work that a reader can inspect directly, independent of any claim about a trained product. The decision about what to withhold — trained weights, training data, routing logic — was made deliberately and is stated here rather than left for a reviewer to guess at.

## Honest scope boundary

No download count, user count, or adoption figure appears on this page, because no current verified public source exists to cite one, and an unsourced number would be worse than none. The larger third model is announced, not published — treat it as not yet available, on no committed date. The security-operations models are a prototype, never deployed, and not published. Publishing an architecture is not the same claim as publishing a finished, audited product: no independent third party has reviewed either the published architecture or the unpublished production system built on top of it.

## Related work

- [Quantitative research system](quantitative-research.md) — the research harness these model bases' trained descendants are evaluated inside.
- [Local-inference platform](local-inference-platform.md) — the application whose local-CPU runtime constraint drove these models' size and family.
- [Security operations practice](security-operations.md) — the task-specific security-model prototype in full.
- [Technical decision records](technical-decision-records.md) — the reasoning behind publishing an architecture while withholding trained weights, alongside twelve other decisions.
