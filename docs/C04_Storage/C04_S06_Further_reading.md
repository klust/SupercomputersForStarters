# Further reading

## Scientific data file format libraries

-   [HDF5](https://www.hdfgroup.org/solutions/hdf5/),
-   [netCDF](https://www.unidata.ucar.edu/software/netcdf/),
-   [SIONlib](https://apps.fz-juelich.de/jsc/sionlib/docu/index.html)
-   [ADIOS](https://www.ornl.gov/project/adios)
    -   [ADIOS2 documentation](https://adios2.readthedocs.io) 


## Related libraries

-   [openPMD C++ and Python API](https://openpmd-api.readthedocs.io) library for implementing
    the [openPMD standard](https://github.com/openPMD/openPMD-standard) for meta data and
    naming schemes for particle-mesh data files, useable with several backends.


## Related trainings

-   Various VSC Python trainings show how to use scientific data file formats and working in a 
    file system friendly way in Python

    -   [Scientific Python](https://gjbex.github.io/Scientific-Python/). The 
        [current version of the slides](https://github.com/gjbex/Scientific-Python/raw/master/scientific_python.pptx)
        contains a chapter on the use of HDF5. The GitHub repository with various examples, contains
        [examples and a Jupyter notebook for HDF5](https://github.com/gjbex/Scientific-Python/tree/master/source-code/hdf5) and
        [examples and a Jupyter notebook for netCDF](https://github.com/gjbex/Scientific-Python/tree/master/source-code/netcdf).

    -   [Python for HPC](https://gjbex.github.io/Python-for-HPC/). Not everything is mentioned on 
        the web site, but the [current version of the slides](https://github.com/gjbex/Python-for-HPC/raw/master/python_for_hpc.pptx)
        has at the end a chapter on I/O, mostly discussing using HDF5 in Python.
        See also [these example files and Jupyter notebook on GitHub](https://github.com/gjbex/Python-for-HPC/tree/master/source-code/hdf5).

    -   Sometimes other data formats may be a better idea, e.g., in AI applications. With Python (and there exist such libraries
        for, e.g, C programmers), one can read directly from various archive file formats, e.g., gzip and tar files.
        See [these examples](https://github.com/gjbex/Python-for-systems-programming/blob/master/hands-on/compressed_files.ipynb)
        from the VSC [Python systems programming course](https://gjbex.github.io/Python-for-systems-programming/)

-   MPI courses will often also discuss MPI I/O which is a building block to implement proper I/O strategies
    in parallel applications, and is also used in the implementation of HDF5, netCDF and likely other libraries.

-   Jülich Supercomputer Centre (between Aachen and Köln) organises [a lot of trainings](https://www.fz-juelich.de/en/ias/jsc/news/events/training-courses).
    The annual training "Parallel I/O and Data Formats" is particularly relevant for this chapter of this tutorial.


## Technologies

-   [Rabbit storage in the El Capitan user guide](https://hpc.llnl.gov/documentation/user-guides/using-el-capitan-systems/file-systems-rabbits)

    -   [YouTube video on Rabbit Storage](https://www.youtube.com/watch?v=xccViZtVye4)


## Illustration

Vastly different SSD speeds depending on capacity, showing that parallelism is important there too:
-   2022 M2 MacBook Pro 13” (launched in June 2022)
    -   [Review on ArsTechnica]9https://arstechnica.com/gadgets/2022/06/m2-macbook-pros-256gb-ssd-is-only-about-half-as-fast-as-the-m1-versions/
    -   [Review on ExtremeTech](https://www.extremetech.com/computing/337476-base-model-macbook-pro-with-m2-has-slower-ssd-than-the-m1-version)
-   2016 iPhone 7 32 GB compared to the 128GB and 256GB SKUs
    -   [ExtremeTech article](https://www.extremetech.com/mobile/238006-iphone-7-storage-tests-show-higher-end-models-are-significantly-faster-than-the-32gb-version), 
        with a more technical explanation of what’s going on.
    -   [Article on CNET](https://www.cnet.com/tech/mobile/yes-your-32gb-iphone-7s-storage-is-slower-but-heres-why-it-doesnt-matter/)
