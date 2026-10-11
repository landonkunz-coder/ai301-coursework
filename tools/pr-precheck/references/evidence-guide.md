# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Checks: scope-faithful, description-true.

**Where it lives.**

- Eval bundle: the plan is the "Plan context" block. Its scope is the
  sentence starting "Scope", its boundary is the "Not in scope" line,
  and the files it will change follow "Files:". Any deviation note is
  in the plan context or the candidate PR's description. What changed
  is the "Diff" block: each `+++ b/<path>` header is a changed file.
  The fidelity claims are in the candidate PR's "Title" and
  "Description", including its checklist.
- Live: the plan is `plan.md` in the top folder of the fork clone,
  including everything under `## Deviations`. What changed is
  `git --no-pager diff main...HEAD` (file list with `--stat`). The
  claims are in `pr_draft.md` (title on line 1, then Summary, Changes,
  and Testing).

**What good looks like.** Every `+++ b/` path is named on the plan's
"Files:" line, sits in a folder the plan names, or is covered by a
deviation note; a planned "changelog entry" names the changelog file.
Every in-scope change the plan names shows up as a hunk, or the
description or deviation notes say it was left out. Every claim in the
description about what was changed, added, or tested points to
something in the diff, and "implements the plan exactly" is only
written when the first two sentences hold.

**Silent drift in each direction.** More: a file or hunk outside the
plan with no note anywhere (for example, a refactor of a file the plan
never names). Less: a planned change or test that is not in the diff
and not mentioned. Either way, a description that claims fidelity the
diff contradicts is silent drift.

**Not required:** that the plan was the best approach (plan-check
already graded that), that the diff touches every file the plan's
folder contains, or that deviations be avoided. A recorded deviation
is honest work.

## Test evidence (harness category: not-tested)

Check: tested.

**Where it lives.**

- Eval bundle: the "Test evidence" block under the candidate PR, plus
  any testing text in the description. Read it against the plan's
  "Test plan" in the plan context and the repro evidence it cites.
- Live: the Testing section of `pr_draft.md`, confirmed against
  `test_evidence.md` next to `plan.md`. Read it against the test plan
  in `plan.md`. Path Review's PR template names four checks:
  `make test-unit`, `make test-integration`, `make lint`, and
  `make typecheck`.

**What good looks like.** The issue's own reproduction (its steps or
input, not just a control) is re-run on the change, and the PR shows
the command or steps with a concrete observed after result that shows
the fixed behavior the plan or issue expects. Concrete means a reader
can check it against the expected behavior: printed output (`1
passed`, `parsed OK`), or a stated value, exit code, message, or
on-screen behavior, even when it is written as a comment after the
command (`# after: 'foo ' not listed`, `-- after: { code = 0 }`). For
an interactive repro, the steps taken and what appeared count. Every
other check the plan or the repo's template names is reported with its
outcome; pasted output is best, and a stated result is enough. A check
that failed or could not run is still evidence when its reason is
shown (for example, `make test-integration` stopping with "no tests
ran" because there are no integration tests yet, or a local typecheck
failure with the cause stated).

**Next to "tests pass."** Not tested looks like: no after run of the
repro at all; only a control or a different input re-run; or a vague
result with nothing a reader can check ("colors work now", "fixed",
"tests pass"). A test the plan names that the PR never mentions is not
tested either.

**Not required:** output pasted verbatim when a concrete result is
stated, printed output for checks other than the reproduction, tests
beyond what the plan names, a fresh before run when the before is
quoted from the posted repro, or CI results (CI runs only after the PR
opens).

## Diff quality (harness category: unreviewable)

Check: clean-diff.

**Where it lives.**

- Eval bundle: the "Diff" block (every hunk under each file header)
  and the "Commits" list.
- Live: `git --no-pager diff main...HEAD` and
  `git --no-pager log main..HEAD --oneline`.

**What good looks like.** Every hunk is the fix, its tests, its docs or
changelog, an edit the repo's own rules require for this fix (in Path
Review, deleting the issue's `xfail` marker and any matching
`pyproject.toml` suppression), or a needed change the plan or
description records. A reviewer can find the fix in one read.

**Debris tells.** Added `print`, `console.log`, or debug logging;
commented-out code; dead code; lines reformatted or reflowed that the
fix does not otherwise change; a rename, refactor, or cleanup the
issue does not need. An unrelated change is debris even when the
description mentions it.

**Not required:** a particular number of commits, or squashed commits.

## Standards and comms (harness category: standards-wall)

Check: template-met.

**Where it lives.**

- Eval bundle: the "Repo facts" block. The "pull requests" line states
  the template's sections and checklist; the "contribution policy" line
  states any AI or disclosure rule. Where the PR honors them is the
  candidate PR's "Description".
- Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`
  as they exist on `main` (`git --no-pager show main:<path>`). Where
  the PR honors them is `pr_draft.md`.

**What good looks like.** Every section or checklist item the template
names is present with content specific to this change ("N/A" with a
reason counts; an unticked box that is honestly not done counts). The
issue is referenced where the template asks (Path Review: `Closes #N`
under Issue). Every stated policy ask is met, such as an AI-use
disclosure when the policy requires one.

**Next to a visibly ignored ask.** An empty section, template
placeholder text left in, a required checklist missing, or a
disclosure the policy requires that is absent.

**Not required:** anything the repo does not state. If the repo facts
say "no stated AI policy", an AI disclosure is not required for this
check. Voice and tone are not graded here.
