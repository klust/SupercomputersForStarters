# A storage revolution?

## Why are flash-based SSDs (not) the solution?

??? "University of Antwerp-specific"
    The joint bandwidth on the BeeGFS scratch file system at the CalcUA compute
    service in use in 2022 is on the order of 7 GB/s. At the same time,
    some NVMe SSDs for PCIe 4 also claim to offer a read bandwidth of 7 GB/s
    and a write bandwidth that is not that much lower, and the even newer
    PCIe 5 generation can be even faster (at least with the proper I/O 
    pattern as with a bad I/O pattern bandwidth can be as low as only 
    100 MB/s).

    So one could wonder if we shouldn't use 105 of those drives instead.

    There is of course the cost of the drives. But also a lot more server
    hardware would be needed simply to connect the drives and also to
    support the bandwidth over the interconnect.

The following table shows prices and properties for some drives in early 2022:


|                  | Seagate Exos X20        | Seagate Nytro 3732 | Seagate  Nytro 3332 | Samsung 980 Pro  | Samsung 970 EVO Plus | Samsung 870 QVO  |
|------------------|-------------------------|--------------------|---------------------|------------------|----------------------|------------------|
| Technology       | spinning magnetic disks | 3D eTLC NAND flash | 3D eTLC NAND flash  | TLC V-NAND flash | TLC V-NAND flash     | QLC V-NAND Flash |
| Market           | datacenter (SAS)        | datacenter (SAS)   | datacenter (2xSAS)  | prosumer (NVMe)  | consumer (NVMe)      | consumer (SATA)  |
| Capacity         | 20 TB                   | 3.2 TB             | 15.36 TB            | 2 TB             | 2 TB                 | 8 TB             |
| Read speed       | 0.28 GB/s               | 1.1 GB/s           | 1.05-2.1 GB/s       | 7 GB/s           | 3.5 GB/s             | 0.56 GB/s        |
| Write speed      | 0.28 GB/s               | 1 GB/s             | 0.95-1 GB/s         | 5.1 GB/s         | 3.3 GB/s             | 0.53 GB/s        |
| Latency          | 4,16 ms                 | 20 µs ???          | 20 µs ???           | 20 µs ???        | 20 µs ???            | 20 µs ???        |
| Endurance        | ?                       | 58.4 PB            | 28 PB               | 1.2 PB           | 1.2 PB               | 2.88 PB          |
| DWPD             | ?                       | 10                 | 1                   | 0,33             | 0.33                 | 0.2 (@5 year)    |
| Data written/day | ?                       | 32 TB/day          | 15.3 TB/day         | 0.66 TB/day      | 0.66 TB/day          | 1.5 TB/day       |
| Time needed      |                         | 8h50m              | 4h15m               | 2m9s             | 3m20 s               | 50 m             |
| Price            | 0.025-0.05 €/GB         | 0,85 €/GB          | 0,31 €/GB           | 0.16 €/GB        | 0.12 €/GB            | 0.08 €/GB        |

In this table we compare a popular high-quality hard drive for use in the datacenter with several
NAND flash based drives, ordered from the highest to the lowest quality measured in durability first
and speed second.

The Nytro 3732 is a very high endurance drive but only exists in relatively small capacities.
It uses a SAS interface which is very popular in servers as it is a good interface to build
large disk systems, where the drives are also further away from the CPU or in this case the
drive controller. The 3332 is a somewhat similar drive but with much higher capacity but lower
endurance. It has two SAS interfaces that can be used to get double the bandwidth.
The Samsung 980 Pro is a NVMe drive in M.2-format, meant to be put in a PCIe 4 slot on
the motherboard. This is a much faster interface than the one used in the two Nytro drives,
which also explains its very high speed. But it is also a less scalable interface as long
distance connections would be expensive, as are the switches that would be needed if the
CPU does ot provide enough PCIe connections itself. The Samsung 970 EVO Plus is a slightly
lower-end drive with a slower interface. All these drives use so-called TCL NAND, which
stands for Triple Level Cell NAND, meaning that it stores 3 bits per memory cell. 
The last drive, the Samsung 870 QVO differs in two aspects from the other SAMSUNG drives
and the datacenter drives: It uses QLC NAND, which stores 4 bits per cell, making it a bit
cheaper, but also uses a much slower interface, SATA, which is an older interface that was
very popular for hard disks in PCs. It also has a hard disk form factor and is not one that
you plug in on the motherboard as the other two Samsung drives.

