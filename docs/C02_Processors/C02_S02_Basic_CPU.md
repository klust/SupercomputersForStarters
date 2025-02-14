# A very basic CPU

Let us start with a look at a very basic computer. It has two important components.
The processor executes simple instructions, e.g., adding two numbers. The memory 
stores the data in a linear structure. A clock governs the pace at which instructions
are processed.

<figure markdown>
  ![The basic CPU](../img/C02_S02_01_Simple_proc.png)
  <caption>A basic computer</caption>
</figure>

Currently a processor is just as single chip, or in fact, just a small part of
a chip, but this was very different in the early days of computers based on chips.
Fast processors often consisted of multiple chips, and sometimes even thousands of
them.

!!! Note
    The Cray 1, a supercomputer from 1976, had a processor built out of
    20,000 chips. The memory was built out of 73,278 chips.
    The mean time between failure was 50 hours, so whenever you
    bought such a machine you got a lot of spare parts with it and
    on-site technicians to replace the parts.

Let us now open up our simple processor and have a look inside.

<figure markdown>
  ![Inside the basic CPU](../img/C02_S02_02_Simple_proc_exploded.png)
  <caption>A look inside the processor</caption>
</figure>

It has several building blocks. 
The ALU or Arithmetic and Logical Unit is the unit that does the 
actual computations.
The registers are a very small block of local memory. On modern 
computers, the ALU can only use data that is already in one of the
registers and will also write results to the registers. 40 years ago
or more this was not always the case and some processors could operate
directly on data from memory.
Another important block is the Address Generation Unit with
load/store unit and and the memory controller. This is the connection
between the main memory and the registers. All data passes through
the load/store unit with the AGU generating the actual address.
The control unit is the part that coordinates all the work.

In our very simple processor, instructions are executed one after
another. But executing each instruction itself consists of multiple steps. Consider, e.g.,
this oversimplified execution pattern distinguishing a few important steps for executing
an instruction in the ALU:

![Instruction execution](../img/C02_S02_03_Execution.png)

Instructions are fetched from memory and decoded in the first step. 
In the second step, the operands of the instruction (the numbers on which it works)
are transported from the registers to the ALU. In the next step the actual instruction
is executed, e.g., two numbers are added. And in the last step the result is stored back
in a register. Instructions involving storage would also consist out of multiple steps.
The steps are synchronised with a clock. So in this simple example executing an instruction
would take 4 clock ticks, so we could also say that we do 0.25 instructions per clock.

Note however that different steps use different parts of the logic in the processor,
and this is the key to improving performance.

!!! Note
    In the past there were computers that worked with data straight from memory without
    registers. Current Intel CPUs still have for compatibility reasons with the older 
    generation chips, instructions that get data from memory and in the same instruction 
    also use it for an arithmetic or logic operation, but in modern implementations these instructions are 
    broken up in the hardware into an instruction that transfers between main memory
    and memory in the register space, and a second instruction that does the arithmetic
    or logic operation. So the above picture with instructions that work on data in 
    registers and instructions that transfer data between registers and main memory, is
    accurate enough to understand the basics of modern computing.
