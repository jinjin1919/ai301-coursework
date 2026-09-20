# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Reading dates

Every recency threshold below is measured against the **capture date**
stamped at the top of the bundle (eval mode), or against today (live
mode). Never against the date the grading run happens to be executed.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | the "maintainer first-response sample" list and the "last 5 default-branch commits" list, both under Repo facts | Passes if EITHER: (a) at least one sampled issue that was opened within 12 months of the capture date shows a first reply from an Owner/Member/Collaborator, OR (b) at least one of the last 5 default-branch commits is dated within 90 days of the capture date. A bot-authored commit counts only when it merges a human's pull request. Entries reading "no maintainer comment in thread" are not evidence of death on their own, and a sample with fewer than 3 entries is graded on (b) alone. | required |
| repo-in-use | the repo line (`archived:`), `last push to any branch`, the newest of the last 5 default-branch commits, and `latest release`, all under Repo facts | Passes if `archived: no` AND the newest default-branch commit is within 90 days of the capture date. A repo with no published releases can still pass: commits carry liveness. Release recency is graded separately as a preferred check, never here. | required |
| scope-bounded | the issue title and body, its labels, and the full comment thread | Passes if the work asked for could land as ONE pull request serving one goal. Fails if ANY of: (a) the issue calls itself a tracking, meta, umbrella, or mega-issue, or its items are independent pieces of work that would each become their own PR (e.g. "annotate every module", "translate all the pages");  (b) the thread shows the design is still being argued with no maintainer decision settling it; (c) it is a feature wish with no agreed specification of the desired behavior and no maintainer endorsement; (d) it is a usage or support question rather than a change; A `good first issue` label does not satisfy this check. | required |
| unclaimed | `this issue: assignees:` and `linked PRs:` (with state) under Repo facts, plus every comment in the thread | Passes if ALL of: no assignee is set; no linked PR is in state `open`; and no claim comment ("I'll take this", "working on this", "can I work on this") dated within 12 months of the capture date stands unanswered. Linked PRs in state `closed` or `merged` are abandoned or unrelated attempts and do NOT block. A claim older than 12 months with no follow-up is stale and does NOT block, especially where a maintainer has since invited takers. Issue-template boilerplate telling contributors how to claim is not itself a claim. A bot nudge asking whether work is still active does not clear a live claim. | required |
| policy-allows-ai | the `contribution policy` line under Repo facts, including the file or section it names | Passes unless the policy bans AI-assisted contribution outright with no carve-out for assisted work that a human understands and takes responsibility for. Fails on wording like "we do not accept AI-generated code or documentation." Passes on conditions: disclosure, personal understanding, testing, human review, or a ban on *fully* AI-generated work where assistive use is explicitly allowed. Silence passes; most repos state nothing, and that is not a restriction. An `AGENTS.md` file is the opposite signal and never fails this check. | required |
| release-recent | `latest release` under Repo facts | A release published within 90 days of the capture date. | preferred |
| maintainer-filed | the `opened by ... (ASSOCIATION)` line on the issue | The issue was opened by an OWNER, MEMBER, or COLLABORATOR: the person who decides what "done" means is already in the thread. | preferred |
| fits-my-profile | the fit profile in `scope.md` | The issue's language, stack, or subject matter matches the fit profile. Ranking only: this check can never accept or reject an issue. | preferred |

## Verdict rule

Emit `accept` if every `required` check grades `pass`. 
Emit `reject` if any `required` check grades `fail`.

`unclear` counts as `fail` on every required check. The one
deliberate exception is written into `maintainer-alive` above, where a
thin response sample falls back to commit evidence instead of failing.

`preferred` checks never change the verdict. They rank the issues that
were accepted, and they are reported in the summary as reasons to prefer one accepted issue over another. `fits-my-profile` in particular orders accepted issues only; fit cannot rescue an issue the required checks reject, and cannot sink one they accept.
