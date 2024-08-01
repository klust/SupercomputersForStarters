# Further reading

## Scientific data file format libraries

-   [HDF5](https://www.hdfgroup.org/solutions/hdf5/),
-   [netCDF](https://www.unidata.ucar.edu/software/netcdf/),
-   [SIONlib](https://apps.fz-juelich.de/jsc/sionlib/docu/index.html)
-   [ADIOS](https://csmd.ornl.gov/adios)
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
    The anual training "Parallel I/O and Data Formats" is particularly relevant for this chapter of this tutorial.
