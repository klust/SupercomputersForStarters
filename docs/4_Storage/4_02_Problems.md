# Problems with a parallel disk setup

## Disks break

Hard drives fail rather often. And though SSDs may not contain moving parts, when not
used in the right way they are not that much more reliable. Given that a single drive
on average fails after 1 or 2 million hours (which may seem a lot), in a stack of 
a thousand drives one can expect that a drive might fail every 50 to 100 days.
Loosing some data every 50 days is already bad, but if all disks are combined with
software to a single giant disks with even average sized files spread out over 
multiple disks for performance, much more data will be damaged than that single disk
can contain. This is clearly unacceptable. Moreover, we don't want to stop
a supercomputer every 50 or 100 days because the file system needs repairs. 
Let alone that big supercomputers have even bigger disk systems...

The solution is to use extra disks to store enough information to recover lost
data by using error correcting codes. Now hard disks and SSDs are block-oriented media:
data is stored in blocks, often 4 kiB in size, so the error correction is done at the
block level. A typical setup would be to use 8 drives with 2 drives for the error 
correction information, which can support up to two drive failures in the group of 10.
But that has implications for reading and writing data. This is especially true for
writing data, as whenever data is written to a block on one disk, the corresponding blocks
on the disks with the error correction information must also be updated. And that requires
also first reading data to be able to compute the changes in the error correcting information.
Unless of course all corresponding blocks on the 8 disks would be written concurrently, as
the we already have all the information to also compute the error correction 
information. But that makes the optimal block size for writing data effectively 8 times 
larger...

For reading data in such a setup you can actually already benefit as you can read from
all drives involved in parallel.

This technique is also known as RAID, Redundant Array of Inexpensive Disks, and there
exist several variants of this technique, some only to increase performance and others to
improve reliability also.

Error correction codes are in fact also used to protect
RAM memory in servers and supercomputers, or internally in flash drives, and sometimes
also in communication protocols (though they may use other techniques also).


## File system block size

A file system organizes files in one or more blocks (which can be different from the blocks
on disk). A block in a file system is the smallest element of data that a file system can 
allocate and manage.

On a PC, the block size of the file system used to be 512 bytes though now it is often 4 kiB.
However, on a supercomputer this would lead to an enormous amount of blocks which would become
impossible to manage. 

There are two solutions for that. One solution is to simply use a larger block size, which is the
case in, e.g., the IBM Spectrum Scale file system. Larger blocks are also a better fit with the
RAID techniques used to increase the reliability of the storage system. However, as the block is
the smallest amount of storage that can be allocated in a file system, it also implies that very small
files will still take a lot of space on such a file system (the size of a block), though some file systems
may have a solution for really small files. This can lead to a waste of space if many small files
are used like that. The disk setup used at the UAntwerp HPC service until 2020 suffered from this problem.
In fact, we once had a user who managed to store 36 bytes worth of data while consuming over 640 kiB on the file 
system as each of the 5 numbers were written to a separate file, effectively using 128 kiB per number (the block
size of that file system) and 512 bytes to store the directory information for each file. Now the first 
IBM-compatible PCs had a memory limit of 640 kiB...

