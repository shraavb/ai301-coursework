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

## Things I never post

- "+1", "same here", or "same as above, can confirm" without my own run.
- A root cause I have not shown evidence for.
- A promise of a fix, a date, or "this will be quick".
- A claim that I reproduced something before I have.
