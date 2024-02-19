# Introduction

Long ago supercomputers were build from specialised processors designed specifically
for supercomputers. This was in the days that chip technology used large geometries and
the speed of even multichip processors was limited by the switching speed of transistors
and not by physical distance. Cheaper minicomputers and the early personal computers used
single chip processors while mainframes and supercomputers used processors that were built
from multiple chips, sometimes several thousands of them, that often also used a faster but more
expensive technology (e.g., ECL instead of CMOS).

Halfway the '80s things started to change slowly. The processors for PC's that were built
from 1 or 2 chip packages (in the case of 2, one for the floating point instructions and one for
everything else) became more competitive, not yet in speed, but in price/performance,
leading to new supercomputer architectures based on those much cheaper processors.
In the course of the '90s processors consisting of multiple chip packages gradually disappeared
as the communication between packages became the speed limiting factor. 

Gradually all supercomputers were no longer being built from very specialised processors, but from
processors derived from those used in PCs, and nowadays even from processors derived from those in
smartphones, but with enhancements for better reliability.

These CPUs have gone through a long evolution. 
Both advances in semiconductor technology and advances in
architecture made possible by the semiconductor technology advances have made it possible to build
ever faster CPUs. The workings of all modern processors is governed by a clock, yet the clockspeed 
(which is measured in GHz nowadays rather than in MHz in the '80s) is not a good basis to compare
the speed of CPUs. This is because there have been so many architectural advances that have increased
the amount of work a CPU can do during each clock cycle. These improvements fall into two categories:

-   Improvements that enable to execute more instructions per clock cycle: this is called
    **instruction level parallelism**, and
-   improvements that enable the CPU to do more work per instruction, and the main strategy here is
    **vectorization** and nowadays even **matrix computing**.

Yet those improvements were not enough to satisfy the ever growing need for compute speed of
researchers, so two more strategies were used:

-   Using more CPUs (now called **cores**) that share memory which is called
    **shared memory parallel computing**, and
-   Using multiple nodes that collaborate not by sharing memory but by sending messages over a
    network, which is called **distributed memory parallel computing**.

All 4 of these strategies - instruction level parallelism, vector and matrix computing,
shared memory parallel computing and distributed memory parallel computing - rely on finding and
exploiting parallelism in the solution of computational problems, and the further down you go in
the list, the more difficult this becomes, but also the more performance you can get.

As we shall see, nowadays even your PC and smartphone rely on the first three types of parallelism for
performance, so learning to deal properly with parallelism is no longer an option but a necessity to
remain competitive.
