# Computer Architecture

## Fundamental Concepts

What is a computer:

- Computation
- Communication
- Storage (memory)

![1557631837501](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557631837501.png)

In the future, these parts may be merging with each other.

![1557632047394](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557632047394.png)

The Von Neumann Model/Architecture, also called stored program computer (instructions in memory), has 2 key properties:

- It has stored program.
  - Instructions stored in a linear memory array.
  - Memory is unified between a linear memory array. The interpretation of a stored value, telling whether it is a instruction or data, depends on the control signals.
- It has sequential instruction processing.
  - One instruction processed (fetched, executed, and completed) at a time.
  - Program counter (instruction pointer) identifies the current instruction.
  - Program counter is advanced sequentially except for control transfer instructions.

![1557632576875](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557632576875.png)

The Von Neumann model has instructions fetched and executed in control flow order, while we may have a “Dataflow model” having instructions fetched and executed in data flow order. Control flow order is the order of the program’s sequence, specified by the instruction pointer. It is sequential unless there is an explicit control flow instruction, leading it to another branch of a sequence of program.

### Data Flow Model

Data flow order has its operands ready and no instruction pointer. The ordering of instructions is specified by data flow dependence, namely, they wait for available input data to appear. Each instruction specifies “who” should receive the result and an instruction can “fire” (execute) whenever all operands are received. Potentially we can execute as many instructions as possible at the same time.

For example:

```pseudocode
v = a + b;
w = b * 2;
x = v - w;
y = v + w;
z = x * y;
```

The sequential order may be written and executed in this way, while the data flow version may be quite different.

![1557633231895](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557633231895.png)

When a and b is ready, the “plus” and “multiply” fires, then “minus” and “plus” fires, then “multiply” fires. In a data flow machine, a program consists of data flow nodes, which fire (fetched and executed) when all its inputs are ready, or all inputs have tokens.

![1557640228568](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557640228568.png)

We can build an entire programming language based on the nodes.

![1557640268860](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557640268860.png)

Barrier synchronization is to pause the threads and wait for all threads to complete some operations before they all proceed onto some next steps.

![1557640991202](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557640991202.png)

Do we need an instruction pointer in the ISA?

- Yes: Control-driven, sequential execution
  - An instruction is executed when IP points to it.
  - IP automatically changes sequentially (except for control flow instructions).
- No: Data-driven, parallel execution
  - An instruction is executed when all its operand values are available (data flow).

## ISA

### What is it

![1557643152536](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557643152536.png)

ISA is agreed upon interface between software and hardware. It is what the software writer needs to know to write and debug system/user programs. Microarchitecture is a specific implementation of an ISA, not visible to the software. A microprocessor is ISA (or uarch) plus circuits. Its architecture is ISA (or microachitecture).

### Basic Content

ISA Content:

- Instructions
  - Opcodes, Addressing Modes, Data Types
  - Instruction Types and Formats
  - Registers, Condition Codes
- Memory
  - Address space, Addressability, Alignment
  - Virtual memory management
- Call, Interrupt/Exception Handling
- Access Control, Priority/Privilege
- I/O: memory-mapped vs instructions
- Task/thread Management
- Power and Thermal Management
- Multi-threading support, Multiprocessor support

Microarchitecture content:

- Implementation of the ISA under specific design constraints and goals, uarch.
- Anything done in hardware without exposure to software
  - Pipelining
  - In-order versus out-of-order instruction execution
  - Memory access scheduling policy
  - Speculative execution
  - Superscalar processing (multiple instruction issue?)
  - Clock gating
  - Caching? Levels, size, associativity, replacement policy
  - Pre-fetching?
  - Voltage/frequency scaling?
  - Error correction?

The purpose of architecture is making tradeoffs.

Tradeoffs: many high-level ones

- Ease of programming (for average programmers)?
- Ease for compilation?
- Performance: Extraction of parallelism?
- Hardware complexity?

Design points of a system:

- The design points are a set of design considerations and their importance. It leads to tradeoffs in both ISA and uarch.
- Considerations are:
  - Cost
  - Performance
  - Maximum power consumption
  - Energy consumption
  - Availability
  - Reliability and Correctness
  - Time to Market

### Elements of ISA

#### Instructions

The elements of ISA are decided by instruction sequencing model and processing style.

- Instruction sequencing model
  - Control flow vs. data flow
  - Tradeoffs?
- Instruction processing style
  - This specifies the number of “operands” and instruction “operates” on and how it does so
  - n address machines
    - 0-address: stack machine (push/pop)
    - 1-address: accumulator machine (ACC id A, st A)
    - 2-address: 2-operand machine (one is both source and destiny)
    - 3-address: 3-operand machine (source and destiny separated)

![1557750368770](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557750368770.png)

For a stack machine, when we are trying to operate $((5+7)\times8)\times9$, we push the numbers onto the stack in the order of 9,8,7,5, so that the order of calculation is achieved.

#### Data Type

- Elements
  - Instructions
    - Opcode
    - Operand specifiers (addressing modes)
      - How to obtain the operands?
  - Data types
    - Definition: Representation of information for which there are instructions that operate on the representation.
    - Integer, floating point, character, binary, decimal, binary code decimal (BCD)
    - Doubly linked list, queue, string ,bit vector, stack

The tradeoffs of Data types:

