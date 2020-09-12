---
title: Isn't Java too slow?
---
# Isn't Java too slow?

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
