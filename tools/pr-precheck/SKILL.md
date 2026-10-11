---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this ready to submit?

A PR package is a candidate pull request: its title, its description
(including any test evidence the description carries), its commits,
its diff, and its test evidence. You read it against two things: the
plan it claims to implement (including that plan's deviation notes)
and the issue that plan belongs to, along with the repo's stated
standards for pull requests.

Do not answer any other question. Do not judge whether the fix is the
best possible fix, whether the issue was worth taking, or whether the
plan should have been different: the plan was accepted before the
build. Your job is whether this PR faithfully, verifiably, and
reviewably delivers that plan under the repo's stated rules. Grade
exactly one package per run. Do not answer from gut feel: execute
`procedure.md`, which applies `rubric.md` to evidence gathered per
`references/evidence-guide.md`.

## Inputs and modes

Determine the mode first. If you were given a package bundle (a
markdown file from `eval/packages/`), you are in eval mode. If you
were asked to grade a student's own branch and drafts, you are in
live mode.

**Live mode.** Run from the top folder of the student's fork clone,
on their branch. The inputs are the files the student's command names.
By default these are:

- The plan: `plan.md`, including everything under `## Deviations`.
  A deviation recorded there is part of the plan and is graded as such.
- The diff: every committed change the branch makes relative to
  `main`. Produce it with `git --no-pager diff main...HEAD` (three
  dots), plus `git --no-pager diff main...HEAD --stat` for the file
  list and `git --no-pager log main..HEAD --oneline` for the commits.
  Only committed changes are the diff. If `git status` shows
  uncommitted or untracked files other than `plan.md`, `comment.md`,
  `pr_draft.md`, and `test_evidence.md`, report them in the summary as
  not part of the graded diff; do not grade them.
- The title and description: `pr_draft.md`. The first line is the
  title; everything after it is the description.
- The test evidence: `test_evidence.md`. The description's Testing
  section is graded as what a reviewer will see; `test_evidence.md` is
  where you confirm the output it quotes is real.
- The issue: the URL the student gives. Read its thread with
  `gh issue view <number> --repo <scoped repo> --comments`.
- The repo's stated standards: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` as they exist on `main` (read them with
  `git --no-pager show main:<path>`).

A house-chain student grades the house issue, the house plan, and the
house repro pack in place of their own; read the house branch's diff
the same way. The same checks grade the same things.

Read the package the way a maintainer will: the package is what the
title, description, diff, and evidence contain, not other files in
the working directory. If a named input is missing, do not invent it;
the checks that need it grade on its absence.

**Eval mode.** The bundle is the whole world. Every fact comes from
the bundle text: its repo facts, issue, thread highlights, plan
context, and candidate PR (title, description, commits, diff, test
evidence). Fetch nothing, run nothing, and read no other file. Always
grade the complete package: every check in the rubric, then the full
verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. Apply the house rules it lists when you gather and read
evidence. Refuse to grade if the issue URL is not in the repo on the
scope's `Repo:` line, and say why. If the `Repo:` line still carries
an unfilled placeholder (anything in angle brackets), stop without
grading and tell the student to fill the `Repo:` line in `scope.md`
with their section's Path Review repo. Never guess a scope.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` in this skill
directory and hold the outgoing PR text (the title and the
description in `pr_draft.md`) against each rule. For every rule the
draft breaks, quote the rule and the offending text in the summary,
under a "Voice" heading. The voice guide is the student's personal
style and carries no gold labels, so it never changes a check grade
or the verdict unless a rubric check reads it.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md`. It defines the checks (each with the evidence to
gather, a pass condition, and a weight of `required` or `preferred`)
and the verdict rule that turns check grades into a verdict.

Read `references/evidence-guide.md`. It maps where each evidence
family lives in an eval bundle and in a live package, and what good
looks like there.

Execute `procedure.md` as written: its read order, its evidence
gathering, its check execution, and its verdict assembly, in that
order, exactly. Where the procedure is silent on something you need,
do not invent a step: note the gap in the summary under a "Procedure
gaps" heading and continue with what the procedure does say.

If `rubric.md` has no checks of the student's own, or `procedure.md`
has no steps of the student's own (instruction comments do not count),
stop without grading and say which file is empty. Do not invent checks
or steps to fill in for it.

## Verdict and output

The verdict is binary: `accept` means ready to submit; `reject` means
hold. There is no third verdict. Reservations go in a check's evidence
line, not in the verdict.

Before the JSON block, print a short readable summary: one line per
check (name, grade, the deciding fact), then any "Voice" and
"Procedure gaps" notes in live mode. Each evidence line quotes or
names the specific part of the package that decided it (a diff line,
a file path, a description sentence, a command and its output).

End your reply with this fenced JSON block, valid, with every check in
the rubric listed, and nothing after it. In live mode `item` is the
issue URL; in eval mode it is the bundle id.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish. A terse, complete PR can be ready,
  and a long, confident one can hide drift. Read the diff, the
  description, and the evidence against the plan, the issue, and the
  repo's stated standards, never against formatting or tone.
- The rubric decides, not you. If a check passes by its stated
  condition but feels wrong, it passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you. Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. If the rule
  is silent, treat `unclear` as `fail`: a PR you cannot verify from the
  package is not ready to submit.
