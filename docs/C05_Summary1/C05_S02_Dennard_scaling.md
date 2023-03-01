# Dennard scaling

For a long time, with every new generation of chip technology, roughly
every two years,
linear dimensions decreased by 30% (x0.7),
and surface dimensions decreased by 50% (x0.7<sup>2</sup>), i.e.,
transistor density doubled.
Moreover the power density remained practically the same as voltage and currents
needed to drive the circuits also lowered proportional to linear dimensions.
Circuit delays went down by 30% (x0.7), so frequencies went up by
40% (0.7<sup>-1</sup>).

Though the first cracks already appeared some generations earlier
(see, e.g., the problems with the Prescott Pentium 4 variant from 2004, built 
on a 90 nm process), this really broke down around 2006. 
No longer did all dimensions of elements on an integrated circuit scale as well
and hence transistor density did not grow as fast anymore.
Moreover, the threshold voltage of semiconductors, the minimum voltage to let them
switch, became more relevant and put a lower boundary on how much voltages can be
reduced. Another element in the power consumption, leakage power, became more and
more important if not dominant. 
And capacitances and inductances are such that the clock frequencies don't go up
as fast anymore either. 

As a result of the breakdown of Dennard scaling, chips have become very hot and
power consumption of supercomputers has become a major concern.
Moreover, there is hardly any further speed increase anymore just from further 
reducing the component size, and in fact chip designers usually need to chose between
a slight speed increase and a slight lowering of the power consumption per transistor.
As a result of this designers need to look much harder for architectural improvements than
before for further speed increases.
The breakdown of dennard scaling is also part of the reason why latencies of various components
and subsystems do no longer improve much.

Transferring data has become the major source of power consumption in computers, more than
doing the actual computations. Nowadays it takes more power to transfer two numbers from 
one end of a chip to the other end than do a simple arithmetic operation with those numbers.

PCs already operate their hardware in the domain of Dennard scaling breakdown so there is no 
hope that one can design a single processor core that is much faster than the one in a PC in
the current state of technology.
