---
tags:
-   vector instructions
-   matrix instructions
---

# CPUs with accelerator features

In the (relatively young) history of personal computers and their microprocessors,
successful accelerators were often integrated in CPUs by means of extensions of the
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
The MMX and SSE instruction sets made it feasible to process audio on the CPU and 
soon eliminated the market of higher-end sound cards with DSPs. 
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
Intel had already some instructions in some CPUs but only for low-precision inference, but
really jumped upon this with a new instruction set extension called AMX launched with 
the Sapphire Rapids server CPUs that came out in 2023. It is still only meant for AI applications,
supporting 8-bit integers and Googles bfloat16 data format. 
A later version, for the 2024 Granite Rapids processor, also supports FP16.
Similarly the ARM V9-A instruction set adds Scalable Matrix Extensions to the architecture.
Instructions support 8, 16 and 32-bit integer computations, FP16, FP32 and bfloat16, and
optionally, depending on the implementation, FP8 and FP64. As is the case with Intel AMX,
many instructions are tailored to AI applications, but there are also some instructions
(part of optional extensions) to make the instruction set very useful to traditional 
double precision scientific applications.

Though these CPU instructions certainly don't make CPUs so fast that they beat GPUs, they
also have two advantages over accelerators: there is no need to pass control to a remote processor,
which can save some time, and there are no issues with data needing to be moved. Also,
one should not forget that a single GPU card for a supercomputer easily costs three times
or more as much as a single CPU socket with memory (and even more if the lower end SKUs in
the CPU line are used), so a CPU doesn't need to be as fast as a GPU to be the most economical
solution.

Many traditional scientific computing applications are often bound by memory bandwidth,
which is typically much higher on a GPU system which use a different memory technology that
doesn't offer the same expandability as we are used to from CPUs, but offers very high bandwidth.
However, that memory technology can be combined with traditional CPU architectures also. This was
done in the all-CPU Fugaku supercomputer which was the fastest supercomputer in the 
[Top500 ranking](https://top500.org/) from June 2020 until Frontier, a GPU system, took over
that position in June 2022. Intel has also experimented with a variant of its server processors
with some built-in HBM memory. And in June 2026, an all-CPU system became again the top system
in the [Top500 list](https://top500.org/lists/top500/2026/06/). The Chinese LineShine system
is built with the LX2 CPUs, a Chinese ARM-based CU that combines HBM and traditional memory
technologies: HBM for bandwidth and traditional memory for capacity. The processor is said to 
support FP64/FP32/FP16 and INT8 in both its vector and matrix units. The LineShine supercomputer
does consume 20% more power per flop on the Linpack benchmark than the fastest GPU system on 
the June 2026 list (El Capitan at number 2), but one should realise that it is almost entirely
built using Chinese fabs and technology with a manufacturing process that is probably not
yet as good as that used for the GPUs of El Capitan (even though those GPUs are also already
a bit older). It also lead to a discussion at ISC'26 in which Jack Dongarra,
Torsten Hoefler and Satoshi Matsuoka, three prominent scientist were
wondering if we wouldn't be better off with a better CPU-based design with good vector and 
matrix units and high-bandwidth memory than with GPUs for a lot of computations,
reflected in the preliminary text ["Do We Still Need GPUs? Rethinking AI and Scientific 
Computing on Matrix-Enhanced CPUs"](https://www.dropbox.com/scl/fi/u389vin4cnuvk2s3x7hcg/Do-We-Still-Need-GPUs-CACM-Revised.pdf) 
for now made available via [DropBox](https://www.dropbox.com/scl/fi/u389vin4cnuvk2s3x7hcg/Do-We-Still-Need-GPUs-CACM-Revised.pdf).


## Top500 list references

-   [LineShine](https://top500.org/system/180490/): ARMv9 all-CPU system with SVE and SME instructions.
    Number one since June 2026.

-   [El Capitan](https://top500.org/system/180307/): AMD MI300A system,
    number 1 from November 2024 up to and including November 2026.

-   [Frontier](https://top500.org/system/180047/): AMD MI250X system, number one from June 2022 up to and including
    June 2024.

-   [Fugaku](https://top500.org/system/179807/): ARMv9 all-CPU system with SVE instructions,
    number 1 from June 2020 up to and including November 2021.

