# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives.**

- *Eval bundle:* the plan's stated boundary is in the **Plan context** block: the `Scope` sentence, the `Not in scope` line, the `Files` list, and any deviation note. What actually changed is in the **Candidate PR** block's **Diff** (the `---`/`+++` file headers and the `@@` hunks). What the PR claims is in its **Title**, **Description** and **Commits**.
- *Live:* the plan is your `plan.md` (scope, not-in-scope, files, and its deviation notes). What changed is `git diff main...HEAD` (add `--name-only` for the file list), run from the working copy. What the PR claims is your draft title and description (`pr_draft.md`) and `git log main..HEAD`.

**What good looks like.**

- Every changed file in the diff is named in the plan's `Files`/scope, or covered by a deviation note that says what changed and why. (A plan that names one source file and one test file, and a diff that touches exactly those two, matches.)
- An honest deviation re-ties a mismatch: the note names the extra or missing change and the reason. A file the diff touches that neither the plan nor a note mentions is silent drift, and so is a change in an area the plan's `Not in scope` line ruled out.
- Silent drift runs in both directions: **more** than the plan (an extra file, an extra behavior change, a refactor the plan excluded) or **less** with no note (a promised deliverable, such as the regression test or the second file, is missing from the diff and nothing admits it).
- The description's fidelity claims are read against the diff and belong here, not under comms: a description that says it "adds a regression test" when the diff has none, or says "no other behavior changes" when the diff does more, is silent drift. Loose wording about counts or granularity ("one function", "tests") is not drift when the substance is in the diff; a claimed deliverable that is missing, or a claim the diff contradicts, is.

## Test evidence (harness category: not-tested)

**Where it lives.**

- *Eval bundle:* the **Candidate PR** block's **Test evidence** section. Read it against the **Plan context** block's `Test plan` and `Repro evidence`, and against the **Repo facts** block's required checks (for example "tests added and passed", pre-commit, a named suite).
- *Live:* your captured test output (the pasted terminal output, test names and results, logs or screenshots in the draft), read against `plan.md`'s test plan, the issue's repro steps, and the repo's checks from its PR template and CONTRIBUTING.md.

**What good looks like.**

- Decisive evidence names an observable behavior and shows both sides: the before result on the unfixed code and the after result on the branch, for the behavior the issue reports (for example `len == 99999` on main and `100000` on the branch from the issue's own script), or a test that fails without the change and passes with it.
- The expected-after matches the plan's test plan, and the evidence exercises the code the diff changed. The outcome of each of the repo's stated checks is visible (suite name and counts, pre-commit result), not just the word "passed".
- Weak evidence is a bare statement ("tested locally", "tests pass", "works now") with no output, or output that would pass with or without the fix, or a suite run that never touches the changed code. That is "statement only" and fails the presence check; vague-but-present evidence is the decisive check's job.

## Diff quality (harness category: unreviewable)

**Where it lives.**

- *Eval bundle:* the **Candidate PR** block's **Diff** (the unified diff) and **Commits** list.
- *Live:* `git diff main...HEAD` for the change itself and `git log main..HEAD --oneline` for the commits.

**What good looks like.**

- A reviewable change shows the fix plainly: every hunk serves the plan, and nothing unrelated rides along. A reviewer can state in one sentence what each hunk is for.
- Debris tells to look for in the diff: debug leftovers (`print`, `console.log`, stray logging), TODO or FIXME scraps, dead or commented-out code, formatting-only churn on lines the fix did not need (whitespace, reordering, import shuffles), and drive-by edits to unrelated code or files. Also look for duplicated or redundant logic added beside existing logic.
- Commits: each message says what changed and why. Messages like `wip`, `fix` or `update` standing alone are the tell (this feeds the preferred commits check only).

## Standards and comms (harness category: standards-wall)

**Where it lives.**

- *Eval bundle:* the repo's asks are in the **Repo facts** block (the `pull requests` line: the PR template's required sections and checklist items; the `contribution policy` line, including any AI-use disclosure rule). Explicit maintainer direction is in the **Issue** block and **Thread highlights**. The PR's response to them is in the **Candidate PR** block's **Title** and **Description**.
- *Live:* the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md` or equivalent) and CONTRIBUTING.md, plus the issue thread, read against your draft title and description.

**What good looks like.**

- Every template section the repo requires is filled with real content for this change, and each checklist item the repo requires is addressed (for example: the issue is linked with its number, tests added, the changelog or whatsnew entry present when the repo asks for one for bug fixes). If the repo states an AI-use disclosure ask, the disclosure is present.
- Explicit maintainer direction in the thread (an approach to take, a naming or scope instruction) is followed or answered in the PR.
- Non-compliant looks like boilerplate left as shipped, a section left blank, a required checklist item silently missing, or a stated ask visibly ignored. A short description that satisfies every stated ask is compliant; a long one that skips an ask is not.
- Whether the description's claims match the diff is plan fidelity above, not a standards problem.
