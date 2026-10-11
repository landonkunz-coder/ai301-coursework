# Procedure: how this skill grades a PR package

These steps grade a pull request someone else built. They do not fix
the PR or rewrite its description. Follow them in order. Where to find
each part is in `references/evidence-guide.md`.

## Read order

1. Read the plan first, before anything in the PR. Write down: its
   in-scope change, its not-in-scope line, every file or folder on its
   "Files:" line, its test plan with any expected after-fix result, and
   every deviation note. This is the baseline. Reading it first keeps
   the PR's description from setting what you expect the diff to do.
2. Read the issue and its thread highlights. Note the reported and
   expected behavior in one line each, and any maintainer direction
   (OWNER, MEMBER, COLLABORATOR) about the approach or the PR.
3. Read the repo's stated standards: the PR template's sections and
   checklist items, the contribution policy, and any AI or disclosure
   rule. List each stated ask. If none are stated, write "none".
4. Read the diff. First list every changed file. Then read each hunk
   and note what it changes. Then read the commit list.
5. Read the test evidence: every command and whatever follows it.
6. Read the title and description last. Note each factual claim it
   makes about what the PR changes, adds, tests, or leaves out,
   including checklist items and ticked boxes.

## Evidence gathering

1. For scope-faithful: make a table of changed files. Next to each,
   write the plan line, folder, or deviation note that names it, or
   "not in plan". Then list each in-scope change from the plan and
   write the hunk that delivers it, the description or deviation line
   that says it was left out, or "missing".
2. For description-true: list each claim from Read order step 6. Next
   to each, write the diff line or file that proves it, or
   "contradicted" (quote the diff line that breaks it), or "nothing
   behind it". For a claim that something was not added, scan the
   whole diff before writing that it holds.
3. For tested: first find the reproduction's after run (the issue's
   own repro steps or input re-run on the change). Write its command
   or steps and quote the after result. Label it "output shown"
   (printed output), "concrete result stated" (a specific value, exit
   code, message, or on-screen behavior, including in a `#` comment
   after the command), "vague" (only "works", "fixed", "passes", or
   similar), "not the repro" (only a control or a different input was
   run), or "missing". Write whether the result shows the expected
   fixed behavior. Then list each other check the plan's test plan or
   the repo's template names, and write the outcome the PR reports for
   it, or "not mentioned". For a check that failed or was not run,
   write the reason given, or "no reason".
4. For clean-diff: for each hunk, write one label: fix, test,
   docs/changelog, repo-required (an edit the repo's own rules require
   for this fix, such as removing the issue's xfail marker),
   recorded-deviation (a change the fix needed that the plan or
   description records), debris (debug output, commented-out code,
   reformatting of lines the fix does not otherwise change), or
   unrelated (a change the issue does not need).
5. For template-met: for each stated ask from Read order step 3,
   write the description line that meets it with real content, "N/A
   with reason", "missing", or "placeholder left". If step 3 found
   none, write "none stated".

## Check execution

1. Grade the checks in rubric order: scope-faithful,
   description-true, tested, clean-diff, template-met. Grade every
   check, even after one fails, so the output shows every problem.
2. Grade each check only against its pass condition in `rubric.md`,
   using the notes from Evidence gathering. Do not add requirements
   the pass condition does not state. In particular, do not fail a PR
   for a short description, for its number of commits, for an
   unticked CI box, for having no tests beyond what the plan names,
   for a before run quoted from the posted repro, for an after result
   stated in a comment rather than pasted when it names a concrete
   observed result, for supporting checks reported in words without
   pasted output, for removing the issue's xfail marker, for a
   shortfall the description or deviation notes disclose, or for not
   disclosing AI use when the repo states no such rule.
3. Grade `pass` when the notes meet the pass condition. Grade `fail`
   when a note shows the condition is broken: "not in plan" or
   "missing" with no note (scope-faithful), "contradicted" or "nothing
   behind it" (description-true), "missing", "not the repro", or
   "vague" on the reproduction's after run, an after result that does
   not show the fix, "not mentioned", or "no reason" (tested),
   "debris" or "unrelated" (clean-diff), "missing" or "placeholder
   left" (template-met).
4. Grade `unclear` only when the evidence the check needs is absent
   from the package (for example, no diff at all) or the package is
   ambiguous in a way you cannot resolve from its own text. Write what
   is missing.
5. For each check, record one line of evidence: a short quote or file
   path from the package that decided the grade. For a fail, quote the
   line or name the file that breaks the condition.
6. If a check passes by its stated condition but seems wrong, it still
   passes. Note the tension in the summary.

## Verdict assembly

1. Apply the verdict rule in `rubric.md`: `accept` only if every
   required check is `pass`; any `fail` or `unclear` on a required
   check gives `reject`.
2. If the verdict is `reject`, name the deciding check in the summary.
   If more than one check failed, name the first failed check in
   rubric order as deciding and list the others.
3. Write a short summary: one line per check with its grade and
   evidence, then the verdict and the deciding check.
4. End with the JSON block from `SKILL.md`, one entry per check in
   rubric order, with the same grades and evidence lines as the
   summary. Nothing after the JSON block.
