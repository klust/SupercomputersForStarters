# Accelerator programming

We will restrict ourselves to GPGPU programming as these are currently the most
widely used accelerators in supercomputers.

## The current state

Accelerator programming is, also due to the early stage of the technology, still
a mess, and standardisation still has to set in. This is not uncommon in the world
of supercomputing: The same happened with message passing where there were several
proprietary technologies until the needs were sufficiently well understood to come to
a standardisation.

Currently there are three competing ecosystems growing:

1.  NVIDIA was the first to market with a fairly complete ecosystem for GPGPU
    programming. They chose to make large parts of their technology proprietary
    to create a vendor lock-in and keep hardware prices high.

    CUDA is the main NVIDIA ecosystem. NVIDIA together with partners also created
    a set of compiler pragma's for C/C++ and Fortran for GPGPU programming with an
    open consortium, but many other compiler vendors are hesitant to pick those up.

    The NVIDIA toolset also offers some support for open, broadly standardised
    technologies, but the support is usually inferior to that for their proprietary
    technologies or standards that are in practice largely controlled by them.

2.  AMD is a latecomer to the market. Their software stack is called ROCm, and they
    open sourced all components to gain more traction in the market. They basically
    focus on two technologies that we will discuss: HIP and Open MP.

3.  Intel calls its ecosystem oneAPI, as it tries to unify CPU and GPGPU programming,
    with libraries that make it easy to switch between a CPU-only version and a 
    GPGPU-accelerated version of an application.

    The oneAPI ecosystem is largely based on Data Parallel C++, an extension of SYCL,
    and Open MP compiler pragmas. 

    The oneAPI is partly open-sourced. In some cases, only the API is available and 
    vendors should make their own implementation, but, e.g., the sources for their
    clang/LLVM based Data Parallel C++ compiler are available to all and this has
    been used already to make ports to NVIDIA and AMD GPU hardware.


## Lower-level models

These models use separate code for the host and the accelerator devices that are then
linked together to create the application binary.

### CUDA

CUDA is the best known environment in the HPC world. It is however a proprietary
NVIDIA technology. It is possible to write GPGPU kernels using subsets of C, C++
and Fortran. CUDA also comes with many libraries with optimised routines for 
linear algebra, FFT, neural networks, etc. This makes it by far the most extensive
of the lower-level models. 

CUDA, which launched in 2007, has gone through a rapid evolution though it is now
reaching a level of maturity with less frequent major updates.


### HIP

HIP or **H**eterogeneous Computing **I**nterface for **P**ortability is AMD's 
alternative to CUDA. It tries to be as close as possible as legally possible.
The HIP API maps one-to-one on the CUDA API, making it possible to recompile HIP
code with the CUDA tools using just a header file. In theory this process is
without loss of performance, but there is one caveat: A kernel that is efficient
on an AMD GPU may not be the most efficient option on a NVIDIA GPU as, e.g., the vector
size is different. So careful programming is needed to ensure efficiency on both
platforms.

HIP is purely based on C++ with currently no support for Fortran GPU kernels. 
Feature-wise it is roughly comparable to CUDA 7 or 8, but features that cannot yet
be supported by AMD hardware are missing. The ROCm platform also comes with a lot
of libraries that provide APIs that are also similar to the APIs provided by their CUDA 
counterpart to further ease porting code from the CUDA ecosystem to the AMD ROCm ecosystem.

HIP also comes with two tools that help in reworking CUDA code into HIP code, though 
both typically need some manual intervention.

### OpenCL

OpenCL or Open Compute Language is a framework for heterogeneous computing developed by
the non-profit, member-driven technology consortium Khronos Group which manages many
standards for GPU software (including also OpenGL and its offsprings and Vulkan). It
develops vendor-neutral standards.

OpenCL is a C and C++-based technology to develop code that not only runs on GPU accelerators,
but the code can also be recompiled for CPU only, and some DSPs and FPGAs are also supported.
This makes it a very versatile standard and it was for a long time a very attractive standard
for commercial software development. It is by far not as advanced as CUD or not even HIP. 
The evolution of the standard has been very slow lately, with vendors hardly picking up 
many of the features of the 2.x versions. This lead to version 3.0 of the standard that 
defined a clear baseline and made the other features explicitly optional so that programmers
at least know what they can expect. 

OpenCL is in principle supported on NVIDIA, AMD and Intel hardware, but the quality of the
implementation is not always spectacular. There are also some open source implementations
with varying hardware support. It has been used in supercomputing software though it was not 
always the only model offered as several packages also contained specialised CUDA code for better 
performance on NVIDIA hardware. The molecular dynamics package GROMACS is one example,
and they will be switching away from OpenCL to newer technologies to support 
non-NVIDIA hardware.

