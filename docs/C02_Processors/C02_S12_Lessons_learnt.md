# Lessons learnt

We have seen that there are four levels of parallelism in supercomputer, and 
three of them are also relevant for PCs or even smartphones:

1.  Instruction level parallelism through pipelining and superscalar execution,
    present to some degree in virtually all processor designs these days, even
    very low power ones for smartphones.

2.  Vector instructions (and now coming up, matrix instructions) to do more work
    per instruction. These are also used in even a lot of low power designs,
    have been crucial to get additional performance from even PC processors in the 
    past 20 years, and are even useful in lots of integer based compute tasks
    on modern CPUs (AI inference, image and sound processing, ...)

3.  Shared memory parallelism: Many cores sharing a single memory space. 
    This type of parallelism is also present in PCs and even smartphones,
    but supercomputers take it to a larger scale, reaching limits of scalability
    of operating systems.

4.  Distributed memory parallelism. This is the only level of parallelism that is
    not found in your PC or smartphone. Both may more and more rely on distributed
    apps where some of the functionality comes from remote servers, but this kind
    of distribution uses very different techniques from what is needed to run, e.g.,
    a simulation code on a distributed memory infrastructure.

The processors in a supercomputer are optimised for parallel performance and not
for sequential performance. Hence a single threaded application may run a lot faster
on a high-end PC then on a supercomputer.

It is also important to realise that computers and compilers have evolved over the
years. As part of the performance growth of processors comes from the addition of new
(mostly vector) instructions, a ten year old binary won't run efficiently. Due to other
changes in the system architecture, it might not even run at all. Code that remained 
unmodified for 20 years probably will not be able to extract all parallelism of
modern supercomputers, even when properly recompiled. It might in fact not even be
efficient on your PC anymore.

It is also important to realise that supercomputers contain 1000's of times 
more parts than a regular PC. Even though care is taken to add some redundancy
where affordable and even though there is a focus on using rather reliable components,
a supercomputer is not as reliable as your PC. Crashes are often contained to a small
part of the supercomputer (e.g., single nodes failing), but if you run on a large
number of nodes you will experience a lot more problems than you are used to from 
running small problems on a PC. It is not a good idea to run a large
multi-day job without making sure that it stores enough data from time to time to
restart an interrupted computation.
