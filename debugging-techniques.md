---
title: Debugging Techniques
---
It is purported that computer programming pioneer Maurice Wilkes
described his own experience of the 1940s:

> As soon as we started programming, we found to our surprise that it
> wasn't as easy to get programs right as we had thought. Debugging had
> to be discovered. I can remember the exact instant when I realized
> that a large part of my life from then on was going to be spent in
> finding mistakes in my own programs.

### Reducing the problem

To diagnose a problem, it is quite common that the program in question
has to be run well beyond a dozen times, making changes in between each
execution. This is known as the [edit-compile-debug
cycle](http://blog.kennardconsulting.com/2009/01/its-length-of-compiledebug-cycle-stupid.html).
It is essential to optimize this cycle, by reducing the time it takes to
reproduce the problem. Common techniques to accomplish that entail:

-   writing a script or a macro to perform the steps that would have to
    be performed manually otherwise
-   saving intermediate results just before the problem occurs, either
    manually or by changing the source code, and then reproducing the
    problem starting from the intermediate result
-   reducing the data size (this is not always possible, but when it is,
    it can save days, even weeks, of work)
-   saving intermediate status (e.g. serializing data structures in
    Java)

### Using a debugger

A debugger (such as the one built into Eclipse) can do wonders to
diagnose problems. Single-stepping, and inspecting the local variables,
*just* before the problem occurs, often provides enough insight to
identify the problem and even to come up with a tentative fix. The trick
is typically to get to the point *just before* the problem occurs.

Some techniques can help getting there quicker:

-   Bisect the code to figure out where the problem occurs. If no stack
    trace provides any indication where the root of the problem's cause
    lies, e.g. when the program crashes "without a trace", set a
    breakpoint roughly in the middle of the code you suspect to be the
    culprit. Run the program. if it stops at the breakpoint, the problem
    occurs in the second half, otherwise it occurs in the first half.
    Rinse and repeat.

-   Bisecting can also be performed by commenting out large chunks of
    the code; This is useful if the symptoms occur at a different point
    than the root of the problem. Example: if a lengthy computation
    (e.g. a Fourier transform) is performed by the program before it
    throws an exception, it makes sense to test whether the Fourier
    transform is required to trigger the problem by simply commenting it
    out and running the test procedure.

-   Define functions testing for invariants. Example: if the bug
    manifests itself in negative values (and negative values are
    obviously incorrect), write a function that tests the data for
    negative values, throwing an exception when the expectation is
    violated. Call that function in strategic places, recapitulating the
    "bisect" strategy described above.

-   Remember that code can be modified, even for the temporary purpose
    of identifying a bug. For example, when the problem happens during a
    loop over 300 items, it is not smart to single-step through 75
    iterations while knowing fully well that the problem occurs in the
    76th iteration, or when, say, the name of the 76<sup>th</sup> item
    is "huzzah!". A simple multi-line conditional with a breakpoint can
    accelerate the debugging procedure dramatically. Java example:  
    ```java
    for (int i = 0; i < list.length; i++) {
        if (i == 76) {
            System.err.println("Set a breakpoint right here to avoid tedious single-stepping!");
        }
        ...
    }
    ```

    Many IDEs also have a [conditional
    breakpoint](http://wiki.eclipse.org/FAQ_How_do_I_set_a_conditional_breakpoint%3F)
    feature that allows a breakpoint to trigger only when the value of a
    particular expression evaluates to true. This feature is especially
    useful when code changes to the program being debugged are not
    possible (e.g., when working with compiled Java classes with
    read-only sources).

-   Instrument data structures, introducing conditionals triggering in
    the unexpected case (read: when the error occurs). Java example:
    assume that a menu entry with the label "Dscho rocks" is introduced
    by mistake, and you have no idea from where. You can subclass the
    MenuItem class and use the subclass everywhere:  
    ```java
    private static class MyMenuItem extends MenuItem {
        private void assertLabel() {
            String label = getLabel();
            if (label != null && label.contains("Dscho")) {
                throw new RuntimeException("Breakpoint here, please!");
            }
        }

        public MyMenuItem(String label) {
            super(label);
            assertLabel();
        }
        ...
    }
    ```

    The same technique can be performed using anonymous subclasses of
    fields. Java example:  
    ```java
    ...
    private final Map<String, Object> mapping = new HashMap<String, Object>() {
        @Override
        public Object put(String key, Object value) {
            if (key != null && key.contains("Dscho")) {
                System.err.println("Set a breakpoint here!");
            }
            return super.put(key, value);
        }
    };
    ...
    ```

### Using a debugger to attach to a running program

Sometimes it seems impossible to trigger bugs in the debugger, but easy
in an external program, say, Fiji or in unit tests run by Maven.

Eclipse's debugger can actually attach to external Java processes by
creating a new debug configuration via *Run&gt;Debug Configurations...*,
selecting the *Remote Java Application* line on the left side and
creating a new launch configuration with the "new document" icon on the
upper left side, provided the Java Runtime Environment with the process
to be debugged has been started with the option
```
-agentlib:jdwp=transport=dt\_socket,server=y,suspend=n,address=localhost:8000
```
where 8000 is the TCP port on which to listen (if that port is used by
another program on your computer, you will have to change that both in
the command line starting the JVM and the launch configuration). If the
problem occurs too quickly for you to attach, you can change the *n* in
*suspend=n* to *y* to ask the Java Runtime Environment to wait for a
debugger before starting the Java main class.

If the problem occurs in a unit test by Maven, you will want to select
only the failing unit test via *-Dtest=&lt;class-name&gt;*, and if your
project is based on pom-scijava, you can configure Java to be started up
appropriately by specifying
```
-DargLine=-agentlib:jdwp=transport=dt\_socket,server=y,suspend=n,address=localhost:8000
```
(if your project is not based on pom-scijava, you will have to configure
the argLine in the maven-surefire-plugin section in your pom.xml).

The GNU debugger *gdb* can attach to running C/C++ programs using the
*attach &lt;pid&gt;* command. Microsoft Visual Studio can attach to
running programs using *Debug&gt;Attach to Process..*., too.

### Debugging Jenkins jobs with a debugger

When a Jenkins job is failing, but you cannot reproduce the problem on
your local machine (e.g. when it is a platform-specific bug), all is not
lost. You can still use the attach-to-running-program strategy described
above, but it is *slightly* more involved.

First of all, disable the Jenkins job. Really, you do not want Jenkins
to interfere with your tests, or worse, your tests to interfere with
Jenkins.

Open an SSH tunnel (because you are unlikely to have direct access to
the TCP ports of the Jenkins node): log into the Jenkins node via:
```
ssh -L 8000:127.0.0.1:8000 &lt;jenkins-node&gt;
```
This will ask ssh to listen on port 8000, and once the debugger connects
to it, forward the connection to the *remote* side's 127.0.0.1
(localhost) port 8000. Now you can use the strategy described above for
attaching to Maven while it runs unit tests.

If you cannot access the Jenkins node via ssh, all is not lost, as long
as you can connect *somehow* (e.g. VNC, remote desktop) and call
```
ssh -R 8000:127.0.0.1:8000 &lt;your-development-machine&gt;
```
on the Jenkins node. If your development machine cannot be accessed from
the Jenkins node via ssh, you can still tunnel by using a publicly
accessible SSH server in the middle.

### Preventing future problems

It is human nature to assume that others' shortcomings do not apply to
oneself. However, experience shows that there are no exceptions when it
comes to reliving Maurice Wilkes' story of realizing that more time is
spent on figuring out causes and fixes to problems than is spent on
introducing said problems.

To that end, it is a remarkably clever idea to introduce regression
tests, i.e. small pieces of code that verify that certain functionality
of the software works as expected (see also *Unit Testing (JUnit)* in
[Developing Software @ LOCI](/developing-loci-software)).

It is also a remarkably clever idea to introduce regression tests *just
after fixing bugs*. After all, whenever a bug was found by a user, and
had to be diagnosed and fixed by a developer, it demonstrates an obvious
lack of regression tests.
