# Offloading

In early accelerator generations (and this is still mostly the case in 2024), a CPU cannot
directly operate on data in the memory of the GPU and vice-versa. Instead data
needs to be copied between the memory spaces. Multiple accelerators in a system
may or may not share a memory space. NVIDIA NVLink for instance is a technology
that can be used to link graphics cards together and create some level of shared memory space,
but coherency is also limited and in practice automatic page migration (migration of blocks of 
memory of a fixed size, called a page) over the fast link
is done. Many modern GPUs for 
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
This is no different from what we saw for distributed computing. 
We have seen that for distributed computing, the fraction of the algorithm that cannot be parallelized
puts a limit to the theoretical speed-up that can be reached, and the communication overhead 
reduces the attainable speed-up even more.
Something similar holds for computing with accelerators: there is always a part of the code
that is not suitable for the accelerators, and that will limit the theoretical speed-up,
but all the data copying to and from the accelerator reduces the attainable speed-up 
even more.
Hence it is clear that for future generations of
accelerators, the main attention will be in making them more versatile to reduce the
fraction that cannot be accelerated, and in integrating them closer with the CPU to reduce
or eliminate the overhead in passing data between CPU and GPU.

The first signs of this evolution were in the USA pre-exascale systems Summit and
Sierra that used IBM POWER9 CPUs and NVIDIA V100 GPUs. 
Those GPUs were connected to the CPU through NVLink,
NVIDIA's interconnect for GPUs, rather than only PCIe links, so that the memory spaces
of CPU and GPU become more merged, with some level of cache coherency, though in 
practice migration of pages remains necessary in most cases.

This technology of the Summit and Sierra supercomputers is carried over to the first three
planned exascale systems of the USA. Frontier, and the European supercomputer LUMI, use the
MI250X variant of the AMD CDNA2 architecture. The nodes in these systems consist of 
4 MI250X GPUs, with each GPU consisting of two dies, and a custom Zen3-based CPU code named Trento.
AMD's InfinityFabric is not only used internally in each package to connect the dies (and the zen3
chiplets with the I/O die in the Trento CPU), but also to connect the GPU packages to each other
and the CPUs to the GPUs, hence creating a unified coherent memory space with some level of cache coherency.
It enables each CPU chiplet to access the memory of the closest attached GPU die with full cache coherency,
but the coherency is not usable over the full system. 
The Aurora supercomputer, which uses
Intel Sapphire Rapids CPUs and Ponte Vecchio GPUs, will also support a unified memory 
and some level of cache-coherent memory space. 
NVIDIA was lagging a bit with the Ampere generation that has no longer a corresponding
CPU that supports its NVLink connection, but returned with its own ARM-based CPU code named
Grace and the Hopper GPU generation. The Grace Hopper superchip combines a Grace CPU die and
a Hopper GPU die with a coherent interconnect between them, creating a coherent memory space
between the CPU and GPU in the same package, though the available whitepapers suggest no 
coherency between different CPU-GPU packages in the system. Placing a CPU and GPU in the same
package also not only ensures much higher bandwidth between both, but also a much lower energy
consumption for the data transfers. NVIDIA has also already announced a combination of the Grace
CPU with two Blackwell GPU dies in a single package.

Some of the latest designs go even further. The AMD MI300A is technically speaking no longer
a GPU but rather an Accelerated Processing Unit or APU as it integrates CDNA3 GPU dies, Zen4 CPU dies
and memory in a single package. The CPU and GPU dies no longer have physically distinct memory
and AMD claims that copy operations between CPU and GPU in a package will be completely unnecessary.
MI300A was launched in December 2023 with almost immediate availability and is used in the El Capitan
supercomputer which became the number 1 in the Top-500 list in November 2024. 
Intel has also hinted at the Falcon Shores successor to Ponte Vecchio and Rialto Bridge, likely a 2025 or 2026 product, 
that will also combine CPU and GPU chiplets in a single package with a fully unified memory
space, but has now postponed this to a later generation. 

Note that current Intel or AMD-based PC's with integrated GPUs are not better in this respect.
The CPU and integrated GPU share the physical RAM in the system, but each has its own
reserved memory space and data still needs to be migrated between those spaces. 
The Apple M-series on the other hand so seem to have a truly unified memory space,
though given the limited information that Apple provides, it is hard to see if it is indeed
a fully hardware based solution. The fact that the M-series processors can be so fast in photo- and video
processing apps though indicates that those chips do indeed have an architecture that 
enables using all accelerators in a very efficient way.

