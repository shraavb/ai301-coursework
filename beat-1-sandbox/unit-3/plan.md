# Plan: fix `ZeroDivisionError` in `KeywordSearcher.index([])` (issue #68)

## Diagnosis

`KeywordSearcher.index()` in `rag/retriever/keyword_search.py` passes its chunk list straight to `BM25Okapi(tokenized_corpus)`. With an empty list, the library divides by its corpus size (0) while computing the average document length, so it raises. The bug is in how `index()` handles an empty list, not in the library for non-empty input.

Repro evidence I rely on (my unit 2 repro comment, `main` at 2f4e82f, Python 3.12.12, rank-bm25 0.2.2):

- Step 3, test with the xfail marker ignored:
  `self.avgdl = num_doc / self.corpus_size` /
  `ZeroDivisionError: division by zero` at `rank_bm25.py:52`
  (`pytest tests/unit/test_keyword_search.py -k test_empty_index --runxfail -v`: 1 failed)
- Step 4, no pytest involved: `search before index: []` prints fine, then `s.index([])` raises the same error. So `search()` on an unindexed searcher already returns `[]`; only `index()` fails.
- Step 5, control: `s.index([{'text':'python code'}])` then `s.search('python')` gives `nonempty ok: 1`. One chunk works, so the failure is specific to the empty list.

Why this cause fits every step: the error is raised inside `BM25Okapi([])` called from `index()` (step 3 and 4), `search()` already has its own `not self.bm25 or not self.chunks` guard returning `[]` (step 4), and a non-empty corpus does not divide by zero (step 5).

## Scope

In scope: one guard at the top of `KeywordSearcher.index()` in `rag/retriever/keyword_search.py`, and removing the `xfail` marker on `test_empty_index` in `tests/unit/test_keyword_search.py`.

Not in scope: `rank_bm25` itself (a third-party library), `search()` (it already handles an empty index), `rag/retriever/hybrid.py`, how callers decide when to index, tokenization, and any other test in the file.

## Files I will touch

1. `rag/retriever/keyword_search.py` (the guard in `index()`)
2. `tests/unit/test_keyword_search.py` (remove the `@pytest.mark.xfail(strict=True, ...)` decorator on `test_empty_index`)

## Approach

1. In `index()`, before building the corpus: if `chunks` is empty, set `self.chunks = []` and `self.bm25 = None`, log `keyword_index_built` with `chunk_count=0` (same event the method already logs), and return.
2. Leave the non-empty path unchanged.
3. Remove the `xfail` decorator from `test_empty_index`. The test body (`index([])` then `search("python", top_k=10) == []`) already states the expected behavior, so I will not rewrite it.

Order: guard first, then the test change, so the test passes the moment the marker comes off. (The marker is `strict=True`, so leaving it on after the fix would fail the suite with XPASS.)

## Test plan

Re-run my unit 2 repro steps after the change, on the same environment:

1. Step 3 command, `pytest tests/unit/test_keyword_search.py -k test_empty_index --runxfail -v`: expect `1 passed` instead of `ZeroDivisionError`.
2. Step 2 command, `pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -v`: expect `PASSED` (no longer `XFAIL`), since the marker is removed.
3. Step 4 command, direct `python -c` run: expect `search before index: []` and then `s.index([])` returns with no traceback.
4. Step 5 control: expect `nonempty ok: 1` unchanged.
5. `make check && make test-unit` (the commands CONTRIBUTING asks for before a PR): expect green, which confirms the rest of `test_keyword_search.py` still passes.

## Risks and unknowns

- I searched `rag/`, `api/`, `core/`, and `agent/` for callers and found `KeywordSearcher` used only in `rag/retriever/hybrid.py` (type annotation); I did not find a non-test call to `index()`. I have not traced how hybrid search is wired, so I do not know if any caller expects `index([])` to raise.
- I only tested rank-bm25 0.2.2. I have not checked whether newer versions handle an empty corpus, so I am guarding in `index()` rather than relying on the library.
- I have not confirmed that `self.bm25 = None` after an empty index is the behavior a maintainer wants (versus raising a clear `ValueError`). The existing test expects `search()` to return `[]`, which is why I chose `None`.

## Deviations

None. The build matched the plan: one guard in `index()` and the xfail marker removed, in the two files named above. No other files changed.
