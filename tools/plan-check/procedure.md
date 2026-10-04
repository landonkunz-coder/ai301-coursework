# Procedure: how this skill grades a plan package

These steps grade a plan someone else wrote. They do not write or fix
the plan. Follow them in order. Where to find each part is in
`references/evidence-guide.md`.

## Read order

1. Read the repro evidence first, before the plan. Write down: the
   environment, each numbered step with its observed result, every
   control step (a run where one thing was changed or removed), and
   the Expected and Actual lines. This is the baseline. Reading it
   first keeps the plan's wording from setting what you expect the
   evidence to say.
2. Read the issue: the reported behavior and expected behavior. Note
   what the reporter says is broken, in one line.
3. Read the thread highlights. For each comment, note the author's
   role tag (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR, NONE) and
   whether it gives direction (working as intended, discuss first, a
   constraint, a preferred approach, a claimed cause). Mark which
   claims come from maintainers (OWNER, MEMBER, COLLABORATOR) and
   which from everyone else.
4. Read the repo-facts block. Note every rule it states: contribution
   policy, AI policy or disclosure rules, and anything about how
   outside work is reviewed.
5. Read the candidate plan. Note its stated cause, its in and out
   scope lines (if any), each change or approach step with the file or
   code site it names, its test plan, and any risks or unknowns.
6. Read the candidate plan comment last. Note each cause, change,
   test, or outcome it states or promises.

## Evidence gathering

1. For diagnosis-grounded: put the plan's stated cause next to each
   repro step from Read order step 1. For each step, write "explains"
   or "contradicts", with the step's result quoted. A step contradicts
   the cause if the failure still happens when the blamed part is
   removed, disabled, or not in the loop, or if the failure goes away
   when only something unrelated to the cause changes. If the plan
   says its cause came from the thread, write down who said it (role
   tag) and whether any repro step backs it on its own.
2. For scope-bounded: list each planned change. For each, write
   whether it is needed to fix the reported bug or is extra work
   (rename, refactor, migration, docs pass, new feature, another
   area). Note whether the plan says it defers part of the issue.
3. For executable: for each change, write the code site it names
   (file, function, or clearly identified site) and what it does
   there. Write "none named" if a change names no site or no action.
4. For test-decisive: quote the test plan. Write which repro step or
   behavior it observes and the expected after-fix result it states.
   Write "no observable result" if it only runs a suite, checks for no
   regressions, or checks that code exists or is registered.
5. For honest-certainty: list each claim the plan states as settled
   (cause, difficulty, outcome). Next to each, write the repro step or
   thread line that backs it, contradicts it, or shows it is still
   open.
6. For comment-faithful: list each cause, change, test, or outcome the
   comment states, and next to each the plan line that contains it, or
   "not in plan".
7. For thread-and-convention: list each maintainer direction from Read
   order step 3 and each rule from step 4. Next to each, write the
   comment or plan line that engages or follows it, or "ignored" or
   "broken". If there is no maintainer direction and no stated rule,
   write "none".

## Check execution

1. Grade the checks in rubric order: diagnosis-grounded,
   scope-bounded, executable, test-decisive, honest-certainty,
   comment-faithful, thread-and-convention. Grade every check, even
   after one fails, so the output shows every problem.
2. Grade each check only against its pass condition in `rubric.md`,
   using the notes from Evidence gathering. Do not add requirements
   the pass condition does not state. In particular, do not fail a
   plan for being short, for having no Risks section, for having no
   new automated test, for deferring part of the issue when it says
   so, or for not disclosing AI use when the repo facts do not ask
   for it.
3. Grade `pass` when the notes meet the pass condition. Grade `fail`
   when a note shows the condition is broken (a "contradicts", "extra
   work", "none named", "no observable result", "not in plan",
   "ignored", or "broken" note on a required item).
4. Grade `unclear` only when the evidence the check needs is absent
   from the package (for example, no test plan at all) or the package
   is ambiguous in a way you cannot resolve from its own text. Write
   what is missing.
5. For each check, record one line of evidence: a short quote from the
   package that decided the grade. For a fail, quote the line that
   breaks the condition; for diagnosis-grounded, quote both the plan's
   cause and the repro step that contradicts it.
6. If a check passes by its stated condition but seems wrong, it still
   passes. Note the tension in the summary.

## Verdict assembly

1. Apply the verdict rule in `rubric.md`: `accept` only if every
   required check is `pass`; any `fail` or `unclear` on a required
   check gives `reject`.
2. If the verdict is `reject`, name the deciding check in the summary.
   If more than one check failed, name the first failed check in rubric
   order as deciding and list the others.
3. Write a short summary: one line per check with its grade and
   evidence quote, then the verdict and the deciding check.
4. End with the JSON block from `SKILL.md`, one entry per check in
   rubric order, with the same grades and evidence lines as the
   summary. Nothing after the JSON block.
