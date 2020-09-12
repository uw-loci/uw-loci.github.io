---
title: Interfacing From Non-Java Code
---
> NB: This article was written circa 2009. While it has been lightly
> updated since, much of the information here is now quite outdated.
> Nonetheless, the core distinction between in-process and interprocess
> remains useful, even if some of the examples highlighted are not
> up-to-date anymore.

# Interfacing From Non-Java Code

Software written in Java is easiest to use with other Java code.
However, it is possible to call Java code from a program written in
another language. But how to do so depends on your program's needs.

Technologically, there are two broad categories of solutions:
**in-process** approaches, and **interprocess** communication. See
below for discussion and examples of each.

For further reading, check out [Codemesh's technology
comparison](http://codemesh.com/technology.html).

## In-process approaches

With an [in-process](http://codemesh.com/in_process.html) approach, your
application directly invokes Java code, either by spawning its own
internal Java Virtual Machine (JVM) and passing data across a bridge, or
otherwise executing the Java code within a single environment.

<table data-border="0">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Strengths</th>
<th>Weaknesses</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><ol>
<li>Tight (API-level) integration</li>
<li>Minimal performance overhead</li>
<li>Few security considerations</li>
</ol></td>
<td><ol>
<li>No shared state between processes</li>
<li>Limited portability</li>
</ol></td>
</tr>
</tbody>
</table>

**Paradigms:**

-   *JNI* – The most common paradigm is the [Java Native
    Interface](http://en.wikipedia.org/wiki/Java_Native_Interface), an
    API for interfacing Java programs with
    [native](http://en.wikipedia.org/wiki/Native_(computing)) C/C++
    code. Functionality exists to 1) call C/C++ methods from Java,
    and 2) spawn a Java Virtual Machine and execute Java instructions
    from C/C++. The latter direction, known as [Java
    Invocation](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/invocation.html),
    is the relevant one for calling Java code from C++. Because using
    the JNI directly involves writing a lot of tedious glue code,
    several projects have emerged for autogenerating such code. *See
    below: raw JNI, Jace, JunC++ion, JuggerNET*
-   *Compilers *– The dominant paradigm in Java is to compile Java
    source to Java bytecode, then execute the bytecode in a Java
    interpreter. However, an alternative is to compile the Java source
    directly to native code so that it can link with other native
    programs. Such an approach requires that the compiler provide
    correct support for all necessary Java standard library features. It
    may also exhibit much different performance (for better or for
    worse) than Sun's Java implementation does. *See below: GCJ*
-   *Runtimes *– The safest way to guarantee correct program behavior is
    to execute Java bytecode using the Java interpreter(s) with which it
    has been tested. However, a Java runtime written using a specific
    framework (e.g., .NET) could enable seamless integration with other
    (non-Java) programs within the same framework. Like the
    compiler-based paradigm above, though, it is reliant on the
    correctness, completeness and performance of the Java implementation
    in question. *See below: IKVM.NET*

## Interprocess communication

The other approach is [interprocess
communication](http://en.wikipedia.org/wiki/Inter-process_communication),
a broad collection of techniques for exchanging data between multiple
running programs. Such techniques take many forms in computing; one
ubiquitous example is web browsers and web servers. Most solutions in
this category are some form of
[middleware](http://en.wikipedia.org/wiki/Middleware).

<table data-border="0">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Strengths</th>
<th>Weaknesses</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><ol>
<li>Share state between multiple processes on multiple machines</li>
<li>Broad portability and language support</li>
</ol></td>
<td><ol>
<li><a href="http://en.wikipedia.org/wiki/Marshalling_%28computer_science%29">Object marshalling</a> incurs significant overhead</li>
<li>Potentially vulnerable to security exploits</li>
</ol></td>
</tr>
</tbody>
</table>

**Paradigms:**

-   *Local communication* – Modern operating systems provide several
    ways to share information between processes, including shared
    memory, file system access, and passing data back and forth with
    [standard input and output
    streams](http://en.wikipedia.org/wiki/Standard_streams). *See below:
    pipes, files*
-   *Messaging* – Networking technology allows a process on one computer
    to send and receive messages from another process on a different
    machine. The
    [client-server model](http://en.wikipedia.org/wiki/Client%E2%80%93server_model)
    is probably most applicable for Java/native integration, with the
    Java portion acting as a server that can be queried from the native
    code. *See below: sockets, XML-RPC*
-   *ORB* – An [object request
    broker](http://en.wikipedia.org/wiki/Object_request_broker) (ORB) is
    a high-level form of middleware for transferring objects between
    multiple running programs. ORBs provide an abstraction that can
    reduce and simplify code written by providing access to a wealth of
    higher-level messaging features. *See below: Ice, CORBA, Codemesh
    Shared JVM*

List of solutions
-----------------

It is a significant challenge to access a complex Java API from code
written in another language, especially in a cross-platform and high
performance way. The table below provides an overview of viable
approaches, with links to source code and instructions where
appropriate. Which approach to use depends on your application's target
platforms and languages, and the interaction model between your
application and the Java code—see the discussion of in-process solutions
versus interprocess communication above for details.

**A note about SWIG.** The [Simplified Wrapper and Interface
Generator](http://www.swig.org/) (SWIG) is an excellent tool for
exposing C++ functionality to higher level languages such as Java.
Unfortunately, calling native code from Java is the wrong direction for
our purposes. However, when combined with an integration solution
specific to C++, SWIG could be used to extend that solution into other
languages (see [SWIG's list of supported
languages](http://www.swig.org/compat.html#SupportedLanguages) for a
complete list).

<table data-border="0">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Solution</th>
<th>Type</th>
<th>Languages</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/invocation.html">Raw JNI</a></td>
<td>In-process (JNI)</td>
<td>C/C++</td>
<td><ul>
<li>You can code your integration layer using pure JNI calls (but we don't recommend it).</li>
<li>Low-level JNI offers full control over the interface between Java and native code.</li>
<li>Raw JNI solutions are <a href="http://codemesh.com/technology.html#jni">time-consuming and error-prone</a> to implement.</li>
<li>We have coded <a href="https://github.com/openmicroscopy/bioformats/blob/develop/components/bio-formats/utils/showinfJNI.cpp">a simple example</a> for calling the <a href="https://loci.wisc.edu/software/bio-formats">Bio-Formats</a> library in this way.</li>
<li>We recommend a higher level integration solution such as Jace instead.</li>
</ul></td>
</tr>
<tr class="even">
<td><a href="http://sourceforge.net/projects/jace/">Jace</a></td>
<td>In-process (JNI)</td>
<td>C/C++</td>
<td><ul>
<li>Jace generates C++ proxy classes, one per Java class, mirroring the original functionality as much as possible.</li>
<li>The C++ proxy classes use JNI under the hood but handle most of the usual JNI pitfalls.</li>
<li>We provide a set of <a href="http://www.openmicroscopy.org/site/support/bio-formats/developers/c-bindings.html">Bio-Formats C++ bindings</a> using Jace.</li>
</ul></td>
</tr>
<tr class="odd">
<td><a href="http://codemesh.com/products/junction/">JunC++ion</a></td>
<td>In-process (JNI)</td>
<td>C/C++</td>
<td><ul>
<li>JunC++ion is a commercial in-process integration solution for C/C++ available from <a href="http://www.codemesh.com/">Codemesh, Inc.</a></li>
</ul></td>
</tr>
<tr class="even">
<td><a href="http://codemesh.com/products/juggernet/">JuggerNET</a></td>
<td>In-process (JNI)</td>
<td>.NET</td>
<td><ul>
<li>JuggerNET is a commercial in-process integration solution for .NET available from <a href="http://www.codemesh.com/">Codemesh, Inc.</a></li>
</ul></td>
</tr>
<tr class="odd">
<td><a href="http://gcc.gnu.org/java/">GCJ</a></td>
<td>In-process (compiler)</td>
<td>C/C++ (GCC only)</td>
<td><ul>
<li>GCJ can compile Java code into machine code.</li>
<li>Instead of JNI, GCJ uses its <a href="http://gcc.gnu.org/java/papers/cni/t1.html">Compiler Native Interface</a> (CNI) to enable access to Java code from C++.</li>
<li>Works with GCC only (i.e., not with Microsoft Visual C++ or other compilers).</li>
<li>Correctness and performance is dependent on the compiler implementation.</li>
</ul></td>
</tr>
<tr class="even">
<td><a href="http://www.ikvm.net/">IKVM.NET</a></td>
<td>In-process (runtime)</td>
<td>.NET/Mono</td>
<td><ul>
<li>IKVM.NET interprets Java byte code on the fly into the .NET/Mono framework.</li>
<li>Interoperability is limited to applications in the .NET/Mono framework.</li>
<li>Correctness and performance is dependent on the runtime implementation.</li>
</ul></td>
</tr>
<tr class="odd">
<td><a href="http://en.wikipedia.org/wiki/Pipeline_%28Unix%29">Pipes</a></td>
<td>Inter-process (local)</td>
<td>Any</td>
<td><ul>
<li>Pipes can work well when quantity of data being transferred is limited.</li>
<li>Be careful about blocking operations causing deadlock.</li>
<li>The <a href="http://www.openmicroscopy.org/site/support/bio-formats/users/ome-server/#source-code">OME Perl server</a> uses a combination of pipes and files to interface with the <a href="https://loci.wisc.edu/software/bio-formats">Bio-Formats</a> library.</li>
</ul></td>
</tr>
<tr class="even">
<td><a href="http://en.wikipedia.org/wiki/Computer_file">Files</a></td>
<td>Inter-process (local)</td>
<td>Any</td>
<td><ul>
<li>Communication via files is slower than pipes, since messages go through disk.</li>
<li>The size of the communication buffer is limited only by available disk space.</li>
<li>The <a href="http://www.openmicroscopy.org/site/support/bio-formats/users/ome-server/#source-code">OME Perl server</a> uses a combination of pipes and files to interface with the <a href="https://loci.wisc.edu/software/bio-formats">Bio-Formats</a> library.</li>
</ul></td>
</tr>
<tr class="odd">
<td><a href="http://en.wikipedia.org/wiki/Unix_domain_socket">Sockets</a></td>
<td>Inter-process (messaging)</td>
<td>Any</td>
<td><ul>
<li>You can use a sockets API directly to create a custom solution (but we don't recommend it).</li>
<li><a href="https://loci.wisc.edu/software/jvmlink">JVMLink</a> was our first cut at such a solution, before we realized that we were essentially inventing our own middleware.</li>
<li>We recommend using Ice or CORBA instead.</li>
</ul></td>
</tr>
<tr class="even">
<td><a href="http://en.wikipedia.org/wiki/XML-RPC">XML-RPC</a></td>
<td>Inter-process (messaging)</td>
<td><a href="http://en.wikipedia.org/wiki/XML-RPC#Implementations">Many</a></td>
<td><ul>
<li>XML-RPC is a cross-platform <a href="http://en.wikipedia.org/wiki/Remote_procedure_call">remote procedure call</a> (RPC) technology using XML.</li>
<li>XML-RPC is a human readable, but inefficient, means of transporting information.</li>
<li>See also: <a href="http://en.wikipedia.org/wiki/SOAP">SOAP</a></li>
</ul></td>
</tr>
<tr class="odd">
<td><a href="http://zeroc.com/ice.html">Ice</a></td>
<td>Inter-process (ORB)</td>
<td>Several</td>
<td><ul>
<li>Ice is high-performance middleware for cross-platform client/server communication.</li>
<li><a href="http://www.zeroc.com/">ZeroC</a> argues that Ice is <a href="http://www.zeroc.com/iceVsCorba.html">superior to CORBA</a>.</li>
<li>The <a href="http://www.openmicroscopy.org/site/support/omero4/">OMERO server</a> uses Ice extensively to provide client/server interoperability cross-language.</li>
</ul></td>
</tr>
<tr class="even">
<td><a href="http://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture">CORBA</a></td>
<td>Inter-process (ORB)</td>
<td>Many</td>
<td><ul>
<li><a href="http://en.wikipedia.org/wiki/RMI-IIOP">Java RMI over IIOP</a> enables communication between Java and other CORBA-supported languages.</li>
<li>CORBA is a mature technology with widespread use in the enterprise community.</li>
<li>Support for CORBA is built in to the Java 2 platform.</li>
<li>Nonetheless, where possible we suggest you use Ice instead.</li>
</ul></td>
</tr>
<tr class="odd">
<td><a href="http://codemesh.com/shared_jvm.html">Codemesh Shared JVM</a></td>
<td>Inter-process (ORB)</td>
<td>C++, .NET</td>
<td><ul>
<li>The Codemesh Shared JVM is a commercial interprocess integration solution available from <a href="http://www.codemesh.com/">Codemesh, Inc.</a></li>
</ul></td>
</tr>
</tbody>
</table>

Know a great integration solution that we missed? [Let us
know](https://loci.wisc.edu/software/mailing-lists)!
