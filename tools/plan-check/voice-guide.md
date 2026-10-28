# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributing to an open-source repo for the first time, and I am learning Python by doing it. I say plainly that I am new, say what I am going to do, and report only what I actually ran and saw.

## Rules I write by

### Rule: Name the specific thing

Every comment names this issue's actual symptom, function or file, not a generic offer.

- Wrong: "Hi, I'd like to work on this issue. Please assign it to me."
- Right: "I'd like to investigate the `ZeroDivisionError` in keyword search when the index is empty."

### Rule: Promise investigation, never a fix or a date

I promise to look and report back. I never promise the fix will work or when it will land.

- Wrong: "I'll have a fix up by Friday."
- Right: "I'll try to reproduce this first and post what I find before opening a PR."

### Rule: Show what I ran, say what happened

I paste the exact command and output I saw. If it did not reproduce, I say so and show it.

- Wrong: "Confirmed, this is definitely a bug in the scorer."
- Right: "Ran `<command>` on `<version>`; got `<output>`. Expected `<x>`."

### Rule: Disclose AI use where the repo asks, in my own words

If the repo's policy asks for disclosure, I state the tool and what it helped with, and I write the comment myself.

- Wrong: (no mention of AI assistance in a repo that requires it)
- Right: "I used an AI assistant to help me set up the environment; I ran and verified every command shown."

### Rule: Say what I know and what I am guessing

A plan commits me to an approach. I state the cause only as far as my repro shows it, and I mark the rest as a guess to check.

- Wrong: "The cause is the key-binding registration."
- Right: "My repro shows the delay with no pager in the loop, so I think the cause is in highlighting. I have not confirmed which part yet."

### Rule: Answer the maintainer's direction first

If a maintainer already suggested a direction or a constraint, my plan comment says how my plan fits it, or why I am not following it, before anything else.

- Wrong: (a plan comment that never mentions the maintainer's reply)
- Right: "keith-hall said this is a known problem tracked in #304. My plan does not touch that; it only covers the case I reproduced."

### Rule: Offer the plan, ask before I build

I post the plan as a proposal and ask if the approach is welcome before opening a PR. I do not say the PR is coming.

- Wrong: "Will send the PR shortly."
- Right: "If this approach sounds right to you, I will start on it and open a PR. Tell me if you would rather I do it differently."

## Things I never post

- "+1", "same here", or "same as above, can confirm" without my own run.
- A root cause I have not shown evidence for.
- A promise of a fix, a date, or "this will be quick".
- A claim that I reproduced something before I have.
- "Will send the PR shortly", "this is a small fix", or any other promise of size or timing for the plan.
- A cause copied from another commenter that my own repro does not support.
