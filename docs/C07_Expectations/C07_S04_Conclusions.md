# Conclusions of this chapter

It is important to realise that not all codes are created equal, even if
they implement the same operation or algorithm. E.g., a code using 
matrix-matrix multiplication internally may itself implement one of our
six variants, or it could be doing the right thing and use an optimised
BLAS library. However, even then we still may be unlucky if that code
only comes as a binary and would be using an older version of the BLAS
library that does not yet contain a proper code path for the CPU you're using.

As we have seen, it is also important to not reinvent the wheel. It is very
likely tha there are already good libraries that you can use for time-consuming
parts of your code, or maybe even a complete code for your problem that is
much better than anything you can come up with in a short time. Writing a proper
BLAS3 library is a Ph.D. thesis by itself...

The matrix multiplication illustration also clearly shows how important it is
to pay attention to the memory access order and to exploit the memory hierarchy.
The first was already illustrated in our six variants, but the optimised BLAS
implementation also shows gain from really taking into account the cache
hierarchy and optimise for the sizes of the caches using clever blocking
inside the code. 

With respect to speed-up and efficiency, the general rule is that for a fixed
problem size using more cores will lead to lower efficiency (though there are
some rare exceptions due to cache effects), while for a fixed number of cores
bigger problems will lead to higher efficiency. Supercomputers are made to 
solve big problems.

On a modern computer system, benchmarking has become very difficult as there are
so many unexpected elements that can influence performance in sometimes 
unpredictable or unexpected ways. As a user you may have control over some of
these factors (e.g., modern clusters sometimes allow users to set an upper limit
for the clock speed), but there are also factors one has no control over as 
one cannot simply ask for exclusive access to a big supercomputer.
