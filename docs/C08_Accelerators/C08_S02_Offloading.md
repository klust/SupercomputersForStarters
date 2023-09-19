# Offloading

In early accelerator generations (and this is still the case in 2022) a CPU cannot
directly operate on data in the memory of the GPU and vice-versa. Instead data
needs to be copied between the memory spaces. Multiple accelerators in a system
may or may not share a memory space. NVIDIA NVLink for instance is a technology
that can be used to link graphics cards together and create a shared memory space,
but even then it is crucial for performance that data is as much as possible in
the memory directly attached to a GPU when being used. Many modern GPUs for 
scientific computing include support for unified memory where CPU and GPUs in the
system can share a single logical address space, but under the hood the data still
needs to be copied. This feature has been present in, e.g., the NVIDIA Pascal and later 
generations, where a page fault mechanism would be used to trigger a migration mechanism.

Control-wise the program running on the CPU orchestrates the work but passes
control to the accelerator to execute those code blocks that can be accelerated
by the accelerator.

The main problem with this model is that all the data copying that is needed,
whether explicitly triggered by the application or implicitly through the unified
memory model of some modern GPUs, can really nullify the gain from the accelerator.
This is no different from what we saw for distributed computing. Just as for distributed
computing, the fraction of the algorithm that cannot be parallelized limits the speed-up,
as does the communication overhead, for accelerators the fraction of the application and 
the overhead to pass data to and from the accelerator will limit the speed-up that can
be obtained from the accelerator. Hence it is clear that for future generations of
accelerators, the main attention will be in making them more versatile to reduce the
fraction that cannot be accelerated, and in integrating them closer with the CPU to reduce
or eliminate the overhead in passing data between CPU and GPU.

The first signs of this evolution were in some USA pre-exascale systems Summit and
Sierra that used a IBM POWER9 CPUs and NVIDIA V100 GPUs. 
Those GPUs were connected to the CPU through NVLink,
NVIDIA's interconnect for GPUs, rather than only PCIe links, so that the memory spaces
of CPU and GPU become physically merged, with all data directly addressable from GPU and
CPU and cache coherency. 

This technology of the Summit and Sierra supercomputers is carried over to the first three
planned exascale systems of the USA. Frontier, and the European supercomputer LUMI, use the
MI250X variant of the AMD CDNA2 architecture. The nodes in these systems consist of 
4 MI250X GPUs, with each GPU consisting of two dies, and a custom Zen3-based CPU code named Trento.
AMD's InfinityFabric is not only used internally in each package to connect the dies (and the zen3
chiplets with the I/O die in the Trento CPU), but also to connect the CPU packages to each other
and the CPUs to the GPUs, hence creating a unified coherent memory space.
This allows each CPU chiplet to access the memory of the closest attached GPU die with full cache coherency,
but it is not clear if coherency is usable over the full system. 
The Aurora supercomputer which uses
Intel Sapphire Rapids CPUs and Ponte Vecchio GPUs will also support a unified and some level of cache-coherent
memory space. NVIDIA was lagging a bit with the Ampere generation that has no longer a corresponding
CPU that supports its NVLink connection, but will return with its own ARM-based CPU code named
Grace and the Hopper GPU generation. The Grace Hopper superchip will combine a Grace CPU die and
a Hopper GPU die with a coherent interconnect between them, creating a coherent memory space
between the CPU and GPU in the same package, though the available whitepapers suggest no 
coherency between different CPU-GPU packages in the system. Placing a CPU and GPU in the same
package also not only ensures much higher bandwidth between both, but also a much lower energy
consumption for the data transfers.

Future generation products will go even further. The AMD MI300 is technically speaking no longer
a GPU but rather an Accelerated Processing Unit or APU as it will integrate CDNA3 GPU dies, Zen4 CPU dies
and memory in a single package. The CPU and GPU dies will no longer have physically distinct memory
and AMD claims that copy operations between CPU and GPU in a package will be completely unnecessary.
MI300-based products should start appearing towards the end of 2023 or early 2024. 
Intel has also hinted at the Falcon Shores successor to Ponte Vecchio and Rialto Bridge, likely a 2024 or 2025 product, 
that will also combine CPU and GPU chiplets in a single package with a fully unified memory
space. 

Note that current Intel or AMD-based PC's with integrated GPUs are not better in this respect.
The CPU and integrated GPU share the physical RAM in the system, but each has its own
reserved memory space and data still needs to be migrated between those spaces. 
The Apple M1 and later chips on the other hand so seem to have a truly unified memory space,
though given the limited information that Apple provides, it is hard to see if it is indeed
a fully hardware based solution. The fact that the M1 can be so fast in photo- and video
processing apps though indicates that the M1 does indeed have an architecture that allows
to make use of all accelerators in a very efficient way.

