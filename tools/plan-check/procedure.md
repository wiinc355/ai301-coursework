# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Live mode only: read `scope.md` first. Confirm the issue is in the
   scoped repo (if not, or if the Repo line is still a placeholder,
   stop without grading). Note any house rules. Then read
   `voice-guide.md` and note its rules for step 4 of Verdict assembly.
   Eval mode: skip `scope.md` and `voice-guide.md` entirely.
2. Read `rubric.md` and list every check name and the verdict rule.
   Then read `references/evidence-guide.md` so you know where each
   check's evidence lives.
3. Read the issue context. Note in one line the behavior the issue
   reports and what the reporter expects instead.
4. Read the repro evidence before the plan. Note: the environment, the
   failing step and its artifact, any control run (what was changed
   and what stayed the same), and the actual-vs-expected line. These
   notes are what the plan's diagnosis and test plan must answer to,
   so they come first; reading the plan first makes its story the
   frame instead of the evidence.
5. Read the thread highlights. Note any direction from a maintainer
   or collaborator (an approach preferred or ruled out, a request to
   wait or coordinate) and who gave it.
6. Read the repo-facts block. Note the contribution policy's
   AI-disclosure rule (and whether it covers comments or only pull
   requests or code) and any template or contributing asks.
7. Only now read the candidate plan, then the candidate plan comment.
   Read the whole package before grading any check.


## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

For each check, pull the fact from the place named below and write it
down as a short quote or paraphrase before grading. In eval mode, quote
only from the bundle; in live mode, use the issue page, its thread, the
student's posted repro comment, and the repo's `CONTRIBUTING.md` or AI
policy file, as the evidence guide says.

1. Grounded diagnosis: quote the plan's stated cause. Next to it, put
   the repro evidence's deciding observation from Read order step 4
   (usually the control run or the actual-vs-expected line).
2. Bounded scope: list everything the plan says it will change (in
   scope, files named, and any extras or follow-ups inside the plan).
   Next to it, put the change the issue asks for and any scope
   direction from the thread.
3. Executable plan: list the files, functions, or areas the plan
   names and the concrete change it describes for each. If none are
   named, record "none named".
4. Decisive test plan: quote the plan's test plan. Next to it, put the
   repro's failing command or input and its expected result, plus any
   control run.
5. Honest unknowns: list each claim the plan or comment states as
   certain. Mark each one "shown" if the repro evidence or thread
   establishes it, or "not shown" if it does not; then record whether
   each "not shown" claim is labelled as a risk or unknown.
6. Thread and conventions: list each maintainer or collaborator
   direction from Read order step 5, and quote how the plan and comment
   respond to it (or record "not addressed"). Note any template or
   contributing ask from the repo facts and whether it is met.
7. AI-use disclosure: quote the policy's disclosure rule and its
   coverage (comments, or pull requests and code only, or none). If it
   covers comments, quote the plan comment's disclosure statement, or
   record "no disclosure".


## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade the checks in the rubric's table order, one at a time,
   against the evidence written down for that check in Evidence
   gathering. Do not re-read the whole package for a check unless its
   gathered evidence is missing a fact the pass condition needs; then
   go back only to the section the evidence guide names for it.
2. Apply the check's pass condition word for word. Grade `pass` if the
   condition is met, `fail` if the evidence shows it is not met.
3. Grade `unclear` only when the evidence the check needs is genuinely
   absent from the package (for example, the plan has no test plan at
   all), never because you did not look. If a plan omits something its
   check requires, that is a `fail` when the pass condition demands it
   be present.
4. Grade each check independently: a failure in one check does not
   change another check's grade, and a well-written plan does not earn
   a pass on a check its evidence fails.
5. Write one line of evidence for each grade: the quote or fact from
   Evidence gathering that decided it.


## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Apply the rubric's verdict rule: `accept` only if every required
   check is `pass`; `reject` if any required check is `fail` or
   `unclear`. There is no third verdict.
2. For a `reject`, name the deciding check or checks in the summary
   and quote the evidence line that failed each one.
3. Output a short readable summary (one line per check), then the
   fenced JSON block from `SKILL.md`, with one entry per rubric check
   in table order, using the check names exactly as the rubric writes
   them. The JSON block is the last thing in the output.
4. Live mode only: before the JSON block, check the draft plan comment
   against each `voice-guide.md` rule and list any rule it breaks,
   quoting the rule. This never changes the verdict.
5. If this procedure did not tell you how to handle something you met
   while grading, say so in the summary as a procedure gap instead of
   inventing a step.

