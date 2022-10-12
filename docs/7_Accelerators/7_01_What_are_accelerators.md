# What are accelerators?

## In short 

We restrict ourselves to the most common types of compute accelerators used in 
supercomputing and do not cover, e.g., accelerators in the network to process
certain MPI calls.

An accelerator is usually a coprocessor that accelerates some computations that 
a CPU might be capable of doing, but that can be done much faster on more specialised
hardware, so that it becomes interesting to offload that work to specialised hardware. 
An accelerator is not a full-featured general purpose processor that can run a regular 
operating system, etc. Hence an accelerator always has to work together with the regular
CPU of the system, which can lead to very complicated programming with a **host program**
that then **offloads** certain routines to the accelerator, and also needs to manage the
transport of data to and from the accelerator.


## Some history

Accelerators have been around for a long time in large computers, and in particular in
mainframes, very specialised machines mostly used for administrative work.

However, accelerators also appeared in the PC world starting in the '90s. 
The first true programmable accelerators probably appeared in the form of high-end sound cards. 
They contained one or more so-called Digital Signal Processor (DSP) chips for all
the digital processing of the sound. 

Graphic cards were originally very specialised fixed-function hardware that was not
really programmable but this changed in the early '00s with graphics cards as the
NVIDIA GeForce 3 and ATI Radeon 9300 (ATI was later acquired by AMD which still uses the
Radeon brand). It didn't take long before scientist looking for more and cheaper compute
power took note and started experimenting with using that programmability to accelerate
certain scientific computations. Manufacturers, and certainly NVIDIA, took note and started
adding features specifically for broader use. This led to the birth of NVIDIA CUDA 1.0 in 
2007, the first successful platform and programming model for programming graphics cards that
were now called Graphics Processing Units (or GPU) as they became real programmable processors.
And the term GPGPU for General-Purpose GPU is also used for hardware that is particularly
suited to be used for non-graphics work also. GPGPU programming quickly became popular,
and even a bit overhyped, as not all applications are suitable for GPGPU computing.


### Types of accelerators

The most popular type of accelerators are accelerators for **vector computing**. All modern
GPUs fall in this family. Examples are

-   NVIDIA Data Center series (previously also called the Tesla series). These started of as basically
    a more reliable version of the NVIDIA GeForce and Quadro GPUs, but currently, with the Ada Lovelace
    GPUs and Hopper GPGPUs these lines start to diverge a bit (and even before, the cards really meant
    for supercomputing had more hardware on them for double precision floating point computations).
    Strictly speaking the NVIDIA architecture is a single instruction multiple data (SIMD) architecture
    but not one that uses explicit vector instructions, but the resulting capabilities are not really 
    different from more regular vector computers (but then vector computers with an instruction set that
    also support scatter/gather instructions and predication, features that are missing from, e.g., 
    the AVX/AVX2 instruction set).

-   AMD has the Instinct series for GPGPU computing. They employ a separate architecture for their 
    compute cards, called CDNA, while their current graphics cards use various generations of the RDNA
    architecture. The CDNA architecture is a further evolution of their previous graphics architecture
    GCN though (used in, e.g., the Vega cards).

    AMD Instinct GPUs are used in the first USA exaflop computer Frontier (fastest system in the Top500
    ranking of June 2022) and in the European LUMI system (fastest European system in the Top500 ranking
    of June 2022). These computers use the CDNA2 architecture. A future USA exascale system, El Capitan, 
    planned for 2023 (or possibly 2024 with the supply chain disruptions largely due to Covid), will
    employ a future version of the architecture, RDNA3, which will bring CPU and GPU very close together.

-   Intel is also moving into the market of GPGPU computing with their Xe graphics products. They
    have supported using their integrated GPUs for computations for many years already, with even support
    in their C/C++/Fortran compilers, but are now making a separate products for the supercomputing market
    with the Xe<sup>HPC</sup> product lines which support additional data formats that are very needed for
    scientific computing applications. The first product in this line is the GPU code named Ponte Vecchio that
    will be used in the USA Aurora supercomputer, which should become the second USA exaflop computer.
    A future European pre-exascale system will also have a compute section with the successor of that 
    chip, Rialto Bridge.

-   The NEC SX Aurora TSUBASA has a more traditional vector computing architecture, but is physically also
    an expansion card that is put in a regular Intel-compatible server. It is special in the sense that the
    original idea was that applications would fully run on the vector processor and hence not use a host 
    programming with offloading, while under the hood the OS libraries would offload OS operations to the
    Intel-compatible server, but in practice it is more and more used as a regular accelerator with a 
    host program running on the Intel-compatible server offloading work to the vector processor.

A second type of accelerator that became very popular recently, are accelerators for **matrix operations**,
and in particular matrix multiplication or rank-k update. They were originally designed to speed up operations
in certain types of neural networks, but were quickly gained support for additional data types that makes them
useful for a range of AI and other HPC applications. Some are integrated on GPGPUs while others are specialised
accelerators. The ones integrated in GPUs are most popular for supercomputing though:

-   NVIDIA Tensor cores in the V100 and later generations.

-   AMD matrix cores in the MI100 and later chips. The MI200 generation may be a little behind
    the similar-generation A100 NVIDIA cards when it comes to low-precision formats used in some
    AI applications, but it shines in higher-precision data formats (single and double precision
    floating point).

-   Intel includes their so-called Matrix Engines in the Ponte Vecchio GPGPUs.

-   The NVIDIA tensor cores, AMD matrix cores and Intel matrix engines are all integrated very closely with
    the vector processing hardware on their GPGPUs, However, there are also dedicated matrix computing accelerators,
    in particular in accelerators specifically designed for AI, such as the Google TPU (Tensor Processing Unit).

    Most neural network accelerators on smartphone processors also fall in this category as they are usually
    in a physically distinct area from the GPU hardware in the SOC (though not a separate chip or die).

A third and so far less popular accelerator in supercomputing is an FPGA accelerator, which stands for
Field Programmable Gate Array. This is hardware designed to be fully configured after manufacturing,
allowing the user to create a specialised processor for their application. One could, e.g., imagine creating
specialised 2-bit CPUs for working with generic data. 
