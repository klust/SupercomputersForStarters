# Why supercomputing?

If supercomputing is that complex, why would one want to use it?

## When a PC or server fails...

Processing of large datasets may require more storage than a workstation or 
a departmental server can deliver, or it may require more bandwidth to 
memory or disk than a workstation or server can deliver, or simply more
processing power than a workstation can deliver. In many cases, a supercomputer
may help in those cases. However, it may require different or reworked
software.

Large simulations, e.g., partial differential equations in various fields of
physics, may require far more processing power than a workstation can deliver,
and often result in large datasets (which then brings us back to the previous
point). Supercomputers are often extremely suited for those very large simulations,
but again it may require different software or a significant reworking of the
software.

But there is also an easier case: applications such as parameter analysis or 
Monte Carlo sampling. In these cases we may want to run thousands of related
simulations with different input. A workstation may have more than enough
processing power to process a single or a few samples, but to work on
thousands of those we need a lot more processing power if we want to finish the
work in a reasonable amount of time. Here also supercomputer can come to the 
rescue.

## Supercomputing jobs

From the above discussion we already get the feeling that there are two big
reasons to use a supercomputer.

1.  We may want to use a supercomputer simply because we want to improve the
    turnaround time for a large computation, or because the computation is not
    even feasible on a smaller computer because of the required compute time 
    and memory capacity. This is also called **capability computing**. In
    capability computing one typically thinks in terms of hours per job.

2.  We may also want to use a supercomputer to improve throughput if we have to
    run a lot of smaller jobs. This is called **capacity computing**. In
    capacity computing one typically thinks in terms of jobs per hour.

Furthermore, we can distinguish simulation and data processing as two big domains
where supercomputing could be used. Some will add AI as a third pillar, but AI is
typically used for data processing and has requirements similar to the other data 
processing jobs we will discuss.

Supercomputers are really build for capability computing jobs. They can also accommodate
capacity computing jobs, but many capacity computing jobs could be run equally well on
just a network of servers, e.g., a cloud infrastructure, at possibly a much lower cost 
(should the user be charged a realistic amount for the compute resources consumed).

There are many examples of capability computing in simulation. Computational fluid dynamics
of turbulent and even many laminar flow requires a huge amount of compute capacity. The
demand becomes only higher when considering fluid-structure interactions causing, e.g., vibration
of the structure. Another example is virtual crash tests during the design of cars. 
That a car manufacturer can produce so much more different models than 30 years ago is partly
the result from the fact that developing a different body for a car is now much cheaper than
it was 30 years ago despite the more stringent safety rules, as far less prototypes need to
be build before having a successful crash tests thanks to the virtual crash tests. Weather and
climate modeling are also examples of capability computing, as is the simulation of large complex
molecules and their chemical reactions. 

But there are also examples of capacity computing in simulation. E.g., when doing a parameter study
that requires simulating a system for multiple sets of parameters. In drug design one often wants to test
a range of molecules for certain properties. The risk analysis computations done by banks are also
an example of capacity computing. Sometimes one really has a combination of capacity computing and
capability computing, e.g., when doing a parameter analysis for a more complex system.

Data processing can also lead to capability computing. One example is the visualisation of very large data sets
or simulation results, were a visualisation workstation may not be enough. Another example are the
electronic stamps offered by the US postal services, certainly in the initial years. Buyers of electronic
stamps would get codes that they can each use once, but of course US postal needs to check not only if a 
code is valid but also if it has not been used yet. And a clever abuser may share codes with someone 
at the opposite site of the country. Yet US postal has to be able to detect abuse in real time to not
slow down the sorting machines. For this they used a computer which around 2010 (when the author of these
notes learned about this application) could truly be considered a supercomputer.

Examples of capacity computing for data processing are many of the data mining applications that often 
consist of many rather independent tasks. The processing of the CERN Large Hadron Collider data is also
mostly capacity computing. Another example is a researcher from a Flemish university who used a VSC supercomputer
to pre-process a text corpus consisting of newspaper articles of many years. All texts had to be grammatically analysed. After the preprocessing, a regular
server was enough for the research.
