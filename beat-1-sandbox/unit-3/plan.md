# Plan: fix #57, tech detector counts vendored and build-output files

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57

## Reproduction this plan builds on

Environment: macOS 26.6.2 (x86_64), Python 3.14.3, structlog 26.1.0,
pytest 9.1.1, `main` at commit `2f4e82f`, unmodified checkout.

1. The issue's eight-file snippet prints
   `{'primary_language': 'JavaScript', 'all_languages': ['JavaScript', 'Python'], 'frameworks': []}`.
2. Control, same call with only `main.py` and `core/app.py`: prints
   `{'primary_language': 'Python', 'all_languages': ['Python'], 'frameworks': []}`.
   So the JavaScript comes only from the `node_modules/` and `build/` files.
3. Calling `TechDetector._should_skip_file` directly:
   `node_modules/lib/index.js` -> `False`, `build/bundle.js` -> `False`,
   `src/node_modules/a.js` -> `True`.
4. `pytest tests/unit/test_tech_detector.py -k "node_modules or build_directory" --runxfail`:
   `2 failed, 25 deselected`.

## Diagnosis

`_detect_tech` does filter vendored and build paths, through
`_should_skip_file` in `agent/tools/tech_detector.py`. The filter checks
whether a pattern such as `"/node_modules/"` or `"/build/"` appears as a
substring of the path, and every pattern starts with a slash. A path at
the top of the repo, like `node_modules/lib/index.js`, has no slash
before the directory name, so it never matches and is counted. A nested
path like `src/node_modules/a.js` does match. Step 3 shows this directly,
and steps 1 and 2 show it is what turns the result from Python into
JavaScript.

## Scope

In scope, one change: make `_should_skip_file` match a skip directory
when it is the first segment of the path as well as when it is nested.
I plan to check the patterns against `"/" + filepath` instead of
`filepath`, which keeps the pattern list and the nested behavior as they
are.

Also in scope, because CONTRIBUTING.md's "Working on a seeded bug"
section requires it: remove the `@pytest.mark.xfail(strict=True, ...)`
markers from `test_node_modules_excluded` and
`test_build_directory_excluded` in `tests/unit/test_tech_detector.py`.

Not in scope:

- How the primary language is chosen. `_detect_tech` uses
  `sorted(languages)[0]`, which is alphabetical, not a file count: a
  list of three `.py` files and one `.go` file returns `Go`. That is a
  real problem, but it is a different behavior change from this issue,
  and I will not touch it here.
- The skip pattern list itself (no adding or removing directories).
- Windows-style `\` paths.

## Files and order of work

1. `agent/tools/tech_detector.py`, `_should_skip_file`: change the
   `any(...)` check to test `"/" + filepath`.
2. `tests/unit/test_tech_detector.py`: delete the two xfail markers.
3. Same test file: add one parametrized test for `_should_skip_file`
   covering a top-level path (`node_modules/a.js`, `build/b.js`), a
   nested path (`src/node_modules/a.js`), and a path that must not be
   skipped (`rebuild/x.js`, `src/main.py`).
4. Run `tests/unit/test_tech_detector.py` in full, then `ruff` and
   `mypy` as CI does, on my fork's branch `fix/57-skip-top-level-vendor-dirs`.

## Test plan

- The issue's snippet: `primary_language` changes from `JavaScript` to
  `Python`, and `all_languages` becomes `['Python']`.
- The control (`main.py`, `core/app.py`) still prints `Python`.
- The two named tests, now without xfail markers: `2 failed` before,
  `2 passed` after.
- The new parametrized test: the top-level and nested paths are
  skipped; `rebuild/x.js` and `src/main.py` are not. The `rebuild/` case
  checks that the leading slash still stops a directory that only ends
  in `build` from being skipped.
- The rest of `test_tech_detector.py` still passes, so nothing that
  passed before breaks.

## Risks and unknowns

- The fix also makes top-level `vendor/`, `dist/`, `.git/`,
  `__pycache__/`, `.venv/` and `venv/` skip, not only the two
  directories the issue names. I believe that is what the pattern list
  was meant to do, but I have not confirmed it with a maintainer, and I
  will say so in the PR.
- The only caller I found is `_detect_tech` in the same file
  (`agent/orchestrator.py` calls the tool, not the helper). I have not
  checked what path format the orchestrator passes in real runs
  (relative vs absolute); absolute paths already worked before, so the
  change should not affect them, but I have not tested that.

## Deviations

Almost nothing changed; the plan held. The fix is the one-line change to `_should_skip_file` the plan describes (checking the patterns against `"/" + filepath`), the two xfail markers are gone, and the new parametrized test is in. The only difference from the plan: the parametrized test has one extra case, `src/build/bundle.js`, so the nested check covers `build/` as well as `node_modules/`. The plan listed only `src/node_modules/a.js` for the nested case. I did not touch the primary-language selection, the pattern list, or Windows paths.

One planned step I have not done yet: checking what path format the orchestrator passes in real runs, which the plan listed as an unknown. It is still an unknown.
