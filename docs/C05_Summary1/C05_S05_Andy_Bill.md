# Andy and Bill's law

<center>**What Andy giveth, Bill taketh away**</center>

The law originates from a humorous one-liner told in the 1990s during computing conferences.
Andy in this law is Andy Grove, CEO of Intel and later Chairman of the board between 1987 and 2004. 
In those days Dennard scaling and the rapid evolution of semiconductor technology made a rapid 
growth of processing power for PCs possible and the single chip designs from Intel were quickly
catching up with the much more expensive often multi-chip designs for workstation and server
processors. Bill in this law is Bill Gates, founder of Micorosft and its CEO and Chairman between
1975 and 2000. Especially in the '90s, when GUIs became popular, PC software rapidly expanded
and always managed to use all available processing power, sometimes not feeling any faster
than the previous version of a package on a previous generation of hardware. With some
packages it felt as if you didn't really get that much more work done quickly even though
your hardware became faster and faster.
However, beyond this law is also a frustration of Andy Grove who felt that Bill Gates
wasn't always making proper use of new performance-enhancing features of the new processors
and hence not using the new hardware to the full potential.
Intel introduced the 80286 in 1982 and it was first used by IBM in the IBM AT in 1984, but
it took until 1987 before an OS appeared that was aimed at regular PC users and fully 
exploited the 80286, with OS/2, which was in fact mostly built by IBM. Though Microsoft
should not be entirely to blame for this as it was not possible to use the extended 
mode (actually called protected mode) while keeping compatibility with older DOS software also, except via an 
undocumented trick (which is what OS/2 used). 
The 80386 launched in 1985 and was the first processor of Intel that offered a 32-bit instruction set, and it 
was also designed to run old 16-bit DOS programs well together with 32-bit software. 
Though there were some Unix variants that supported the CPU in 32-bit mode and 
a 32-bit version of OS/2 in early 1992, it took Microsoft until Windows NT 3.1 in 
July 1993 to come with an OS for typical PC use that fully exploited the 32-bit 
features of the 80386 (by then succeeded by the 80486 and Pentium).

It used to be common practice in much of the scientific computing community to 
ridicule Microsoft and to claim that UNIX and UNIX-derived operating systems are superior.
A large part of the scientific computing community isn't doing any better though
and we can think of several laws equivalent to Andy and Bill's law that 
apply to scientific computing.

<center>**What Andy giveth, Cleve taketh away**</center>

where Cleve is Cleve Moler who really started the development of efficient linear
algebra libraries such as LINPACK and EISPACK, predecessors to LAPACK, but then
also developed MATLAB as a user-friendly environment to experiment with those
libraries and in 1984 was one of the founders of MathWorks, the company that 
went on to develop Matlab into what it is today. Matlab evolved into an 
excellent system to prototype numerical algorithms. However, its language
is not nearly as efficient as traditional programming languages when it comes
to execution efficiency and hence is a good way to slow down modern hardware.

<center>**What Andy giveth, James taketh away**</center>

