# Contribution: Form Instructions content update

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Derrick Woodall
**GitHub Username:** wiinc355
**Issue:** https://github.com/HHS/simpler-grants-gov/issues/9275
**Pull Request:** https://github.com/HHS/simpler-grants-gov/pull/13214

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: 2026-10-21 — Running the issue-scout scan and grading my picks**

I ran my four `gh search issues` queries (400 results, 362 unique) through the ranking filters and graded the top 5 plus the two issues I had claimed. The scout accepted dapr/python-sdk#1335 and HHS/simpler-grants-gov#9275. It rejected my Altinn pick, altinn-studio#14892, because an open PR from August already closes it, and that is something I missed when I claimed it. The scan also showed that my first three filters did nothing and that "fewest comments first" filled the top 5 with brand-new repos, so I cut the per-repo cap from 2 to 1 and noted a badge-blind spot in the Maintainer engagement check as a rubric patch to test before I change it.

**Unit 6: 2026-10-25 — Installing a voice guard and preserving focused repository memory**

I installed the voice guard as a global Claude Code hook and made the
repro-check, plan-check, and pr-precheck skills use one canonical voice guide.
The guard stopped my first claim-draft question because it was too broad, so I
rewrote it to ask about the specific Apply Tech Leads decision and verified the
revised draft manually. I also created a locally excluded `CLAUDE.local.md` in
my fork that records the contribution rules, Form Instructions render path,
and exact frontend test commands; this prevents repeated broad orientation
without adding personal notes to an upstream PR.

**Unit 7: 2026-11-01 — Installing the ship-gate and recording project checks**

I installed the ship-gate and filled in the HHS/simpler-grants-gov manifest
with dependency setup, the focused test, lint, type check, and format check.
The hook test on `main` showed a ship-gate stop while the voice-guard log
recorded its own stop, and the gate also caught that I had created the feature
branch in the coursework repo instead of the fork. After I corrected the
branch location, all five checks passed, but precheck rejected the first
draft because it lacked a maintainer mention and Apply Tech Leads alignment
was pending. I added @btabaska, disclosed the unresolved alignment and UAT
in a draft for discussion, updated the evidence, and reran the gate
successfully.

**Unit 8: 2026-11-08 — Installing the PR monitor and writing my review playbook**

I installed the monitor and researched five recent external PRs: their median
wait for a first maintainer response was 12.8 days, so my playbook uses 17
quiet days before considering one nudge. The monitor first reported
`NO PR YET`, then drafted a wait note after I opened my draft PR; I did not
post it. Its first note placed the 17-day threshold on November 26 instead
of November 25, so I added a rule to verify UTC date arithmetic independently
and its next draft gave the correct November 25 timestamp. The monitor
reported no checks, but I found three Actions runs marked `action_required`
and confirmed the frontend run was awaiting maintainer approval, so I will
also check the Actions page rather than assume CI has run.

---

## Why I Chose This Issue

I searched with four GitHub CLI queries based on my Python and frontend
interests, then used my issue-scout to rank and grade the results. The
scout accepted HHS/simpler-grants-gov#9275: the repository is active, the
issue had no assignee or linked pull request when I selected it, and the
request is a bounded content/UI change for the Required and Conditionally
Required form tables.
After the Unit 6 scope patch, I rated it **ambitious** because the first
acceptance criterion requires an engineering plan aligned with the Apply Tech
Leads; I asked in my claim whether existing implementation notes or constraints
should guide that step. It fits my goal of improving frontend skills while building
on my Python and VS Code experience, and I want to learn how an established
public-service application handles a small content change through review and
testing.

The scan also showed why I need the scout instead of choosing from a title
alone. It caught an earlier Altinn candidate that already had an open pull
request, and it later rejected a different Altinn claim because its design
decision was still unresolved. My ranking filters initially favored
brand-new repositories, so I reduced the per-repository cap from two
results to one; I also recorded that contributor badges can hide maintainer
status and that any rubric patch for that limitation needs testing first.
My claim and plan comment is
https://github.com/HHS/simpler-grants-gov/issues/9275#issuecomment-6329956447.

**Toolkit**

- Tool: issue-scout, using four `gh search issues` queries, ranking
  filters, and the rubric to grade the scan results and my claimed issues.
