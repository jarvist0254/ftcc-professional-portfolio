# Technical Decision Records

*The alternatives I rejected, and what the evidence showed.*

Anyone can list what they chose. The more useful record is what was rejected, and why — that reasoning is where engineering judgment actually shows. Where a decision rests on something measured, the sample size is stated with the conclusion, not left implicit; a number without its sample size is decoration, not evidence. A few of these were wrong at first and corrected in public — the correction is part of the record, not an edit to hide.

## 1. Deterministic verifier over an AI judge

**Decision.** Task completion in an automated worker pipeline is certified by a deterministic, rule-based verifier, not a second AI model judging the first's output.

**Alternatives considered.** LLM-as-judge grading the worker; trusting the worker's self-report; manual human review of every claim.

**Why this choice.** A second model grading a first reintroduces the trust problem the control exists to remove — the judge can be wrong or persuadable the same way the worker can. A program checking fixed, pre-declared conditions has no incentive to be generous.

**What the evidence showed.** In operation, this caught a false completion claim a self-report-only pipeline would have accepted.

**Boundary.** It only checks what was written into acceptance criteria in advance, not a quality dimension nobody encoded as a check.

## 2. Snapshot-diff verification over file-existence checks

**Decision.** Verification compares a filesystem snapshot before a task to one after — path, size, timestamp, content hash — rather than checking only whether an expected output file exists.

**Alternatives considered.** Existence-only checks; trusting the worker's reported exit code; checking file size alone.

**Why this choice.** Existence proves nothing about what a specific attempt produced. A worker once reported success for a task asking it to modify a file that did not exist — a leftover file from an earlier, rejected attempt satisfied the check.

**What the evidence showed.** That incident led directly to the fix: pre/post comparison of path, size, timestamp, and hash. A file byte-identical before and after is now stale, not passing.

**Boundary.** Diffing proves a file changed, not that the change is correct — that still depends on the task's declared acceptance criteria.

## 3. Fail-closed evidence handling over default pass

**Decision.** An evidence item that could not be, or was never, checked counts as a failure by default, not a pass.

**Alternatives considered.** Default-pass on unchecked items; partial credit when evidence is missing; escalating every ambiguous case to a human.

**Why this choice.** A system that silently treats "not checked" as "fine" looks rigorous while leaking exactly the failures it exists to catch.

**What the evidence showed.** Not measured — a design judgment, codified as a rule rather than tested against a labeled dataset.

**Boundary.** Fail-closed trades false negatives for false positives — a correct result can be rejected for missing evidence, not wrong logic. That only works with an escalation path for a human to review a rejected-but-plausible case.

## 4. Private overlay networking over forwarded public ports

**Decision.** Remote access to a personally operated workstation runs over a private, device-authorized mesh overlay network. No service on the workstation is intentionally exposed on a public port.

**Alternatives considered.** Forwarding SSH or a file-share port through the home router; a commercial remote-desktop relay; a site-to-site VPN with a shared key.

**Why this choice.** A forwarded port publishes an authentication endpoint to the open internet, scanned continuously by bots. A device-authorized overlay means there is no inbound public listener to defend.

**What the evidence showed.** Not a controlled measurement; the reasoning is architectural — an absent surface versus a defended one.

**Boundary.** This removes one exposure route, not a claim of zero attack surface, and it is a personal-scale decision, not an enterprise architecture.

## 5. Measured CPU/GPU crossover routing over GPU-always

**Decision.** Compute work is routed to CPU or GPU per workload by a measured throughput crossover point, not by defaulting every workload to the accelerator.

**Alternatives considered.** Always route to GPU; a fixed static split decided once at design time; route by job type without measuring either path.

**Why this choice.** Launch and transfer overhead can dominate at small batch sizes; a warm CPU can beat a GPU exactly where that overhead outweighs the compute gain.

**What the evidence showed.** Crossover thresholds were measured per workload class across batch sizes. Below threshold the GPU is slower, sometimes by a wide margin, and the router leaves it idle.

**Boundary.** Thresholds are specific to the workloads and hardware measured and do not generalize without remeasuring.

## 6. Interval and task-boundary flushing over per-write fsync

**Decision.** Durable output is flushed at task boundaries and on a fixed interval, rather than synchronously after every write.

**Alternatives considered.** Synchronous flush per record; buffering entirely in memory with a flush only at exit; a separate write-ahead log.

**Why this choice.** Per-record synchronous flushing guarantees durability but forces every write to wait on disk I/O, which dominated wall-clock time on the affected workload.

**What the evidence showed.** A controlled, same-machine comparison showed a roughly threefold throughput improvement with byte-identical output, on one workload's tail segment — a small sample, not a full run. An earlier, separate claim of a low-single-digit-percent speedup was traced to an arithmetic error and withdrawn.

**Boundary.** The figure describes one tail segment under one comparison, not a general multiplier, and interval flushing accepts a wider data-loss window on a crash than per-write fsync does.

## 7. Storage tiers by measured latency over vendor specification

**Decision.** Storage devices are assigned to roles — active work versus archive — by measured write-durability latency, not rated specification, with a rule that live computation never targets an archive-tier device.

**Alternatives considered.** Trusting datasheet write-speed ratings; a single storage tier for everything; assigning tiers by drive age or price.

