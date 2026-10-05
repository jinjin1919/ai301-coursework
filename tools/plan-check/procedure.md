# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

Read in this order, every time, and take the noted-down fact with you
rather than re-deriving it later:

1. **Repo facts** (eval: the `## Repo facts` block; live: the repo's
   CONTRIBUTING docs and templates per `scope.md`). Note any stated
   AI-use disclosure requirement and contribution template before
   reading anything else — `thread-and-conventions` needs this as a
   fixed fact, not something to go hunt for after the comment reads
   fine.
2. **Issue + thread highlights** (eval: `## Issue` and `## Thread
   highlights`; live: the issue thread). Note any explicit maintainer
   direction — a named cause, a requested approach, an open PR already
   in flight.
3. **Repro evidence** (eval: `## Repro evidence`; live: the student's
   posted repro comment, or the house repro pack on the house issue).
   Note every numbered step with its expected/actual result, and flag
   any control run explicitly — it is the fact `grounded-diagnosis`
   will be checked against.
4. **Candidate plan**, in its own section order: Diagnosis, Scope,
   Files, Approach, Test plan, Risks. Note each section's claim as you
   pass it.
5. **Candidate plan comment**, read last, against everything noted
   above.

This order matters because the diagnosis and scope checks are only as
good as the repro evidence and thread direction read *before* the
plan — reading the plan first invites grading it on its own confident
terms instead of against what actually happened.

## Evidence gathering

| Check | Pull from | Record |
|---|---|---|
| grounded-diagnosis | Repro evidence's steps/controls (step 3 above) vs plan's Diagnosis | the step or control, if any, the stated cause fails to explain |
| bounded-scope | Plan's Scope (not-in-scope line) vs its own Files/Approach | any file, module, or subsystem touched that the not-in-scope line didn't promise |
| executable | Plan's Files and Approach | any step with no named file, or language marking a deferred decision ("somewhere," "maybe," "whichever") |
| decisive-test-plan | Plan's Test plan vs repro evidence's steps/artifacts | the specific input->outcome the test plan names, or its absence |
| honest-unknowns | Plan's Risks vs its Scope/Approach commitments | an unknown named with a reason, or a commitment with no corresponding risk line |
| thread-and-conventions | Plan comment vs thread highlights and repo-facts policy | the maintainer direction (if any) the comment addresses or contradicts; whether disclosure is present when required |

A check whose evidence is not written down by the time step 5 finishes
has not been gathered — go back to the relevant read-order step rather
than inferring it from memory.

## Check execution

Grade in the rubric's table order (grounded-diagnosis, bounded-scope,
executable, decisive-test-plan, honest-unknowns,
thread-and-conventions). Each check grades only against the evidence
recorded for it; do not re-read the whole package per check, only the
one or two noted facts, except when gathering `thread-and-conventions`,
which always rereads the comment itself since no earlier step read it
closely.

- If the section a check's evidence should come from is **entirely
  absent** from the package (no Risks section at all, no Test plan
  section at all), grade that check `fail` directly — an absent
  section is itself the defect `executable` or `decisive-test-plan` is
  watching for, not an unclear case.
- Grade `unclear` only when the relevant section exists but, even after
  rereading it once against its evidence-guide entry, you cannot tell
  whether the pass condition is met (e.g. the repro evidence has no
  control run either way, so `grounded-diagnosis` has nothing to
  confirm or rule the cause out with).
- A `fail` on `grounded-diagnosis` does not stop grading the rest: grade
  every other check against what the plan actually says, even when the
  cause underneath it is wrong. The verdict rule, not early exit, is
  what turns that into a reject.

## Verdict assembly

Apply the rubric's verdict rule exactly: `accept` only if
grounded-diagnosis, bounded-scope, executable, decisive-test-plan, and
thread-and-conventions are all `pass`; any of those five graded `fail`
or `unclear` makes the verdict `reject`. `honest-unknowns` is
`preferred` — its grade is reported but never flips the verdict.

For the output JSON's `evidence` field on each check, quote the exact
fact recorded in the Evidence-gathering table for that check (the
specific step number, the specific file name, the specific missing
section) — never a paraphrase like "looks fine" or "plan is bounded."
The deciding check for a `reject` verdict is whichever required check
failed first in table order; name it in the pre-JSON summary.
