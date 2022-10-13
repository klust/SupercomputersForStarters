# Conclusions

## Cost-concious computing

Supercomputing does not come cheap. Some really big runs run on bigger
supercomputers may easily cost 10k EURO. Academic users typically don't
see the full bill. They either pay a minor contribution, or have to submit
a proposal to get the compute time, but someone has to pay the bill.
Given the high cost, it is clear that a supercomputer should be used
efficiently.

Unfortunately, using a supercomputer efficiently is not that trivial.
But as we have seen, even your work that you do on a regular PC or
workstation may benefit from it as after all, a modern PC has all the 
levels of parallelism in a supercomputer with the exception of the
distributed memory level and probably the NUMA characteristic of
the memory.

It is important to realise that a supercomputer is not a machine
that wil automatically and in a magical way run every program faster
than a PC, though at first it may look like there is a high degree of
compatibility with Linux PCs. (And that even works in the other direction:
it is not hard to install some of the middleware used on a supercomputer
also on your PC to, e.g., test distributed memory programs.)
A supercomputer is no excuse for lousy programming. One needs to first
optimise a program and then, if the cheaper hardware is still not capable
of running it properly, move to a (larger) supercomputer, and not the 
other way around, first move to a supercomputer and then if it still doesn't
work think about developing better code.


## Cost-concious computing as a software user

It is important to select the software that you use with care and to follow
the evolutions in your field. Hardware evolves, and some software packages
evolve with the hardware while others stay behind.
EuroHPC, the European initiative for supercomputer, adn the USA funding agencies
and in particular the national labs, invest a lot of resources into improving
or rewriting popular scientific software packages, and these are often available
as open source or free to license for academic research.
The most hyped package or technology may not always be the best one for your needs.
But the package that your Ph.D. advisor used 25 years ago for their research 
may not be adequate anymore either.

It is also important to learn to use the software packages efficiently,
with a balance between execution efficiency and time-to-solution.
Most packages, and this is even more true for the free ones, have no
auto-tune facility. Users need to do that work instead.
For a numerical simulation it is also important to understand the limits
of the models (accuracy-wise) and of the numerics. Asking for more precision
than makes sense given the errors in the model and the stability of the numerical
algorithms may come at a very high cost or even failed runs.


## Cost-concious computing as a developer

Prototype languages such as Matlab are really meant for prototyping algorithms
but not for production runs at scale, though the situations is somewhat improving
with some languages having decent just-in-time compilation features or being more
designed for efficient execution on modern hardware than only easy of programming.
[Julia](https://julialang.org/) is a modern language developed at MIT that 
offers much of the ease-of-use of Matlab but where being able to compile the 
program to efficient computer code was taken into account when designing the
language. In fact, some users who have translated existing code to Julia mention 
that they hardly lost speed compared to Fortran while their Matlab and Python
codes run orders of magnitude faster when rewritten in Julia.

The same holds for languages meant for scripting, with Python the most used one
in scientific computing. Such languages were never designed for efficiency and
for tasks that execute continuously and require a lot of CPU time. 
Python can be acceptable when it is used to bind modules together that themselves
each are highly optimised C/C++ code, but it is very hard to get pure Python code
to work efficiently. There have been several efforts to try to write just-in-time
compilers that can increase the efficiency of pure Python code, but none of the
efforts has succeeded in making one that is fairly universal. An example that is
somewhat popular in scientific computing is [Numba](https://numba.pydata.org/),
that seems to work well especially with code that uses data structures from
NumPy. (Numba is in fact based on the LLVM compiler back-end that was mentioned
so often in these nodes.) Here again, Julia can be a nice alternative.

## Why should you care?

A typical reaction from a Ph.D. student could be: *"Why should I care? I need
to write my Ph.D. as fast as possible, produce papers, and by the way, it's (almost) free,
isn't it?"*

Resources are not infinite and most clusters run at a fairly high load.
On bigger machines, a user typically gets a certain allocation (in terms of
core-hours etc.) and can only use that much. On smaller supercomputers,
at the level of a university, a fair share policy is often implemented to
come to a somewhat reasonable distribution of resources. A fair share policy
typically implies that users or groups who have used a lot of computer time
recently, will get a lower priority.  Few if no supercomputers will use a
first come, first served policy which would allow you to simply dump a lot
of work onto the machine and not care at all.

Moreover, funding agencies in the end have to pay the bill and they will also
look at the outcome of the research done on a supercomputer before deciding
to fund the next one. So the user community can better optimise the scientific
return and often economic return of a supercomputer to ensure funding for 
the next machine. The way in which research can be made usable for companies
is becomeing more and more important. And for companies, compute time comes
at a high cost so they are not often interested in using inefficient procedures
to solve their problems.

We also live in a time where energy comes with an increasing cost. One should 
realise that supercomputers do consume a lot of energy. A supercomputer
consuming only 100 kW is still small, and the biggest machines on earth at the
moment are closer to 30 MW. The supercomputer world is looking at many ways
to lower the energy consumption. Software that uses hardware more efficiently
or uses more efficient hardware (such as accelerators) is often the easiest
route to go...  Hardware designers have already developed ways to cool 
supercomputers with water that is so warm even when it enters the 
computer that "free cooling" can be used to cool the hot water that leaves
the computer, i.e., just radiators and fans to pass the air over the radiator.
Some centres are experimenting with heat recovery for the purpose of heating
their buildings. 
The waste hear of LUMI, a European pre-exascale machine installed in Kajaani,
Finland, is used to [produce heat for the city of Kajaani](https://lumi-supercomputer.eu/the-waste-energy-of-lumi-supercomputer-produces-20-percent-of-the-district-heat-of-kajaani-csc-and-loiste-lampo-have-signed-an-agreement/).

Another reaction one sometimes hears is *"But by the time I've reworked
my code or rolled out a better application in my research, faster computers
will be around so I don't need to care anymore."*

This was to some extent true from roughly 1980 to 2005 if you look at 
personal computers. It was as if Intel engineers were optimising 
your program while you were having lunch or going to a party, by
designing better, faster processors. In those days, code would run
considerably faster on a new processor even without recompiling,
though recompiling would improve that even more. However, these
days are long over. Around 2005, further increasing the clock
speed became very hard. We've moved from 4 MHz in 1980 to 
almost 4 GHz in 2005, but from then on evolution slowed down
(and took even a step back for a while) and now we are somewhere
around 5.5 GHz for processors that are really optimised for sequential
code and consume a lot of power. It also became harder and harder
to get gains from increased instruction level parallelism. Instead of
that, we saw growing popularity of vector instructions and the use
of multiple cores. 8 to 16 cores is not uncommon anymore in regular
PC's, and the first 24-core PC processor was announced in late 2022.
So the further speed increases almost exclusively came from
other levels of parallelism that require more effort from the programmer:
vector instructions and more cores in shared memory computing, and in
supercomputing, more nodes in distributed memory.

Nowadays even that is slowing down. The number of transistors that
can be put on a chip is no longer growing as fast, the power consumption
per transistor is no longer decreasing much with every new generation,
and the cost of a transistor isn't really decreasing anymore, but in 
fact is starting to increase again when moving to a smaller manufacturing process.

The focus in computing is more and more on performance per Watt as
energy bills become prohibitively high for supercomputer centres and 
centres operating large server farms or as portability matters as in 
smartphones, where you want more and more performance while the battery
capacity isn't increasing that quickly. This implies more but slower
cores, so more need to exploit parallelism and less single-thread performance.
There are only a few markets that don't care much about this, but those
markets are becoming too small to carry the cost of designing faster and 
faster processors with limited parallelism.

