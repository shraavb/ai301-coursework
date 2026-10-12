# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

shraavb

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-6118328200

> Hi,
> Taking issue #68. Will re-produce the `ZeroDivisionError` from `KeywordSearcher.index([])` in `rag/retriever/keyword_search.py` on a fresh checkout of main, using the `test_empty_index` case in `tests/unit/test_keyword_search.py`, and post my environment and the exact command, here before I change any code. If it reproduces, my next step is to look at handling the empty chunk list in `index()` and removing the `xfail` marker as the issue asks.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-6118386911

> **Reproduced** `ZeroDivisionError` from `KeywordSearcher.index([])` on `main` at 2f4e82f.
>
> **Environment:** macOS 15.6.1 (arm64), Python 3.12.12, rank-bm25 0.2.2, fresh venv with `pip install -e ".[dev]"` from my fork's `main` (commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`). No Docker or database needed for this test.
>
> **Steps:**
>
> 1. `python3 -m venv .venv && .venv/bin/pip install -e ".[dev]"`
> 2. Normal run, where the marker hides the failure:
>
> ```
> $ .venv/bin/pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -v
> tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index XFAIL [100%]
> 1 xfailed in 0.94s
> ```
>
> 3. Same test with the xfail marker ignored:
>
> ```
> $ .venv/bin/pytest tests/unit/test_keyword_search.py -k test_empty_index --runxfail -v
> >       self.avgdl = num_doc / self.corpus_size
> E       ZeroDivisionError: division by zero
> .venv/lib/python3.12/site-packages/rank_bm25.py:52: ZeroDivisionError
> 1 failed, 16 deselected in 0.12s
> ```
>
> 4. Directly, without pytest:
>
> ```
> $ .venv/bin/python -c "
> from rag.retriever.keyword_search import KeywordSearcher
> s=KeywordSearcher()
> print('search before index:', s.search('python'))
> s.index([])"
> search before index: []
> ... File ".../rank_bm25.py", line 52, in _initialize
>     self.avgdl = num_doc / self.corpus_size
> ZeroDivisionError: division by zero
> ```
>
> 5. Control, one non-empty chunk indexes and searches fine:
>
> ```
> $ .venv/bin/python -c "
> from rag.retriever.keyword_search import KeywordSearcher
> s=KeywordSearcher(); s.index([{'text':'python code'}]); print('nonempty ok:', len(s.search('python')))"
> nonempty ok: 1
> ```
>
> **Expected:** `index([])` returns without raising, like `search()` on an unindexed searcher, which returns `[]`.
> **Actual:** `index([])` raises `ZeroDivisionError` inside `BM25Okapi([])` (`rank_bm25.py:52`), called from `index()` in `rag/retriever/keyword_search.py`.
>
> I have not changed any code yet. Next step: look at who calls `index()` and handle the empty list there.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 18/20 (bar 18, passed). Misses: pkg-09 and pkg-10 (gold accept, rubric reject), both on `behavior-matches-issue`.
2. Partial `--only` run after revising that check, with canaries pkg-02, pkg-08, pkg-14, pkg-16, pkg-17, pkg-20 (plus pkg-09 and pkg-10): 8/8.
3. Full run: 20/20 (bar 18/20: PASS). This is the committed `eval-run.txt`.

**Package analysis**

pkg-09 (sharkdp/fd#2033, honest cannot-reproduce). Rubric decision on the first run: reject, failing `behavior-matches-issue`. Gold label: accept. Final rubric: accept.

Why it read that way: my first wording of the check required the artifact to show "the same behavior the issue describes". pkg-09's artifacts show fd working normally, because it honestly could not reproduce the argument-size reordering, so a grader reading the check literally found no match and failed it. The evidence guide says an evidenced cannot-reproduce is a pass, so the check had to judge whether the attempt was a fair test, not whether the bug appeared.

**Check rationale**

Check `behavior-matches-issue`, as it reads in `tools/repro-check/rubric.md`:

> | behavior-matches-issue | The output excerpts, logs or screenshots in the repro report, read against the exact error, output or crash the issue describes, and the trigger (input, syntax, version, config) the issue names. | If the report says it reproduced: the artifact shows the same behavior the issue describes (same error text, exit status, crash vs graceful error, same input shape). If the report says it did NOT reproduce: the attempt used the issue's own trigger (same input shape or syntax, a version or setup it names, deviations acknowledged) and the real output is shown, so the non-reproduction is a fair test. Fail if the artifact shows an adjacent symptom, runs a modified input or syntax, tests a different version without saying so, shows only that the tool runs, or an attempt that does not use the issue's trigger. | required |

Why it reads that way: I split it into two cases. If the report says it reproduced, the artifact must show the issue's behavior (same error text, exit status, crash versus graceful error), which is what catches the wrong-target packages. If it says it did not reproduce, the attempt must have used the issue's own trigger and shown the real output, so an honest negative is a fair test. That revision is what fixed pkg-09 and pkg-10.

**Trade-offs**

The revision loosened the check, so it could have flipped packages that agreed before. I re-ran pkg-09 and pkg-10 with canaries from the wrong-target category (pkg-02, pkg-08, pkg-16, pkg-17), a no-evidence package (pkg-14) and the disclosure package (pkg-20). All 8 agreed with gold, and the confirming full run was 20/20 with every category matched.

What the check gives up: a not-reproduced report that happens to use the issue's trigger but a different environment could pass if it acknowledges the difference. It also relies on the grader judging "a fair test", which is less mechanical than matching an error string.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
