# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

Live-mode output of `claude "issue-select: grade these candidate first issues: <#72> <#68> <#62>"`, run 2026-09-20 against the installed skill at `~/.claude/skills/issue-select/`. Three candidates were graded; #72 is the one I chose (ranked first, verdict `accept`).

````
All three are in the scoped repo (`codepath/pathreview-ai301-fa26-s3`). Reference date: 2026-09-20.

**Shared repo facts:** not archived; last 5 `main` commits are 2026-09-16 ×3 and 2026-08-24 ×2, all by the human `Aburke225`; no published release, last push 4 days ago; `docs/CONTRIBUTING.md` + PR template are silent on AI (no ban); zero PRs exist in the repo, so no abandoned attempts anywhere. `responds-to-issues` grades **unclear** for all three — every issue in the tracker was opened by a maintainer, so there's no non-maintainer sample (preferred only, no verdict effect).

### Accepted, in fit order

**1. #72 — `verify_password` raises `UnknownHashError`** *(all required pass, 3/4 preferred)*
Best fit: a two-file Python fix (`core/security.py` + `tests/unit/test_security.py`) with the covering test already written and marked `xfail` — you verify by deleting the marker and running `make test-unit`, no services required. Effort stated as 1–2 hours.
One tension worth naming: a classmate (`rafiatasafi`) pushed a commit on a fork referencing this issue today. The rubric's claim definition covers assignees, open linked PRs, and claim comments — a fork commit is none of those, and the Path Review house rule says classmate claims don't block anyway. It passes, but you'd be working alongside someone.

**2. #68 — Keyword search `ZeroDivisionError` on empty index** *(all required pass, 3/4 preferred)*
Same shape — `rag/retriever/keyword_search.py` plus an existing `xfail` test — but 2–4 hours, and `acordero4852` posted a claim comment on 2026-09-19. Per the house rule that's not a blocker, just company.

