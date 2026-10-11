# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md` first. If the `Repo:` line is still
   a placeholder, stop without grading and tell the student to fill
   it. Confirm the issue URL and the fork's upstream (`git remote -v`)
   are the scoped repo; if not, refuse. Note the house rules. Then
   read `voice-guide.md` and note its rules for Verdict assembly step
   4. Eval mode: skip both files entirely.
2. Read `rubric.md` and list every check name in table order, plus the
   verdict rule. Then read `references/evidence-guide.md` so you know
   where each check's evidence lives.
3. Read the issue (eval: the Issue section; live: `gh issue view <url>
   --comments`). Note in one line the behavior the issue reports.
   Then read the thread (eval: Thread highlights) and note any
   maintainer or collaborator direction and who gave it.
4. Read the repo's stated asks (eval: the Repo facts block; live:
   `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, any AI
   policy file). List every required template section, checklist
   item, issue-link line, and required file entry (changelog,
   whatsnew), and the AI policy's disclosure rule and what it covers.
5. Read the plan before the PR (eval: the Plan context block; live:
   `plan.md`, including `## Deviations`). Write down: (a) the planned
   change and every file it names, (b) everything it puts out of
   scope or defers, (c) every deviation note, (d) the repro's failing
   command or input and its expected-after, plus any control, and (e)
   every suite or check command the test plan names. The plan comes
   first because every side-by-side check measures the PR against it;
   reading the description first makes the PR's own story the frame.
6. Read the diff next (eval: the Diff section; live: `git diff
   main...HEAD` and `git log --oneline main..HEAD`), before the
   description. List each changed file and, for each hunk, one line
   saying what it does. The diff is read before the description so
   the description is checked against the diff, not the other way.
7. Read the test evidence (eval: Test evidence section; live:
   `test_evidence.md`, plus any before/after pasted in the
   description).
8. Read the title and description last (eval: Title and Description;
   live: `pr_draft.md`). List every claim it makes about what changed.
   Read the whole package before grading any check.

## Evidence gathering

Write each fact down as a short quote or paraphrase before grading.
In eval mode, quote only from the bundle.

1. Diff matches plan: make a two-column list. Left: each hunk from
   Read order step 6. Right: the plan item or deviation note that
   covers it, or "none". Then list each in-scope plan item and mark
   it "in diff", "deferred by note", or "missing". Mark any hunk that
   touches something on the plan's not-in-scope list.
2. Description matches diff: for each claim from Read order step 8,
   write "in diff" or "not in diff", quoting the hunk. Quote any
   fidelity claim ("exactly", "no other changes", "in full") and set
   it beside any hunk marked "none" in step 1.
3. Decisive test evidence: quote the plan's repro command or input
   and expected-after. Beside it, quote what the evidence says was
   run on the branch and its stated result. Mark "same repro",
   "control or different path only", or "no repro named". If the plan
   names more than one repro or failure mode, mark each one.
4. Planned checks reported: list each suite or check command from Read
   order step 5(e). Beside each, quote its reported outcome, or write
   "not reported".
5. Clean diff: go through every hunk and mark any debug print or log,
   commented-out code, dead or unused function, whitespace, re-indent
   or import-only churn, or edit unrelated to the fix. Quote the
   lines. Note commit messages such as "wip" or "misc cleanups".
6. Template and stated asks: for each item from Read order step 4,
   quote where the description or diff meets it, or write "missing".
   For each maintainer direction from step 3, quote how the PR
   follows it, or write "contradicted" if the PR does the opposite.
7. AI-use disclosure: quote the policy's disclosure rule and what it
   covers. If it requires disclosure for PRs, code, or all AI use,
   quote the description's disclosure statement, or write "no
   disclosure".

## Check execution

1. Grade the checks in the rubric's table order, one at a time,
   against the evidence written down for that check. Do not re-read
   the whole package for a check unless its gathered evidence is
   missing a fact the pass condition needs; then go back only to the
   part the evidence guide names.
2. Apply the pass condition word for word. Grade `pass` if it is met
   and `fail` if the evidence shows it is not.
3. Grade `unclear` only when the evidence a check needs is genuinely
   absent from the package (for example, no test evidence section at
   all), never because you did not look. If the package omits
   something the pass condition requires to be present, that is a
   `fail`.
4. A gap is not drift when it is disclosed: a planned item marked
   "deferred by note" (a deviation note in the plan, or the same gap
   stated plainly in the description) passes Diff matches plan.
5. Grade each check independently: a fail in one check does not change
   another check's grade, and a well-written description does not
   earn a pass on a check its evidence fails.
6. Write one line of evidence per grade: the quote or fact that
   decided it.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if every check is
   `pass`; `reject` if any check is `fail` or `unclear` (unclear counts
   as fail). There is no third verdict.
2. For a `reject`, the deciding check is the first failing or unclear
   check in rubric table order. Name it in the summary and quote its
   evidence line; list any other failing checks after it.
3. Print a short summary, one line per check with its grade.
4. Live mode only: hold the title and description in `pr_draft.md`
   against each `voice-guide.md` rule and "Things I never post" item,
   and list any the draft breaks, quoting the rule and the line. This
   never changes the verdict.
5. If this procedure did not cover something you met, say so in the
   summary as a procedure gap instead of inventing a step.
6. End with the fenced JSON block from `SKILL.md`: one entry per
   rubric check in table order, check names exactly as the rubric
   writes them. Nothing follows the block.