For each of the drive series, we compare the larger capacity SKUs that one can get as after
all we are interested in building big storage systems and as they also tend to be relatively
cheaper. For SSDs, the larger capacity drives are sometimes also a bit faster than the lower
capacity ones in the same series. This is because a flash drive itself is already a highly
parallel device using parallelism over multiple banks of flash memory chips to increase
the speed. The smaller drives in a given series may not have enough banks of flash memory
to fully exploit all the parallelism that the controller chip of the drive (itself a
processor more powerful than those in the first smartphones) is capable of using.

The main weakness of hard drives and strength of flash drives becomes immediately clean
when we look at the rows for (peak) read and write speed and for latency: Hard disks have 
much lower peak read and write speeds and a latency that is orders of magnitude higher.
Note that we did not find precise latency numbers for the flash drives in the table,
but the numbers are a reasonable estimate based on other drives for which the data
was available. One should be careful interpreting the write bandwidth. Hard disks become
slower for both reading and writing as they fill up partly because of mechanical reasons as 
the outer zones of the drive are also used and the read and write heads have to move more,
and partly because of a phenomenon known as drive fragmentation, where data that belongs
together gets spread all over the disk instead of stored in a single zone of the disk. 
There is software to try to correct the latter. But SSDs also get slower the more data
is already on them, and this is more pronounced the more bits are stored per memory cell. 
But SSDs have another problem: Just as regular hard drives, data is written in blocks.
But data cannot be erased or overwritten in single blocks. Before overwriting data,
the block has to be erased, but this can only be done in clusters of blocks. Therefore,
if a drive is getting full and data is written and rewritten all the time, the drive
has to reorganise its storage all the time which can lead to write speeds that are 
much lower than on a well-organised hard disk. Some modern hard disks with very high capacity
also have a similar problem, but those are only used for archiving while slightly lower
capacity drives that don't have this problem are used for more active storage.
And another problem is with drives that store multiple bits per cell, which is currently
basically all drives. When the drive is rather empty and only one bit needs to be stored
per cell, write speeds are much higher than when the drive is filling up and 3 or 4 bits are
stored per cell.

The table does however clearly show another problem of SSDs: endurance. Unfortunately 
endurance for hard disks and SSDs is measure differently so that it is hard to compare
in the table. But basically, the Seagate Exos is suitable for near continuous reading
and writing and will last for the 5 years of its warranty period. For SSDs the story 
is very different. In the early years, an SSD memory cell could be erased and rewritten
several thousands of times without failing. However, both the miniaturisation and the use
of multiple bits per cell have severely lowered that life span. Some flash memory cell
can be erased and rewritten only 150 to 500 times without failing. Drives try to compensate
for that by very clever write and erase strategies and by keeping some spare storage on the
drive that can be used to replace worn out parts, so that a fixed size can be reported to
the operating system. This technique is called *wear levelling*. This complex management of
data on the drive to appear as a regular drive to the operating system is also one of the
reasons why flash drives have actually rather powerful processors. Endurance of SSDs is 
measured in Drive Writes Per Day, abbreviated DWPD. In the table above, these have all been
normalised to a 5-year life span of the drive. Another measure is the amount of data
that can be written per day to have a 5 year life span of the drive. It is obvious that the
larger the drive, the more data can be written also. We compare this with how long it would
take to write that daily capacity to the drive in the (invalid) assumption that we could 
write at the maximum write bandwidth. Now we see that the Nytro 3732 is a very good drive.
It supports 10 DWPD and even at its relatively small capacity this is still so much data per day 
that one could probably write almost continuously to it. It is certainly a drive suitable for
a scenario with high write loads, as is the case for supercomputer scratch storage.
For the Nytro 3332 which is 1 DWPD it is really only the capacity that saves us. It is slow
enough that one can still write a lot of data to it. But if we would use a smaller and cheaper
1 DWPD in, e.g., a compute node, we would probably run into problems as that local drive in the
compute node would typically be used to contain a temporary copy of large data sets, and hence 
have a lot of data written to it in each job. The Samsung NVMe drives are meant for a totally
different access scenario though. They can only sustain 0.33 DWPD, and if you could write that
data at the full write speed, this really means that you could only write to them for a few 
minutes per day. That high speed is certainly not meant to put a higher write load on the drives.
On the contrary, these drives are meant for use in PCs under a typical workload for a PC, where
much of the data on the drive is very static and consists of files that may be read often,
or are archived for long times. The QVO drive can sustain only 0.2 DWPD, but still ingest
quite a lot of data per day due to its high capacity. And due to its slow write speed due 
largely to the slow SATA interface, it also looks as if one can write for quite some time
to it, but also given how much quad level cells can slow down when filling up, it is really
a drive for archiving, and for storing data where the speed of the write operations doesn't
really matter.

