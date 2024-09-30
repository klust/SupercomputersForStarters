---
tags:
-   cache (CPU)
-   BLAS
-   FFT
---

# The memory performance gap

## What is the memory performance gap?

Memory bandwidth and latency haven't improved at the same rate as
processor performance.

Let us compare a workstation-class Intel architecture processor from 
1996 (that would have been used in distributed memory supercomputers based
on the Intel architecture) with the high-end AMD processor used in 
several big supercomputers around 2023.

The 1996 processor would have been an Intel Pentium Pro processor running at a 
clock speed of 200 MHz and capable of 1 floating point operation per cycle, so
a floating point performance of 0.2 Gflops on its single core. Its memory technology would have 
been PC100 SDRAM. The memory latency would have been on the order of 300 ns,
and the processor had a 64-bit memory bus running at 66 MHz, so was capable
of a theoretical peak memory bandwidth of 0.52 GB/s.

The 2023 Intel architecture processor would have been the AMD 4<sup>th</sup> gen EPYC 9654
processor, a processor from the Genoa family. This beast has 96 cores that 
can run at 2.4 GHz (guaranteed base clock with proper cooling). 
When counting the so-called fused
multiply-add instruction as two floating point operations (a fused 
multiply-add instruction is an instruction that computes $a*b+c$ in a single
pass), and taking into account that each core has 2 256-bit vector units,
the chip is capable of 16 floating point operations per cycle per core
which translates into a theoretical peak floating point performance 
of 3686 Gflops. Its memory architecture is DDR5, with a memory latency
of somewhere around 110-120 ns. 
You'll find different and lower numbers also, but it all depends on
how you measure (to the core, to the socket, first byte or the whole
cache line, ...) and whether you take best case or average performance.
Each socket has 12 64-bit memory channels running at a data rate of
4800 MHz, so the theoretical peak memory bandwidth is 461 GB/s.

|                         | 1996 Pentium Pro | 2023 AMD EPYC 9654 | Change             |
|:------------------------|-----------------:|-------------------:|--------------------|
| Peak flops              | 0.2 Gflops       | 3686 Gflops        | $\times 18,430$    |
| Peak memory bandwidth   | 0.52 GB/s        | 461 GB/s           | $\times 890$       |
| Memory latency          | 300 ns           | 80-120 ns          | $/ 2.5 - 4$        |
| Latency in clock cycles | 60 cycles        | 192-288 cycles     | $\times 3.2 - 4.8$ |

While peak flops have exploded over this period - a factor of over 18,000 on a per 
socket basis between chips that are meant for technical computing - the memory bandwidth
has not followed that pace as it grew only with a factor of 890. And we actually took
the first member of a new generation that usually gives higher bandwidth per core or
per flop for the 2023 chip. And even worse, memory latency only decreased with a 
factor around 3 and has in fact been stagnant for many years now.
This is an excellent example of how parameters of a computer system evolve at vastly
different rates.

Memory bandwidth is clearly a problem. The 1996 Pentium Pro could produce 1.6 GB/s 
of double precision floating point results which is three times the bandwidth of the
memory system. And this does not yet take into account that to produce those results,
one needs input too. So clearly this chip could not work at full speed at all if all
data has to come from main memory. However, for the 2023 AMD EPYC processor the situation
is even much worse. That chip can in theory produce 29,488 GB/s of results with is over
60 times the memory bandwidth (and its predecessor was even worse as there was a big
generational jump in memory bandwidth). So clearly this chip would be running at an
even lower fraction of its peak speed if all data had to come from memory.

The memory latency is an even bigger problem. 300ns latency on the Pentium Pro means
that 60 floating point operations could have been done in that period. 
However, the 120ns latency on the AMD EPYC from 2023 corresponds to more than 400,000
floating point operations. Obviously this is an exaggeration as the 2023 chip is a multi-core
chip and in fact multiple cores can use different parts of the main memory in parallel,
but still...

## Speed-limiting factors that cause the memory performance gap

Memory speed and latency are limited by both physical and economical constraints.

Larger memories are usually slower due to physical constraints: If a memory device
becomes physically larger, it has to be put further away from the CPU so the travel
distance for electric signals to the memory becomes longer, but as the memory device
becomes larger travel distances inside the memory device also become longer. Longer 
distances come with expensive signal regeneration introducing additional delays
and/or lower transfer speeds.

The number of connections one can make from the CPU to other devices is also limited
by both technological and economical constraints. Those connections are expensive,
so the bandwidth one can reach to devices external to the CPU socket is also limited.
Also, there are faster memory types than the DRAM technology currently used, but 
they are expensive and also difficult to scale economically to large capacities.

There are however two other possibilities.

On-die memory can be very fast, but is is limited in capacity due to size and
price constraints. It uses a different type of memory cell that takes more
space per bit. However, very wide and high-speed data paths are possible to that memory.
(Though here too problems are starting to appear as the size of the memory cell doesn't
shrink nicely with new process technologies.)

Second, modern packaging technologies enable the integration of memory in the package
that also contains the CPU dies. As that memory sits closer to the CPU, higher transfer
speeds are often possible, and it is also possible to use a much wider data path the memory.
Some CPUs and GPUs use this to add additional very fast memory of the type discussed in the 
previous paragraph, but it is now also a popular technique to add memory that uses the same
DRAM technology as main memory. It is in fact a standard practice in smartphones.
The Apple Silicon M-series processors also use this technology, with a data path of 512 bits wide
(8 64-bit channels) in the Max-series, resulting in a memory bandwidth to the CPU and integrated GPU
that is higher than the per-socket memory bandwidth in many supercomputer CPUs. 
The technology is now also starting to appear in x86-based PCs. E.g., the Intel Core Ultra 200V series
code named Lunar Lake have all memory integrated in the socket, but so far this is only done for 
a reduction of power and increase of the clock speed of the memory bus, but not to get a wider
memory bus.

