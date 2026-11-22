# Contribution: Emit a JSON result from `release pull`

**Contribution Number:** 2
**Student:** Shraavasti Bhat
**GitHub Username:** shraavb
**Issue:** https://github.com/dsx-ai-factory/nvkr/issues/393
**Pull Request:** https://github.com/dsx-ai-factory/nvkr/pull/510

---

## Toolkit

Each story section below ends with a Toolkit block. The four weekly entries below are the toolkit's own timeline; Units 5 to 7 are the same tool work as in `contribution-story.md`.

**Unit 5: issue-scout, a GitHub-wide search, rank and grade skill**

Same scout as contribution 1. It graded #393 as accept (difficulty: moderate), because the change spans two files and has an open question about how directory artifacts are represented. See `contribution-story.md` for what broke and what I changed.

**Unit 6: voice-guard and a repo-memory file**

Same voice guard and `CLAUDE.local.md` as contribution 1. No voice-guard stop is recorded for #393.

**Unit 7: ship-gate and a gate manifest**

The manifest for nvkr is the one described in `contribution-story.md`. No ship-gate run is recorded for `feat/393-release-pull-json`: #510 opened on 2026-11-16 and I installed the gate on 2026-11-21, so the gate has no verdict on this branch.

**Unit 8: pr-monitor, a read-only PR state check and drafting pass, with my own playbook**

I ran the monitor on 2026-11-21 from `feat/393-release-pull-json`, after #510 had already merged. It read the PR as merged and approved, with 30 checks passing, and drafted a short "merged follow-up" thank-you. I did not post it, because the PR was merged and the thank-you added nothing. It could not see whether #393 had auto-closed or who merged, and its draft told me to check both. It has not yet been used on an open PR.

---

## Why I Chose This Issue

#393 was my second pick from the `gfi-watch` scan of the `dsx-ai-factory` org, after #398. The scout graded it accept, with moderate difficulty: the issue names `cmd/nvkr/release_pull.go` and `pkg/nvkr/delivery/release_pull.go`, and leaves one question open about how directory artifacts are represented. I claimed it on 2026-11-08 (02:32 UTC) after saying #398 came first, and the maintainer assigned it to me on 2026-11-09. Claim comment: https://github.com/dsx-ai-factory/nvkr/issues/393#issuecomment-6716217502

It fits what I wanted to learn next: #398 was input validation, and this one is a script-facing output contract, where a field name chosen now is hard to change once scripts depend on it. It is a size-S feature with acceptance criteria written as testable outcomes. The scout's weaknesses are the same ones recorded in `contribution-story.md`: it cannot see `/claim` comments at search time, and its responsiveness check passed on a placeholder comment.

**Toolkit**

- Tool: issue-scout, run on the hand-fed #393 URL, then re-run after the scope patch
- What it produced: accept for #393, difficulty moderate
- Difficulties: same two as contribution 1, the claim check and the placeholder-comment responsiveness check
- What changed: nothing new for this issue

---

## Understanding the Issue

### Problem Description

`nvkr release pull` prints quoted prose after it downloads an output. A script that downloads an executable or chart needs the resulting paths and hashes, so today it has to scrape that text or guess the archive's internal filenames.

### Expected Behavior

`release pull --format json` emits exactly one parseable JSON value after success. It holds the supplied root reference, the exact output identity, kind and platform, the destination, and the actual returned file records with relative names, hashes and sizes. It states plainly what was and was not checked. An unknown format or a JSON/TUI conflict fails before the pull runs, an operation error produces no success JSON, and the default text output keeps its exact bytes.

### Current Behavior

At base commit `95b61bc5` there is no machine format. The flag is not defined: `--format json` fails with `flag provided but not defined: -format`, exit 2, and only the text message is written.

### Affected Components

`cmd/nvkr/release_pull.go` (the command and its text output), a new `cmd/nvkr/release_pull_result.go` for the JSON presentation, `pkg/nvkr/delivery/release_pull.go` (which already returns `PulledOutput` and `PulledFile`), the tests in `cmd/nvkr/release_pull_test.go` and the delivery tests, and `docs/release-navigation.md`. After review the result also reuses `releaseview.InspectionChecks` from `pkg/nvkr/releaseview`.

**Toolkit**

- Tool: none recorded for this phase
- What it produced: nothing recorded; the answers above come from the issue's own pinned links
- Difficulties: none met
- What changed: nothing

---

## Reproduction Process

### Environment Setup

macOS on Apple Silicon, upstream `dsx-ai-factory/nvkr` at base commit `95b61bc5`. The rebased branch was tested with Go 1.27.4. The repo is on an external exFAT drive, which I think affected some tests (see Testing Strategy).