- What is the benefit of having more or high-level data types in the ISA?
  - The programmers’ life get easier.
- What is the disadvantage?
  - The microarchitect gets so much more complex.

We introduce the concept of semantic gap. It measures the distance between the ISA and the high-level language. The tradeoffs of the magnitude of semantic gap is crucial to the Data type tradeoffs.

#### Memory Organization

- Address space is How many uniquely identifiable locations in memory.
- Addressability is How much data does each uniquely identifiable location stores.
  - Byte addressable: most ISAs, characters are 8 bits.
  - Bit addressable: Burroughs 1700, why?
    - Become a powerful optimizer, being able to run any ISA.
  - 64-bit addressable: Some supercomputers, why?
    - They operates on large data values.
  - 32-bit addressable: First alpha
  - Food for thought
    - How do you add 2 32-bit numbers with only byte addressability?
    - How do you add 2 8-bit numbers with only 32-bit addressability?
- Support for virtual memory.

### Registers

- How many?
- Size of each register
- Why is having registers a good idea?
  - Programs exhibit a characteristic called data locality. A recent produced/accessed value is likely to be used more than once (temporal locality).
    - Storing that value in a register eliminates the need to go to memory each time that value is needed.

Hence, we have a programmer visible (architecture) state and a programmer invisible state.

![1557752135191](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557752135191.png)

Basically, the instructions (and programs) specify how to transform the values of programmer visible state. The invisible state is where the programmers cannot make a difference to it.

#### Evolution of Register Architecture

- Accumulator
  - a legacy from the “addressing” machine days
  - One register only
- Accumulator + address registers
  - need register indirection
  - initially address registers were special-purpose, i.e., can only be loaded with an address for indirection.
  - eventually arithmetic on address became supported
- General purpose registers (GPR)
  - all registers good for all purposes
  - grew from a few register to 32 (common for RISC) to 128 in Intel IA-64

#### Instruction Classes

- Operate instructions
  - Process data: arithmetic and logical operations
  - Fetch operands, compute results, store results
  - Implicit sequential control flow
- Data movement instructions
  - Move data between memory, registers, I/O devices
  - implicit sequential control flow
- Control flow
  - Change the sequence of instructions that are executed

#### Load/store vs. Memory/memory Architectures

- Load/store: operate instructions operate only on registers
  - MIPS, ARM and many RISC ISAs
  - It does not mean that there is no registers, just not accessible.
- Memory/memory architecture: operate instructions can operate on memory locations
  - x86, VAX and many CISC ISAs

#### Different Addressing Modes

- Another example of programmer vs. microarchitect tradeoffs
- Advantage of more addressing modes:
  - Enables better mapping of high-level constructs to the machine: some access are better expressed with a different mode. More modes may result in reduced number of instructions and code size.
    - Think of array accesses (auto-increment mode)
    - Think of indirection (pointer chasing)
    - Sparse matrix accesses
  - array index, pointer reference, …
  - Good for high-level programmers
- Disadvantage
  - More work for the complier
  - More work for the microarchitect

#### Interface with I/O Devices

- Memory mapped I/O
  - A region of memory is mapped to I/O devices
  - I/O operations are loads and stores to those locations
- Special I/O instructions
  - IN and OUT instructions in x86 deal with ports of the chip
- Tradeoffs
  - Which one is more general purpose?
    - Memory mapping is obviously more important than interfaces with I/O

#### Others

- Privilege mode
  - User vs. supervisor
  - Who can execute what instructions?
- Exception and interrupt handlin
  - What procedure is followed when something goes wrong with an instruction?
  - What procedure is followed when an external device requests the processor?
  - Vectored vs. non-vectored interrupts (early MIPS)
- Virtual memory
  - Each program has the illusion of the entire memory space, which is greater than physical memory.
- Access protection

#### ISA Orthogonality

- Orthogonal ISA:
  - All addressing modes can be used with all instruction types
  - Example: VAX

### Tradeoff: Complex vs. Simple Instructions

- Complex instruction: An instruction does a lot of work, e.g. many operations
  - Insert to a doubly linked list
  - Compute FFT
  - String copy
- Simple instruction: An instruction does small amount of work, it is a primitive using which complex operations can be built on.
  - Add
  - XOR
  - Multiply
- Advantages of having Complex instructions
  - Denser encoding $\rightarrow$ smaller code size $\rightarrow$ better memory utilization, saves off-chip bandwidth, better cache hit rate (better packing of instructions)
  - Simpler compiler: no need to optimize small instructions as mush
- Disadvantages of having Complex instructions
  - Larger chunks of work $\rightarrow$ complier has less opportunity to optimize (limited in fine-grained optimizations it can do)
  - More complex hardware $\rightarrow$ translation from a high level to control signals and optimization needs to be done by hardware

The factor of tradeoffs is also measured by the semantic gap.

### Semantic Gap

- Simple compiler, complex hardware vs. complex compiler, simple hardware
  - Caveat: Translation (indirection) can change the tradeoff!
- Burden of backward compatibility
  - Old versions of programming language may not be compatible.
- Performance? Energy Consumption?
  - Optimization opportunity: Example of VAX INDEX instruction: who (compiler vs. hardware) puts more effort into optimization?
  - Instruction size, code size
- Small vs. Large

![1557760482424](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557760482424.png)