# Rubric: is this pull request ready to submit?

<!--
Checks map onto the four failure families (harness categories):
Diff matches plan + Description matches diff = silent-drift;
Decisive test evidence + Planned checks reported = not-tested;
Clean diff = unreviewable; Template and stated asks + AI-use
disclosure = standards-wall. Built from my Unit 3 plan-check rubric,
rewritten for a PR.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff matches plan | Every file and hunk in the diff, read against the plan's scope (in-scope change, files named, not-in-scope list) and its deviation notes, using the Plan fidelity section of `references/evidence-guide.md`. | Every hunk in the diff does work the plan or a deviation note names, and every change the plan puts in scope appears in the diff or is deferred by a deviation note (in the plan) or a plain statement in the description. Fails when the diff adds unplanned behavior (a new option, flag, setting, renaming pass, or rewrite of a file or function the plan never mentions) with no deviation note, or touches something the plan explicitly scopes out, or silently drops a planned change. Tests added for the planned change, and changelog or template-required entries, count as planned work. | required |
| Description matches diff | Each claim the PR description makes about what changed ("implements the plan exactly", "no other changes", "the docs now say...", the listed changes) read against the diff's actual hunks, using the Plan fidelity section of `references/evidence-guide.md`. | Every change the description claims is present in the diff, and the description does not claim fidelity ("exactly as planned", "no changes beyond the plan", "no functional changes outside X") that the diff contradicts. A description that is terse or omits minor details passes; one that claims something the diff does not contain, or denies something the diff does contain, fails. | required |
| Decisive test evidence | The test evidence (and any before/after in the description) read against the plan's test plan and the repro evidence's failing command or input, using the Test evidence section of `references/evidence-guide.md`. | The evidence re-runs the plan's own repro case on the branch (the same failing command, input, or case the plan's test plan names, identified concretely) and states its observed after-result in terms that would differ from the broken build; where the plan's test plan names more than one failure mode or repro, each is re-run or explicitly deferred. Fails when the evidence is only a general claim ("works on my machine", "colors work now", "verified for a day", "tests pass") without the plan's repro named, or exercises only a control or a different path than the one the fix changes. | required |
| Planned checks reported | The test evidence read against the suite, lint, or check commands the plan's test plan names, using the Test evidence section of `references/evidence-guide.md`. | Every suite or check command the plan's test plan names is reported with its outcome (pass, or fail with an honest reason). Passes when the plan names no such command. A failing check reported honestly with its reason passes; a named check never mentioned fails. | required |
| Clean diff | The unified diff's hunks (and, secondarily, the commit list), using the Diff quality section of `references/evidence-guide.md`. | Every hunk is part of the change: no debug prints or logging left in, no commented-out code or commented-out earlier attempts, no dead functions or unused helpers, no whitespace-only or re-indent or import-reshuffle churn, and no edits to lines the change does not need. A short comment explaining the new code is fine. Any debris hunk fails, even when the fix itself is right. | required |
| Template and stated asks | The repo's stated PR asks (the repo-facts block's PR template sections and required checklist items, the contributing guide's asks such as an issue-closing line or a changelog or whatsnew entry) and any explicit maintainer direction in the thread, read against the PR description and the diff, using the Standards and comms section of `references/evidence-guide.md`. | Every section or item the repo states as required is present with content specific to this PR (a required "closes #N" line has the number, a required checklist is filled, a required changelog or whatsnew file entry appears in the diff), and the PR does not contradict explicit maintainer direction in the thread. Passes when the repo states no template or asks beyond what the PR meets. Fails when a required section, checklist, issue link, or file entry is visibly missing or left as template boilerplate. | required |
| AI-use disclosure | The repo's contribution or AI policy (repo-facts block, CONTRIBUTING.md, AI_POLICY.md) read against the PR description, using the Standards and comms section of `references/evidence-guide.md`. | Treat every package as AI-assisted work, because it was built and checked with this tool; whether the PR mentions AI is not the test. If the policy requires disclosing AI use in pull requests, code, or "all AI usage", the description must contain a statement naming the tool and the extent of its use, and without it this fails. Passes when the policy states no disclosure requirement, or only asks that comments be written in the contributor's own words. | required |

## Verdict rule

Accept only when every required check passes. Reject when any required
check fails or is unclear: `unclear` counts as fail, because a PR that
cannot be verified from the package is not ready to submit. There are
no preferred checks, so every check gates the verdict. An honestly
disclosed shortfall (a deviation note in the plan and the same gap
stated in the description) is not a fail under any check.
