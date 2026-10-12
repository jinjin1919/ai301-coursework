# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

Read the whole package in the order below before grading any check. At each step, write down the listed notes; the checks later compare these notes side by side, so do not grade from memory of the raw text.

1. **The issue.** Read the issue and any explicit maintainer direction in the thread (approach, naming, scope asks). Note: the reported behavior in one line, the reproduction steps, and each maintainer instruction.
2. **The plan.** Read `plan.md` (live) or the plan-context block (eval) in full, including deviation notes. Note: the scope/boundary (every file or area the plan names), each deliverable the plan promises, each deviation note and what it re-ties, and the plan's test plan.
3. **The repo's asks.** Read the repo's PR template, stated policy and checks (live: the template and CONTRIBUTING.md; eval: the repo-facts block). Note: each template section, any disclosure ask (such as AI use), and each check the repo says must be run.
4. **The diff.** Read the diff before the description. Note: the list of every changed file, what each hunk does in one line, and any hunk that looks unrelated to the plan (debug leftovers, dead or commented-out code, formatting-only changes).
5. **The commits.** Read the commit list. Note: what each commit says it does, and any commit that touches something step 4 did not expect.
6. **The description.** Read the PR title and description. Note: every claim it makes about what changed, whether it says it fully delivers the plan, any shortfall or deviation it admits, which template sections it fills, whether it links the issue, and any disclosure.
7. **The test evidence.** Read the test-evidence section last. Note: what was actually run, the output or result shown, whether a before and an after are both visible, and the outcome of each of the repo's checks from step 3.

Why this order: the issue and the plan come first because they are the promise everything else is graded against. The diff comes before the description on purpose: reading the description first lets its framing tell you what the diff "means", and silent drift hides in exactly that gap. The description comes after both, so each of its claims can be checked against the plan's promise and the diff's contents. The test evidence comes last because it is read against the plan's test plan, the issue's reproduction, and the behavior the diff changes, all of which must already be in hand.

If a part in the order is missing from the package, do not skip it silently: note "absent" for it and carry that into the check that depends on it.




## Evidence gathering

Gather the evidence for every check from the notes taken in the read order, as pairings. Write each record down before grading. Use only what the package contains (live: the plan, the diff from `git diff main...HEAD`, the draft and the quoted test output; eval: the bundle text). Where a part is absent, record "absent".

| Check | Pull from | Record |
|---|---|---|
| Diff stays inside plan | Changed files (step 4) vs the plan's scope and deviation notes (step 2) | each changed file marked in-scope, covered by a deviation note (quote it), or neither; every "neither" |
| Plan delivered or shortfall disclosed | Plan's deliverables (step 2) vs the diff (step 4), then the description's admissions (step 6) | for each deliverable, the hunk that delivers it or "not found"; for each "not found", the quoted admission in the description or deviation notes, or "none" |
| Description matches diff | Description's claims (step 6) vs the diff's hunks (step 4) | each claim marked backed (name the hunk), loose wording with substance holding (name the hunk and the loose word), unsupported (a claimed deliverable or behavior with no hunk), or contradicted; any change in the diff the description never mentions |
| Proof is present | Test-evidence section (step 7) | the literal items shown (command and output, test name and result, log, screenshot), or "statement only" if there are none |
| Proof is decisive | Test-evidence section (step 7) vs the issue's reported behavior and repro (step 1) and the plan's test plan (step 2) | whether a before and an after are both shown for the reported behavior, whether the evidence exercises the changed code, whether it would also pass without the fix, and any test-plan step with no matching evidence |
| Repo checks run | Test-evidence section (step 7) vs the repo's required checks (step 3) | for each required check, its visible outcome, the stated reason it was not run, or "nothing shown" |
| Diff is reviewable | Hunk notes (step 4) | every hunk that is debug output, a TODO scrap, dead or commented-out code, formatting-only churn, or an unrelated drive-by edit, with file and line; or "none" |
| Template and disclosure honored | Description's sections (step 6) vs the repo's template and policy (step 3) | each template section marked real content, boilerplate, or blank; whether a required disclosure is present |
| Links the issue and thread direction | Description (step 6) vs the issue and maintainer instructions (step 1) | whether the issue is referenced; each maintainer instruction marked followed, answered, or ignored |
| Commits are understandable | Commit list (step 5) | each commit message marked "says what and why" or "empty or vague" (quote the vague ones) |

