# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

issue # 15 
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15

**Verdict output**

ranks based on Verdict in rubric.md

1. #15 – clears Orchestrator/ContextManager/session-store cache between runs — core agent-orchestration internals, closest to your target skill.
2. #16 – wires failed tool_results entries into the review output — also core orchestrator/tool-call plumbing.
3. #20 – new DependencyAuditTool parsing requirements.txt/package.json/pyproject.toml for outdated majors — 
   deployment-adjacent (dependency/version auditing) new tool.                #19 – new contribution_streak tool on github_tool.py — agent tool work, but asimpler data feature, less orchestration/deployment flavor.#57 – bug fix in tech_detector.py path filtering — smallest, mostutility-bugfix in nature, least tied to orchestration/deployment.
                                                                     
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/iss "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Newest of last 5ault-branch commits is 2026-09-16, within 90 days of today (2026-09-20)"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived:est commit 2026-09-16 is within 90 days"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single fix: emptytextManager cache and clear session state on run start, 3 named files, 3-4himate"},   {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; noments; no linked PRs"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "No  TRIBUTING.md/AGENTS.md/AI policy file found (404s) — silence"},
      {"name": "release-recent", "grade": "fail", "evidence": "Releases APIurned 0 releases"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "openrke225, author_association: COLLABORATOR"},
      {"name": "fits-my-profile", "grade": "pass", "evidence": "Pythont-orchestrator session/state handling — matches agent-orchestration focus"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/16",
    "checks": [                                                                         {"name": "maintainer-alive", "grade": "pass", "evidence": "Newest of last 5ault-branch commits is 2026-09-16, within 90 days of today"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived:est commit within 90 days"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single fix: surfacefailed tool_results entries into review output, 2 named files, 4-6h estimate"},      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; nocomments; no linked PRs"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "No policy ffound — silence"},
      {"name": "release-recent", "grade": "fail", "evidence": "0 releases found"      {"name": "maintainer-filed", "grade": "pass", "evidence": "opened by
Aburke225, COLLABORATOR"},                                                        {"name": "fits-my-profile", "grade": "pass", "evidence": "Python,orchestrator/tool-result plumbing — core agent-framework work"}    ],
    "verdict": "accept"                                                       },
  {                                                                                 "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/20",
    "checks": [                                                                   {"name": "maintainer-alive", "grade": "pass", "evidence": "Newest of last 5
default-branch commits is 2026-09-16, within 90 days of today"},                      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false;
newest commit within 90 days"},                                                   {"name": "scope-bounded", "grade": "pass", "evidence": "Single new tool with
clear spec (parse manifest files, flag major-version-behind deps), named files, 5-8h estimate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; nocomments; no linked PRs"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "No policy ffound — silence"},      {"name": "release-recent", "grade": "fail", "evidence": "0 releases found"},      {"name": "maintainer-filed", "grade": "pass", "evidence": "opened byAburke225, COLLABORATOR"},
      {"name": "fits-my-profile", "grade": "pass", "evidence": "Python, new tool auditing dependency/package files — deployment-adjacent subject matter"}
    ],                                                                          
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/19",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Newest of last 5
default-branch commits is 2026-09-16, within 90 days of today"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false;
newest commit within 90 days"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single new tool with
clear spec (longest consecutive commit-streak calc), one named file, 4-6h
estimate"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no
comments; no linked PRs"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "No policy file
found — silence"},
      {"name": "release-recent", "grade": "fail", "evidence": "0 releases found"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "opened by
Aburke225, COLLABORATOR"},
      {"name": "fits-my-profile", "grade": "pass", "evidence": "Python agent tool,
but a standalone data feature rather than orchestration/deployment work"}
    ],
    "verdict": "accept"
  },                                                                                {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57",    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Newest of last 5 
default-branch commits is 2026-09-16, within 90 days of today"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived: false;
newest commit within 90 days"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single bug fix withrepro steps and named failing tests, one file (tech_detector.py)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no comments; no linked PRs"},
      {"name": "policy-allows-ai", "grade": "pass", "evidence": "No policy file found — silence"},
      {"name": "release-recent", "grade": "fail", "evidence": "0 releases found"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "opened by Aburke225, COLLABORATOR"},
      {"name": "fits-my-profile", "grade": "pass", "evidence": "Python, but a path-filtering bugfix — least tied to orchestration or deployment focus"}
    ],
    "verdict": "accept"
  }
]

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

3 runs: 

first smoke run:  python3.12 run_eval.py --rubric ../skill/rubric.md --limit 3

grading 3 bundle(s) with rubric.md, model sonnet, 5 worker(s)...
  issue-03: reject
  issue-02: reject
  issue-01: accept

item      gold    verdict  agree  note
issue-01  accept  accept   yes    
issue-02  reject  reject   yes    
issue-03  reject  reject   yes    

agreement: 3/3 scored items

then did a full on 20 issues: python3.12 run_eval.py --rubric ../skill/rubric.md

then did a 3 issues `--only` run: 
python3.12 run_eval.py --rubric ../skill/rubric.md --only issue-04,issue-15

One last run save to eval-run.txt: `agreement: 18/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

`issue-04` — my rubric's decision: **reject** (gold: **accept**). my verdict My `scope-bounded` check failed it because the body lists several named rule types and
ends in "etc.," which reads like an open-ended list rather than one deliverable. The
gold note calls it a "small active repo, maintainer-filed bounded bug, unclaimed" 

**Check rationale**

for `scope-bounded`

I wrote the (a)/(b) fail conditions to catch tracking issues and copy-paste-style
work spread over the whole codebase, which is what actually turns a "first issue"
into a multi-week slog. I wrote the "Explicitly NOT fails" carve-outs after noticing
that length and itemization are not the same as unboundedness — a maintainer who
writes a precise plan or names several files for one deliverable is making the issue
*more* tractable, not less, so I didn't want the check to punish detail.

**Trade-offs**

The carve-out I wrote protects "naming several files that serve one deliverable," but
`issue-04` shows it doesn't yet protect "naming several instances of the same missing
case within one file/feature" — a list of rule names ending in "etc." still trips
condition (b) because nothing in the check tells apart "repeated edit across unrelated
modules" from "the same fix applied to several instances of one concept." That's a
case I'm accepting the current wording will keep missing: any bug report that lists
multiple examples of one class of bug (rather than one file per unrelated module) is
at risk of a false `scope-bounded` fail until I add an explicit exception for it.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Issue #15 is Python work inside the agent's own orchestrator (session/context
   caching between reviews), which sits squarely in the agent-orchestration track
   I'm trying to get reps in, not a CRUD or frontend task. The 3-4 hour estimate and
   `tier-1`/`good first issue` labels fit the time I actually have for a first
   contribution this week.
2. The verdict correctly established the objective facts: the repo is active (a
   commit 4 days old), nobody has claimed or opened a PR against the issue, the fix
   is scoped to three named files with one clear goal (clear the cache/session state
   on `run`), and there's no policy blocking AI-assisted work. What the rubric
   couldn't weigh is which *specific* skill I want reps on right now: I picked #15
   over the other accepted issues (a new dependency-audit tool, a new streak-counter
   tool) because debugging a stateful lifecycle bug in an existing orchestrator is
   closer to what breaks in real agent deployments than building a new green-field
   tool, and `fits-my-profile` can only match language/subject, not rank which kind
   of task teaches me the most.
3. Since assignees, comments, and linked PRs are all empty, claiming itself should be
   uncontested, and the Path Review house rule means even a classmate's claim comment
   wouldn't block me. The real difficulty is in the fix, not the claim: there's no
   repro script given, so before touching `context_manager.py` or `session_store.py`
   I'll need to write my own two-review repro to confirm which cached keys survive
   across `Orchestrator.run` calls.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