- What it produced: 362 unique search results and an accept verdict for
  HHS/simpler-grants-gov#9275, supported by active-project, maintainer,
  policy, available-work, and bounded-scope evidence.
- Difficulties: the first filters did not narrow the query results, fewest
  comments favored brand-new repositories, and maintainer badges can appear
  as CONTRIBUTOR when organization membership is private.
- What changed: I reduced the per-repository cap from two candidates to one.
  I also replaced the strict "Bounded, actionable scope" check with
  `scope-feasible`: true umbrella issues still reject, while an open edge now
  becomes a straightforward, moderate, or ambitious difficulty note instead
  of an automatic rejection.

---

## Understanding the Issue

**Problem Description**

The application did not clearly distinguish a form with no instructions from
one whose instructions were unavailable. This made the Form Instructions
field ambiguous in both the Required and Conditionally Required tables.

**Expected Behavior**

When a form has no instructions, both tables should show "Not Applicable"
according to the acceptance criterion. The issue summary instead says "Not
applicable"; I requested Apply Tech Leads confirmation of the capitalization.
This is a content-state change, not a change to system-error handling.

**Current Behavior**

On the unmodified main commit used for reproduction, a missing instruction
`download_path` fell back to the English "Unavailable" message. In the
baseline component test, the translation mock rendered the
`attachmentUnavailable` key, which the English catalog maps to "Unavailable".

**Affected Components**

The affected area is
`frontend/src/app/[locale]/(base)/workspace/applications/[applicationId]/_components/ApplicationFormsTable.tsx`.
Its `InstructionsLink` fallback renders the Instructions cell for both the
Required and Conditionally Required tables. The focused regression coverage is
in `ApplicationFormsTable.test.tsx`. The issue's first acceptance criterion
requires the engineering plan to align with the Apply Tech Leads; that
approval remains pending.

**Toolkit**

- Tool: issue-scout and the upstream issue, used to read the acceptance
  criteria and the current-versus-proposed UI examples.
- What it produced: a bounded problem statement: forms without instructions
  need the explicit "Not Applicable" state in two application-page tables.
- Difficulties: the issue requires Apply Tech Leads alignment, and its
  summary and acceptance criterion disagree about capitalization.
- What changed: I traced the shared fallback and test, implemented the
  proposed dedicated message, and opened a draft to seek confirmation of
  the approach and capitalization.

---

## Reproduction Process

**Environment Setup**

The baseline reproduction used macOS and fork commit
`48d93f882c63a711bdec4b16e3150d1293cdff43`, Node `v24.11.1`, and npm
`11.6.4`. That Node version was below the frontend's then-declared `>=24.19.0`;
this is source-and-unit-test evidence, not a supported browser run. The first
test attempt failed with `sh: jest: command not found` because dependencies
were absent; `npm ci` from `frontend/` installed them. For the fix, I reran
dependency setup and checks under Node `v26.3.0`.

**Steps to Reproduce**

1. On baseline commit `48d93f882`, run
   `npm run test -- ApplicationFormsTable.test.tsx` from `frontend/`.
2. In the passing `Renders without errors` case, inspect the required-form
   fixture with no `form_instruction.download_path`; the assertion expects
   `attachmentUnavailable`.
3. Inspect `InstructionsLink` in `ApplicationFormsTable.tsx`: an absent
   download path renders `t("attachmentUnavailable")` in the Instructions
   column shared by the Required and Conditionally Required tables.
4. On the baseline commit, observe that the English translation is
   `"Unavailable"`, not the acceptance criterion's requested wording.
5. On branch `wiinc355/9275-form-instructions`, rerun the focused test and
   verify that both tables render `"Not Applicable"` for a form without an
   instruction download path.

**Reproduction Evidence**

- **Commit showing reproduction:** unmodified fork commit
  `48d93f882c63a711bdec4b16e3150d1293cdff43` without code changes.
- **Screenshots/logs:** `npm run test -- ApplicationFormsTable.test.tsx`
  passed 1 suite and 4 tests. The run emitted existing React `act(...)` and
  jsdom navigation warnings, but Jest passed.
- **My findings:** on the baseline, the required-form test rendered the
  absent-path branch and `InstructionsLink` used the same
  `attachmentUnavailable` fallback for both tables. The English message was
  `"Unavailable"`. On the fix branch, the focused suite passes with
  `"Not Applicable"` assertions in both tables. I did not claim a browser
  reproduction.

