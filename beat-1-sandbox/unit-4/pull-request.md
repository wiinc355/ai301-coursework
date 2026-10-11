# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/134

**Branch**

fix/57-skip-top-level-vendor-dirs

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, the one committed as `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

That was the only run. No row came back NO, so there was nothing to re-grade with `--only`, and I made no rubric, procedure, evidence-guide, or SKILL.md edits after it. The fingerprints in the `eval-run.txt` header match the files in `tools/pr-precheck/`.

**Package analysis**

**pkg-13** (neovim, category `clear-accept`). My rubric decided **accept**, and the gold label is **accept**.

This is the package I expected to be hardest, because it delivers less than the whole problem. The fix caret-escapes `& ^ | < >` for `cmd.exe`, but `%VAR%` expansion is not handled. My rubric still passed every check, because the shortfall is disclosed in both places it needs to be. For Diff matches plan, the grader's evidence was: "Only ui.lua and ui_spec.lua change; gsub('[&|^<>]', '^%0') matches the plan's caret-escape of & ^ | < >; % deferred per plan's known limit." The pass condition lets a planned change be "deferred by a deviation note (in the plan) or a plain statement in the description", and pkg-13 has both: the plan's "Known limit" sentence and the description's "percent-sign environment expansion (`%VAR%`) is NOT handled here".

Its test evidence is short, but it is tied to the plan. It names the plan's own repro command (`:lua =vim.ui.open("https://duckduckgo.com/?q=hello&t=brave"):wait()`) and shows the before (`code = 1`, the URL cut at `&`) next to the after (`code = 0`, the full URL). The grader's evidence line was: "Re-runs the issue's own vim.ui.open(\"...?q=hello&t=brave\"):wait() on the branch; after: code = 0, full URL opens, vs before code = 1 with truncation." That is what separates it from pkg-04, whose evidence ("colors work now in paging mode") never names the plan's repro and failed the same check.

**Check rationale**

| Decisive test evidence | The test evidence (and any before/after in the description) read against the plan's test plan and the repro evidence's failing command or input, using the Test evidence section of `references/evidence-guide.md`. | The evidence re-runs the plan's own repro case on the branch (the same failing command, input, or case the plan's test plan names, identified concretely) and states its observed after-result in terms that would differ from the broken build; where the plan's test plan names more than one failure mode or repro, each is re-run or explicitly deferred. Fails when the evidence is only a general claim ("works on my machine", "colors work now", "verified for a day", "tests pass") without the plan's repro named, or exercises only a control or a different path than the one the fix changes. | required |

It reads this way because I wrote it against two packages that look alike: pkg-13 (accept) and pkg-04 (reject). Both test-evidence sections are a few lines long, and both say the fix works on Windows 11. A length- or format-based check ("has a test evidence section", "says it was tested on the affected platform") cannot tell them apart. So the deciding phrase is "re-runs the plan's own repro case on the branch (the same failing command, input, or case the plan's test plan names, identified concretely)": pkg-13 names its repro command and shows `code = 1` before and `code = 0` after; pkg-04 says only "colors work now". The check asks whether the evidence is tied to the plan, not how much it pastes.

The second sentence handles the not-tested packages where something *was* run, but the wrong thing: pkg-07 re-runs only the single-file control, and pkg-14 exercises a GET with header casing instead of the POST repro. "Exercises only a control or a different path than the one the fix changes" names that directly. "Where the plan's test plan names more than one failure mode or repro, each is re-run or explicitly deferred" is there for calib-04, where the plan names two failure modes and the evidence covers only one.

What I rejected: folding "the repo's suite was run" into this check. Several clear accepts (calib-01, pkg-19) never mention a suite. So I split that into its own check, Planned checks reported, which only asks about commands the plan's own test plan names.

**Trade-offs**

Nothing changed after the committed run, and here is how I know: there was only one full run, it agreed on all 20 packages, and I made no edits to `SKILL.md`, `rubric.md`, `procedure.md`, or `references/evidence-guide.md` afterwards. The sha256 fingerprints in the `eval-run.txt` header are of exactly those files, and they match the copies in `tools/pr-precheck/`. Because I did not loosen or tighten any check, I had no reason to run `--only` canaries.

The trade-off I accept is in Decisive test evidence. It asks that the evidence name the plan's repro and "state its observed after-result", not that it paste a raw transcript. A short annotated result line, like pkg-13's `-- after:  { code = 0, stderr = "" }`, is enough. That means a PR could name the right command and type the right result without having run it, and my rubric would accept it, because nothing in a package can prove a command was really run. I accept that miss because requiring full raw transcripts would reject terse but honest PRs like pkg-13 and calib-01, which the gold labels call ready.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
