# An important difference between a PC and a supercomputer...

As we have seen, many supercomputers share a lot of similarities with 
PC's. Yet there is an important difference that cannot be underestimated:

The answer to the question "Can a PC be faster than a supercomputer today?" may
actually surprise some, but the correct answer is "Yes". 

That is because they are optimised for very different things. Hence software
that is written with PC hardware in mind does not always perform as well on
supercomputers, and there are some science domains where the step from PC
or small server to a supercomputer is becoming a big problem.

We will discuss one cause now, while another big problem when moving from
a PC or small server will be discussed in the 
[chapter on storage](../C04_Storage/index.md).

The power consumption of a chip does not scale linearly with the clock speed of
a chip. Doubling the clock speed for a given core architecture requires far
more than twice the power. In fact, when close enough to the limits of the 
design a factor of 4 or more is not uncommon. Now for ease of computing assume
a factor of 4. So assume that for a given core architecture we would be able to run
4 cores at 4 GHz using a total of 100W of power. Then with the same amount of power
we would be able to run 16 cores at 2 GHz. Now to make computations easy we also assume 
that the IPC would be the same at both clock speeds (which is not the case as 
the wait time for data from main memory doesn't change much with the clock speed),
then each core in the first configuration, with the cores running at 4 GHz, would
be twice as fast as each core in the second configuration. A single-threaded
application would run twice as fast on the first configuration as on the second.
However, the total amount of instructions that the whole package could process
is twice as high in the second configuration. Even though each core is only half 
as fast, we have four times the number of cores. So a parallel application that can
use all cores effectively would be twice as fast in the second configuration.

The reality is a bit more complex as nowadays CPUs don't run at a fixed clock
frequency but have a so-called turbo mode that can run some cores at a higher clock
speed when the other cores are not used, or can even temporarily increase the clock 
speed and power consumption under a heavy load (or, e.g., when a program doesn't make
heavy use of vector instructions). But even then we still see important differences
between the chips used in PC's and in supercomputer nodes.

Processors for your desktop PC and to some extent laptops are optimised to run moderately
parallel and serial applications well. So the choice was made to use fewer cores but run
those at a higher clock speed. Moreover, if not all cores are in use, the cores that are
in active use can be clocked even higher and laptop chips may even have an extremely high
single core boost frequency compared to their base frequency, the frequency they run at 
in normal use with a normal power consumption. That turbo boost allows to run single 
threaded applications at higher speed, while keeping some of the benefits of more but
slower clocked cores for better parallel throughput per Watt.

Processors for supercomputers and servers are optimised for a different application profile.
They use way more cores, but run those cores at a lower clock speed and the turbo boost
algorithms are also tuned differently, not boosting to nearly as high frequencies as in the
PC chips. 

E.g., consider the latest chip generation of Intel in February 2023. The top version of the
13<sup>th</sup> gen "raptor lake" CPU, the i9-13900K, has two types of cores, the 8 performance 
cores that support very fast single-thread execution, and the 16 efficiency cores that 
improve parallel performance at a given power consumption, and are also used when the high
speed is not needed. The chip has a base power of 125 W, but when clocking up some or all
cores its power consumption can temporarily increase to 253 W until the chip becomes too hot.
For the performance cores the base frequency is 3 GHz but one core can clock up to 5.8 GHz 
to run single threaded applications at very high speed. Intel's top server chip at that time
was the 4<sup>th</sup> generation Xeon Scalable processor code named Sapphire Rapids. 
The core is actually very similar to the cores used in Raptor Lake, though the latter have 
the AVX-512 instructions disabled as these instructions are not supported by the 
efficiency cores. The top-in-the-line variant, with 60 cores, runs those cores at a base
frequency of only 1.9 GHz, 35% slower than the PC processor, and only allows the frequency 
to boost to 3.5 GHz, only 15% higher than the base frequency of the PC processor. 

Another illustration is the table below, for two older processor types before the days
of turbo boost etc. in server processors, when it was much easier to analyse performance:

<center>

|                       | E5-2623v3 | E5-2660v3 | E5-2637v4 | E5-2698v4 |
|:----------------------|----------:|----------:|----------:|----------:|
| cores                 | 4         | 10        | 4         | 20        |
| clock                 | 3 GHz     | 2.6 GHz   | 3.5 GHz   | 2.3 GHz   |
| power                 | 105 W     | 105 W     | 135 W     | 135 W     |
| Gflops peak DP/core   | 48        | 41.6      | 56        | 36.8      |
| Gflops peak DP/socket | 192       | 416       | 224       | 736       | 

</center>

The first two processors in this table from the generation code-named Haswell, 
are two 105 W parts, which was a level of power often used for supercomputer
nodes in those days. We see that for that same amount of power we could run
4 cores at 2.6 GHz, but 10 cores at a speed that was less than 15% lower. 
The 4 core chip of course offers slightly higher single thread performance,
but if we look at the total amount of double precision floating point 
operations the chip was in theory capable of, then the 10-core chip is over
twice as fast at the same power level. The last two columns in the table are for
two 135 W parts from the next generation, code-named Broadwell, and are two
135 W parts (there was one part with even more cores but that one is left out
of the comparison as there is no low core count variant that runs at the same
power). The 4-core part can now run at 3.5 GHz which is considerably faster
than the 20-core part that runs at only 2.3 GHz, 35% lower. This of course
translates into the single thread performance. But now the total number of 
double precision floating point operations the 20-core chip can do per second is 
more than three times that of the 4-core part at the same quoted power level.

This shows that if your software or at least your workflow is not parallel, running
on a supercomputer is a waste of money. Then there is no faster machine to run your
application than a high-end gaming PC (though you can omit the expensive graphics card
as your software will probably not be able to use that one properly either). 
And incidentally, these PC's also tend to have disk storage that is way faster than
a supercomputer can offer for disk access patterns with lots of small random reads,
something that we will discuss in the 
[chapter of storage](../C04_Storage/index.md).

**Supercomputers need appropriate software to function as a supercomputer! 
In fact, one key point of supercomputing since the mid '80s has been adapting 
software to be able to use cheaper hardware at scale rather than investing
in extremely expensive hardware that still cannot do the job as well as
good software can do.**