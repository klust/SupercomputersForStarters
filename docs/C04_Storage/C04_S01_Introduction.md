# Introduction

We've seen that physics makes it impossible to build a single processor core
that is a thousand or a million times faster than a regular CPU core in a PC and that
we need to use parallelism and lots of processors instead in a supercomputer.
The same also holds for storage. It is not possible to make a single hard disk
that would spin a thousand times faster and have a capacity a thousand times more
than current hard disks for use in a supercomputer. Nor would it be possible to upscale
the design of a solid state drive to the capacities needed for supercomputing and
at the same time also improve access etc.

A storage system for a supercomputer is build in the same way as the supercomputer
itself is build from multiple processors: Hundreds or thousands of regular disks
or SSDs are combined with the help of some hardware and mostly clever software
to appear as one large and very fast disk. 

In fact, some of this technology is even used in PCs and smartphones as an SSD
drive also uses multiple memory chips and exploits parallelism to get more performance
out of the drive as would be possible with a single chip. This is why, e.g.,
[the 256 GB hard drive in the M2 MacBook Pro is slower than the 512 GB one](https://arstechnica.com/gadgets/2022/06/m2-macbook-pros-256gb-ssd-is-only-about-half-as-fast-as-the-m1-versions/)
as it simply doesn't contain enough memory chips to get sufficient parallelism
and saturate the drive controller.

However, just as not all programs can benefit from using multiple processors, not all
programs can benefit from a supercomputer disk setup. A parallel disk setup only
works when programs access large amounts of data in large files. It can improve bandwidth a lot,
but the latency of each individual device still restricts the latency of the system as a whole.
And the same is true for storage as for computing: The storage of your PC can be
faster than the shared storage of a supercomputer if you don't use 
the supercomputer storage in the proper way.
But similarly accessing files in the right way may make your already fast PC storage
even faster as there are applications that are so badly written that they use the
SSD in your PC also at only 5% or 10% of its potential data transfer speed...
