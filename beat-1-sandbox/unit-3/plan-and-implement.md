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

jinjin1919

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15#issuecomment-5986578023 


Posting my plan before starting on this.

**Diagnosis:** `Orchestrator` keeps one `ContextManager` for its whole lifetime, and its cache key (`agent/memory/context_manager.py:26`) is `f"{tool_name}:{input_hash}"` with no `profile_id`. Since `market_analyzer`'s input is hardcoded to `{"detected_skills": {}}` (`agent/orchestrator.py:127`), its cache key is identical on every `run()` call, so a second profile silently gets the first profile's cached result:

```
tool_result_cache_hit          key=market_analyzer:95e4a8f9... tool=market_analyzer
...
market_analyzer call_count: 1
profile A market_analyzer result: {'call_number': 1}
profile B market_analyzer result: {'call_number': 1}
```

Separately, `SessionStore.delete()` (`agent/memory/session_store.py:68-81`) is fully implemented but `run()` never calls it, so a profile's previous session state is never purged before a new run (`redis DELETE calls across both runs: []`).

**Fix:** at the start of `Orchestrator.run()`, clear the `ContextManager` cache (new `ContextManager.clear()`) and call `session_store.delete(profile_id)` before the plan executes, so every run starts from a clean cache and clean session state. This replaces the current "load previous session state and merge" logic, which becomes dead code once delete runs first.

**Scope:** touches `agent/orchestrator.py` (`run()`) and `agent/memory/context_manager.py` (new `clear()`). Not touching `SessionStore` (already correct) or `market_analyzer`'s hardcoded input (pre-existing TODO, separate from this issue). `Orchestrator` isn't wired into the API yet, so no other call sites are affected.

**Test plan:** adding `tests/unit/test_orchestrator.py`, adapting the repro's `FakeRedis`/`CountingTool` doubles, asserting `market_analyzer` now runs once per profile with distinct results, and that `session_store.delete` is called once per run with that run's `profile_id`. I'll also re-run my original repro script and confirm it now fails on the assertions that encoded the buggy behavior — expected, since those assertions describe the bug.

Will open a PR with this change.



---

## Your branch

**Branch**

- Branch: `fix/15-Agent-sesson-state-not-cleared`

**Evidence**

[Your Unit 2 reproduction steps re-run against the built change: the before, then the
after. Paste both, including the commands you ran and their output.]




## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

3 runs consistently getting 18/20 

re-run eval for the packages that is not agreed with gold label with `--only pkg-14` and revise the rubric. 

final run save to eval-run.txt


**Package analysis**

`pkg-14` (category `clear-accept`). The gold label is **accept**; my rubric decided
**reject**, failing only `executable`. Re-running with `--only pkg-14` gave the same result,
so it is not run-to-run noise. All other checks passed, including `grounded-diagnosis`,
`bounded-scope` and `decisive-test-plan`.

The grader's reason: the plan's Files line reads "the client attach/reattach path in
`zellij-server` (session connection handling) and `zellij-client`'s terminal query issuance;
exact functions to be pinned in the PR after tracing the query issuance with debug logs".
My `executable` check fails any plan that defers "a real decision", and the grader read
"exact functions to be pinned in the PR" as that kind of deferral. It also noted there is no
separate ordered Approach section.

Looking at the package, I think the gold label is right and my rubric is too strict here.
The plan does name the subsystem (the reattach handshake), says what the
change is (drain pending OSC color responses before pane input is wired), and says how the
leak's origin was found (`zellij --debug`). What it leaves open is which function to edit,
which is a detail a contributor finds in the first hour, not a decision about what to build.
My rubric treated "no exact function named" as "decision deferred".

**Check rationale**

The `executable` row as it reads now in `tools/plan-check/rubric.md`:

> | executable | The Files and Approach sections. | Pass if named files/locations plus an ordered approach let someone who has never seen the issue can get started working easily; fail if any real decision is deferred ("somewhere," "whichever is easier," "not sure which layer"). | required |

It reads this way because I wanted to catch plans a stranger couldn't start, and I used
the lecture's examples of deferred decisions ("somewhere," "whichever is easier," "not sure
which layer") as the fail trigger. I kept it as a `required` check because an unbuildable plan
is a real failure family (`pkg-10`, `pkg-17` and `pkg-18` are all `unbuildable` and all
agreed). The cost is the `pkg-14` miss: "any real decision is deferred" is loose enough that a
plan with a narrowed, named location but no function name gets caught. I have not yet changed
this row. The pass condition also has a grammar slip ("someone who has never seen the issue
can get started"), which I should fix when I revise it. The likely revision is to say that
naming the crate or module plus the intended change is enough, and that fail is for plans
that leave open *which layer* or *which approach*, not *which function*.

**Trade-offs**

Loosening `executable` as above would flip `pkg-14` to accept, taking agreement from 19/20 to
20/20, but at the risk of letting a vague plan through. 

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
