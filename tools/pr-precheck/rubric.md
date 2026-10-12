# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff stays inside plan | The diff's changed files and hunks, read against the plan's stated scope/boundary and its deviation notes | Every changed file is named in the plan's scope, or covered by a deviation note that says what changed and why. Fail if any changed file has neither. | required |
| Plan delivered or shortfall disclosed | The plan's stated deliverables, read against the diff and the description | Every deliverable the plan promised is visible in the diff, or the description/deviation notes say plainly what was left out and why. A disclosed shortfall passes; a silently missing deliverable fails. | required |
| Description matches diff | The description's claims about what changed ("fixes X", "also updates Y", "no behavior change"), read against the diff | Every claim about what the change does, which files or behavior it touches, and what was added or tested is true in substance of the diff, and the description does not hide a behavior change or scope expansion. Loose wording about counts or granularity ("one function" for a changed function plus a small helper, "tests" for one test with several asserts) is not a fail when the substance holds. Fail if the diff does the opposite of a claim, a claimed deliverable (test, doc, file) is absent, or the diff changes behavior or scope that the description denies or never mentions. | required |
| Proof is present | The test-evidence section, read against the plan's test plan and the issue's reproduction steps | The package contains actual observable evidence (command output, a before/after result, a test name with its outcome, a screenshot or log). Fail if the only support is a statement such as "tested locally" or "works now". | required |
| Proof is decisive | The same test evidence, read against the behavior the issue reports | The evidence shows the reported behavior before and the fixed behavior after (or a test that fails without the change and passes with it), and ties to the plan's test plan. Fail if the evidence is vague, does not exercise the changed code, or would also pass without the fix. Grade `unclear` when it cannot be told either way. | required |
| Repo checks run | The test-evidence section and the repo-facts block's stated checks (test suite, linter, CI requirement) | The package shows the repo's own stated checks were run with a visible outcome, or states why one could not be run. Fail if the repo names a check and nothing shows it was run. | required |
| Diff is reviewable | The unified diff and commit list | Every hunk serves the change. Fail if the diff carries debug leftovers (prints, TODO scraps), commented-out or dead code, formatting churn on untouched lines, or unrelated drive-by edits that a reviewer would have to separate from the fix. | required |
| Template and disclosure honored | The repo-facts block's PR template sections and stated policy (including AI-use disclosure), read against the description | Every template section the repo requires holds real content for this change, and a required disclosure is present. Fail if a section is blank, boilerplate left as is, or a stated ask is ignored. | required |
| Links the issue and thread direction | The description, read against the issue and any explicit maintainer direction in the thread | The description references the issue it implements, and any explicit maintainer instruction in the thread (approach, naming, scope) is followed or answered. Fail if a maintainer's stated direction is visibly ignored. | required |
| Commits are understandable | The commit list | A reviewer can tell from the commit messages what changed and why; no "wip", "fix", or empty messages standing alone. | preferred |

## Verdict rule

Accept if every `required` check passes. Reject if any `required` check fails. `unclear` counts as fail: a claim that cannot be verified from the package is not ready to submit. `preferred` checks never change the verdict; report them in the check evidence lines only.

A PR that honestly discloses a shortfall is not penalized for it: "less than everything" passes the plan-delivered check when the package says so plainly and the rest of the checks hold.
