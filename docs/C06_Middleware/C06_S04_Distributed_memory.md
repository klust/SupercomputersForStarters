---
tags:
-   distributed memory
-   MPI
-   PGAS
-   Julia
---

# Distributed memory supercomputing

Automatic strategies through the compiler have never really made it past
the research phase, but there are other options.


## Messaging libraries

In the early days of distributed parallel computing, each vendor had its own
communication library which meant that it was very difficult to write portable
programs. 
[PVM](https://www.epm.ornl.gov/pvm/pvm_home.html) which stands for "Parallel Virtual Machine"
was a research project that developed a very popular library that could even be used to
combine a couple of workstations into a distributed memory cluster. 
Being more a research project, and basically being developed by a small number of groups,
it never really reached a level of support by supercomputer vendors themselves.

However, just two years after the launch of PVM, a standardisation effort was started
which led to the hugely successful [MPI standard](https://www.mpi-forum.org/) 
which is still in use today. 
MPI stands for Message Passing Interface
and is fully standardised. This implies that software that compiles with one MPI library
should also compile with any other MPI library adhering to that version of the standard.
Compatibility is only at compile time though. The binary interface of the MPI libraries is
not standardised and differs between implementations. 
The current version of the standard is 4.1, which was launched in November 2023.
There are two main public domain implementations, [Open MPI](https://www.open-mpi.org/)
and [MPICH](https://www.mpich.org), 
which has an offspring that one could consider a third implementation,
[MVAPICH](https://mvapich.cse.ohio-state.edu/).
Most vendor based MPI libraries are derived from one of these implementations.
Note that the version of the MPI standard and version number of those libraries are not the same!

Many MPI programs skip the shared memory level, using one single-threaded process per
core or even hardware thread. On modern CPUs it may be more efficient though to combine
MPI with one of the shared memory programming techniques (most often OpenMP) and run with, 
e.g., one process per node, per socket, per NUMA domain or on AMD architectures, per L3 cache 
domain (CCD on zen3 or CCX on Zen2). 
With the rapid increase of the number of cores on a supercomputer node this is more and more
becoming a necessity as just the amount of messages risks to overwhelm the network adapter
and the number of processes in an MPI job slows down program startup and collective communication
(the type of communications where all processes compute a single result, e.g., a sum of numbers
across all processes) become too expensive.
Such applications are called *hybrid MPI/OpenMP applications* (assuming they use OpenMP).
Examples of programs that are sometimes used in hybrid mode are QuantumESPRESSO, Gromacs and VASP.

Examples of vendor-specific MPI libraries:

-   Intel MPI is a derivative of MPICH,
-   HPE Cray MPI is a derivative of MPICH,
-   Mellanox/NVIDIA distributes Open MPI.


## Languages

Some programming languages have distributed memory concurrent computing built into the language
itself. 

One such language that we already mentioned is [Julia](https://julialang.org/). 

Another example is 
[Charm++](https://charmplusplus.org/), a language that is used in the popular molecular dynamics
code NAMD but also in a few other lesser known or more specialised applications. 


## PGAS languages

Partitionad Global Address Space (PGAS) languages distinguish between local and remote memory
but allow the latter to be used almost as if it is local memory, though with a severe speed penalty.
It is then up to the compiler to translate that in message for the underlying hardware/OS/middleware
combination.
These languages were all the hype between roughly 2000 and 2010 when DARPA funded the development 
of new high performance high productivity languages through the HPCS program. 
It led to the development of three languages: 
[Fortress](https://en.wikipedia.org/wiki/Fortress_(programming_language)) (by Sun Microsystems), 
[X10](https://en.wikipedia.org/wiki/X10_(programming_language)) (by IBM),
and [Chapel](https://chapel-lang.org/) (by Cray), but Chapel seems to be the only surviving
language of that DARPA program and is 
still begin developed by HPE Cray. 
Two other languages of this type not developed in the DARPA program are 
[co-array Fortran](https://en.wikipedia.org/wiki/Coarray_Fortran) and
[Unified Parallel C (UPC)](https://upc.lbl.gov/). 
Co-array Fortran became part of the Fortran 2008 standard and some Fortran
compilers offer basic support for it. Unified Parallel C was derived from C99. It never became
part of a standardisation process and as a result official support by compiler vendors is poor. 
However, there are still compilers based on Clang and on GCC, and HPE Cray supports UPC
in their Cray Compiling Environment compiler (which is based on Clang).

The problem with PGAS languages is that performance is often poor. Therefore codes sometimes
opt to still do some of the work with MPI.

There are also library based approaches to PGAS. SHEM/OpenSHMEM and GASPI are just two examples.
GASPI is also sometimes used as the runtime library underlying other PGAS languages. 
Single-sided communications in MPI-2 and later MPI standards use a very similar approach as those
libraries.

PGAS languages and libraries have never become very popular, maybe with the exception of single-sided 
MPI communications.
