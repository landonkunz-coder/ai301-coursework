# Contribution: vault_read module shows an error when disabling certificate validation

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Landon Kunz
**GitHub Username:** landonkunz-coder
**Issue:** https://github.com/ansible-collections/community.hashi_vault/issues/456
**Pull Request:** https://github.com/ansible-collections/community.hashi_vault/pull/510

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Grew issue-select into issue-scout with a written search strategy, then patched the rubric after the first real scan**

I installed the scout over my Unit 1 skill, rewrote my fit profile, and built a search strategy of four `gh search issues` queries and eight free ranking rules (Claude drafted most of the queries and rules from my profile; I test-ran each query by hand and changed what didn’t hold up). Those hand runs showed things I didn't want to see like a `Stellar Wave` program label filling the top results and single owners bulk-filing issues across many repos, so I added `--sort updated`, label exclusions, and a one-result-per-owner rule before paying for a scan. The first scan (308 unique issues, 238 after the drop rules) accepted all five of its top picks, including aws-powertools #8584, which bundles six unrelated bugs, so I added a fifth fail condition to `single-bounded-change` (re-checked against Unit 1 eval issues 04, 06 and 14: 3/3 agreed) and a rule dropping issues less than 7 days old. Grading my three Issue Selection picks then correctly rejected nanocoder #1745 as claimed, but still accepted LPC #296 even though two PRs referencing it are already merged, a gap I haven’t patched yet.

**Unit 6: Installed the voice guard outside my global settings, then used it and a 37-line repo memory file on my real claim**

I added four upstream rules to my voice guide (Claude drafted them and I edited them and approved them) and installed the voice guard, but I put the hook in a project `settings.local.json` in my notes folder instead of `~/.claude/settings.json`, a deviation from INSTALL.md step 4 that leaves my global settings untouched and means the guard only fires when I start Claude Code from that folder. That fell short on claim day because the posting steps run from the Unit 6 materials folder, so I copied the hook there, kept it out of git with `.git/info/exclude`, and re-ran the wiring test; my guard log shows that stop and then a pass on my claim and on my repro note before each one posted (Oct 25), though anything I post from the browser or a plain terminal still skips it. For repo memory I ran one scoped orientation over `plugins/module_utils/` and its unit tests (five read-only tool calls), and Claude distilled it, my contributor-guide notes, and a unit-test command I ran myself (1025 passed) into a 37-line `CLAUDE.local.md` that stays out of git. My CodePath credits ran out on Oct 25, so the guard checks, the orientation run, and my repro-check grades that day ran on my personal Claude account.

**Unit 7: Installed the ship-gate by hand, wrote a two-check manifest, and shipped PR #510 through it**

I installed the ship-gate from INSTALL.md's manual fallback instead of install.py, because the script always edits my global Claude Code settings, so both hooks (voice guard, then ship-gate) live in my hashi_vault clone's project settings. The first wiring test only got one stop: on macOS's built-in Python 3.9, check_gate.py crashes as it loads (it uses `str | None` type hints) and Claude Code treats that exit code as non-blocking, so a real `gh pr create` would have gone out ungated until I pointed the hook and the manual doorway at my venv's Python 3.12 and re-ran the test. My manifest gates on the module_utils unit tests and the sanity tests, mined from the repo's CI file and contributor guide, with dated exclusions for the full unit suite (three lookup tests already fail on main), the integration tests (they need Docker) and shellcheck (not installed). The gate passed my branch twice, once before and once after I fixed a leftover template fence its voice note flagged, and I opened PR #510 through it.

**Unit 8: Installed the PR monitor, wrote a playbook from the repo's real wait times, and overruled its first read of my PR**

I installed the PR monitor and wrote my playbook from hashi_vault itself: the contributor guide states no wait, and the five most recent merged PRs from outside contributors got a first maintainer response after 0.1, 0.0, 11.0, 5.9 and 10.1 days, so my threshold is 14 quiet days before a first nudge (Claude drafted the playbook from that data and my unit 6 pings rules, and I approved it). The free `--state` check classified PR #510 as "FEEDBACK-IN: review feedback is in", but the only comment on the thread was SonarCloud's bot reporting a passed quality gate, so a full run would have drafted a reply to a bot. I added a Response rule that a bot comment never gets a reply and that a bot-only thread means waiting, and the full run (one Sonnet call on 2026-11-04) followed it: it drafted a wait note quoting my rule, with the next check around 2026-11-16. A wait note stays off the thread, so I posted nothing.

