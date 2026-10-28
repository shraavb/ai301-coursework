# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

shraavb

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-6459699710

> Reproduced on `main` (2f4e82f, rank-bm25 0.2.2): `KeywordSearcher.index([])` passes an empty corpus to `BM25Okapi`, which divides by a corpus size of 0 (`rank_bm25.py:52`). `test_empty_index --runxfail` fails with `ZeroDivisionError: division by zero`. A one-chunk index works, and `search()` before indexing returns `[]`.
>
> Plan: add an early return in `index()` for an empty list (reset `chunks` and `bm25`), and remove the `xfail` marker from `test_empty_index`. I won't touch `rank_bm25`, `search()`, or `hybrid.py`.
>
> Check: re-run my repro (the `--runxfail` test should pass, `index([])` should return, the one-chunk control should be unchanged), then `make check && make test-unit`.
>
> Not checked: only rank-bm25 0.2.2, and I haven't traced the hybrid retriever's callers. If this approach fits, I'll open a PR.
>
> Relevant files:
>
> - `rag/retriever/keyword_search.py`
> - `tests/unit/test_keyword_search.py`

---

## Your branch

**Branch**

fix/68-empty-keyword-index

**Evidence**

Before the fix (my Unit 2 repro, `main` at 2f4e82f):

```
$ .venv/bin/pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -v
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index XFAIL [100%]
1 xfailed in 0.94s

$ .venv/bin/pytest tests/unit/test_keyword_search.py -k test_empty_index --runxfail -v
>       self.avgdl = num_doc / self.corpus_size
E       ZeroDivisionError: division by zero
.venv/lib/python3.12/site-packages/rank_bm25.py:52: ZeroDivisionError
1 failed, 16 deselected in 0.12s

$ .venv/bin/python -c "
from rag.retriever.keyword_search import KeywordSearcher
s=KeywordSearcher()
print('search before index:', s.search('python'))
s.index([])"
search before index: []
... File ".../rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero

$ .venv/bin/python -c "
from rag.retriever.keyword_search import KeywordSearcher
s=KeywordSearcher(); s.index([{'text':'python code'}]); print('nonempty ok:', len(s.search('python')))"
nonempty ok: 1
```

After the fix:

```
$ pytest tests/unit/test_keyword_search.py -k test_empty_index --runxfail -v
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 17 items / 16 deselected / 1 selected

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED [100%]

======================= 1 passed, 16 deselected in 0.28s =======================

$ pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -v

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED [100%]

============================== 1 passed in 0.10s ===============================

$ direct empty index
search before index: []
index([]) returned; search: []

$ control
nonempty ok: 1

$ full file
.................                                                        [100%]
17 passed in 0.11s
```

## Eval iterations

**Run history**

One full run: 19/20. It passed the bar (18/20) and every category had a match. I did not revise the rubric, procedure or evidence guide after it.

**Package analysis**

pkg-20 (ghostty-org/ghostty#11261), the one disagreement. Gold is `reject`; my rubric graded it `accept`. The gold note reads: "excellent bounded plan that follows the thread's direction, but the comment contains no AI-use disclosure and ghostty's stated policy requires disclosing all AI usage". My rubric read it as accept because every required check passed: the cause (a stale `prev` pointer after page growth) fits the repro and its no-hyperlink control, the scope is bounded, and the test names both fuzz cases. The only check that reads the repo's contribution policy is "Comment matches thread and repo", which is `preferred`, so its fail cannot hold a package. The repo-facts block states "All AI usage in any form must be disclosed", and the comment has no disclosure.

**Check rationale**

| Diagnosis fits evidence | The plan's stated cause, read against every step and timing in the repro evidence block (including any step that removes a suspect from the loop). Also note whether the cause is the plan's own or only repeated from a thread comment. | Passes if the stated cause explains each repro observation and no repro step contradicts it (e.g. the bug still occurs with the named component bypassed or unchanged). Fails if any repro step contradicts the cause, or the cause is only quoted from the thread with no tie to the repro. Unclear if the plan states no cause. | required |

It reads this way because the sample rubric's diagnosis check ("passes if the plan says what causes the bug") accepted calib-03, whose cause came from the thread while repro step 3 showed the 25.8 s delay with no pager involved. I rejected "says what causes the bug" for a condition that compares the cause with the repro steps, including steps that remove a suspect, and I added the "tied to the repro" requirement so a quoted thread claim fails.

**Trade-offs**

This check gives up speed on thin plans: a short plan whose cause is stated without any repro tie-in fails even if the guess is right. The cost I accept is the pkg-20 miss, where the comms check is only `preferred`. Making it `required` would probably fix pkg-20, but I did not test it, and it could flip a clear-accept package whose comment skips a policy point. I did not run any `--only` canaries, because I made no revisions after the full run.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
