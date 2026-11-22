# Contribution: Reject unknown `resolve` output formats before loading inputs

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Shraavasti Bhat
**GitHub Username:** shraavb
**Issue:** https://github.com/dsx-ai-factory/nvkr/issues/398
**Pull Request:** https://github.com/dsx-ai-factory/nvkr/pull/442

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: issue-scout, a GitHub-wide search, rank and grade skill**

I built a search strategy of LMCache, ros2_control, Python SDK and Python bug queries with a 16-step ranking filter, and the scout graded the top results with my rubric. My first filters let through 126 results, mostly brand-new hobby repos and bounty bait, so I added drops for zero comments, bounties, stale labels and tracking issues. Every later search I ran, across LMCache, the vLLM and SGLang orgs, Rust, C and C++ repos, and security keywords, hit issues already claimed through open PRs or `/claim` comments, which ranking can't see, so I added `-linked:pr` to the LMCache queries and logged the rest in scan-notes.md. The only issue that passed grading was hypertorch#433, which I claimed on GitHub.

**Unit 6: voice-guard, a pre-post check on my own words, and a repo-memory file**

I merged my three voice guides into one file at `~/.claude/voice-guard/voice-guide.md`, wrote Upstream rules for claims, questions and pings, and hooked `check_voice.py` into Claude Code so every `gh issue comment` is held against them. It fired for real on my claim for nvkr#398: the guard log shows it stopped the first two drafts, quoting my own rules "Promise investigation, never a fix or a date" and "Claim only what I will do next, with a concrete first step", and passed the third. It also has gaps I found: its first manual run passed an empty draft, and it stops `gh issue comment --help` because it can't read a body. For the repo memory file I dropped the `@AGENTS.md` import because nvkr's `CLAUDE.md` is a symlink to `AGENTS.md`, and my scoped orientation corrected a wrong path I had assumed (`pkg/nvkr/planview`, not `internal/planview`), so I wrote the right one into `CLAUDE.local.md`.

**Unit 7: ship-gate and a gate manifest, set up after the PR had already merged**

I wrote `ship-manifest.md` for nvkr with three required checks, `make check`, `make build` and a commit-trailer check, each sourced to CONTRIBUTING.md or AGENTS.md, and installed the ship-gate on 2026-11-21. I did not run it on `fix/398-validate-resolve-output`: I opened PR #442 on 2026-11-07 and it merged on 2026-11-09, before the gate was installed, so the gate has no verdict on my branch and I will not claim one. What I ran by hand at the time matches the manifest: `make check` and `make build`, with the same Go test failures in four packages on untouched `main` and on my branch. The manifest's "Excluded" table now holds those failures, dated 2026-11-07 (observed on untouched `main`) with the reason, so the gate does not stop on `make check` for a reason my fix does not touch; I did not diagnose their causes. A reviewer also caught what a gate should have caught: both commits carried an agent `Co-Authored-By` trailer that nvkr's AGENTS.md forbids. The third manifest row now checks for that trailer, and the ship-gate would have flagged it before the PR opened. I also ran the two `make` commands by hand on 2026-11-21 and both stopped at `Go go1.27.3 does not match pinned go1.27.4`, because `.go-version` has since moved, so neither check has passed on my machine since I installed the gate. I have not tested the hook itself.

**Unit 8: pr-monitor, a read-only PR state check and drafting pass, with my own playbook**

On 2026-11-21 I installed the PR monitor, wrote my `playbook.md` (answer first then thank in a clause, 1-3 sentences, a 7-day nudge threshold sourced to CONTRIBUTING.md, one nudge only, close after 14 quiet days), and ran it from my `nvkr` clone on `feat/393-release-pull-json`. It read PR #510 (my #393 branch, not #442): merged, approved, 30 checks passing, quiet for 4 days. It drafted a short "merged follow-up" thank-you that cites my rules 1 and 2, and I did not post it, because the PR had already merged and the thank-you added nothing. It fell short where it could not see GitHub: it could not tell whether #393 auto-closed or who merged, so its draft told me to check both before posting. My 15-PR sample for the staleness threshold also cannot separate outside contributors from the team, since every author shows as CONTRIBUTOR, so I wrote that limit into the playbook. I changed the playbook, not the tool: for the next PR the 7-day threshold and the single nudge are now written down before I need them.

