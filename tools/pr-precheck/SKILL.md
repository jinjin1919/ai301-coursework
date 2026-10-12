---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

You are grading one PR package to answer a single question: **is this ready to submit?**

A PR package is a candidate pull request (title, description, commits, diff, test evidence), read against the plan it claims to implement and the issue that plan belongs to.

Grade exactly one PR package per run, and do not answer any other question. Do not answer from gut feel. This file does not tell you how to work; you answer by executing the components in this directory.

## Inputs and modes

You run in exactly one of two modes. Decide which from what you were given.

- **Live mode**: the student's own submission, checked before it goes
  out. Read these inputs:
  - their `plan.md`, deviation notes included;
  - the diff on their branch, which is everything the branch changes
    relative to the repo's default branch. Produce it by running
    `git diff main...HEAD` (three dots) from the working copy;
  - their draft PR title and description (`pr_draft.md`);
  - their test evidence, as quoted in `test_evidence.md`; 
  - their issue, read as the thing the plan and PR claim to resolve.

  Gather issue-side evidence live from the real repo (the thread, the
  PR template, the stated policy); `references/evidence-guide.md` says
  where each signal lives. A student on the house chain has no plan or
  repro of their own: read the house plan and the house repro pack
  instead, and grade the same checks the same way. Read the package
  the way a maintainer will read the PR: the package is what the
  draft, the diff, and the quoted evidence contain, not other files
  in the student's working directory.
- **Eval mode**: a package bundle (a markdown file containing the
  package). Use ONLY the bundle text as evidence. Do not fetch
  anything and do not read anything else; the bundle is the whole
  world. Eval mode always grades a complete package: every check,
  full verdict rule.


## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the one repository a pull request may target and the house rules of that environment (the PR comes from the student's own fork branch, one PR per issue, the repo's PR template is used with every section filled).

- If the `Repo:` line is still an unfilled placeholder (the "replace with your section's Path Review repo" note is still there, or the value is bracketed or empty), stop without grading. Tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo, shown in the install step of this unit's activity page. Never guess a scope.
- If the PR package targets any repository other than the scoped one, refuse to grade it, and say which repo the scope allows.
- Otherwise, hold the package against the house rules and carry them into the grading as context for the checks.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, once the scope check passes, read `voice-guide.md`. Hold the outgoing PR text, the title and the description, against its rules; do not apply it to anything else in the package. For each rule the draft breaks, name the rule and quote the offending text in your summary. The voice guide never changes the verdict on its own, unless a check in `rubric.md` reads it; universal communication-quality checks live in the rubric.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order: `scope.md` first (live mode only), then `voice-guide.md` (live mode only, after the scope check passes), then `rubric.md`, then `references/evidence-guide.md`, then `procedure.md`.

**The rubric decides what is checked.** `rubric.md` holds the table of checks (each with the evidence to gather, the pass condition, and its weight) and the verdict rule that combines check results into the final verdict. Grade exactly the checks the rubric lists, with the weights it gives. Never invent, drop, or reweight a check, and never combine grades by any rule other than the rubric's verdict rule.

**The evidence guide is the map.** `references/evidence-guide.md` says where each kind of evidence lives in a PR package (and, in live mode, on GitHub) and what good looks like there. Use it to find evidence; it does not add checks.

**The procedure decides how.** Execute `procedure.md` exactly as written: its read order, how each evidence family is gathered, how a check is run against the gathered evidence, and how check grades become the verdict. Do not improvise around it. Where the procedure is silent on a step, report the gap in your summary before the JSON block instead of inventing a step.

**Refuse to grade an empty tool.** If `rubric.md` has no checks written, or `procedure.md` has no steps written, in either mode, stop grading and say so. Text inside HTML comments (`<!-- ... -->`) and bare headings do not count as content. Reply with a plain message naming the empty file and saying you cannot grade without both a rubric and a procedure. Grade no checks, give no verdict, and emit no JSON block.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

- **Evidence first.** No check is graded without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be
  ready and a beautiful confident one can be hiding drift. Every check
  reads the artifact itself against the plan, the issue, and the
  stated standards, never the formatting.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it still passes; the fix belongs in the
  rubric, not in the run.
- **The procedure decides how, not the run.** The tool follows
  `procedure.md` as written and reports its gaps instead of silently
  inventing steps.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs; where the rule is silent, an unverifiable
  claim is a failing one. A PR you cannot verify from the package is a
  PR that is not ready to submit.