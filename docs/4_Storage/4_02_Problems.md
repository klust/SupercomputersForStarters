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



