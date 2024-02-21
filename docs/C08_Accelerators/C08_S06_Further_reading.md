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
  
    -   [Whitepaper: Introducing AMD CDNA<sup>TM</sup> 2 Architecture](https://www.amd.com/system/files/documents/amd-cdna2-white-paper.pdf)
        which includes information on how the MI250X connects to the CPU, and on the vector and matrix cores of the GPU.
  
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

-   OpenCL

    -   [Khronos Group OpenCL documentation](https://www.khronos.org/opencl/)

-   OpenACC

    -   [The OpenACC Organization](https://www.openacc.org)

    -   [Clacc, OpenACC in LLVM project page](https://csmd.ornl.gov/project/clacc)

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

    -   [Alpaka information on the CASUS web site](https://www.casus.science/research/software-repository/alpaka/)

    -   [Alpaka information on GitHub](https://github.com/alpaka-group/alpaka)

    -   [Alpaka training from 2020 on YouTube](https://www.youtube.com/playlist?list=PLVyQXsMxRYdEoahVQAqf9_rewGj3VkXb4)

-   Libraries

    -   [MAGMA](https://icl.utk.edu/magma/)

    -   [heFFTe](https://icl.utk.edu/fft/)