**Toolkit**

- Tool: repro-check, the focused Jest suite, and source/fixture inspection.
- What it produced: a repeatable focused test command, the baseline
  `"Unavailable"` translation, and the shared fallback render path.
- Difficulties: Claude Code's repro-check session could not read its installed
  rubric files, and baseline Node `v24.11.1` was below the repository's
  `>=24.19.0` requirement.
- What changed: I disclosed the baseline runtime limitation, installed and
  used Node `v26.3.0` for the fix checks, and limited the reproduction claim
  to source-and-unit-test evidence.

---

## Solution Approach

**Analysis**

`ApplicationFormsTable` renders both form tables through the shared
`ApplicationTable` component in
`frontend/src/app/[locale]/(base)/workspace/applications/[applicationId]/_components/ApplicationFormsTable.tsx`.
Each row's `InstructionsLink` checks `form.form.form_instruction.download_path`.
On the baseline, if that path was absent, the caller passed
`t("attachmentUnavailable")` as `unavailableText`; the English translation
was "Unavailable". That shared fallback described a normal form with no
instructions as unavailable in either table. The focused coverage is
`ApplicationFormsTable.test.tsx`, where the baseline missing-path assertion
checked the translation key rather than the requested user-visible wording.

**Proposed Solution**

I implemented the proposed dedicated `notApplicable` translation key for
the missing form-instructions fallback in both tables, replacing the
now-unused `attachmentUnavailable` key whose name described a different
state. The fix leaves download links, competition
instructions, form selection, and error handling unchanged. The issue summary
says "Not applicable" while its acceptance criterion says "Not Applicable";
the implementation follows the acceptance criterion pending Apply Tech
Leads' confirmation. Their approval is not claimed; the draft PR asks them
to confirm or revise the approach and capitalization before it is ready.

**Implementation Plan**

1. Implement the proposed behavior locally; Apply Tech Leads alignment
   remains pending and is not claimed.
2. Add the `notApplicable` English translation key, remove the now-unused
   `attachmentUnavailable` entry, and pass the new key only to the shared
   form-instructions fallback.
3. Update `ApplicationFormsTable.test.tsx` so a Required and a
   Conditionally Required form without a download path both assert the
   visible fallback text. Add a test-local conditional form for the
   missing-path case and retain the existing downloadable conditional form
   as a control.
4. Run `npm --prefix frontend ci` and the focused test, lint, type check,
   and format check under Node v26.3.0; record outputs in `test_evidence.md`.
5. Request Apply Tech Leads confirmation of the approach and capitalization
   in a draft PR, and update the implementation and plan if required.

**Plan deviation:** I implemented the fix before Apply Tech Leads alignment
was received and initially kept the branch local. I later opened a draft PR
to ask for that alignment, explicitly leaving both the decision and UAT
outstanding.

**Toolkit**

- Tool: plan-check, Claude Code, and source/test inspection, used to trace
  the missing-instructions fallback and evaluate the proposed plan.
- What it produced: an `accept` verdict on the plan's seven required checks
  and actionable notes about test coverage, the unused old key, exact repo
  checks, and keeping maintainer alignment explicit.
- Difficulties: the first plan-check run could not read the skill files, and
  the next could not read the issue thread or voice guide. After granting the
  needed read access, it accepted the revised evidence description but noted
  that issue-thread reproduction evidence is not posted and Apply Tech Leads
  approval remains pending.
- What changed: I clarified that the baseline Jest mock asserted a key, not
  the English catalog value; updated the test plan for both tables and the
  downloadable control; removed the unused key; and recorded the
  capitalization difference. Apply Tech Leads confirmation is still pending;
  I initially kept the implementation local, then requested direction in a
  draft PR.

---

## Testing Strategy

**Baseline, before the fix:** `npm --prefix frontend run test --
ApplicationFormsTable.test.tsx` passed 1 suite and 4 tests under Node
v24.11.1. The test's local
`next-intl` mock returns keys, so the missing-path assertion sees
`attachmentUnavailable`, not the visible English text. The English catalog
maps that key to "Unavailable"; this is source-and-test evidence, not a
browser reproduction.

