# To remember from this chapter

Supercomputer file systems, just as their compute capacity, are build from relatively
standard but better reliability components. Their speed does not come from novel physics
or so but just from clever software.

Supercomputers like large files and large reads or writes. Just as with memory, streaming
data to and from a file is much faster than random access to data.
It may in fact seem that a PC is less sensitive to this but this is only partially true.
Even a PC SSD will have much higher bandwidth when streaming sufficiently large amounts
of data to it then when using random access inside files and to many files.
It is certainly essential to avoid writing many small files. E.g., when running 1000s of
small jobs for a parameter study, it would be much better to accumulate the data in 
fewer files and/or to use a database to process the results.

Opening and closing files is also very expensive on a parallel file system and should
be avoided where possible.

A good step towards improved use of supercomputer file systems is to use appropriate 
data formats. Libraries such as
[HDF5](https://www.hdfgroup.org/solutions/hdf5/),
[netCDF](https://www.unidata.ucar.edu/software/netcdf/),
[ADIOS](https://csmd.ornl.gov/adios)
and [SIONlib](https://apps.fz-juelich.de/jsc/sionlib/docu/index.html)
help to organise large amounts of data in a small number of files that work well
on supercomputers. 
For some compression formats libraries exist that can be used to read data from the
compressed archive directly into memory which may already improve performance when\
a lot of small files would otherwise be read in, but of course it requires adapting
the code to use such a library rather than the functions calls to read from the 
file system.
You can think of such libraries as creating a hierarchy to manage the complexity: 
the file system manages datasets as a single file for each dataset, and the file
library creates a kind of a file system in the application to store the different 
data objects that compose the dataset, also in a format that can be optimised for
that specific dataset.

Another remark that we have not yet really discussed is that it is not a good idea to
read or write numeric data in text (ASCII or UTF) format. There is hardly a portability
advantage to using text to store numbers as binary data formats have mostly been standardised
with really two options remaining and many libraries and tools supporting both (little endian
and big endian which defines the byte ordering for multi-byte numbers, but almost all CPUs
nowadays use little endian). Storing numbers as ASCII text expands the needed capacity with a
factor of approximately three and the conversion to and from binary data is very slow.

When discussing scaling a storage system from the size of a PC storage system to the size of a 
supercomputer storage system, one should realise:

-   Scaling capacity is cheap. Often one only needs to add disks and disk enclosures and not so
    much servers when using drives with interfaces that are specifically made for that (and in
    particular SAS drives as we have seen in the drive technology comparison table).

-   Scaling bandwidth is harder and more expensive. Simply adding disks is not enough as we need
    more bandwidth between the disks and the servers and more bandwidth from the server to 
    the interconnect. As both are limited, we will need to add more servers.

    Joint bandwidth to a number of files is also easier to scale (at least if those files are spread
    over the whole system and a sufficient number of requests come in in parallel). The strength of many
    supercomputers is that they also offer extremely scalable bandwidth to large files. But this also
    requires large parallel applications using a properly written parallel I/O framework.

-   Scaling the number of I/O operations that the system can process per second (called IOPS) is
    extremely hard and expensive and is sometimes even physically impossible. This has several causes.
    
    -   Metadata access is much harder to parallelise, especially for access to a single (sub)directory by a single user.

    -   Due to the larger physical and logical distance of the application to the storage, the latency
        of I/O operations will be one or more magnitudes higher. This also means that if the I/O operations
        are launched sequentially and one has to finish before the next one can start, the application will
        run a lot slower than on a PC with fast local storage.

        In fact, it is doable to make supercomputer storage with very high IOPS when there are a lot of 
        parallel I/O requests, from many users to files all over the file system so that metadata and data
        access can be spread over many servers. But it is not possible to improve IOPS for a single threaded
        application with synchronous file access.




