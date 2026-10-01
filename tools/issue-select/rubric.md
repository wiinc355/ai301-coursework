# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Active project | Repo-facts block: `archived`, the five default-branch commit dates, and the latest release date. | The repository is not archived, and at least one default-branch commit or release is dated within 180 days of the bundle capture date. | required |
| Maintainer engagement | Repo-facts block: maintainer first-response sample and whether the issue was opened by an owner, member, or collaborator. | A maintainer response in the sample occurred within 90 days, or the issue itself was opened by an owner, member, or collaborator. | required |
| Contribution policy | Repo-facts block: contribution policy. | The policy has no restriction on AI-assisted contributions, or it explicitly permits assistive AI use when the contributor understands and takes responsibility for the change. | required |
| Available work | Repo-facts block: assignees and linked PRs; issue comment thread. | The issue has no assignee or open linked PR, and no non-bot commenter has said they are actively implementing it or have opened a PR within the last 90 days before capture. | required |
| Bounded, actionable scope | Issue body and comment thread. | The issue names a concrete user-visible problem or documentation change and identifies a bounded target (such as a file, component, command, API, or page) or an explicit acceptance criterion. It is not an umbrella, tracking, or unresolved product/design decision. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only when every required check passes. Reject if any required check
fails or is unclear. There are no preferred checks in this rubric.