where James is James Gosling, the main developer of the Java programming language.
Java, and many other programming languages from that area, may have good ideas to 
improve programmer productivity or make it easier to run code on multiple machines,
but this also came at a cost of performance. The first versions of the Java virtual 
machine were just slow, and even later versions based on a just-in-time compiler are
not that spectacular. Designers of just-in-time compilers have long promised us better
performance than regular compilers as they can use runtime information to further
improve the generated code, but the reality is that gathering that information and
using it properly to improve the generated code is too expensive and cumbersome. 
Abstracting away too much of the memory system is also not a good idea as making
proper use of the memory hierarchy is essential for performance and one cannot expect
compilers to do be able to do the necessary code transformations on their own.
Integrating with code written in other programming languages that make it easier
to write high-performance library routines is also very cumbersome.
And getting garbage collection to work well in a distributed memory context also
requires build-in support in the virtual machines for this type of parallelism
and cannot be done via a simple library add-on (there has been an effort do do MPI
for Java but that didn't work well because of this).
Granted not everything about Java is bad though. The language did support concurrency
in the base language and was hence ready for shared memory execution.
And in theory the just-in-time compiler concept also allows to quickly adapt to new
processor architectures, if it were not that the language lacked the features to 
enable to compiler to easily vectorise code, one of the features that influences 
performance of modern processors most on scientific code.

At some point Java gained some popularity in scientific computing basically because it
became the first programming language taught at many universities and hence the one
that beginning researchers were most familiar with, but it is mostly given here
as an example of languages that try to abstract away to much of the underlying system 
architecture and hence tend to run with less than optimal efficiency.

<center>**What Andy giveth, Guido taketh away**</center>

where Guido is Guido van Rossum, the original developer of the Python scripting language
(back in 1989 already). Python for a long time was a scripting language only known
by system administrators and the like, but from 2005 on, with the advent of NumPy,
became more and more popular in scientific computing. The Python ecosystem exploded 
with lots of small and often badly tested packages, and the language designers got in 
the habit to break code with every minor release every 18 months. Moreover, the 
language is usually interpreted and the interpreter is extremely inefficient on
much code. In fact, the Python designers aren't really to blame for this as the 
language was developed with a completely different purpose in mind and did a good job
at that for a very long time. There have been several efforts to develop just-in-time compilers
(and an ahead-of-time compiler to C) 
for Python, but as of today there is still no JIT that does well on most Python code,
and several companies that invested in the development of one have given up, though 
all compilers will probably come with examples where they offer a 100x or 1000x speed
increase over naively written pure Python code for small specific fragments.
Cython is an example of an ahead-of-time compiler that needs some help as regular 
Python code doesn't really offer enough type information to generate efficient code. 
Numba and PyPy are two examples of just-in-time compilers wheve Numba seems to do best
with code that heavily uses NumPy data structures while PyPy works better on non-NumPy
code.

Never mind that we also tend install Python and its packages from repositories that often
only contain generic binaries compiled to run on as large a range of hardware as possible
rather than binaries that exploit specific features of each processor to optimise
performance.

Of course it is easy to write bad performing code in any programming language, but the point is
that there are languages where it is near impossible or even just impossible to write
truly efficient code. One used to get away with that in the days that the performance-for-money
ratio improved a lot with every new generation of hardware. But as we have discussed,
these days are over for now and it may be a long time before they return. For now, progress
will have to come from better, more efficient software that better exploits the features of
current hardware. 

There may be a lot of discussion today about how quantum computers or computers with optical
components will solve all problems. This is just discussion by the hopeful. The reality today
is that the quantum computer is still looking for an application in which it will excel,
and currently needs to be attached to a rather powerful traditional computer also 
simply to turn the information that it produces in something useful. The cooling needed
for a quantum computer is also extremely expensive as it needs to be cooled to 
nearly the absolute zero. Every fraction of a Kelvin above the absolute zero makes the results
worse because of noise (which manifests itself as errors in the computation).
Don't be fooled by the pictures you see of quantum computers: Most of these pictures don't even show
the quantum computer nor all the hardware that is needed to measure the quantum state.
The most popular pictures tend to show the nice-looking multistage cooling system.
The same holds for optical computers. The reality there is that we are still very far from 
integrated optical circuits, let alone ones that are powerful enough to do computations quicker than
our current computers. The amount of money needed to develop that technology into something
useful may turn out to be prohibitive unless we really hit barriers very hard.

In fact, we have seen this happening before. As we have discussed, flash memory has serious
problems. Longevity is a big problem. And furthermore it is still a block-oriented medium 
limiting the ways in which it can be used. There were two promising technologies to replace it
over time: the memristor and phase change memory. A form of the latter was brought to market
as 3D XPoint by Micron and Intel in 2017. However, it was expensive compared to flash memory,
partly also because there was a huge development cost that needed to be paid back with the
relatively low initial volumes, and it would still have required a lot of funding to become
truly price-volume competitive with flash memory. The technology was abandoned in 2022 because of that. 







