# Toolkit Portfolio

**Student:** Derrick Woodall

## Featured Tools

I chose issue-scout, voice guard, ship-gate, and PR monitor because they
represent four critical stages of my open-source software development
process: identifying suitable issues, verifying technical claims, validating
code and repository readiness, and monitoring pull requests. Through
developing, testing, and improving these tools, I gained practical experience
identifying errors, strengthening automated checks, and understanding where
human judgment is still necessary to ensure accuracy, reliability, and
accountability.

**issue-scout**

**What it is, and the judgment hole I wrote:** I developed and used
issue-scout to help identify, evaluate, and select suitable open-source issues
for contribution. My judgment focused on determining whether an issue was
available, actively maintained, appropriately scoped, and worth pursuing. I
used evaluation criteria to distinguish legitimate contribution opportunities
from issues that were already claimed, inactive, or unsuitable. This helped
me make more informed decisions instead of relying solely on automated
search results.

**How it evolved, dated:** On October 6, 2026, the earlier issue-selection
evaluation achieved 20 out of 20 agreement across its scored test cases. On
October 21, issue-scout processed 400 search results and identified 362 unique
issues. During testing, I discovered that the first three filters did not
reduce the results and that sorting by the fewest comments could favor newer
repositories. The tool also detected an Altinn issue that already had an open
pull request. These findings demonstrated the value of the evaluation
criteria while identifying opportunities to improve filtering and issue
ranking. On October 21, 2026, I changed issue-scout’s per-repository result
limit from two issues to one to improve the diversity of search results,
reduce duplicate opportunities from the same repository, and increase my
chances of identifying suitable open-source contributions across a broader
range of projects.

**Evidence:** `eval-run.txt` (the Unit 1 `issue-select` predecessor):

```text
# eval run written by run_eval.py at 2026-10-06T01:03:54Z
...
categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

`scan-output.md` (the later issue-scout scan):

```text
| Combined, duplicates dropped | 362 |
| After filter 1 (assignees) | 362 |
| After filter 2 (updated within 60 days, cutoff 2026-08-23) | 362 |
| After filter 3 (10 comments or fewer) | 362 |
```

`scan-notes.md`:

```text
Changed: per-repo cap from 2 to 1 (Feyo filled two slots with the same failure).
```

**voice guard**

**What it is, and the judgment hole I wrote:** I used voice guard to review
technical claims and ensure that my written explanations were specific,
supported by evidence, and appropriate for the situation. My judgment was
reflected in the guard rule requiring me to explain the evidence I reviewed
before asking one answerable question that would help determine a safe next
step. This provided a consistent standard for reviewing AI-assisted
communication rather than accepting generated statements without
verification.

**How it evolved, dated:** On October 25, 2026, voice guard stopped a
claim-draft question because it violated the established rule. I reviewed
the issue and revised the draft to satisfy the requirement. A subsequent
manual check passed, with both results documented in `guard-log.md`. This
demonstrated how the guard could identify a communication problem and help
validate a corrected version. I linked repro-check, plan-check, and
pr-precheck to the shared voice guide so all three tools would follow the same
communication standards. This improved consistency, reduced conflicting
instructions, and helped ensure that technical findings, recommendations,
and pull-request checks were communicated clearly and supported by evidence.

**Evidence:** `guard-log.md`:

```text
- 2026-10-25 14:41 [manual] stop: rule broken: "I explain the relevant evidence I reviewed, then ask one answerable question that helps me choose a safe next step." | target: ../ai301-unit6-starter/claim-draft.md
- 2026-10-25 14:41 [manual] pass | target: ../ai301-unit6-starter/claim-draft.md
```

**ship-gate**

**What it is, and the judgment hole I wrote:** I used ship-gate to evaluate
whether my open-source contribution was ready to proceed through the
pull-request workflow. My judgment was reflected in the validation manifest
and the checks used to examine repository context, branch readiness, and
local requirements. I wanted a repeatable process that could detect
preventable mistakes before attempting to publish changes. The tool helped
separate successful local validation from approvals that still depended on
maintainers or external systems.

**How it evolved, dated:** On November 1, 2026, ship-gate blocked my workflow
because I was on the repository's main branch rather than the intended
contribution branch. This revealed the importance of validating the active
branch before running the remaining checks. On November 8, I ran ship-gate
against `wiinc355/9275-form-instructions` in the HHS/simpler-grants-gov
repository. The tool recorded a passing verdict, with all five local checks
and `pr-precheck` passing. This demonstrated measurable progress in my
contribution workflow while confirming that external approvals still required
separate verification.

**Evidence:** `gate-log.md`:

```text
2026-11-01 23:28 [manual] HHS/simpler-grants-gov @ unknown (base main)

