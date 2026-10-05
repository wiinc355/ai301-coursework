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
