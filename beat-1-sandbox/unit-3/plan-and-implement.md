# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

landonkunz-coder

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5982095823

Plan for this, building on my repro above (commit `2f4e82f`, passlib 1.7.4, bcrypt 4.3.0).

**Cause:** `verify_password` in `core/security.py` calls `pwd_context.verify()` with no error handling. When the stored hash isn't a format passlib recognizes, passlib raises `UnknownHashError` (`passlib/context.py:1132` in my repro output), and it goes straight to the caller.

**Change:** catch `passlib.exc.UnknownHashError` in `verify_password` and return `False`, and update the docstring to match. I'll also remove the `xfail` marker on `test_verify_with_wrong_hash_format`, like the issue says. Two files: `core/security.py` and `tests/unit/test_security.py`.

**Not changing:** other passlib exceptions I haven't reproduced (like a truncated bcrypt hash), `hash_password`, the JWT code, or the login route. The route already treats `False` as a failed login.

**How I'll check it:** re-run my repro command. Today it's `1 xfailed`, and `UnknownHashError` with `--runxfail`. After the fix it should be `XPASS(strict)` until I remove the marker, then `1 passed`, with the rest of `tests/unit/test_security.py` still passing.

I used Claude to help draft this plan. I read the code and the repro output it's based on myself.

---

## Your branch

**Branch**

`fix/72-verify-password-unknown-hash`

**Evidence**

**Before** (my Unit 2 posted repro, commit `2f4e82f`, https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5860933386):

```
.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
================ 24 deselected, 1 xfailed, 2 warnings in 2.26s =================

.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format --runxfail
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified

.venv/lib/python3.12/site-packages/passlib/context.py:1132: UnknownHashError
...
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
================= 1 failed, 24 deselected, 2 warnings in 0.18s =================
```

Re-checked on the branch before any edit:

```
.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format --runxfail
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - passlib.exc.UnknownHashError: hash could not be identified
================= 1 failed, 24 deselected, 2 warnings in 0.54s =================
```

**After the fix, marker still on:**

```
.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format FAILED [100%]
[XPASS(strict)] issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False
================= 1 failed, 24 deselected, 2 warnings in 0.25s =================
```

**After the fix, marker removed:**

```
.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]
================= 1 passed, 24 deselected, 2 warnings in 0.25s =================

.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format --runxfail
tests/unit/test_security.py .                                            [100%]
================= 1 passed, 24 deselected, 2 warnings in 0.14s =================

.venv/bin/mypy --python-version 3.12 api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files

make test-unit
================= 376 passed, 52 xfailed, 5 warnings in 14.86s =================
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Calibration trap check (`--only calib-01,calib-03 --include-calibration`, unscored): calib-01 accept (gold accept), calib-03 reject (gold reject).
2. Full run: 19/20 scored items (bar: 18/20: PASS). Categories: clear-accept 6/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. This is the run saved in `eval-run.txt`.

**Package analysis**

pkg-14. My rubric said reject and the gold label said accept. It failed only on executable. The grader quoted "exact functions to be pinned in the PR after tracing the query issuance with debug logs" and decided no file or function was named. My executable check says the plan has to name a file, function, or clearly identified code site and what the change does there, so the grader read "functions to be pinned later" as not naming one. I think the gold label is right though because the plan does name the reattach path in `zellij-server` and the session connection handling, it has a working debug trace, and the other 6 checks all passed. So the plan was specific enough to start on and my check was just stricter than it needed to be on the "clearly identified code site" part.

**Check rationale**

Quoted from `tools/plan-check/rubric.md`:

| executable | The plan's change, approach, or files lines. | A stranger could start the work without asking the author anything: the plan names where the change goes (a file, function, or clearly identified code site) and what the change does there. Fail if the plan only says it will look around, figure out where something lives, or "fix" the behavior without saying how. | required |

It reads that way because of calib-02 in the activity, where the plan just said it would poke around the editor code and figure out where undo lives. I wanted the check to fail that, so I made it name where the change goes and what it does there, and I put the "look around" and "figure out where" wording in the fail line so the grader knows what bad looks like. I said a new automated test or exact line numbers aren't needed so short plans like calib-01 still pass, since it only names one callback in one file.

**Trade-offs**

Keeping executable strict cost me pkg-14, a clear accept that got held because it said it would pin the exact functions later. I decided not to loosen it because I was already at 19/20 with every category matched, and all 3 unbuildable packages agreed. Loosening it would mean `--only` runs on pkg-14 plus unbuildable canaries and then another full run for about $4, and it could flip one of the unbuildable ones and drop a category I already had. So I accept that my rubric will miss a plan that names the area but leaves the exact function for later, in exchange for catching plans that don't name anything at all.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
