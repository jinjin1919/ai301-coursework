My first open-source contribution just got merged into Apache DataFusion Comet, the Rust engine that speeds up Apache Spark. I replaced eight near-identical map-hashing loops with one Rust macro, and showed the hash results still match Spark exactly. 

I don't know Rust when I started. I picked the issue for its Spark connection and only found out after reading the code that the work was all Rust macros. I used Claude Code to walk me through the code piece by piece, validate each response Claude Code gave and asking it how I could check that its answer was right. 

Before changing anything, I tested whether the existing tests would catch a mistake. They didn't: swapping key and value order still passed all 94. So I first added a test whose expected values come from running Spark's own hash functions. Then I made the refactor and confirmed the inlining and the optimized build was identical to the original. 

Now I'm ready for harder issues. https://github.com/apache/datafusion-comet/pull/7569  

#OpenSource #ApacheSpark #Rust #DataFusion

