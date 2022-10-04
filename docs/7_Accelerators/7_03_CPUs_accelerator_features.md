# CPUs with accelerator features

In the (relatively young) history of personal computers and their microprocessors,
succesful accelerators were often integrated in CPUs by means of extensions of the
CPU instruction set. Though never as performant as a dedicated accelerator, it was
often a "good enough" solution to the extent that some accelerators even disappeared
from the market, and as these are now part of the instruction set of the CPU, programming
is also greatly simplified as there is no need to pass data and control to a coprocessor.

Vector accelerators have long had an influence on CPUs. 
The original Intel MMX instructions (which is rumoured to stand for MultiMedia eXtensions) were 
designed to compete with the DSPs used in sound cards in the second half of the '90s.
They were introduced in an update of the Pentium architecture in 1997. This instruction set
reused 64-bit registers from the floating point unit, so both could not be used together.
Two years later, in 1999, intel introduced the first version of SSE, which used new
128-bit registers. 
The MMX and SSE instruction sets made it feasible to process audio on the CPU and quickly
erased the market of higher-end soundcards with DSPs. 
The SSE instruction set continued to evolve for several generations and also adopted support
for floating point computing. It became essential to get the full performance out of Intel 
CPUs in scientific codes.
The various editions of the SSE instruction set where superseded by the AVX and later AVX2
instruction sets, both of which use 256-bit registers, defining vector operations working
on 4 double precision or 8 single precision floating point number simultaneously.
Maybe less known is that Intel's current vector instruction set for scientific computing, 
AVX512, which as the name implies uses 512-bit registers that can hold 8 double precision or
16 single precision floating point numbers, has its origin in a failed project to build a
GPU with a simplified x86 architecture, code-named Larrabee. The Larrabee design was recycled
as the Xeon Phi, a chip for supercomputing meant to compete with the NVIDIA GPUs, and in a 
second generation Xeon Phi product the instruction set was thoroughly revised to become
the AVX512 instruction set which is the first x86 vector extension with good support for
scatter and gather operations and predication.

Another interesting processor design is the ARM-based Fujitsu A64fx which is used in the 
Japanese Fugaku supercomputer which held the crown of fastest computer in the world from
June 2020 until June 2022 when it was finally surpassed by the Frontier supercomputer.
The A64fx processor was built specifically for supercomputers. Together with ARM, a new
vector extension to the ARM instruction set was developed, Scalable Vector Extensions or SVE,
which later became an official part of an update of the ARM architecture. The A64fx combines
48 or 52 cores on a chip with a very high bandwidth but relatively small memory system
that uses the same technology as the supercomputer GPUs of NVIDIA. It could be used
to build supercomputers that were not only as fast as GPU-based systems, but also almost
as power-efficient, and performance was good even on some applications that are less suited
for vectorisation but also don't run very good on traditional CPUs due to the lack of memory
bandwidth in the latter.

Matrix accelerators, although fairly new in the market, are also already starting to
influence CPU instruction sets. 
IBM has added matrix instructions for both AI and linear algebra (the latter requiring 
single and double precision floating point) to the POWER10 processor.
Intel has already some instructions in some CPUs but only for low-precision inference, but
will add a new instruction set extension called AMX in Sapphire Rapids, a server CPU that
should come out in late 2022 or early 2023. It is still only meant for AI applications,
supporting 4 and 8-bit integers and Googles bfloat16 data format.
Similarly the ARM V9-A instruction set adds Scalable Matrix Extensions to the architecture.
As is the case for Intel, these extensions are also aimed at AI applications, supporting 
8-bit integer and bfloat16 data formats.

Though these CPU instructions certainly don't make CPUs so fast that they beat GPUs, they
also have two advantages over accelerators: there is no need to pass control to an remote processor,
which can save some time, and there are no issues with data needing to be moved. Also,
one should not forget that a single GPU card for a supercomputer easily costs three times
or more as much as a single CPU socket with memory (and even more if the lower end SKUs in
the CPU line are used), so a CPU doesn't need to be as fast as a GPU to be the most economical
solution.
