## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/153

**Issue title:** Faithfulness checker crashes when a context chunk has text: None

**Tier:** [✅] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
When retrieving context chunks for the RAG pipeline, a chunk's `text` field can sometimes be `None` instead of an empty string or missing entirely. The `FaithfulnessChecker.check()` method uses `chunk.get("text", "")` to build context text, but `.get()`'s default only applies when the key is missing — not when the value is `None`. This causes `" ".join(...)` to raise a `TypeError` since it can't join a `NoneType`. This affects `rag/evaluator/faithfulness_checker.py`, part of the RAG pipeline's hallucination-detection safeguard. A successful fix would treat a `None` text value the same as empty text, returning a faithfulness score without crashing.

**"Is this right for me?" checklist reasoning:** I chose issue #153 for two reasons. First, navigating a new codebase and opening my first pull request is new to me, and this issue is tagged good-first-issue and tier-1, making it an appropriate entry point. Second, RAG is within my scope of understanding — I know what its inputs are and what it returns — so I could reason about the bug's root cause rather than just pattern-matching a fix.

**Branch name:** fix/153-context-chunk-text-error

**Setup confirmation:** [ ✅] App runs locally at localhost:5173

**Cohort ledger:** [✅ ] Issue added to cohort ledger


## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/Yashu16/pathreview/commit/0c9a09f


**Reproduction summary:**
I ran the existing test `test_none_context_chunk_text` in `tests/unit/test_faithfulness_checker.py`, which passes a context chunk with `{"text": None}`. Instead of returning a faithfulness score between 0.0 and 1.0, it raised `TypeError: sequence item 0: expected str instance, NoneType found`, confirming the bug.

**PLAN.md link:** https://github.com/Yashu16/pathreview/blob/fix/153-context-chunk-text-error/PLAN.md


## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
I have implemented the fix in `rag/evaluator/faithfulness_checker.py` by changing `chunk.get("text", "")` to `chunk.get("text") or ""`. I have run the existing test `test_none_context_chunk_text`, which now passes without raising a `TypeError`. I also ran the full test suite for `tests/unit/test_faithfulness_checker.py`, and all tests passed, confirming that my change did not break any other functionality.

**Next steps:**
I have to run the full test suite for the entire project to ensure that my fix does not introduce any regressions. After confirming that all tests pass, I will prepare a pull request with a Conventional Commit message referencing issue #153.

**Blockers:**

---

### Check-in 2 (end of week)

**PR link:** [link to your submitted pull request]

**Branch:** [the branch name you worked on, e.g. `fix/123-short-description`]

**What you built:**
[1–3 sentences summarizing what your fix does and how it works]

**Tests added or updated:**
[Which test files did you touch? What do they cover?]

**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes

**Draft PR feedback received from:** [name or Slack handle, or "none"]
