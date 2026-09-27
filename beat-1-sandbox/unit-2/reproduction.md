# Unit 2 — Claim and Reproduce

## Your identity upstream

**GitHub username**

landonkunz-coder

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5860909216

Picking this up as my first contribution. The issue says `verify_password` in `core/security.py` raises passlib's `UnknownHashError` when the stored hash is malformed, instead of returning `False`. I'll reproduce it on current `main` and post a repro report here with my environment, the exact steps, and the output.

I used Claude to help me draft this and plan the repro; I'll run every step myself.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5860933386

Reproduced on current `main`.

**Environment:** commit `2f4e82f` (2026-09-16), macOS 26.3.1 (arm64), Python 3.12.14 (via mise), passlib 1.7.4, bcrypt 4.3.0. I skipped the Docker, database, and frontend parts of setup since this test doesn't touch them.

**Steps** (from a fresh clone of my fork):

```
cp .env.example .env
mise exec python@3.12 -- python -m venv .venv
.venv/bin/python -m pip install --upgrade pip setuptools wheel
.venv/bin/pip install -e ".[dev]"
.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
.venv/bin/pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format --runxfail
```

The test calls `verify_password("password", "not_a_valid_bcrypt_hash")`. The second run uses `--runxfail` so pytest ignores the xfail marker and shows the real error.

**Normal run:**

```
================ 24 deselected, 1 xfailed, 2 warnings in 2.26s =================
```

**With `--runxfail`:**

```
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified

.venv/lib/python3.12/site-packages/passlib/context.py:1132: UnknownHashError
...
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
================= 1 failed, 24 deselected, 2 warnings in 0.18s =================
```

**Expected:** `verify_password` returns `False` for a malformed stored hash.

**Actual:** it raises passlib's `UnknownHashError`, same as the issue describes.

I used Claude to help me set up the environment and format this report. I ran every command and the output above is from my machine.

## Eval iterations

**Run history**

1. `--limit 1` smoke test: 1/1.
2. First full run: 14/20. All six misses were packages gold accepts that my rubric rejected (pkg-03, 05, 07, 09, 10, 12), so the rubric was too strict: outcome-evidenced failed 3, steps-followable 2, conventions-respected 1.
3. After loosening outcome-evidenced, steps-followable, behavior-matches, and conventions-respected, `--only` on the six misses plus canaries pkg-14, pkg-18, pkg-20, calib-03: 4/5 graded packages agreed (03, 05, 07, 10 fixed, 09 still rejected). The other 5 errored because I hit my course spend limit, so I switched the rest of my runs to my personal Claude account.
4. After adding an honest-attempt clause to behavior-matches, `--only pkg-09,pkg-12` plus canaries pkg-14, pkg-18, pkg-20, calib-03: 5/5 scored agreed, calib-03 still rejected.
5. Confirming full run with `--save-run`: 20/20, every category matched.

**Package analysis**

pkg-07 (p5.js#7168). In my first full run my rubric decided reject and the gold label is accept. It failed conventions-respected. The grader's evidence was that the AI disclosure "appears only in the claim comment, not in the repro report comment ... though the evidence guide requires both comments to disclose it." But p5.js's policy only says fully AI-generated contributions aren't accepted and that assistive use is fine if the contributor takes responsibility. It never asks for disclosure in comments at all. My evidence guide said "If it requires disclosing AI assistance, both comments disclose it," and the grader read that as a rule the repo actually had. So my rubric invented a requirement. After I rewrote the check to count only rules the policy explicitly states for issue comments, pkg-07 came back accept, and pkg-20, the package where the repo really does require disclosure in every comment, still came back reject.

**Check rationale**

> **conventions-respected** (required)
>
> Evidence: The repo's rules for contributors (AI-assistance disclosure, required templates or formats): in eval mode the repo-facts block; in live mode the repo's CONTRIBUTING, AGENTS.md, README, and .github/ templates (see the evidence guide's Comms section). Read against every comment in the package.
>
> Pass condition: Every requirement those rules explicitly state for issue comments is met. Count only what the policy says about comments: rules about pull requests or code, and conditions a reader cannot check from the text ("must understand the work"), impose nothing here. A policy that allows AI use without asking for disclosure requires no disclosure. If the policy requires disclosure in every comment, each comment must disclose; otherwise one disclosure in the package covers it. If the rules state no such requirement, pass.

The first version said every requirement in the repo-facts block had to be met by both comments. That broke twice. In live mode there is no repo-facts block, so my own claim would have graded unclear and failed, so I added where to look in live mode. Then pkg-07 showed the grader stretching "both comments disclose" into a disclosure rule p5.js never wrote. I rewrote it to count only what the policy says about issue comments, to ignore rules about PRs or code and conditions nobody can check from the text, and to require disclosure in each comment only when the policy says every comment. I rejected just deleting the disclosure part, because pkg-20 is the one package where that rule is real, and the category floor needs it caught.

**Trade-offs**

Loosening conventions-respected risked letting the disclosure wall through, so pkg-20 went into every `--only` run after that change as a canary. It errored once on the spend limit, then stayed reject in run 4 and in the confirming run. The case I accept missing: a repo whose policy asks for AI disclosure only in the pull request. My check now says that imposes nothing on comments, so a claim or repro there with no disclosure passes. That's on purpose, since comments aren't PRs, but a maintainer who expects disclosure everywhere would disagree.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
