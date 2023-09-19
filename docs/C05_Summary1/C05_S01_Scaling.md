In this chapter we summarise the previous three chapters on processor, memory and storage technology.
We also want to stress that HPC stands for High Performance Computing and that a supercomputer is not 
like a very High-end Personal Computer.

# Scaling

The performance of a computer cannot be understood from a single parameter. Instead many
parameters characterise the performance of computer. The clock speed of a CPU is only one
of those parameters. There are also many lantencies that need to be taken into account:
memory latencies at the various levels of the memory hierarchy and of storage, communication
latency, but if you would study into more detail then is possible in these lecture notes how
to obtain maximal performance from a computer, also latencies in instruction execution.
And there is also the bandwidth of several components that has to be taken into account:
bandwidth to the various levels of the memory hierarchy and to storage, bandwidth between
CPUs in a shared memory system and bandwidth of the interconnect. And the number of
instructions a CPU can execute simultaneously is also a kind of bandwidth.
When you're interested in solving big problems, you're also interested in the capacity 
of memory and storage.

Not all these parameters are as cheap to scale, or improve over time at the same rate.
As we will also discuss a bit further in this chapter, physical limitations have put a bound to
improvements in CPU clock speed and latencies.
The finite speed of light (30 cm/ns in vacuum and roughly 20 cm/ns in glasfiber)
and speed of signals in copper wires is just one of those limitations.
The growth of the bandwidth of memory, disks and network connections tends to be slower
than the growth of the theoretical peak performance of a computer system.

As a result of these restrictions it is simply not possible to build a supercomputer were
all these parameters would be, e.g., 100 times better than in your PC or smartphone so that your PC software
would simply run and run 100x faster. 
In fact, the opposite may be true. For some applications a High-end PC is unbeatable because of 
its compact size and (though less so nowadays) thin software layer as it is a personal device,
as this guarantees minimal latencies. 
As we have seen in several examples, "bigger" often means higher latencies that need to be hidden.
E.g., a bigger memory has to sit physically further from the CPU so access will be slower. 
A bigger disk system needs a different way of managing it then the SSD that sits just next
to the processor in your PC and will be slower. We no longer build processors cores out of multiple
chips, not only because it is not really needed anymore, but if we did the clock speed would be low
as signals would travel too slowly to the other end of the processor core.


