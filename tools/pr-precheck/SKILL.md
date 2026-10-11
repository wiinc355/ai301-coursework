---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
pull request ready to submit? A PR package is a candidate pull
request (its title, description, commits, diff, and test evidence),
read against the plan it claims to implement (with that plan's
deviation notes and the repro evidence it built on) and against the
issue that plan belongs to (its thread, and the repo's stated PR
template and contribution policy). Grade exactly one package per run.
Do not grade whether the plan was a good plan, whether the issue is
worth fixing, or whether the code is how you would have written it:
grade only whether this PR delivers its plan, proves it, is
reviewable, and meets the repo's stated asks. Never answer from gut
feel; answer by executing the components in this directory.

## Inputs and modes

Run in exactly one of two modes.

- **Live mode**: the student's own PR, checked before it is opened.
  Run from the top folder of the student's fork clone, on their
  branch. Read these inputs and nothing else as the package:
  - the plan: `plan.md` in the working directory, including its
    `## Deviations` section (deviation notes are part of the plan);
  - the diff: the output of `git diff main...HEAD` (three dots), run
    in the working copy, plus the commit list from
    `git log --oneline main..HEAD`. Only committed changes count;
    uncommitted or untracked files are not part of the PR;
  - the draft PR title and description: `pr_draft.md` in the working
    directory (the first line is the title, everything after it is
    the description);
  - the test evidence: `test_evidence.md` in the working directory;
  - the issue: the issue URL the student gives, read with `gh issue
    view <url> --comments` (or the GitHub API or web) for the body
    and the full thread;
  - the repo's stated asks: `.github/PULL_REQUEST_TEMPLATE.md` and
    `docs/CONTRIBUTING.md` in the working copy (and any AI policy
    file the repo has).
  A house-chain student reads the house plan and the house repro pack
  instead of their own; the same checks grade the same things. If the
  student names other file paths for any input, use those.
- **Eval mode**: a package bundle (a markdown file with repo facts,
  the issue, thread highlights, a plan-context block, and the
  candidate PR's title, description, commits, diff, and test
  evidence). The bundle is the whole world: take every fact from the
  bundle text, fetch nothing, and read no other file except this
  tool's own components. Eval mode always grades a complete package:
  every check in the rubric, with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. If its `Repo:` line still carries the bracketed placeholder
(anything like `<ORG>/<PATH-REVIEW-REPO>`), stop without grading and
tell the student to fill the `Repo:` line in `scope.md` with their
section's Path Review repo. Never guess a scope. Otherwise confirm the
issue URL and the PR's target repo (the fork's upstream, from
`git remote -v`) are the scoped repo; if not, refuse to grade and say
why. Apply the house rules scope.md lists as part of the repo's
stated asks (branch named with a type prefix, the issue number, and a
slug; one PR per issue; the template always used). In eval mode,
ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the outgoing PR text, the title and the
description in `pr_draft.md`, against each rule and each item on its
"Things I never post" list. In the summary, list every rule the draft
breaks, quoting the rule and the line of the draft that breaks it.
The voice guide never changes the verdict on its own: no rubric check
reads it, so a voice note is advice for the student, not a grade. In
eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read and use the components this way:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. It decides WHAT is graded.
- `references/evidence-guide.md` maps where each evidence family
  lives, in a bundle and in live mode, and what good looks like. It
  decides WHERE to look.
- `procedure.md` is the operating procedure: the read order, the
  evidence gathering, how each check executes, and how grades become
  the verdict. Execute it as written, step by step. Where the
  procedure is silent on something you meet, report it in the
  summary as a procedure gap; never improvise a step around it.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in (instruction comments do not count as content), refuse to
grade: say which file is empty and stop. Do not invent checks or steps
at runtime.

## Verdict and output

The verdict is binary: `accept` means ready to submit (open the PR);
`reject` means hold (fix first). There is no third verdict and no
score; put reservations in check evidence lines, not in the verdict.

Before the JSON you may print a short readable summary: one line per
check with its grade, the deciding check for a reject, any procedure
gaps, and (live mode) any voice-guide notes. Then end the reply with
the fenced JSON block below, one entry per rubric check in rubric
order, using the rubric's check names exactly. The block must be
valid JSON and the last thing in the reply; nothing follows it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, before the PR exists, use the issue URL followed by the
branch name as the `item` (for example
`https://github.com/org/repo/issues/57 @ fix/57-slug`).

## Grading discipline

- Evidence first: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready,
  and a beautiful confident one can hide drift. Read the diff itself
  against the plan, the evidence itself against the test plan, and
  the description against the diff; never trust a description's
  claim about the diff without checking the diff.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Unclear defaults to fail: treat `unclear` as the rubric's verdict
  rule directs. Where the rule is silent, an unverifiable claim is a
  failing one: a PR you cannot verify from the package is not ready
  to submit.
- Honest shortfalls are not failures: a PR that delivers less than
  everything can be ready when the plan's deviation notes and the
  description both state the gap plainly.