- verdict: BLOCK (block)
- manifest checks: none ran
- reasons quoted:
  - you are on main, the base branch itself. Nothing here is a pull request: check out your fix branch and run again.
```

```text
2026-11-08 15:54 [manual] HHS/simpler-grants-gov @ wiinc355/9275-form-instructions (base main)

- verdict: PASS
```

**PR monitor**

**What it is, and the judgment hole I wrote:** I used PR monitor to track
pull-request activity and determine when follow-up communication was
appropriate. My judgment was reflected in the monitoring playbook, which
established when to wait, when to follow up, and how to evaluate the available
activity. The goal was to avoid unnecessary reminders while ensuring that
contributions did not remain unattended without appropriate follow-up.

**How it evolved, dated:** On November 8, 2026, PR monitor generated a
recommendation indicating that no follow-up reminder was due. During my
review, I identified a one-day calculation error in an earlier draft and
corrected the follow-up threshold to November 25, 2026. I also added a
verification rule to improve future date calculations. This experience
demonstrated that monitoring automation can support timely decisions, but its
calculations and recommendations must still be independently verified. If
someone other than me comments on or reviews the pull request, PR monitor
resets the 17-day quiet period from the timestamp of that latest activity.
This means the November 25, 2026, follow-up threshold would no longer apply,
and a new follow-up date must be calculated to avoid sending unnecessary
reminders while the review process is active.

**Evidence:** The first monitor draft:

```text
The earliest the threshold could be met is **2026-11-26 at about 22:01 UTC**, assuming no non-author activity before then.
```

`drafted-move.md` after the playbook revision:

```text
MOVE: No nudge yet. The playbook's quiet-time threshold has not been met.
...
The threshold is met at 2026-11-25T22:01:39Z. That is the opening timestamp plus 17 × 24 hours. If anyone other than you comments or reviews before then, recalculate from that timestamp.
```

`playbook.md`:

```text
- Verify any proposed date independently by adding 17 * 24 hours to the
  exact UTC timestamp; recalculate if someone else comments or reviews.
