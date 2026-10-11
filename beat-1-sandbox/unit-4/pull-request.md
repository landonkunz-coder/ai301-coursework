# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/140

**Branch**

fix/72-verify-password-unknown-hash

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--only calib-01,calib-03 --include-calibration` (unscored trap check): calib-01 disagreed (gold accept, my verdict reject, failed `tested`), calib-03 agreed (reject).
2. `--only calib-01 --include-calibration` after loosening `tested` so only the repro's after run needs printed output: calib-01 agreed (accept).
3. Full run: 17/20 scored items, below the bar. categories: clear-accept 4/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. Misses: pkg-08, pkg-11, pkg-13, all clear-accept, all failed on `tested`.
4. `--only pkg-08,pkg-11,pkg-13,pkg-07,pkg-10` after changing `tested` to accept a concrete observed result (pkg-07 and pkg-10 as not-tested canaries): 5/5.
5. Full run, saved with `--save-run eval-run.txt`: 19/20 scored items (bar: 18/20: PASS). categories: clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. Only miss: pkg-11 (failed `description-true`).

**Package analysis**

pkg-11 (clear-accept, gold accept):
- Run 3: my rubric said reject, failed `tested`. Evidence line: "After run is only `#` comments ('# after: searches normally, no error'; '# after: 'foo ' not listed'), no printed output". That is what made me change `tested`.
- Run 5: my rubric said reject again, but on `description-true`. Evidence line: "Claims tests for 'escaped, unescaped, and double-backslash cases' but the only test, escaped_trailing_space_is_kept, covers escaped and double-backslash; no plain unescaped-trim case is in the diff."
- Why it is arguable: the test input `foo\   ` does contain unescaped trailing spaces that get trimmed, so the unescaped case is arguably covered inside the one test. In run 3 the same `description-true` wording passed it ("'one function touched' is loose but not contradicted").
- What it shows: same rubric text, two different reads across runs. My `description-true` says a claim fails if it "has nothing in the diff behind it", and the grader has to decide how literally to read "tests added for ... unescaped" against one combined test.

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`, the `tested` pass condition:

> Pass if (a) the issue's own reproduction (its steps or input, not only a control or a different case) is re-run on the change, and the PR shows the command or steps with a concrete observed after result (printed output, or a stated value, exit code, message, or on-screen behavior) that shows the fixed behavior the plan or issue expects (the before may be quoted from the posted repro), and (b) every other check the plan's test plan or the repo's template names is reported with its outcome, where a failing or not-run check passes when the reason is stated. Fail if the reproduction's after run is missing, if only a control or a different input was run, if the after result is vague ("works now", "fixed", "tests pass") with no concrete observed result, if it does not show the fix, or if a named check is never mentioned. Not required: output pasted verbatim (a concrete result stated in a comment after the command counts); printed output for checks other than the reproduction; tests beyond what the plan names; CI results, since CI runs after the PR opens.

- First version came from the activity rule: a `#` note after a command is not proof, every test-plan item needs command plus printed output.
- calib-01 failed it: its smoke check was described, not pasted, but gold accepts. Loosened so only the repro's after run needs output.
- Full run 3 still held pkg-08, pkg-11, pkg-13: their after runs were a keystroke narrative or `#`/`--` comments with specific results (`'foo ' not listed`, `{ code = 0, stderr = "" }`). Gold accepts all three.
- The not-tested rejects showed the real line: pkg-04 was vague prose ("colors work now"), pkg-07 ran only a control, pkg-10 never re-ran the repro. So the check now asks for a concrete observed result of the repro itself, not pasted output.
- Rejected: keeping "printed output required", because it held clear accepts.

**Trade-offs**

- Loosening `tested` could have flipped not-tested rejects. pkg-07 and pkg-10 were the only rejects that failed on `tested` alone, so I re-ran them as canaries in run 4. Both stayed reject. Every other reject also failed another check, so the change could not flip them; run 5 confirmed not-tested 4/4.
- What it gives up: in live mode, my tool will not hold a PR whose repo checks are only described in words. It held my own PR anyway, for a different reason: my plan's "XPASS(strict)" test-plan step was never reported, so I added that output before opening.
- Accepted miss: pkg-11 flips between runs on `description-true`. I did not loosen that check, because the silent-drift packages depend on it catching false test claims.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
