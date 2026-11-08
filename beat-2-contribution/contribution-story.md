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
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

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

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