**After the fix:** `npm --prefix frontend run test --
ApplicationFormsTable.test.tsx` passed 1 suite and 5 tests under Node
v26.3.0. The suite checks "Not Applicable" for missing-path forms in both
tables; the existing downloadable form and click-event assertions continue
to pass. This uses the acceptance criterion's capitalization; Apply Tech
Leads confirmation is still pending.

**Checks before the rebase:** `npm --prefix frontend ci` completed under Node v26.3.0.
`npm --prefix frontend run lint` passed with 0 errors and 36 warnings;
`npm --prefix frontend run ts:check` passed; and
`npm --prefix frontend run format-check` passed. The full ship-gate ran all
five repository checks successfully, then pr-precheck rejected the original
draft because the maintainer mention and Apply Tech Leads alignment were
missing.

**Checks after the 2026-11-08 rebase:** under Node v26.3.0, the ship-gate
passed dependency setup, the focused test, lint, type check, and format check
again. The focused suite passed 1 suite and 5 tests and still emitted React
`act(...)` and jsdom navigation warnings. Because the test mocks translations,
I also inspected the English catalog and confirmed `notApplicable` maps to
"Not Applicable"; I did not perform a browser reproduction or UAT.

**GitHub Actions:** the frontend, pa11y, and end-to-end runs showed
`action_required` with no jobs run. GitHub specifically said the frontend
workflow was awaiting maintainer approval, so the local passes are not a
claim that GitHub CI has passed.

**Toolkit**

- Tool: the focused Jest test and frontend CI commands from
  `.github/workflows/ci-frontend.yml`.
- What it produced: the rebased focused suite passed 1 suite and 5 tests,
  and all five local manifest checks passed; before the rebase, lint
  reported 36 warnings with no errors.
- Difficulties: baseline Node v24.11.1 was below the declared v24.19.0
  minimum; the test emitted React `act(...)` and jsdom navigation warnings,
  and GitHub's frontend workflow awaits maintainer approval.
- What changed: I used Node v26.3.0, reran the checks after rebasing, and
  distinguished local passes from GitHub Actions that have not run yet.

---

## Implementation Notes

**2026-10-21**

Documented the issue-scout scan and its 362 unique results, then claimed
the selected issue with a bounded first step and a request for existing
implementation guidance.

**2026-10-25**

Installed the voice guard, revised the claim-draft question after a guard
stop, and traced the shared form-instructions fallback. The baseline focused
test passed with a translation-key mock, so I recorded the English catalog
value separately instead of calling it a browser reproduction.

**2026-11-01**

Installed the ship-gate from the Unit 7 materials and filled in the local
manifest. My first installer attempt used the coursework folder as its
working directory and failed to locate `skill/install.py`; rerunning it from
the Unit 7 folder succeeded. `npm ci` installed the target frontend
dependencies, and the baseline checks ran without source changes. The
`gh pr create --help` hook test produced a ship-gate stop and a voice-guard
log entry. Despite the missing Apply Tech Leads reply, I created
`wiinc355/9275-form-instructions`, replaced the missing-instructions fallback
with a dedicated `notApplicable` message, and added coverage for both tables.
The focused suite passed 5 tests; lint, type and format checks passed. The
ship-gate passed all repository checks but pr-precheck rejected the draft
because the required alignment was pending; the draft also lacked a
designated maintainer mention at the time. I added @btabaska to the local
draft afterward; at that point I had not pushed the branch or opened the PR.

**2026-11-08**

The feature branch was 93 commits behind upstream, so I rebased it onto
`d16806e34` and reran the manifest checks with the updated dependency files.
The gate passed; its voice notes led me to add the exact test command,
separately verify the catalog text, and report the current results rather
than just the pre-rebase results. I used the repository template's
`Work for #9275` wording because UAT is pending, then passed the voice guard
and ship-gate again, pushed the branch, and opened a draft PR asking for
Apply Tech Leads direction. The monitor drafted a wait note, but its first
17-day date was one day late; I verified the arithmetic, added a playbook
rule, and reran it without posting a nudge. GitHub's frontend workflow is
awaiting maintainer approval, and the other two reported Actions runs also
showed `action_required`.

**Toolkit**

- Tool: the Unit 7 ship-gate installer, Claude Code hook test, and focused
  frontend validation commands.
- What it produced: a local manifest and working-file exclusions, installed
  hooks, baseline evidence, a tested and committed feature-branch change,
  and a ship-gate report.
