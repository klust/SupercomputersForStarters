# Middleware for shared memory computing

When discussing shared memory parallelism in the 
[section on processors](../C02_Processors/C02_S06_Shared_memory.md),
we've already said that automatic shared memory parallelisation has not been
ver successful so far.

## OpenMP

One of the more popular ways today to develop scientific software that makes use
of shared memory parallelism, is OpenMP.

OpenMP works through compiler pragmas which are hints placed in the code in such
a way that they would be neglected by compilers that don't know the pragmas.
In C and C++ this is done via lines starting with `#pragma` while in Fortran a notation
that would be interpreted as just another comment in the code is used.

OpenMP supports two styles of parallelism: data-parallel and task-parallel computing.
In data-parallel computing each thread works on a part of the data and it is a type of
parallelism that is very popular in scientific applications as it has the potential to
scale to more and more processors the larger the data set is. 
In task-parallel computing each thread works on a specific task instead. 
Though in fact one could consider data parallelism as a special case of task parallelism
where each task is "operate on a particular part of the data".

OpenMP is a fairly open standard. OpenMP pragmas are not vendor-specific. 
The latest version at the last revision of this chapter is OpenMP 5.2, which was launched
at the Supercomputing Conference in November 2021. That standard nature implies that code
developed with one compiler with OpenMP support will also compile with another compiler as long
as the code and the compilers adhere to the language standards and OpenMP standard. 

OpenMP originally only supported shared memory parallelism. Version 4 however was a major revision
introducing support for vectorisation and for offload to coprocessors (really designed with GPU computing in mind).

In OpenMP, the runtime behaviour such as the number of threads used and the mapping of those 
threads on the cores of the node, are often set through environment variables that are in
fact also partly standardised. Some codes will use OpenMP library functions instead and will
expect information from the user in, e.g., the input files.

OpenMP for shared memory parallelism is currently widely supported in C/C++ and Fortran compilers
for scientific use. On the VSC clusters it is supported in both the GNU C/C++ and Fortran compilers
and the Intel compilers.
Also on LUMI it is supported in all available C/C++ and Fortran compilers (GCC, AMD aocc and ROCm, and
the Cray Compiling Environment compilers).


## Other approaches

In C++ frameworks such as the Intel Thread Building Blocks (TBB) are also often used.
The Intel TBB library is open-sourced and can be used with other compilers also.
It has also been adopted in Intel's oneAPI specifications and is called oneTBB in
that context.

Some languages also have thread concepts or other concurrent processing concepts built
into the language or its standard runtime library.
Java is one such example, but Java is not really suited for distributed memory applications
as the garbage collection strategy causes performance problems in the typical scientific
distributed memory applications as many of those applications tend to use data parallelism
which typically requires all processes to run quasi-synchronised as they frequently exchange data.
Whenever a garbage collector kicks in at random it also slows down the other processes as they cannot
communicate properly anymore with the process where the garbage collection is going on.
Microsoft C# is another such language, but not really used in scientific computing at scale. 
Another such language is Google Go but that one also is not very suitable for supercomputing 
partly due to high memory consumption due to poor memory management which is also why Google isn't
using the language as much anymore as they used to. A language really designed for scientific computing
with parallelism in mind is Julia which can offer a nice and more performant alternative to 
Python and Matlab. Strangely enough though the language designers focused on distributed memory
computing first and have only started adding shared memory computing concepts in more recent years.

Lastly, one can use explicit OS threading, especially for task-based parallelism.
Linux supports the POSIX standard for threads via the Pthreads library.
This is very low-level thoug and rather cumbersome as now the programmer has to code all
thread managment themselves. Certainly for data parallelism this is rarely a good idea.