### Steps to Reproduce

This is a missing feature, so there is no bug to trigger. The "before" evidence is a binary built from the base commit:

1. Build `nvkr-before` from `95b61bc5`.
2. Run `nvkr-before release pull --reference <ref> --output tools --out <dest> --anonymous --format json`.
3. Observed: `nvkr: [NVKR-E-USAGE] invalid command usage: flag provided but not defined: -format`, exit 2.

### Reproduction Evidence

- **Commit showing reproduction:** none (no changes; this is the base commit `95b61bc5`)
- **Screenshots/logs:** the exit-2 output in step 3, quoted from the PR description.
- **My findings:** The gap is confirmed: no JSON result exists, and the flag is not defined. I have not run a successful JSON pull against a real registry; the contract tests inject the pull operation, and a reviewer ran loopback-registry pulls.

**Toolkit**

- Tool: repro-check was not run on #393; no repro draft exists for it
- What it produced: nothing for this block
- Difficulties: the tool was unused, not unhelpful
- What changed: nothing

---

## Solution Approach

### Analysis

`release_pull.go:29` has no machine-format flag and `release_pull.go:75` writes only the text message. The delivery owner already returns `PulledOutput` and `PulledFile`, so the information exists. What is missing is a presentation layer that exposes chosen public fields without serializing internal delivery fields.

### Proposed Solution

Add `--format text|json`. Build the JSON from a detached presentation type in `cmd/nvkr/release_pull_result.go` that copies only chosen public fields, versioned as `nvkr.dev/release-pull-result/v1`. Admit the format before the pull operation, reject explicit `--ui=tui` with JSON through the existing presentation selector, and emit no JSON on operation errors. `oci` and `apt-repository` outputs are reported as a directory with no file list and no invented checksum, and aggregate images keep every required platform. I am not changing the download operation, writing any file or checksum sidecar, or producing a verification receipt.

### Implementation Plan

1. Add the flag and admission in `release_pull.go`, before the pull operation.
2. Add `release_pull_result.go` with the detached result type.
3. Add tests for executable, chart and OCI-directory results, for admission and failure, and for unchanged text output.
4. Document the format in `docs/release-navigation.md` and the reconciliation row.

Deviation after review, recorded: the first commit used a `scope` block with booleans and a command-layer table of payload kinds. After review I replaced `scope` with `inspectionDetails.checks` using the shared `releaseview.InspectionChecks` type, matched `release browse`'s platform field names, and moved the payload-shape decision into each delivery pull arm. Both changes are in the second commit, `3758a342`.

**Toolkit**

- Tool: plan-check was not run on this plan; no plan-check verdict is recorded for #393
- What it produced: nothing for this block
- Difficulties: the tool was unused, not unhelpful
- What changed: I would run plan-check before building on my next contribution

---

## Testing Strategy

**Before and after on the real CLI.** Before (base `95b61bc5`): `--format json` exits 2 with `flag provided but not defined: -format`. After: `--format yaml` and `--format JSON` both exit 2 with `[NVKR-E-USAGE] release pull format must be text or json`, and the destination directory is not created. `--format json` against an unresolvable registry exits 30 with 0 bytes on stdout and no destination created.

**Contract tests with an injected operation.** `go test ./cmd/nvkr -run 'ReleasePullJSON|ReleasePullText|ReleasePullPayload' -count=1 -v` passes. `TestReleasePullJSONResult` compares the full JSON for an executable, a chart and an OCI directory, with internal delivery fields populated so a leak would fail. The JSON-with-TUI case uses a viewer that fails the test if it opens. `TestReleasePullTextDefaultUnchanged` compares default and `--format text` output to a hardcoded literal.

**After review.** The second commit added a loopback test that runs `--format json` through the real pull and compares each reported path, digest and size with the written file, a disk-image case, a write-failure case, an `--ui=auto` case, and a test that decodes a second value and requires `io.EOF`.

**Repo checks.** On the rebased branch (Go 1.27.4, macOS), `go test ./cmd/nvkr -count=1` passed. `make check` did not go green on my machine: tests in `pipeline`, `buildgraph` and `factory` fail, and I said so in the PR. I re-ran them on a clean copy of the base, where the two `pipeline` Packer tests, `TestOVNLockOnlyPropagation` and `TestAICRNVKEBundleFlags` still failed. I put the cause of the Packer and bash failures down to macOS, and I did not diagnose `buildgraph`. I did not run `make vulncheck`, the `make ci-check` extras, CodeQL, the DCO app or any Linux run of the `pipeline` tests.

**Toolkit**

