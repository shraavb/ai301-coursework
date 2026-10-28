# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis fits evidence | The plan's stated cause, read against every step and timing in the repro evidence block (including any step that removes a suspect from the loop). Also note whether the cause is the plan's own or only repeated from a thread comment. | Passes if the stated cause explains each repro observation and no repro step contradicts it (e.g. the bug still occurs with the named component bypassed or unchanged). Fails if any repro step contradicts the cause, or the cause is only quoted from the thread with no tie to the repro. Unclear if the plan states no cause. | required |
| Change is bounded | The plan's scope statement and Changes list. | Passes if the plan names each file or area it will change and says what it will not touch, and every listed change serves the stated cause. Fails if a listed change is unrelated to the cause, or the files are unnamed. | required |
| Stranger can start | The plan's Changes list, read as if by someone who has never seen the repo. | Passes if a stranger could open the named file(s) and begin the first change without asking the author anything (what to change, where, and roughly how). Fails if it says only "look into" or "poke around". | required |
| Test proves the fix | The plan's test plan, read against the repro evidence's steps. | Passes if the test names an observable outcome for the fix itself, taken from the repro (e.g. the repro step that failed now shows the expected result). Fails if it is only "run the test suite" or checks something other than the reported symptom. | required |
| Unknowns stated | The plan and plan comment, read against the repro evidence and thread highlights. | Passes if anything the plan has not verified is flagged as unverified, and nothing is stated as fact that the evidence does not show. Fails if a guess is presented as certain. | preferred |
| Comment matches thread and repo | The plan comment, read against thread highlights and the repo-facts block (contribution policy, bug-report template). | Passes if the comment does not ignore a maintainer reply or a stated repo rule (e.g. review limits, AI policy) that bears on posting the plan. | preferred |

## Verdict rule

Accept only if every required check passes. Any required check graded
fail holds the package (reject). A required check graded unclear also
holds the package: a plan that cannot be verified from the package is
not ready to build from. Preferred checks never change the verdict;
note their grades in the summary.