OpenCL is largely superseded by newer technologies developed by the Khronos Group.
Vulkan is their effort to create an API that unifies 3D graphics and computing and is mostly
oriented towards game programming etc., but less towards supercomputing (as several GPUs used
for supercomputing even start omitting graphics-specific circuits) while SYCL, which we will 
discuss later in these notes, is a new initiative for GPGPU programming.


## Compiler directives

In these models, there are no separate sources for the GPU that are compiled with a separate
compiler. Instead, there is a single base of code with compiler directives instructing the
compiler how certain parts of the code can be offloaded to an accelerator. These directives
appear as comments in a Fortran code to compilers that do not support the technology
or use the `#pragma` directive in C/C++ to make clear that they are directives to compilers
that support those.

### OpenACC

OpenACC was the first successful compiler directive model for GPGPU computing. It supports
C, C++ and Fortran programming.

The first version of standard made its debut at the supercomputing computing conference SC'11
in November 2011. The technology was largely developed by 4 companies: the GPU company NVIDIA,
the compiler company Portland Group which was later renamed PGI and later bought by NVIDIA,
the supercomputer manufacturer Cray which got bought by HPE and largely lost interest in 
OpenACC, now more promoting an alternative, and CAPS Entreprises, a French company and university
spin-off creating tools for accelerator programming that failed in 2013. 

The OpenACC standard is currently controlled by the OpenACC Organization, and though it does
count other companies that develop GPUs among its members, it still seems rather dominated by
NVIDIA which may explain why other companies are somewhat hesitant to pick up the standard.
The standard is at the time of writing of this section at version 3.1, released in November 2021,
but is updated almost annually in November.

OpenACC is well supported on NVIDIA hardware through the NVIDIA HPC compilers (which is the 
new name of the PGI compilers adopted after the integration of PGI in NVIDIA). GCC offers some 
support on NVIDIA and some AMD hardware for version 2.6 of the standard (the November 2017 version) since version 10 of the
GCC compilers., but the evolution is slow and performance is often not that great.
More promising for the future is the work going on in the clang/LLVM community to support
OpenACC, as this effort is largely driven by the USA national labs who want to avoid having
to port all OpenACC-based code developed for the NVIDIA based pre-exascale supercomputers to
other models to run on the new AMD and Intel based exascale supercomputers. In fact,
the clang/LLVM ecosystem is the future for scientific computing and not the GNU Compiler
Collection ecosystem as most compiler vendors already base their compilers on that technology.
The NVIDIA, AMD and new Intel compilers are all based on LLVM and the clang frontend for C and
C++-support, with NVIDIA and AMD still using a Fortran front-end developed by PGI and donated
to the LLVM project while Intel is already experimenting with a new community-developed
more modern front-end for Fortran. OpenACC support in the LLVM ecosystem will build upon the
OpenMP support, the technology that we will discuss next, using extensions for those OpenACC
features that still have no equivalent in OpenMP. However, as of version 15 (summer 2022)
this support is still very incomplete and experimental.


### OpenMP

We've already discussed OpenMP as a technology for shared memory parallelism and for 
vectorisation (the latter since version 4.0 of the standard). But OpenMP is nowadays even 
more versatile. Since version 4.0 of the standard, released in July 2013, there is also
support for offloading to accelerators. That support was greatly improved in 
version 5.0 of the standard which was released at the SC'18 supercomputer conference.
It became a more descriptive and less prescriptive model (offering the compiler enough information
to decide what it should do rather then enforcing the compiler to do something in a particular way),
with the prescriptive nature being criticised a lot by the OpenACC community who claimed superiority
because of this. It also contained much better support for debuggers, performance monitoring tools, etc.

OpenMP has since had minor extensions i the form of version 5.1 at SC'20 and 5.2 at SC'21. The
standard is controlled by a much larger consortium than the OpenACC standard.

OpenMP is an important technology in the AMD ROCm and Intel oneAPI ecosystems. Intel has in fact
supported OpenMP offload to some of its own GPUs for many years, long before establishing the oneAPI
ecosystem. GCC has had some support for OpenMP offload to NVIDIA hardware since version 7 and to 
some AMD hardware since version 10. However, as is the case for OpenACC, it may not be the most
performant option on the market. The clang/LLVM ecosystem is working hard for full support of
the newest OpenMP standards. The AMD and new oneAPI Intel compilers are in fact fully based
on clang and LLVM using some of their own plug-ins to offer additional features, and the NVIDIA 
HPC compiler also largely seems to be based on this technology. NVIDIA and AMD also use the LLVM
backend to compile CUDA and HIP kernels respectively, showing once more that this is the compiler
ecosystem for the future of scientific computing.


