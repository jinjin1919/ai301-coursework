# My first Apache PR: refactoring Spark-compatible map hashing in DataFusion Comet

**PR:** https://github.com/apache/datafusion-comet/pull/7569 (part of issue #6984)


---

## Slide 1: Title (0:00 to 0:30)

**On screen**
- My first Apache PR: refactoring Spark-compatible map hashing in DataFusion Comet
- Screenshot: the purple "Merged" badge on PR #7569
- Your name and GitHub username

**Say**

> My first open-source contribution was just merged into Apache DataFusion Comet. It's a refactor of how Comet hashes map columns. The code change itself was small. What I want to talk about is how I proved it didn't change any results, because in this project being "the same as before" isn't enough. It has to be the same as Spark.

---

## Slide 2: What Comet is and why hashes matter (0:30 to 1:15)

**On screen**
- Spark (JVM) → Comet (native Rust): same query, same results, faster
- `hash(map)` in Comet must equal `hash(map)` in Spark, bit for bit
- Hashes decide joins, partitioning, and shuffles

**Say**

> Comet is an accelerator for Apache Spark. It takes Spark operations and runs them in native Rust instead of on the JVM. Users just turn it on, so Comet has to give exactly the same answers Spark would. That includes hash values. Spark uses hashes to decide which partition a row goes to and to match rows in joins. If Comet hashed even one map differently from Spark, a row could end up in the wrong place and a query could silently return wrong results.

---

## Slide 3: The problem: eight copies of one loop (1:15 to 2:00)

**On screen**
- File: `native/spark-expr/src/hash_funcs/utils.rs`, Map arm of `create_hashes_internal!`
- A 4 × 2 grid: four (key, value) pairs × {fast loop, null-aware loop} = 8 hand-written loops
  - (Utf8, Int32), (Int32, Utf8), (Utf8, Utf8), (Int32, Int32)
- Only differences: the array types, and `.to_le_bytes()` on an Int32 side

**Say**

> The issue was duplication. For maps, the hashing code handled four key and value type pairs, and each pair had two loops: a fast one for data with no nulls and a null-aware one. That's eight loops that were almost identical. Any change to how map entries are hashed had to be made eight times, and every copy had to keep Spark's rules: hash the key before the value, skip a null map, and skip a null key or value on its own.

---

## Slide 4: The fix: one macro (2:00 to 2:35)

**On screen**
- New `hash_map_entries!` macro, modelled on `hash_list_primitive!` in the same file
- Strings pass `|v| v`, Int32 passes `|v: i32| v.to_le_bytes()`
- `utils.rs`: +45 / −100 lines

**Say**

> The fix was one macro, `hash_map_entries!`. I based it on a macro that already did the same job for lists in the same file. Each pair now passes in a small transform: strings pass the bytes through as they are, and integers convert to little-endian bytes first. I used a macro rather than a function because it's called inside another macro. That cut about a hundred lines down to forty-five. And Rust was a new language for me, so copying an existing pattern made this much easier to learn.

---

## Slide 5: The hard part: proving nothing changed (2:35 to 3:50)

**On screen**
Three checks, one line each:
1. **Would the tests notice?** Broke the code on purpose → all 94 tests still passed
2. **Test against Spark, not Comet.** Expected values from Spark 4.1.3's own `Murmur3Hash` and `XxHash64`
3. **Performance.** Timings swung from −61% to +213% → compared binaries instead: byte-identical (same SHA-256)

**Say**

> Writing the macro was the easy part. Proving it was correct was the hard part.
>
> First, before changing anything, I asked: would the existing tests even notice a mistake? I broke the code on purpose. I swapped the key and value order, and I removed the null-map check. All 94 tests still passed. So "tests pass" would have told me nothing.
>
> Second, I wrote a test whose expected values came from running Spark's own hash functions, not from Comet's output. Otherwise I'd only be checking that Comet agrees with itself. That test failed on both of my deliberate breakages, which showed me it could actually catch them.
>
> Third, performance. The benchmark timings on my laptop swung from minus 61 to plus 213 percent, which was pure noise. So I built the optimized binary from the old code and from the new code, and compared them. They were byte-for-byte identical. Same machine code means same speed, and it also proves the compiler inlined everything.

---

## Slide 6: Review made it better (3:50 to 4:45)

**On screen**
Reviewer asked for three changes, and all three were made:
1. Move ~100 lines of hardcoded hash values into SQL tests (`hash.sql`), so Spark computes them at run time
2. Confirm whether a Parquet map column reaches the fast loop
3. Add a test for sliced maps → `test_specialised_map_slice_matches_unsliced`

Result: 96 Rust tests pass, 24/24 CI checks pass, approved ("LGTM") and merged

**Say**

> The reviewer checked the macro against the old code and found no behavior change, but asked for three things. First, my Spark test had about a hundred lines of hardcoded hash numbers. They suggested moving those checks into the project's SQL tests, so Spark computes the expected values at run time and nobody has to keep the numbers in sync. Second, confirm whether data read from Parquet would reach the fast loop. Third, test a sliced map. That one was a real gap: after slicing, the offsets don't start at zero, and nothing tested that before. I made all three changes, all 24 CI checks passed, and the maintainer approved it and merged it.

---

## Slide 7: Limitations, lessons, and what's next (4:45 to 5:30)

**On screen**
- **Limits:** benchmark only reaches murmur3, binary comparison done on one machine (x86_64 macOS)
- **Lessons:** ask "how would we know if this is wrong?" first, and try the cheap reliable check (compare binaries) before the noisy one (timings)
- **Next:** the string/binary list half of #6984 (handled by another contributor), and add an Int32 → Int32 map case to the benchmark

**Say**

> There are limits. The benchmark can only call the murmur3 path, so xxhash64 relies on the tests. And I compared binaries on one machine only.
>
> Two lessons I'll keep. Start by asking "how would we know if this is wrong?", inversion always works. because breaking the code on purpose taught me more than any passing test. And try the cheap, reliable check before the noisy one.
>
> Next, the other half of the issue, the list hashing, is being handled by another contributor, and I'd like to add more map cases to the benchmark. Thank you. I'm happy to take questions.

---


