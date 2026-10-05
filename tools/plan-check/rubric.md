# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Grounded diagnosis | The candidate plan's stated cause read against the repro evidence block's steps, control runs, and actual-vs-expected lines (and the issue's description of the behavior), using the Diagnosis and grounding section of `references/evidence-guide.md`. | The stated cause explains the behavior the repro evidence actually shows, including any control run (what varies and what stays fixed), and contradicts none of it. A cause that blames a component the repro evidence rules out, calls a reproduced factor a red herring, or ignores the evidence's deciding observation fails. | required |
| Bounded scope | The candidate plan's in-scope and not-in-scope statements and the files or areas it names, read against the issue's requested change and any maintainer direction in the thread highlights, using the Scope section of `references/evidence-guide.md`. | The plan makes one change that fixes the issue's behavior and touches only what that fix needs. Drive-by refactors, migrations, API or format redesigns, dependency swaps, or "while I'm here" cleanup that the issue does not need fail, even when labelled as optional or follow-up work inside this plan. | required |
| Executable plan | The candidate plan's named files, functions, or areas and its approach and order of work, read against the repo-facts block and the repro evidence, using the Executability section of `references/evidence-guide.md`. | A stranger could start the work without asking the author anything: the plan names where the change goes (a file, function, module, or clearly identified area) and what the change is. A plan that only restates the goal ("fix the parsing", "investigate and improve") or defers the approach to later discovery fails. | required |
| Decisive test plan | The candidate plan's test plan read against the repro evidence's failing steps and artifacts, using the Test plan section of `references/evidence-guide.md`. | The test plan names an observable outcome that would differ between a broken and a fixed build, tied to the reproduced behavior (the repro's failing command, input, or case, now producing the expected result), and keeps a control or existing behavior from regressing where the repro had one. "Run the tests", "make sure it works", or checks that would pass on the unfixed code fail. | required |
| Honest unknowns | The candidate plan's statements of certainty, risks, and open questions, and the plan comment's claims, read against what the repro evidence and thread actually establish, using the Honesty section of `references/evidence-guide.md`. | Claims are stated at the certainty the evidence supports: anything the repro evidence did not show (an untested platform, version, or code path the plan depends on) is named as an unknown or risk rather than asserted as fact. Asserting an untested cause, outcome, or guarantee as settled fails. | required |
| Thread and conventions | The candidate plan comment read against the thread highlights (maintainer and collaborator direction) and the repo-facts block's templates and contributing asks, using the Comms section of `references/evidence-guide.md`. | The comment and plan follow any explicit direction from a maintainer or collaborator in the thread (a preferred approach, a ruled-out approach, a request to wait or coordinate) and any stated contributing asks, and the comment is specific to this issue. Ignoring or contradicting explicit maintainer direction, or posting generic boilerplate, fails. | required |
| AI-use disclosure | The repo-facts block's contribution policy (CONTRIBUTING.md, AI_POLICY.md, or similar) read against the candidate plan comment, using the Comms section of `references/evidence-guide.md`. | Treat every package as AI-assisted work, because it was drafted and checked with this skill; whether the comment mentions AI is not the test. If the policy requires disclosing AI use in issues or comments, the plan comment must name the tool and the extent of its use, and without that statement this fails. Passes when the policy has no disclosure requirement, requires disclosure only for pull requests or code, or only asks that comments be in the contributor's own words. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept only when every required check passes. Reject when any required
check fails or is unclear: an `unclear` (written as ?) counts as a
fail, because a plan that cannot be verified from the package is not
ready to build from. There are no preferred checks, so every check
gates the verdict.
