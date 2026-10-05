# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I am an AI301 student with experience using Python and VS Code, and I am
learning how to contribute to open-source projects. I write in a friendly,
direct way and make it clear what I tested, what I observed, and what I still
do not know.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Say what I actually did

I describe completed actions and observed results, not work I only intend to
do or conclusions I have not verified.

- Wrong: "I confirmed this bug and know what is causing it."
- Right: "I followed the reported steps and reproduced the error; I have not identified the cause yet."

### Rule: Name the evidence

I include the important environment detail and the exact result instead of
calling something broken or fixed without support.

- Wrong: "It does not work for me either."
- Right: "On Python 3.14.3 on macOS, the command exits with `ValueError` after the second step."

### Rule: Be friendly without using filler

I thank maintainers when it is meaningful, but I keep the comment focused on
the issue and avoid generic enthusiasm that hides the useful information.

- Wrong: "Hi! This is an awesome project and I would absolutely love to work on this amazing issue!"
- Right: "Hi, I would like to reproduce this issue and report the environment, steps, and output I observe."

### Rule: Do not promise a deadline

I state my next action without guaranteeing when I will finish or implying
that maintainers must reserve the issue for me.

- Wrong: "I will have this reproduced and fixed by tomorrow."
- Right: "I plan to test the reported steps and will post the results when the reproduction is complete."

### Rule: Mark uncertainty plainly

I separate what the evidence proves from what I suspect so readers do not
have to guess which statements are confirmed.

- Wrong: "The dependency update definitely caused this."
- Right: "The failure appears after the dependency update, but I have not yet isolated it as the cause."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- Deadlines or completion promises I cannot guarantee.
- Claims that I reproduced, fixed, or diagnosed something without evidence.
- Demands that an issue be assigned to me or held for me.
- Vague comments such as "same issue" without environment and result details.
- Logs, screenshots, or generated text that I did not review for accuracy or
  sensitive information.
