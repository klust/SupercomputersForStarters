---
tags:
-   CUDA
-   ROCm
-   HIP
-   oneAPI
-   OpenCL
-   OpenACC
-   OpenMP
-   SYCL
-   Kokkos
-   RAJA
---

# Further reading on accelerators

-   AMD GPUs
  
    -   [Whitepaper: Introducing AMD CDNA(tm) 2 Architecture](https://www.amd.com/content/dam/amd/en/documents/instinct-business-docs/white-papers/amd-cdna2-white-paper.pdf)
        which includes information on how the MI250X connects to the CPU, and on the vector and matrix cores of the GPU.
  
    -   [Whitepaper: Introducing AMD CDNA(tm) 3 Architecture](https://www.amd.com/content/dam/amd/en/documents/instinct-tech-docs/white-papers/amd-cdna-3-white-paper.pdf)
        is a whitepaper covering both the MI300A APU and MI300X GPU.
  
    -   [HPCwire article on the AMD MI300 APU](https://www.hpcwire.com/2022/06/21/amds-mi300-apus-to-power-exascale-el-capitan-supercomputer/)
        that will be used in the El Capitan supercomputer.

    -   [AnandTech article on the MI300](https://www.anandtech.com/show/17445/amd-combining-cdna-3-and-zen-4-for-mi300-data-center-apu-in-2023),
        based on information from AMD's 2022 Investors Day. The article shows some not very detailed slides about the memory architecture.

    -   [GPU comuting at the AMD Financial Analyst Day 2022 on YouTube (not an official AMD recording)](https://youtu.be/-VYHtSseX9k?t=4130). The CDNA3 part starts at 1:07:50.

    -   [MI300 @ AMD CES'2023 keynote (YouTube)](https://youtu.be/OMxU4BDIm4M?t=5382). The MI300 part starts at 1:29:42.

-   NVIDIA GPUs

    -   [Whitepaper: Summit and Sierra Supercomputers: An Inside Look at the U.S. Department of Energy’s New Pre-Exascale Systems](http://www.teratec.eu/actu/calcul/Nvidia_Coral_White_Paper_Final_3_1.pdf) 
        for information on the physically unified memory space made possible by the NVLink connections
        between the CPUs and GPUs. 

    -   [Whitepaper: NVIDIA Grace Hopper Superchip Architecture](https://nvdam.widen.net/s/qjzrmfdn2j/nvidia-grace-hopper-superchip-architecture-whitepaper-v1.0)
         for all information on the combination of the Grace ARM-based CPU and the Hopper GPUs.

-   NVIDIA CUDA

    -   [NVIDIA CUDA toolkit](https://developer.nvidia.com/cuda-toolkit)

-   AMD ROCm

    -   [AMD ROCm information portal](https://docs.amd.com/)


-   Intel oneAPI

    -   [Intel oneAPI overview](https://www.intel.com/content/www/us/en/developer/tools/oneapi/overview.html)

-   HIP

    -   [HIP in ROCm(tm)](https://rocm.docs.amd.com/projects/HIP/en/latest/)
    
    -   Implementation for Intel GPUs: 
        [chipStar compiler](https://github.com/CHIP-SPV/chipStar) from the 
        [CHIP-SPV project](https://github.com/CHIP-SPV) which itself builds on the
        [HIPCL](https://github.com/cpc/hipcl) and
        [HIPLZ](https://www.anl.gov/argonne-scientific-publications/pub/183259) projects.

-   OpenCL

    -   [Khronos Group OpenCL documentation](https://www.khronos.org/opencl/)

-   OpenACC

    -   [The OpenACC Organization](https://www.openacc.org)

    -   [Clacc, OpenACC in LLVM project page](https://www.exascaleproject.org/highlight/clacc-an-open-source-openacc-compiler-and-source-code-translation-project/)

    -   [Paper: "Clacc: OpenACC for C/C++ in Clang"](https://doi.org/10.1177/10943420241261976)
        from ORNL

-   OpenMP

    -   [OpenMP Architecture Review Board web site](https://www.openmp.org/)

-   SYCL

    -   [Khronos Group SYCL documentation](https://www.khronos.org/sycl/resources)

    -   [AdpativeCpp (formerly hipSYCL)](https://github.com/AdaptiveCpp/AdaptiveCpp)

    -   [Intel oneAPI DPC++/C++ compiler](https://www.intel.com/content/www/us/en/developer/tools/oneapi/dpc-compiler.html)

    -   [Intel oneAPI DPC++ compiler GitHub](https://github.com/intel/llvm/tree/sycl#oneapi-dpc-compiler)

-   Kokkos

    -   [Kokkos ecosystem home page](https://kokkos.org/)

-   RAJA framework

    -   [RAJA home page](https://computing.llnl.gov/projects/raja-managing-application-portability-next-generation-platforms)

    -   [RAJA documentation](https://raja.readthedocs.io/)

-   Alpaka abstraction library

    -   [Alpaka information on the Helmhotz web site](https://helmholtz.software/software/alpaka)

    -   [Alpaka information on GitHub](https://github.com/alpaka-group/alpaka). The README is the best
        source of information on compatibility with various compilers and backend technologies.

    -   [Alpaka documentation](https://alpaka.readthedocs.io/en/stable/)

    -   [Alpaka training from 2020 on YouTube](https://www.youtube.com/playlist?list=PLVyQXsMxRYdEoahVQAqf9_rewGj3VkXb4)

-   Libraries

    -   [MAGMA](https://icl.utk.edu/magma/)

    -   [heFFTe](https://icl.utk.edu/fft/)

-   CPU instruction set extensions

    -   ["What is Intel^(r)^ Advanced Matrix Extensions (Intel^(r)^ AMX)?"](https://www.intel.com/content/www/us/en/products/docs/accelerator-engines/what-is-intel-amx.html)
        with pointers to further documentation.

    -   [ARM SME Overview](https://developer.arm.com/documentation/109246/0100/SME-Overview)
  