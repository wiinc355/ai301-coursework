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

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->

In an eval bundle, read the issue context and repo-facts block for the
versions, platform, dependencies, configuration, and other conditions the
issue targets. Compare those with the environment record in the candidate
repro report. In live mode, read the GitHub issue and any linked repository
setup or contribution documentation, then compare them with the environment
written in the student's draft report.

Good evidence identifies the software version or revision, operating system
or runtime, and any dependency or configuration that can affect the result.
It either matches the issue's target or states the difference clearly enough
for a maintainer to judge whether the run is comparable.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

In an eval bundle, the steps live in the candidate repro report; use the
issue context to identify the promised trigger and the repo-facts block for
required setup conventions. In live mode, compare the draft report's setup,
commands, inputs, files, and actions with the issue thread and the
repository's documented setup.

Good steps give a stranger a definite starting state and enough ordered
actions to reach the attempted trigger. They include every material command,
input, configuration, or file that the result depends on, without requiring
the reader to invent a missing transition.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->

In an eval bundle, find artifacts in the candidate repro report: terminal
output, error text, logs, screenshots, test output, or other captured
observations. Read them directly against the expected and actual behavior in
the issue context. In live mode, use the same artifacts in the draft and
compare them with the live issue description and relevant maintainer
clarifications.

Good evidence visibly reaches the issue's trigger and shows either the same
reported behavior or a successful run where that behavior should have
appeared. An unrelated error, an earlier setup failure, or a screenshot that
does not expose the claimed state is evidence of an adjacent problem, not of
the issue.

For a cannot-reproduce, ask whether the attempt targeted the issue's own
scenario (the same commands, inputs, layout, or configuration the issue
names) and recorded what happened. If it did, the behavior shown matches,
even if the report says a precondition (an environment detail, a limit
being hit) may not have been reached; naming that gap is a sign of honesty,
not a wrong target. Wrong-target means the artifact comes from a different
input, syntax, or symptom than the issue's, presented as the issue.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

In an eval bundle, compare the candidate report's result and conclusion with
its environment, steps, and artifacts. Also compare the claim comment with
what the report actually attempted. In live mode, make the same comparison
within the student's drafts and check the issue thread for facts that limit
what can be claimed.

Good conclusions use the evidence's actual level of certainty. A report may
say reproduced, not reproduced, or inconclusive when its artifacts support
that result; a careful cannot-reproduce is useful evidence. It must not turn
a setup failure into a product conclusion, omit conflicting output, or claim
to have tested conditions it did not test.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->

In an eval bundle, read the candidate claim comment and repro report against
the issue context, the repo-facts contribution policy, and any quoted issue
or comment template requirements. In live mode, inspect the GitHub issue
thread, `CONTRIBUTING.md`, linked contributor documentation, dedicated AI
policy files, and applicable issue or pull-request templates, then compare
them with the student's draft comments.

A good claim names the work the student intends to do without pretending it
is already complete or promising an unsupported deadline. A good repro
comment states the tested environment, result, and useful evidence in terms
specific to that issue. Both satisfy required templates; generic
boilerplate or claims broader than the evidence do not pass.

## Disclosure

In an eval bundle, the AI policy lives in the repo-facts block's
contribution policy line (CONTRIBUTING.md, AI_POLICY.md, or similar). In
live mode, read the repo's `CONTRIBUTING.md`, any `AI_POLICY.md`, and the
issue and pull-request templates.

First decide what the policy covers. "All AI usage in any form must be
disclosed" or "AI-assisted issues and comments" covers comments. A
disclosure rule only for pull requests or code contributions, a rule that
comments be in the contributor's own words, or no AI policy at all does not
require disclosure in a claim or repro comment. Then, because every package
is treated as AI-assisted, a policy that covers comments requires an
explicit statement in the claim comment or repro report naming the tool and
how it was used. A missing statement fails, even if nothing in the text
mentions AI.
