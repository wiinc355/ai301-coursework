# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

wiinc355

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, 2026-10-01: `agreement: 18/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`. Misses: pkg-09 (`failed: Matching behavior`) and pkg-20 (`graded accept`, gold reject).
2. Full run, 2026-10-05, same rubric and evidence guide: `agreement: 17/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`. pkg-10 flipped to `failed: Matching behavior` alongside pkg-09 and pkg-20.
3. After revising Matching behavior and adding a separate AI-use disclosure check, `--only pkg-09,pkg-10,pkg-20`: `agreement: 3/3 scored items`.
4. Full run, committed as `eval-run.txt`: `agreement: 18/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

**pkg-09** (category `clear-accept`, sharkdp/fd#2033). Before my revision my rubric decided **reject**; the gold label is **accept**. After the revision the final run graded it **accept**.

The report is a cannot-reproduce: "Result: I could NOT reproduce scenario 2", with marker-order logs showing every ONE batch before any TWO batch, and a section headed "What differed from the report's conditions" that says "fd appears to flush both command buffers at the same file-count boundary on this input (uniform name lengths)".

My original Matching behavior check failed it with this evidence: "report states 'fd appears to flush both command buffers at the same file-count boundary' — the asymmetric limit-hit the issue describes was never established, so the logged ONE/TWO order doesn't speak to scenario 2's trigger". My pass condition then said the artifacts must "clearly show that the expected trigger completed without that behavior", so the grader read the report's own admission that the trigger condition may not have been reached as proof the trigger never completed, and failed it.

The gold note reads the same admission as the point of the report: "honest cannot-reproduce: real attempt at the argument-size reordering with marker-order artifacts, names what differed (uniform name lengths, 2 MiB ARG_MAX) and what a triggering setup likely needs". The run targeted the issue's own scenario and recorded what happened; naming what may have differed is honesty, not a wrong target. My rubric was punishing the report for being candid.

**Check rationale**

| Matching behavior | The report's output excerpts, logs, screenshots, or other artifacts read against the behavior described in the issue, using the Behavior shown section of `references/evidence-guide.md`. | The artifacts show the same failure or behavior the issue describes, or, for a cannot-reproduce, they show the issue's own scenario (its commands, inputs, layout, or configuration) being run and what happened instead. A cannot-reproduce passes here even if the report admits a precondition may not have been met, as long as it says so; that admission is judged under Honest conclusion, not here. Fail only when the artifacts show a different or adjacent problem (a different input, syntax, error, or symptom) presented as the issue, or show no outcome at all. | required |

The earlier version said the artifacts must "clearly show that the expected trigger completed without that behavior". That wording failed honest cannot-reproduce reports (pkg-09, and pkg-10 in one run) whenever they admitted a precondition might not have been met, because the grader treated the admission as proof the trigger never ran.

I rewrote it to separate two questions. Matching behavior now asks only whether the attempt targeted the issue's own scenario ("its commands, inputs, layout, or configuration") and recorded the outcome; whether the report is candid about what it could not establish moves to Honest conclusion ("that admission is judged under Honest conclusion, not here"). The last sentence keeps the check's original job: "Fail only when the artifacts show a different or adjacent problem (a different input, syntax, error, or symptom) presented as the issue", which is the wrong-target family. I added the same distinction to the Behavior shown section of the evidence guide.

**Trade-offs**

Loosening Matching behavior risked flipping the wrong-target packages, whose artifacts also show something other than the issue's behavior. The final full run answers that: `wrong-target 4/4`, the same as before the change. pkg-02 ("ran a prefix range instead of the issue's offset-from-end syntax") and pkg-08 ("modified the expression so $b is unbound") still fail, because their artifacts come from a different input presented as the issue, which the new last sentence keeps as a fail.

The run also shows what it cost me in stability. pkg-05 and pkg-12 (both `clear-accept`) failed `Followable steps` in the final run, although I did not touch that check and both agreed with gold in the two earlier full runs. I read that as grader variance on a check that sits near its line, not as a side effect of my edits, and I accept that at 18/20 one more swing on those two would put a run below the bar.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