This brings us to the last line of the table, the cost of the drives and here we see the 
big problem of SSDs. The cheaper Samsung drives may only be 2 to 5 times more expensive 
then the enterprise quality hard drive that we are comparing with, but this is really an
apples-and-oranges comparison. Those cheaper SSDs are not really suitable for datacenter use.
One could imagine building a storage system with high capacity for high read
load scenarios from the QVO drives, and some companies build such storage, but these drives
are not suited for the typical load on supercomputer file systems, neither for a local drive
or for, e.g., the scratch file system. Some SSDs are suitable (and the Nytro 3732 certainly
is) but then the cost is easily 10 times or more higher per byte than for good quality hard
drives. This is one of the reasons why SSDs are not used more in supercomputer storage.
After all, in supercomputer storage we need storage with a high capacity that can deal with
a high write load.

??? "Nice to know"
    Around 2010 a file storage expert at KU Leuven looked at how storage was used on their
    cluster to determine which type of storage should be bought. It turned out that the
    load was 90% write and only 10% read, which means that most of the data written to
    disk was even never read... It is hoped that this text makes clear that writing data
    is all but free.


## New memory types for a revolution?

As we can see from the previous discussion, flash memory comes with a higher price tag than
one would think from looking at prices for drives for PCs, and it has some other issues also,
like durability issues as each memory cell has a very short lifespan in terms of number of
rewrites, and the unpredictable slow-down especially under a random small writes load when the
drive starts to fill up.