---

## Why I Chose This Issue

I went into claim day with nothing usable from Unit 5. My scout had accepted LPC #296, but when I imported the issue's JSON into the live generator it already worked, because two merged PRs had fixed it. WrenAI #1127 needed Docker and Ollama, and nanocoder #1745 had an open PR. So I searched again: the AI301 Issue Selection site filtered to Python bugs, then free GitHub searches Claude ran for me, first for `good first issue` + `bug` and then for `help wanted` + `bug`. The `good first issue` route kept hitting the same wall: beets and Qiskit ban AI on their good first issues, BookWyrm doesn't accept AI-generated code at all, and I use Claude and disclose it. beets #5491 was my first grade, and my scout rejected it twice over: someone had asked for it five days earlier, and beets' AI policy bans AI on good first issues. The `help wanted` search turned up ansible-collections/community.hashi_vault #456: `vault_read` with cert auth and `validate_certs: false` fails with `cacert must be True, a file_path, or valid CA Certificate.` even though a cacert is set. Nobody had asked for it in two years, there were no linked PRs, and Ansible's community AI policy allows AI with disclosure. This one was looking promising.

My scout rejected #456 too, and I chose it anyway. The only required check it failed was `commits-alive`: the newest commit on main was 102 days old against my 90-day rule. I read that as a slow repo, not a dead one, because a collaborator answered issue #502 the same day in August and the last release was within the year. Every other required check passed, and I checked the two open PRs near this code (#504 and #505) and neither one fixes it. This is why I overrode my scout. It fits my profile: Python, a dev tool, one bug in an auth method with a repro I can run myself. It's also closer to real IT work than anything else I looked at, since Ansible and Vault are tools teams use to manage servers and secrets. I want to learn how a real collection is tested and reviewed, and how to work a fix through maintainers who answer slowly, since it seems like this might be a recurring problem I might face. I claimed it here: https://github.com/ansible-collections/community.hashi_vault/issues/456#issuecomment-6407917750

My honest read of the scout: it was right about the things that would have hurt me (a fresh claim on beets, an AI ban, an open PR on nanocoder), but it's not something I can trust blindly. I need to check it myself and find something that fits what I want to do. It got the issue age wrong on beets (it said 356 days when it was about 720). It still counts a two-year-old "can I work on this?" as stale, while the course says any ask means taken. It looked for CONTRIBUTING.md and missed the contributor guide in `docs/`. It can't see PRs that touch the same code without linking the issue, and it still accepts an issue that merged PRs already fixed. Re-grading my three Unit 5 picks with the patched rubric gave the same three verdicts, so the scope patch didn't change anything on those, only added difficulty notes. I may need to adjust my scout.

**Toolkit**

- Tool: issue-scout in direct mode (`issue-scout: grade <url>`), with my rubric after this unit's scope patch: `single-bounded-change` replaced by `scope-feasible` plus a difficulty note, my old fail conditions (3) to (5) kept as named cases, and `spec-settled` narrowed so an open "how" question makes an issue ambitious instead of failing.
- What it produced: five grades this unit. beets #5491 reject (`unclaimed`: claim on Oct 16; `ai-policy-allows`: AI banned on good first issues). hashi_vault #456 reject on `commits-alive` only (last commit Jul 11, 102 days). Re-grades of my Unit 5 picks: LPC #296 accept (straightforward), WrenAI #1127 accept (ambitious), nanocoder #1745 reject (open PR #1756), the same verdicts as Unit 5.
- Difficulties: a date slip on beets `fresh-issue` (356 days instead of about 720); `unclaimed` uses a 90-day stale rule that is looser than the course's "any ask means taken"; `ai-policy-allows` passed #456 for the right answer but the wrong reason, since it missed both the contributor guide in `docs/docsite/rst/` and Ansible's org-wide AI policy; it can't see unlinked PRs touching the same code (#504, #505); it still accepts issues already fixed by merged PRs (#296). I haven't regression-checked the scope patch against the Unit 1 eval set.
- What changed: I applied the scope patch with a dated line in `scan-notes.md`, and I overrode the scout's reject on #456 with the reasons above instead of following it. Patches I'd make next: count any "can I work on this" comment as a claim, look in `docs/` and org-level policies before grading the AI check, and treat a merged PR that references the issue as "check it still reproduces" instead of a pass.

