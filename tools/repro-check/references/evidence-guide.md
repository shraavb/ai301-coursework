# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: eval bundle: the first lines of the "Candidate repro report" (an "Environment:" line or equivalent), plus the "Repo facts" block (latest release) and the issue body (the version the issue targets). Live: the top of the draft repro comment and the issue thread.
- What good looks like: OS and tool version or build are named, and they match the issue's target or the difference is called out in the report. If the issue's behavior depends on build type, config or shell, that is recorded too. No environment record, or a silent deviation (an old version tested against a bug reported on latest), is not good.

## Steps

- Where it lives: the numbered steps or command lines in the "Candidate repro report"; for live, the draft comment's steps.
- What good looks like: a stranger can go from a clean starting state to the trigger using only what is written: exact commands or inputs, the config or file contents needed, nothing that exists only in a private repo or on the author's machine. Short is fine if complete.

## Behavior shown

- Where it lives: code blocks, output excerpts, logs and screenshots in the repro report; compare with the exact error, output or crash quoted in the issue body and thread highlights.
- What good looks like: the artifact itself shows the issue's behavior: same error text and exit status, same input shape or syntax, same kind of failure (a crash is not a graceful error message). An artifact that only shows the tool starting, an altered input, or a different symptom is an adjacent behavior and does not count.

## Honesty

- Where it lives: the report's stated result and its closing expected/actual lines, next to the artifacts above them; for the claim, any sentence that says "I reproduced" or "I verified".
- What good looks like: every claim has an artifact under it. A real attempt that does not reproduce, with the output shown and the differences named, is honest and good. A confident root cause or "verified" with no artifact, or a result its own output contradicts, is not.

## Comms

- Where it lives: the "Candidate claim comment" and the report text, read against the issue and the repo-facts lines on the bug-report template and the contribution policy (including any AI policy).
- What good looks like: the claim names this issue's specifics, says what happens next, and promises only investigation (no guaranteed fix, no date). The comments follow the repo's template asks and AI policy: disclosure where required (tool and extent), human voice where required. Interchangeable boilerplate or a bare "+1" is not good.