---

## Why I Chose This Issue

I found nvkr#398 by feeding my scout a list of good-first-issue candidates from a `gfi-watch` scan of the `dsx-ai-factory` org. After the scope patch, the scout graded #398 and #393 as accept: both pass every required check (maintainer committing daily, no assignee or linked PR, no AI ban, one bounded deliverable). Its difficulty notes were **straightforward** for #398, because the issue names `cmd/nvkr/resolve.go` lines 20, 29 and 77, and **moderate** for #393, which spans two files and has a small open question about how directory artifacts are represented. I chose #398 as my main issue and claimed #393 as a second pick for after it. Earlier I graded the three LMCache issues I had picked, #1175, #5313 and #5402, and the scout rejected all three because open PRs and `/claim` comments already covered them, so I did not choose against my scout.

#398 fits my profile: a small logic bug with an exact repro and a stated expected result, in code I can read, with tests to write. It is in Go rather than Python, and I want to learn how a fail-closed CLI validates input before doing any work, and how to write a test for the TUI branch that an invalid value must not reach. My honest read of the scout: it held up on the repo and claim checks, but it can't see `/claim` comments or linked PRs at search time, which is why most of my searches ended in already-taken issues until I added `-linked:pr`. Its `maintainer-responsive` check passes here only because the maintainer left a placeholder "—" on each thread, and the 90-day claim rule read literally would fail an issue on my own claim comment, so I would patch both. Claim comment: https://github.com/dsx-ai-factory/nvkr/issues/398#issuecomment-6716217331

**Toolkit**

- Tool: issue-scout (my search strategy, my rubric), run on hand-fed issue URLs from the `gfi-watch` list, then re-run after the scope patch
- What it produced: accept for nvkr#398 (difficulty: straightforward) and #393 (moderate), and rejects for LMCache #1175, #5313 and #5402
- Difficulties: it treated my own claim comment as a possible active claim, and its `maintainer-responsive` check passed on a placeholder comment
- What changed: I replaced `scope-bounded` with `scope-feasible` plus a difficulty note, logged the claim and responsiveness tensions in scan-notes.md, and would patch both checks

---

## Understanding the Issue

**Problem Description**

`nvkr resolve --output` documents five formats, but nothing checks the value. Any other string, including a typo, falls into the `default:` branch of the output `switch` and prints the default YAML. A script with `--output jsno` therefore looks like it worked and hands the next step the wrong format, and the real cause only shows up later.

**Expected Behavior**

Only `ir`, `canonical`, `nodes`, `digest` and `components` are accepted. An unknown, empty or wrongly cased value fails with `CodeUsage` (exit status 2), names the five accepted formats, prints nothing on stdout, and fails before any catalog is read or the TUI can open. The five valid formats keep their exact bytes and exit codes.

**Current Behavior**

On `origin/main` at `95b61bc5`, `--output jsno` exits 0 with empty stderr and prints the full 82-line YAML graph, byte-identical to the default `ir` output. With a nonexistent `--data` path and the same typo, it fails on the catalog (`NVKR-E-INVALID-CATALOG`, exit 16) instead of reporting the format, so the format is not checked first.

**Affected Components**

`cmd/nvkr/resolve.go`: the `output` flag and the `Action` in `cmdResolve`, where the output `switch` lives. `selectCommandPresentation` in `cmd/nvkr/command_presentation.go`, which picks the TUI branch the check must run ahead of. `CodeUsage` in `pkg/nvkr/errors`, which maps to exit status 2. `planview.GraphOptions.Validate`, the local fail-closed precedent the issue names. Tests go in `cmd/nvkr`.

**Toolkit**

- Tool: Claude Code with my `CLAUDE.local.md` repo-memory file, used to read `resolve.go`, `command_presentation.go` and `planview.GraphOptions.Validate` next to the issue's pinned lines
- What it produced: the four answers above, with the file and function for each
- Difficulties: the issue's links pin commit `799db0c5` and `main` had moved to `95b61bc5`; I checked that `resolve.go` was unchanged between them before relying on the line numbers
- What changed: nothing; I used `95b61bc5` as the base for the repro and the fix

