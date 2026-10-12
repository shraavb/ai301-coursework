# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
## Grading — 3 candidates, repo in scope (`codepath/pathreview-ai301-fa26-s3`), classroom house rule applied

**Repo facts (shared):** not archived · last push 2026-09-16 (~25d ago) · last 5 commits all human (Andrew Burke) within 180d · no releases, push recency alone passes · `docs/CONTRIBUTING.md` exists, no AI policy mentioned anywhere (silence = pass) · no maintainer (OWNER/MEMBER/COLLABORATOR) replies found in any of these 3 threads or a 5-issue sample → maintainer-responsive fails (preferred only, no verdict impact).

**no-active-claim judgment call:** all three issues have zero assignees but many open student PRs linked (classmates, not maintainer/outsiders) plus many claim comments. Scope's house rule explicitly says claim comments from classmates don't block, reasoning that "course credit attaches to the PR you open, not whether it merges, so a shared issue costs nobody anything" — that same reasoning covers the inevitable open classmate PRs this produces. Graded pass under that reading; flagging it since rubric text names open PRs literally.

#### #68 — BM25 `index()` ZeroDivisionError on empty corpus
repo-not-archived: pass · maintainer-commits-recent: pass · repo-in-use: pass · scope-bounded: pass (one guard clause, named files+test) · no-active-claim: pass (see above) · ai-policy-allows: pass · good-first-issue-signal: pass (labeled + maintainer-opened) · clear-acceptance: pass · maintainer-responsive: fail
**Verdict: accept**

#### #72 — `verify_password` doesn't fail closed on malformed hash
Same required-check results as #68, all pass. Smaller estimated effort (1-2h vs 2-4h), equally bounded single-fix + xfail test.
**Verdict: accept**

#### #73 — `.env.example`/README OpenRouter key mismatch
Required checks: same pattern, all pass (docs-only but single bounded deliverable, not an umbrella).
**Verdict: accept**

