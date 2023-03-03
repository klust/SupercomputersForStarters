# Three keywords: Streaming, Parallelism, and Hierarchy

There are three keywords in programming for supercomputers: streaming, parallelism and hierarchy,
and we will now discuss each of them separately.


## Streaming

We have already mentioned several problems with data access on computers, not only supercomputers.

*   Data access requires a lot of power. The further the data is from the processing units, the
    more power it costs to get the data to the processing unit. But unfortunately we can only
    store a limited amount of data really close to a processing unit.
*   The further the memory is from the processing element, the higher the latency to get it 
    there to process which was the main reason why cache memory was introduced (as it was introduced
    long before the power associated with data transport became a problem).

Hence getting the data flowing smoothly through the computer, all the way from permanent storage
to processing, is key to performance.
An important way to deal with latency is the use of caches. Some are fully managed in hardware,
like the caches between CPU and RAM memory, while others are managed in software, e.g., file
systems also have a caching mechanism.
It is important to ensure that those caches can work efficiently, and that requires:

*   predictable data accesses, so that prefetching mechanisms can fetch data into the cache
    before it is needed so that part of the latency can be hidden, and
*   data accesses in sufficiently large chunks to avoid that data in caches is never used and
    hence wasted and so that the effective bandwidth is not too much reduced by latency.
    If you'd fetch individual 4-byte numbers from RAM memory and if the memory latency would
    be 100ns and if there would be no way to have those data accesses overlap as you cannot
    predict what the next piece of data would be, then you effectively get at most a bandwidth of
    4 bytes / 100ns / 1024^3 = 0,037 GByte/s which is only a fraction of the bandwidth 
    one can get from RAM memory. 

Random access to small blocks of data is bad at all levels in computers, not only supercomputers. 
Only the definition of "small" varies depending on the type of memory or storage. For RAM memory
"small" is on the order of a cache line or 64 bytes on many popular CPUs, for permanent storage
"small" is measured in kilobytes or even 10s or 100s of kilobytes for shared parallel file systems.
We haven't discussed the architecture of main memory in much detail, but there also streaming is 
important to get the fastest performance as internally memory is also accessed and buffered in
larger blocks than a single request, and a subsequent request to data that is already in that 
larger buffer will be quicker than an access to data that is not.

This is also not a new message. Some level of streaming has been important in supercomputers ever
since the 70s, and when it comes to permanent storage it has been important on PCs ever since the 
first PCs were build. One may have the impression that it has become less important with modern
flash memory based SSDs, but that is only partly true. The latency is extremely small compared to
any storage device that uses mechanically moving parts, but even then if you only access data
through small files with size in the order of kilobytes so that the caching mechanisms in file
systems cannot work, you will only reach a fraction of the theoretical bandwidth of those 
devices, and this again becomes more and more pronounced with every new generation as the 
peak bandwidth of SSDs improves quickly while the latency stays about the same. 


## Parallelism

Parallelism is important at all levels in supercomputers.
We have thoroughly discussed the 4 levels of parallelism in a the processing units in a supercomputer:
Instruction Level Parallelism and vectorisation (and in fact, recently matrix computations) in the
core, the use of multiple cores in shared memory setup, and distributed memory parallel computing.
We've also seen that parallelism is important in the design of storage systems for supercomputers,
with even specialised shared storage systems. However, as we have mentioned, in fact all but the
most basic SSDs in PCs and smartphones also rely on parallelism to reach the high bandwidth 
and high number of IO operations per second they promise to be capable of.
We haven't discussed main memory in that much detail, but in fact modern computer memory also
relies on parallelism. modern processor packages offer multiple memory controllers that work
in parallel, and usually each memory controller already controls more than one memory channel.
And even within a modern memory module there is already a level of parallelism.

Most parallelism is not automatic. Instruction level parallelism does not require much work
from a user though there are some programming techniques that improve and others that harm
instruction level parallelism. Exploiting vector computation already requires more work from
the programmer as we shall see in the following chapter, and matrix units are even harder to exploit.
Shared and distributed memory parallelism almost always requires a significant effort from the
programmer. Both can of course be exploited by simply running multiple binaries in a capacity
computing context but that does not improve the time-to-solution for a single problem.
Similarly, good performance of central storage is all but a given and requires careful
thinking about how data is stored. Some storage systems may be more forgiving than other
storage systems, but even on an SSD in a PC a bad data access pattern will leave a lot
of potential of the storage system unused.

This is also not a new lesson. Supercomputers have relied on forms of parallelism that require
help from the programmer since the '70s. In PCs some form of vector computing returned in the '90s
and became essential for floating point performance with the Pentium 4 processor in 2002. 
The first regular PCs with more than one core appeared already in 2006, and nowadays 4 cores or more
are common in even laptops and the cheapest smartphones.

In PCs single thread performance still improves over every generation but at the expense of steeply
rising power consumption. In servers and supercomputers the single thread performance has been 
stagnating for several years already as they are more optimised for performance per Watt. Even though
every new generation of core claims to do more work per clock cycle (better instruction level parallelism), 
the clock speed is often also lowered so that the net gain is practically zero.

Just as streaming is almost as important on PCs as on supercomputers, the same also holds for parallelism. 
There is only one level of parallelism that is specific to supercomputers: distributed memory parallelism.
And at the file system levels PCs also tend to be a lot more forgiving for bad access patterns, even though
you will still be exploiting only 10 percent or less of the potential of your storage.


## Hierarchy

Hierarchies appear in different ways in supercomputers, but also more and more in regular PCs.

Memory is organised in a hierarchical way with typically 3 levels of cache where the first two
levels tend to be organised per core while the third level is a cache that is shared by multiple
if not all cores, and then often two or more levels of RAM memory. Many supercomputers are built
out of building blocks with two processor sockets and accessing memory that is physically attached
to the other socket is slower than accessing local main memory, but even within a socket not all
memory might be equal. The latter is already the case on the AMD Epyc server processors and will
also happen on some versions of the Intel Sapphire Rapids series that became available in early
2023. PCs also have the same cache hierarchy, but the main memory has only one level unless you
opt for some workstation-class systems that are really built using variants of processors for servers.

There is also a hierarchy in the levels of parallelism for processing. Instruction level parallelism
and vectorisation is parallelism at a very fine scale as it is done in the instruction stream itself
and does not require any special communication or synchronisation. Shared memory parallelism is 
the next level in the hierarchy. It comes with a startup cost and cost to synchronise the threads
for some operations. It requires bigger chunks of work to be executed in parallel before the gain
from parallelism offsets the startup and synchronisation costs. Distributed memory parallelism tends
to be even coarser as the startup cost is higher and the communication between the processes
is a lot more complicated then the communication between threads in a shared memory program.

Discussing the hardware architecture and software parallelism models of GPUs in much detail is 
outside the scope of this two-lecture introduction. However, both the hardware and the lower-level
programming models for GPUs are very hierarchical.

We can expect that parallel storage will also become more hierarchical than it is today as 
supercomputer manufacturers are looking for ways to bring some storage again closer to the
processing elements without losing too much of the manageability, and as flash storage
will remain too expensive in the foreseeable future to build an all flash file system
and hence can only be used for an extra level in the hierarchy.

Exploiting the memory hierarchy is extremely important for performance as will also be
illustrated later in this tutorial. Mapping threads and processes in shared and distributed
memory parallel computing on that hierarchy is for many codes also important for performance.
One may of course wish that it would be different, but in practice some understanding of the
hardware architecture is needed even if you are only using parallel computers and not programming
them.


