# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/275

**Branch**

`fix/68-empty-keyword-index`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. 20/20 (graded `../skill` before `procedure.md` was finished: only Read order and Evidence gathering were filled, so the grader leaned on `rubric.md` and `evidence-guide.md`)
2. 20/20 (final full run of the installed copy with `procedure.md` complete, saved to `eval-run.txt`: "agreement: 20/20 scored items  (bar: 18/20: PASS)")

**Package analysis**

**pkg-01** (pandas-dev/pandas#66657, category standards-wall). My rubric decided `reject`; the gold label said `reject`, so we agree.

The staff note on the gold label reads: "solid C-parser fix with decisive ASAN evidence, but pandas' required PR checklist is visibly ignored: no closes #xxxx, no checklist, and the stated whatsnew entry is absent from the diff". My rubric reads it that way because the repo-facts block says "the PR template is a required checklist: closes #xxxx with the issue number, tests added and passed, all code checks passed (pre-commit), type annotations added to new arguments, and an entry in the latest doc/source/whatsnew/vX.X.X.rst". The candidate description has a title and a summary paragraph but no `Closes #`, no checklist, and the diff has no whatsnew file. `standards-and-comms` is required and its pass condition is "The repo's template is filled in as asked", so it fails and one required fail rejects. The strong plan-fidelity and test-evidence results (ASAN before/after, regression test) do not offset it, because the verdict rule needs all four required checks to pass.

**Check rationale**

> | standards-and-comms | The repo-facts block's template and disclosure asks, read against the PR title and description | The repo's template is filled in as asked and any required disclosures are present. The title says what changed and the description links the issue. | required |

It reads this way because the pass condition judges the outcome (is each ask the repo states actually met, and does the description link the issue) and not the shape of the write-up. A check on length or headings would let a boilerplate-filled template pass, which is what the standards-wall packages do. I tied it to the repo-facts block so the standard is the repo's own stated asks, not my taste, and I put "the title says what changed" in it so a title like "Fix bug" fails. I rejected a stricter reading that required every PR to carry a disclosure section, because a repo with no AI policy (calib-01 delta, and Path Review's template) has no disclosure to make.

**Trade-offs**

Nothing changed in the rubric between my two full runs: both scored 20/20 with the same `rubric.md`; only `procedure.md` went from half-filled to complete, so no `--only` canary re-run was needed.

What the check gives up: it judges only what the package contains. A PR that pastes plausible but invented test output, or a description that claims a template section was filled when it was filled with boilerplate that reads as real content, will pass, because the tool cannot run the code or see the repo beyond the bundle. I accept that miss. The rubric also passes a PR that honestly discloses a shortfall (a test gap or a deviation from the plan), which trades a stricter "hold anything incomplete" rule for the honest-outcome rule.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
