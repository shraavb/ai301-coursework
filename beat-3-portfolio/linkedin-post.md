<!-- DRAFT: reword in your own words, then delete this comment. -->
I fixed a bug in an open source software tool, and the fix was accepted and merged by the project's maintainers. The tool lets people build software from the command line, and it silently ignored a typo instead of warning the user.

Here is what went wrong. If you asked the tool for an output format it didn't recognize, it didn't say so. It carried on and produced a different format, so a script using it would break later, far from the real cause. Now the tool stops right away and lists the formats it does support.

I wrote tests to prove the bug existed, checked that they failed before my change and passed after it, and opened a pull request, which is a proposed change that the maintainers review. A maintainer approved it and merged it two days later.

What I learned about how I work:
- I should reproduce a bug on a clean copy of the code before I change anything.
- I should say plainly what I couldn't get working, rather than leave it out.
- I should read a project's contribution rules before every submission. I missed one, and the reviewer caught it.

https://github.com/dsx-ai-factory/nvkr/pull/442

#opensource #softwareengineering
