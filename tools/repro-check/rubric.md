# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment line (tool + version, OS, commit/tag) | Pass if tool version, OS, and the exact commit/tag/build are all named, precisely enough that a reader could check out the same state; fail if any of the three is missing or vague ("latest", "my machine"). | required |
| Steps reproducible from a stated start | The numbered steps in the repro report, from starting state through the command run | Pass if the steps start from a named, obtainable starting state (fresh clone, a specific tag/checkout) and list every step needed to reach the run, including the exact command; fail if a step is skipped, implied, or depends on information only the author has. | required |
| Expected vs actual stated | The expected/actual lines in the repro report | Pass if both what was expected and what happened are stated as concrete, checkable outcomes (exit code, output line, specific behavior); fail if either side is vague, general, or missing. | required |
| Artifact backs the actual | The pasted output/log/screenshot attached to the report | Pass if the actual outcome claimed above appears verbatim in the attached artifact, shown rather than merely asserted; fail if the artifact is missing, truncated past the relevant line, or does not contain the claimed text. | required |
| Behavior matches the issue's target | The artifact's content read against the issue's description | Pass if the error or behavior shown is the same one the issue describes (same failure mode, same code path); fail if the artifact shows an adjacent bug, a different error, or a difference from the issue that isn't called out as such. | required |
| Outcome stated honestly | The claim comment's verdict language against the artifact and steps | Pass if the comment claims no more than the evidence supports — an evidenced "could not reproduce" counts as a pass; fail if the comment asserts a reproduction or confirmation the artifact doesn't actually show. | required |
| Claim comment states specific, honest intent | The candidate claim comment's own text | Pass if it names something specific to this issue (what was checked, what's planned next) grounded in the repro; fail if it is interchangeable assign-me boilerplate ("I'll take this") or promises an outcome a first-time contributor can't actually guarantee (a fixed delivery date, certainty of root cause). | required |
| AI-use disclosure follows repo policy | The repo-facts/contribution-policy block's stated AI-use rule, against the claim and repro comments | Pass if the repo states no AI-disclosure requirement, or states one and the comment discloses; fail only if the repo requires disclosure and the comment omits it. A repro that is otherwise flawless still fails here if disclosure is required and missing. | required |

## Verdict rule

Accept only if every required check passes. `unclear` on a required
check counts as fail and holds the package — proof that can't be
verified from the package isn't proof yet. There are no preferred
checks in this rubric: every family the eval set forces (environment,
steps, behavior-matches-target, honesty, comms specificity, and
disclosure) gates the verdict on its own, since a package can be
flawless everywhere else and still fail on exactly one of them.