**Why this choice.** Rated specifications describe best-case conditions that do not reliably predict sustained write-commit behavior under a real workload.

**What the evidence showed.** A separate compression-planning estimate for this layout assumed roughly double the space savings actually achieved; the plan was corrected once the gap was measured.

**Boundary.** The measurements are specific to the devices and workload on hand and say nothing about a different drive model or access pattern.

## 8. Local CPU inference over a hosted model API for the core path

**Decision.** The core decision-inference path in a desktop application runs entirely on local CPU, with no hosted model API in that path.

**Alternatives considered.** Calling a hosted model API per decision; a hybrid path falling back to local only when offline; shipping a smaller model to the cloud instead of the client.

**Why this choice.** A hosted call in the core path means the application does not work offline, costs money per inference at real volume, and sends every input to a third party's servers.

**What the evidence showed.** Not a controlled comparison — an architectural property of where inference executes, not a measured benchmark.

**Boundary.** This covers the core inference path only. Optional external features send data to third parties when explicitly enabled, separately disclosed. A GPU handles other, unrelated workloads on the same machine.

## 9. Test-gated model promotion over manual approval

**Decision.** A trained model is authorized for the highest-consequence code path — able to act on a user's behalf — only after clearing a mechanical test bar in code, not by a developer deciding by eye.

**Alternatives considered.** Manual sign-off before each release; promoting every model that finishes training by default; review with no automated gate.

**Why this choice.** Deciding by eye makes the developer's own judgment the single point of failure. A mechanical bar applies the same standard every time and is auditable after the fact.

**What the evidence showed.** Only a minority of the trained models in the ensemble clear the bar and reach the highest-consequence path.

**Boundary.** The gate only tests what its criteria measure. It does not certify performance outside what the bar checks for.

## 10. Recursive redaction before optional third-party context transfer

**Decision.** When a user opts into sending context to a third-party AI service, a recursive redaction pass strips sensitive identifiers — including ones nested inside larger structures — before anything leaves the machine.

**Alternatives considered.** Trusting each call site to scrub fields manually; a flat, single-level redaction pass; no redaction.

**Why this choice.** Outbound text sent to a third-party API should be assumed logged by someone else. A single-level scrub misses fields nested inside objects, so the pass has to walk the full structure.

**What the evidence showed.** Not measured — a design control applied at the trust boundary, not a tested reduction rate.

**Boundary.** This covers the one explicit, opt-in transfer path. It does not cover data handling inside that third party's own systems once received.

## 11. A lean container platform over a heavier managed cloud stack, by workload

**Decision.** Hosted services split across two platforms by complexity: a heavier managed cloud stack for one complex, stateful workload, a leaner container platform for simpler, stateless services.

**Alternatives considered.** Standardizing on the heavier stack; migrating everything to the leaner platform; a single self-managed server for all services.

**Why this choice.** The heavier stack's managed state and tooling earn their overhead on the one workload that needs them; simpler services don't need that machinery.

**What the evidence showed.** Not published here as a verified figure — cost comparisons informed the internal reasoning but are not stated as verified facts on this page.

**Boundary.** Both platforms remain in active use for different services. This is a workload-matched split, not a migration away from either.

## 12. Retracting invalid research findings over preserving attractive results

**Decision.** Several headline findings from an internal research effort were formally retracted once later review invalidated them, rather than kept with a qualifying footnote.

**Alternatives considered.** Softening the language instead of withdrawing it; keeping the number with a caveat attached; quietly dropping the finding without documenting why.

**Why this choice.** A caveated but still-published number keeps doing the damage a clean one does — it gets cited at a strength the analysis does not support. A plain retraction, with the error's mechanism stated alongside it, does not mislead a later reader.

**What the evidence showed.** Four mechanisms were found: a backtest that ignored realistic exit paths and overstated results; a result that held only on a small, hardcoded set of instruments, not a representative universe; a threshold calibrated on placeholder data instead of real signal; and duplicate rows counted as separate economic events instead of deduplicated first. None of the retracted figures are restated here.

**Boundary.** Retraction addresses findings affirmatively checked and found invalid, not a claim that every other unretracted finding has been independently re-verified.

## 13. Independent-validator agreement for useful-compute settlement

**Decision.** In a distributed network where participants are paid for real computational work, a worker's own report of a completed job does not settle payment; an independent panel of validators re-checks the work first.

**Alternatives considered.** Trusting the worker's self-reported completion; a single validator instead of a panel; settling immediately and clawing back later.

**Why this choice.** The same failure shows up everywhere self-attestation stands in for verification: the party with the incentive to claim success is the party reporting whether it succeeded. Independent agreement removes that conflict before money moves.

**What the evidence showed.** Not measured on this page — a design principle at the architecture level, not a claim about agreement rates or throughput.

**Boundary.** This describes the settlement mechanism's design intent, not a claim the network is live, publicly deployed, or carrying real economic volume. It shares its principle with decision 1: the actor performing the work is never the actor certifying it.

---

Several measurements above — the flushing comparison, the compression estimate, the storage latency figures, the CPU/GPU crossover thresholds — come from a single workload on a single machine, observed once or a few times, not repeated trials elsewhere. They are stated at that strength deliberately: re-measuring them in a different environment could move the number, and one figure already had to be corrected once checked properly.
