# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

**Where it lives.**
- Eval bundle: `## Plan context` holds the plan's scope (the "Files:" list, the "Not in scope" line) and any deviation notes. `## Candidate PR` → `### Diff` and `### Commits` show what changed. `### Description` holds the fidelity claims ("rendering unchanged", "only removes X").
- Live: your `plan.md` and its deviation notes; `git diff main...HEAD`; your draft PR description.

**What good looks like.** Every changed file and behavior in the diff falls inside the plan's stated scope, or a deviation note in the description names it. Every plan step has a matching hunk, or the description says it was dropped. Silent drift is the diff doing more than the plan (an unnamed file or behavior) or less (a plan step missing with no note). A description that claims fidelity the diff contradicts is drift too.


## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

**Where it lives.**
- Eval bundle: `### Test evidence` under `## Candidate PR`, read against the test plan in `## Plan context` and the required checks in `## Repo facts` (the `pull requests:` line).
- Live: your captured terminal output, CI link, and the repo's PR template and CONTRIBUTING.md for required checks.

**What good looks like.** Each outcome the test plan names has pasted output (command plus result), a log, or a CI link showing it, ideally before and after. The repo's own required checks (pre-commit, test suite) show as run, or the gap is disclosed. "Tested locally", "tests pass", or output for something other than the test plan's outcome is not observable.


## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

**Where it lives.** Eval bundle: `### Diff` and `### Commits` under `## Candidate PR`. Live: `git diff main...HEAD` and `git log main..HEAD`.

**What good looks like.** Every hunk serves the change and a reviewer can see the fix at a glance. Debris tells: stray or generated files, debug prints, commented-out blocks, dead code, whitespace or formatting churn, unrelated drive-by edits, an unrelated file in the file list.


## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

**Where it lives.**
- Eval bundle: `## Repo facts` (`pull requests:` template or contribution asks, `contribution policy` including any AI-use disclosure) read against `### Title` and `### Description` under `## Candidate PR`. Thread maintainer direction is in `## Thread highlights`.
- Live: the repo's `.github/PULL_REQUEST_TEMPLATE`, CONTRIBUTING.md, and your draft title and description.

**What good looks like.** Each template section or stated ask is filled with real content, not boilerplate. Required disclosure is present. The title says what changed and the description links the issue. Any explicit maintainer direction in the thread is engaged. If the repo has no template, its stated asks (e.g. "stay small, reference the issue") are met. Whether the description's claims match the diff is plan fidelity, not this check.

