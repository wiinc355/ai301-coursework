This fall I opened a fix for Simpler.Grants.gov, the federal grants platform, where a form with no instructions was labeled "Unavailable" — wording that reads like an error to someone applying for a grant. I changed it to say "Not Applicable," added tests proving the fix works in both of the affected tables, and submitted the change as a pull request for the maintainers to review.

What I'm most proud of isn't the fix itself — it's how it got there. I built my own quality checks for this project: one that scores whether an issue is worth taking on, one that blocks my work from going out until five local checks pass, and one that tracks my pull request so I follow up at the right time, not too early. Those checks caught my real mistakes. One stopped me because I was working on the wrong branch. Another rejected my first draft because I hadn't named a reviewer or disclosed an open design question. A third miscalculated a follow-up date by one day, and after I added a verification rule, its next run got it right.

The pull request is still open and waiting on the team's design decision, so I'm saying "opened a fix," not "fixed." But every claim in it is backed by a passing test, and I know exactly when I'll check back in.

The work: https://github.com/HHS/simpler-grants-gov/pull/13214

#OpenSource #SoftwareEngineering #CivicTech
