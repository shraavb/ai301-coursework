<!-- DRAFT: reword in your own words, then delete this comment. -->
# nvkr: PR #442, reject unknown resolve output formats (fixes #398)

## Overview

nvkr is a Go command-line tool, and its `resolve --output` flag lists five formats but never checked the value, so a typo like `--output jsno` exited 0 and printed the default YAML. I reproduced the bug on a clean checkout of `main`, added a validation function that runs before any input is read, and wrote three tests that fail without it. The maintainer approved [PR #442](https://github.com/dsx-ai-factory/nvkr/pull/442) on 2026-11-08 and squash-merged it on 2026-11-09, closing issue #398; the one review finding was an agent `Co-Authored-By` trailer on my commits, which I had not checked against the repo's rules.

## What happened

**The bug.** `resolve --output` accepts `ir`, `canonical`, `nodes`, `digest` and `components`, but any other value fell into the `default:` branch of the output `switch`. I reproduced it on `origin/main` at `95b61bc5` with the command from the issue:

```sh
nvkr resolve --data inputs/catalogs/nvke/module-tags \
  --component nvkectl --platform linux/amd64 --output jsno
```

It exited 0 with empty stderr and printed 82 lines of YAML, byte-identical to the default `ir` output (`cmp` printed nothing). With a nonexistent `--data` path and the same typo, it failed on the catalog (`NVKR-E-INVALID-CATALOG`, exit 16) and never reported the format.

**The fix.** I added one local function, `validateResolveOutput`, in `cmd/nvkr/resolve.go`. It accepts exactly the five formats and returns `CodeUsage` for anything else, and the `resolve` action calls it first, before `selectCommandPresentation`, resolution and the TUI branch. I left the output `switch` and the flag's default unchanged. The same command now gives:

```
exit status: 2
stdout bytes: 0
nvkr: [NVKR-E-USAGE] unknown resolve output "jsno" (use ir, canonical, nodes, digest, or components)
```

**The tests.** I added three tests in `cmd/nvkr/resolve_output_test.go`: unknown, empty and wrongly cased values return `NVKR-E-USAGE`; the format error wins over a missing catalog; and an invalid format cannot reach the TUI viewer. I ran them on untouched `main` first, and they failed:

```
--- FAIL: TestResolveOutputRejectsUnknownFormats (0.01s)
    --- FAIL: TestResolveOutputRejectsUnknownFormats/output=jsno (0.01s)
        resolve_output_test.go:46: error = <nil>, want NVKR-E-USAGE
--- FAIL: TestResolveOutputInvalidFormatCannotTakeTheTUIBranch (0.00s)
    resolve_output_test.go:64: invalid output format reached the viewer
```

On my branch all three pass (`ok  github.com/dsx-ai-factory/nvkr/cmd/nvkr  0.301s`). The existing `TestResolveOutputFormsShareBuildIdentity` already runs all five valid formats through the new check, so I did not duplicate it.

**A change I made because a check stopped me.** My voice guard, a hook I wrote that checks my own GitHub comments before they post, stopped my first `gh pr create` on 2026-11-07 at 19:47. The log line reads `stop: rule broken: "Paste test output, do not summarize it"`. I revised the description to paste the real before and after output, and the guard passed it at 19:48. <!-- CHECK: confirm from your own memory what the first draft said and what you changed. --> The final PR also has a section, "What I could not get green", listing the Go test failures that occur on untouched `main`.

**What landed.** The maintainer approved with this comment: "A mistyped `--output` value used to get past the CLI. Now it stops right away with `NVKR-E-USAGE` and exit code 2, before any catalog reads or the TUI, which is exactly what someone scripting against the CLI needs." They merged it the next day. Their one finding was minor: "Both commit messages end with `Co-Authored-By: Claude Sonnet 5.5 <noreply@anthropic.com>`", which the repo's AGENTS.md forbids. The squash message kept only my sign-off, so the trailer did not reach `main`.

## Limitations and what's next

- `make check` does not go green on my machine. The Go tests fail in four packages (`buildgraph`, `catalogmaintenance`, `factory`, `pipeline`), and the same tests fail on untouched `main`. I did not diagnose the causes, and I said so in the PR.
- The format list is written in three places: the validator, the output `switch` and the flag's usage text. Adding a format means editing all three, and the test keeps its own copy of the list on purpose.
- I opened the PR without running a plan check or a ship-gate (a script that runs a repo's required checks before I ship), so nothing checked my commit messages against the attribution rule. My ship manifest now has a row that fails on a `Co-Authored-By` line, and its "excluded" table lists the failing Go tests with a date.
- Next I would run that check before every PR. My second merged contribution, [PR #510](https://github.com/dsx-ai-factory/nvkr/pull/510), adds `--format json` to `release pull` (issue #393), and I would write it up the same way.