---

## Reproduction Process

**Environment Setup**

macOS on Apple Silicon (Darwin kernel 24.6.0, arm64), Go 1.27.3 (the repo pins this in `.go-version`), and upstream `dsx-ai-factory/nvkr` at `origin/main` commit `95b61bc50d7472b0a4e3b503607d6e7b833403c3`. The issue's links pin `799db0c5`; main has moved since, and the `resolve` behavior below is unchanged. I used a clean detached worktree of `origin/main`, with none of my branch changes, and built it with `GOTOOLCHAIN=local go build -o /tmp/nvkr-main-bin ./cmd/nvkr`. Below, `nvkr` means that binary. Challenge: my working branch already had a fix in progress, so I reproduced in a separate clean worktree to be sure I was seeing main's behavior, not mine.

**Steps to Reproduce**

From the repo root, with the binary built above:

1. Typo format on a valid catalog (the issue's command): `nvkr resolve --data inputs/catalogs/nvke/module-tags --component nvkectl --platform linux/amd64 --output jsno`
2. Pair the typo with a catalog that does not exist, to see whether the format is checked first: `nvkr resolve --data /nonexistent --component nvkectl --platform linux/amd64 --output jsno`
3. For contrast, a valid format with the same bad catalog: `nvkr resolve --data /nonexistent --component nvkectl --platform linux/amd64 --output ir`
4. Compare step 1's stdout with the default (`ir`) output using `cmp`.
5. Observed result: the typo is accepted as if it were valid (see evidence).

**Reproduction Evidence**

- **Commit showing reproduction:** none (no changes; this is `origin/main` at `95b61bc5`)
- **Screenshots/logs:**

  Step 1: exit `0`, stderr empty (0 bytes), stdout 82 lines. First lines and last line:
  ```
  nodes:
  - artifactPath: cli/bin/nvkectl
    builderType: oci-hermetic
  ...
  # buildDigest: sha256:999b5af0f81e18412b137705621ab279e3788a266e1b5a961d770045100af1ef
  ```
  Step 2:
  ```
  nvkr: [NVKR-E-INVALID-CATALOG] reading registry.yaml from dir:/nonexistent: no such file or directory
  exit=16
  ```
  Step 3 (valid format, same bad catalog): the identical line, `exit=16`.
  Step 4: `cmp` printed nothing and exited 0.
- **My findings:** The bug reproduces. `--output jsno` exits 0 and prints the full YAML graph, byte-identical to the default `ir` output, so a misspelled format silently falls back to the default. The format is also not checked before the catalog is loaded: step 2 fails on the catalog (exit 16), the same as a valid format would in step 3, where the issue expects a usage error (exit 2, `CodeUsage`) naming the five accepted formats and no graph.

**Toolkit**

- Tool: repro-check, run on `claim-draft.md` (my posted claim) and `repro-draft.md` against issue #398
- What it produced: accept on all eight checks. The first pass also left voice-guide notes: paste real output for steps 1, 3 and 4, name which binary `nvkr` means, and fix "macOS 24.6.0", which is the Darwin kernel version
- Difficulties: it had no way to check the repro itself, so it graded my claimed outputs, which I had to run myself; it also noted that my claim says "DCO sign-off" but the repo requires `-S` as well as `-s`
- What changed: I pasted the missing output, named the binary and corrected the OS line, and I will mention the signing requirement in my plan comment

---

## Solution Approach

**Analysis**

In `cmd/nvkr/resolve.go` at `95b61bc5`, the `output` flag (line 21) has the default `ir` and a usage string listing five formats, but no code compares the value to that list. The `Action` first calls `selectCommandPresentation(c)` (line 25), then resolves the graph (around line 31), then branches on `view.tui` (line 34), and only then runs `switch c.String("output")` (line 37). That switch handles `canonical`, `nodes`, `digest` and `components` explicitly, and its `default:` branch (line 77) calls `g.MarshalDeterministic()` and prints YAML. So `ir` and every unknown string take the same path. Two things follow: a typo is indistinguishable from the default, and the value is not looked at until after resolution and after the TUI decision.

**Proposed Solution**

Add one local function, `validateResolveOutput(format string) error`, in `resolve.go`. It accepts exactly `ir`, `canonical`, `nodes`, `digest` and `components`, and returns `e.New(e.CodeUsage, ...)` listing them for anything else. Call it first in the `Action`, before `selectCommandPresentation`, resolution and the TUI branch. It follows `planview.GraphOptions.Validate` as the issue asks. I am not changing the output `switch`, the flag's default or usage text, any rendering code, or other commands; I am not adding a shared validation helper; and #396 (which components `resolve` displays) stays out, even though it touches the same file.

**Implementation Plan**

1. In `cmd/nvkr/resolve.go`, add `validateResolveOutput` and call it as the first statement of the `resolve` action.
2. Add `cmd/nvkr/resolve_output_test.go` with three tests: unknown, empty and wrongly cased values return `NVKR-E-USAGE` with the accepted formats and no stdout; an invalid format with no catalog still returns the format error; and an invalid format cannot reach the TUI viewer. Rely on the existing `TestResolveOutputFormsShareBuildIdentity` for "all valid values" and unchanged bytes.
3. Run the new tests on `main` to confirm they fail, then on my branch to confirm they pass; re-run the issue's repro before and after.
4. Run `make check`, `make build`, `make notices-check` and `make actionlint`, and compare any failure against untouched `main`.
5. Commit with `-S -s`, one commit for the fix and tests, and open the PR using the repo's template, with `Closes: #398`.

Deviation, recorded: I added a second commit after opening the PR, `docs(cli): explain resolve output validation in comments`, which only adds comments (where the accepted formats are documented, that only `ir` reaches the `default:` branch, and why the tests are shaped as they are). It changed no behavior.

**Toolkit**

- Tool: plan-check was not run on this plan. I wrote this section from the built change, after the PR had merged, so it records the plan I followed rather than a plan I had graded first
- What it produced: nothing for this block; no `plan.md`, `plan-comment.md` or plan-check verdict exists for #398
- Difficulties: I opened the PR before the plan-check step; the tool was unused, not unhelpful
- What changed: I will run plan-check before building on my next contribution, and the Unit 7 entry above says the same about the ship-gate

---

## Testing Strategy

**Repro, before and after.** The issue's command: `nvkr resolve --data inputs/catalogs/nvke/module-tags --component nvkectl --platform linux/amd64 --output jsno`.

Before (`origin/main` at `95b61bc5`): exit 0, stderr empty, 82 lines of YAML on stdout, byte-identical to the default `ir` output.

After (my branch):
```
exit status: 2
stdout bytes: 0
nvkr: [NVKR-E-USAGE] unknown resolve output "jsno" (use ir, canonical, nodes, digest, or components)
```

**New tests fail without the fix and pass with it.** Command: `go test ./cmd/nvkr/ -run 'TestResolveOutputRejectsUnknownFormats|TestResolveOutputFormatErrorWinsOverMissingCatalog|TestResolveOutputInvalidFormatCannotTakeTheTUIBranch' -v`.

On `main`: all three fail (5 failing cases), for example `error = <nil>, want NVKR-E-USAGE` for `output=jsno`, `output=` and `output=Digest`, and `invalid output format reached the viewer` for the TUI test.

On my branch: all three pass (`ok  github.com/dsx-ai-factory/nvkr/cmd/nvkr  0.301s`).

**Valid formats unchanged.** The existing `TestResolveOutputFormsShareBuildIdentity` runs all five formats through the new check and still passes, so I did not duplicate it. The maintainer's review also compared base and head output byte for byte for the five formats on `module-tags`: identical, exit 0.

**Repo checks.** On my branch, `make check` showed formatting ok, `go vet` clean, `0 issues.` from the linter, shellcheck clean, 36 Python tests OK, and `ok .../cmd/nvkr 98.136s`. `make notices-check` printed `THIRD_PARTY_NOTICES.md is up to date`, and `make actionlint` printed no findings. The Go test step of `make check` still fails in four packages (`buildgraph`, `catalogmaintenance`, `factory`, `pipeline`) on my macOS machine, and fails the same way on untouched `main`. The failing tests are `TestAICRNVKEBundleFlags`, `TestClosureListsTheMaintainedBundles`, `TestMaintainedGitHubBundles`, `TestOVNLockOnlyPropagation`, both `TestPackerDiagnostic*` tests and `TestProjectionTransportsMatchInBinaryGenerator`, with causes such as `NVKR-E-PIPELINE-DRIFT` and `permission denied`. I did not diagnose them, and I said in the PR that I could not get `make check` green. I did not run `make ci-check` as a whole, `make docs-check`, or the E2E workflow.

**Toolkit**

- Tool: the repo's own `make check`, `make build`, `make notices-check`, `make actionlint` and `go test`, run by hand. I did not write `test_evidence.md`, and the ship-gate did not run these for me
- What it produced: the before and after outputs above, and the list of failures that also occur on `main`
- Difficulties: `make check` cannot go green on my machine; the same tests fail on `main`, and CONTRIBUTING.md says workflow drift needs read access to private NVKE source, which I did not confirm as the cause
- What changed: I ran the failing packages on untouched `main` to separate those failures from my change, and reported them in the PR instead of hiding them; my ship manifest still needs an "Excluded" row for them

---

## Implementation Notes

Dates are my local time (UTC-8); the pull request shows the same events in UTC.

**2026-11-07**

Claimed #398 at 18:32 after the voice guard stopped my first two drafts. Reproduced on a clean detached worktree of `origin/main` (`95b61bc5`), not on my working branch, so I was seeing main's behavior: `--output jsno` exits 0 with YAML, and with a bad catalog it fails on the catalog first. Wrote `validateResolveOutput` and called it first in the action, before `selectCommandPresentation`. I put it before presentation, not just before the `switch`, because the issue requires an invalid format not to reach the TUI branch.

Wrote three tests in `resolve_output_test.go`. I ran them against `main` to prove they fail, which they did (5 failing cases). One thing I got wrong: my "format wins over missing catalog" test omits `--component` and `--platform`, so on `main` it fails on the platform check, not the catalog. The maintainer later pointed this out; it pins a stricter ordering than the issue asks, so I left it.

Committed the fix at 19:44 (`54cd9ffe`) and opened PR #442 at about 19:48. The voice guard stopped my first `gh pr create` with "Paste test output, do not summarize it"; I pasted the real before and after output and it passed. I also wrote up that `make check` fails in four packages on `main` as well, instead of leaving it out.

Added a comment-only commit at 19:57 (`05b6ddc6`) noting where the formats are documented; CodeRabbit had flagged docstring coverage on the PR.

**2026-11-08**

Maintainer njhensley approved the PR (23:05 local). Their one finding: both of my commits ended with `Co-Authored-By: Claude Sonnet 5.5`, which nvkr's AGENTS.md forbids. I did not check the commit messages against AGENTS.md before opening the PR. Their suggested fix was a rebase plus force-push, or dropping the line at squash time.

**2026-11-09**

njhensley squash-merged #442 as `27beccbe` at 07:33. The squash message carries only my `Signed-off-by`, so the trailer did not reach `main`. I did not rewrite my commits.

**Toolkit**

- Tool: Claude Code with my `CLAUDE.local.md` memory file wrote the fix and tests, and the voice guard's hook on `gh pr create` checked my PR description
- What it produced: two commits, three tests that fail on `main` and pass on the branch, and a PR description that the guard held until it carried real output
- Difficulties: the commits got the agent `Co-Authored-By` trailer that this repo forbids, and nothing checked commit messages against AGENTS.md before the PR opened; the voice guard reads the PR body, not the commits
- What changed: I would check the repo's attribution rule before every commit and add a commit-trailer check to my ship manifest

---

## Pull Request

**PR Link:** https://github.com/dsx-ai-factory/nvkr/pull/442

**PR Description:** Title: `fix(cli): reject unknown resolve output formats`. It follows the repo's PR template. Summary: `resolve --output` advertised five formats but sent every unknown value to the default YAML renderer, so a misspelling exited 0 and printed the wrong format; it now returns `CodeUsage` naming the five accepted formats. Motivation: a typo in a script silently produced YAML where JSON was expected, and CONTRIBUTING.md requires an unknown value to error with a coded, distinct exit status. `Closes: #398`; related to #396, which touches the same file for a different purpose. Implementation notes: `validateResolveOutput` runs first in the action, before presentation selection, resolution and the TUI branch, and the output `switch` is unchanged. Testing: the issue's repro before (exit 0, full YAML) and after (exit 2, no stdout, `NVKR-E-USAGE`), the three new tests failing on `main` and passing on the branch, the `make` checks, and a section "What I could not get green" listing the four packages that fail on untouched `main` too. Risk: low; a script that relied on the YAML fallback for a misspelled `--output` will now exit 2, which is intended.

**Maintainer Feedback:**

- 2026-11-08: njhensley approved. Praised the early exit before catalog reads and the TUI, and pinning the existing formats byte for byte. Raised one minor finding: both commits carried an agent `Co-Authored-By` trailer that AGENTS.md forbids. Also noted CI's Merge Gate was `action_required` until fork workflows were approved.
- 2026-11-09: I did not rewrite the commits. njhensley squash-merged and the squash message has only my `Signed-off-by`, so the trailer did not reach `main`.

**Status:** Merged (squash commit `27beccbe`, 2026-11-09)

**Toolkit**

- Tool: voice-guard, which holds `gh pr create` until the description passes my Upstream rules; the ship-gate did not run on this PR
- What it produced: one stop on 2026-11-07 at 19:47 ("Paste test output, do not summarize it"), then a pass at 19:48 once the description pasted the real before and after output
- Difficulties: the guard reads the description and never the commit messages, so it could not see the `Co-Authored-By` trailer the reviewer found; I also opened the PR with no ship-gate run, so `make check` failures on `main` were explained by hand
- What changed: I would check the repo's AGENTS.md attribution rule before committing and add a commit-trailer check to the ship manifest

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how a fail-closed Go CLI admits input: `validateResolveOutput` runs first in the `resolve` action and returns `CodeUsage` (exit 2) with the accepted values, so a bad value errors before a catalog is read or the TUI can open. I learned to write a test for the branch an invalid value must not reach, by injecting a fake viewer and asserting it was never called. I learned to prove a new test is real by running it on untouched `main` first (5 failing cases) and then on my branch. I also learned to reproduce in a clean detached worktree of `origin/main`, and the repo's `-S -s` signing and DCO rule.

**Challenges Overcome**

Finding an issue nobody had taken was hard: most of my searches ended in issues already claimed through open PRs or `/claim` comments, and adding `-linked:pr` to the queries fixed that. The voice guard stopped my first two claim drafts and my first `gh pr create`, and I fixed each by claiming only a concrete first step and pasting real test output. `make check` could not go green on my machine, because four packages fail the same way on untouched `main`. I ran them on `main` to show they were not mine and reported them in the PR instead of hiding them.

**What I'd Do Differently Next Time**

I would check the repo's AGENTS.md attribution rule before the first commit. Both of my commits carried a `Co-Authored-By` trailer that the repo forbids, and the reviewer caught it, not a tool. I would run plan-check before building and install the ship-gate before opening the PR, instead of after it merged. I would also keep my local Go version matched to `.go-version` and fill in my monitor playbook before I need it.

---

## Resources Used

- nvkr CONTRIBUTING.md, for the `make check` and `make build` pre-PR checks, the `-S -s` signing rule and the PR checklist: https://github.com/dsx-ai-factory/nvkr/blob/main/CONTRIBUTING.md
- nvkr AGENTS.md, which forbids agent attribution in commits, the rule my commits broke: https://github.com/dsx-ai-factory/nvkr/blob/main/AGENTS.md
- Issue #398, which named `cmd/nvkr/resolve.go` and the `planview.GraphOptions.Validate` precedent: https://github.com/dsx-ai-factory/nvkr/issues/398
- The review thread on PR #442, where njhensley approved and found the trailer: https://github.com/dsx-ai-factory/nvkr/pull/442
