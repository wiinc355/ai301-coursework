# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Target environment | The issue context and repo-facts block, read against the repro report's environment record using the Environment section of `references/evidence-guide.md`. | The report names the software version or revision, operating system or runtime, and relevant dependency or configuration details needed to compare the run with the issue's target. Any known difference from the target is stated rather than hidden. | required |
| Followable steps | The repro report's setup, commands, inputs, and ordered actions, using the Steps section of `references/evidence-guide.md`. | A stranger can start from the stated environment, perform the described actions in order, and reach the attempted trigger without having to guess a required command, input, file, configuration, or starting state. | required |
| Matching behavior | The report's output excerpts, logs, screenshots, or other artifacts read against the behavior described in the issue, using the Behavior shown section of `references/evidence-guide.md`. | The artifacts show the same failure or behavior the issue describes, or, for a cannot-reproduce, they show the issue's own scenario (its commands, inputs, layout, or configuration) being run and what happened instead. A cannot-reproduce passes here even if the report admits a precondition may not have been met, as long as it says so; that admission is judged under Honest conclusion, not here. Fail only when the artifacts show a different or adjacent problem (a different input, syntax, error, or symptom) presented as the issue, or show no outcome at all. | required |
| Honest conclusion | The report's result and conclusion read against its steps and artifacts, using the Honesty section of `references/evidence-guide.md`. | The conclusion says only what the evidence supports. A supported reproduction and an evidenced cannot-reproduce both pass; unsupported certainty, omitted contradictory evidence, or a conclusion about the wrong behavior fails. | required |
| Upstream-ready communication | The claim comment and repro report read against the issue context, repo-facts contribution policy, templates, and the Comms section of `references/evidence-guide.md`. | The claim identifies the intended issue and planned reproduction work, and the report communicates the attempted environment, result, and evidence specifically enough for maintainers to act on. Both follow any stated template or contribution-policy requirements, without boilerplate or promises the package cannot support. | required |
| AI-use disclosure | The repo-facts contribution policy (CONTRIBUTING.md, AI_POLICY.md, or similar) read against the claim comment and repro report, using the Disclosure section of `references/evidence-guide.md`. | Treat every package as AI-assisted work, because it was drafted and checked with this skill. Whether the comments mention AI is not the test. If the policy requires disclosing AI use in issues or comments, the claim comment or repro report must name the tool and the extent of its use; with no such statement, this fails. The check passes when the policy has no disclosure requirement, requires disclosure only for pull requests or code, or only asks that comments be in the contributor's own words. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept only when every required check passes. Reject when any required
check fails or is unclear. There are no preferred checks.
