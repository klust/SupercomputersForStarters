# Some examples

[QuantumESPRESSO](https://www.quantum-espresso.org/) is a suite of codes for
electronic structure calculations and materials monitoring. Some of those codes can
scale to very large supercomputers when used to solve large problems. 
QuantumESPRESSO is nowadays a hybrid code that combines MPI (the most mature 
parallelisation technique in the code) with an increasing use of OpenMP. 
To use the code properly, you need to understand the parallelism well.
It is so important that it has 
[its own chapter in the user manual](https://www.quantum-espresso.org/Doc/user_guide/node17.html)
and [a complete section in the FAQ](https://www.quantum-espresso.org/faq/faq-parallel-execution/).
And both make it very clear that running the code properly is all but an automatic process...

[GROMACS](https://www.gromacs.org/) is a molecular dynamics code. 
The new 2023 manual has a 
[section explaining the various parallelisation schemes](https://manual.gromacs.org/current/user-guide/mdrun-performance.html#parallelization-schemes)
which includes vectorisation, OpenMP, an MPI library implemented on top of threading,
MPI, GPU acceleration (which we will discuss in a later section) and a hybrid model
combining MPI and OpenMP.

A not-so-good example is [SAMtools](http://www.htslib.org/), a bio-informatics tool. 
The default settings are downright poor and not even suitable anymore for a modern PC.
E.g., the sort tool runs by default still on a single thread and will not even try to
detect multiple cores in the computer, and the default amount of memory per thread is
even in the newest version still a meager 768 MB. So if a naive user is using this
code and reserving a supercomputer node for it, the code will actually use only a 
fraction of the available memory and CPU capacity and use an out-of-core memory 
instead for large problems, using space on disk as work space, slowing down the
operation a lot. Even though distributed sorting algorithms were already developed
in the '80s, SAMtools still does not support distributed memory computing
(though for the typical problem sizes the code is used for this may not yet be a 
big problems).
