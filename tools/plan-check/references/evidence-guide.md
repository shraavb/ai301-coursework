# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

**Where it lives.**
- Eval bundle: the plan's stated cause is in `## Candidate plan`, under "Diagnosis" (or the line starting "Cause:" in a terse plan). The behavior it must explain is in `## Repro evidence`: the numbered steps, timings, and the "Actual" line. Thread claims are in `## Thread highlights`.
- Live: the plan's Diagnosis section; the student's posted repro comment on the issue; the issue thread.

**What good looks like.** The stated cause explains every repro step, including steps that remove a suspect (a component bypassed, a flag off, a variable fixed). Bad: the cause is quoted from a thread comment ("as identified in this thread") and a repro step shows the bug persisting without that component. A cause that is the plan's own and fits each step is good, even if it is one sentence.


## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

**Where it lives.** The "Scope" section of `## Candidate plan` (or the "In:" / "Out:" lines of a terse plan), plus the file names in the "Changes" list (or "Change:" line).

**What good looks like.** Each file or area to be changed is named, a not-in-scope line says what stays untouched, and every listed change serves the stated cause. One bounded change looks like "one callback in one named file; push-status computation untouched". A drive-by rewrite adds refactors, extra features, or files the cause does not need.


## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

**Where it lives.** The "Changes" list of `## Candidate plan` (or the "Change:" line). Read the first item as a newcomer would.

**What good looks like.** A stranger could open the named file and start the first change without asking the author: it says what to change, where, and roughly how. Bad: "poke around the editor code", "look into the pager", no file named. Short is fine if the file and the action are named.


## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

**Where it lives.** The "Test plan" section of `## Candidate plan` (or the "Test:" line), read against the numbered steps in `## Repro evidence`.

**What good looks like.** The test re-runs the repro step that showed the failure and names the observable result that proves the fix (e.g. "at step 3 the color must flip without leaving the view"). Bad: "run the full test suite", or a test that checks something other than the reported symptom. A manual check is decisive if it names the step and expected outcome.


## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

**Where it lives.** Sentences in `## Candidate plan` and `## Candidate plan comment` that flag unverified items (risks, "I haven't confirmed", assumptions), and sentences stated as fact. Compare both with `## Repro evidence`. Live: a deviation note recorded in `plan.md`.

**What good looks like.** Anything the repro evidence does not show is marked as unverified, and nothing is stated as certain that the evidence does not support. False confidence: a thread guess written as settled cause. A plan with no unknowns listed is fine if every claim is backed by the repro.


## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

**Where it lives.** `## Candidate plan comment`, read against `## Thread highlights` (maintainer replies, especially COLLABORATOR or OWNER) and the `## Repo facts` block (bug template, contribution policy, AI-disclosure rules).

**What good looks like.** The comment engages what the thread and the repo ask: it does not ignore a maintainer's reply, and it respects stated policy (e.g. a review-bandwidth note, an AI disclosure rule). Bad: boilerplate that would fit any issue, or a comment that contradicts a maintainer's reply or the policy.

