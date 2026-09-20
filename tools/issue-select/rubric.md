# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## How to read dates and roles

- **Reference date.** Every "within N days" below is measured against the
  capture date stamped at the top of the bundle (eval mode) or today
  (live mode). Never against the issue's open date.
- **Maintainer** means a commenter or opener whose association is
  `OWNER`, `MEMBER`, or `COLLABORATOR`. `CONTRIBUTOR` and `NONE` are not
  maintainers. A `[bot]` account is never a maintainer, even when its
  association says `MEMBER`.
- **Claim comment** means a comment by a non-maintainer that says they
  are taking or working on the issue: "I'll take this", "can I work on
  this", "working on this", "I've started on this", `@<bot> claim`, or a
  link to a PR they opened for it.
- **Bug report** means the body describes behavior that happens and
  behavior that should happen instead (or includes a stack trace / error
  output). Anything else (new capability, enhancement, docs addition) is
  a **change request**. A docs correction (fixing a wrong or dead
  reference) counts as a bug report.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `commits-alive` | The "last 5 default-branch commits" list under Repo facts: each commit's date and author. | At least 2 of the 5 listed commits are dated within 90 days of the reference date, and at least one of those is authored by a non-bot account or is a bot merge of a human's pull request. Fail if fewer than 2 qualify. Unclear only if the list is missing. | required |
| `repo-not-archived` | The `archived:` flag on the repo line under Repo facts (live: the "This repository has been archived" banner). | `archived: no`. Fail on `archived: yes`. | required |
| `repo-shipping` | The "latest release" and "last push to any branch" lines under Repo facts. | Either (a) the latest release is dated within 365 days of the reference date, or (b) the repo has no published release and the last push to any branch is within 90 days of the reference date. Fail if neither holds (a release older than 365 days is a fail even if there was a recent push). | required |
| `unclaimed` | The "this issue: assignees / linked PRs" line under Repo facts, plus every comment in the Comments section (author, association, date, text). | All of the following hold: (1) `assignees: none`; (2) no linked PR is in state `open`; (3) no comment links to or mentions a PR for this issue that is still open; (4) no claim comment dated within 90 days of the reference date. Closed/unmerged linked PRs are not claims. Claim comments older than 90 days are stale and do not count. Bot comments (including bot-driven assign/unassign notices) are not claims. Fail if any of (1)-(4) fails. | required |
| `single-bounded-change` | The issue title, labels, and body. | The body asks for one deliverable that one pull request could close. Fail only if one of these is present: (1) the title or body calls itself a tracking, umbrella, meta, mega, or epic issue; (2) the body says its items are to be split into separate PRs or claimed by different people; (3) the body asks a usage/support question ("how do I ...") rather than requesting a change; (4) a maintainer comment says the fix touches core internals, needs a refactor first, or is blocked on another issue. Otherwise pass. A body that lists several files, pages, or steps passes when they all serve one stated outcome and nothing says to split them; an item marked optional or lower priority does not make the issue an umbrella. | required |
| `spec-settled` | The issue body, the labels on the header line, and the full comment thread, including author associations. | Pass if at least one of these holds: (a) it is a bug report as defined above; (b) the opener is a maintainer; (c) the body names the specific files, functions, pages, or UI locations to change, or lists acceptance criteria; (d) a maintainer comment states the desired behavior or tells a contributor to go ahead. Fail if (a)-(d) all fail. Also fail, regardless of (a)-(d), when: the body says a required input to the work is undecided ("TBD", "asset TBD", "needs a decision", "open question") or a maintainer comment says the project has not decided whether it wants this; or a maintainer asked what the behavior should be and no later maintainer comment answers it; or the issue is a change request that carries no labels at all and has no maintainer comment (nobody with authority has triaged it). A label applied to the issue counts as maintainer triage. | required |
| `no-abandoned-attempts` | The "linked PRs" state list under Repo facts, plus any PRs mentioned in comments. | Fewer than 2 linked or mentioned PRs for this issue are `closed` without being merged. Two or more closed-unmerged attempts is a fail (the issue is harder than it looks). | required |
| `ai-policy-allows` | The "contribution policy" line under Repo facts (live: `CONTRIBUTING.md`, `.github/`, `AI_POLICY.md`, `AI_USAGE_POLICY.md`, and any contributor docs they link to). | Pass if the policy is absent, silent on AI, or sets conditions only (disclose AI use, understand and test your changes, human review required). Fail if the policy says AI-generated code or documentation is not accepted, banned, prohibited, or will be closed on sight. Conditions are not bans. | required |
| `responds-to-issues` | The "maintainer first-response sample" list under Repo facts. | Among sampled issues that were not opened by a maintainer, at least one received a maintainer comment within 30 days of being opened. Unclear if every sampled issue was opened by a maintainer. | preferred |
| `maintainer-filed` | The opener's association on the issue header line. | Opener is `OWNER`, `MEMBER`, or `COLLABORATOR`. | preferred |
| `friendly-label` | The labels list on the issue header line. | Carries one of: `good first issue`, `good-first-issue`, `help wanted`, `easy`, `beginner`, `starter`, `documentation`. | preferred |
| `fresh-issue` | The issue's open date on the header line. | Opened within 365 days of the reference date. | preferred |

## Verdict rule

Accept if and only if every `required` check grades `pass`. Any `fail`
on a required check rejects the issue. `unclear` on a required check
counts as `fail`: an issue whose liveness, claim state, scope, or policy
cannot be verified from the evidence is not a safe first issue.
`preferred` checks never change the verdict; among accepted issues, rank
by the number of preferred checks passed (more is better), then by the
fit profile in `scope.md`.
