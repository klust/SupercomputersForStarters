---
tags:
-   CUDA
-   vector accelerator
-   matrix accelerator
-   FPGA
---

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


## Types of accelerators

The most popular type of accelerators are accelerators for **vector computing**. All modern
GPUs fall in this family. Examples are

-   NVIDIA Data Center series (previously also called the Tesla series). These started of as basically
    a more reliable version of the NVIDIA GeForce and Quadro GPUs, but currently, with the Ada Lovelace
    GPUs and Hopper GPGPUs, these lines start to diverge a bit (and even before, the cards really meant
    for supercomputing had more hardware on them for double precision floating point computations).
    Strictly speaking the NVIDIA architecture is a single instruction multiple data (SIMD) architecture
    that does not use explicit vector instructions, but scalar instructions that have to be executed 
    in lock-step across multiple "threads".
    The resulting capabilities are not really 
    different from more regular vector computers (but then vector computers with an instruction set that
    also support scatter/gather instructions and predication, features that are missing from, e.g., 
    the AVX/AVX2 instruction set).

-   AMD has the Instinct series for GPGPU computing. They employ a separate architecture for their 
    compute cards, called CDNA, while their current graphics cards use various generations of the RDNA
    architecture. The CDNA architecture is a further evolution of their previous graphics architecture
    GCN though (used in, e.g., the Vega cards).

    AMD Instinct GPUs are used in the first USA exaflop computer Frontier (fastest system in the Top500
    ranking from June 2022 till June 2024) 
    and in the European LUMI system (fastest European system in the Top500 ranking
    from June 2022 till June 2024). 
    These computers use the CDNA2 architecture. The latest USA exascale system
    and fastest system in the Top500 list of November 2024, El Capitan, 
    employs the CDNA3, launched in December 2023, which brings CPU and GPU very close together in one 
    of the variants and the variant used in El Capitan
    (but more about this later in this section).
    HLRS (Stuttgart, Germany) is also investing in this technology for its Hunter system which is a
    development system for an upcoming exascale system based on a yet unannounced GPU.

-   Intel is also moving into the market of GPGPU computing with their Xe graphics products. They
    have supported GPU computing using their integrated GPUs for computations for many years already, with even support
    in their C/C++/Fortran compilers, but are now making a separate product for the supercomputing market
    with the Intel Data Center GPU MAX series based on the Xe<sup>HPC</sup> architecture,
    which support additional data formats that are very needed for
    scientific computing applications. The first product in this line is 
    is also known as the GPU code named Ponte Vecchio that
    is used in the USA Aurora supercomputer, the second USA exaflop computer
    (albeit slightly slower than Frontier, the first one).
    One of the European pre-exascale systems was planned to have a compute section with the successor of that 
    chip, code named Rialto Bridge, so that there would be large systems in Europe with NVIDIA, AMD
    and Intel GPUs, but as that chip is cancelled they switch to NVIDIA GPUs.

-   The NEC SX Aurora TSUBASA has a more traditional vector computing architecture, but is physically also
    an expansion card that is put in a regular Intel-compatible server. It is special in the sense that the
    original idea was that applications would fully run on the vector processor and hence not use a host 
    programming with offloading, while under the hood the OS libraries would offload OS operations to the
    Intel-compatible server, but in practice it is more and more used as a regular accelerator with a 
    host program running on the Intel-compatible server offloading work to the vector processor.

A second type of accelerator that became very popular recently, are accelerators for **matrix operations**,
and in particular matrix multiplication or rank-k update. They were originally designed to speed up operations in certain types of neural networks, but quickly gained support for additional data types that makes them useful for a range of AI and other HPC applications. Some are integrated on GPGPUs while others are specialised accelerators. The ones integrated in GPUs are the most popular for supercomputing though:

-   NVIDIA Tensor cores in the V100 (Volta generation) and later generations (Ampere, Turing and Hopper).

-   AMD matrix cores in the MI100 and later chips. The MI200 generation may be a little behind
    the similar-generation A100 NVIDIA cards when it comes to low-precision formats used in some
    AI applications, but it shines in higher-precision data formats (single and double precision
    floating point), as it was developed in the first place for the needs for the Frontier
    exascale simulation whose procurement predated the days were AI became very popular.
    The MI300 generation launched in December 2023 is very competitive with the
    NVIDIA H100 and H200 though, also for lower precision data formats.

-   Intel includes their so-called Matrix Engines in the Ponte Vecchio GPGPUs.

