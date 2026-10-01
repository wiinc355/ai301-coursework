# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57

**Verdict output**

Live-mode run: `claude "issue-select: grade these candidate first issues: .../issues/57 .../issues/63 .../issues/40"`. Verdict for #57: `accept`.

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
I graded all three issues against your rubric, and all three are accepted. Since the rubric has no preferred checks, the order below comes only from your fit profile.

I gathered the evidence from the public GitHub API through web fetches, because the `gh` CLI needed approval this session wouldn't give. One limit: I only found 6 pull requests, numbered #78–#83, and none of them mention #40, #57 or #63.

**How the shared checks came out** (the same for all three):
- **Active project:** the repo isn't archived, and the latest commit to the main branch is from 2026-09-16, 14 days ago.
- **Maintainer engagement:** all three were opened by Aburke225, a collaborator on the repo.
- **Contribution policy:** `docs/CONTRIBUTING.md` doesn't mention AI at all, and silence counts as a pass.

**Ranked (all accepted):**

1. **#40: "Copy link" share button.** This is the best fit because it's the only frontend issue (`ReviewPage.tsx`, `shareService.ts`), which is what you want to get better at. It also has a Python side (`api/routes/reviews.py`), and Python is what you already know. No one has commented, claimed it or linked a PR. The scope passes because it names the files and has clear requirements: the link works without login and expires after 30 days.
   - **Catch:** it's the biggest of the three. It's labeled tier-2, estimated at 5–8 hours, and touches the frontend, the API and token expiry. The rubric doesn't check size, so it passes, but you'd be learning frontend and taking on a full feature at once.
2. **#57: tech detector counts `node_modules/` and `build/` files.** This is a bounded Python bug in `tech_detector.py`, with two failing tests that define when it's done. Four classmates have posted claims or reproduction reports. Your house rule says those don't block you, and the root cause (path patterns that only match with a leading slash) has already been worked out in the comments.
3. **#63: README test fixture too short.** This is the smallest job: one test file, with a one-command reproduction (`assert 51 > 100`). It's Python and tests only, so it doesn't help your frontend goal. Two classmates have posted on it, which doesn't count against you.

**Rejected:** none.

