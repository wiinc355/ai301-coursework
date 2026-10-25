# Contribution: Form Instructions content update

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Derrick Woodall
**GitHub Username:** wiinc355
**Issue:** https://github.com/HHS/simpler-grants-gov/issues/9275
**Pull Request:** Not opened yet

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

**Unit 5: Running the issue-scout scan and grading my picks**

I ran my four `gh search issues` queries (400 results, 362 unique) through the ranking filters and graded the top 5 plus the two issues I had claimed. The scout accepted dapr/python-sdk#1335 and HHS/simpler-grants-gov#9275. It rejected my Altinn pick, altinn-studio#14892, because an open PR from August already closes it, and that is something I missed when I claimed it. The scan also showed that my first three filters did nothing and that "fewest comments first" filled the top 5 with brand-new repos, so I cut the per-repo cap from 2 to 1 and noted a badge-blind spot in the Maintainer engagement check as a rubric patch to test before I change it.

**Unit 6: Installing a voice guard and preserving focused repository memory**

I installed the voice guard as a global Claude Code hook and made the
repro-check, plan-check, and pr-precheck skills use one canonical voice guide.
The guard stopped my first claim-draft question because it was too broad, so I
rewrote it to ask about the specific Apply Tech Leads decision and verified the
revised draft manually. I also created a locally excluded `CLAUDE.local.md` in
my fork that records the contribution rules, Form Instructions render path,
and exact frontend test commands; this prevents repeated broad orientation
without adding personal notes to an upstream PR.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with four GitHub CLI queries based on my Python and frontend
interests, then used my issue-scout to rank and grade the results. The
scout accepted HHS/simpler-grants-gov#9275: the repository is active, the
issue has no assignee or linked pull request, and the request is a bounded
content/UI change for the Required and Conditionally Required form tables.
After the Unit 6 scope patch, I rated it **ambitious** because the first
acceptance criterion requires an engineering plan aligned with the Apply Tech
Leads; I asked in my claim whether an existing decision or discussion should
guide that step. It fits my goal of improving frontend skills while building
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

The application currently does not clearly distinguish between a form whose
instructions are unavailable and a form that simply has no instructions to
show. That makes the Form Instructions field ambiguous in the Required and
Conditionally Required form tables.

**Expected Behavior**

When a form has no instructions, both tables should show the text "Not
Applicable." This is a content-state change, not a change to system-error
handling.

**Current Behavior**

The current UI uses wording that can imply that instructions are unavailable
instead of clearly stating that the form has no applicable instructions.

**Affected Components**

The affected area is the application-page UI that renders Form Instructions
for Required and Conditionally Required forms. The issue's first acceptance
criterion requires an engineering plan aligned with the Apply Tech Leads, so
the exact component and test files still need to be confirmed before code
changes begin.

**Toolkit**

- Tool: issue-scout and the upstream issue, used to read the acceptance
  criteria and the current-versus-proposed UI examples.
- What it produced: a bounded problem statement: forms without instructions
  need the explicit "Not Applicable" state in two application-page tables.
- Difficulties: the issue requires agreement with the Apply Tech Leads before
  implementation, so I do not yet know the exact source files or test paths.
- What changed: I posted a claim comment that commits to tracing the render
  path and confirming the implementation approach before editing code.

---

## Reproduction Process

**Environment Setup**

I used macOS and fork commit
`48d93f882c63a711bdec4b16e3150d1293cdff43` with Node `v24.11.1` and npm
`11.6.4`. The frontend declares Node `>=24.19.0`, so this is focused
source-and-unit-test evidence rather than a supported browser run. The first
test attempt failed with `sh: jest: command not found` because dependencies
were absent; from `frontend/`, I ran `npm ci` with the committed lockfile.

**Steps to Reproduce**

1. From `frontend/`, run `npm run test -- ApplicationFormsTable.test.tsx`.
2. In the passing `Renders without errors` case, inspect the required-form
   fixture with no `form_instruction.download_path`; the assertion expects
   `attachmentUnavailable`.
3. Inspect `InstructionsLink` in `ApplicationFormsTable.tsx`: an absent
   download path renders `t("attachmentUnavailable")` in the Instructions
   column shared by the Required and Conditionally Required tables.
4. Observe that the English translation is `"Unavailable"`, not the
   `"Not Applicable"` wording requested in the issue.

**Reproduction Evidence**

- **Commit showing reproduction:** none; the reproduction uses fork commit
  `48d93f882c63a711bdec4b16e3150d1293cdff43` without code changes.
- **Screenshots/logs:** `npm run test -- ApplicationFormsTable.test.tsx`
  passed 1 suite and 4 tests. The run emitted existing React `act(...)` and
  jsdom navigation warnings, but Jest passed.
- **My findings:** the required-form test renders the absent-path branch, and
  the shared `InstructionsLink` code uses the same `attachmentUnavailable`
  fallback for both tables. The English message is `"Unavailable"`. I did
  not claim a supported browser reproduction because the available Node
  version is below the repository's declared floor.

**Toolkit**

- Tool: repro-check, the focused Jest suite, and source/fixture inspection.
- What it produced: a repeatable focused test command, the current
  `"Unavailable"` translation, and the shared fallback render path.
- Difficulties: Claude Code's repro-check session could not read its installed
  rubric files, and the available Node `v24.11.1` is below the repository's
  `>=24.19.0` requirement.
- What changed: I disclosed the unsupported runtime and limited the finding
  to source-and-unit-test evidence instead of claiming a browser reproduction.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
