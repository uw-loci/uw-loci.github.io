---
title: Developing LOCI Software
---
# Developing LOCI Software

This article is a getting-started guide for programming at
[LOCI](https://loci.wisc.edu/).

Software development is more than just writing code, every programmer should familiarize themselves with
five key concepts of the trade:

1.  Version control: [Git](http://try.github.io/).
1.  Build systems and dependency management: [Maven](http://wiki.imagej.net/Maven) for Java and [uv](https://docs.astral.sh/uv/getting-started/first-steps/) for Python.
1.  Integrated Development Environment (IDE): [Visual Studio code](https://code.visualstudio.com/docs/introvideos/basics).
1.  Debugging tools: [Visual Studio code debugger](https://code.visualstudio.com/docs/debugtest/debugging) and [pdb](https://realpython.com/python-debugging-pdb/).
1.  Command line tools: [bash](http://www.tldp.org/LDP/abs/html/) and [vim](http://vim.wikia.com/wiki/Tutorial).

## Guiding principles

Although a programmer creates code, the code is a liability and in the case of open source software includes a commitment to maintenance.
Below are some of the programming principles and patterns we have found useful over the years. Over time will certainly build your
own intuition about what works for you!

- Writing [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) code.
- Identifying and avoiding [anti-patterns](https://en.wikipedia.org/wiki/Anti-pattern).
- Striving for [TDD](https://en.wikipedia.org/wiki/Test-driven_development).
- Understanding programming paradigms:
    - [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming)
    - [FP](https://en.wikipedia.org/wiki/Functional_programming)

## Open Source

At LOCI all of our projects must be open source with a [permissive license](https://opensource.org/licenses). For scientific purposes
permissive open source license promotes [FAIR](https://www.go-fair.org/fair-principles/) principles.

## Public Communication

Part of developing OSS includes developing in the public and interacting with the community (*i.e. other developers and users). At LOCI
we communicate primarily on three channels:

- Source code: [GitHub](https://github.com/uw-loci/)
- Developer chat: [imagesc.zulipchat.com](https://imagesc.zulipchat.com/)
- Community forum: [forum.image.sc](https://forum.image.sc/)


## An example LOCI developer workflow

Here's an example of how we develop at LOCI using Git and other tools.

1. Start from a GitHub issue describing the work to be done (*e.g.* an issue or feature request).
1. Clone the associated repo(s).
1. Open the code in your development environment.
1. Create a new branch for your work.
1. Make your code changes.
1. Make sure tests pass and/or write new tests.
1. Record your changes with `git` using message [best practices](https://www.simplethread.com/what-makes-a-good-git-commit/).
1. Push your commits back to your repo:
    - Always ensure you push your commits at the end of the day.
    - If the work isn't complete, prefix your message with `WIP`.
1. When the work is complete merge your changes back into `main` with a pull request.
1. When the pull request is merged, [close the original issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/administering-issues/closing-an-issue).