-   The NVIDIA tensor cores, AMD matrix cores and Intel matrix engines are all integrated very closely with
    the vector processing hardware on their GPGPUs, However, there are also dedicated matrix computing accelerators,
    in particular in accelerators specifically designed for AI, such as the Google TPU (Tensor Processing Unit).


??? Note "Neural network accelerators in smartphones and PCs"
    Some neural network accelerators on smartphone processors also fall in this category as they are usually
    in a physically distinct area from the GPU hardware in the SOC (though not a separate chip or die).
    Not all neural network accelerators are of the matrix computing type though. E.g., the AMD XDNA
    architecture, used in some Ryzen chips, is based on vector processors.

A third and so far less popular accelerator in supercomputing is an FPGA accelerator, which stands for
Field Programmable Gate Array. This is hardware designed to be fully configured after manufacturing,
allowing the user to create a specialised processor for their application. One could, e.g., imagine creating
specialised 2-bit CPUs for working with generic data. 

### A note on the name "GPU"

Though usually called GPU computing or GPGPU computing, the accelerators still have an architecture 
that is an evolution of that of the programmable GPUs from 2010 and later, 
but often lack the full hardware rendering pipeline
that one would expect on a true GPU. The increasing demand for performance while the cost per transistor
tends to stay flat or even increase a bit and while new semiconductor manufacturing processes don't deliver
the gains they used to 15 years ago, resulted in an evolution towards distinct "GPUs" for compute (traditional HPC and AI) and
graphics rendering. We've outlined this already a bit when discussing the types of accelerators earlier
on this page. It also implies that compute GPUs do not always support typical graphics software stacks 
such as OpenGL or Vulcan, or that part of these stacks have to rely on software-based rendering accelerated
by the vector functions of the GPU rather than full hardware rendering.

The NVIDIA line-up has had different cards for compute and rendering for quite a while already. 
The Volta architecture launched in 2017, which was the first one to offer the tensor cores for AI, 
was used in a few high-end video cards, but the Turing architecture launched a year later was the
main architecture for rendering GPUs (though that one in turn was also used in some compute cards).
The Ampere generation succeeded both in 2020, but with distinct chips for compute (the A100) and
for rendering (with the GA102 as the most powerful one) and distinct differences between
both chips. E.g., the A100 had much more FP64 hardware and more tensor hardware, 
while the GA102 had ray tracing cores and offered much more regular precision CUDA cores,
even though it had only half as many transistors and a 30% smaller die in a slightly larger 
process node. 
The NVIDIA Hopper compute GPUs and Ada Lovelace rendering GPUs launched in late 2022
were clearly designed together and
share some characteristics, but are still very different beasts, this time emphasised by
different code names. 
The ray tracing units present in the Ada Lovelace architecture are not in the Hopper architecture, the raster
engines also seem to be gone, and there is also no trace of video encoding blocks in the architectural
documentation, though there is hardware decoding for some video formats and JPEG images as these can
be useful in AI applications.

AMD only really started in GPU compute architectures towards late 2018 (with basically a first product
just to try the software stack) and its architectures for compute and rendering GPUs have been diverging from the start.
AMD's rendering GPUs use the RDNA architecture of which the first iteration launched in 2019 and the 
third iteration was launched by the end of 2022,
while the compute GPUs use the CDNA architecture which is a descendant of the VEGA architecture with a relatively
different structure of the compute units. The CDNA GPUs also lack the ray tracing units of RDNA2/3, 
and the texture units and raster engine that are needed in rendering GPUs.
AMD has announced it is working on a more unified architecture again, UDNA, but expect specialised variants
for different workloads.

Intel has its Xe architecture, with different variants for integrated graphics, discrete rendering cards,
and compute. The latter is the Data Center GPU MAX (code-named Ponte Vecchio) used in the USA supercomputer
Aurora.

It is to be expected that compute and render GPUs will only diverge more over time as it is increasingly impossible
to build hardware that does both well and is still cost-effective for a large enough market. 
Within compute-oriented GPUs we are also starting to see a split between architectures optimised for traditional 
HPC with lots of FP64 compute power, and architectures more optimised for the lower-precision data formats that 
are popular in AI applications and less emphasis on FP64 compute which is still required by many scientific computing
applications. One example is the NVIDIA Blackwell B200 compute GPU line that appeared on the market at the end of 2024.
When we also add the cloud and regular server market, at the AI level, we can also expect that there will be cards that
specialise in deep learning inference computations only, and more versatile cards that are suitable for training.
This is again just a matter of costs and size of markets, and also triggered by stagnating transistor costs.
