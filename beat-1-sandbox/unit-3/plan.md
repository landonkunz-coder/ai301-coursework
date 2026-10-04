# Plan: issue #72, `verify_password` raises on malformed stored hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72
Repro report: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5860933386

## Repro evidence this plan relies on

From my posted repro (commit `2f4e82f`, macOS 26.3.1 arm64, Python
3.12.14, passlib 1.7.4, bcrypt 4.3.0). The test calls
`verify_password("password", "not_a_valid_bcrypt_hash")`.

Normal run:

```
================ 24 deselected, 1 xfailed, 2 warnings in 2.26s =================
```

With `--runxfail`:

```
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified

.venv/lib/python3.12/site-packages/passlib/context.py:1132: UnknownHashError
...
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
================= 1 failed, 24 deselected, 2 warnings in 0.18s =================
```

## Diagnosis

`verify_password` in `core/security.py` (line 37) returns
`bool(pwd_context.verify(plain_password, hashed_password))` with no
error handling. When the stored hash is not a format passlib
recognizes, `CryptContext.verify` cannot pick a handler and raises
`passlib.exc.UnknownHashError` (`passlib/context.py:1132` in the
traceback above). Nothing in `verify_password` catches it, so the
exception reaches the caller instead of the function returning
`False`.

## Scope

In scope: making `verify_password` return `False` when passlib raises
`UnknownHashError` for the stored hash, and removing the xfail marker
on `test_verify_with_wrong_hash_format`.

Not in scope:

- Other exceptions passlib can raise (for example a `ValueError` on a
  hash that looks like bcrypt but is truncated). My repro only shows
  `UnknownHashError`, so I am not catching anything I have not shown.
- `hash_password`, the JWT functions, and `pwd_context` settings.
- The caller in `api/routes/auth.py` (line 80). It already treats a
  `False` result as a failed login, so it needs no change.

## Files

- `core/security.py`: `verify_password` only.
- `tests/unit/test_security.py`: remove the `@pytest.mark.xfail(...)`
  marker on `test_verify_with_wrong_hash_format`. The test body stays
  as it is.
- `pyproject.toml`: no change. I checked for a suppression tied to
  #72 or H-05 and found none.

## Approach

1. Confirm the test still fails on my branch before changing anything
   (re-run the `--runxfail` command and expect `1 failed`).
2. In `verify_password`, import `UnknownHashError` from `passlib.exc`,
   wrap the `pwd_context.verify` call in `try`/`except
   UnknownHashError`, and return `False` in the `except`. Update the
   docstring's Returns line to say a malformed or unrecognized hash
   also returns `False` (Google style).
3. Re-run the test with the marker still on. Expect `XPASS(strict)`
   reported as a failure, which shows the fix works and the marker
   now has to go.
4. Delete the xfail marker. Re-run and expect `1 passed`.
5. Run `make check && make test-unit` before pushing.

Commit message: `fix(api): return False from verify_password on unrecognized hash`
with footer `Fixes #72`. Branch: `fix/72-verify-password-unknown-hash`.

## Test plan

Re-run my Unit 2 repro steps on the branch:

```
.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
```

- Before the fix (from my posted repro): `1 xfailed`, and with
  `--runxfail`, `UnknownHashError` and `1 failed`.
- After the fix, marker still on: `XPASS(strict)`, `1 failed`.
- After the fix and the marker removed: `1 passed`. The same command
  with `--runxfail` also gives `1 passed`, with no `UnknownHashError`.
- `make test-unit`: every other test in `tests/unit/test_security.py`
  still passes, including `test_verify_password_correct` and
  `test_verify_password_incorrect`, so a valid hash still verifies.

## Risks and unknowns

- A login against a user row with a malformed hash changes from an
  unhandled exception to a normal failed login. That is what the
  issue asks for ("fail closed"), but it also means a corrupted hash
  stops being loud. I am not adding logging, because that is outside
  the issue's scope.
- I have not checked whether other malformed inputs (a truncated
  `$2b$` hash, `None`) raise something other than `UnknownHashError`.
  If one comes up while building, I will record it under Deviations
  instead of quietly widening the `except`.
- I could not run the parts of the setup that need Docker, so I can
  run `make test-unit` but not the integration tests locally. CI will
  run them.

## Deviations

The code matched the plan exactly going from a 1 xfailed to 1 pass as predicted. The thing that differed was make check stopped at mypy because my venv is python 3.12 the config is 3.11 so I re ran it with 3.12 which then reported success. One thing I didn't explore was the truncated hash case from risks never came up so the except stayed narrow.
