Hi, I'd like to work on #57 as my first contribution here. I plan to run the issue's eight-file snippet and the two named tests (`test_node_modules_excluded`, `test_build_directory_excluded`) on an unmodified checkout of `main`, add a control run without the `node_modules/` and `build/` paths, and then look at how `agent/tools/tech_detector.py` filters those paths. I'll post the environment, commands, and output I see before making any code changes.

AI use: I'm using Claude Code to help run the reproduction and draft my comments. I review the commands and output myself before posting.
