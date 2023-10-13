# Further reading

-   Vectorisation
    -   [Article in HPC wire: "Vectors: How the Old Became New Again in Supercomputing" 
         on the re-emergence of vector instructions](https://www.hpcwire.com/2016/09/26/vectors-old-became-new-supercomputing/)
-   Vector instruction extensions for CPUs
    -   AVX instruction set:
        -   [Intel AVX whitepaper](https://www.intel.com/content/dam/develop/external/us/en/documents/intro-to-intel-avx-183287.pdf)
    -   AVX512
        -   [Talk about the AVX512 history (by Tom Forsyth who works/worked at Intel)](https://guide.handmade-seattle.com/c/2019/talks/lifecycle-of-an-instruction-set/) and
            [corresponding slides](https://handmade-seattle.com/2019/slides/tom_forsyth.pdf)
    -   AVX512 has been revised in an improved specification AVX10 as all the extensions have
        turned it into a mess.
        -   [Intel AVX10 whitepaper](https://www.intel.com/content/www/us/en/content-details/784267/intel-advanced-vector-extensions-10-intel-avx10-architecture-specification.html)
        -   Article ["AVX10/128 is a silly idea and should be completely removed from the specification"](https://chipsandcheese.com/2023/10/11/avx10-128-is-a-silly-idea-and-should-be-completely-removed-from-the-specification/)
            also contains nice information about the evolution of vector instruction sets in
            the x86 architecture.
-   Interconnects to link processor packages together, or dies in a package,
    preserving global memory:
    -   [UPI - Intel Ulta Path Interconnect on WikiPedia](https://en.wikipedia.org/wiki/Intel_Ultra_Path_Interconnect)
    -   [CXL - Compute Express Link](https://www.computeexpresslink.org/)
    -   [UCIe - Unified Chiplet Interconnect Express](https://www.uciexpress.org/)
    -   [AMD Infinity Fabric](https://www.amd.com/en/technologies/infinity-architecture)
        -   [Very technical article on WikiChip](https://en.wikichip.org/wiki/amd/infinity_fabric)
