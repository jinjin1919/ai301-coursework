# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-diagnosis | The plan's stated cause (Diagnosis section) read against every step and control result in the repro-evidence block. | Pass if the stated cause explains all of the repro evidence's observed behavior, including any control runs; fail if a control run or a step rules the cause out, or the diagnosis is silent on behavior the evidence shows. | required |
| bounded-scope | The plan's in-scope/not-in-scope statement, read against its own Files and Approach sections. | Pass if a not-in-scope line exists and the Files/Approach sections name only changes inside the stated in-scope boundary; fail if the approach adds a rewrite, migration, redesign, or new feature the issue did not ask for. | required |
| executable | The Files and Approach sections. | Pass if named files/locations plus an ordered approach let someone who has never seen the issue can get started working easily; fail if any real decision is deferred ("somewhere," "whichever is easier," "not sure which layer"). | required |
| decisive-test-plan | The Test plan section read against the repro evidence's steps and artifacts. | Pass if the test plan names a specific input and a specific observable outcome (output, exit code, re-run of a repro step) that would confirm the fix; fail if it names no observable outcome ("should feel fixed," "run the full suite") or doesn't map back to the repro evidence. | required |
| honest-unknowns | The Risks section, read against what the Scope/Approach sections actually commit to. | Pass if deferred work or real uncertainty is named explicitly, with a reason; fail if the plan presents untested claims or deferred work as settled, or omits risks a reader would need to know about. | preferred |
| thread-and-conventions | The candidate plan comment read against the thread highlights and the repo-facts block (contribution policy, stated AI-use disclosure requirements). | Pass if the comment does not contradict explicit maintainer direction already in the thread, and includes AI-use disclosure when the repo-facts block says the repo requires it; fail on either violation. | required |

## Verdict rule

Accept only if every `required` check passes. Any `required` check that
is `fail` or `unclear` holds the package at `reject` — `unclear` is
treated as `fail` throughout, since a plan the package doesn't let you
verify is not a plan that's ready to build from. `preferred` checks
(`honest-unknowns`) never flip the verdict; a `fail` there is reported
in the summary but doesn't block posting.
