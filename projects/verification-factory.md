# Multi-Agent Verification Factory
*An orchestration system that treats an automated worker's report of success as a claim to be checked, not a fact.*
**Status:** Implemented — built and tested by a single operator; never independently reviewed or audited.

## Problem

AI coding and research agents readily report success, and are sometimes wrong about it. In a workflow dispatching several worker models to modify code, run tests, or produce data artifacts, a wrong "SUCCESS" is worse than an honest failure, since nothing downstream flags it. If the operator must manually re-check every claim, automation stops saving labor.

The problem is governance, not modeling: given that a worker's account of its own work cannot be trusted on its word, how do you decide, cheaply and repeatably, whether a task was completed — and refuse it when the evidence does not support that?

## What I built

A three-role task pipeline where no single actor certifies its own work. A manager writes a JSON contract pinning a task's scope and acceptance criteria. A worker executes and returns a receipt: a claim. A separate deterministic verifier, run after the worker finishes, reads actual filesystem state and issues the only verdict that counts: VERIFIED_PASS, VERIFIED_FAIL, ESCALATE, or INDETERMINATE. Verification compares filesystem snapshots taken before and after the task — path, size, modification time, content hash per file — so a file identical before and after cannot count as that task's output. Most tasks also require a second worker, from a different model family, to audit the result before closing. Work routes to the cheapest capable lane first, with costlier lanes seeing only unresolved work. The system has 4,395 durable task artifacts on record — contracts, receipts, verdicts, adjudications, and escalations — across 157 Python modules and 7 JSON schemas.

![Verification lifecycle](../assets/verification-lifecycle.svg)

## Technical decisions

**A deterministic verifier, not an AI judge.** An LLM grading another LLM's work reintroduces the trust problem the system exists to solve — a program has no incentive to be generous.

**Snapshot diffing, not existence checks.** The system originally checked only whether an output file existed. A worker once reported SUCCESS, exit code 0, for a task asking it to modify a file that did not exist — it had created the file instead, and a leftover file from an earlier rejected attempt satisfied the existence check. Existence proved nothing about what this attempt produced. The fix was pre/post snapshot comparison, now codified by a regression test: an unchanged file is stale, not passing.

**Fail closed on missing evidence, not fail open.** An unchecked item silently counting as a pass is the failure mode that makes verification worthless, so "not checked" is treated as failing by default, never as passing.

**Route by measured claim-honesty, not assumed capability or price.** Lanes are ranked by how often a worker claimed success but verification failed, not by price or reputation. Across 246 measured verdicts, claim-failure rates ranged from about 8% for the best lane to about 25% for the worst — the cheapest lane was also the most honest, an assumption based on price alone would have missed.

The organizing idea — the actor performing work and the actor attesting to it must be different — is separation of duties and independent audit, security-control principles applied here to automated software work.

## Publicly demonstrated / evidence available

**Publicly verifiable now:** the diagram above, showing the fail-closed lifecycle and the separation between worker output and verdict, is in this repository for anyone to inspect directly.

**Available only on request, not independently verifiable from this repository:** the artifact counts above are internal records, not a source a reader can check themselves. The stale-file incident, its fix and regression test, and the claim-failure comparison across worker lanes are internal material available on request to a legitimate reviewer — not public proof until shown.

## Outcome

On real task volume, not a designed benchmark, independent verification caught a false completion claim a trust-the-worker pipeline would have accepted, and routing by measured honesty rather than assumed capability changed which lane a task should reach.

## Honest scope boundary

This is a single-operator system, never independently reviewed. The verifier itself has never been audited by anyone but me — it verifies workers, but nothing verifies it. There is no formal latency or throughput measurement. Adaptive routing that shifts allocation automatically as a lane's error rate moves is designed, not implemented (**Prototype**, on paper only); routing is revised from historical batches by hand. Some lane statistics, especially for lower-volume lanes, rest on small samples and should be read as directional, not conclusive.

## Related work

- [Distributed systems](distributed-systems.md) — the broader pattern of independent validation before a result is allowed to settle, applied outside this specific pipeline.
- [Security operations](security-operations.md) — the same separation-of-duties principle, applied to intrusion analysis and access governance instead of automated task claims.
- [Technical decision records](technical-decision-records.md) — the fuller record of judgment calls behind this and other projects, including ones that did not make this page.
