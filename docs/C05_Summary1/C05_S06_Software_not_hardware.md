# Software first, not hardware

It is the software more than the hardware that defines a supercomputer.

In the early 60s computers were still mostly build from individual transistors.
There were already smaller slower and bigger faster computers, but architectures
differed vastly. Powers of two for number of bits, or binary number representations
were not yet standard. The first programming languages, Fortran for technical computing
and Cobol for business computing, already existed.

In the late 60s and early 70s computer were build from 100s or 1000s of small
integrated circuits (though some already appeared earlier). In this era
we saw smaller minicomputers and large mainframes. The minicomputers were 
more scaled-down versions of the mainframes.

By the second half of the 70s a very specific supercomputer architecture that was
not very well suited for general computing appeared on the market: The vector computers
built by Cray and CDC. Specialised hardware meant that software also had to be
adapted: The compilers did need some assistance to generate proper vector code,
and the first numeric libraries that helped exploit the vector architecture also
appeared. 

By the second half of the 80s the next big paradigm shift showed up. It had become 
prohibitively expensive to design supercomputer hardware. Vector machines were still
fairly popular, but keeping developing them had become very expensive, while a market
for smaller workstations that offered decent performance for the time appeared. 
Hence research labs and Intel started to experiment with building supercomputers that would
reuse some of that workstation hardware. The first computer of that kind was probably
the Cosmic Cube developed at Caltech which was based not even on a workstation
processor but the two-chip 8086/8087 combo that was also used in personal computers.
64 of those were linked together using a special-purpose network. This research later
led to the Intel iPSC product line of supercomputer built out of regular 80386 and later
the Intel i860 RISC processors. Other manufacturers also picked up the idea, e.g.,
IBM with the SP line using processors developed for their POWER workstations in 1993
(IBM also built a vector computer in the late 80s). By 1995 distributed memory 
computers based on processors designed for workstations or PCs already took a lot
of the top spots in the Top 500 list, a list of fastest supercomputers on the 
Linpack benchmark. Supercomputer manufacturers differentiated more in their
designs for the interconnect and in the software than in the processor hardware
as the latter was shared with less capable workstations or even came from another
manufacturer. 

This trend only became even more pronounced from the late 90s on. These are the days
when Linux started to become more popular among researchers and when Intel processors
for PCs had fully caught up with typical workstation processors in terms of performance,
gradually pushing the latter out of the market as the growing design costs had to
be amortised on a too low volume. 
Modern supercomputers try to minimise the hardware cost by reusing technologies 
that have other larger volume applications also and in some cases even reusing 
more volume hardware. Even custom network technologies, for a long time the feature
that vendors used to distinguish their supercomputers from more regular computers,
 have largely disappeared
from the market in favour of InfiniBand, a network technology originally designed for
other applications in the data centre. Cray and Fujitsu are notable exceptions, both still
investing in their home-grown interconnect technologies and still doing so today,
though Cray has been bought by HPE. One can argue though that the most recent Cray network
technology, called SlingShot, is largely derived from Ethernet with some customisations that 
are in fact also partly software. More than ever before does the system and application 
software make the supercomputer: Programming models implemented in libraries and
compilers to deal with distributed computing, parallel file systems to turn huge arrays
of disks and servers into a high bandwidth file system capable of efficiently serving
multi-terabyte data files to applications, applications that exploit all levels of
parallelism and the hierarchical structure of modern supercomputers, ... 
In fact, without all that software a high-end gaming PC could very well be faster for 
your application than a supercomputer!

This evolution is only normal. Designing hardware is expensive and hardware also needs 
a lot of testing before it can be brought to market as many errors are hard to correct
afterwards. Moreover there is also a high production cost associated with hardware,
and the cost is higher for hardware that can only be used in small volumes. 
Software may not be cheap to design either, but it is easier to bring to market as it
is easy to correct errors afterwards, and the cost of distributing it is low compared to
the costs for hardware. As it is easy to continue improving software it is possible to
upgrade a system and make it better during its lifetime.

It is also largely the software that ensures that a supercomputer is still a different infrastructure
from a server farm or a cloud infrastructure (though there is one element, the interconnect, that 
still remains very important and tends to differ from those infrastructures also).
Supercomputers focus on latency and staying close to "bare metal" to be able to get the
maximum performance out of the hardware and to enable thousands of processors to work 
together on solving big problems that cannot be split up in hundreds of almost independent
tasks that can be executed in parallel. Supercomputers focus on scalability for capability
applications and everything that compromises that scalability is not implemented.
Cloud infrastructures on the other hand focus more
on isolating even small users from one another, security and the creation of a personal 
environment. They are typically only suited for fairly coarse grained parallelism and 
applications where different tasks that can be executed in parallel are fairly independent
of each other (think of the thousands of requests coming in on web servers), though
there are cloud infrastructures that are good enough to build small virtual clusters
capable of running reasonably sized simulations.
Cloud infrastructures may be rather dense, but supercomputers tend to be built even more
dense to reduce costs even more (the cables connecting parts are very expensive),
and improve latencies as much as possible. But that density also comes with new restrictions.
E.g., local disks are not only a management problem but in fact it becomes physically
difficult to add them to a node and ensure that all components get proper cooling.
Supercomputers and cloud infrastructures have also very different exploitation models.
That partly results from the different type of environment they intend to offer, but is also 
partly the result of the fact that many supercomputers are installed in supercomputer centres
that serve mostly academia or in academic institutions themselves, while the largest cloud
infrastructures are commercial. Users think completely differently about resource use if they
get an allocation upfront and don't see the bill then when they actually have to pay full cost
for resources at a commercial cloud infrastructure. 
Supercomputers tend to focus on a rapid succession of jobs of different users, relying mostly on
shared storage as managing bare-metal node based storage in a multi-user environment can be
hard. Modern Linux may offer some low-overhead solutions to ease some of those problems, but for some
reason they have not become popular yet on supercomputers. The focus on rapid succession of jobs
is only normal on a cluster where compute time is not billed in terms of money as there is not
enough incentive for a user to carefully consider if it makes sense to keep the same resources
unused for a short while to save on some other startup cost instead. This is different on a cloud
infrastructure where there is a financial incentive to think economically and where it is not 
uncommon to keep some servers a bit longer to, e.g., save on the data transport cost to bring
data from a remote data store service to a local (virtualised) disk. The full virtualisation 
that is often used also makes it a lot easier for the system to clean up afterwards, making
it easier to offer local storage in a manageable way.

