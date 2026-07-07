---
title: Developing LOCI Software
---
# Developing LOCI Software

This article is a getting-started guide for programming at [LOCI](https://loci.wisc.edu/).

Software development is more than just writing code; every programmer should familiarize themselves with key concepts including:

1. **Version control:** [Git](http://try.github.io/).
2. **Build systems and dependency management:** [Maven](http://wiki.imagej.net/Maven) for Java and [uv](https://docs.astral.sh/uv/getting-started/first-steps/) for Python.
3. **Integrated Development Environments (IDE):** [Visual Studio Code](https://code.visualstudio.com/docs/introvideos/basics).
4. **Debugging methods:** [Debugging in VS Code](https://code.visualstudio.com/docs/debugtest/debugging) and [pdb](https://realpython.com/python-debugging-pdb/).
5. **Command line tools:** [bash](http://www.tldp.org/LDP/abs/html/) and [vim](http://vim.wikia.com/wiki/Tutorial).

## Open Source

At LOCI, we create open-source software (OSS) with a [permissive license](https://opensource.org/licenses). A permissive open-source license is essential for open science, promoting [FAIR](https://www.go-fair.org/fair-principles/) principles, ensuring our work remains maintainable, accessible, and reusable by the broader research community.

## Guiding Principles

Although programmers create code, that code is also a liability and a burden, as it includes a lasting commitment to maintenance. Below are some of the programming principles and patterns we have found useful in creating maintainable code. Over time, you will certainly build your own intuition about what works for you!

* Keeping code [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (Don't Repeat Yourself).
* Identifying and avoiding [anti-patterns](https://en.wikipedia.org/wiki/Anti-pattern).
* Striving for [TDD](https://en.wikipedia.org/wiki/Test-driven_development) (Test-Driven Development).
* Understanding programming paradigms, such as:
    * [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) (Object-Oriented Programming)
    * [FP](https://en.wikipedia.org/wiki/Functional_programming) (Functional Programming)

## Public Communication

Part of developing open-source software includes working in public and actively interacting with the community (other developers and users). At LOCI, we have three general channels for communication:

* **Project planning:** [GitHub](https://github.com/uw-loci/)
* **Developer chat:** [imagesc.zulipchat.com](https://imagesc.zulipchat.com/)
* **Announcements and user feedback:** [forum.image.sc](https://forum.image.sc/)

## Project Management

Software development is not just about creating code - it's also deciding the who, what, when, and even *if* code is necessary. Then there's the matter of tracking status and keeping collaborators up-to-date. All of these topics fall under the "project management" umbrella.

Project management can be challenging: it is understandable and okay to struggle with it at first. It is typically not taught as part of computer science coursework, and so must be learned through experience. Tasks like creating and updating issues also can feel like a barrier to doing "actual work" - why spend time on things that aren't coding?

Done well, healthy project management practices should actually make your life easier. Providing a clear expectation of what needs attention focuses effort, and is essential if more than one person is working on a given project to avoid collision or duplication of effort.

Because our source code is stored on GitHub, the most common project management tools you will encounter are:
- [GitHub Issues](https://github.com/features/issues) to describe, discuss, and assign tasks.
- [Project boards](https://docs.github.com/en/issues/planning-and-tracking-with-projects) to manage collections of related Issues.
- [Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests) to review and finalize effort.

### Effective GitHub Issue Use

Writing code doesn't happen in a vacuum - more precisely, a software developer translates concepts to functional implementations. Issues provide a space to flesh out and discuss those concepts, and thus are a core driver of our work.

Issues often fall into one of two broad categories: new **features**, and **bug reports**. In either case, the fundamental needs are similar:
- A clear description of the underlying concept
- The proposed solution (potentially including why this solution is selected from a number of possibilities)
- The "completion criteria": what will be the milestone(s) when this issue is complete?
- In the case of bug reports, *steps to reproduce* are also essential (including environment setup)

As work is done on an issue, it can be updated with comments providing status updates, asking questions, describing new problems, or even proposing implementation shifts. This provides a helpful narrative of the work, to complement the developer-focused git history.

### Pull Request Best Practices

Pull requests are the complement to Issues: the two bookend your work, providing at-a-glance summaries describing what to do (the issue) and what was done (the pull request). Both are used in conjunction to gather feedback, review, and validation from other (often senior) developers.

Pull requests are most useful when used when *you believe your work is complete* and ready for external testing and verification; however, you can also create a [draft pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests#draft-pull-requests) as a place to provide meta-information like build and testing instructions for a branch in need of support.

Ideally, the Issue driving your branch should implicitly tell you when you are ready to open a pull request. The clearest indicators you can provide that the "completion criteria" have been satisfied include **unit tests** that prove the desired function or bug fix is complete, or scripts demonstrating a *user's perspective* on the new capability.

Pull Requests are typically lightly populated compared to issues, but there is still some essential information to include:
- What steps should a reviewer take to test the associated branch?
- Are there particular areas to highlight for feedback?

### Example LOCI Developer Workflow

Puting everything together, here is an example of how you might expect the development loop to progress at LOCI:

1. Select a GitHub issue to start working on (e.g., the highest priority issue assigned to you in a project).
2. Clone the associated repository.
3. Open the code in your development environment.
4. Create a new branch dedicated to your work.
5. Make necessary file changes.
6. Ensure existing tests pass and write new tests, as appropriate.
7. Record your changes with Git, following commit message [best practices](https://www.simplethread.com/what-makes-a-good-git-commit/).
8. Push your commits back to your repository:
    * **Always** ensure you push your commits at the end of the day.
    * If the work is incomplete, prefix your commit message with `WIP:` (Work In Progress).
9. When the work is complete, open a Pull Request (PR) to merge your changes back into `main`.
    * **Note** - if your branch has `WIP` commits on it, it should [be rewritten](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) to remove them first!
10. Once the Pull Request is merged, [close the original issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/administering-issues/closing-an-issue).

## LLMs at LOCI

Large Language Models (LLMs) have become powerful assistants for software development, but like any tool, they require deliberate guidance. Below are the available resources and our core expectations for using LLMs at LOCI.

### Existing Resources

* [UW-Madison Generative AI Services](https://it.wisc.edu/generative-ai-services-uw-madison/). This includes links to all the LLM services that are freely accessible with your wisc account.
* [GitHub Copilot for Students/Educators](https://docs.github.com/en/copilot/how-tos/copilot-on-github/set-up-copilot/enable-copilot/set-up-for-students) (Free access for students)
* **Local LLMs (for running models privately on your machine):**
    * [Ollama](https://ollama.com/)
    * [LM Studio](https://lmstudio.ai/)
* **Free online LLMs (no sign-up required):**
    * [DuckDuckGo AI Chat](https://duck.ai/)
    
### Usage Guidelines

LLMs can generate code easily and prolifically, but remember: **code is a liability**. Our goal is to build *maintainable* software; the goal isn't just *production*, but creating tools that are as easy as possible to understand and edit in the future. Before opening a PR, ask yourself: *"When a human looks at this in 5 years, will my intent be clear?"*

#### Effective Strategies
* **Jump-starting ideas:** LLMs are fantastic for sketching out prototypes, brainstorming architectures, or overcoming "blank page" syndrome.
* **Rubber ducking:** You can use LLMs as interactive [rubber ducks](https://en.wikipedia.org/wiki/Rubber_duck_debugging) to talk through complex problem logic. [MCP](https://modelcontextprotocol.io/docs/getting-started/intro) utilization can allow you to chat with an LLM while it connects with your tools in real time, providing a "consumer" perspective that is incredibly valuable.
* **Learning high-level concepts:** They excel at explaining complex mathematical concepts, data structures, or unfamiliar APIs. Treat them as a highly customizable personal tutor.
* **Context-driven development:** LLMs benefit immensely from explicit constraints and clear codebase context. Utilizing agentic tools or IDE integrations that have direct context of your workspace is one of the most effective ways to create a tight, productive development loop. If you're not getting the output you expect or desire, consider how you phrased your prompt and what could be more specific.
* **Bootstrapping tests:** Writing tests is a tough discipline to build. LLMs can help generate test boilerplate—just be incredibly **wary of poor-quality tests** (e.g., tests that pass because they are developed based on the source's output, instead of the conceptual correctness).

#### What to Avoid
* **LLMs as a proxy for human interaction:** LLMs can assist in writing e-mails and comments, but are not a substitute for your own investment and synthesis. Communication builds comprehension and engagement, and if a developer wants an LLM's opinion they could always generate one themselves. 
* **Blind trust:** LLMs are statistical constructs, not truth engines. Be deeply skeptical of their solutions; they are highly capable of producing flawless-looking code that contains catastrophic logic errors. Just because it compiles and runs does not mean the results are correct.
* **Skipping the conceptual work:** LLMs are not a substitute for your own understanding. If you don't know what the generated code is doing, or if it actually accomplishes your goals, you shouldn't commit it. A Pull Request should never be the first time a human actually reads, or runs, the code. Take the time to audit and test your LLM's output.
* **Using LLMs as a debugging crutch:** LLMs are great at catching common code mistakes, but reasoning about *runtime state* requires interactive debugging techniques. 