**3. #62 — Health check references `settings.redis_host`** *(all required pass, 3/4 preferred)*
Smallest diff (`redis_host`/`redis_port` → `redis_url`), but it ships no covering test and reproducing it wants Redis and the API actually running — the environment setup your fit profile says to rank lower.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "commits-alive", "grade": "pass", "evidence": "3 commits dated 2026-09-16 and 2 dated 2026-08-24, all authored by non-bot Aburke225"},
      {"name": "repo-not-archived", "grade": "pass", "evidence": "gh repo view: \"isArchived\": false"},
      {"name": "repo-shipping", "grade": "pass", "evidence": "latestRelease: null and last push 2026-09-16T21:50Z (4 days)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], zero PRs in repo, zero comments; only event is a fork commit reference, not a claim comment or open PR"},
      {"name": "single-bounded-change", "grade": "pass", "evidence": "one deliverable: return False on UnknownHashError in core/security.py and drop the xfail in tests/unit/test_security.py"},
      {"name": "spec-settled", "grade": "pass", "evidence": "bug report (\"should fail closed (return False), not raise\"), opener association COLLABORATOR, names both files"},
      {"name": "no-abandoned-attempts", "grade": "pass", "evidence": "repo has zero pull requests, so zero closed-unmerged attempts"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no mention of AI; no AI_POLICY.md"},
      {"name": "responds-to-issues", "grade": "unclear", "evidence": "every issue in the tracker was opened by COLLABORATOR Aburke225; no non-maintainer sample exists"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "author_association: COLLABORATOR (Aburke225)"},
      {"name": "friendly-label", "grade": "pass", "evidence": "labels: bug, good first issue, api, tier-1"},
      {"name": "fresh-issue", "grade": "pass", "evidence": "created_at 2026-09-10 (10 days before reference date)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "commits-alive", "grade": "pass", "evidence": "3 commits dated 2026-09-16 and 2 dated 2026-08-24, all authored by non-bot Aburke225"},
      {"name": "repo-not-archived", "grade": "pass", "evidence": "gh repo view: \"isArchived\": false"},
      {"name": "repo-shipping", "grade": "pass", "evidence": "latestRelease: null and last push 2026-09-16T21:50Z (4 days)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no linked/mentioned PRs; sole claim comment (acordero4852, NONE, 2026-09-19) is a classmate claim the Path Review house rule says does not block"},
      {"name": "single-bounded-change", "grade": "pass", "evidence": "one deliverable: make index([]) not raise in rag/retriever/keyword_search.py and drop the xfail"},
      {"name": "spec-settled", "grade": "pass", "evidence": "bug report (\"index([]) raises ZeroDivisionError ... shouldn't raise on an empty corpus\"), opener COLLABORATOR, names both files"},
      {"name": "no-abandoned-attempts", "grade": "pass", "evidence": "repo has zero pull requests, so zero closed-unmerged attempts"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no mention of AI; no AI_POLICY.md"},
      {"name": "responds-to-issues", "grade": "unclear", "evidence": "every issue in the tracker was opened by COLLABORATOR Aburke225; no non-maintainer sample exists"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "author_association: COLLABORATOR (Aburke225)"},
      {"name": "friendly-label", "grade": "pass", "evidence": "labels: bug, good first issue, rag, tier-1"},
      {"name": "fresh-issue", "grade": "pass", "evidence": "created_at 2026-09-10 (10 days before reference date)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "commits-alive", "grade": "pass", "evidence": "3 commits dated 2026-09-16 and 2 dated 2026-08-24, all authored by non-bot Aburke225"},
      {"name": "repo-not-archived", "grade": "pass", "evidence": "gh repo view: \"isArchived\": false"},
      {"name": "repo-shipping", "grade": "pass", "evidence": "latestRelease: null and last push 2026-09-16T21:50Z (4 days)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], zero comments, zero PRs in the repo, no assign/link timeline events"},
      {"name": "single-bounded-change", "grade": "pass", "evidence": "one deliverable: point the Redis probe in api/routes/health.py at settings.redis_url"},
      {"name": "spec-settled", "grade": "pass", "evidence": "bug report with repro (\"call GET /health with Redis running and observe a 503 ... AttributeError for redis_host\"), opener COLLABORATOR"},
      {"name": "no-abandoned-attempts", "grade": "pass", "evidence": "repo has zero pull requests, so zero closed-unmerged attempts"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no mention of AI; no AI_POLICY.md"},
      {"name": "responds-to-issues", "grade": "unclear", "evidence": "every issue in the tracker was opened by COLLABORATOR Aburke225; no non-maintainer sample exists"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "author_association: COLLABORATOR (Aburke225)"},
      {"name": "friendly-label", "grade": "pass", "evidence": "labels: bug, good first issue, api, tier-1"},
      {"name": "fresh-issue", "grade": "pass", "evidence": "created_at 2026-09-10 (10 days before reference date)"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke run, `--limit 3`: `agreement: 2/3 scored items`. issue-01 was graded `reject` against a gold `accept`; the note column read `failed: single-bounded-change, spec-settled, responds-to-issues (preferred), maintainer-filed (preferred)`.
2. After tightening `single-bounded-change` and `spec-settled` (see Issue analysis), re-grade with `--only issue-01`: `agreement: 1/1 scored items`.
3. First full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.
4. Confirming full run with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, same category tallies, `run written to eval-run.txt`. This is the run committed as `eval-run.txt`.

**Issue analysis**

`issue-01` (conda/conda#16475, category `clear-accept`). Gold label: `accept`. My rubric's first decision (smoke run): `reject`, failing two required checks. My rubric's final decision: `accept`.

The issue is a docs task opened by a `CONTRIBUTOR` with the `type::documentation` label and no comments. The body asks for a new task page plus updates to `manage-pkgs.rst`, `pip-interoperability.rst`, and `new-features.md`, and adds "Consider a global `troubleshooting.rst` entry — Lower priority, but worth naming."

Why the first version rejected it: my `single-bounded-change` check said to fail "a checklist of items each intended to become its own separate PR," and the grader read five files with one optional item as that kind of list. My `spec-settled` check had a fail trigger for "a change request from a non-maintainer with no maintainer comment approving it," which fired because the opener is a `CONTRIBUTOR`, not a maintainer, and the thread is empty. Both triggers were aimed at other issues in the set (the tldr-pages "megaissue" and the excalidraw feature request with `labels: none`) and were written too broadly.

Why the final version accepts it: `single-bounded-change` now passes "a body that lists several files, pages, or steps ... when they all serve one stated outcome and nothing says to split them," and says "an item marked optional or lower priority does not make the issue an umbrella." `spec-settled` now treats a label as triage: the fail trigger applies only to "a change request that carries no labels at all and has no maintainer comment," and "A label applied to the issue counts as maintainer triage." issue-01 carries `type::documentation`, so the trigger no longer fires, and the body names the exact files to change, satisfying pass condition (c).

**Check rationale**

From `tools/issue-select/rubric.md`, the `spec-settled` row, quoted as currently written:

> Pass if at least one of these holds: (a) it is a bug report as defined above; (b) the opener is a maintainer; (c) the body names the specific files, functions, pages, or UI locations to change, or lists acceptance criteria; (d) a maintainer comment states the desired behavior or tells a contributor to go ahead. Fail if (a)-(d) all fail. Also fail, regardless of (a)-(d), when: the body says a required input to the work is undecided ("TBD", "asset TBD", "needs a decision", "open question") or a maintainer comment says the project has not decided whether it wants this; or a maintainer asked what the behavior should be and no later maintainer comment answers it; or the issue is a change request that carries no labels at all and has no maintainer comment (nobody with authority has triaged it). A label applied to the issue counts as maintainer triage.

Reasoning: the four `scope` issues in the eval set fail for different reasons, and this check is the one that has to separate "small but unsettled" from "small and ready." Pass conditions (a)-(d) are four independent ways an issue can show that the work is defined: a bug report carries its own spec (observed vs. expected), a maintainer opener is the spec's author, named files are a spec, and a maintainer go-ahead is a spec by proxy. The fail triggers override those when the thread shows the design is still open. The last trigger, about unlabeled change requests, exists for excalidraw#11811 (issue-20): a one-line feature wish from a bot account with `labels: none` and no comments, where the pass conditions could otherwise be argued ("Likely surface: `packages/excalidraw`" reads like a file name). Treating a maintainer-applied label as triage is what lets the same trigger leave conda#16475 alone: that issue was labeled `type::documentation` by the project, which is a maintainer saying "this belongs here," even though no maintainer commented.

**Trade-offs**

The label-as-triage clause is the trade. Before it, `spec-settled` rejected issue-01 (gold `accept`); after it, issue-01 passes and issue-20 still fails, confirmed by `--only issue-01` (1/1) and then two full runs (20/20 each). What the check now gives up: a project that auto-applies labels from an issue template (many do, e.g. a `bug` or `enhancement` label added by the form itself) will pass this trigger without any human having looked at the issue. In that case a non-maintainer feature request with no maintainer comment sails through `spec-settled` on the label alone, and the only thing standing between it and an accept is whether (a)-(d) hold and whether the body says "TBD." None of the 20 eval issues is shaped that way, so nothing else in the run changed, but in live mode on a repo with template-applied labels I would expect this check to be too generous, and the fix would be to require the label to be one a human chooses (`good first issue`, `help wanted`) rather than any label.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit and time.** #72 is the smallest of the three the tool accepted: one function in `core/security.py`, with the test already written and marked `xfail`, and staff estimate 1–2 hours. That fits the time I have this week, and it's Python with no Redis or API server to run, which is what my fit profile asked for. It also keeps my Claude Code spend low, since I won't need many runs to reproduce and verify it.

2. **What the verdict got right, and what I weighed that the rubric could not.** The verdict was right that the repo is active (commits four days ago), that the issue is staff-filed with both files named and a clear expected behavior (return `False` instead of raising), and that nobody is assigned or has a PR open. What I weighed that the rubric couldn't: I've never used passlib, so I'm picking this partly *because* the test is already written; it tells me exactly what "fixed" looks like. And the rubric doesn't count another student's fork commit as a claim, which is correct under the house rule, but I still had to decide I'm okay working on the same bug as a classmate. I am, since my credit comes from my PR, not from being first.

3. **Anticipated difficulty in claiming.** Low. The house rule means I can claim even though someone else is already on it. The only thing I expect to be tricky is writing a claim comment that's specific about the fix rather than just "I'll take this."

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
