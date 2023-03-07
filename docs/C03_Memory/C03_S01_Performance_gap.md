# The memory performance gap

Memory bandwidth and latency haven't improved at the same rate as
processor performance.

Let us compare a workstation-class Intel architecture processor from 
1996 (that would have been used in distributed memory supercomputers based
on the Intel architecture) with the high-end AMD Rome processor used in 
several big supercomputers around 2020.

The 1996 processor would have been an Intel Pentium Pro processor running at a 
clock speed of 200 MHz and capable of 1 floating point operation per cycle, so
a floating point performance of 0.2 Gflops on its single core. Its memory technology would have 
been PC100 SDRAM. The memory latency would have been on the order of 300 ns,
and the processor had a 64-bit memory bus running at 66 MHz, so was capable
of a theoretical peak memory bandwidth of 0.52 GB/s.

The 2020 Intel architecture processor would have been the AMD EPYC 7H12
processor, a processor from the Rome family. This beast has 64 cores that 
with proper cooling can run at 2.6 GHz. When counting the so-called fused
multiply-add instruction as two floating point operations (a fused 
multiply-add instruction is an instruction that computes $a*b+c$ in a single
pass), and taking into account that each core has 2 256-bit vector units,
the chip is capable of 16 floating point operations per cycle per core
which translates into a theoretical peak floating point performance 
of 2660 Gflops. Its memory architecture is DDR4, with a memory latency
of 120 ns. You'll find different and lower numbers also, but it all depends on
how you measure (to the core, to the socket, first byt or the whole
cache line, ...) and whther you take best case or average performance.
Each socket has 8 64-bit memory channels running at a data rate of
3200 MHz, so the theoretical peak memory bandwidth is 205 GB/s.

|                       | 1996 Pentium Pro | 2020 AMD EPYC 7H12 | Change          |
|:----------------------|-----------------:|-------------------:|-----------------|
| Peak flops            | 0.2 Gflops       | 2660 Gflops        | $\times 13,300$ |
| Peak memory bandwidth | 0.52 GB/s        | 205 GB/s           | $\times 400$    |
| Memory latency        | 300 ns           | 80-120 ns          | $/ 2.5 - 4$     |