---

## Understanding the Issue

**Problem Description**

When you use the `vault_read` module (or anything else in the collection) with `auth_method: cert` and `validate_certs: false`, it fails with hvac's raw error `cacert must be True, a file_path, or valid CA Certificate.`, even though the task sets `cacert`. The message makes it look like the CA file is missing or wrong, when the real cause is that certificate validation is turned off, and nothing tells the user that or how to fix it.

**Expected Behavior**

The reporter expected the secret to be read. hvac refuses cert login without some certificate checking, so with validation off the login can't work as configured. What should happen is a clear failure that says cert auth needs `validate_certs: true` (with `ca_cert` if the CA isn't trusted by the system). The other option, having cert auth use the `cacert` anyway, is still the maintainers' call; I offered it on the issue and in the PR.

**Current Behavior**

On main (collection 7.2.0, `eb163dd`, hvac 2.4.0) the issue's task fails before any request reaches Vault:

```
fatal: [localhost]: FAILED! => {"changed": false, "msg": "Task failed: Module failed: cacert must be True, a file_path, or valid CA Certificate."}
```

The same task with `validate_certs: true` gets past that check, so `validate_certs: false` is the trigger.

**Affected Components**

- `plugins/module_utils/_connection_options.py`, `_boolean_or_cacert()` (lines 258-261): when `validate_certs` is false, the client's `verify` becomes `False` and `ca_cert` isn't used.
- `plugins/module_utils/_auth_method_cert.py`, `HashiVaultAuthMethodCert.authenticate()`: calls `client.auth.cert.login()` without `cacert`.
- hvac 2.4.0, `hvac/api/auth_methods/cert.py` (lines 287-293): `login()` raises `CertificateAuthError` when `cacert` is empty and the client's `verify` is falsy, before sending anything.
- `plugins/modules/vault_read.py` (line 99) only catches `NotImplementedError` and `HashiVaultValueError`, so hvac's error escapes raw. Cert auth is shared by every module and lookup in the collection.

**Toolkit**

- Tool: my repo memory (`CLAUDE.local.md`) from one scoped, read-only orientation run in Claude Code over `plugins/module_utils/` and its tests, plus Claude in my Cowork session walking me through the code path with six questions (Q1-Q6).
- What it produced: a map of the three files that matter and how they connect, and the explanation above: `validate_certs: false` drops `ca_cert`, cert auth never passes `cacert`, and hvac refuses before any request.
- Difficulties: I asked Claude to answer the Q1-Q6 code-reading questions instead of writing them myself, then checked each answer against the quoted lines and approved them, so this explanation started as Claude's reading. The orientation run, like my scout, never mentioned the contributor guide in `docs/`.
- What changed: I didn't trust "no Vault server needed" from reading alone; my repro points the task at an unused port, and the bug run failing with the cacert error while the control fails with "Connection refused" proved the error happens before any request.

---

## Reproduction Process

**Environment Setup**

- macOS 26.3.1 (arm64)
- community.hashi_vault 7.2.0: my fork `landonkunz-coder/community.hashi_vault`, `main` at upstream commit `eb163dd`, cloned to `~/ai301-hashi-vault/ansible_collections/community/hashi_vault` (ansible-test and Ansible's collection loader need that `ansible_collections/<namespace>/<name>` path, per the collection's contributor guide)
- Python 3.12.14 in a venv at `~/ai301-hashi-vault/.venv` (created with `mise exec python@3.12 -- python -m venv .venv`), ansible-core 2.21.5, hvac 2.4.0 (`.venv/bin/pip install ansible-core hvac`)
- `ANSIBLE_COLLECTIONS_PATH=~/ai301-hashi-vault` so Ansible loads my clone, not an installed copy

