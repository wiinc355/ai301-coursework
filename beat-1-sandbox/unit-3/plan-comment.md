Plan for #57, built from my reproduction on `main` at `2f4e82f` (macOS 26.6.2, Python 3.14.3).

**What I saw.** The issue's eight-file snippet prints `primary_language: 'JavaScript'`. The same call with only `main.py` and `core/app.py` prints `'Python'`. With `--runxfail`, `test_node_modules_excluded` and `test_build_directory_excluded` both fail.

**Cause.** `_should_skip_file` in `agent/tools/tech_detector.py` does filter vendored paths, but every pattern starts with a slash (`"/node_modules/"`, `"/build/"`), and the check is a substring match. A top-level path has no slash in front of the directory name, so it is never skipped. Calling the helper directly: `node_modules/lib/index.js` -> `False`, `build/bundle.js` -> `False`, `src/node_modules/a.js` -> `True`.

**Change (one PR, branch `fix/57-skip-top-level-vendor-dirs` on my fork).**
- In `_should_skip_file`, check the patterns against `"/" + filepath`. The pattern list does not change.
- Remove the two `xfail(strict=True)` markers, as CONTRIBUTING's seeded-bug section asks.
- Add one parametrized test for the helper: top-level and nested paths are skipped; `rebuild/x.js` and `src/main.py` are not.

**Not changing.** How the primary language is picked: it is `sorted(languages)[0]`, so alphabetical, not by file count (three `.py` files and one `.go` file return `Go`). That is a separate behavior change, so I'm leaving it out of this PR. Also not changing the pattern list or Windows `\` paths.

**How I'll check it.** The issue's snippet should go from `JavaScript` to `Python`; the two-file control should stay `Python`; the two named tests should go from 2 failed to 2 passed without their markers; the rest of `test_tech_detector.py` should still pass.

**Unknowns.** This also makes top-level `vendor/`, `dist/`, `.venv/` and the other listed directories skip, not just the two the issue names. I think that is what the list is for, but please say if you'd like it narrower. I also have not checked what path format the orchestrator passes in real runs.

AI use: I used Claude Code to help trace the code, run the reproduction, and draft this plan. I read the code and the output myself and checked the plan against them.