### Ranked (fit: wants Python, small bounded logic bugs with tests; docs-only is least preferred)
1. **#72** — Python logic bug, smallest estimated effort (1-2h), has xfail test to un-mark, matches "small bounded logic bug" best.
2. **#68** — same shape, slightly bigger (2-4h).
3. **#73** — docs/config-only, explicitly the profile's least-preferred type; still accepted but ranks last.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false"},
      {"name": "maintainer-commits-recent", "grade": "pass", "evidence": "last 5 commits all by Andrew Burke, newest 2026-09-16, within 180d of 2026-10-11"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "last push 2026-09-16, ~25 days before capture"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "body names one fix (fail closed on UnknownHashError), two files, 1-2h estimate"},
      {"name": "no-active-claim", "grade": "pass", "evidence": "assignees: []; open PRs (#78,#91,#92,#107...) and claim comments are all classmates, covered by scope.md house rule reasoning"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use policy; silence passes"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "no OWNER/MEMBER/COLLABORATOR comment in this thread or in 5-issue sample"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "labels: bug, good first issue, api, tier-1; opened by maintainer Aburke225"},
      {"name": "clear-acceptance", "grade": "pass", "evidence": "body states expected behavior: 'should fail closed (return False), not raise'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false"},
      {"name": "maintainer-commits-recent", "grade": "pass", "evidence": "last 5 commits all by Andrew Burke, newest 2026-09-16, within 180d"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "last push 2026-09-16, ~25 days before capture"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "body names one guard-clause fix, two files, 2-4h estimate"},
      {"name": "no-active-claim", "grade": "pass", "evidence": "assignees: []; open PRs (#74,#83,#89...) and ~15 claim comments are all classmates, covered by house rule reasoning"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use policy; silence passes"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "no OWNER/MEMBER/COLLABORATOR comment in this thread or in 5-issue sample"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "labels: bug, good first issue, rag, tier-1; opened by maintainer Aburke225"},
      {"name": "clear-acceptance", "grade": "pass", "evidence": "body states expected behavior and names the xfail test (manifest H-01) to un-mark"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "isArchived: false"},
      {"name": "maintainer-commits-recent", "grade": "pass", "evidence": "last 5 commits all by Andrew Burke, newest 2026-09-16, within 180d"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "last push 2026-09-16, ~25 days before capture"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "body asks to align 2 files (README.md, .env.example), single deliverable, 1-2h estimate"},
      {"name": "no-active-claim", "grade": "pass", "evidence": "assignees: []; open PRs (#85,#112,#137...) and claim comments are all classmates, covered by house rule reasoning"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use policy; silence passes"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "no OWNER/MEMBER/COLLABORATOR comment in this thread or in 5-issue sample"},
      {"name": "good-first-issue-signal", "grade": "pass", "evidence": "labels: bug, good first issue, docs, tier-1; opened by maintainer Aburke225"},
      {"name": "clear-acceptance", "grade": "pass", "evidence": "body states concrete done-condition: make README.md and .env.example agree"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run: 17/20. Misses: issue-01, issue-04, issue-19 (gold accept, rubric reject), all on `scope-bounded`.
2. Partial `--only` re-grade of those three to read the grader's reasons: 0/3 (rubric unchanged).
3. Partial `--only` after loosening `scope-bounded` (issue-01, 04, 19, 05, 10, 15, 20): 5/7. issue-15 and issue-20 flipped to accept.
4. Partial `--only` after adding abandoned-PR/long-thread and unspecified-input fail conditions (issue-01, 04, 05, 09, 10, 15, 19, 20): 8/8.
5. Full run: 20/20 (bar 18/20: PASS). This is the committed `eval-run.txt`.

**Issue analysis**

issue-20 (excalidraw/excalidraw#11811, "Add company logo shape to the toolbar"). Rubric decision: reject. Gold label: reject (category scope, "one-line feature wish with no spec and a product decision hiding inside").

Reasoning: after I loosened `scope-bounded` to stop rejecting legitimate multi-part issues, the grader accepted issue-20 because it reads as one deliverable with a "Success looks like" line. The bundle says "Logo asset TBD", the author is `cursor[bot] (NONE)`, there are no labels and 0 comments, so no maintainer has acknowledged it. I added that pattern as a fail condition: a key input still "TBD", a bot or unaffiliated author, and no maintainer acknowledgment. The rubric then rejected it.

**Check rationale**

Check `scope-bounded`, as currently written in `tools/issue-select/rubric.md`:

> | scope-bounded | Issue body and comment thread | Issue asks for ONE bounded deliverable (a bug fix, a docs page plus its related edits, a performance fix, a small set of similar missing items in one feature). Fail only if one of these holds: the issue calls itself an umbrella, megaissue or tracking issue, or its list is explicitly meant to be split into separate issues or PRs; a maintainer says outright that it touches core internals; the thread shows the design is still being debated with no maintainer-settled direction; it is a pure usage/support question; it is a vague wish with no spec where a product decision is hidden inside (for example a key input is still "TBD", the author is a bot or unaffiliated account, and no maintainer has acknowledged or labeled it); the thread shows 2 or more abandoned or closed-unmerged PR attempts, or a long thread (30+ comments) with the design still unsettled and no maintainer-stated final direction, even if a good-first-issue label is present. An old issue whose maintainer has settled the approach or invited takers is NOT failed by age alone. A list inside the body does NOT make it an umbrella when the items are parts of one deliverable, similar small instances of one fix, or candidate causes/suggestions for one problem; a contributor may do a reasonable subset. A terse body, trailing "etc.", missing repro steps, or a non-maintainer author alone does NOT fail. | required |

Reasoning: the first version failed any issue whose body held a list of sub-items, which wrongly rejected issue-01, issue-04 and issue-19 (all gold accept). The evidence guide says short is not the same as unscoped, so the current form fails only on explicit umbrella/tracking issues, maintainer-stated core internals, unsettled design, support questions, spec-less wishes, and repeated abandoned attempts. A list that is parts of one deliverable no longer counts as an umbrella.

**Trade-offs**

Loosening the list rule changed issue-15 and issue-20 (both gold reject) to accept until I added the extra fail conditions. Canaries re-run with `--only` after the final edit: issue-05 and issue-10 (umbrella/megaissue rejects), issue-15, issue-20, and issue-09 (old issue with stale claim, gold accept). All 8 issues in that re-run agreed with gold.

What it gives up: an umbrella issue that does not call itself one and has a short list of items could pass. The 30+ comment threshold is also a rough number, so a long thread that is not a design debate could be rejected.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

I looked for issues with recent commits and comments, which indicates the repo is active. I checked that the commits were made by a human, that the repo is in use, and that the repo is not archived. The scope is suitable for me, and it is a good first issue.

1. **Fit.** I want to learn Python. #68 is a small logic bug (`ZeroDivisionError` when the keyword search index is empty) in two files, plus an `xfail` test marker to remove, so I get to read existing Python and write a test. It is labeled good first issue and the estimate is 2-4 hours.
2. **Verdict vs. what I weighed.** The skill accepted all three candidates and ranked #72 first, #68 second, #73 last because #73 is docs-only. I chose #68 over #72 because #72 has 16 open competing PRs against 7 on #68, and #72 changes password-hash verification, which I would rather not make my first change. The rubric could not see either of those, since the house rule makes shared claims pass.
3. **Claiming.** The thread has 47 comments and 7 open PRs from classmates. Under the Path Review house rule that does not block the issue, and credit attaches to the pull request I open, not to whether it merges, so I will claim it anyway in Unit 2. Difficulty: the fix is probably already described in the existing PRs, so my work has to be my own, and I will read the thread before starting.
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
