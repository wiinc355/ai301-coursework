Plan and result for #57, built from my reproduction on `main` at `2f4e82f` (macOS 26.6.2, Python 3.14.3).

Credit first: @BishalChhetri traced the leading-slash cause above, @hworku24 pointed out the alphabetical primary-language pick, and @kneha07 posted a plan with the same fix. I checked the cause and the alphabetical pick in my own run, and this is my own trace, test plan, and build; the `rebuild/x.js` and `src/build/bundle.js` test cases are what I added.

**What I saw.** The issue's eight-file snippet prints `primary_language: 'JavaScript'`. The same call with only `main.py` and `core/app.py` prints `'Python'`. With `--runxfail`, `test_node_modules_excluded` and `test_build_directory_excluded` both fail.

**Cause.** `_should_skip_file` in `agent/tools/tech_detector.py` does filter vendored paths, but every pattern starts with a slash (`"/node_modules/"`, `"/build/"`), and the check is a substring match. A top-level path has no slash in front of the directory name, so it is never skipped. Calling the helper directly: `node_modules/lib/index.js` -> `False`, `build/bundle.js` -> `False`, `src/node_modules/a.js` -> `True`.

**Change (one PR, built on branch `fix/57-skip-top-level-vendor-dirs`).**
- In `_should_skip_file`, the patterns are now checked against `"/" + filepath`. The pattern list does not change.
- Removed the two `xfail(strict=True)` markers, as CONTRIBUTING's seeded-bug section asks.
- Added one parametrized test for the helper: top-level and nested `node_modules/` and `build/` paths are skipped; `rebuild/x.js` and `src/main.py` are not. The `rebuild/` case checks that a directory that only ends in `build` is not caught.

**Not changing.** How the primary language is picked: it is `sorted(languages)[0]`, so alphabetical, not by file count (three `.py` files and one `.go` file return `Go`). That is a separate behavior change, so I'm leaving it out of this PR. Also not changing the pattern list or Windows `\` paths.

**What I checked after the change.** I re-ran my repro steps on the branch. The issue's snippet went from `JavaScript` to `Python`; the two-file control stayed `Python`; the helper now returns `True` for `node_modules/lib/index.js` and `build/bundle.js`; the two named tests went from 2 failed to 2 passed without their markers; the full `test_tech_detector.py` passes (33 tests), and `ruff`, `black --check` and `mypy` pass on the two changed files.

**Unknowns.** This also makes top-level `vendor/`, `dist/`, `.venv/` and the other listed directories skip, not just the two the issue names. My read is that the list was meant to work this way, but I haven't confirmed it; if only `node_modules/` and `build/` should change, I can narrow the fix. I also have not checked what path format the orchestrator passes in real runs.

AI use: I used Claude Code to help trace the code, run the reproduction, make the edits, and draft this plan. I read the code and the output myself and checked the plan against them.
