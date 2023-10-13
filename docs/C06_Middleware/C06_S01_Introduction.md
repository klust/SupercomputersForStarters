# Introduction: Why do I need to know this?

A supercomputer is not only about hardware. It is the software environment that turns
hardware that is often just more or less regular server hardware repackaged for density
in a performant supercomputer, and a lot of that software is so-called middleware that
sits in between the operating system and the user application.

Now a typical reaction is "I'm not a programmer, should I know this."
The answer is yes, for multiple reasons.

It is important to realise that while your workstation is some standard hardware
with a standard Linux distribution on top of it, this is not the case for a supercomputer.
We've seen that some of the hardware is relatively standard, and almost all academic 
supercomputers run some variant of Linux. But with that all you have is a loose set
of servers, and not a supercomputer. There is a lot of software on top of that which 
we call here the middleware, that turns this into a supercomputer. Some of it is management
software and special file systems, but there are also libraries that provide you with 
different means to exploit parallelism, e.g., communication libraries for distributed
memory supercomputing, or libraries that are used internally by programming languages,
e.g., the OpenMP runtime library that a compiler that supports OpenMP uses for many
different operations. Many of those libraries are standardised at the API level (Application
Programming Interface, the set of function names, predefined constants, etc., that you can
use in your programs) but they are not standardised at the ABI level (Application Binary Interface,
the interface that your program compiled with that library links to). MPI is a notorious example
of a library with a well standardised API but non-standardised ABI. 
OpenMP may be very standardised at the level of the programming language, but 
implementers of the runtime libraries have total freedom except for some functions
defined by the OpenMP standard. All this implies that a program that is compiled 
with one compiler or one MPI library, cannot use the runtime libraries of another compiler
or a different MPI library when running. One one hand, this implies that mixing compilers
in your environment can cause problems which is why so many clusters are very strict about
which programs can be used with which other programs. On the other hand, it also implies that
getting programs that come as precompiled binaries to run on a cluster, is not always
possible. E.g., it may be compiled with an MPI library that does not support the 
interconnect on the cluster while it is also impossible to swap that library with one
that supports the system because the ABI is different. 

Because of this, many scientific applications come as source code. Knowing something about middleware
that is currently in use on supercomputers helps you judge whether a code is ready for
modern hardware. It also helps you to figure out which components you'll need on the cluster
and hence to check if these components are available or can run on that cluster.
The middleware that has been used for the application also affects the way you have to start
your program. E.g., distributed memory programs are often started through another program that
is part of the middleware, the so-called *program starter*.  In other cases, some environment
variables need to be set to tune the performance. This is often the case with shared memory
programs but also more and more often for distributed memory programs, and certainly for big
programs on big clusters.

Moreover, often the support teams can no longer do all software installations for their users
as the variety in software becomes too large and the quality of the installation procedures too low,
so more and more personpower is needed for software installations, personpower that is
often not available.

We'll now run through middleware covering three levels of parallelism: vectorization, shared and
distributed memory, but will do so in a different order that is more suited to explain the concepts.

!!! Note "LUMI users"
    Getting precompiled binaries to run is a particular problem on LUMI. 
    It needs an MPI library with explicit support for the Slingshot 11
    interconnect for good performance, which implies that it must use the
    libfabric library on LUMI. Cray MPICH does so and supports the 
    "MPICH 3.4 ABI", so it can replace other MPICH libraries that support
    that ABI at least if there are no other runtime or general library
    conflicts. But Open MPI had a different ABI and Open MPI software 
    rarely runs well on LUMI, and certainly cannot use GPU-aware MPI
    on LUMI at the moment (late 2023).

    The same can happen with software that is compiled with a newer version
    of ROCm than we have on the system. Sometimes the functionality needed by
    that package will be supported by the current device driver on the system,
    but sometimes it won't and there will be no way to get that package to work,
    not even in a container.
