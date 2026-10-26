# Contribution: [Issue Title]

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Landon Kunz
**GitHub Username:** landonkunz-coder
**Issue:** [GitHub issue link]
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

[1-2 paragraphs in your own words: how you searched, what your scan
surfaced, what your scout's verdict said, why this issue fits your
profile, and what you hope to learn. If your scout's verdict on it
was reject and you chose it anyway, say so here and say why; choosing
against your own tool silently is what loses points. Close with an
honest read of the tool itself: where the scout fell short on a real
wild search and what you changed or would change, or what convinced
you it held up. **Record the link to your claim comment in this section too.**
Not claimed yet? Write `claim pending` and today's date where the link goes,
and add the link the moment your claim lands.]

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  and how you used it
- What it produced: [the scan and its verdict on this issue, described]
- Difficulties: [where it fell short, or "none met" if none did]
- What changed: [what you patched or would update, or "nothing"]

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
