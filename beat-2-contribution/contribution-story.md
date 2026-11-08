# Contribution: Reuse hash_list_primitive! for string/binary list elements and factor out map-entry hashing loops

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jeanne Xu
**GitHub Username:** jinjin1919
**Issue:** https://github.com/apache/datafusion-comet/issues/6984
**Pull Request:** https://github.com/apache/datafusion-comet/pull/7569

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: [one line on the tool work]**


I built the issue scout skill with search strategy and my fit profile for ranking the scan and grade the 3 issues I chose from `AI301 Issue Selection site`. 
the first grading rejected all 3 issues because of checks `unclaimed` and `fit my profile`.  So I changed the restriction of unclaimed to no assignee is required. 
but comments or claim are okay but prefer no claims. 



**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

I wrote the voice-guard based on the lectures'drafting wrong/right example. and at the beginning my hook was not able to fire because my python version caused the check_voice.py command fail to run. and the hook was return 500 error. It turns out python3.19 up version is required. 
I graded my claim-draft with my repro-check twice and revised according to its fix comments and submitted my claim through claude code. so it was an iterative improvement. 

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I ran my issue-scout skill twice: once on two datafusion-comet issues
(#6982, #6984), and once on #6984 next to Raftersecurity/rafter-cli#32.
All of them were accepted. For #6984 every required check passed: the
maintainer is active (andygrove replies within hours), the repo is in
use, it is one bounded refactor in a single file (hash_funcs/utils.rs),
and the repo allows AI-assisted work. It was also filed by a maintainer,
andygrove (MEMBER). Two preferred checks went against it, and I chose it
anyway. First, it is not fully unclaimed: med9110 offered on 10-16 to do
the string/binary list hashing and said the map-entry refactor should be
a separate PR. So I asked only for that left-out map-entry part, and I
offered to step back if they want it. Second, the scout graded it as a
weak fit, because the work is Rust and Scala and my profile says Java.
In the second run it ranked rafter-cli#32 first, because nobody had
claimed it. I still picked #6984 because I know Apache Spark well: I
built my own mini clone of Spark as a personal project. Comet is a
Spark accelerator, so I understand what this code is for even though
the language is new to me. I also wanted my first contribution to go to
a real Apache project that people use. I hope to learn Rust macros, how
Comet keeps its hash output identical to Spark's, and how an Apache
project reviews outside contributions.

The scout fell short in a few places. On my first search it rejected
all three issues I gave it because "unclaimed" was too strict, so I
relaxed it to "no assignee required, unclaimed preferred." Then the
opposite problem appeared: "fits-my-profile" was so loose ("has some
value") that it passed rafter-cli and Comet equally, so it couldn't
rank them. It also has no check for a partial claim like med9110's, or
for when I am the second claimant. I would tighten the fit wording and
add a check for split claims. What convinced me the required checks
held up is that when I read the thread myself, I found the same claim
and the same maintainer activity the scout reported.

Claim comment (2026-10-29):
https://github.com/apache/datafusion-comet/issues/6984#issuecomment-6504660476

**Toolkit**

- Tool: issue-scout, my own skill built on my search strategy, fit
  profile, and rubric. I gave it issue URLs directly and it graded
  them; I ran it twice (#6982 + #6984, then #6984 + rafter-cli#32).
- What it produced: an `accept` for #6984 in both runs, with every
  required check passing (maintainer-alive, repo-in-use, scope-bounded,
  policy-allows-ai) and a "straightforward" difficulty note. It graded
  `unclaimed` as fail because of med9110's 10-16 offer on the
  string/binary half, ranked rafter-cli#32 first, and suggested I offer
  to take the map-entry part med9110 had left out.
- Difficulties: "unclaimed" was first too strict and rejected all
  three issues I tested. "fits-my-profile" ("has some value") was too
  loose to tell a Java-fit issue from a Rust one. And the rubric has no
  check for a partial claim, or for when I am the second claimant.
- What changed: I relaxed "unclaimed" to "no assignee required, no
  claim preferred" and made it a preferred check. I would tighten the
  fit wording so it ranks for real, and add a check for split claims.




---

## Understanding the Issue

**Problem Description**

The issue is not a bug or a fix, rather a refactor for duplicated map-entry hashing loops for map arm that sits inside a rust macro that run hashing loops and it replaces the Spark's JVM implementation of hash() (Murmur3Hash / XxHash64 in spark-catalyst)

**Expected Behavior**

Running natively is the whole point of Comet. so the rust native code is where it speeds up Spark by moving operators and expressions out of JVM into Rust. they run on Arrow columnar batches through DataFusion. 

therefore, the baseline behavior before and after refactor need to match and results must match Spark bit for bit. 

**Current Behavior**

added test checks the results match Spark bit for bit before and after refactor. the Rust code copies Spark's byte layout exactly, key before value, skipping nulls. It's also why new test checks against values produced by Spark 4.1.3's own JVM classes. the JVM is the reference, and the Rust code has to reproduce it. 

we built the hash benchmark with the old util.rs (before refactor) and once with the refactor, then compared with two binaries. they are byte-identical: same SHA-256, symbol table included. Identical machine code means the refactor can't make that path faster or slower. 

**Affected Components**

native/spark-expr/src/hash_funcs/murmur3.rs 
native/spark-expr/src/hash_funcs/utils.rs


**Toolkit**

- Tool: issue-scout skills for searching in the wild github open repo with my fit profile. 
- What it produced: scan-output.md

```
I scanned and graded the top 5. All five pass every required check, so all five are accepted.

Ranking and grading used only what the strategy and rubric specify. The query returned 100 results, and 36 survived the filters. I took the top 5 in order: good first issue label first, then fewer comments. All 36 had zero comments, so the order inside the top 5 is just the search order. I did not read it as signal.

Accepted, in fit order

#: 1
Issue: iweidujiang/spring-insight#8 (https://github.com/iweidujiang/spring-insight/issues/8) — add CONTRIBUTING.md
Why it fits: It is a Spring agent-monitoring project, which is the closest match to your Spring/Java-agent
profile. The task is one new file plus a README link. It has a release from today and was filed by the owner.
The issue text is in Chinese.
────────────────────────────────────────
#: 2
Issue: oscarbol09/jvm-mcp#139 (https://github.com/oscarbol09/jvm-mcp/issues/139) — remove the obsolete Java 8
management-agent.jar path in JmxConnectionManager
Why it fits: It is real Java 21 code in an MCP/AI-agent tooling project. It has a release from 10-09 and was filed
by the owner. The project is very active (many contributors, commits within hours), so another contributor could
 pick it up first.
────────────────────────────────────────
#: 3
Issue: bashizip/abada-platform#173 (https://github.com/bashizip/abada-platform/issues/173) — add an HR
leave-request example (leave-request.apl.yaml)
Why it fits: It is a governed AI-agent process platform, but the work is a YAML example, not Java. It has a
release from 10-17 and was filed by the owner.
────────────────────────────────────────
#: 4
Issue: bashizip/abada-platform#172 (https://github.com/bashizip/abada-platform/issues/172) — add an
insurance-claims triage example
Why it fits: Same repo and shape as #173. It is a sibling issue, so don't take both at once.
────────────────────────────────────────
#: 5
Issue: olo-labs/olo-toolgate#17 (https://github.com/olo-labs/olo-toolgate/issues/17) — add a docs table for HTTP
401/403 in debug/README.md
Why it fits: It is docs-only and well scoped. It has no published release, so release-recent fails. It points at
setup-agent-mimic.bat, which suggests a Windows-oriented project, and you said you want to avoid those.

```


- Difficulties: the scan accepted all five, so it did not narrow
  anything down. The ranking had no real signal: all 36 issues that
  survived the filters had zero comments, so the top 5 were just the
  search order. "fits-my-profile" was too loose. It passed a YAML
  example (#173) and two docs-only tasks (#8, #17) even though my
  profile is Java. #17 was accepted even though release-recent failed
  and it looks like a Windows project, which my profile says to avoid.
  It also listed two sibling issues from the same repo (#172, #173) as
  separate picks, and did not flag that the #8 issue text is in Chinese.
  
- What changed: I would make "fits-my-profile" require code in my
  languages (Java/Spark), not just a related topic, and turn "avoid
  Windows-oriented projects" into a check that can reject. I would
  replace the comment-count tiebreak with one that carries signal, such
  as how recently the maintainer replied, and group sibling issues from
  the same repo into one pick.

---

## Reproduction Process

**Environment Setup**

macOS 14.8.9, set up Rust toolchain via rustup (rustc 1.99.1, cargo 1.99.1), 
OpenJdk 21.0.1.  I forked apache/datafusion-comet, cloned my fork, and worked on `main` at commit [hash]. The code for this issue is in the `datafusion-comet-spark-expr` crate (native/spark-expr), so I only needed Cargo for it: `cargo test -p datafusion-comet-spark-expr hash_funcs`
builds the crate and runs the 94 hash tests, which all pass on `main`.
To get expected values that don't come from Comet itself, I ran Apache
Spark 4.1.3 [how: spark-shell / pyspark / local jar] and evaluated
`Murmur3Hash` and `XxHash64` with seed 42 on the same rows. 



**Steps to Reproduce**

This issue is a refactor, not a bug, so "reproducing" it means confirming
the duplication the issue describes and checking what the existing tests
would catch if a refactor got it wrong.

1. In my fork, check out `main` at `962db532f` and open
   `native/spark-expr/src/hash_funcs/utils.rs`. Go to the Map arm
   (L1134-L1249).
2. Compare the four (key, value) pairs: `(Utf8, Int32)`, `(Int32, Utf8)`,
   `(Int32, Int32)`, `(Utf8, Utf8)`. Each has a loop for input without
   nulls and a null-aware loop, and the only differences are the downcast
   types and `.to_le_bytes()`. This is what the issue describes.
3. Run `cargo test -p datafusion-comet-spark-expr hash_funcs`.
   Result: 94 tests pass. The Map tests in `xxhash64_diff.rs`
   (`map_utf8_int32`, `map_int32_int32`, ...) cover all four pairs, but
   only for xxhash64 and only through the null-aware loop.
4. Make one deliberate change to the `(Int32, Int32)` arm, re-run step 3,
   then revert it. Repeat for each change:
   - Hash the value before the key in the null-aware loop →
     `map_int32_int32` fails.
   - Make the same swap in the loop without nulls → all tests pass.
   - Remove the `map_array.is_null(row_idx)` check (a null map that owns
     an entry) → all tests pass.
5. Observed result: two of the three wrong changes pass the existing
   tests, so a refactor of these loops could change hash output and still
   go green. To close that gap, I evaluated Spark 4.1.3's `Murmur3Hash` and
   `XxHash64` (seed 42) on the same rows, [how you ran Spark], and wrote a
   test with those expected values. Current Comet matches Spark on all of
   them, and the new test fails on both changes the old tests miss.

**Reproduction Evidence**

- **Commit showing reproduction:** 
https://github.com/apache/datafusion-comet/compare/main...jinjin1919:datafusion-comet:refactor/6984-map-entry-hashing
- **Screenshots/logs:** 

Benchmark test before and after refactoring. because comet is an accelerator for Spark. so the performance comparsion before and after is done to prove that 
refactoring does not change performance. 
https://github.com/jinjin1919/datafusion-comet/blob/refactor/6984-pr-contributionstory/benmark_test.md


the speed test is noisy on my macOS, so the before and after code get build and compiled to machine code and saved to /private/tmp/claude-501/-Users-jinxu-datafusion-comet/f0e0371e-4b7c-4590-93f9-e446eb067f23/scratchpad/asm/xx-before.txt and /private/tmp/claude-501/-Users-jinxu-datafusion-comet/f0e0371e-4b7c-4590-93f9-e446eb067f23/scratchpad/asm/xx-after.txt 

I ran the diff on those 2 assembly code file. and the diff is empty and it showed two files are IDENTICAL. 

https://github.com/jinjin1919/datafusion-comet/blob/refactor/6984-pr-contributionstory/diff%20on%20before%20and%20after%20machine%20code.png

- **My findings:** 

the before refactoring and after refactoring produced same assembly code, therefore it is a strong evidence that after refactoring it matches Spark, because the old code (before refactoring) did. 


**Toolkit**

- Tool: Claude Code, to make the deliberate changes to the `(Int32, Int32)` arm, re-run the
  `hash_funcs` tests after each one, and revert. A small Java program run with `java -cp`
  on the Spark 4.1.3 `spark-catalyst` and `spark-core` jars from `~/.m2`, to get expected
  `Murmur3Hash` and `XxHash64` values that don't come from Comet. Then criterion (`cargo bench
  --bench hash`) and `cargo rustc --emit=asm` in two worktrees to compare before and after.
- What it produced: the gap in the old tests (2 of 3 wrong changes still passed all 94), the
  new test `test_specialised_map_pairs_match_spark` with Spark's own values, and
  `benmark_test.md` with the two screenshots: noisy timings, then an empty assembly diff for
  `create_murmur3_hashes` and `create_xxhash64_hashes`, and the same SHA-256 for both bench
  binaries.
- Difficulties: the criterion timings swung from -61% to +213% on my laptop, so they could
  not show "no change". Building both commits with a shared target dir would make cargo reuse
  the first build and report a false "identical", so I had to give each worktree its own
  target dir. An empty diff can also mean the check is broken, so I needed a control. And
  some evidence still points at my local scratchpad (`/private/tmp/...asm/xx-before.txt`),
  which no reader can open.
- What changed: I switched from timing to comparing machine code, and added the control i saved build and compiler to assembly code to under  /tmp/claude-501/-Users-jinxu-datafusion-comet/f0e0371e-4b7c-4590-93f9-e446eb067f23/scratchpad/asm/ folder. 
  (murmur3 vs xxhash64 gives 44,694 diff lines, so the diff does see real differences). 

---

## Solution Approach

**Analysis**

The duplication is in the Map arm of the hashing macro in
`native/spark-expr/src/hash_funcs/utils.rs` (L1134-L1249). The arm
handles four (key, value) pairs: `(Utf8, Int32)`, `(Int32, Utf8)`,
`(Int32, Int32)` and `(Utf8, Utf8)`. Each pair has its own copy of two
loops: one for input with no nulls, and one null-aware loop. The eight
loops are the same except for the downcast types and whether the value
goes through `.to_le_bytes()` before it is hashed. Every loop must keep
Spark's byte layout: hash the key before the value, skip a null map, and
skip a null key or value on its own.

The risk is in the tests, not the code. The `xxhash64_diff` Map tests
cover all four pairs, but only for xxhash64 and only through the
null-aware loop. When I made deliberate changes to the `(Int32, Int32)`
arm, swapping key/value order in the loop without nulls passed every
test, and so did dropping the `map_array.is_null(row_idx)` check. So a
refactor could change hash output and still go green.

**Proposed Solution**

Add a `hash_map_entries!` helper modelled on the existing
`hash_list_primitive!`. It takes the key and value arrays plus a
transform for each side (for example `.to_le_bytes()` for Int32, the
raw bytes for Utf8), and the four pairs call it instead of carrying
their own loops. Before touching the loops, I add a test with expected
values from Spark 4.1.3 itself, so the refactor is checked against
Spark, not against Comet's own output.

What I am deliberately NOT changing:

- the fast-path condition that picks the loop without nulls
- the key-before-value order
- the separate null checks (null map, null key, null value)
- the string/binary list hashing, which med9110 offered to do in a
  separate PR
- any hash output: results must stay bit-for-bit identical to Spark

**Implementation Plan**

1. Add a test that runs all four pairs through both Murmur3Hash and
   XxHash64 (seed 42), on two row sets: one with no nulls, and one with
   a null map that owns an entry and a null value. Expected values come
   from Spark 4.1.3's `Murmur3Hash` / `XxHash64`. Confirm it passes on
   `main` and fails on the two changes the old tests missed.
2. In `utils.rs`, add the `hash_map_entries!` macro next to
   `hash_list_primitive!`, keeping the fast-path condition, key-before-
   value order and null checks as they are.
3. Replace the eight loops in the Map arm with four calls to
   `hash_map_entries!`.
4. Re-run `cargo test -p datafusion-comet-spark-expr hash_funcs` (the
   94 existing tests plus the new one).
5. Check performance with the `hash` bench before and after. It only
   has `Utf8 -> Int32` maps today, so I may add an `Int32 -> Int32`
   case.

Deviations:

- Step 5: the `hash` bench timings were too noisy on my macOS to show
  "no change" clearly. Instead I built the bench binary from the old
  `utils.rs` and from the refactor and compared them: the generated
  assembly diff is empty and the binaries have the same SHA-256. Same
  machine code means the refactor cannot change speed or output.
  [Say whether you added the `Int32 -> Int32` bench case or not.]

**Toolkit**

- Tool: ran plan-check skill to grade plan.md and plan-comment.md
- What it produced: 
ran twice. first time failed at scope and missign what files the implementation would touch. 

revised according to the revise to accept comment the skill outputs. 

ran again with plan-check skill,  accepted the plan. 
```
{
  "item": "https://github.com/apache/datafusion-comet/issues/6984",
  "checks": [
    {"name": "grounded-diagnosis", "grade": "pass", "evidence": "Diagnosis explains all three rows of the posted 2026-11-02 change table: null-loop swap fails map_int32_int32; fast-loop swap and dropped is_null(row_idx) pass all 94"},
    {"name": "bounded-scope", "grade": "pass", "evidence": "Not in scope: list arms (med9110), generic fallback, new pairs, hash order changes; Files touch only the utils.rs Map arm plus new macro and one murmur3.rs test"},
    {"name": "executable", "grade": "pass", "evidence": "utils.rs L263/L1121/L1134-L1249 (verified at 962db532f), transforms |v| v and |v: i32| v.to_le_bytes(), 5 ordered steps, bench case decision made"},
    {"name": "decisive-test-plan", "grade": "pass", "evidence": "4 pairs x {no nulls, nulls incl. null map with hidden entry} x {murmur3, xxhash64}, seed 42, vs Spark 4.1.3 values; must fail on change-table rows 2 and 3"},
    {"name": "honest-unknowns", "grade": "pass", "evidence": "Risks: inlining 'I have not shown that yet' with 5%/3-run threshold, bench noise, Spark 4.1.3 only, overlap with med9110"},
    {"name": "thread-and-conventions", "grade": "pass", "evidence": "Follows issue body's 'benchmark that inlining is preserved' and the hash_map_entries! option; repo states no AI-disclosure requirement"}
  ],
  "verdict": "accept"
}
```
- Difficulties: [what fell short, or "none met"]
- What changed: revised the plan-comment.md. the scope and what files i would touch. 

---

## Testing Strategy

A refactor is correct only if the hash output does not change, so every
check below asks one question: does the code still give Spark's exact
values? Full evidence:
[test_evidence.md](test_evidence.md). Branch
`refactor-map-entry-hashing`, based on `962db532f`; macOS 14.8.9,
x86_64, rustc 1.99.1.

**1. Re-running my reproduction against the new test (before the
refactor).** I re-ran the deliberate changes from Steps to Reproduce on
`main` with the new test in place. The old tests missed two of them
(key/value swap in the loop without nulls, and dropping
`map_array.is_null(row_idx)`). The new test fails on both, so it can
catch the mistakes a refactor of these loops could make.

**2. New Spark-checked test.**
`murmur3::tests::test_specialised_map_pairs_match_spark` in
`native/spark-expr/src/hash_funcs/murmur3.rs` fills three gaps in the
existing tests: there was no murmur3 test for these four pairs, nothing
reached the loop without nulls, and no row was a null map that still
holds entries. It builds each of the four pairs in two shapes and checks
both murmur3 and xxhash64:

- Without nulls: rows of 2, 0, 1 and 3 entries. This reaches the loop
  without nulls.
- With nulls: a null map row that still holds an entry (which must not
  be hashed), and a null value whose key must still be hashed. This
  reaches the null-aware loop.

The test also asserts which loop each shape reaches. The expected values
come from Spark 4.1.3's own `Murmur3Hash` and `XxHash64` (seed 42),
which I ran from Spark's jars (`spark-catalyst`, `spark-core`) in a small
Java program, not from Comet's output. It passes before and after the
refactor.

**3. The repo's own test suite.**

```shell
cargo test --manifest-path native/Cargo.toml -p datafusion-comet-spark-expr hash_funcs
```

Result after the refactor: `95 passed; 0 failed`. That is the 94 tests
that passed at `962db532f` (27 murmur3, 14 xxhash64, 51
xxhash64-vs-Spark comparison tests in `xxhash64_diff`, 2 in `utils.rs`)
plus the new test. The existing Map tests (`map_utf8_int32`,
`map_int32_utf8`, `map_utf8_to_utf8`, `map_int32_int32`) still pass.

**4. Lint and format.** From `native/`, `cargo fmt --all -- --check`
and `cargo clippy --all-targets --workspace -- -D warnings` are both
clean.

**5. Performance and inlining.** The issue asks for a benchmark showing
that inlining is preserved. I built the optimized `hash` bench binary
(release, thin LTO) once with the old `utils.rs` and once with the
refactor. Both binaries have the same SHA-256 (`f6a80a36...`), symbol
table included. If any of the 8 closures I pass to `hash_map_entries!`
had not been inlined, it would show up as an extra `{closure#N}` symbol
and the files would differ. Same machine code means the same speed, so I
did not time the two versions and I claim no timing numbers.

**What these checks do not cover**

- The bench only calls `create_murmur3_hashes`, because
  `create_xxhash64_hashes` is `pub(crate)`. The xxhash64 side rests on
  the tests and on both hashes expanding the same macro.
- The binary comparison was done on one target only (x86_64 macOS).
- Identical binaries show the refactor matches the old code, not that
  the old code matches Spark. The Spark-checked test is what covers that.
- I did not run the JVM suites (`CometHashExpressionSuite`, `hash.sql`).

**Toolkit**

- Tool: Claude Code to run the tests overnight, the bench build and the SHA-256 comparison. 
- What it produced: test_evidence.md: 95/95 tests passing,
  fmt/clippy clean, byte-identical bench binaries
- Difficulties: bench timings too noisy on macOS to show "no
  change"
- What changed:  switched from timing the bench to comparing the
  binaries. 

---

## Implementation Notes

**2026-10-30**

Set up on `main` at `962db532f` and ran
`cargo test --manifest-path native/Cargo.toml -p datafusion-comet-spark-expr hash_funcs`:
94 passed, 0 failed (rustc 1.99.1). Read the Map arm in `utils.rs`. The issue says L1110, but
on this commit the arm starts at L1121 and the four pairs are at L1134-L1249, so the line in
the issue is stale. I picked `hash_list_primitive!` (L263) as the model for the new helper,
since it already does the same kind of job for lists. I also had to learn that `cargo` from
the repo root needs `--manifest-path native/Cargo.toml`, because my shell did not stay in
`native/`.

**2026-11-01**

Checked what the existing tests would catch before changing anything. I swapped key and value
order in the `(Int32, Int32)` arm: in the null-aware loop only `map_int32_int32` failed, and
in the loop without nulls all 94 still passed. Dropping `map_array.is_null(row_idx)` also
passed everything. That changed my plan: the test has to come before the refactor, or the
refactor could break hashing and still go green.

So I wrote `test_specialised_map_pairs_match_spark` in `murmur3.rs`: 4 pairs x {no nulls,
nulls with a null map that still holds an entry} x {murmur3, xxhash64}, seed 42. I decided the
expected values must not come from Comet, so I wrote a small Java program and ran Spark
4.1.3's `Murmur3Hash` and `XxHash64` with the `spark-catalyst` and `spark-core` jars from
`~/.m2`. The test passed on unmodified `main` and failed on both changes the old tests missed.

**2026-11-02**

Posted the plan on the issue: the change table above, the new test, and the
`hash_map_entries!` plan, with the string/binary list part left to @med9110. Two decisions
went in the plan. First, no new `Int32 -> Int32` bench case, because all four pairs expand
from the same macro. Second, a stop rule: if a map bench case is more than 5% slower in 3 runs
in a row, diff the release assembly, and if it differs, keep the old loops and report back
instead of opening a PR. I also wrote my ship manifest. Workspace-wide clippy fails here before
it lints anything, because the `proto` crate needs `protoc` and I don't have it installed, so
the gate runs clippy on `datafusion-comet-spark-expr` only and I left the workspace run to CI.
[Maintainer reply on the plan: date and what they said, or "none yet".]

**2026-11-07**

Wrote `hash_map_entries!` next to `hash_list_primitive!` and replaced the eight loops with
four calls (commit `87c95035c`, `utils.rs` +45/-100 lines). The two branches are
copied from the old loops: same `null_count() == 0` check on map, keys and values, key hashed
before value, and the three separate null checks. The only thing that differs per pair is the
transform: `|v| v` for a string side and `|v: i32| v.to_le_bytes()` for an `Int32` side.
I left the `_ =>` fallback arm alone. 95 tests pass (the 94 plus the new one), and fmt and
crate clippy are clean.

One deviation from the plan: the plan said to add the test first and convert the pairs in a
separate step. The test was written and checked first on 11-01, but it was never committed on
its own, so the test and the refactor landed in the same commit.

**2026-11-08**

Wrong turn on performance. I ran the `hash` bench with `--save-baseline before` on `main` and
compared after the refactor. The results swung from -61% to +213%, and the `_forward` and
`_reversed` versions of the same workload moved in opposite directions. That is noise on my
laptop, not a real signal, and it was too noisy to apply my 5% rule. So I used the fallback
my plan already had: compare the machine code. I built `962db532f` and `87c95035c` in separate
worktrees with separate target dirs, because with a shared target dir cargo reuses the first
build and you get a false "identical". The assembly of `create_murmur3_hashes` and
`create_xxhash64_hashes` is identical once `.loc` line markers are removed, and the optimized
bench binaries have the same SHA-256 (`f6a80a36...`). As a control, diffing murmur3 against
xxhash64 gave 44,694 diff lines, so the check does see real differences. I wrote this up in
`benmark_test.md` and committed it with the two screenshots (`9ea3937c2`).

**Toolkit**

- Tool: Claude Code, to write the macro, run the test and lint gates, build the before and
  after binaries, and diff their assembly. Also my ship manifest, which lists the three checks
  the ship-gate runs before the PR (fmt check, crate clippy, `hash_funcs` tests).
- What it produced: the `hash_map_entries!` refactor, 95/95 tests passing, `test_evidence.md`,
  and `benmark_test.md` showing identical assembly and an identical bench binary.
- Difficulties: the criterion timings were too noisy to show anything. Workspace clippy
  couldn't run because `protoc` is missing. And my notes don't all agree: `test_evidence.md`
  and the PR draft say workspace clippy is clean, while the ship manifest says it can't run
  here, and the Testing Strategy names branch `refactor-map-entry-hashing`, but the commit is
  on `refactor/6984-map-entry-hashing`.
- What changed: I switched from timing to comparing the machine code, ran clippy only on the
  changed crate, and [fix the clippy and branch-name wording in test_evidence.md, pr_draft.md
  and Testing Strategy before the PR opens].

---

## Pull Request

**PR Link:** https://github.com/apache/datafusion-comet/pull/7569 

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [2026-11-08]: "none yet"


**Status:** [Awaiting review]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

- learned installing the hook. and fixing hooks if it does not run. for example check the settings.json file under .claude folder that the hook is install and manually run the `python3 \"/Users/jinxu/.claude/ship-gate/check_gate.py\"` to see why I get errors. and realized that I need at least python3.19+ in order to use some of the libraries in check_gate.py. 

- learned the difference between macro and function in rust code. the issue offerred both: "a hash_map_entries! helper macro, or a generic function taking key/value accessor closures that produce impl AsRef<[u8]>."  I picked macro because it is called inside another macro. `create_hash_internal!` is itself a macro.  and in macro, it is Always "inlined", because it's pasted in when a macro expands. 

- learned how to guide the claude code to get the desired result. also ask Claude to tell me how to verify its conclusion and output. and use my judgement to validate claude's response and test result. 

- learned from the shortcoming of this issue-selection and working towards pr. the importance of selecting multiple open source issues. and claim on multiple issues. and try to reproduce some of the issues before deciding working on it. I chose this issue because it is Spark and Java. after understanding the code and read the code. I realized this is actually trying to use rust native to replace JVM hash(). so I actually don't know Rust. so even it is a simple refactor issue. it is a lot to learn and when I started, I simply can only reply on Claude to help me understand things and watched a youtube video about basic Rust syntax. 

- learned the use case for the repo or open source issue we are working on would also help us to think in a way that would make this fix or contribution more meaningful. 


**Challenges Overcome**

- **Working in a language I didn't know.** I picked this issue for the Spark side and only
  found out after reading the code that the work is all Rust macros. I got through it by
  asking Claude Code to explain each part of the Map arm, watching a video on basic Rust
  syntax, and copying the shape of `hash_list_primitive!` instead of designing a helper from
  nothing. Having a working pattern already in the same file made it much smaller to learn.

- **The tests didn't protect the code I was changing.** On 11-01 I made deliberate breakages
  in the `(Int32, Int32)` arm. Swapping key and value in the loop without nulls, and dropping
  the null-map check, both passed all 94 tests. So "tests still pass" would not have shown
  that my refactor was correct. I fixed that before touching the loops: I wrote
  `test_specialised_map_pairs_match_spark`, covering both hashes, both loops, and a null map
  that still holds an entry, and checked that it fails on both breakages.

- **Getting expected values that didn't come from Comet.** Copying Comet's own output into
  the test would only check that Comet agrees with itself. I wrote a small Java program and
  ran Spark 4.1.3's own `Murmur3Hash` and `XxHash64` from the jars in `~/.m2`, so the test
  checks against the JVM implementation that Comet is meant to match.

- **The benchmark was too noisy to tell me anything.** The before and after timings swung
  from -61% to +213%, so my "more than 5% slower in 3 runs" rule couldn't be applied. I used
  the fallback I had written into the plan: compare the machine code. The assembly was
  identical and so was the bench binary's SHA-256. I also ran a control (murmur3 against
  xxhash64) to make sure the comparison could actually find a difference.

- **Being the second person on the issue.** @med9110 had already offered to take part of
  #6984. I asked only for the Map part they had left out, offered to step back, and kept my
  plan and PR inside the Map arm so our changes don't overlap.

- **Tools that broke along the way.** My issue-scout rejected every issue at first because
  "unclaimed" was too strict, so I relaxed it. My voice-guard hook failed until I found it
  needed a newer Python. And workspace-wide clippy can't run here without `protoc`, so I
  scoped my ship-gate to the changed crate and left the full workspace run to CI.

**What I'd Do Differently Next Time**

- **Read the code before I choose an issue.** I picked #6984 for its Spark connection and
  only found out it was all Rust macros after I read `utils.rs`. Next time I would open the
  files an issue names and try a quick reproduction on two or three candidates before I
  claim one. I would also add a language check and a split-claim check to my issue-scout,
  since both of those surprised me here.

- **Try the cheap, reliable check before the noisy one.** I ran the criterion benchmark
  first and got swings from -61% to +213% that told me nothing. Comparing the machine code
  is what settled it. For a refactor that shouldn't change behavior, I would compare the
  assembly or binaries first and only time the code if they differ.

- **Commit the test on its own, before the change.** I wrote and checked the Spark-checked
  test before the refactor, but it went into the same commit. If the test were its own
  commit, a reviewer could check out that commit and see the test pass on the old code.

- **Keep my evidence somewhere it won't disappear.** The assembly files from the comparison
  are in a temporary session folder, and the Java program that produced the Spark values
  isn't saved in my repo. Next time I would put the scripts and outputs in a folder I keep,
  so anyone can rerun them, including me.

- **Keep my notes in sync as things change.** My test evidence and PR draft still say
  workspace clippy is clean, but my ship manifest says it can't run here, and one section
  names the wrong branch. I would update every note at the moment a fact changes, rather
  than finding the mismatches at the end.

- **Ask Claude how to check its answer, every time.** The checks that helped me most were
  the ones I asked for: the deliberate breakages, the control diff, and the separate target
  folders. I would ask "how would we know if this is wrong?" from the start, not only once a
  result looked suspicious.

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [rust and cargo doc](https://doc.rust-lang.org/cargo/getting-started/installation.html)
- [Issue #6984](https://github.com/apache/datafusion-comet/issues/6984): andygrove's issue
  body gave the two options (a `hash_map_entries!` macro or a generic function with
  closures) and asked for a benchmark showing inlining is kept. med9110's 10-16 comment
  showed me which part was already taken, so I claimed only the Map part.
- [Comet Development Guide](https://github.com/apache/datafusion-comet/blob/main/docs/source/contributor-guide/development.md):
  the build order, and the "Submitting a Pull Request" steps I used for my ship manifest
  (format, clippy, Rust tests).
- [`pr_build_linux.yml`](https://github.com/apache/datafusion-comet/blob/main/.github/workflows/pr_build_linux.yml):
  where I found the `cargo fmt --check` form of the format step, which checks without
  rewriting files.
- [`hash_list_primitive!` in `utils.rs`](https://github.com/apache/datafusion-comet/blob/962db532f/native/spark-expr/src/hash_funcs/utils.rs#L263):
  the existing macro I modelled `hash_map_entries!` on.
- [`xxhash64_diff.rs`](https://github.com/apache/datafusion-comet/blob/962db532f/native/spark-expr/src/hash_funcs/xxhash64_diff.rs):
  the existing Map tests. Reading them showed me they cover only xxhash64 and only the
  null-aware loop, which is the gap my new test fills.
- Apache Spark 4.1.3 `spark-catalyst` and `spark-core` jars from my local `~/.m2`: I ran
  `Murmur3Hash` and `XxHash64` from them in a small Java program to get the expected values
  for my test.
- [YouTube video on basic Rust syntax: title and link]
- Claude Code: explained the Rust macros to me, ran the tests and builds, and did the
  assembly comparison. I checked its results with the deliberate breakages and the control
  diff.
