# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

All dates are measured against the capture date in the repo-facts block (live mode: today).

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-not-archived | `archived:` on the repo line of repo facts | `archived: no`. Any archived repo fails. | required |
| maintainer-commits-recent | "last 5 default-branch commits" list: dates and authors | At least one commit within 180 days of capture by a human author (name not ending in `[bot]`, not an automated/generated-content commit). Bot-only recent commits fail. Fewer than 180 days is a pass even if the newest commit is not from a maintainer badge, since the sample has no badges. | required |
| repo-in-use | "last push to any branch" and "latest release" in repo facts | Last push to any branch within 180 days of capture OR latest release within 365 days of capture. A repo with no releases passes on push recency alone. Both failing = fail. | required |
| scope-bounded | Issue body and comment thread | Issue asks for ONE bounded deliverable (a bug fix, a docs page plus its related edits, a performance fix, a small set of similar missing items in one feature). Fail only if one of these holds: the issue calls itself an umbrella, megaissue or tracking issue, or its list is explicitly meant to be split into separate issues or PRs; a maintainer says outright that it touches core internals; the thread shows the design is still being debated with no maintainer-settled direction; it is a pure usage/support question; it is a vague wish with no spec where a product decision is hidden inside (for example a key input is still "TBD", the author is a bot or unaffiliated account, and no maintainer has acknowledged or labeled it); the thread shows 2 or more abandoned or closed-unmerged PR attempts, or a long thread (30+ comments) with the design still unsettled and no maintainer-stated final direction, even if a good-first-issue label is present. An old issue whose maintainer has settled the approach or invited takers is NOT failed by age alone. A list inside the body does NOT make it an umbrella when the items are parts of one deliverable, similar small instances of one fix, or candidate causes/suggestions for one problem; a contributor may do a reasonable subset. A terse body, trailing "etc.", missing repro steps, or a non-maintainer author alone does NOT fail. | required |
| no-active-claim | "this issue: assignees / linked PRs" in repo facts, plus claim comments in the thread | Fail if ANY holds: an assignee is set; a linked or thread-mentioned PR is open for this issue; a recent claim comment ("I'll take this", "working on this") within 90 days of capture is unanswered by a maintainer and not followed by the claimant stepping back. Pass if the only claims are older than 90 days with no PR (stale), or a closed unmerged PR with no new claimant. A good-first-issue label does not imply free; believe the thread over the sidebar when they disagree. | required |
| ai-policy-allows | "contribution policy" line in repo facts | Fail only if the policy outright bans AI-generated code or documentation. Disclosure, review, understanding or testing conditions pass. No stated policy passes. | required |
| maintainer-responsive | "maintainer first-response sample" and maintainer comments in this thread | At least one sampled issue got an owner/member/collaborator reply within 30 days, or a maintainer commented in this thread. | preferred |
| good-first-issue-signal | Issue labels, maintainer-authored body | Labeled good first issue / documentation, or opened by a maintainer/member. | preferred |
| clear-acceptance | Issue body | Body states expected behavior or a concrete done-condition (checklist, proposed changes, repro). | preferred |

## Verdict rule

Accept if and only if every `required` check passes. Any required `fail` means reject. A required check graded `unclear` counts as fail, EXCEPT `ai-policy-allows`, where silence or no policy line counts as pass. `preferred` checks never change the verdict; they only rank accepted issues.