The second solution is to use a 2-level hierarchy. Big files are split in a special way in smaller files
called objects that are then stored on smaller separate servers called the object servers. 
As these servers are smaller, they can use
a smaller block size. And small files would use only a single object, making the smallest amount of disk space
consumed by a file the block size of a single object server. 
Examples of this approach are the [Lustre](https://www.lustre.org/) and 
[BeeGFS](https://thinkparq.com/products/beegfs/) file system used in supercomputing.

??? info "UAntwerp-specific"
    The supercomputer storage of the CalcUA facility of the University of Antwerp used the IBM Spectrum Scale
    file system (then still known as GPFS) for its disk volumes. The scratch storage had a block size
    of 128 kiB. One user managed to store 36 bytes worth of data while consuming over 640 kiB on the file 
    system as each of the 5 numbers were written to a separate file, effectively using 128 kiB per number (the block
    size of that file system) and 512 bytes to store the directory information for each file. 
    And that user ran thousands of tests each storing
    5 such files... Now remember the first IBM-compatible PCs had a memory limit of 640 kiB and students had
    to use those to write a complete Ph.D. thesis...

    The storage that was installed in 2020 at the CalcUA service uses [BeeGFS](https://thinkparq.com/products/beegfs/)
    for the file system. Which comes with its own problems as we shall see later.


## Physics and the network

On your PC, the storage sits both physically and logically very close to the processor. 
The fastest SSDs, NVMe drives, are often even directly connected to the CPU, and on the
M-series MAc this is even taken one step further, with part of the drive (the controller) 
integrated into the CPU (though this probably saves more on power consumption than it
gives additional speed). Moreover, at the logical level, there is only one file system
in between your program and the drive. The program talks directly to the OS which talks 
directly to the drive.

With shared storage on a supercomputer the picture is very different. The storage is
both physically and logically further away from your application. Physically because
there are (at least) two processors and a network involved (and on the server side disks
usually not be as close to the processor as on your PC, except in some of the most expensive
storage systems). The physical delay caused by the network may not be that important with
hard disk storage, but it is important when accessing SSDs or cached storage. 
The software adds even more delays. After all, your program talks to a network file system
which then sends the request to the server where it also has to pass through multiple layers
of software: through the network stack, to the file server software, to the file system (which may
be similar to that on your PC but doesn't have to), and back through the network stack before
the answer to the request is off again to your application, where it also first has to pass
through the network stack and network file system again.

Parallel file systems often have an optimised and shorter route for reading and writing data,
but often at the cost of more costly open and close operations and hence a very high cost
for access to small files. And the path is still much, much longer than that to a local drive.

This comes with a number of consequences:

-   Programs that open and close hundreds of small files in a short time may work slower than
    on a PC. This is particularly true if all data access comes from a single thread as your
    program will be waiting for data all the time. That software also puts a very high load on\
    the file systems of a supercomputer, to the extent that several supercomputer centres nowadays
    take measures to keep that software off the supercomputers.

-   Unpredictable file access patterns may make things even worse as any logic to prefetch data\
    and hide latency will fail.

One may wonder why supercomputers don't always provide local drives to cope with the slowness of
shared storage. There are many reasons:

-   From a management point of view, there are several problems. The management environment has
    to clean them at the end of a job, but it is not always clear which files can be removed if
    multiple jobs are allowed to run on a single node of the supercomputer. And the software that
    needs to local storage most, also tends to be the software that cannot use a full node of a
    supercomputer efficiently.

    Modern Linux does have a solution to the cleaning problem (namespaces), but that solution then comes with
    other restrictions that users may have trouble living with, e.g., starting processes on
    other nodes should always be done through the resource manager software to ensure that the
    processes start in the right namespace. Which implies that, e.g., `ssh`, a very popular
    mechanism to start a session on a different host, should not be used. 

    Moreover, the data becomes inaccessible to the user when the job ends, so if the so-called
    *job script*, the script that instructs the supercomputer what to do, is ended because the
    resources expire or because of a crash, the data on the local drive will be lost to the user.
    For reading one also loses time as the data needs to be copied first to the local drive.

-   It is physically also not easy to add local drives to a supercomputer node. Supercomputer nodes
    are built with very high density as it is important to keep all links in a supercomputer as short
    as possible to minimise communication delays between nodes. Modern CPUs and GPUs run very hot,
    while storage prefers a lower temperature. 

    On an air cooled node, the storage has to be early in the air flow through the node as once the
    air has gone through the CPU or GPU coolers, it is way too hot to cool the storage sufficiently.
    But given the small size of a supercomputer node, those storage devices may hinder the air flow
    through the node and hence make the cooling less effective. 

    On a water cooled node, things aren't that much easier though the situation is improving. As 
    M-type SSDs (those that you insert in a slot on the motherboard close to the CPU) nowadays
    even need cooling in a regular PC, they have been made more friendly to the addition of cooling
    elements.

-   However, reliability of SSD drives is also an issue. SSD drives based on flash memory (which is
    currently practically any drive still in production) have a limited life span under a heavy write
    load, while the use suggested here, as a temporary buffer for the duration of a job, is precisely
    a scenario with a high write load. 

    Replacing broken hardware is an issue, made worst because of the dense construction of a supercomputer.

One may wonder why local drives are so much more common in cloud infrastructure. The constraints in cloud
infrastructure are different. 

-   Supercomputers, with the exception of some commercial clusters, are built to be as cost-effective as
    possible. So one tends to solve problems with better software rather than adding more hardware.

    Cloud infrastructures on the other hand are usually commercial offerings. They are built at a very
    large scale, with often an even more custom hardware design, and they are simply overprovisioned.
    E.g., a server may have two SSD drives where the software will simply switch over to the second\
    drive when the first one breaks, but the broken one will never be replaced.

-   The management model of a cloud infrastructure is also very different. Cloud is based on virtualisation
    technologies to isolate users, and let users built a virtual network of servers in which regular Linux
    access methods can be used. These layers of software add additional overhead (even with hardware features
    to support virtualisation) which is undesirable on a supercomputer where each additional 100 nanoseconds of
    communication delay may limit how far a job can scale on the computer. Note though that the virtualisation
    overhead, thanks to hardware support, has become low enough that small supercomputer jobs can run very
    well on some cloud infrastructures.


## Metadata

Each file contains both the actual data in the file, and so-called metadata such as the name of the file,
access rights to the file, the date of creation and of last use, ... 
This metadata is stored in a directory which on a traditional file system with folder and subfolder structure
is a special type of file for each (sub)directory. 
This implies that if you do many small disk accesses to files in the same directory, or store a lot of files
in a single directory and access them simultaneously, you create a bottleneck as many updates are needed 
to that directory. Most file systems are not very good at parallelising that directory access.

Bad patterns of metadata access are probably the most common source for performance problems on supercomputer
file systems. A typical scenario is when in a distributed memory application, each process creates its own
set of files in the same shared directory, rather than use a feature called parallel file I/O to create
a few giant files to which data is written in orchestrated multi-node operations. This can bring almost
every supercomputer file system to its knees. Building a file system that can cope with this load might
be technologically impossible or would at least make storage an order of magnitude more expensive.

An equally stupid idea is to open a file before every write and then close it again to ensure that data is
written to disk immediately. This is already an expensive operation on a PC with local storage that will
slow down your program a lot if those writes are very frequent, but it will kill almost any networked file
system and is even worse for the so-called parallel file systems on supercomputers as they tend to have
more expensive open and close operations.
