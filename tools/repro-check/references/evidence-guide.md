## Environment

Feeds: `env-recorded`

- Where it lives (eval): the repro report's environment record (a
  line or block naming version/commit, OS, and runtime), read against
  the issue context's stated target version or branch and the
  repo-facts block's latest release or default branch.
- Where it lives (live): the draft repro comment's environment line;
  the target comes from the issue body and the repo's tags or
  default-branch commit.
- What good looks like: the report names the project version, tag, or
  commit it ran and the OS it ran on. A tool run through a language
  runtime (Python, Node) also names that runtime's version. If the
  version tested differs from the one the issue targets, the report
  says so.

## Steps

Feeds: `steps-followable`

- Where it lives (eval): the repro report's steps section, from the
  starting state it names (e.g. "fresh clone") to the command that
  produced the shown output.
- Where it lives (live): the draft repro comment's steps.
- What good looks like: each step is an exact command or exact input
  a stranger can type as written. Setup the project's own docs require
  (installing dependencies, checking out a version) is either listed
  or named as "per README". Each input file or data the trigger uses is shown, taken verbatim from the issue, or described precisely enough to recreate it. An input a stranger cannot obtain (private repo, internal config, unnamed download) is not followable. Prose-only steps ("ran the tool on a
  file", "set it up normally") are not followable.

## Behavior shown

Feeds: `behavior-matches`

- Where it lives (eval): the repro report's pasted artifacts
  (terminal output, traceback, log excerpt, test output), read against
  the failure the issue context describes.
- Where it lives (live): the artifacts pasted in the draft repro
  comment, read against the issue body on GitHub.
- What good looks like: the artifact shows the same failure the issue
  names: the same exception or error name, the same message, the same
  exit code, or the same wrong value. A different error from the same
  command, a failure at an earlier step (install, config, import), or
  a result that only says a test failed without showing which error
  (e.g. a bare `XFAIL` or `1 failed`) does not show the issue's
  behavior. For a cannot-reproduce, the artifact shows the issue's trigger (or, if the issue gives no exact command, a run built from its described conditions) running and not failing, and the report names what differed.

## Honesty

Feeds: `outcome-evidenced`

- Where it lives (eval): the sentence where the repro report states
  its outcome, read against the artifacts in the same report; also the
  claim comment's statements about what the author has done.
- Where it lives (live): the same, in the drafts.
- What good looks like: the report states one outcome (reproduced, or
  could not reproduce), and that outcome is visible in the pasted
  output. An evidenced cannot-reproduce is honest and passes. Claims
  the artifacts do not support ("confirmed", "everything in the issue
  is accurate", "root cause is X") with no output behind them are not.

## Comms

Feeds: `conventions-respected`, `claim-specific`

- Where it lives (eval): the repo-facts block's contribution policy
  (AI-use disclosure rules, required comment or PR templates), read
  against both the claim comment and the repro report; the claim
  comment read against the issue context.
- Where it lives (live): the repo's CONTRIBUTING, AGENTS.md, README,
  and `.github/` templates, plus any house rules in `scope.md`; the
  draft claim comment against the issue body.
- What good looks like:
  - Conventions: every requirement the policy states for comments is
    met. If it requires disclosing AI assistance in comments, the comments disclose it (each comment only when the policy says every comment). Rules about pull requests or code, and conditions that cannot be checked from the text, impose nothing on comments. If the policy states no requirement, the check
    passes.
  - Claim: names something only this issue has (the function, error,
    or behavior) and says what the author will post next. It promises
    the investigation, not a fix or a date. A claim that could be
    pasted onto any issue ("please assign me", "I can fix this") is
    boilerplate.