# Assets

Status: **no asset in this directory has been produced yet.** Every item below is a placeholder
describing what will be made and what rules it must follow. Do not commit a real file into any of
these slots until it satisfies the "must show" / "must not show" constraints for that asset.

## Diagrams

### `verification-lifecycle.svg`
**What it must show:** the path a piece of work takes through the verification system —
`contract (task definition)` → `worker output (a claim)` → `deterministic verifier` → `verdict`.
The diagram must make two things visually obvious:
- the **fail-closed path**: insufficient or ambiguous evidence routes to a FAIL/BLOCKED verdict,
  never to a default PASS.
- **the worker never certifies itself** — there is no arrow from "worker output" directly to
  "verdict." The verifier is a separate stage with its own box, positioned so the eye cannot
  mistake it for part of the worker's own output.

**What it must not show:** any tool name, model name, vendor name, internal project codename, file
path, or run identifier. Use generic stage labels only ("Task Contract", "Worker Claim",
"Deterministic Verifier", "Verdict: Pass / Fail-closed").

### `network-topology.svg`
**What it must show:** two panels, side by side or before/after — (1) the port-forwarding pattern
that was replaced (router exposing a forwarded port to the public internet) and (2) the
private-overlay-network pattern that replaced it (devices joined to a private mesh, no inbound
ports exposed to the public internet). The diagram should communicate *why* the second pattern is
the better security posture (no public attack surface, encrypted device-to-device tunnel,
device-level auth) without needing caption text longer than a label.

**What it must not show — hard requirement:** NO real IP addresses, NO real hostnames, NO real
device names, NO account names, NO service provider name tied to an identifiable account. Node
labels must be strictly generic: "workstation", "phone", "tablet", "router", "internet". This
diagram is illustrating a *pattern*, not documenting the actual network.

### `compute-routing.svg`
**What it must show:** the measured CPU/GPU crossover point — a simple chart or flow diagram
illustrating that small-batch work is routed to CPU workers and large-batch work is routed to GPU
scoring, because GPU sits idle (dispatch/transfer overhead dominates) below a measured batch-size
threshold. Axis or flow should read as "batch size" vs "routing decision," with the crossover point
marked. Use relative/measured-shape framing (e.g., "below threshold" / "above threshold") rather
than a fabricated precise number if the exact figure is not independently reproducible from a
public source.

**What it must not show:** internal benchmark file names, run IDs, machine names, or absolute
throughput numbers that cannot be reproduced/verified by a reader. If a number is used, it must be
a number that is genuinely defensible and stated conservatively.

## `screenshots/`
Sanitized screenshots illustrating the architecture (e.g., a dashboard view, a verifier output
panel, a job queue view). **No screenshot has been captured or committed yet.**

### Redaction checklist — apply to every screenshot BEFORE it is committed
A screenshot fails this checklist until every applicable item is crossed off:

- [ ] Crop or blur every visible file path (no `/mnt/...`, no `C:\`, `E:\`, or any drive letter).
- [ ] Crop or blur any username or account name.
- [ ] Crop or blur any hostname or machine name.
- [ ] Crop or blur any IP address (private or public).
- [ ] Crop or blur any email address.
- [ ] Close or crop out browser tabs and bookmarks bars — they leak visited sites and account state.
- [ ] Crop or blur terminal prompts that show `user@host` or a working-directory path.
- [ ] Crop or blur window titles that contain a file path or project path.
- [ ] Dismiss or crop out notification popups (OS notifications, chat popups, email previews).
- [ ] Crop or blur the system tray / menu bar icon row — it reveals installed software and running
      services.
- [ ] Crop or blur any account identifier, session token, or session ID visible in a UI.
- [ ] Confirm no third-party name (employer, instructor, classmate) appears anywhere in the frame.
- [ ] Re-view the final image at full resolution (not just thumbnail) before committing — blur
      artifacts and small text are easy to miss at thumbnail size.

Only after every applicable box is checked does a screenshot move from a local staging location
into `screenshots/` for commit.
