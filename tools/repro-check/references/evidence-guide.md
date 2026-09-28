# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives: eval bundle — the "Environment:" sentence at the top of
the **Candidate repro report**. Cross-check it against the **Repo
facts** block's "latest release" line and against any version the
**Issue** section or **Thread highlights** name as the trigger (a
thread comment pinning the bug to a specific dependency version is
part of the target, not noise). Live mode: the same sentence in the
draft repro comment; cross-check against the repo's releases/CHANGELOG
and the issue's own stated version.

What good looks like: tool version (or exact commit/build), OS, and any
dependency version the issue or thread calls out are all named — enough
that a reader could put themselves in the same state. If the named
version differs from the issue's target or the latest release, the
report says so and gives a reason the delta doesn't invalidate the run
(a silent old-version run is a fail, not a pass with an asterisk).

## Steps

Where it lives: the fenced command block(s) in the **Candidate repro
report**, read against the **Issue** section's own numbered
reproduction steps (the trigger condition the issue claims causes the
bug). Live mode: the draft's steps section; also check whether every
file, config, or fixture a step references is something a stranger
could actually obtain (public repo state, a pasted config) rather than
left implicit.

What good looks like: steps start from a state any reader can reach
(fresh clone/install, a bare CLI invocation, or an explicitly pasted
config) and name the literal command(s) run, using the same trigger
condition the issue describes — not a substituted argument, operator,
or flag that changes what's being tested. A step that only works inside
the author's private setup, or that quietly drops a platform-specific
flag the issue's OS needs, fails this even if the rest of the report
reads well.

## Behavior shown

Where it lives: the "Actual:" line plus any attached output/log/error
block in the **Candidate repro report**, read against the **Issue**
section's described current-result (and its labels — "crash" vs
"incorrect output" vs "silent no-op" are different targets) plus
anything **Thread highlights** narrow the real cause down to.

What good looks like: the artifact shows the same failure mode as the
issue — same error type, same code path, same missing/incorrect
output — not a different exit code, a graceful validation error
standing in for a reported crash, or a passing run narrated as if it
failed. A report that could not reproduce the issue's behavior is fine
here if it says so plainly and names what differed (environment, input
shape, timing) instead of quietly substituting a nearby symptom and
calling it the same bug.

## Honesty

Where it lives: the claim comment's and repro report's own verdict
language ("reproduced," "confirmed," "could not reproduce," "I
verified...") in the **Candidate claim comment** and **Candidate repro
report**, read against that same report's "Actual:" line and artifact.

What good looks like: the words claim exactly what the artifact shows —
"reproduced" only when the artifact demonstrates the issue's behavior,
"could not reproduce" when it doesn't, either way with the specific
outcome named. A claim of confidence, certainty, or a diagnosed root
cause with no artifact behind it fails here even when the prose reads
fluently and specifically; fluency is not evidence.

## Comms

Where it lives: the **Candidate claim comment**'s own text, read
against the **Repo facts** block's stated bug-report template,
contribution policy, and any AI-use/disclosure line.

What good looks like, as two separate, checkable things:

- Specific intent: the claim comment names something concrete to this
  issue — what was checked, what's planned next, a scope — rather than
  an interchangeable "I'll take this" template. It does not promise
  what a first-time contributor can't actually guarantee (a fixed
  delivery date, certainty about the root cause before doing the work).
<!-- - Disclosure: if the Repo facts block states an AI-use or AI-disclosure
  policy, the claim or repro comment discloses AI assistance (in those
  words or plainer ones). No stated policy, or a stated policy that is
  permissive with no disclosure requirement, means this half passes by
  default. A repo that requires disclosure and gets none fails here
  regardless of how good the repro itself is — this check is meant to
  hold even a flawless package. -->
