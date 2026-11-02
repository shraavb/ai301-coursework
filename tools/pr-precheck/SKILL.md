---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

This tool answers the quality of a PR by checking the pull request against the plan it intended on implementing and the issue the plan belongs to. It contains the pull request, the plan, the issue, branch, skill, procedure, voice guide, scope.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

Live mode involves checking the student's own submission before it is released. The inputs include: 'plan.md', the diff on the branch, PR title and description draft, test evidence, the issue. On the other hand, the house chain reads the house plan, the house repro pack. The live mode will gather evidence from the real repo. 

In eval mode, the input is the bundle text. Eval mode will always grade a complete package.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, the tool reads 'scope.md' first. The scope describes where the student's PR must live and the house rules of that environment. The tool will not grade work outside this scope. If scope.md is unfilled, it stops without grading a tells the student to fill the 'Repo:' line in 'scope.md' with their section's Path Review repo.

It will never guess a scope. In eval mode, scope.md is ignored completely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, the tool reads 'voice-guide.md', it holds the outgoing PR text, the title and the description, against those rules, reporting any rule the draft breaks in the summary.

The voice guide never changes the verdict by itself unless the rubric has a check that reads it.

In eval mode, 'voice-guide.md' is ignored entirely: voice is personal and carries no gold labels. Universal communication-quality checks live in the rubric.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

The rubric.md defines the check and verdict rule, not the run. references/evidence-guide.md maps to where each evidence family lives. procedure.md is executed as written. The tool will report gaps in procedure.md instead of silently inventing steps. A tool with no rubric content or no procedure content refuses to grade and says so, by design. 

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict accept means submit, reject means hold. Unclear defaults to fail. If the rule is silent, an unverifiable claim is a failing one. A PR you cannot verify from the package is a PR that is not ready to submit. 

The tool should end its reply with the fenced JSON block below, valid and last, with nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

1. Evidence first: First the check needs to be graded, naming the fact or quote that decided it. Grading needs to cite evidence.

2. Every check reads the artifact itself against the plan, the issue and the stated standards, never the formatting.

3. If a check passes by its stated condition but feels wrong it still passes. In this case, the fix belongs in the rubric not the run.

4. The procedure decides how, not the run. The tool follows 'procedure.md' as written and reports its gaps instead of silently inventing steps.

5. Unclear defaults to fail. Treat 'unclear' as the rubric's verdict rule directs. Where the rule is silent and unverifiable claim is a failing one. A PR you cannot verify from the package is a PR that is not ready to submit. 