- Tool: the repo's own `go test` and `make` targets, run by hand
- What it produced: the before and after output above and a table of failures that also occur on the base
- Difficulties: `make check` cannot go green on my macOS and exFAT setup
- What changed: I re-ran each failing test on a clean copy of the base to separate those failures from my change, and listed them in the PR instead of hiding them

---

## Implementation Notes

Dates are UTC.

### 2026-11-16

Opened #510 at 04:47. The first commit, `4aae9a7a`, had been committed at 04:39. The reviewer `depohmel` reviewed it at 19:40 with no blocking findings and three optional recommendations: keep a real delivery-to-JSON regression, require EOF after the JSON test document, and limit the "no output on failure" docs to pull failures. `njhensley` reviewed it at 22:16 with six findings. The major one was that my `scope` block used a third vocabulary for what `release browse`, `diff` and `export` already report under `inspectionDetails.checks`, and left out the metadata check that pull performs.

### 2026-11-17

Pushed `3758a342` at 16:45 addressing all six findings and the three recommendations, and replied to both reviewers at 16:49. `njhensley` approved at 17:46 with one new nitpick: a new delivery arm that forgets to set `Payload` still compiles and then fails with `CodeInternal` only after `--out` has been written, and four of the seven arms have no test that would catch it. They merged at 18:09, and #393 closed two seconds later. I pushed no change for the nitpick.

**Toolkit**

- Tool: none recorded for this phase
- What it produced: two commits and the tests described above
- Difficulties: the first design invented its own result vocabulary instead of reusing the one the neighbouring commands already share
- What changed: the second commit replaced it

---

## Pull Request

**PR Link:** https://github.com/dsx-ai-factory/nvkr/pull/510

**PR Description:** Title `feat(release): add --format json to release pull`, following the repo template. It adds `release pull --format json`, a script-readable result with the supplied root reference, exact output identity, destination and the returned file records, and leaves the default text response unchanged. Motivation: a script that downloads an executable or chart has to scrape quoted prose today. `Closes: #393`. The description records the real before and after CLI output, says I have not run a successful JSON pull against a real registry, lists the macOS test failures that also occur on the base, and lists what I did not run locally.

**Maintainer Feedback:**

- 2026-11-16: `depohmel` reviewed with no blocking findings and three optional recommendations. `njhensley` reviewed with six findings, one major (the `scope` vocabulary).
- 2026-11-17: I pushed `3758a342` addressing all of them and replied to both reviewers. `njhensley` approved with one new nitpick (unclassified delivery arms), and merged.

**Status:** Merged (2026-11-17, 18:09 UTC, by `njhensley`)

**Toolkit**

- Tool: none recorded for opening the PR; no voice-guard stop is recorded, and the ship-gate did not run on this PR
- What it produced: nothing recorded
- Difficulties: none met; both commits carry only my `Signed-off-by`, with no agent trailer
- What changed: nothing

---

## Learnings and Reflections

### Technical Skills Gained

I learned to build a script-facing output contract: a detached presentation type that copies only chosen public fields, so internal delivery fields cannot leak, with an explicit schema version. I learned that a new result format should reuse the vocabulary of the commands next to it, here `releaseview.InspectionChecks`, because a reviewer pointed out it is cheap to fix before v1 ships and costly once scripts depend on it. I also learned to test the real producer, not only hand-built values, by running `--format json` through a loopback registry and comparing each reported path, digest and size with the written file, and to check a stream ends with `io.EOF` after the document.

### Challenges Overcome

The hard part was review: my first result shape invented its own names for facts that three other commands already report. I rebuilt it on the shared type, matched browse's platform fields and moved the payload-shape decision into each delivery arm, all in one follow-up commit, and answered each finding point by point. `make check` could not go green on my macOS and exFAT setup, so I re-ran the failing tests on a clean copy of the base and listed them in the PR.

### What I'd Do Differently Next Time

I would read how the sibling commands name the same facts before choosing field names, since that was the main finding. I would run the ship-gate and plan-check before opening the PR. I would also install a Go version that matches `.go-version` before relying on `make check`, and decide what to do with the final nitpick, an arm that forgets `Payload`, instead of leaving it open after approval.

---

## Resources Used

- Issue #393 and its acceptance criteria, which named the two `release_pull.go` files and the injected-operation seam: https://github.com/dsx-ai-factory/nvkr/issues/393
- The review thread on PR #510, from `njhensley` and `depohmel`: https://github.com/dsx-ai-factory/nvkr/pull/510
- `pkg/nvkr/releaseview/description.go`, which `njhensley` pointed to for `inspectionDetails.checks`
- nvkr CONTRIBUTING.md and AGENTS.md, for the PR checklist, signing rule and attribution rule: https://github.com/dsx-ai-factory/nvkr/blob/main/CONTRIBUTING.md
