# I opened a fix for an ambiguous UI state in simpler-grants-gov, gated by tools I built myself

## Overview

Simpler.Grants.gov is the federal grants platform maintained by HHS. In its
application workspace, a form that simply has no instructions was labeled
"Unavailable" — the same wording a user would read as a system failure. I
picked up [issue #9275](https://github.com/HHS/simpler-grants-gov/issues/9275),
traced the shared fallback that rendered that label in both the Required and
Conditionally Required form tables, replaced it with a dedicated
"Not Applicable" message, and added regression coverage for both tables. I
did not work alone: every phase ran through a validation toolkit I wrote
myself — an issue-selection rubric, a ship-gate manifest listing the five
local checks a change must pass, and a PR-monitoring playbook that decides
when a follow-up is appropriate. The result is
[draft PR #13214](https://github.com/HHS/simpler-grants-gov/pull/13214),
opened on November 8, 2026, which today remains an open draft awaiting Apply
Tech Leads direction on the approach and capitalization.

## What happened

### Finding and bounding the problem

My issue-scout scan on October 21 processed 400 search results (362 unique
issues) and accepted #9275 as active, unclaimed, and bounded. The same run
caught that an Altinn issue I had already claimed had an open PR from August
closing it — something I had missed — so I released that claim instead of
duplicating work. The scan also exposed a flaw in my own rubric: ranking by
fewest comments filled the top results with brand-new repositories, so I cut
the per-repository cap from two candidates to one.

On the unmodified baseline commit, the focused Jest suite showed the bug's
mechanism: a missing `form_instruction.download_path` fell through to the
`attachmentUnavailable` key, which the English catalog maps to "Unavailable".

### The fix, with the diff

My change swaps the shared fallback to a new `notApplicable` key and removes
the now-unused one:

```diff
-                unavailableText={t("attachmentUnavailable")}
+                unavailableText={t("notApplicable")}
```

```diff
       attachment: "Attachment",
-      attachmentUnavailable: "Unavailable",
       conditionalForms: "Conditionally-Required Forms",
...
       include: "Include with application",
+      notApplicable: "Not Applicable",
```

The updated test asserts the user-visible wording in both tables instead of
the translation key, and adds a missing-instructions case for a conditionally
required form:

```diff
-    expect(tables[0]).toHaveTextContent("attachmentUnavailable");
+    expect(tables[0]).toHaveTextContent("Not Applicable");
```

After the fix, the focused suite passed 1 suite and 5 tests (up from 4),
alongside passing lint, type, and format checks under Node v26.3.0.

### My own gate blocked me first

Before pushing anything, I ran my ship-gate — a manifest of five local checks
I wrote for this repository. Its first run on November 1 stopped me cold,
because I had created my feature branch in the wrong place and was validating
the base branch itself. From `gate-log.md`:

```text
2026-11-01 23:28 [manual] HHS/simpler-grants-gov @ unknown (base main)
- verdict: BLOCK (block)
- reasons quoted:
  - you are on main, the base branch itself. Nothing here is a pull request:
    check out your fix branch and run again.
```

I corrected the branch, and pr-precheck then rejected my first PR draft for
two more reasons: no designated maintainer mention, and unresolved Apply Tech
Leads alignment. The second draft added @btabaska and disclosed the pending
alignment and UAT explicitly instead of claiming them. After rebasing past 93
upstream commits, the gate's next run passed:

```text
2026-11-08 15:54 [manual] HHS/simpler-grants-gov @ wiinc355/9275-form-instructions (base main)
- verdict: PASS
```

That pass — all five local checks plus pr-precheck — is what let me push the
branch and open the draft PR the same day.

### The monitor's miss, and its next run catching it

Once the PR was open, my PR monitor drafted a wait note using my playbook's
17-day quiet threshold. Reviewing it, I found a one-day arithmetic error: it
placed the threshold on November 26 instead of November 25. I corrected the
calculation and added a verification rule to the playbook:

```text
- Verify any proposed date independently by adding 17 * 24 hours to the
  exact UTC timestamp; recalculate if someone else comments or reviews.
```

The monitor's next run produced the correct date it had previously missed:

```text
The threshold is met at 2026-11-25T22:01:39Z. That is the opening timestamp
plus 17 x 24 hours.
```

The PR has not merged. It is an open draft with no maintainer comments yet,
and that is the honest state of the contribution today: the code, tests, and
local checks are done; the decision belongs to the Apply Tech Leads.

## Limitations and what's next

My evidence is source-and-unit-test evidence, not a browser reproduction: the
Jest suite mocks translations, so I verified the English catalog mapping
separately and never ran the change in a supported browser or through UAT.
GitHub's frontend, pa11y, and end-to-end Actions all show `action_required`
awaiting maintainer approval, so my five local passes are not a claim that CI
has passed. My monitor also reported "no checks" when those three runs
existed — it cannot see approval-gated Actions, which is exactly the kind of
silent gap that caused its date error.

Next, I will add date-verification tests to the monitor, make missing
Actions-approval state visible in its reports, and test both against the
November 25 follow-up threshold. If the Apply Tech Leads request a different
approach or capitalization ("Not applicable" appears in the issue summary),
the test suite I added makes that a one-line revision rather than a redesign.
