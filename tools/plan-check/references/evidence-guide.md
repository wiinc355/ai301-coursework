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

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

In an eval bundle, the cause lives in the candidate plan (usually a
Diagnosis or cause statement), and the behavior it must explain lives
in the repro evidence block: the failing step and its artifact, any
control run, and the actual-vs-expected line. The issue section gives
the reporter's description and any suspected cause. In live mode, the
cause is in the student's draft `plan.md`; the behavior is in the
student's posted repro comment on the issue (or, on the house issue,
the house repro pack as the drafts quote it).

Good: the stated cause explains every observation the repro evidence
shows, especially what a control run varied (with vs without a flag,
one version vs another) and what that ruled in or out. A cause that
blames a component the evidence never touched, or waves off a factor
the repro shows matters as a "red herring", does not follow from the
evidence.


## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

In an eval bundle, the plan bounds itself in its in-scope and
not-in-scope statements and in the files or areas it names; look also
for extras tucked into the approach or a "while I'm here" or follow-up
line. Read them against the issue's requested change and any scope
direction in the thread highlights. In live mode, read the same parts
of the draft `plan.md` against the live issue and its thread.

Good: one change that fixes the reported behavior, touching only the
code that change needs. Refactors, migrations, renames, dependency
swaps, format or API redesigns, or cleanup of neighbouring code that
the fix does not require make the change unbounded, even when the plan
labels them optional.


## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

In an eval bundle, look in the candidate plan's approach or steps for
named files, functions, modules, config keys, or clearly identified
areas, and for what will be changed in each. The repo-facts block
says what kind of project it is. In live mode, read the draft
`plan.md`; the repo's source tree on GitHub can confirm that named
files exist.

Good: a stranger could open the named place and start the change
without asking the author where or what. A plan that only restates the
goal ("fix the parser", "improve handling") or leaves the approach to
"investigate first" cannot be started.


## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

In an eval bundle, the test plan lives in the candidate plan (a test
plan, verification, or "how I'll know it works" part). Map it onto the
repro evidence block: its failing command or input, the artifact that
showed the failure, the expected result, and any control run. In live
mode, map the draft `plan.md`'s test plan onto the student's posted
repro comment.

Good: the test plan names an observable result that differs between
the broken and fixed code: the repro's failing command or case, run
again, now producing the expected output, plus a check that the
control or existing behavior still holds. "Run the test suite", "make
sure it works", or a test that would also pass on the unfixed code
proves nothing.


## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

In an eval bundle, look at the candidate plan's risks, unknowns, or
open-questions part, at its statements of certainty ("this is
definitely", "guaranteed", "always"), and at the plan comment's claims,
read against what the repro evidence and thread actually establish. In
live mode, read the draft `plan.md` and comment the same way; if the
build later deviated, the deviation note belongs in `plan.md`.

Good: anything the repro evidence did not show (an untested platform,
version, or code path, a guessed root cause) is named as an unknown or
risk. False confidence is stating an untested cause, outcome, or
guarantee as settled fact.


## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

In an eval bundle, read the candidate plan comment against the thread
highlights (look for maintainer, member, or collaborator direction: a
preferred or ruled-out approach, a request to wait, coordinate, or
open a discussion first) and against the repo-facts block's templates,
contributing asks, and contribution policy. In live mode, read the
draft comment against the live issue thread, `CONTRIBUTING.md`, any
`AI_POLICY.md`, and the repo's issue and pull-request templates.

Good: the comment answers this issue specifically, follows any
explicit maintainer direction in the thread (or says why not and asks),
and meets the repo's stated asks. Generic "I'll fix this" boilerplate
or a plan that contradicts maintainer direction is not thread-aware.

For AI-use disclosure, first decide what the policy covers. "All AI
usage in any form must be disclosed" or "AI-assisted issues and
comments" covers comments. A disclosure rule only for pull requests or
code, a rule that comments be in the contributor's own words, or no AI
policy at all does not require disclosure in a plan comment. Because
every package is treated as AI-assisted, a policy that covers comments
requires an explicit statement in the plan comment naming the tool and
how it was used. A missing statement fails, even if nothing in the
text mentions AI.

