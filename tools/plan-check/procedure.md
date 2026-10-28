# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Read `rubric.md` and `references/evidence-guide.md` first. List each check and the verdict rule. In live mode, read `scope.md` before that.
2. Read the repro evidence block before the plan. Note every step, timing, or observation, and in particular any step that removes a suspect from the loop (a component bypassed, a flag turned off, a variable held fixed). This is the ground truth the plan's cause must fit.
3. Read the issue and the thread highlights. Note which causes commenters claim, and who claims them. A thread claim is a suspect, not a fact.
4. Read the candidate plan in order: Diagnosis, Scope, Changes, Test plan. Note the stated cause, the files named, what is declared out of scope, and the test's observable outcome.
5. Read the candidate plan comment last. Note what it claims and whether it mentions repo rules from the repo-facts block (contribution policy, bug template).
6. Do not grade yet. Reading all parts first matters because the diagnosis check needs the repro evidence in mind before the plan's cause is read.


## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

1. Diagnosis fits evidence: copy the plan's stated cause in one line. Then go through the repro steps one at a time and write, for each, whether the cause explains it. If a step shows the bug persisting with the named component removed or unchanged, record that step's number and quote it. If the cause is quoted from the thread (e.g. "as identified in this thread"), record that.
2. Change is bounded: pull the plan's in-scope and out-of-scope lines and the file names from the Changes list. Record each file named. Record any change in the list that does not serve the stated cause.
3. Stranger can start: pull the first item in the Changes list. Record whether it names a file, what to change in it, and how, in words a newcomer could act on.
4. Test proves the fix: pull the test plan. Find the repro step that showed the failure and record whether the test re-runs it and names the expected result. Record "run the suite" or similar as non-observable.
5. Unknowns stated: pull any sentence in the plan or comment that says something is unverified or uncertain. Also pull any sentence stated as fact that the repro evidence does not show.
6. Comment matches thread and repo: pull the maintainer replies from the thread highlights and the policy line from the repo-facts block. Record whether the comment engages them.
7. If a piece of evidence is not in the package, write "absent" for it. Do not fill the gap from outside knowledge. In eval mode use only the bundle.


## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade checks in rubric order, diagnosis first. Use only the evidence recorded in the previous stage; re-read a part of the package only to resolve a specific doubt.
2. For each check, compare the recorded evidence with the check's pass condition in `rubric.md` and grade `pass` or `fail`. Apply the condition as written, even if the result feels wrong.
3. If the evidence a check needs is absent from the package, grade `unclear`. Do not guess and do not fail it for being absent. A check you did not look for evidence on is not `unclear`; go back and look.
4. If the evidence is present but ambiguous, grade `fail` when it contradicts the pass condition and `unclear` only when it neither meets nor contradicts it.
5. Write one line per check: the fact or quote that decided it. For the diagnosis check, name the repro step number that contradicts the cause, or state that none does.
6. Ignore polish. Length, headings, and confident tone do not move any grade.


## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Apply the verdict rule in `rubric.md`: `accept` only if every required check is `pass`.
2. A required check graded `fail` or `unclear` gives `reject`. Preferred checks never change the verdict.
3. In the output, list every check with its grade and evidence line. Put the deciding check first in the summary when the verdict is `reject`, with its quote. Name the first failing required check in rubric order.
4. Emit the JSON block with `accept` or `reject` as the last thing in the output. Do not add a third verdict.