<!-- Censored by Maria Girone <Maria.Girone@cern.ch> in the LUMI texts where a very similar text was used. -->
??? Example "Case Study: Bringing CERN LHC computations to an HPC infrastructure."
    This example illustrates that cloud and HPC infrastructures are different and that
    moving from a cloud infrastructure to an HPC infrastructure may require thinking a lot
    about the software and the way data is handled.

    CERN came telling on a EuroHPC Summit Week before the COVID pandemic that they would start using more
    HPC and less cloud and that they expected a 40% cost reduction that way.
    At that time they were working with CSCS to bring part of the computations to the 
    Piz Daint system, a very large Cray supercomputer. 
    It turned out to be a lot harder than expected, to quote from the CSCS website:
    *["The data access patterns and rates of our workflows are not typical of a supercomputer environment. 
    In addition, for some workflows, certain resource requirements exceed what a 
    general-purpose supercomputer typically provides; a lot of tuning therefore needs to be put in place."](https://www.cscs.ch/science/physics/2019/piz-daint-takes-on-tier-2-function-in-worldwide-lhc-computing-grid)*
    And in the end the conclusion was that 
    *[""Piz Daint" is slightly more cost-effective."](https://www.cscs.ch/science/physics/2019/piz-daint-takes-on-tier-2-function-in-worldwide-lhc-computing-grid).*

    Several publications show the work that needed to be done. E.g.,
    [F.G. Sciacca on behalf of the ATLAS Collaboration, "Enabling ATLAS big data processing on Piz Daint at CSCS", EPJ Web of Conferences **245**, 09005(2020)](https://doi.org/10.1051/epjconf/202024509005)
    shows that in the end a dedicated partition had to be created as the needs for the LHC
    processing were to different to fit in the regular HPC compute partitions of Piz Daint.
    To quote from the paper, *["A large part of the codebase, like event generators and detector
    simulation toolkits feature legacy code that has historically been developed according to the serial
    paradigm: events are processed serially on a single thread and embarrassingly parallel processing
    occurs for scalability purposes in multi-processor systems. HPC systems, on the contrary are
    usually optimised for scalable parallel software that exploits the tight interconnection between
    nodes and makes use of accelerators. In addition, network and I/O patterns diﬀer greatly from
    those of HEP workloads. This raises the necessity of adapting the HPC to such aspects of the
    HEP computational environment."](https://doi.org/10.1051/epjconf/202024509005)*

    Additional hardware and system software needed to be brought in, including servers to bring 
    CernVM-FS to the compute nodes in a way compatible with the Cray environment of Piz Daint.
    Large Cray systems limit the number of Linux daemons running on the compute nodes as the
    so-called "OS jitter" from those daemons can limit scalability. 
    Hence only the Lustre parallel filesystem is available
    as a remote filesystem on the compute nodes, while other remote file systems are served through
    a so-called Data Virtualisation Service (DVS), forwarding all requests to a set of management 
    nodes that run the actual filesystem software. 
    A specifically tuned GPFS file system also needed to be installed (and also needed additional
    hardware) as it turned out to perform better than Lustre under the load of the CERN applications.
    A significant portion of the cost advantage of an HPC infrastructure was lost due to the 
    cost of all the customizations and additional hardware.

    It is clear that a center can only adapt an HPC system to such applications if there is a sufficient
    additional budget, not only for the hardware but also for the additional system administration tasks,
    and that the more usual case for all but extremely large projects is that the workflow has
    to adapt to the HPC cluster as even if money and resources would not be a problem, it is not feasible to make
    (likely conflicting) modifications to an infrastructure for each project. And a dedicated partition for each
    application is not an option either as that makes it impossible to run large scalable applications
    at the scale of the full machine. The latter may not be a huge issue for a Tier-2 system in the VSC
    (but it could be from a management point of view) as users who want to run big jobs can move up
    to larger systems, 
    but it is an issue for supercomputers that are built in the first place to enable very large jobs,
    as to some extent the Flemish Tier-1 systems but even more the European-level Tier-0 supercomputers,
    as moving elsewhere to an even bigger machine is then impossible.
<!-- END censored text -->


