# Parallelism in a modern (super)computer

A modern supercomputer combines all those levels of parallelism

-   Almost all modern supercomputers are distributed memory machines, consisting
    of a network of compute nodes and some specialised nodes for special purposes.
-   Each node is a shared memory machine. In most cases these are of the NUMA
    type with two (or more) CPU packages (or sockets). 
    Even single socket nodes often have an on-chip NUMA character, e.g., the
    Fujitsu A64fx processor used in Fugaku (and soon also in a European
    petascale machine)
-   Each socket contains a multi-core CPU very similar to the ones used in
    PC's or in some cases smartphones but with a lot more cores,
-   Each of those cores often operates as two (or in rare cases more) virtual
    cores through SMT.
-   Each processor core on such a chip nowadays supports vector instructions
-   and has extensive instruction-level parallelism.
-   And it becomes even more complicated when adding an accelerator, e.g., a GPU
    to the mix.

However, most of those levels of parallelism are not exclusive to supercomputers.
All modern servers have all but the first level of parallelism in this list and
are shared memory machines with in some cases more sockets than a typical 
supercomputer node.

PC's contain only one socket, but that socket also contains a multi-core CPU,
often with SMT, with vector instructions and extensive instruction level parallelism.
In fact, some PC's have NUMA-like behaviour, not caused by the memory structure
but by the way processor cores are linked to each other and share certain buffers
in the system (cache memory), and others are not even uniform. E.g., Intel 12<sup>th</sup>
and 13<sup>th</sup> gen Core processors code-named Alder Lake and Raptor Lake 
(and later generations) combine two different types of cores, one type optimized for
fast sequential execution and the other type optimized for power efficiency and
better parallel performance per square centimetre of die. (And unfortunately as
the efficiency cores don't support the same vector instruction set the more
advanced vector instructions are also disabled in the performance cores).
PC's often also use their GPU to accelerate some applications.

Smartphones are not that different from modern PC's. Many smartphones contain two
or three types or cores, or at least a single type in two different versions, one version
more optimised for fast performance and the other for better power efficiency.
The GPUs can often also be used for accelerating computations in some apps,
and most modern smartphones also have an accelerator for some deep learning AI
operations.

So much of the things you have to learn to efficiently build software for a supercomputer,
or even just to run software, may also help you when you program for servers, PC's or even smartphones,
or sometimes also to use them properly. So what you learn for your Ph.D. or postdoc about
supercomputers may still be useful later on, certainly when developing software 
(even non-scientific applications) and in some cases also when using or managing 
infrastructures.
