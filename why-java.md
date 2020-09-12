---
title: Why Java?
---

From a practical perspective, we use Java because it is cross-platform
and widely used, with a vast array of libraries for handling common
programming tasks. C++ or Python would also have been reasonable
choices, but we chose Java because it is easier than C++ and faster than
Python, with a larger assortment of de facto standard libraries (e.g.,
Swing) than either.

Java is also one of the easiest languages from which to deploy
cross-platform software. In contrast to C++, which has a large number of
complex platform issues to consider, and Python, which leans heavily on
C and C++ for many of its components (e.g., NumPy and SciPy), Java code
is compiled one time into platform-independent byte code, which can be
deployed as is to all supported platforms. And despite this enormous
flexibility, Java manages to provide time performance nearly equal to
C++, often better in the case of I/O operations.

Historically, LOCI's software projects grew around efforts to
harness [VisAD](http://visad.ssec.wisc.edu/), a Java component library
for interactive and collaborative visualization and analysis of
numerical data, within our VisBio application for visualization of
multidimensional microscopy data. We also added support for several
microscopy formats to VisAD before splitting the code into a standalone
library, [Bio-Formats](https://eliceirilab.org/software/bio-formats).
The choice to use Java and VisAD (rather than, e.g., C++ and VTK) was
partially motivated by the fact that LOCI's lead software architect
Curtis Rueden was already an expert on Java and VisAD technologies.
Choosing Java also enabled cross-platform integration
with [ImageJ](https://imagej.net/), one of the most popular freely
available image processing tools in the life sciences field, as well as
with the [OMERO](https://openmicroscopy.org/omero) system for
visualization, management, and annotation of microscopy data.

## Isn't Java too slow?

Java's time performance has been comparable to C++ for many years now,
especially in the realm of file I/O where
[SCIFIO](https://scif.io) is focused. According
to [one thorough study](http://drdobbs.com/cpp/184401976) performed in
2005, "Java often outperforms C++ in operations such as memory
allocation and file I/O while C++ often outperforms Java in arithmetic
and trigonometric operations." This statement is corroborated in [an
earlier study from
2004](http://www.osnews.com/story/5602/Nine_Language_Performance_Round-up_Benchmarking_Math_File_I_O) that
also includes an I/O benchmark: "If we exclude the trigonometry
component, Java performed virtually identically to Visual C++, the
fastest of Microsoft's languages." Some of the theoretical basis for
such results is discussed in [another 2004
article](http://scribblethink.org/Computer/javaCbenchmark.html), which
also finds that "Java performance on numerical code is comparable to
that of C++, with hints that Java's relative performance is continuing
to improve."

We have seen two refutations
([\[1\]](http://www.freewebs.com/godaves/javabench_revisited/), [\[2\]](http://bruscy.republika.pl/pages/przemek/java_not_really_faster_than_cpp.html))
of these figures, but neither includes an I/O benchmark, and according
to their results Java's computational performance is within a factor of
two of C++'s. Even from a pessimistic perspective, we believe the
trade-off is acceptable when considering the other advantages of Java
such as cross-platform deployment, widespread support and ease of
development. As the second article above states: "It seems that it's
much, much easier to create a well performing program in Java. So,
please consider it for a moment before you start recoding your Java
program in C++ just to make it faster."

For further reading, check out this June 2010 article on [popular Java
myths](http://it-tactics.blogspot.com/2010/06/popular-java-myths.html),
and Wikipedia's [comparison of Java performance to other
languages](https://en.wikipedia.org/wiki/Java_performance#Comparison_to_other_languages).

## Isn't Java too complicated?

Compared to Python, sure. But statically typed languages like Java have
enormous potential for just-in-time optimization. Check out the [GraalVM
project](https://www.graalvm.org/). Java combines the benefits of performant
JIT-compiled execution with "write once, run anywhere" source
code&mdash;something other major languages have still not fully achieved.