If you want a safer first PR, #57 is the tier-1, Python-only option. Pick #40 if growing your frontend skills matters more than finishing quickly.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/40",
    "checks": [
      {"name": "Active project", "grade": "pass", "evidence": "archived: false; latest default-branch commit 2026-09-16 (14 days before 2026-09-30)"},
      {"name": "Maintainer engagement", "grade": "pass", "evidence": "Issue opened by Aburke225, author_association COLLABORATOR"},
      {"name": "Contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI restriction (silence)"},
      {"name": "Available work", "grade": "pass", "evidence": "No assignees, no linked PRs, 0 comments, not referenced by any of the 6 PRs found (#78-#83)"},
      {"name": "Bounded, actionable scope", "grade": "pass", "evidence": "Names ReviewPage.tsx, shareService.ts, reviews.py; acceptance criteria: read-only link, no login, expires after 30 days"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57",
    "checks": [
      {"name": "Active project", "grade": "pass", "evidence": "archived: false; latest default-branch commit 2026-09-16 (14 days before 2026-09-30)"},
      {"name": "Maintainer engagement", "grade": "pass", "evidence": "Issue opened by Aburke225, author_association COLLABORATOR"},
      {"name": "Contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI restriction (silence)"},
      {"name": "Available work", "grade": "pass", "evidence": "No assignees or linked PRs; student claim comments (e.g. ApoorvThite 2026-09-21 'working on it') are ignored per Path Review house rule"},
      {"name": "Bounded, actionable scope", "grade": "pass", "evidence": "tech_detector.py must skip node_modules/ and build/; failing tests test_node_modules_excluded and test_build_directory_excluded"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63",
    "checks": [
      {"name": "Active project", "grade": "pass", "evidence": "archived: false; latest default-branch commit 2026-09-16 (14 days before 2026-09-30)"},
      {"name": "Maintainer engagement", "grade": "pass", "evidence": "Issue opened by Aburke225, author_association COLLABORATOR"},
      {"name": "Contribution policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI restriction (silence)"},
      {"name": "Available work", "grade": "pass", "evidence": "No assignees or linked PRs; comments from Alonso-Lopez-1 and anshbabar are investigation claims, ignored per house rule"},
      {"name": "Bounded, actionable scope", "grade": "pass", "evidence": "test_readme_with_all_quality_signals fixture is 51 words vs '> 100' assertion; repro: pytest tests/unit/test_readme_scorer.py -q"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run: 0/0. Every issue errored with `claude exited 1: `, so nothing was graded (a CLI problem, not the rubric).
2. Smoke run (`--limit 1`): 0/1. issue-01 graded reject, gold accept, `failed: Bounded, actionable scope`.
3. Full run (`--save-run eval-run.txt`): **15/20**. `categories: claimed 4/4  clear-accept 4/8  dead-repo 3/3  policy 1/1  scope 3/4`. This is the committed `eval-run.txt`.

**Issue analysis**

**issue-15** (zulip/zulip#19589). My rubric: **accept**. Gold label: **reject** ("years of design debate and two abandoned PRs behind a friendly label").

Every required check passed. Active project: "last push 2026-08-04, commits through 2026-08-03, release 2026-06-26". Maintainer engagement: "#39859 at 2.6 days, within the 90-day threshold". Contribution policy: "AI tools allowed; contributors must personally understand, test, and be able to explain every change". Available work: "linked PRs #20840 and #23123 both closed; last visible claim comment (souvik150, 2024-02-16) is far outside the 90-day pre-capture window". Bounded scope: "splitting outgoing webhook `text` into `command`/`text` fields matching Slack's slash-command format, with a concrete sample payload as acceptance criterion".

The rubric read it as accept because Available work only counts *open* PRs and *recent* claims, so the two closed PRs counted as "available". The scope check only asks whether a target is named. Neither check asks why earlier attempts failed. The thread runs from 2021 to 2023, with repeated claims and two closed PRs, which is a sign the design is still unsettled. My rubric has no check that looks for that.

**Check rationale**

Quoted from `tools/issue-select/rubric.md`:

| Bounded, actionable scope | Issue body and comment thread. | The issue names a concrete user-visible problem or documentation change and identifies a bounded target (such as a file, component, command, API, or page) or an explicit acceptance criterion. It is not an umbrella, tracking, or unresolved product/design decision. | required |

Why it has this form: the scope family is what sinks first contributions that pass every liveness and claim check. An issue can be in a live repo and unclaimed and still be an umbrella like issue-05 (codebase-wide type annotations) or issue-10 (a self-described megaissue). So the pass condition needs two parts. First, something concrete: a user-visible problem or doc change plus a named target or acceptance criterion. Second, an explicit exclusion of umbrella, tracking and undecided-design issues, because those often carry a `good first issue` label anyway. I made it `required` because a newcomer can't finish an issue with no endpoint, however healthy the repo is.

**Trade-offs**

The check is too strict about "a bounded target". In the 15/20 run it rejected three issues whose gold label is accept: issue-01 ("a multi-file documentation umbrella, not a single bounded target"), issue-04 ("an open-ended list of multiple fixes"), and issue-19 ("two unresolved 'potential causes'... plus three open-ended 'Additional suggestions'"). Each one enumerates its work: named files, named cases, named causes. The grader treated "more than one target" as "umbrella". The check also doesn't look for failed earlier attempts, so it let issue-15 through (see Issue analysis). The trade is that it catches every umbrella in the scope category except issue-15 (`scope 3/4`), at the cost of half the clear-accepts (`clear-accept 4/8`).

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit and time.** I have less than 3 hours a week for this, so I wanted a small issue in a language I already know. #57 is a tier-1 Python bug in one file (`tech_detector.py`), and two failing tests (`test_node_modules_excluded` and `test_build_directory_excluded`) tell me when I'm done. I do want to get better at frontend, and my skill ranked #40 first for that reason. But #40 is estimated at 5–8 hours across the frontend, the API and token expiry, which is more than I can fit right now.

2. **What the verdict got right, and what I weighed.** The skill was right that the repo is active, that a collaborator filed the issue, that there's no open linked PR, and that the scope is bounded with clear tests. What the rubric couldn't weigh was my own schedule. It has no check for effort or time, so it accepted #40 just as readily as #57. Choosing between them came down to my time, not the rubric.

3. **Difficulty claiming it.** Several classmates have already commented on #57 with claims and reproduction reports, and the cause has been discussed in the thread. The house rule says their claims don't block me. The hard part will be making my claim and my reproduction my own work, from my own setup and in my own words, and not just repeating what's already there.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
