# Evidence guide: where evidence lives in a plan package

For each family: where to look in an eval bundle, where to look in
live mode, what good looks like, and what is NOT required. The "not
required" lines matter: a check must not fail a plan for missing
something this guide does not ask for.

Live mode sources, used below:

- The draft plan: `plan.md` in the fork's top folder (sections:
  Diagnosis, Scope, Files, Approach, Test plan, Risks and unknowns,
  Deviations).
- The draft comment: `comment.md` in the same folder.
- The issue and thread: `gh issue view <number> --repo <repo>
  --comments`, with each commenter's role from the issue page.
- The repro evidence: the student's posted repro comment on the same
  issue (their week-2 report). Only what the drafts quote or what that
  comment shows counts.
- Repo conventions: `docs/CONTRIBUTING.md` in the repo, plus the
  house rules in `scope.md`.

## Diagnosis and grounding

**Where it lives.** Bundle: the plan's Diagnosis or Cause line, read
against the Repro evidence block (numbered steps, any Control line,
timings, Expected, Actual). If the plan credits the thread, the
matching line in Thread highlights. Live: the Diagnosis section of
`plan.md`, read against the posted repro comment.

**What good looks like.** The stated cause explains every repro step,
including the control. In calib-01 the cause ("that view's model is
not refreshed") matches step 4: re-entering the view rebuilds it and
the color fixes itself. The planned change acts on that cause.

**What bad looks like.** A repro step rules the cause out. In
calib-03 the plan blames the pager's key bindings, but step 3 runs
with no pager at all and is still slow, and step 2 uses the same
pager with color off and is fast. A cause repeated from a NONE
commenter's thread claim, with no repro step behind it, is not
grounded.

**Not required.** A code-level root cause with line numbers. A plain
statement of which component misbehaves, backed by the steps, is
enough.

## Scope

**Where it lives.** Bundle: the plan's Scope section or its In and
Out lines, and its list of Changes or Approach steps. Live: the Scope
and Files sections of `plan.md`.

**What good looks like.** Every change is needed to fix the reported
bug. calib-01: one callback in one file, with an Out line naming what
it will not touch. A plan that fixes part of the issue and says which
part it defers is bounded.

**What bad looks like.** "While I'm here" work: renames, refactors,
migrations, a docs pass, new flags or features, or edits to areas the
bug does not reach.

**Not required.** A separate not-in line, if the changes themselves
are clearly bounded. Fixing the whole issue at once.

## Executability

**Where it lives.** Bundle: the plan's Change, Changes, or Approach
lines and any files named. Live: the Files and Approach sections of
`plan.md`.

**What good looks like.** Each change names a code site (a file,
function, or callback) and what it does there, so a stranger could
open the file and start. calib-01: "the push completion callback in
`pkg/gui/controllers/sync_controller.go` adds the commits context to
its post-push refresh scope."

**What bad looks like.** "Poke around the editor components", "figure
out where the undo history lives", "make it work" (calib-02).

**Not required.** Exact line numbers, a diff, or a step order beyond
what the change needs.

## Test plan

**Where it lives.** Bundle: the plan's Test or Test plan line, read
against the Repro evidence steps and Expected line. Live: the Test
plan section of `plan.md`, read against the posted repro comment's
steps and output.

**What good looks like.** The same behavior the repro shows, observed
again, with the after-fix result stated. calib-01: "repro steps
above; at step 3 the color must flip without leaving the view."

**What bad looks like.** "Run the full test suite and make sure
nothing regresses" (calib-04), "undo works after toggling" with no
steps (calib-02), or a test that only checks code is registered and
never observes the bug's behavior (calib-03's binding smoke test).

**Not required.** A new automated test. A manual re-run of the repro
steps with the expected result stated is decisive.

## Honesty

**Where it lives.** Bundle: the plan's claims about cause, difficulty,
and outcome, and any Risks or Unknowns lines, read against the Repro
evidence and Thread highlights. Live: the Diagnosis, Risks and
unknowns, and Deviations sections of `plan.md`. A mid-build change of
plan is recorded under Deviations and re-graded.

**What good looks like.** Claims match what the evidence shows.
Things the evidence does not settle are stated as open.

**What bad looks like.** An unverified thread claim presented as the
confirmed cause; "should be easy" when a maintainer said the fix is
hard, with no account of how the plan avoids the hard part; an
outcome promised that no step supports.

**Not required.** A Risks section. A plan with nothing uncertain does
not need to invent risks.

## Comms

**Where it lives.** Bundle: the Candidate plan comment, read against
the Candidate plan, the Thread highlights (with each author's role
tag), and the Repo facts block (contribution policy, AI policy,
bug-report template). Live: `comment.md`, read against `plan.md`,
the live thread, `docs/CONTRIBUTING.md`, and the house rules in
`scope.md`.

**What good looks like.** The comment states the plan's cause,
change, and test, and promises nothing more. Where an OWNER, MEMBER,
or COLLABORATOR gave direction, the comment engages it. Where the
repo facts state a rule, the comment and plan follow it. calib-01
names its one change and says it is "keeping it minimal given the
review-bandwidth note in CONTRIBUTING."

**What bad looks like.** Enthusiasm with no plan ("I'm going to fix
it! Wish me luck!", calib-02). A comment that promises more than the
plan shows. A comment that ignores a maintainer's "working as
intended" or "discuss first", or breaks a stated AI or contribution
rule.

**Not required.** AI disclosure when the repo facts do not ask for
it. Engaging the thread when it has no maintainer direction. Voice
rules (no timelines, no em dashes): those are the voice guide's job
in live mode, not a rubric fail.