## C++ extensions

### SYCL

SYCL is a programming model based on recent versions of the C++ standard. 
The earlier drafts of the standard go back to the 2014-2015 time frame, but SYCL really took
off with the 2020 version of the standard. SYCL is also a logical successor to OpenCL,
but now making it possible to target CPU, GPU, FPGA and possibly other types of accelerators
from a single code base. Though the programmer is of course free to provide alternative
implementations for different hardware for better performance, it is not needed
by design. SYCL is heavily based on C++ template libraries, but to generate code
for accelerators it still needs a specific compiler.

There are several compilers in development with varying levels of support for
the standard, targeting not only GPUs, but also, e.g., the NEC SX Aurora Tsubasa vector
boards. 
Most if not all of these implementations are again based on Clang and LLVM.
One implementation worth mentioning is Open SYCL. This project started under the
name hipSYCL, which as its name suggests
targets HIP for the backend to support AMD CPUs, but it has been extended to
support all three major GPU families now using ptx for NVIDIA, amdgcn code for AMD and 
SPIR-V for Intel GPUs, and the project is also working
on multi-CPU support.


### DPC++ or Data Parallel C++

Data Parallel C++ is the oneAPI implementation of SYCL. It is basically a project
by Intel to bring SYCL into LLVM, and all code is open sourced. The implementation does
not only cover CPUs and Intel GPUs, but with the help of others (including the 
company Codeplay that has since been acquired by Intel) it also adds support for 
NVIDIA and AMD GPUs and even Intel FPGAs, the latter through a backend based on
OpenCL and SPIR. That support is not included in the binaries that Intel distributes
though.

When DPC++ initially launched, it was really an extension of the then-current SYCL
standard, which is why it gets a separate subsection in these notes. 
However, it is currently promoted as a SYCL 2020 implementation.

### C++AMP

C++AMP or C++ Accelerated Massive Parallelism is a programming model developed by Microsoft.
It consists of C++ libraries and a minor extension to the language.
There are experimental implementations for non-Microsoft environments, but these are not
really popular and the technology is not really taking off in scientific computing. 
The technology is now deprecated by Microsoft.
Yet we want to mention it in these notes as it was a source of inspiration for
SYCL.


## Frameworks

There exist also several C++ frameworks or abstraction libraries that support creating
code that is portable to regular CPUs and GPU systems of various vendors. They let
you exploit all levels of parallelism in a supercomputer except distributed 
computing.

[Kokkos](https://kokkos.org/) is a framework developed by Sandia National Labs and probably the most popular one
of the frameworks mentioned here. It was first released in 2011 already but grew to a
complete ecosystem with tools to support debugging, profiling and tuning also, and now even
some support for distributed computing also. Kokkos already supports backends for CUDA
and ROCm, and there are experimental backends (based on SYCL and OpenMP offload) 
that can also support the Intel GPUs that will
be used in the Aurora supercomputer.

[RAJA](https://computing.llnl.gov/projects/raja-managing-application-portability-next-generation-platforms) 
is a framework developed at Lawrence Livermore National Laboratory, based on standard C++11. 
Just as Kokkos, RAJA has several backends supporting SIMD, threading through the TBB library or
OpenMP, but also GPU computing through NVIDIA CUDA, AMD HIP, SYCL and OpenMP offload, though not all 
back-ends are as mature or support all features.In particular the SIMD, TBB, SYCL and OpenMP target offload 
(the latter needed for Intel GPUs) are still experimental at the time this section was
last revised (March 2023).

[Alpaka](https://www.casus.science/research/software-repository/alpaka/) 
is a framework developed by CASUS - Center for Advanced Systems Understanding of the 
Helmholtz Zentrum Dresden Rossendorf. Alpaka also supports various backends, including a CUDA
back-end for NVIDIA GPUs. There is also work going on on a HIP backend for AMD GPUs, with
support for Intel GPUs coming through a SYCL and an OpenMP offloac backend. 


## Libraries

One can often rely on already existing libraries when developing software for GPUs.

NVIDIA CUDA comes with a wide range of libraries.

AMD ROCm provides several libraries that mimic (subsets of) libraries in the CUDA ecosystem,
also easing porting from NVIDIA to AMD hardware.

Intel has adapted several of its CPU libraries to use GPU acceleration also in its oneAPI platform.

However, there are also several vendor-neutral libraries, e.g.,

-   [MAGMA](https://icl.utk.edu/magma/) which stands for Matrix Algebra on GPU and Multicore Architectures, which is an early 
    example of such a library.

-   [heFFTe](https://icl.utk.edu/fft/) is a library for FFT 