```

## Inventory

- **issue-select:** I originally developed issue-select to evaluate and
  filter open-source issues using defined criteria for availability, project
  activity, contribution scope, and suitability. It served as the foundation
  for issue-scout, which expanded the process into broader issue discovery,
  repository filtering, and identification of already-claimed contributions.
- **repro-check:** I used repro-check to support the reproduction and
  verification of reported software issues. It helped organize the steps and
  evidence needed to determine whether an issue could be reproduced before
  proceeding with implementation. It remains a supporting tool in my
  contribution workflow.
- **plan-check:** I used plan-check to review proposed implementation plans
  before making code changes. Its purpose was to help evaluate whether a
  proposed solution addressed the identified issue and provided a reasonable
  approach for implementation and testing. It remains part of my planning
  and review process.
- **pr-precheck:** I used pr-precheck to validate pull-request readiness
  against defined requirements before publication. On November 8, 2026, it
  passed as part of my ship-gate validation workflow. It remains an active
  supporting check, although successful local validation does not replace
  maintainer approval or GitHub Actions verification.
- **CLAUDE.local.md:** I used this locally excluded repository memory file to
  preserve project-specific working notes, instructions, and development
  context for my toolkit. It helped maintain consistency between work
  sessions without adding my personal working notes to the shared repository.

## The Arc

**Beat 1 — Issue Discovery and Evaluation**

Date: 2026-10-21

Friction: While evaluating issue-scout, I discovered that the initial scan returned 400 results containing 362 unique issues, but the first three filters did not reduce the number of candidates. I also noticed that sorting by the fewest comments could favor newer repositories instead of identifying the most suitable contribution opportunities.

Change: I reviewed the filtering and ranking behavior and used the issue-selection criteria to evaluate candidates more carefully, including checking whether an issue had already been claimed or had an existing pull request.

Result: On October 21, 2026, issue-scout identified that an Altinn issue I had already claimed had an existing open pull request and flagged my claim for release. This helped me recognize the conflict, plan to release the claim, and redirect my efforts toward an available contribution opportunity rather than duplicate work already in progress.

**Beat 2 — Improving Technical Claim Verification**

Date: 2026-10-25

Friction: During manual testing, voice guard stopped my claim-draft question because it did not satisfy the rule requiring an explanation of the reviewed evidence followed by one answerable question that would help determine a safe next step.

Change: I reviewed the guard's feedback and revised the claim-draft question to meet the established communication and evidence requirements.

Result: The revised draft passed the subsequent manual check. Both the failed and successful evaluations were recorded in `guard-log.md`, demonstrating that the correction addressed the identified problem.

**Beat 3 — Repository and Branch Validation**

Date: 2026-11-01

Friction: While preparing my open-source contribution, ship-gate detected that I was on the repository's main branch rather than the intended feature branch. The validation process stopped before running the remaining checks.

Change: I used the reported branch problem to correct my workflow and ensure that future validation would run against the intended contribution branch rather than the base branch.

Result: The November 1 run remained blocked, but the problem was identified before proceeding. A later run on November 8 used the correct contribution branch and passed validation.

**Beat 4 — Successful Validation and Monitoring Improvement**

Date: 2026-11-08

Friction: I needed to confirm that my contribution passed its local checks while also verifying the accuracy of PR monitor's follow-up recommendations. During review, I identified a one-day error in an earlier monitoring draft.

Change: I ran ship-gate against `wiinc355/9275-form-instructions`, reviewed the validation results, corrected PR monitor's follow-up calculation, and added a verification rule to reduce future date errors.

Result: Ship-gate reported a passing verdict, with all five local checks and `pr-precheck` passing. PR monitor established November 25, 2026, as the corrected follow-up threshold and determined that no reminder was due on November 8. These outcomes demonstrated improvements in both validation reliability and monitoring accuracy.

## Limitations and What's Next

Throughout this project, I learned that AI-assisted development tools can
improve efficiency, accuracy, and consistency, but they cannot replace human
judgment or independent verification. Each tool helped address a specific
challenge in my development workflow, but the evaluations also revealed
limitations involving incomplete information, automated decision-making, and
external approval requirements.

My toolkit still cannot independently verify every aspect of an open-source
contribution from issue discovery through final pull-request approval. Some
decisions depend on maintainers, repository permissions, GitHub Actions, and
information that may not be available to the tools. I also learned that
successful automated checks do not necessarily mean a contribution is ready
to merge.

My next priority is to improve PR monitor by implementing additional
date-verification tests and making missing GitHub Actions approval information
visible in its monitoring reports. I will test the updated monitoring process
against the November 25, 2026, follow-up threshold and document the results to
determine whether the improvements prevent incorrect reminders.

**Tool limits.**

issue-scout: The tool cannot always identify maintainer roles when organization membership is private, and its filtering and ranking process may favor newer repositories rather than the most suitable contribution opportunities.

voice guard: The tool can identify claims that violate its communication rules, but passing a guard check does not independently verify the accuracy or completeness of the underlying technical evidence.

ship-gate: The tool can validate local repository and branch conditions, but it cannot independently provide maintainer alignment, user acceptance testing, or GitHub Actions approval.

PR monitor: The tool cannot reliably capture every GitHub Actions approval state or decisions made outside the pull-request discussion, and its follow-up date calculations require independent verification.
