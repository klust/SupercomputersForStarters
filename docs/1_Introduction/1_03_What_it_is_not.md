# What supercomputing is not

Around 2010 the author of these notes got a phone call from a prospective user
with the question: "I have a Turbo Pascal program that runs too slow on my PC,
so I want to run it on the supercomputer." Now Turbo Pascal is a programming
language that was very popular in the '80s and the early '90s of the previous
centuty. It is almost a miracle that the program even still ran on the PC of that
user, and it is also very limited in the amount of memory a program can use.

Assuming that prospective user had a modern PC on their desk, and forgetting for a
while that Turbo Pascal generates software that cannot even run on Linux, the OS of
almost all supercomputers nowadays, the program would still not run any faster on
the supercomputer for reasons that we will explain later in these lecture notes.

The reader should realise that supercomputers only become supercomputers when running software
developed for supercomputers. This was different in the '60s and early '70s of the previous
century, the early days of supercomputing. In those days one had smaller computers with 
slower processors, and bigger machines with much faster processors, but one could essentially
recompile the software for the faster machine if needed. That started to change in the second
half of the '70s, with the advent of so-called vectorsupercomputers. There one really needed
to adapt software to extract the full performance of the machine. Since then things only got
worse. Due to physical limitations it is simply not possible to build ever faster processors,
so modern supercomputers are instead parallel computers combining thousands of regular processors
that are not that different from those in a PC to get the job done. And software will not automatically
use all these processors in an efficient way without effort from the programmer.

Yet there is no need to be too pessimistic either. 
In some cases, in particular capacity computing, the efforts to get your job running efficiently can 
be relatively minor and really just require setting up a parallel workflow to run a sufficient number
of cases simultaneously. But developing code for capability computing is much more difficult, but then
a supercomputer can let you compute results that you could not obtain in any other way. And in many cases
someone else has done the work already for you and good software is already available.
