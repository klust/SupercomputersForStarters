# Parallel file systems

On PCs and many more regular file server file systems, each block on disk can 
be used for either metadata or data. (Well, in practice there will be a zone
that is used exclusively for metadata but that zone is extended transparently
when needed.) These file systems are very flexible and support small file
sizes very well. But at the same time it is very difficult to ge very high
bandwidth, as a file will be managed by a single storage server, so even with
the fastest and most expensive storage attached to the storage servers, the 
single server would ultimately be a performance bottleneck.

Larger supercomputers need a different kind of file system for higher performance,
one where multiple servers can be used concurrently to access a single file.
This is called a parallel file system. 

??? "Popular examples and their use at the VSC"
    -   [Lustre](https://www.lustre.org/) is probably the most used file parallel file system on large supercomputers.
        It is used at KU Leuven, the VSC Tier-1 system Hortense at UGent and on the 
        [LUMI](https://lumi-supercomputer.eu/) system in Finland, a pre-exascale
        supercomputer project to which Belgium and Flanders participate.

        Lustre uses two layers. It is implemented on top of another file system that
        is used for the actual disk accesses.

        Lustre is open-source but with companies offering support and doing most of the
        development.

    -   [BeeGFS](https://www.beegfs.io/) has a very similar architecture as Lustre
        and is used for the scratch file system on the clusters of the University
        of Antwerp.

        BeeGFS is open-source, but most of the development is done by 
        a spin-off of a German research laboratory, and they also offer (paid) support.

    -   In the early days of the VSC, [IBM Spectrum Scale](https://www.ibm.com/products/spectrum-scale),
        then known as [GPFS](https://www.ibm.com/docs/en/gpfs/4.1.0.4?topic=guide-introducing-general-parallel-file-system),
        was used on all clusters. However, increasing licensing costs made this impossible.

        Spectrum Scale/GPFS doesn't have the same two-layer architecture as Lustre or BeeGFS (or at least,
        it is not visible), but does the full managment of the disks itself.

    -   [Panasas PanFS](https://www.panasas.com/) is a storage system that has its roots in the
        same research project as Lustre. It is a commercial offering consisting of software and
        dedicated hardware.

    -   [WEKA](https://www.weka.io/) is also a fully commercial offering, but one running on more
        standard file server hardware. It requires a full SSD system though which makes it a rather
        expensive offering.

In a parallel file system, the metadata is separated from the actual data. The metadata servers 
take care of all metadata operations, which include controlling the process of opening and closing
files. However, after that the content of the file can be served by multiple servers, called the
*object servers* in Lustre and BeeGFS (but not to be confuse with object storage such as Amazon S3
or Ceph). Each object server is responsible for certain parts of the file, but a big read or write
access can engage multiple servers simultaneously. Parallel file systems will use their own client
software to access the file storage. The metadata server(s) will pass all information that is needed
to the clients on the nodes involved with the file access, so that the clients can then directly
talk to the object servers that are involved with the data transfer. But since the metadata servers
are not involved with the actual data transfer, it should be clear that opening and closing a file 
is a more expensive operation, as the metadata server has to pass the necessary information to the
client(s) when opening a file and orchestrate the opening and closing of the file with the object servers.

Such a setup can produce very high bandwidth for large read and write operations coming from 
optimised parallel software using the right libraries to optimise that data transport, and this
at a very reasonable cost. However, it has trouble dealing with lots of small files, and
metadata access, certainly to files in one directory, can be a bottleneck. Moreover, the
amount of storage space for metadata is determined when the system is purchased or 
configured as it is on separate servers, so there is also a strict limit to the number
of files such a system can store.

It is not uncommon nowadays for supercomputer centres to impose strict limits on what users
can do on the shared file systems. Just as software needs to adapt to the processing model
of a supercomputer, software also needs to adapt to the way large storage systems on supercomputers
work. You cannot simply run hundreds of instances of that naive PC program that accesses thousands
of files in a short time on your supercomputer, but you have to organise your data in a better
way to scale to supercomputer storage.

??? "The storage on the clusters at the University of Antwerp"
    At the CalcUA supercomputer service, we have different storage systems. The Linux home
    directory is kept small deliberately so that it can be served from a few SSD drives in
    a very high reliability setup to prevent data loss. The system is small, hence a broken
    drive should not break the bank, while there are clear advantages to using SSD drives for
    home directories. The home directories are on a regular Linux file system served to the 
    compute nodes via NFS as that better fits the typical data access pattern to the home
    directory than a parallel file system.

    The centrally installed applications are also installed on SSD drives. As program files 
    are relatively small to very small and as packages like Python and R need thousands of 
    very small files, it is served by a regular Linux file system exported to the compute
    nodes via NFS as this setup is better at dealing with small files than a parallel file
    system, certainly on a cluster the size of the one at the University of Antwerp.

    Next there is a 50TB file system on regular hard drives in a RAID setup with two
    redundant drives for every 8 drives. It uses a regular Linux file system served
    through NFS. We made this choice to also have a somewhat larger file system that
    can deal better with small files than a parallel file system can. It is, e.g., a 
    good file system for users who want to build their own Python or R installation.

    The largest file system is a roughly 0.6 PB scratch file system. That uses 105
    regular hard drives spread over 7 object servers for the storage of the data,
    and a number of SSDs for the metadata. The low latency of SSDs is very important
    to get good performance from the metadata servers, and again a broken SSD won't
    break the bank as there are only few. But for the object servers hard disks are 
    used as they are an order of magnitude cheaper than SSDs. 