Challenges and how I handled them:
- My system `python3` is 3.9.6, too old for current ansible-core, so I used Python 3.12 through mise, the same as my Path Review setup.
- I don't have Docker, and the collection's integration tests run Vault in docker-compose. Reading the code first showed that hvac raises this error before it sends any request, so I pointed the task at `https://127.0.0.1:8200`, where nothing is running. If the module had contacted Vault, I would have seen a connection error instead, so the empty port is part of the test, not a shortcut.
- My first fork-and-clone command didn't run (no fork, no folder), so I re-ran it and checked the fork and remotes before going on.
- Ansible 2.21 prints only the error message, not the exception class. I confirmed which exception it is by searching hvac's source: that message appears once, in `hvac/api/auth_methods/cert.py` line 293, inside `raise self.CertificateAuthError(...)`.

**Steps to Reproduce**

1. Make throwaway certs (the error doesn't depend on their contents):
   `openssl req -x509 -newkey rsa:2048 -nodes -days 1 -subj "/CN=repro-ca" -keyout certs/ca.key -out certs/ca.crt`, and the same with `/CN=repro-client` for `certs/client.key` and `certs/client.crt`.
2. Save the issue's task as `repro-456.yml`: `community.hashi_vault.vault_read` with `url: https://127.0.0.1:8200`, `path: secret/test`, `auth_method: cert`, `cacert: certs/ca.crt`, `cert_auth_private_key: certs/client.key`, `cert_auth_public_key: certs/client.crt`, and `validate_certs` from a variable (default `false`).
3. Run `ANSIBLE_COLLECTIONS_PATH=~/ai301-hashi-vault ansible-playbook repro-456.yml` (bug run, `validate_certs: false`).
4. Run it again with `-e validate=true` (control run, same task and files).
5. Observed result: the bug run fails with `cacert must be True, a file_path, or valid CA Certificate.` before contacting Vault. The control run gets past that check and fails only with `Connection refused` on `127.0.0.1:8200`.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:**

  Bug run (`validate_certs: false`):
  ```
  TASK [Issue #456 task (validate_certs: False)] *********************************
  [ERROR]: Task failed: Module failed: cacert must be True, a file_path, or valid CA Certificate.
  fatal: [localhost]: FAILED! => {"changed": false, "msg": "Task failed: Module failed: cacert must be True, a file_path, or valid CA Certificate."}
  ```

  Control run (`validate_certs: true`):
  ```
  TASK [Issue #456 task (validate_certs: true)] **********************************
  [ERROR]: Task failed: Module failed: HTTPSConnectionPool(host='127.0.0.1', port=8200): Max retries exceeded with url: /v1/auth/cert/login (Caused by NewConnectionError("HTTPSConnection(host='127.0.0.1', port=8200): Failed to establish a new connection: [Errno 61] Connection refused"))
  ```

  `-vvv` shows it ran my clone of main:
  ```
  ansible collection location = /Users/landonkunz/ai301-hashi-vault
  Using module file /Users/landonkunz/ai301-hashi-vault/ansible_collections/community/hashi_vault/plugins/modules/vault_read.py
  ```

- **My findings:** The bug from October 2024 (collection 6.2.0) still happens on 7.2.0. `validate_certs: false` is the trigger: with it on, the same task gets past the check. The failure is inside hvac's cert login, before any request, which is why I could reproduce it without a Vault server. I posted a short note with this on the issue and asked the maintainers which behavior they want before I plan a fix: [note link](https://github.com/ansible-collections/community.hashi_vault/issues/456#issuecomment-6408134392). The full report stays here and in my PR, per the instructions form the course.

**Toolkit**

- Tool: repro-check (my Unit 2 skill, with the staff's real-repo scope swapped in this unit), plus the voice guard checking both posts.
- What it produced: a claim-only grade (accept: `claim-specific` and `conventions-respected` pass, report checks not yet applicable) with two voice notes, then a claim plus repro grade (accept, 6 of 6). The guard passed the claim and the note before they posted.
- Difficulties: it told me to post the repro report on the thread next, which the course says not to do. `conventions-respected` passed because it found "no CONTRIBUTING.md", but it missed the collection's contributor guide in `docs/docsite/rst/` and Ansible's community AI policy, the same miss my scout made. It also flagged my hvac line number as unverified, which was fair until I checked it. My CodePath credits ran out on Oct 25, so both grades and the guard checks ran on my personal Claude account.
- What changed: I took both claim notes (named `CertificateAuthError`, changed "the report" to "the issue description"). I kept the report off the thread and posted only a short note with one question. I cut my code-reading paragraph from the repro, because my voice guide says not to post a root cause I haven't shown output for; it moves to Understanding the Issue. Next patch candidate for the evidence guide: look in `docs/` for a contributor guide and check org-level policies before passing conventions.

---

## Solution Approach

**Analysis**

With `validate_certs: false`, `_boolean_or_cacert()` in `_connection_options.py` sets the client's `verify` to `False` and ignores `ca_cert`. `HashiVaultAuthMethodCert.authenticate()` in `_auth_method_cert.py` then calls `client.auth.cert.login()` without `cacert`, and hvac's `login()` raises `CertificateAuthError` because it has neither a `cacert` nor a truthy `verify` (its own comment: "Must have cert checking or a CA cert"). That exception isn't a `HashiVaultValueError`, so modules like `vault_read` don't catch it and the user sees the raw library message.

**Proposed Solution**

Option B from my question on the issue: fail early with a clear message. Catch hvac's `CertificateAuthError` in `authenticate()` and raise the collection's own `HashiVaultValueError` explaining that cert auth needs certificate validation and how to fix it. Modules and lookups already handle `HashiVaultValueError`. Catching hvac's error also covers `VAULT_SKIP_VERIFY` and keeps me out of `_connection_options.py`, which open PR #504 also edits.

Not changing: passing `ca_cert` through to the login (option A, unless the maintainers ask for it), anything in `_connection_options.py`, the docs in `plugins/doc_fragments/auth.py`, the lookup tests that already fail on main, and the integration tests (they need Docker).

**Implementation Plan**

1. `plugins/module_utils/_auth_method_cert.py`: in `authenticate()`, add `except client.auth.cert.CertificateAuthError as e:` after the existing `NotImplementedError` clause and raise `HashiVaultValueError` with the clear message (reaching the exception through the client, so no new hvac import).
2. `tests/unit/plugins/module_utils/authentication/test_auth_cert.py`: add `test_auth_cert_authenticate_no_cert_verification`, which makes the mocked login raise hvac's real `CertificateAuthError` and expects `HashiVaultValueError` mentioning `validate_certs`.
3. `changelogs/fragments/456-cert-auth-validate-certs.yml`: a `bugfixes` entry.

Deviations:
- The base moved: upstream merged #507 (CI matrix) on 2026-10-27, so I rebased my commit onto `2a0b75c` (now `17fbf53`), moved my local `main` to `upstream/main`, and re-ran every check.
- My first "before" run of the new test failed for the wrong reason (`KeyError: 'mount_point'`), because `authenticate()` reads options with `get_filled_options()`; I set `role_id` and `mount_point` to `None` like the existing test, and it then failed for the right reason.
- The changelog fragment links the issue, since there was no PR number yet.
- I found the org-level PR template in `ansible-collections/.github` during the build and followed it.
- Otherwise as planned: three files, `_connection_options.py` untouched, and the maintainers still haven't said which behavior they prefer.

**Toolkit**

- Tool: plan-check (my unit 3 skill with the real-repo scope from unit 6), run on `plan.md` and my draft plan comment on the CodePath org on 2026-10-31.
- What it produced: accept, 7 of 7 (`diagnosis-grounded`, `scope-bounded`, `executable`, `test-decisive`, `honest-certainty`, `comment-faithful`, `thread-and-convention`), after spot-checking the hvac source, the error handling in `vault_read` and the open PRs.
- Difficulties: the maintainers never answered my A-or-B question, so I had to pick an approach myself and say so on the thread. plan-check also pointed out that my comment was Claude's draft word for word, which under Ansible's AI policy should be disclosed.
- What changed: before posting I added the commit and versions, pasted the real `fatal:` line (my earlier repro note was prose only), and added a line saying Claude helped draft the plan. The comment offers option A if the maintainers prefer it.

---

## Testing Strategy

Four observable checks, all run on my branch after rebasing onto `2a0b75c`, with full logs kept on my machine:

1. The issue's repro, before and after. Before (main): `"msg": "Task failed: Module failed: cacert must be True, a file_path, or valid CA Certificate."`. After (my branch): `"msg": "Authentication method cert requires TLS certificate validation, but it is turned off (validate_certs is false, or VAULT_SKIP_VERIFY is set), so ca_cert is not used. Set validate_certs to true, and set ca_cert if the CA that signed the Vault server's certificate is not trusted by the system. hvac said: cacert must be True, ..."`. The control run with `validate_certs: true` fails with `Connection refused` both before and after, so only the bug path changed.
2. The new unit test, `test_auth_cert_authenticate_no_cert_verification`: before the fix `1 failed, 14 passed`, with `hvac.api.auth_methods.cert.Cert.CertificateAuthError` escaping; after, `15 passed`.
3. The repo's own unit tests for the code I changed: `ansible-test units --venv --requirements --python 3.12 tests/unit/plugins/module_utils/` went from `1454 passed` on main to `1455 passed`.
4. The repo's sanity tests: `ansible-test sanity --venv --requirements --python 3.12` exit 0 (24 tests, including `changelog`, `pep8`, `pylint`, `import` and `validate-modules`; shellcheck skipped because it isn't installed).

Not run: the integration tests, which need Docker and a Vault server. The full unit suite has three failures in `tests/unit/plugins/lookup/test_hashi_vault.py::TestHashiVaultLookup::test_vault_addr_low_pref` on unmodified main as well as on my branch; my change doesn't touch the lookup.

**Toolkit**

- Tool: `ansible-test` (units and sanity, with `--venv` since I have no Docker), driven by the two must-pass checks in my unit 7 ship manifest, plus my repro playbook.
- What it produced: the before and after results above, saved verbatim in `test_evidence.md` for my pr-precheck to read.
- Difficulties: the full unit suite is red on main (three lookup tests), so it couldn't gate my change; integration tests need Docker; and the first "before" run of my new test failed for the wrong reason (`KeyError: 'mount_point'`), which a count of "1 failed" alone would have hidden.
- What changed: I scoped the must-pass row to `tests/unit/plugins/module_utils/` and recorded the full suite, integration tests and shellcheck as dated exclusions; I now check the `E` line of a failing test, not just the count; and I re-ran every check after the rebase.

---

## Implementation Notes

**2026-10-25**

Claimed #456 and reproduced it on main (7.2.0, `eb163dd`) without a Vault server: the issue's task pointed at an unused port fails with the cacert error, while the `validate_certs: true` control fails with "Connection refused". Posted a short note on the issue asking which behavior the maintainers want (use the `cacert`, or fail early with a clearer error).

**2026-10-27**

Installed the ship-gate by hand to keep my global Claude Code settings untouched. Found the gate crashed on macOS's Python 3.9 and would have let a PR through, and switched it to my venv's Python 3.12. Wrote my ship manifest, and found the full unit suite already fails on main (three lookup tests), so I gated on the module_utils tests and sanity instead. With no maintainer answer, chose option B (a clear error) and wrote the plan.

**2026-10-31**

plan-check accepted the plan 7 of 7. Added the commit, the real `fatal:` line and an AI disclosure to my plan comment and posted it. Created `fix/456-cert-auth-validate-certs-error`, added the test first (its first failure was a `KeyError` from a missing option, not the bug, so I fixed the test until it failed on hvac's `CertificateAuthError`), then the fix (15 passed) and the changelog fragment. Re-ran the repro and both manifest checks, and committed with `Fixes #456` and an `Assisted-by:` trailer.

**2026-11-01**

Upstream `main` had moved (#507), so I rebased onto `2a0b75c`, moved my local `main`, and re-ran everything. Found the org-level PR template and wrote `pr_draft.md` to it. The gate passed; I fixed a leftover template label its voice note flagged, ran it again (pass), pushed to my fork, set the default repo, and opened PR #510 through Claude Code, where the door honored the manual pass.

**2026-11-04**

Installed the PR monitor and wrote my playbook (14 quiet days before a first nudge, from the repo's recent PRs). `--state` called SonarCloud's bot comment "review feedback", so I added a rule that bots never get a reply; the full run then drafted a wait note, and I posted nothing.

**2026-11-08**

PR #510 still has no maintainer review (6 quiet days, under my threshold). Finished the contribution story.

**Toolkit**

- Tool: Claude in my Cowork session as a pair programmer: it wrote the test, the fix and the changelog fragment into my clone and drafted my working files; I read every diff, ran every command myself and made every commit.
- What it produced: one commit, `17fbf53` (3 files, +30 -1), plus `plan.md` with its Deviations, `test_evidence.md` and `pr_draft.md`.
- Difficulties: its first test failed for a reason unrelated to the bug; my local `main` went stale after upstream merged #507; and the link between Claude and my computer dropped more than once, so I had to paste terminal output back by hand.
- What changed: I check why a test fails before trusting the failure, I move my local `main` after every rebase, and my commit carries an `Assisted-by:` trailer as Ansible's AI policy suggests.

---

## Pull Request

**PR Link:** https://github.com/ansible-collections/community.hashi_vault/pull/510

**PR Description:** Opened 2026-11-01 as "cert auth - raise a clear error when certificate validation is off", from `landonkunz-coder:fix/456-cert-auth-validate-certs-error` to `main`: one commit (`17fbf53`, with `Fixes #456` and an `Assisted-by:` trailer), 3 files, +30 −1. The repo has no PR template of its own, but the org-level one in `ansible-collections/.github` applies and recent PRs use it, so the description follows it. It opens with one line saying this is my first contribution and that I used Claude to help plan the fix and draft the description, and that I read the change and ran every command myself. SUMMARY says it fixes #456, explains the cause (with `validate_certs: false` the connection options drop `ca_cert` and hvac refuses cert login without certificate checking), describes the change (catch hvac's `CertificateAuthError` in `HashiVaultAuthMethodCert.authenticate()` and raise `HashiVaultValueError` with a clear message), and offers to switch to using `ca_cert` instead if the maintainers prefer. ISSUE TYPE is Bugfix; COMPONENT NAME is the cert auth method. ADDITIONAL INFORMATION pastes the before and after output of the issue's task, the `validate_certs: true` control, the new unit test going from `1 failed, 14 passed` to `15 passed`, the two checks I ran with `ansible-test --venv` (`1455 passed`, sanity exit 0), and what I didn't run: integration tests (no Docker) and the three lookup tests that already fail on main.

**Maintainer Feedback:**

- 2026-11-08: none yet. The only comment is SonarCloud's bot ("Quality Gate passed", 0 new issues). No maintainer has reviewed the PR or replied to my plan comment on #456. It has been 6 quiet days, under my 14-day threshold, so per my playbook I haven't nudged; the PR monitor drafted a wait note on 2026-11-04.

**Status:** Awaiting review.

**Toolkit**

- Tool: the ship-gate, a hook on `gh pr create` that runs my manifest's checks and then my own pr-precheck on the branch. I ran it by hand from the clone (`~/ai301-hashi-vault/.venv/bin/python ~/.claude/ship-gate/check_gate.py --branch --title ... --body-file pr_draft.md`) until it passed, then opened the PR through Claude Code so the voice guard and the gate both checked it at the door.
- What it produced: PASS. Both manifest checks passed (module_utils units in 7s, sanity in 24s) and my pr-precheck accepted 5 of 5: `scope-faithful` (the diff is exactly the plan's In list), `description-true`, `tested` (the repro re-run shows the new message), `clean-diff` and `template-met`. Its one voice note was a leftover "paste below" label on a code fence from the template; I fixed it and the second run passed too. When I opened the PR through Claude Code, the gate log shows the door honoring that manual pass ("PASS (honored the manual pass at 2026-11-01 19:29)") and the voice guard passing the same command.
- Difficulties: install.py would have edited my global settings, so I installed by hand. On Python 3.9 the gate crashed silently and would have let a PR through. The full unit suite is red on main, so I had to scope the must-pass row and record an exclusion. After upstream merged #507, my local `main` was stale, so the gate would have compared my branch against the old base and counted #507's changes as mine. `template-met` passed while saying the repo "states no template", which is wrong: the org template applies (my description followed it anyway).
- What changed: the hook and manual doorway now run on my venv's Python 3.12; the manifest gates on the module_utils subset with three dated exclusions; I rebased onto `2a0b75c`, moved my local `main` to `upstream/main` and re-ran every check; I fixed the fence and re-ran the gate before shipping. Next I'd patch my pr-precheck evidence guide to check `<org>/.github` for an org-level PR template, the same gap my scout and repro-check had with the contributor guide.

---

## Learnings and Reflections

**Technical Skills Gained**

- Working inside a real Ansible collection: the `ansible_collections/<namespace>/<name>` clone layout, `ansible-test units` and `sanity` with `--venv` when Docker isn't available, and changelog fragments.
- Tracing a bug across a project and a library it depends on: from `validate_certs` in the connection options, through the cert auth method, into hvac's `login()`.
- Writing a test that fails for the right reason before the fix, using the repo's own fixtures and `mock.patch.object`.
- Git on a fork: `origin` vs `upstream`, rebasing onto a moved `main`, keeping working files out of commits with `.git/info/exclude`, and `gh repo set-default` so a PR opens on the real repo.
- Claude Code hooks and headless runs: a hook that exits with the wrong code fails open, not closed.

**Challenges Overcome**

- Finding an issue I could honestly take: beets, Qiskit and BookWyrm all ban AI-assisted work on good first issues, and other picks were already claimed. I overrode my scout's reject on #456 because the only failing check was a commit-age rule.
- Reproducing without Docker or a Vault server, by pointing the task at an unused port and using a `validate_certs: true` run as the control.
- The ship-gate silently not running on Python 3.9, which I only caught because the wiring test showed one stop instead of two.
- A full unit suite that was already red on main, handled with a scoped must-pass check and a dated exclusion instead of ignoring it.
- No maintainer answer to my design question, so I picked option B, said so on the thread, and left option A open.

**What I'd Do Differently Next Time**

- Read every issue thread and the repo's AI policy myself before grading anything, instead of after a scout run; that would have saved the beets grade and a wrong Slack post that went to GitHub by mistake.
- Write the code-reading answers myself first and only then compare with Claude's reading.
- Look for org-level files (`<org>/.github` templates and policies) as part of reading the rules, and patch my tools to do it, since my scout, repro-check and pr-precheck all missed them.
- Check upstream for new commits right before the gate run, not after.

---

## Resources Used

- community.hashi_vault contributor guide (testing commands, auth method layout, changelog fragments): https://github.com/ansible-collections/community.hashi_vault/blob/main/docs/docsite/rst/contributor_guide.rst
- Ansible Community Policy for AI-Assisted Contributions (disclosure and the `Assisted-by:` trailer): https://docs.ansible.com/projects/ansible/13/community/ai_policy.html
- The repo's CI workflow, where I mined my manifest's checks: https://github.com/ansible-collections/community.hashi_vault/blob/main/.github/workflows/ansible-test.yml
- The org-level PR template my description follows: https://github.com/ansible-collections/.github/blob/main/PULL_REQUEST_TEMPLATE.md
- hvac's cert auth source, where the error is raised (`login()`): https://github.com/hvac/hvac/blob/main/hvac/api/auth_methods/cert.py
- The AI301 Issue Selection site, where my search started: https://open-source-issue-tracker.onrender.com/
- The AI301 unit 6, 7 and 8 materials (voice guard, ship-gate, PR monitor): https://github.com/codepath/ai301-unit6-starter, https://github.com/codepath/ai301-unit7-starter, https://github.com/codepath/ai301-unit8-starter
