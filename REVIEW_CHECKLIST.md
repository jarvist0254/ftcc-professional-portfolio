# Pre-publication review checklist

Work through this checklist, in order, before every `git push` to a public remote. Do not skip a
section because "nothing changed there this time" — run the commands; do not rely on memory of
what the repo contains.

**Primary shell: Windows PowerShell.** Run every command below from the repository root
(`cd` there first). A Unix/WSL/Git Bash equivalent is given under each PowerShell command for
anyone working outside Windows — it is optional, not required.

A single true positive on any check blocks the push until fixed. Every hit must be reviewed
individually; do not bulk-dismiss a whole check because most hits look like false positives.

---

## 1. Secrets and identifiers

### 1.1 Drive letters / local Windows or WSL paths

Looking for: any local filesystem path that reveals machine layout (`C:\`, `E:\`, `/mnt/c/`, etc.).

```powershell
Get-ChildItem -Recurse -File -Exclude .git | Select-String -Pattern '[A-Za-z]:\\|/mnt/[a-z]/'
```

Optional Unix equivalent:
```bash
grep -rniE '[a-z]:\\\\|/mnt/[a-z]/' --exclude-dir=.git .
```

- [ ] Zero real hits (this checklist's own example patterns are the only expected matches).

### 1.2 `.env` files present in the tree

Looking for: any `.env`-style file that isn't a template.

```powershell
Get-ChildItem -Recurse -File -Filter '*.env*' | Where-Object { $_.FullName -notmatch '\\\.git\\' }
```

Optional Unix equivalent:
```bash
find . -iname '*.env*' -not -path './.git/*'
```

- [ ] No `.env` file (other than `.env.example`) is present anywhere in the tree.

### 1.3 Private key material

Looking for: PEM-style private key headers.

```powershell
Get-ChildItem -Recurse -File -Exclude .git | Select-String -Pattern 'BEGIN PRIVATE KEY|BEGIN RSA PRIVATE KEY|BEGIN OPENSSH PRIVATE KEY'
```

Optional Unix equivalent:
```bash
grep -rn 'BEGIN PRIVATE KEY\|BEGIN RSA PRIVATE KEY\|BEGIN OPENSSH PRIVATE KEY' --exclude-dir=.git .
```

- [ ] No private-key markers found.

### 1.4 Secrets, credentials, tokens, passwords, API keys

Looking for: any credential-shaped string.

```powershell
Get-ChildItem -Recurse -File -Exclude .git,REVIEW_CHECKLIST.md,.gitignore |
    Select-String -Pattern 'api[_-]?key|secret|password|passwd|token' -CaseSensitive:$false
```

Optional Unix equivalent:
```bash
grep -rniE 'api[_-]?key|secret|password|passwd|token' --exclude-dir=.git --exclude=REVIEW_CHECKLIST.md --exclude=.gitignore .
```

- [ ] Every `secret`/`token`/`password`/`api_key` hit reviewed individually and is not a real value.

### 1.5 IPv4 address patterns

Looking for: any real IP address, including inside diagram source files.

```powershell
Get-ChildItem -Recurse -File -Exclude .git | Select-String -Pattern '\b(\d{1,3}\.){3}\d{1,3}\b'
```

Optional Unix equivalent:
```bash
grep -rnE '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' --exclude-dir=.git .
```

- [ ] Every IP-address-shaped hit reviewed and is not a real address (`.svg` source files
      included — text inside SVGs matches this grep too, see section 3).

### 1.6 Email addresses

Looking for: any personal or third-party email address.

```powershell
Get-ChildItem -Recurse -File -Exclude .git | Select-String -Pattern '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}'
```

Optional Unix equivalent:
```bash
grep -rnE '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}' --exclude-dir=.git .
```

- [ ] Every email-address hit reviewed and is not a personal or third-party address.

### 1.7 Long hex hashes

Looking for: commit hashes, API tokens, or credential IDs rendered as long hex strings
(32+ hex characters — MD5/SHA-family length or similar).

```powershell
Get-ChildItem -Recurse -File -Exclude .git | Select-String -Pattern '\b[0-9a-fA-F]{32,}\b'
```

Optional Unix equivalent:
```bash
grep -rnE '\b[0-9a-fA-F]{32,}\b' --exclude-dir=.git .
```

- [ ] Every long-hex hit reviewed and is not a real hash, token, or key fragment.

### 1.8 UUID patterns

Looking for: UUIDs that could be run IDs, device IDs, or account identifiers.

```powershell
Get-ChildItem -Recurse -File -Exclude .git | Select-String -Pattern '\b[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}\b'
```

Optional Unix equivalent:
```bash
grep -rniE '\b[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}\b' --exclude-dir=.git .
```

- [ ] Every UUID-shaped hit reviewed and is not a real identifier tied to a system or account.

### 1.9 Student ID / account number style strings

Looking for: long digit runs that could be a student ID, account number, or credential ID.

```powershell
Get-ChildItem -Recurse -File -Exclude .git | Select-String -Pattern '\b\d{6,}\b'
```

Optional Unix equivalent:
```bash
grep -rnE '\b[0-9]{6,}\b' --exclude-dir=.git .
```

- [ ] Every long digit-run hit reviewed and is not a student ID, account number, or credential ID.

---

## 2. Banned content

Looking for: content that should never appear in this repo regardless of accuracy — military or
veteran affiliation references, degree/university claims, self-reported usage or revenue figures,
external audit claims, cloud deployment claims, self-issued scores, and the internal handle
the stale handle (see note below).

```powershell
Get-ChildItem -Recurse -File -Exclude .git |
    Select-String -Pattern 'military|veteran|\bWGU\b|Western Governors|b\.s\.|bachelor|graduated|degree|STALE-HANDLE' -CaseSensitive:$false
```

Optional Unix equivalent:
```bash
grep -rniE 'military|veteran|\bwgu\b|western governors|b\.s\.|bachelor|graduated|degree|STALE-HANDLE' --exclude-dir=.git .
```

- [ ] No military or veteran affiliation reference appears anywhere in the repo.
- [ ] No WGU / Western Governors / degree-claim language appears anywhere in the repo. University
      work is described only as coursework.
- [ ] No download count, user count, or revenue figure appears anywhere in the repo.
- [ ] No claim of an external audit appears anywhere in the repo.
- [ ] No claim of a cloud deployment on Azure or GCP appears anywhere in the repo. Cloud platform
      mentions are scoped to coursework, tooling familiarity, design, or prototype only, and say so
      explicitly.
- [ ] No self-issued numeric score or rating (e.g., "9.4/10", "X/100", a maturity rating) appears
      anywhere in the repo.
- [ ] No occurrence of any prior/stale GitHub handle appears anywhere in the repo.

---

## 3. Claim verification

- [ ] Every "live" / "deployed" / "published" claim in the repo either links to a working public
      URL, or has been reworded to past tense / "deployment status not independently verified."
- [ ] No certification is printed with a date or a credential ID. Each certification claim reads
      a working public verification link (Credly), or is marked pending if no link exists yet.
- [ ] No invented credential date or ID appears anywhere (a date or ID that cannot be verified
      against the real credential).

---

## 4. Assets

Looking for: any generated SVG that has hostnames, IPs, file paths, account names, vendor names,
or dates baked into its text content — SVG text layers can hide content that is invisible when the
image is only viewed rendered, not opened as text.

```powershell
Get-ChildItem -Recurse -File -Filter '*.svg' | Select-String -Pattern '[A-Za-z]:\\|/mnt/[a-z]/|\b(\d{1,3}\.){3}\d{1,3}\b|@|\b\d{4}-\d{2}-\d{2}\b'
```

Optional Unix equivalent:
```bash
grep -rniE '[a-z]:\\\\|/mnt/[a-z]/|([0-9]{1,3}\.){3}[0-9]{1,3}|@|[0-9]{4}-[0-9]{2}-[0-9]{2}' --include='*.svg' .
```

- [ ] Every SVG (`verification-lifecycle.svg`, `network-topology.svg`, `compute-routing.svg`, and
      any other diagram) reviewed at full size, opened AS TEXT and grepped, not just viewed
      rendered.
- [ ] Every SVG contains only generic stage/node labels — no hostnames, IPs, file paths, account
      names, vendor names, or dates.
- [ ] Every file in `assets/screenshots/` has the full redaction checklist in `assets/README.md`
      applied and checked off before it was committed.
- [ ] No diagram or screenshot contains a third-party name (employer, instructor, classmate).

---

## 5. Repository hygiene

- [ ] `.gitignore` is committed and in place **before** the first commit of this repository. Do
      not run `git init` / the first `git add` until this is confirmed.

### 5.1 Review working tree state

```powershell
git status
```

Optional Unix equivalent — same command, both shells:
```bash
git status
```

- [ ] `git status` shows no unexpected untracked or modified files immediately before push.

### 5.2 Confirm no archive, dossier, transcript, or coursework file is staged

```powershell
git status --porcelain | Select-String -Pattern '\.(zip|7z|tar|tar\.gz|rar)$|dossier|transcript|\.(pka|pkt|docx|pptx)$'
```

Optional Unix equivalent:
```bash
git status --porcelain | grep -iE '\.(zip|7z|tar|tar\.gz|rar)$|dossier|transcript|\.(pka|pkt|docx|pptx)$'
```

- [ ] No archive (`.zip`, `.7z`, `.tar`, `.tar.gz`, `.rar`), dossier, transcript, or coursework
      file (`.pka`, `.pkt`, `.docx`, `.pptx`) is staged.

### 5.3 Check commit history for anything committed and later deleted

**Deleting a file in a new commit does NOT remove it from git history.** If a secret or sensitive
file was ever committed, it remains recoverable from history even after a later commit deletes it.

```powershell
git log --all --diff-filter=A --name-only | Sort-Object -Unique |
    Select-String -Pattern '\.env|secret|credential|key|dossier|transcript'
```

Optional Unix equivalent:
```bash
git log --all --diff-filter=A --name-only | sort -u | grep -iE '\.env|secret|credential|key|dossier|transcript'
```

Also review full history, not just filenames, for anything that slipped through as file content:
```powershell
git log --all --full-history --oneline
```
```bash
git log --all --full-history --oneline
```

- [ ] History check run and reviewed — not skipped because "nothing was deleted this time."
- [ ] If this finds a real hit in history, the fix is rewriting history (e.g., `git filter-repo`)
      before this repository is ever pushed publicly — not just deleting the file in a new commit.
- [ ] **If a real secret was ever committed (even once, even if since deleted), it must be treated
      as compromised: rotate/revoke the actual credential.** Removing it from git history does not
      undo exposure if the repository was ever pushed or cloned before the rewrite — rotation is
      mandatory, deletion alone is not sufficient.

---

## 6. Final read

- [ ] **The two-minute test.** Read the README and one project entry start to finish, timed. At
      the end, can you state, from what's on the page alone:
      - the target role he wants,
      - his single strongest piece of proof,
      - one honest limitation he states about his own work?
      If any of the three is unclear or missing, the copy is not done — fix it before pushing.

---

## Note on the stale-handle check

`STALE-HANDLE` above is a placeholder. Substitute any previous or incorrect GitHub
username you have used before running those commands. The literal handle is deliberately
not written into this public file. The correct account for this repository is `jarvist0254`.
