# Rubric: is this plan ready to post and build from?

Every check reads the plan against the package's own evidence, never
its length, headings, or polish. A short plan with no Risks section
and no new automated test can pass every check; a long, confident plan
can fail one.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (diagnosis or cause line), read against every step, control, timing, and the Actual line in the repro evidence. Also note where the cause came from (the student's own repro, or a thread comment). | The stated cause explains every repro step, including any control step. Fail if any step shows the failure persisting when the blamed part is removed, disabled, or not involved, or shows the failure going away when only an unrelated thing changes. Fail if the cause is taken from a thread comment and the repro evidence does not back it on its own. Fail if the planned change works around the symptom downstream while the evidence points at a different cause. | required |
| scope-bounded | The plan's scope statement (in and out lines, if any) and its list of changes or approach steps, read against the issue's reported behavior. | Every planned change serves fixing the reported bug. Fail if the plan adds unrelated work: renames, refactors, config or data migrations, docs passes, new features, or changes to areas the bug does not touch. A plan that fixes only part of the issue passes if it says which part it defers. A missing not-in line alone is not a fail. | required |
| executable | The plan's change, approach, or files lines. | A stranger could start the work without asking the author anything: the plan names where the change goes (a file, function, or clearly identified code site) and what the change does there. Fail if the plan only says it will look around, figure out where something lives, or "fix" the behavior without saying how. | required |
| test-decisive | The plan's test plan, read against the repro evidence's steps and Expected line. | The test plan names an observable check of the reported behavior (re-running the repro steps, or a test that exercises the same behavior) and states the result expected after the fix, so the result would differ between broken and fixed code. Fail if the test plan is only "run the suite", "nothing regresses", "it works", or checks that code exists or is registered without observing the bug's behavior. A manual re-run of the repro steps passes; a new automated test is not required. | required |
| honest-certainty | The plan's claims about cause, difficulty, and outcome, and any risks or unknowns lines, read against the repro evidence and the thread highlights. | The plan does not state as settled anything the package shows is unsettled or contradicted (for example, presenting an unverified thread claim as the confirmed cause, or calling a fix simple when a maintainer said it is hard without saying how the plan avoids that). Open questions, if any, are stated as open. A missing Risks section alone is not a fail. | required |
| comment-faithful | The candidate plan comment, read against the candidate plan. | Every cause, change, and test the comment states is in the plan, and the comment states the plan's cause and approach in some form. Fail if the comment promises an outcome, scope, or test the plan does not contain, or carries no plan at all (enthusiasm, "please assign me", "I'll dig in"). | required |
| thread-and-convention | The candidate plan comment and plan, read against the thread highlights (each comment's author role: OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR, NONE) and the repo-facts block (contribution policy, AI policy, bug-report template). | The comment does not contradict or ignore direction from an OWNER, MEMBER, or COLLABORATOR in the thread (for example: working as intended, discuss first, a stated constraint, a preferred approach); where such direction exists, the comment engages it. The plan and comment follow every rule the repo-facts block states (for example, required AI disclosure or human-written comments, discuss-before-PR). Passes if the thread has no maintainer direction and the repo facts state no rule the package breaks. | required |

## Verdict rule

- `accept` if every required check grades `pass`.
- `reject` if any required check grades `fail` or `unclear`. Unclear
  counts as fail: a plan I cannot verify from the package is not
  ready to build from.
- There are no preferred checks; nothing outside the table changes the
  verdict.
- Length, formatting, section headings, and tone never count toward
  pass or fail. Tone and voice are the voice guide's job in live mode,
  not the rubric's.
