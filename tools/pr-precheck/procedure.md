# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

1. Read `rubric.md` and `references/evidence-guide.md` first. List each check and the verdict rule. In live mode, read `scope.md` before that.

2. Read the repo facts block before the plan. Note the PR template asks, required checks (pre-commit, tests, whatsnew entry), and any AI/disclosure policy. These are the standard the PR is held to.

3. Read the issue and the thread highlights. Note the reported symptom and any maintainer replies. This is context only; do not grade it.

4. Read the plan context before the diff. Note: (a) the in-scope files and the out-of-scope lines, (b) any deviation notes, (c) the test plan's observable outcome. Reading the plan first fixes the boundary before the diff can anchor you.

5. Read the diff. List every changed file and, per file, a one-line summary of what its hunks do. Flag any file not in the plan's scope and any hunk that looks like debris (formatting churn, stray files, unrelated edits).

6. Read the test evidence. Note what was run, what output is pasted or linked, and whether it re-runs the plan's test-plan outcome. Mark "tested locally" with nothing observable as non-observable.

7. Read the PR title and description last. Note every claim: what changed, fidelity to plan, tests run, disclosed deviations or gaps, the linked issue, and template fields filled. These get compared against the diff and evidence, so read them after both.

8. Do not grade yet. The three side-by-side checks (diff vs plan, evidence vs test plan, description vs diff) each need both sides in view.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

1. plan-fidelity: From Plan context, list the files named after "Files:" and copy the "Not in scope" line. From Candidate PR -> Diff, list every changed file and one line on what each hunk does. Record any file or behaviour in the diff that the plan does not name, and any plan step with no matching hunk. Then list each fidelity claim in the Description ("rendering unchanged", "only removes X") and mark whether the diff supports it. 

2. test-evidence: From Plan context, copy the test plan's observable outcomes (calib-01: clean configparser parse, gruvmax-fang still renders). From Candidate PR -> Test evidence, record for each outcome whether pasted output exists (command plus result) or only a claim. Then, from Repo facts, list the required checks and record whether the evidence shows each one run. Mark "tested locally" with no output as non-observable.

3. diff-quality: From Diff and Commits, record every hunk that does not serve the change (stray files, whitespace churn, commented-out code, unrelated edits). If none, write "none".

4. standards-and-comms: From Repo facts -> 'pull requests:' and CONTRIBUTING lines, list each ask (template fields, issue link, size, disclosure). From Title and Description, record whether each is met: title says what changed, issue number linked, template sections filled with real content.

5. demo (preferred): record whether the change is user-visible and whether a screenshot is attached.

6. If a section the check needs is missing from the package, write "absent". Do not fill the gap from outside knowledge. In eval mode use only the bundle.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade checks in rubric order: plan-fidelity, test-evidence, diff-quality, standards-and-comms, then demo. Use only the evidence recorded in Evidence gathering; re-read the package only to resolve a specific doubt.

2. For each check, compare the recorded evidence with the pass condition in `rubric.md` and grade `pass` or `fail`. Apply the condition as written, even if the result feels wrong.

3. The side-by-side checks need both sides recorded. Grade plan-fidelity only with the plan's scope and the diff's file list both in hand, test-evidence only with the test plan's outcomes and the pasted evidence both in hand. If one side is absent from the package, the check is `unclear`.

4. A disclosed shortfall is not a failure. If the diff deviates from the plan or the testing has a gap, and the description names it accurately, grade `pass`. Grade `fail` when the shortfall is undisclosed, or the disclosure is contradicted by the diff or evidence.

5. If the evidence a check needs is absent from the package, grade `unclear`. Do not guess and do not fail it for being absent. A check you did not look for evidence on is not `unclear`; go back and look.

6. If the evidence is present but ambiguous, grade `fail` when it contradicts the pass condition and `unclear` only when it neither meets nor contradicts it.

7. Write one line per check: the fact or quote that decided it. For plan-fidelity, name the file or claim that falls outside the plan, or state that none does. For test-evidence, name the test-plan outcome with no observable evidence, or state that all have it.

8. Ignore polish. Length, headings, tone, and commit count do not move any grade.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Apply the verdict rule in `rubric.md`: `accept` only if all four required checks (plan-fidelity, test-evidence, diff-quality, standards-and-comms) are `pass`.

2. A required check graded `fail` or `unclear` gives `reject`. An `unclear` counts as a fail because an unverifiable claim fails. `demo` is preferred and never changes the verdict.

3. When more than one required check fails, the deciding check is the first failing required check in rubric order. Quote it first in the summary: the file, claim, or missing evidence that decided it, in the words of the package.

4. In the output, list every check with its grade and its one-line evidence, then state the verdict. Report any voice-guide breaks in live mode, without changing the verdict.

5. End with the JSON block from `SKILL.md`, with `accept` or `reject` as the verdict, as the last thing in the reply. Add no third verdict and nothing after the block.