It is also a standard technology in GPUs for scientific computing.
E.g., the AMD MI100 and NVIDIA V100 GPUs for scientific computing and the Fujitsu A64fx CPU for
supercomputers all have a 4096-bit wide data bus to memory (in fact, 4 1024-bit buses and but at a slower speed).
The NVIDIA A100 and regular H100 have 5120 bit wide buses (in fact, 5 1024 bit buses), 
the NEC SX-Aurora TSUBASA first and second gen vector accelerators have a 6144 bit wide bus (in fact, 6 1024-bit buses)
and the AMD MI250X GPU used in LUMI and its successor the MI300 series 
have even an 8192 wide bus (in fact, 8 1024-bit buses).
However, that memory is limited in capacity. E.g., the MI250X is limited to 128 GB
and the newer MI300X to 192 GB (the largest of all GPUs available in the early fall of 2024).
All these processors or accelerators use various generations of so-called HBM memory.

<!-- E.g., the theoretical peak memory bandwidth of an MI250X is 3.2 TB/s and the peak memory bandwidth
of an NVIDIA H100 compute GPU is around 2 TB/s -->
E.g., the theoretical peak memory bandwidth of an MI300X is 5.3 TB/s and the theoretical peak
memory bandwidth of an NVIDIA H200 compute GPU is around 4.8 TB/s,
while an NVIDIA 4090 Super gaming
card, which does not have the memory integrated in the package but uses a hardware 
architecture developed together with that of the H200, has a theoretical peak memory
bandwidth around 1 TB/s. All three of these GPUs became available in early 2024 (though
the NVIDIA ones are really small updates of a previous design).


## What can we do to deal with the memory performance gap?

The previous discussion already gives a key to the solution: We have access to different memory 
technologies with different speed and capacity trade-offs, so the solution is to go for a
hierarchical setup that combines different technologies, preferably in a way that is transparent
to the user when it comes to correctness of the program so that the processor remains compatible
with previous generations. This is exactly what happens: All modern processors implement so-called 
caches, buffer memory using a very fast memory technology that is managed by the processor
hardware (with most processors also giving some level of user-control).

In fact, even for that memory a hierarchy is used, typically with three levels:

-   Level 1 or L1 caches are very small and very specialised as they sit very close to
    specific parts of the processors. Processors will often have one for instructions and
    one for integer data. Floating point data is not always stored in that cache. The typical
    size is in the range of 32 to 64 kB.

-   The level 2 or L2 cache is larger. On processors for scientific computing it usually still
    at the core level and not shared between cores, but it can now contain both instructions and
    all data types. Sizes range from 256 kB to 2 MB on modern CPUs.

-   The level 3 or L3 cache is typically shared by a number of cores (but not always by all cores).
    Both the total capacity and capacity per core is all over the place in modern CPUs. E.g.,
    on AMD Milan and Genoa EPYC CPUs the L3 cache is shared by all cores of a CCD (chiplet) and
    is 32 MB or 96 MB per CCD.

Some vendors are starting to experiment with even different technologies. E.g, Intel now has
some processors for scientific computing from the "Sapphire Rapids" generation that also embed 
so-called HBM memory on the socket. These chips are currently known as the Intel Xeon
CPU Max 9xxx series. They contain 4 HBM stacks, for a theoretical memory bandwidth of 1.6 TB/s and
a practical memory bandwidth of 1 TB/s. They can be used either as cache for the larger external
memory, or directly as memory and in fact the processor could even run without external DRAM
memory attached. 
Previously Intel also experimented with an L4 cache provided by 64-128 MB of
eDRAM in some Intel Broadwell (i7-5xxx) and Skylake (i7-6xxx) chips for consumer PCs.

Though caches - at least on CPUs - are transparent with respect to correctness, they need to be taken
into account to get a good performance. It is very important to organize data access in code so that data 
accesses become local (i.e., the next memory cell you need is close to the one you just accessed) and
predictable in nature and not random. Ideally you'd stream data through a processor and do as much 
computations as possible with a bit of data before progressing to the next bit. We're not talking about 
a performance doubling here, but rather about factors of 100x or even 1000x, so several orders of magnitude,
for the best codes compared to a bad code for essentially the same algorithm.

Luckily we don't need to write all that code. Many scientific codes spend most of their time in a few pretty
standard routines for linear algebra, FFT, etc. There are very good optimised libraries for linear algebra,
FFT, image processing and other core operations of popular algorithms. It is important to use those libraries
and not think that we can easily do better. One example is the BLAS library for basic vector and matrix
operations. This library is used in many other linear algebra libraries. The reference implementation in 
Fortran sucks on modern systems, but there are many excellent commercial and free implementations that give
great performance: Intel MKL, AMD Core Math Library, OpenBLAS, Bliss, Atlas, ... Basically any microprocessor
company that builds processors that may be used in scientific applications, will support an optimised BLAS library.
For other types of applications, e.g., solving PDEs, there exist frameworks that will help you in organising data
access in a proper way.

So even if you only use code, it is important that you realise that not all code is created equal and that big
performance differences do occur. Moreover, code from books such as numerical recipes typically sucks from a 
performance point of view. 