A check whose evidence is not written down by the time gathering finishes has not been gathered. Go back to the relevant read-order step rather than inferring it from memory.

## Check execution

Grade in the rubric's table order (Diff stays inside plan, Plan delivered or shortfall disclosed, Description matches diff, Proof is present, Proof is decisive, Repo checks run, Diff is reviewable, Template and disclosure honored, Links the issue and thread direction, Commits are understandable). Each check grades only against the evidence recorded for it in the gathering table; do not re-read the whole package per check, only the one or two noted facts the record points to. Grade each check against its rubric pass condition, not against how the package feels.

- If the part a check's evidence should come from is **entirely absent** from the package (no test-evidence section at all, no plan or deviation notes at all, no commit list, no description), grade that check `fail` directly. An absent part is itself the defect the check is watching for, not an unclear case.
- Grade `unclear` only when the relevant part exists but, even after re-reading it once against its evidence-guide entry, you cannot tell whether the pass condition is met (for example, the test output shows a pass but does not say which test ran, so `Proof is decisive` has nothing to confirm or rule out the fix).
- A `fail` on one check does not stop grading the rest. Grade every other check against what the package actually contains, even when an earlier check failed. The verdict rule, not early exit, turns the fails into a reject.

How each check runs, once its record is in hand:

- **Diff stays inside plan:** fail if the record lists any changed file marked "neither". A deviation note counts only if it names the change and says why; a note that merely lists the file does not cover it.
- **Plan delivered or shortfall disclosed:** fail if any deliverable is "not found" and the record shows no quoted admission. A "not found" with a plain admission passes; do not fail an honest shortfall.
- **Description matches diff:** fail if any claim is marked unsupported or contradicted. A claim marked "loose wording, substance holds" passes; do not fail a description over a count or granularity word when the hunk it describes is in the diff. An unmentioned change in the diff fails only when it is substantive or the description says it changes nothing else; a one-line tweak the description leaves out does not.
- **Proof is present:** fail if the record is "statement only". Pass if at least one literal item (output, test result, log, screenshot) is shown, whatever else is wrong with it; whether it is good enough is the next check.
- **Proof is decisive:** pass only if the record shows a before and an after for the reported behavior (or a fail-then-pass test), exercising the changed code. Fail if the evidence would also pass without the fix or does not touch the changed code. Grade `unclear` if the evidence exists but you cannot tell either way. If `Proof is present` failed for an absent section, this check fails as well.
- **Repo checks run:** fail if any required check's record is "nothing shown". Pass a check with a stated reason it could not be run. If the repo names no required checks, pass.
- **Diff is reviewable:** fail if the record lists any debris or unrelated hunk. A single trivial leftover that does not obscure the fix is still listed; the pass condition says every hunk serves the change, so grade the record, not your impression.
- **Template and disclosure honored:** fail if any required section is blank or boilerplate, or a required disclosure is missing. If the repo states no template or disclosure ask, pass.
- **Links the issue and thread direction:** fail if the issue is not referenced, or any maintainer instruction is marked ignored. If the thread holds no maintainer instruction, grade on the issue link alone.
- **Commits are understandable:** grade from the commit marks. This check is `preferred`; grade it as usual and report it in the evidence line, but it never changes the verdict.

For every check, the `evidence` line in the output quotes the one fact from the record that decided it (a file name, a quoted claim, a missing section), never "looks fine".

## Verdict assembly

Apply the rubric's verdict rule to the grades, and only that rule:

1. If every `required` check is `pass`, the verdict is `accept`.
2. If any `required` check is `fail` or `unclear`, the verdict is `reject`. `unclear` enters the rule as `fail`.
3. `preferred` checks never change the verdict, whatever their grade.

Name the deciding check in your summary: the first `required` check, in rubric table order, that is `fail` or `unclear`. Quote its recorded evidence (the file, claim, or missing section that decided it) in the summary line for the verdict. List the other failing checks after it, without re-deciding which one counts. On `accept`, there is no deciding check; say that every required check passed.

Then write the JSON block: one entry per check, with the grade as you gave it (`unclear` stays `unclear` in the JSON even though it counts as `fail` for the verdict), and the verdict you just assembled.
