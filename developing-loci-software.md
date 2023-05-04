---
title: Developing LOCI Software
---
# Developing LOCI Software

This article is a getting-started guide for programming at
[LOCI](https://eliceirilab.org/).

Aside from merely
[knowing how to write code](https://docs.oracle.com/javase/tutorial/),
every professional programmer should master five key tools of the trade:

1.  Distributed version control system (DVCS; e.g.,
    [Git](http://try.github.io/))
2.  An integrated development environment (IDE; e.g.,
    [Eclipse](http://eclipsetutorial.sourceforge.net/))
3.  Command line tools (e.g., GNU tools,
    [bash](http://www.tldp.org/LDP/abs/html/),
    [vim](http://vim.wikia.com/wiki/Tutorial))
4.  Build systems and dependency management (e.g.,
    [Maven](http://wiki.imagej.net/Maven))
5.  [Debugging](http://ericlippert.com/2014/03/05/how-to-debug-small-programs/)
    and debuggers

Programmers who really understand all of these tools will accomplish
their goals thousands of times faster than those who do not.

If you dabble in programming, you may not have time to dive in to all of
the above, and that is OK! But if your aspiration is to be a
professional programmer, you owe it to yourself to cultivate the [three
great virtues of a programmer](https://thethreevirtues.com/). Computers
exist to automate things. You have better things to do with your life
than tedious, repetitive tasks all day long. Automation is [*always*
superior](https://xkcd.com/1319/). (In all seriousness: [it is often
worth the time](https://xkcd.com/1205/).)

The links above are geared toward learning to develop in Java, but the
skills are ultimately very similar for other software disciplines such
as web programming.

Here are some other important considerations regarding software
development on community projects.

## Open Source

At LOCI, our rule of thumb is to make projects open source. There have
to be good reasons to do otherwise (e.g., secrets included in the code).
As much as we can, we aim to make reusable software and tools at LOCI,
which means both open source and open process. This fosters
collaboration and scientific exchange, as well as interdisciplinary
projects. In short: open source is [good for science](/oss).

## Public Communication

Since most of our software is open source, we collaborate with other
scientists and groups quite often, e.g. via forums and mailing lists. It
is good practice, therefore, to join the [Image.sc
Forum](https://forum.image.sc/) as first action when starting as a
developer at LOCI. We strongly favor all collaboration and communication
take place in public, even if that communication is between two members
of the same physical group. Asking a question, and receiving an answer,
in a private mail benefits no one but yourself, whereas discussing
issues in public informs and benefits the entire worldwide community.

## Semantic Versioning

Reusable software needs to be developed in a reliable manner, with very
few, well-defined moments of backwards-compatibility breaking API
changes (e.g. when a public, protected or package-local Java class,
interface, method or field changes the name or other parts of its
signature). We follow the best practice called [*Semantic
Versioning*](http://semver.org/) for that reason:
version numbers consist of a major version number (reflecting
backwards-incompatible changes), a minor version (reflecting
backwards-compatible API changes) and a micro version (reflecting bug
fixes only).

## Source Code Management: Git

When developing code, even as a single developer, we use Git to manage
source code revisions. It serves us well for the following purposes:

- documentation
- fixing regressions
- backup

We commit and push our changes at least daily, to avoid losing work. See
the [SCM history](https://imagej.net/Coding_style#SCM_history) section
of the ImageJ coding style guide for further details, and the ImageJ
site's [Using Git](https://imagej.net/Git_Notes) page for tips and
tricks.

In particular, we aim to inspect all changes before committing and we
aim to never leave uncommitted changes at the end of a work day.

To inspect the changes before committing, simply call `git diff`, or use
[GitHub Desktop](https://desktop.github.com/). It is a good idea to
separate pure style fixes (such as indentation fixes, or spelling
corrections) from functional changes: it is too easy for bugs to hide in
overly complex commits otherwise.

To avoid leaving changes uncommitted at the end of a work day, the
recommended practice is to create a temporary branch and commit
everything as a *WIP* ("work in progress") commit:
```
git checkout -b tmp &&
git add -A . &&
git commit -m wip &&
git push origin HEAD
```
The next day, the changes can be transported back into the `master`
branch by calling
```
git fetch origin tmp &&
git cherry-pick -n tmp &&
git push origin :tmp
```
If the Git commands given in this paragraph seem too complex, or if it
is not obvious to you how they work, or what they do, it is probably a
good idea to go through GitHub's [15-minute tutorial on
Git](https://try.github.com/) right now.

### Meaningful commit messages

It is a good practice to look at the diff before committing. By doing
that, one can avoid conflating different changes into a single commit,
e.g. have one crucial bugfix hidden between a thousand lines that only
fix the formatting.

We strive to document the intention and the motivation behind patches in
[good commit
messages](http://chris.beams.io/posts/git-commit/).
In general, it is safe to assume that the reader does not want to read
anything that can be deduced easily from looking at the diff. Instead,
we aim to provide the background information, such as: who reported a
bug? who had the cunning idea to solve it that way? which way seemed to
be obvious, but did not work, and why? It is probably a good idea to
read a little in the commit messages of our projects to get a good feel
how to do things.

### Topic branches

While bug fixes are usually contained in individual commits that are
directly pushed to the *master* branch once tested and verified, more
involved features are developed on topic branches. We [rewrite
history](http://git-scm.com/book/en/Git-Tools-Rewriting-History#Splitting-a-Commit)
in such topic branches extensively before merging, in order to provide a
clean and readable commit history that serves both as documentation to
others as well as the developers themselves (after six months, it is
hard to remember details).

### GitHub (repositories, issues, pull requests)

All of our open source code lives in [Git repositories on the social
coding site GitHub](https://github.com/uw-loci). GitHub offers a vast
array of useful resources, such as [a 15-minute tutorial on
Git](https://try.github.com/), tools for
[collaborating](https://help.github.com/articles/be-social), extensive
[guides](https://guides.github.com/), issue trackers, websites, and
wikis. It even offers a [desktop
application](https://desktop.github.com/) for Windows and macOS, making
it a breeze to work with GitHub.

## Unit Testing (JUnit)

Every reliable and reusable project needs to ensure that the basic
functionality on which third-party software might rely works, and keeps
working. The easiest way to guarantee that is to write unit tests. For
Java projects, we use JUnit—unless the project already uses another unit
testing framework.

It is essential that unit tests run quickly. It is okay to take up to a
few seconds if it is really crucial to test this particular
functionality, but it is not okay to take minutes. The longer a test
takes, the less often it is executed, diminishing its value greatly. In
most cases, long-running cases can easily be reduced in size without
reducing the number of exercised code paths.

For example, when testing an image processing filter, it is typically
enough to test it on a 16x16 image auto-generated from a two-dimensional
sine wave and then verifying the output values only at, say, 8 different
locations against known-good values. There is no need to use a
full-blown 1024x1024 image obtained using a confocal microscope. Using
auto-generated data for testing also avoids the repository bloat typical
of projects adding test data into the same repository as code (the test
data can easily occupy 100-10,000x as much space as the code itself in
such cases).

A good tutorial how to write unit tests can be found
[here](http://www.vogella.com/tutorials/JUnit/article.html), a good
example to follow is the
[FileUtilsTest](https://github.com/scijava/scijava-common/blob/master/src/test/java/org/scijava/util/FileUtilsTest.java)
of SciJava Common.

## Continuous Integration (CI)

Unit tests are useful only if they are run. When developing, it is
all-too-easy to forget, and regressions sometimes result from updates in
dependencies. Therefore, we use [Travis CI](https://travis-ci.com/)
(a so-called "continuous integration" server, or "robot butler" of
sorts), whose duty it is to monitor source code repositories, and to
compile and run the tests whenever anything changes. Travis CI, being
public, is therefore a very good health report of our projects.

## Dependency Management (Maven)

When developing Java projects, we rely on
[Maven](https://maven.apache.org/) for dependency management.
This is particularly important due to our focus on *reusable* software:
proper versioning and easy upgrading to newer versions is essential due
to the highly modular nature of our projects. The benefit is obvious: we
maintain hundreds of software components with only three full-time
developers.

Making a project Maven-compatible is very easy: put the source code into
`src/main/java/`, additional resources required by the source code into
`src/main/resources/`, create the `pom.xml` file containing the project
description, and you're good to go. Unit tests should not be bundled in
the resulting *.jar* file (called "artifact" by Maven), hence there is a
different location for the tests' source code (`src/test/java/`) and the
tests' resources (`src/test/resources/`).

A good example to follow is the [minimal ImageJ 1.x plugin
project](https://github.com/imagej/example-legacy-plugin).

## Use Eclipse unless you are a NetBeans/IntelliJ expert

Java projects should be developed with the help of an Integrated
Development Environment. It not only helps with setting up a project
ready to be developed, but also helps with powerful tools such as code
completion, convenient Javadoc display, and refactoring.

At LOCI, we use Eclipse extensively. Therefore it makes most sense to
use Eclipse for developing Java code, unless you are already a real
maven in another Maven-compatible tool like NetBeans or IntelliJ.

We also work on the command line extensively and use editors like vim
and emacs. Such tools complement IDEs very well, but cannot fully
replace them in terms of productivity.

## Setting up your system (macOS)

To set up your system, you will:

- Install [Homebrew](https://brew.sh/)
- Open a Terminal
- Install Git: `brew install git`
- Install Maven: `brew install maven`
- Install mr: `brew install myrepos`

Then you can start cloning source code:

```
mkdir -p code/ctrueden
cd code/ctrueden
git clone git://github.com/ctrueden/dotfiles
cd dotfiles
sh setup.sh
```

And now your shell is 'supercharged'.  This sets up all kinds of
goodies: bash updates, zsh updates, better vim stuff, etc.

Then you can get ALL the code:

```
cd ~/code
mr up
```

And it will start cloning up a storm.

Then, again:

```
mr up
```
