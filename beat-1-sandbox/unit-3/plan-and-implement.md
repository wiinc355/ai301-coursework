# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

[Your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comment upstream is identified by this name, and it is
the only thing that ties it to you. Several students may plan the same
house issue, so this is what keeps their comments off your score and
yours off theirs.]

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

---

## Your branch

**Branch**

[The name of the branch you built the change on, exactly as it appears in your fork. The
naming shape is a type prefix, then the issue number, then a short description. **The issue
number in the branch name must be the number of the issue you claimed** — a name carrying
any other number does not satisfy this field.]

**Evidence**

[Your Unit 2 reproduction steps re-run against the built change: the before, then the
after. Paste both, including the commands you ran and their output.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3`: `agreement: 3/3 scored items` (pkg-01, pkg-02, pkg-03 all agreed with gold).
2. Full run, the one committed as `eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

No rubric, procedure, or evidence-guide edits happened between the two runs; the smoke run only confirmed the setup before spending on the full run.

**Package analysis**

**pkg-14** (category `clear-accept`). My rubric decided **reject**; the gold label is **accept**. It was my only disagreement.

The one failing check was Honest unknowns. The grader's evidence line was: "Asserts as fact 'reattach-path change in 0.44.2' and the stdin-before-drain mechanism, neither shown by the repro; comment claims '0.44.2 on leaking' though repro ran 0.44.3 and 0.44.1; only keystroke-eating is named as a risk."

The plan does say "0.44.1, which predates the reattach-path change in 0.44.2, is clean on the same setup", and the repro never ran 0.44.2 itself. My pass condition says "anything the repro evidence did not show (an untested platform, version, or code path the plan depends on) is named as an unknown or risk rather than asserted as fact", so the grader applied it literally: an untested version named as the cause, without being labelled as an inference, fails.

The gold note reads it differently: "honestly scoped-down: reattach handshake fix with a regression-window repro; defers the untestable Windows variant and says so; arguable on the deferral, ready as scoped." A clean 0.44.1 and a broken 0.44.3 bracket the regression, so pointing at 0.44.2 is a fair inference from the evidence, not false confidence. And the plan is honest where it matters: it explicitly defers the Windows variant ("I cannot test Windows") and names its one real risk. My check cannot tell a reasonable inference from a regression window apart from an invented cause, so it holds a plan the gold label considers ready.

**Check rationale**

| AI-use disclosure | The repo-facts block's contribution policy (CONTRIBUTING.md, AI_POLICY.md, or similar) read against the candidate plan comment, using the Comms section of `references/evidence-guide.md`. | Treat every package as AI-assisted work, because it was drafted and checked with this skill; whether the comment mentions AI is not the test. If the policy requires disclosing AI use in issues or comments, the plan comment must name the tool and the extent of its use, and without that statement this fails. Passes when the policy has no disclosure requirement, requires disclosure only for pull requests or code, or only asks that comments be in the contributor's own words. | required |

It reads this way because of what went wrong in Unit 2. There, disclosure was one clause ("including required AI-use disclosure") inside a broad communication check, and the grader passed pkg-20 (Ghostty, whose policy says all AI usage must be disclosed) with the reasoning "no AI use indicated so AI_POLICY.md disclosure is not triggered". That cost me the disclosure category floor. I fixed it in Unit 2 by splitting disclosure into its own required check, and I wrote it that way from the start here.

The two sentences that matter are "Treat every package as AI-assisted work" and "whether the comment mentions AI is not the test": they remove the escape hatch the Unit 2 grader used. The pass list at the end is the other half: it keeps the check from rejecting repos whose AI rule covers only pull requests or code, or only asks for comments in the contributor's own words, which an over-broad "always disclose" check would have rejected. Both thread-convention packages (pkg-04, pkg-20) agreed with gold in the full run.

**Trade-offs**

The trade-off I accept is in Honest unknowns, and the package it changes is pkg-14. The check fails any cause, version, or code path the repro did not directly show unless the plan labels it as an unknown. That strictness is what I want against plans that dress up a guess as a diagnosis, but it also catches a reasonable inference: pkg-14's "reattach-path change in 0.44.2" follows from a clean 0.44.1 and a broken 0.44.3, yet the check counted it as an untested claim stated as fact, and the run graded a clear accept as reject.

I considered loosening the pass condition to let a claim through when it "follows directly from the repro's evidence, such as a regression window", but did not make that change. The run already clears the bar at 19/20, and a looser wording risks letting through plans that state an untested cause as settled, which is exactly what this check exists to stop. I did not re-run any `--only` canaries because I changed nothing after the committed run. I accept that this check will keep missing plans like pkg-14 that reason from a bracketed regression window without saying it is an inference.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
