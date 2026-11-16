# My first Apache PR: refactoring Spark-compatible map hashing in DataFusion Comet

## Overview

PR: https://github.com/apache/datafusion-comet/pull/7569 (part of issue #6984)

Apache DataFusion Comet speeds up Apache Spark by running Spark operations in native Rust code instead of on the JVM. Because it replaces Spark's own code, its results must match Spark exactly, including hash values.

In `native/spark-expr/src/hash_funcs/utils.rs`, the Map arm of the `create_hashes_internal!` macro had the same hashing loop written out by hand four times, once for each (key, value) type pair: (Utf8, Int32), (Int32, Utf8), (Utf8, Utf8) and (Int32, Int32). Each copy had two loops: a fast loop for input with no nulls and a null-aware loop. That made eight loops that differed only in the array types and in whether an Int32 side went through `.to_le_bytes()` first. I replaced them with one `hash_map_entries!` macro, based on the `hash_list_primitive!` macro already in the same file. The change was 45 lines added and 100 removed in `utils.rs`.

Writing the code was the easy part. The hard part was proving that the hash results did not change and still matched Spark.

## What happened
**Checking whether the existing tests would catch a mistake.** Before changing anything, I broke the (Int32, Int32) arm on purpose. Swapping the key and value order in the fast loop still passed all 94 existing tests, and so did removing the null-map check. The existing map tests only covered xxhash64 and only the null-aware loop. So "tests still pass" would not have proved my refactor was correct.

**Testing against Spark itself.** I wrote a test covering all four pairs for both murmur3 and xxhash64, through both loops. Its expected values came from running Spark 4.1.3's own `Murmur3Hash` and `XxHash64`, not from Comet's output. I confirmed that it failed on both of my deliberate breakages and passed on `main`.

**Checking that performance didn't change.** The before and after benchmark timings on my Mac varied from -61% to +213%, which was too noisy to tell me anything. Instead, I built the release benchmark binary from the old code and from the refactored code. The two were byte-identical (same SHA-256). That means every closure passed to the macro was inlined, and the compiled code is exactly what the hand-written loops produced.

**Review and revision.** @comphead compared the old arms with the macro expansion and found no behavior change. They made three requests:

1. Move the ~100 lines of hardcoded murmur3/xxhash64 values out of the Rust test and into the SQL tests in `expressions/hash/hash.sql`, so Spark computes the expected values at run time and nothing has to be kept in sync by hand.
2. Confirm whether a Parquet map column would reach the null-free fast loop.
3. Add a test that hashes a sliced map (for example `array.slice(1, n)`).

I made all three changes. The `hash.sql` cases now compare all four pairs against Spark in both the null-free and null-aware shapes. I confirmed the Parquet case. I also added `test_specialised_map_slice_matches_unsliced`, which checks that a sliced map hashes the same as the matching rows of the unsliced array, in both loops. This matters because the macro reads keys and values through offsets that don't start at 0 after slicing, and nothing tested that before. I also added `test_specialised_map_null_row_skips_owned_entries`, which checks that a null map row that still holds an entry is skipped.

**Merged.** All 96 Rust hash tests passed and all 24 CI checks passed. @andygrove approved the PR ("LGTM") and merged it into `main` through the merge queue.


## Limitations and what's next


**Limitations**

- **The benchmark only covered murmur3.** The `hash` bench calls `create_murmur3_hashes`. `create_xxhash64_hashes` is `pub(crate)`, so the bench can't reach it. The xxhash64 side relies on the tests and on both hashes expanding the same macro.
- **Identical binaries prove "same as before", not "same as Spark".** Only the Spark-checked test covers correctness against Spark 4.1.3, and it covers four key/value pairs: (Utf8, Int32), (Int32, Utf8), (Utf8, Utf8), (Int32, Int32).
- **I didn't run the JVM suites locally** (`CometHashExpressionSuite`, `hash.sql`). I relied on CI for those, and on CI for workspace-wide clippy, because it needs `protoc`.
- **My evidence isn't all reproducible.** The Java program that generated the Spark hash values and the assembly diff files were kept in a temporary session folder, not in my repo.

**What's next**

- The other half of issue #6984, reusing `hash_list_primitive!` for string/binary list elements, was left to @med9110 for a separate PR. I can review it or pick it up if it stalls.
- Add an `Int32 -> Int32` map case to the `hash` bench (it only has `Utf8 -> Int32` today) and find a way to benchmark the xxhash64 path too.
- Commit the Spark-checked test on its own before the refactor, so a reviewer can see it pass on the old code.
- Save the Java program and comparison scripts in my repo so anyone can rerun them.
- For my next contribution: read the code an issue touches before claiming it (I picked this one for Spark and found out it was all Rust macros), and try the cheap check like comparing binaries before the noisy one like timing benchmarks.
