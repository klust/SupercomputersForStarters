# Introduction: Why do I need to know this?

A supercomputer is not only about hardware. It is the software environment that turns
hardware that is often just more or less regular server hardware repackaged for density
in a performant supercomputer, and a lot of that software is so-called middleware that
sits in between the operating system and the user application.

Now a typical reaction is "I'm not a programmer, should I know this."
The answer is yes, for multiple reasons.
Many scientific applications come as source code. Knowing something about middleware
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
as the variety in software becomes too large and the quality of the installation procedures too low
and as hence more and more personpower is needed for software installations, personpower that is
often not available.

We'll now run through middleware covering three levels of parallism: vectorization, shared and
distributed memory, but will do so in a different order that is more suited to explain the concepts.
