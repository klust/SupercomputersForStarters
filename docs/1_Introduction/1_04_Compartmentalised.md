# A compartmentalised supercomputer

Even though a supercomputer usually runs some variant of Linux, it does work differently
from a regular Linux workstation or server. When you log on on a PC, you immediately get
access to the full hardware. This is not the case on a supercomputer. A supercomputer consists
of many compartments:

-   When users log on to a supercomputer, they land on the so-called login nodes. These are
    one or more servers that each look like a regular Linux machine but should not be used for
    big computations. They are used to prepare jobs for the supercomputer: small programs that
    tell the supercomputer what to do and how to do it.

-   Each supercomputer also has a management section. This consists of a number of servers that
    are not accessible to regular users. The management section is responsible for controlling and
    managing the operation of the supercomputer, including deciding when a job may start and properly
    starting that job.

-   Each supercomputer also has a storage section, a part of the hardware that provides permanent storage
    for data or a scratch space that can be used on the complete machine.

-   But the most important part of each supercomputer is of course the compute section, or compute sections
    in many cases as most supercomputers provide different types of compute resources to cover different needs
    of applications.

In these notes, we will mostly talk about the structure of the compute section of the cluster, but we will
also cover some typical properties of the storage system as that has a large influence on what one can do
and what one cannot do on a supercomputer.
