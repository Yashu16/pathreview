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

**Reproduction commit link:** [link to commit documenting the reproduced issue]

**Reproduction summary:**
I ran the existing test `test_none_context_chunk_text` in `tests/unit/test_faithfulness_checker.py`, which passes a context chunk with `{"text": None}`. Instead of returning a faithfulness score between 0.0 and 1.0, it raised `TypeError: sequence item 0: expected str instance, NoneType found`, confirming the bug.

**PLAN.md link:** [link to PLAN.md in your fork]