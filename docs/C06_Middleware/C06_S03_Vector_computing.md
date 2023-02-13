# Vector computing

Automatic vectorization in compilers is moderately successful. The main problem
is that most programming languages are not written with vectorization in mind
and focus on scalar computing concepts, while the language does not always
include enough information for the compiler to detect if it is safe to vectorize.
E.g., languages that allow overlapping memory structures such as C and C++ make
the life of an automatic vectoriser very difficult. On the other hand, Fortran does
not allow such behaviour but the compiler has no means to check if the programmer
adheres to these rules, which can lead to subtle bugs if the compiler assumes 
code is safe for vectorisation.


## pragmas

Part of the solution comes again from compiler pragmas to help the compiler with
deciding whether code can be vectorised. In the past each compiler vendor had
its own set of pragmas that only work with that vendor's compiler, though quickly
at least some more or less common pragmas appeared. After the age of vector computers
these pragmas were somewhat forgotten but they did make a return with the short 
vector instruction additions to various instruction sets (including SSE and AVX in 
the Intel/AMD architecture).

With version 4.0 OpenMP got extended with support for vectorisation though many
considered the OpenMP 4.0 pragmas worse than the vendor-specific ones 
Later versions tried to offer some improvements though. 
As these pragmas are standardised, they should work with all compilers that support
the standard.
One major criticism to the OpenMP vectorisation pragmas is that they are too much
prescriptive rather than descriptive. Prescriptive means that they force the compiler
to do certain things, while prescriptive means that they just give the compiler additional
information to let the compiler decide if it is worth vectorising the code.


## Libraries

Often the critical part of the code that can benefit most from vectorisation is 
contained in a few routines, and chances are that these are actually pretty standard
operations for which good libraries exist, e.g., some basic linear algebra libraries,
FFT or image processing. In those cases it is better to rework the code to make proper
use of such libraries. E.g., optimised BLAS libraries for linear algebra (which are
also discussed later in these course notes), the FFTW library which is a popular
highly optimised library for FFT, ...


### Intrinsics

The method of last resort is manual vectorisation using intrinsics and additional data
types that correspond to vector registers. These intrinsics and data types translate directly
into vector instructions. As these are basically machine instructions and data types, they 
are CPU-specific and they may also be compiler-specific (though often other compilers follow
the intrinsics chosen by the CPU vendor's compiler).
These intrinsics should be used with care as you loose portability to other CPU architectures
or even other variants of the architecture. They may be an option though to get
the last bit of performance out of an intensely used kernel in the code, and there are
applications that use them in that way.
