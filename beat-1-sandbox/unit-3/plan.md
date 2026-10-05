# issue 15 plan:
- source: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15

## Repo facts

- `agent/orchestrator.py`: `Orchestrator.__init__` (line 30) makes one `ContextManager` for the orchestrator's lifetime; `run()` (lines 32-76) never resets it and never calls session delete.
- `agent/memory/context_manager.py`: cache key (line 26) is `f"{tool_name}:{input_hash}"` — no `profile_id`, no reset method.
- `agent/memory/session_store.py`: `delete()` (lines 68-81) is fully implemented but never called.
- `grep -rn "Orchestrator("` (excluding the repro script) finds nothing — not wired into the API yet, so the fix is contained to `agent/`.

## Repro evidence

From `claim_comment.md`:
> Root cause: `market_analyzer`'s input is hardcoded to `{"detected_skills": {}}` (agent/orchestrator.py:127), and `Orchestrator.__init__` creates one `ContextManager` (agent/orchestrator.py:30) whose cache key (`agent/memory/context_manager.py:26`) never includes `profile_id`, so any tool call with matching input collides across profiles... `SessionStore.delete()` ... exists and works but is never called from `run()`.

From `repro_report.md`'s captured output (profile B's run):
```
tool_result_cache_hit  key=market_analyzer:95e4a8f9... tool=market_analyzer
...
market_analyzer call_count: 1
profile A market_analyzer result: {'call_number': 1}
profile B market_analyzer result: {'call_number': 1}
redis DELETE calls across both runs: []
```
`readme_scorer` (control, genuinely different input per profile) correctly ran twice (`call_count: 2`), proving the bug is scoping/reset, not "caching is broken."

## Candidate plan

### Diagnosis
Two bugs in `Orchestrator`, both causing stale state across profiles:
1. The shared `ContextManager` cache is never cleared, so a repeated `(tool_name, input_hash)` — guaranteed for `market_analyzer`, whose input is hardcoded — returns a prior profile's result.
2. `session_store.delete(profile_id)` is never called at the start of `run()`, so old session keys for that profile are never purged.

### Scope
**In:** reset the in-memory cache and delete prior session state at the start of every `run()`.
**Out:** per-profile cache-key scoping (unneeded once cleared each run), fixing `market_analyzer`'s hardcoded input (separate pre-existing TODO), changes to `SessionStore` itself (already correct), wiring `Orchestrator` into the API (not yet connected).

### Files
- `agent/orchestrator.py` — modify `run()`
- `agent/memory/context_manager.py` — add `clear()`
- `tests/unit/test_orchestrator.py` — new regression test

### Approach
1. `ContextManager.clear()`: reset `self.results = {}`.
2. In `run()`, right after the `orchestrator_start` log: call `self.context_manager.clear()` and `self.session_store.delete(profile_id)` (if a store is configured). This makes the old `get(profile_id)`/merge lines always return `{}`, so remove them and persist `results` directly via `session_store.set(profile_id, results)`.
3. No changes to `SessionStore` or tool implementations needed.

### Test plan
Re-run the Unit 2 repro, expected results after the fix:
1. `python3.12 repro_issue_15.py` (unmodified) — now **fails** at `assert market_tool.call_count == 1, "BUG: ..."` since it will be `2`. That failure is the expected, correct outcome (today it passes and prints "Reproduced: ...").
2. New `tests/unit/test_orchestrator.py`, adapted from the repro's `FakeRedis`/`CountingTool` doubles, asserting the fixed behavior: `market_tool.call_count == 2`, `result_a[...] != result_b[...]` for `market_analyzer`, `fake_redis.delete_calls == ["session:profile-A", "session:profile-B"]`, and `readme_scorer` control still `== 2`.
3. `pytest` (full suite) — no regressions, since nothing else references `Orchestrator`/`ContextManager`.

### Risks and Unknowns
- Clearing the whole cache per run (vs. per-profile keys) also drops same-run cache sharing between tools — not used today, so low risk.
- Removing the old session merge means no state carries forward across reviews for the same profile anymore — that's the intended fix, but flagging in case a future feature wanted partial carry-forward.
- Extra Redis `delete` call on every run is a harmless no-op when no prior session exists.
- Not fixing `market_analyzer`'s hardcoded input — out of scope, but worth noting it would still collide with itself within a single run if ever called twice.


## Deviation
- nothing changed, the build followed the fix plan.
