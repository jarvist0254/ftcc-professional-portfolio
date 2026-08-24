# Local-Inference Desktop Platform
*A desktop application whose core decision inference runs entirely on local CPU, with no hosted model API in that path — separately disclosed external features aside.*

## Problem

Software that gives a user AI-assisted decisions usually sends their data to a hosted model API — a per-inference cost, a network dependency, and the vendor's servers seeing every input.

I wanted to know whether a desktop application could give up that hosted-API path for its core decision logic: run its own model locally, on ordinary consumer CPU, and still ship as a normal installable program. The application operates in the options-trading domain, but the problem is architectural — a core inference path that runs on the user's machine, with no hosted model API in it.

## What I built

A Windows desktop application whose decision layer is a nine-model gradient-boosting ensemble, trained offline and shipped as bundled artifacts, executing entirely on local CPU at run time with no hosted model API in that path. Optional external features exist outside it and are disclosed below, not folded into this claim.

A locally shipped model still needs licensing, updates, and payment, so I built a separate hosted backend — a licensing and commerce service (Python/FastAPI, PostgreSQL) on a container platform, integrated with a payment provider. The client caches a signed license locally, so short outages don't stop it from working.

Two of the ensemble's model bases are published publicly under an open license; a third, larger model has been announced but not yet published. A test-enforced promotion policy authorizes only three of the nine for the highest-consequence code path, the one that can act on the user's behalf — checked in code, not decided by hand.

**Disclosed external data paths, separate from core inference:** stored credentials are encrypted at rest using the OS's native credential-protection facilities. Licensing and commerce run through the hosted backend above. An optional feature lets a user send selected context to a third-party AI service of their choice — before it leaves the machine, a recursive redaction pass strips sensitive identifiers out, including ones nested inside larger structures. The application also integrates with several brokerage APIs to place, size, and exit trades within user-configured limits.

## Technical decisions

**Local CPU inference instead of a hosted model API, in the core decision path.** Calling a hosted model per decision is simpler but doesn't work offline and costs money per inference at real volume. Running locally means that call does not send user data to a hosted API — a privacy property in that specific path, not the whole application.

**A gradient-boosting ensemble instead of a neural network.** Boosted trees train and run on ordinary consumer CPU, without requiring a GPU for this path, and each model stays inspectable — I can see what it's doing, which matters more than accuracy a neural net might offer at the cost of opacity.

**Test-gated promotion instead of manual sign-off.** I could have decided by eye which models were ready. Instead a model must clear a mechanical test bar first, removing my own judgment as the single point of failure.

**Recursive redaction before any third-party API call.** Anything sent to an external model's API should be assumed to be logged by someone else. Rather than trust a caller to scrub sensitive fields, the redaction pass walks the full structure, nested fields included, before transmission.

## Public proof or evidence available

**Publicly verifiable now:** the two published model bases, including their architecture, are public under an open license — a reader can inspect them directly.

**Available only on request:** the test suite enforcing the promotion policy, the redaction logic, the licensing backend's architecture, and the internal defect review below are internal material, available to a legitimate reviewer on request, not public proof.

## Outcome

The project demonstrates that a decision-support application can run its core inference offline, on consumer CPU, with no hosted model API in that path, while still supporting a normal licensing and payment lifecycle through separately disclosed hosted services. It also demonstrates a code-enforced pattern for restricting which trained models may act with consequence, and a privacy control applied before data crosses a trust boundary.

## Honest scope boundary

No trading-performance results are published or claimed here. Purchases and downloads are currently paused, so the product is not commercially active. Only Windows is shipped; macOS and Linux builds don't exist. An internal review found a number of defects, mostly low severity and outside the highest-consequence path; none were reported reachable through the promotion-gated path. No third-party security audit has occurred, and this is a single-developer project with no external code review process.
