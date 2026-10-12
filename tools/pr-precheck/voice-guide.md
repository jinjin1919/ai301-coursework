# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

Learning in contributing to open source repo, new to contributing upstream, taking a course-assigned issue as a first PR. 

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Claim only what the artifact shows

I don't say "confirmed" or "reproduced" unless my own output actually
demonstrates the bug. If I ran it and it didn't trigger, I speak the truth. 

- Wrong: "Confirmed, this is definitely the bug — happens every time."
- Right: "Reproduced on 3.2.4: the header is dropped in my run too (log below)."


### Rule: No assign-me boilerplate

Every claim comment names something specific I actually checked or
plan to check next — never a generic "I'll take this."

- Wrong: "I'd like to work on this issue, will submit a PR soon!"
- Right: "Reproduced the missing Content-Type with one custom header
  (repro below); next I'll check apply_missing_repeated_headers()
  against the multidict versions mentioned in the thread."


### Rule: Name the version and the behavior

Every claim points to a specific version and a specific observed
behavior — never a vague reference to "this bug."

- Wrong: "I can reproduce this bug on my machine."
- Right: "Confirmed: --style is ignored on v1.20.0, exactly as
  described."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
- "I will fix this by tomorrow, promise!" — or any date/timeframe I
  haven't actually scoped.
- "I'd like to work on this issue!" — with no repro, no version, no
  next step named.
- "This is such an amazing project!!" — enthusiasm standing in for
  content.
- "I can reproduce this bug." — without naming the version and the
  exact observed behavior.
- "Should be a quick fix." — a guess about effort dressed as a fact.