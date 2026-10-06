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

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-6006344267

Hi, I'd like to work on #57 as my first contribution here. I plan to run the issue's eight-file snippet and the two named tests (`test_node_modules_excluded`, `test_build_directory_excluded`) on an unmodified checkout of `main`, add a control run without the `node_modules/` and `build/` paths, and then look at how `agent/tools/tech_detector.py` filters those paths. I'll post the environment, commands, and output I see before making any code changes.

AI use: I'm using Claude Code to help run the reproduction and draft my comments. I review the commands and output myself before posting.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-6006345360

Reproduction report for #57. **Result: reproduced** on an unmodified checkout of `main`.

**Environment**

- macOS 26.6.2 (x86_64), Python 3.14.3. The project targets `>=3.11`; 3.14.3 is above that, and this code is plain string handling, but I'm noting the difference.
- Commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, `git status` clean.
- Only the packages this code and its tests need, in a fresh venv: structlog 26.1.0, pytest 9.1.1. No database, Docker, or API key.

**Setup**

```bash
git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
cd pathreview-ai301-fa26-s3
git checkout 2f4e82f52efbcfcc57d65b3fa5348672163ca088
python3 -m venv .venv
.venv/bin/pip install structlog==26.1.0 pytest==9.1.1
```

Steps 1, 2 and 4 are Python snippets. Run them in one session of the venv's interpreter, started from the repo root with `.venv/bin/python`, and paste them in order (steps 2 and 4 reuse step 1's import). Plain `python3` will not find `structlog`.

**1. The issue's snippet:**

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
print(t.execute({'files': files}).data)
```

```
2026-10-05 18:27:27 [info     ] tech_detected                  frameworks_count=0 languages_count=2 primary_lang=JavaScript
{'primary_language': 'JavaScript', 'all_languages': ['JavaScript', 'Python'], 'frameworks': []}
```

**2. Control**: same call with only the two Python files:

```python
print(TechDetector().execute({'files': ['main.py','core/app.py']}).data)
```

```
2026-10-05 18:27:27 [info     ] tech_detected                  frameworks_count=0 languages_count=1 primary_lang=Python
{'primary_language': 'Python', 'all_languages': ['Python'], 'frameworks': []}
```

So the JavaScript result comes only from the `node_modules/` and `build/` paths.

**3. The two named tests**, with the xfail markers ignored (output trimmed to the assertion and summary lines):

```
$ .venv/bin/python -m pytest tests/unit/test_tech_detector.py -k "node_modules or build_directory" --runxfail
E       AssertionError: assert 'JavaScript' == 'Python'
E       AssertionError: assert 'JavaScript' == 'Python'
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_node_modules_excluded
FAILED tests/unit/test_tech_detector.py::TestTechDetector::test_build_directory_excluded
2 failed, 25 deselected in 0.11s
```

**4. Where the filter misses.** `_detect_tech` does filter paths through `_should_skip_file`. Calling it directly:

```python
for p in ['node_modules/lib/index.js', 'build/bundle.js', 'src/node_modules/a.js']:
    print(p, TechDetector._should_skip_file(p))
```

```
node_modules/lib/index.js False
build/bundle.js False
src/node_modules/a.js True
```

The skip patterns all start with a slash (`"/node_modules/"`, `"/build/"`), so a path where the directory is at the top level is not skipped, while the same directory nested under `src/` is.

**Expected (per the issue):** `primary_language` is `'Python'`.
**Actual:** `'JavaScript'`, in the snippet and in both named tests.

**Something I noticed but did not chase:** the primary language is picked alphabetically, not by file count. `['a.py','b.py','c.py','x.go']` returns `Go`. That is separate from this issue; I'm only noting it.

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
