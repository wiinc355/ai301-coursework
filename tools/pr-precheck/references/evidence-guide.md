# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives, eval bundle: the plan's scope, files, not-in-scope
list, and any deviation or deferral note are in the Plan context block
(the "Plan:" paragraph). What actually changed is in the candidate
PR's Diff section (one `--- a/` / `+++ b/` pair per file, then hunks)
and the Commits list. The description's claims about what changed are
in the Description section.

Where it lives, live mode: the plan is `plan.md` in the fork clone,
with deviation notes under `## Deviations`. The diff is
`git diff main...HEAD` (committed changes only) and
`git diff main...HEAD --stat` for the file list. The description is
`pr_draft.md` (title on the first line).

What good looks like: every file and hunk in the diff maps to the
plan's in-scope change or a deviation note, and every in-scope plan
item is in the diff or deferred by a note. Drift in the "more"
direction is an unplanned option, flag, setting, rename pass, or
rewrite riding along; drift in the "less" direction is a planned item
missing with no note. The description's claims match the diff: a
description that says "exactly as planned" or "the docs now say X"
over a diff that adds extra work or has no docs hunk is silent drift.
An honest shortfall (plan note plus description stating the gap) is
not drift.

## Test evidence (harness category: not-tested)

Where it lives, eval bundle: the Test evidence section of the
candidate PR, plus any before/after pasted in the Description. Read
it against the plan's "Test plan:" sentence and the "Repro evidence"
paragraph at the top of the Plan context block (the failing command
or input, the expected-after, and any control).

Where it lives, live mode: `test_evidence.md` next to `plan.md`
(before on `main`, after on the branch, and the output of
`make test-unit`, `make test-integration`, `make lint`,
`make typecheck`), read against the `## Test plan` and
`## Reproduction this plan builds on` sections of `plan.md`.

What good looks like: the evidence names the plan's own repro command
or input, says it was re-run on the branch, and states an after-result
that would differ on the broken build (the output, exit code, or
behavior the plan expected). Each failure mode the test plan names is
re-run or explicitly deferred, and each suite or check command the
plan names has a reported outcome. Not tested looks like "works on my
machine", "tests pass" with no command, or a run of only the control
or a path the fix does not change.

## Diff quality (harness category: unreviewable)

Where it lives, eval bundle: the Diff section's hunks, and the Commits
list for message tells. Live mode: `git diff main...HEAD` hunk by
hunk, and `git log --oneline main..HEAD`.

What good looks like: every `+` and `-` line belongs to the fix or its
tests; nothing unrelated rides along. Debris tells: `print`,
`eprintln!`, `console.log`, or `DEBUG` lines; commented-out code or
an earlier attempt left as comments; a function marked unused or
`allow(dead_code)`; `TODO` notes added to untouched code; hunks that
only re-indent, reorder imports, or re-print identical lines; and
commit messages like "wip", "fix", or "misc cleanups while
debugging". A short comment that explains the new line is not debris.

## Standards and comms (harness category: standards-wall)

Where it lives, eval bundle: the Repo facts block's "pull requests:"
line (template sections, required checklist, issue-link line,
changelog or whatsnew entry) and "contribution policy:" line (AI
policy and what it covers), plus Thread highlights for maintainer
direction. Where the PR honors them: the Description (sections,
checklist, "closes #N" or "fixes #N", AI disclosure) and the Diff
(the changelog or whatsnew file entry).

Where it lives, live mode: `.github/PULL_REQUEST_TEMPLATE.md` (Path
Review's sections: Summary, Issue with `Closes #`, Changes, Testing
checklist, Screenshots / Demo, Notes for Reviewers), `docs/CONTRIBUTING.md`
(branch naming, Conventional Commits, remove the seeded bug's xfail
marker and any `pyproject.toml` suppression, fill the template
completely), `scope.md`'s house rules, and the issue thread. The
course always asks for an AI-use disclosure; Path Review's template
has no section for it, so it goes in Notes for Reviewers.

What good looks like: every required section, checklist item, issue
line, and file entry is present with content specific to this PR, no
section is left as the template's placeholder comment, ticked boxes
are true per the evidence, and where the policy requires AI-use
disclosure the description names the tool and how it was used. A
visibly skipped required checklist, a missing "closes #", a missing
whatsnew entry, or no disclosure under an "all AI use" policy is a
standards wall. (Whether the description's claims match the diff is
plan fidelity, above.)