- Difficulties: my first installer invocation used the wrong working
  directory; the hook-test reply showed only the ship-gate stop, while the
  voice-guard log separately recorded its stop. Node v24.11.1 was below the
  baseline project's minimum.
- What changed: I reran the installer from the Unit 7 folder, corrected the
  branch location after the gate caught it, checked both hook logs, used
  Node v26.3.0 for branch validation, and opened a draft that makes the
  pending alignment and UAT explicit.

---

## Pull Request

**PR Link:** https://github.com/HHS/simpler-grants-gov/pull/13214

**Draft Title:** [Issue 9275] Show Not Applicable for forms without instructions

**PR Description:** The draft uses a dedicated "Not Applicable" translation
for missing instructions in both tables and adds focused coverage. It links
the issue as work in progress, reports the before-and-after checks, discloses
AI assistance, and asks @btabaska to help confirm the approach and
capitalization. Apply Tech Leads alignment and UAT are not claimed.

**Maintainer Feedback:**

- 2026-11-08: none yet on the PR.

**Status:** Awaiting review (draft; Apply Tech Leads alignment and UAT pending).

**Toolkit**

- Tool: ship-gate and pr-precheck, used to run repository checks and assess
  the PR draft.
- What it produced: all five local checks and precheck passed on the rebased
  branch; the earlier draft had been rejected.
- Difficulties: the first draft omitted a designated maintainer and did not
  have the issue's required alignment; the current draft still needs that
  direction and UAT.
- What changed: I tagged @btabaska, disclosed the unresolved items, used
  `Work for` as the repository template requests for UAT, and opened a draft
  rather than claiming the issue was complete.

---

## Learnings and Reflections

**Technical Skills Gained**

I traced the shared `InstructionsLink` fallback through both form tables,
followed its `download_path` check to the translation key, and updated the
English catalog and component test together. I also used a test-local
translation mock to assert the mocked text for missing-path forms in both
table variants, and learned to inspect the actual catalog separately because
the mock cannot prove the catalog text.

**Challenges Overcome**

The first ship-gate run found that I had created the feature branch in the
coursework repository rather than the target clone. I returned the coursework
repo to `main`, created the branch in the target clone, committed the fix,
and reran the gate. The gate passed all repository checks and then rejected
the first draft for a missing maintainer mention and Apply Tech Leads
alignment. I initially kept the PR unpublished, then opened a clearly labeled
draft requesting direction after updating the evidence and passing the gate.

**What I'd Do Differently Next Time**

I would create and verify the feature branch in the target clone before
editing, and start with a supported Node version before installing
dependencies. I would also capture the exact CI outputs as they run and
confirm the required maintainer approach before implementing, so the final
plan, diff, and PR draft stay aligned. For monitoring, I would check date
arithmetic and GitHub Actions separately rather than trusting a generated
wait note or an empty check summary.

---

## Resources Used

The issue description and examples defined the requested behavior; the
frontend component, English message catalog, and focused Jest test located
the existing fallback. The repository workflow and contribution guidance
provided the required validation commands and external-contributor review
convention.

- [Issue #9275](https://github.com/HHS/simpler-grants-gov/issues/9275)
  and [my claim](https://github.com/HHS/simpler-grants-gov/issues/9275#issuecomment-6329956447)
- [Contribution guidance](https://github.com/HHS/simpler-grants-gov/blob/main/CONTRIBUTING.md),
  [PR template](https://github.com/HHS/simpler-grants-gov/blob/main/.github/pull_request_template.md),
  and [frontend workflow](https://github.com/HHS/simpler-grants-gov/blob/main/.github/workflows/ci-frontend.yml)
- [Unit 8 starter and monitor](https://github.com/codepath/ai301-unit8-starter),
  my voice guide, ship manifest, playbook, and local build/test notes
- Five merged external PRs used for response-time research:
  [12683](https://github.com/HHS/simpler-grants-gov/pull/12683),
  [12399](https://github.com/HHS/simpler-grants-gov/pull/12399),
  [12179](https://github.com/HHS/simpler-grants-gov/pull/12179),
  [12178](https://github.com/HHS/simpler-grants-gov/pull/12178), and
  [11858](https://github.com/HHS/simpler-grants-gov/pull/11858)
