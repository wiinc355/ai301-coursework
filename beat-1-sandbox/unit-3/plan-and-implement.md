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

wiinc355

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

---

## Your branch

**Branch**

fix/57-skip-top-level-vendor-dirs

**Evidence**

My Unit 2 reproduction steps, run from the repo root on macOS 26.6.2 (x86_64), Python 3.14.3, structlog 26.1.0, pytest 9.1.1. Steps 1, 2 and 4 of the repro (issue snippet, control, helper check) were pasted in order into one `.venv/bin/python` session:

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
print(t.execute({'files': files}).data)
print(TechDetector().execute({'files': ['main.py','core/app.py']}).data)
for p in ['node_modules/lib/index.js', 'build/bundle.js', 'src/node_modules/a.js']:
    print(p, TechDetector._should_skip_file(p))
```

**Before**: branch `fix/57-skip-top-level-vendor-dirs` at `2f4e82f` (same as `main`), no changes.

```
$ .venv/bin/python   # snippets above
2026-10-05 18:34:34 [info     ] tech_detected                  frameworks_count=0 languages_count=2 primary_lang=JavaScript
{'primary_language': 'JavaScript', 'all_languages': ['JavaScript', 'Python'], 'frameworks': []}
2026-10-05 18:34:34 [info     ] tech_detected                  frameworks_count=0 languages_count=1 primary_lang=Python
{'primary_language': 'Python', 'all_languages': ['Python'], 'frameworks': []}
node_modules/lib/index.js False
build/bundle.js False
src/node_modules/a.js True

$ .venv/bin/python -m pytest tests/unit/test_tech_detector.py -k "node_modules or build_directory" --runxfail
E       AssertionError: assert 'JavaScript' == 'Python'
E       AssertionError: assert 'JavaScript' == 'Python'
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded
======================= 2 failed, 25 deselected in 0.12s =======================
```

**After**: same branch at `7203012` (the fix).

```
$ .venv/bin/python   # same snippets
2026-10-05 18:35:00 [info     ] tech_detected                  frameworks_count=0 languages_count=1 primary_lang=Python
{'primary_language': 'Python', 'all_languages': ['Python'], 'frameworks': []}
2026-10-05 18:35:00 [info     ] tech_detected                  frameworks_count=0 languages_count=1 primary_lang=Python
{'primary_language': 'Python', 'all_languages': ['Python'], 'frameworks': []}
node_modules/lib/index.js True
build/bundle.js True
src/node_modules/a.js True

$ .venv/bin/python -m pytest tests/unit/test_tech_detector.py -k "node_modules or build_directory" --runxfail -v
tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded PASSED [ 25%]
tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded PASSED [ 50%]
tests/unit/test_tech_detector.py::TestTechDetector::test_should_skip_file_top_level_and_nested[node_modules/lib/index.js-True] PASSED [ 75%]
tests/unit/test_tech_detector.py::TestTechDetector::test_should_skip_file_top_level_and_nested[src/node_modules/lib/index.js-True] PASSED [100%]
======================= 4 passed, 29 deselected in 0.08s =======================
```

The two named tests now pass with their xfail markers removed; the `-k` filter also picks up two cases of the new parametrized test because their ids contain `node_modules`. The full file: `33 passed`. `ruff check`, `black --check` and `mypy` pass on both changed files. The pytest output above is trimmed to the assertion and result lines; the Python session output is complete.

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