Several companies have worked on other types of solid state memory for permanent storage
that does not have those issues. Two memory types have been considered that would
allow byte level access instead of block level, have much better write endurance and
that also could be rewritten at the byte level rather than having to erase whole blocks
and copying data round in the drive to free such a big block.
HPE (then still called HP) and Sandisk explored a type of memory cell called 
[*memristor*](https://en.wikipedia.org/wiki/Memristor). 
Intel and Micron worked together on a type of memory cell which they
called [*3D-XPoint*](https://en.wikipedia.org/wiki/3D_XPoint), 
sometimes marketed by Intel as Optane (though that name was later recycled for
other types of storage also). The memristor never made it to market. 3D-XPoint did
result in a couple of products, but they were only competitive for some very special markets.

3D-XPoint memory appeared in several forms. It was used to build datacenter solid state drives
with excellent endurance. Even though the I/O performance wasn't stellar on paper, the fine
print in the specs really mattered: 3D-XPoint drives coped much better with read and write
loads that lacked enough parallelism, while most flash drives must look at a larger number of 
read and write requests simultaneously and reorder them to get close to their quoted speed.
They were also used as a cache for flash drives, buffering writes and keeping some data that
was read a lot also available as in some cases it could be read faster from the 3D-XPoint cache.
And finally, it could also be plugged in some servers instead of RAM. However, it didn't act as 
RAM at all as it is still a lot slower than regular RAM, and as its write endurance doesn't come
close to that of RAM memory. Instead, it was marketed as memory for large database servers where
much of the database could be kept in 3D-XPoint memory yet accessed as if it were RAM, at a lower
cost as a fully RAM-equipped system and at a higher reliability in case of, e.g., power problems.
The last usage scenario is an example of so-called Storage Class Memory (sometimes abbreviated
as SCM): Memory that can operate as (slower) RAM but that just as regular disks storage maintains
its state when the system is powered off.

However, developing a new memory technology to compete with an already established and very far
developed technology is hard and requires extremely deep pockets. In the end, technological evolution
created good enough alternatives for many of the use cases of 3D-XPoint memory, or people simply
didn't see enough benefits to pay for it, and the development was stopped in 2021 by Micron and
2022 by Intel.

High-endurance SSDs are simply much cheaper than 3D-XPoint drives and are a good alternative for
all but a few cases where software has a really bad drive access pattern. But then for bigger companies
it is probably cheaper to simply rework the software to shine on cheaper drives. The benefit of 3D-XPoint
as a cache for SSDs was questionable because of the small size and also because of the way it was
implemented, increasing the complexity of the system, and nowadays some drive use some SLC (single bit per cell)
flash memory as a cache. The RAM capacity per socket has also increased a lot with more memory channels
per socket and larger memory modules. 

Another technology that would allow larger RAM memories is also coming up. 
*Compute eXpress Link* (\CXL) is an open standard that build upon the PCIe standards to provide
an interface that would be suitable for connecting several kinds of components in large systems:
CPUs, GPUs, other accelerators with compute capability, additional RAM memory, ... It also builds
on the experience with other technologies, as IBM's OpenCAPI, that tried to reach some of these
goals though it is not compatible with any of those. It would even be possible to build networks
of CXL connections to build a reconfigurable computer: A user can select the number of CPUs, GPUs, memory
blocks, etc., and those are connected on the fly the the CXL fabric. 

This may sound nice but it remains to be seen how useful this will be in practice. It is not possible
to make very large switched fabrics as the latency would simply be too large to use this in a way 
current memory and accelerators are used. On the contrary, as we shall also see in 
[the chapter on accelerators](../C08_Accelerators/index.md) and as we have already seen to some extent
in the [chapter on memory technology](../C03_Memory/index.md), the needs for many supercomputer applications
but also regular applications are exactly the opposite. Large memories are useless if they also come with
much higher latency unless applications are reworked to hide the latency and make clever use of the 
memory hierarchy with nearby faster RAM and more remote larger but slower RAM. As we shall see, the 
performance improvement that one can obtain from using accelerators can also be limited by the data
transfers to and from the accelerators. Not all applications can be reworked to cope with the much higher
latency in such a CXL-based reconfigurable system or even just an ordinary server with a large bank of 
slower memory. In fact, many applications need in fact the opposite, a much closer integration of 
memory, CPU and accelerators. This is precisely the reason why, e.g., the Apple M-series processors 
sometimes provide much better performance than one would expect from the chip in applications.


## Local storage in supercomputer nodes

As the speed difference between the processing capacity of a supercomputer node and the storage
keeps increasing, there is a renewed interest in adding local storage again to compute nodes,
something that certainly the large supercomputers avoided because of reliability and management
issues.

Modern high-end SSDs have become fairly reliable and as shapes have mostly standardised, it 
does become possible to build them into water cooled nodes without having a negative impact 
on the cooling of other components or a performance impact because of a too high temperature.

Manufacturers are also working on software to make them more manageable in a supercomputer
context and more useful also to parallel programs as those local SSDs cannot be accessed
directly from other compute nodes.

[Intel DAOS](https://docs.daos.io/) was originally developed for the much delayed USA Aurora
exascale system where it would work with 3D-XPoint drives in the compute nodes. It is also 
designed to integrate with the Lustre file system that will be used on Aurora. It is not clear
how Intel envisions using DAOS though as it does rely on storage class memory and not only
NVMe drives, and was really designed with 3D-XPoint in mind and its server processors with
built-in support for that memory.

HPE is working a what they call a 
[near-node storage system code-named Rabbits](https://www.hpcwire.com/2021/02/18/livermores-el-capitan-supercomputer-hpe-rabbit-storage-nodes/)
for the third USA exascale computer, El Capitan. 
It consists of a storage server that sits close to a number of compute nodes with
fast dedicated PCIe connection to each of them. The server has its own processor so can
work independently from the compute nodes to, e.g., transfer data that was written
by the job to the larger remote Lustre file system. Each server has 16 SSDs but also two
spares so that it can reconfigure automatically when an SSD fails. These SSDs can be
accessed as if they are a directly attached drive, essentially operating as an SSD in
the node, or as a network drive acting as a cache to the larger remote Lustre file
system. It will work in conjunction with a new scheduler as Slurm cannot easily be
made sufficiently aware of the architecture of the attached software to manage it and
allocate proper resources.
