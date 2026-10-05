# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

Where it lives: eval bundle's `## Repro evidence` block (every
numbered step, its expected/actual, and any control run — a step that
reproduces the same inputs minus the suspected trigger) read against
the candidate plan's `### Diagnosis` section. Live: the student's own
posted repro comment (or the house repro pack, on the house issue)
read against the draft `plan.md`'s diagnosis.

What good looks like: the stated cause explains *every* step's result,
not just the failing one. A control run is the sharpest tool here —
if a step changes one variable and the behavior persists or vanishes,
the diagnosis must be consistent with that step too. A diagnosis that
explains the failure but is silent on (or contradicted by) a control
run fails even if it reads as confident and well-written.

## Scope

Where it lives: the candidate plan's `### Scope` section — specifically
the not-in-scope line, which is the boundary a reviewer holds the diff
to — read against its own `### Files` and `### Approach` sections.

What good looks like: the not-in-scope line names a real adjacent
temptation (a related redesign, a migration, a broader abstraction),
and the Files/Approach sections touch only what the in-scope line
promised. Count subsystems, not words: one bounded change touches the
files the issue's single reported behavior requires; a drive-by
rewrite touches additional files or modules the issue never mentioned,
however well-reasoned the explanation for doing so.

## Executability

Where it lives: the candidate plan's `### Files` and `### Approach`
sections.

What good looks like: concrete file paths or named locations (not "the
tokenizer somewhere" or "the relevant module"), plus an ordered list of
steps where every real decision is already made. A stranger can open
the named file and start on step 1 without asking the author anything.
Words like "whichever is easier," "not sure which layer," or "maybe
also check X" mark a deferred decision, not a plan.

## Test plan

Where it lives: the candidate plan's `### Test plan` section read
against the repro-evidence block's own steps and artifacts.

What a decisive test plan names: a specific input and a specific
observable outcome — an exit code, a printed value, a re-run of one of
the repro evidence's numbered steps with the expected result flipped.
"Run the full test suite" or "should feel fixed" names no outcome tied
to *this* fix and fails even when the rest of the plan is strong — the
outcome must be something a reader could check without taking the
author's word for it.

## Honesty

Where it lives: the candidate plan's `### Risks` section, read against
what `### Scope` and `### Approach` actually commit to. Live: an honest
mid-build deviation is recorded in the student's own `plan.md` (see
SKILL.md's "After an honest deviation"), not only in the diff or the
follow-up comment.

How to tell stated unknowns from false confidence: an honest plan names
what it has not verified ("untested on Windows," "assumes the cache key
format is stable") and, when it defers work, says why. False confidence
looks like the Scope or Approach section quietly asserting something
the repro evidence never established, with no corresponding line in
Risks.

## Comms

Where it lives: the candidate plan comment read against the eval
bundle's `## Thread highlights` section and the `## Repo facts` block
(contribution policy, any stated AI-use disclosure requirement). Live:
the same comment read against the live issue thread and the repo's
CONTRIBUTING docs, per `scope.md`'s house rules.

What thread-aware looks like next to boilerplate: the comment does not
contradict or ignore direction a maintainer already gave in the thread
(e.g. a named root cause, a requested approach, an open PR to engage
with instead of race). If the repo-facts block states an AI-use
disclosure policy, the comment includes it — its absence is a fail on
its own, independent of how good the plan underneath is.
