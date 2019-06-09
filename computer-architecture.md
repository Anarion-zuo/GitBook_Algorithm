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

#### Registers

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

### Tradeoffs

#### Complex vs. Simple Instructions

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

#### Instruction Length

- Fixed length: Length of all instructions are the same
  - Easier to decode single instruction in hardware
  - Easier to decode multiple instructions concurrently
    - Suppose there is a series of commands of a certain length. If the length of each command is known to the decoders, the decoders would smoothly decode the commands at the same time, while if the length is not known, the problem is somewhat troublesome, for they do not no where to begin and end.
    - Huffman’s encoding
  - Wasted bits in instructions (Why is this bad?)
  - Harder-to-extend ISA (how to add new instructions?)
- Variable length: Length of instructions different (determined by opcode and sub-opcode)
  - Compact encoding (Why is this good?)
  - More logic to decode a single instruction
  - Harder to decode multiple instructions concurrently
- Tradeoffs
  - Code size (memory space, bandwidth, latency) vs. hardware complexity
  - ISA extensibility and expressiveness vs. hardware complexity
  - Performance? Energy? Smaller code vs. ease of decode

#### Uniform Decode

- Uniform decode: Same bits in each instruction correspond to the same meaning
  - Opcode is always in the same location
  - Ditto operand specifiers, immediate values, …
  - Many RISC ISAs: Alpha, MIPS, SPARC
  - Easier decode, simpler hardware
  - Enables parallelism: generate target address before knowing the instruction is a branch
  - Restricts instruction format (fewer instructions?) or wastes space
- Non-uniform decode
  - E.g., opcode can be the 1st-7th byte in x86

There are certain differences for RISC and CISC:

- RISC
  - Simple instructions
  - Fixed length
  - Uniform decode
  - Few addressing modes
- CISC
  - Complex instructions
  - Variable length
  - Non-uniform decode
  - Many addressing modes

#### Number of Registers

- Affects:
  - Number of bits used for encoding register address
  - Number of values kept in fast storage (register file)
  - (uarch) Size, access time, power consumption of register file
- Large number of registers:
  - Enables better registers allocation (and optimizations) by compiler $\rightarrow$ fewer saves/restores
    - Larger space for frequently used variables
    - When there is one variable more than the number of registers, the variable will be pushed onto the stack, called the process of spilling, and store back to the registers, called the process of filling
  - Larger instruction size
    - More bits to encode more registers
  - Larger register file size
    - Consuming more power

#### Addressing Mdodes

- Addressing mode specifies how to obtain an operand of an instruction
  - Register
  - Immediate
  - Memory (displacement, register indirect, indexed, absolute, memory indirect, auto-increment, auto-decrement)
- More modes:
  - help better support programming constructs (arrays, pointer-based accesses)
  - make it harder for the architect design
  - too many choices for the compiler?
    - Many ways to do the same thing complicates compiler design
- Register indirect mode
  - Access register by address, $m[r_2]$
- Memory indirect mode
  - Access memory by address, $m[m[r_2]]$
- MIPS: Aligned Access
  - Alignment: the address of some variable should be multiple of the word size, $A\%N==0$
  - LW/SW alignment restriction:-byte word-alignment
    - not designed to fetch memory bytes not within a word boundary
    - not designed to rotate unaligned bytes into registers
  - Provide separate opcodes for the infrequent case
    - LWL/LWR is slower
      - LWL: Low-word-left, starting from low words and goes left
    - Note LWL and LWR still fetch with word boundary

### Food for Thought

- How would you design a new ISA?
- Where would you place it?
- What design choices would you make in terms of ISA properties?
- What would be the first question you ask in this process?

## Single-Cycle

### Introduction

#### Instruction Processing

How does a machine process instructions?

- What does processing an instruction mean?
- Remember the Von Neumann model

$$
\text{AS} = \text{Architecture (programmer visible) state before an instruction is processed}\\\Downarrow\\\text{Process Instruction}\\\Downarrow\\
\text{AS'} = \text{Architecture (programmer visible) state after an instruction is processed}
$$

The “Process instruction” Step:

- ISA specifies abstractly what AS’ should be, given an instruction and AS
  - It defines an abstract finite state machine where
    - State = programmer-visible state
    - Next-state logic = instruction execution specification
  - From ISA point of view, there are no “intermediate states” between AS and AS’ during instruction execution
    - One state transition per instruction
- Microarchitecture implements how AS is transformed to AS’
  - There are many choices in implementation
  - We can have programmer-invisible state to optimize the speed of instruction execution: multiple state transitions per instruction
    - Choice 1: AS $\rightarrow$ AS’ (transform AS to AS’ in a single clock cycle)
    - Choice 2: AS $\rightarrow$ AS+MS1 $\rightarrow$ AS+MS2 $\rightarrow$ AS+MS3 $\rightarrow$ AS’ (take multiple clock cycles to transform AS to AS’)

A single-cycle machine is a very basic instruction processing engine:

- Each instruction takes a single clock cycle to execute
- Only combinational logic is used to implement instruction execution
  - No intermediate, programmer-invisible state updates![1558165214766](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558165214766.png)
- What is the clock cycle time determined by?
  - The time of the longest instruction

Single-cycle vs. Multi-cycle:

- Single-cycle
  - Each instruction takes a single clock cycle
  - All state updates made at the end of an instruction’s execution
  - Big disadvantage: The slowest instruction determines cycle time $\rightarrow$ long clock cycle time
- Multi-cycle
  - Instruction processing broken into multiple cycles/stages
  - State updates can be made during an instruction’s execution
  - Architecture state updates made only at the end of an instruction’s execution
  - Advantage over single-cycle: The slowest “stage” determines cycle time
- Both single-cycle and multi-cycle machines literally follow the Von Neumann model at the microarchitecture level

#### Cycle

Instruction cycle:

- Instructions are processed under the direction of a “control unit” step by step
- Instruction cycle: Sequence of steps to process an instruction
- Fundamentally, there are 6 phases. Not all instructions require all six stages.
  - Fetch
  - Decode
  - Evaluate Address
  - Fetch Operands
  - Execute
  - Store Result

Instruction processing “cycle” vs. machine clock cycle:

- Single-cycle machine
  - All six phases of the instruction processing cycle take a single-machine clock to complete
- Multi-cycle machine
  - All six phases of the instruction processing cycle can take multiple machine clock cycles to complete
  - In fact, each phase can take multiple clock cycles to complete

#### Datapath

Another way of viewing instruction processing:

- Instruction transform Data (AS) to Data’ (AS’)
- This transformation is done by functional units
  - Units that “operate” on data
- These units need to be told what to do to the data
- An instruction processing engine consists of two components
  - Datapath: Consists of hardware elements that deal with and transform data signals
    - functional units that operate on data
    - hardware structures (e.g. wires and muxes) that enable the flow of data into the functional units and registers
    - storage units that store data (e.g. registers)

Single-cycle vs. Multi-cycle on Controlling and Data

- Single-cycle machine:
  - Control signals are generated in the same clock as the one during which data signals are operated on
  - Everything related to an instruction happens in one clock cycle (serialized processing)
- Multi-cycle machine:
  - Control signals needed in the next cycle can be generated in the current cycle
  - Latency of control processing can be overlapped with latency of datapath operation (more parallelism)

Many ways of Datapath and Control Design

- There are many ways of designing the data path and control logic
- Single-cycle, multi-cycle, pipelined datapath and control
- Single-bus vs. multi-bus datapaths
- Hardwired/combinational vs. microcoded/microprogrammed control
  - Control signals generated by combinational logic versus
  - Control signals stored in a memory structure

#### Performance Analysis

- Execution time of an instruction
  - {CPI} $\times$ {clock cycle time}
  - CPI = cycles per instruction
- Execution time of a program
  - Sum over all instructions [{CPI} $\times$ {clock cycle time}]
  - {# of instructions} $\times$ {Average CPI} $\times$ {clock cycle time}
- Single-cycle microarchitecture performance
  - CPI = 1
  - Clock cycle time = long
- Multi-cycle microarchitecture performance
  - CPI is different for each instruction
    - Average CPI $\rightarrow$ hopefully small
  - Clock cycle time = short

### Closer Look

![1558165214766](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558165214766.png)

#### Assumptions

- “Magic” memory and register file
- Combinational read
  - output of the read data port is a combinational function of the register file contents and corresponding read select port
- Synchronous write
  - the selected register is updated on the positive edge clock transition when write enable is asserted
    - Cannot affect read output in between clock edges
- Single-cycle, synchronous memory
  - Contrast this with memory that tells when the data is ready
  - i.e. Ready bit: indicating the read or write is done

#### Instruction Processing

- 5 generic steps
  - Instruction fetch (IF)
  - Instruction decode and register operand fetch (ID/RF)
  - Execute/Evaluate memory address (EX/AG)
  - Memory operand fetch (MEM)
  - Store/writeback result (WB)

![1558171538298](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558171538298.png)

Full MIPS Datapath:
![1558171578619](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558171578619.png)

### Single-Cycle Datapath for Arithmetic and Logical Instructions

![1558172125649](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558172125649.png)

#### R-Type MIPS

R instructions are used when all the data values used by the instruction are located in registers. All R-type instructions have the following format:

```assembly
OP rd, rs, rt
```

For the case of additions, the rs and rt gives the location of the variables to be added and rd is the destination of the result.

```assembly
add $s1, $s2, $s3
```

in which case this is the same as:

```pseudocode
s1 = s2 + s3
```

Converting an R mnemonic into the equivalent binary machine code is performed in the following way:

| opcode | rs     | rt     | rd     | shift (shamt) | funct  |
| ------ | ------ | ------ | ------ | ------------- | ------ |
| 6 bits | 5 bits | 5 bits | 5 bits | 5 bits        | 6 bits |

- opcode

  The opcode is the machine code representation of the instruction mnemonic. Several related instructions can have the same opcode. The opcode field is 6 bits long (bit 26 to bit 31).

- rs, rt, rd

  The numeric representations of the source registers and the destination register. These numbers correspond to the \$X representation of a register, such as \$0 or ​\$31. Each of these fields is 5 bits long. (25 to 21, 20 to 16, and 15 to 11, respectively). Interestingly, rather than rs and rt being named r1 and r2 (for source register 1 and 2), the registers were named "rs" and "rt" for register source, register target and register destination.

- Shift (shamt)

  Used with the shift and rotate instructions, this is the amount by which the source operand *rs* is rotated/shifted. This field is 5 bits long (6 to 10).

- Funct

  For instructions that share an opcode, the **funct** parameter contains the necessary control codes to differentiate the different instructions. 6 bits long (0 to 5). Example: Opcode 0x00 accesses the ALU, and the funct selects which ALU function to use.

#### R-Type ALU Instructions

- Assembly (e.g. register-register signed addition)

  ```assembly
  ADD rd_reg rs_reg rt_reg
  ```

- Machine encoding

  - ![1558171775794](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558171775794.png)
  - Notice: the number of bits on each block in the picture above correspond to the number of required bits for the address of the registers, as is shown in the pictures to come, having number notations 25-21, 20-16, 15-11

- Semantics
```pseudocode
if MEM[PC] == ADD rd rs rt
    GPR[rd] = GPR[rs] + GPR[rt]
    PC = PC + 4
```
where GPR is “general purpose register”.

If the current program counter points to a command as above, add the values in the registers accordingly and store the result accordingly, while increment the program counter at the same time.

![1558172092675](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558172092675.png)

#### I-Type MIPS

I instructions are used when there is immediate values involved. The immediate value may be a maximum of 16 bits long and no larger.

```assembly
OP rt, IMM(rs)
```

Or another form:

```assembly
OP rt, rs, IMM
```

Where rt is the target register, rs is the source register and IMM is the immediate value. Hence, addi instruction may be called as:

```assembly
addi $s1, $s2, 100
```

meaning:

```
s1 = s2 + 100
```

the value of s2 plus 100 is stored in s1.

I instructions are converted into machine code words in the following format:

| opcode | rs     | rt     | IMM     |
| ------ | ------ | ------ | ------- |
| 6 bits | 5 bits | 5 bits | 16 bits |

- Opcode

  The 6-bit opcode of the instruction. In I instructions, all mnemonics have a one-to-one correspondence with the underlying opcodes. This is because there is no **funct** parameter to differentiate instructions with an identical opcode. 6 bits (26 to 31)

- rs, rt

  The source and target register operands, respectively. 5 bits each (21 to 25 and 16 to 20, respectively).

- IMM

  The 16 bit immediate value. 16 bits (0 to 15). This value is usually used as the offset value in various instructions, and depending on the instruction, may be expressed in two's complement.

#### I-Type ALU Instructions

- Assembly (e.g. register-immediate signed additions)

  ```pseudocode
  ADDI rt_reg rs_reg immediate_16
  ```

- Machine encoding

![1558172367214](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558172367214.png)

- Semantics

```pseudocode
if MEM[PC] == ADDI rt rs immediate
	GPR[rt] = GPR[rs] + sign-extend(immediate)
	PC = PC + 4
```

This is very similar to the R-type, only changing the source of the second operand of addition to a immediate value.

![1558172681382](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558172681382.png)

### Single-Cycle Datapath for Data Movement Instructions

#### MIPS Load and Store

```assembly
LW $t, C($s)
SW $t, C($s)
```

LW loads from memory C(\$s) to register \$t, while SW saves from register \$t to memory C(\$t).

#### Load Instructions

- Assembly (e.g. load 4-byte word)

```assembly
LW rt_reg offset_16 (base_reg)
```

- Machine encoding

![1558179537273](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558179537273.png)

- Semantics

```pseudocode
if MEM[PC] == LW rt offset_16 (base)
	EA = sign-extend(offset) + GPR[base]
	GPR[rt] = MEM[translate(EA)]
	PC = PC + 4
```

![1558179730585](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558179730585.png)

#### Store Instructions

- Assembly (e.g. store 4-byte word)

```assembly
SW rt_reg offset_16 (base_reg)
```

- Machine encoding

![1558179974052](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558179974052.png)

- Semantics

```pseudocode
if MEM[PC] == SW rt offset_16 (base)
	EA = sign-extend(offset) + GPR[base]
	MEM[translate(EA)] = GPR[rt]
	PC = PC + 4
```

![1558180053762](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558180053762.png)

#### Load-Store Datapath

![1558180687150](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558180687150.png)

#### Datapath for Non-Control-Flow Instructions

![1558180733302](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558180733302.png)

### Control Flow Instructions

#### J-Type MIPS

J instructions are used when a jump needs to be performed. The J instruction has the most space for an immediate value, because addresses are larger numbers.

```assembly
OP LABEL
```

where OP is the mnemonic for the particular jump instruction, and LABEL is the target address to jump to.

J instructions have the following machine-code format:

| Opcode | Pseudo-Address |
| ------ | -------------- |
|        |                |

- Opcode

  The 6 bit opcode corresponding to the particular jump command. (26 to 31).

- Address

  A 26-bit shortened address of the destination. (0 to 25). The two least significant bits are removed, and the 4 most significant bits are removed, and assumed to be the same as the current instruction's address.

#### MIPS Jump

Instruction j or jr with let the program counter jump to the specified location. The difference between them is that J takes immediate value and JR takes register, or in other word, J is the J-type instruction and JR is the R-type instruction.

```assembly
J IMM
JR $t0
```

We can do more than jumping. Jump and Link instructions are similar to the jump instructions, except that they store the address of the next instruction (the one immediately after the jump) in the return address (\$ra; \$31) register. This allows a subroutine to return to the main body routine after completion. JAL is a J-type instruction and JALR is a R-type instruction.

```assembly
JAL IMM
JALR $t0
```

#### Unconditional Jump Instructions

In C, an unconditional jump is any “break”, “continue”, “goto”, “return”, which is not in the body of an “if” or “else” or “case”. A conditional jump requires other information such as a boolean flag. Therefore, unconditional jumps do not read from any register or memory except for the instructions themselves, while conditional jumps do. The location of a unconditional jump is and must be specified by the instruction.

- Assembly

```assembly
J immediate_26
```

- Machine encoding

![1558181120866](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558181120866.png)

- Semantics

```pseudocode
if MEM[PC] == J immediate_26
	target = {PC[32:28], immediate_26, 2'b00}
	PC = target
```

The unconditional jump instruction concat the current program counter and the operand from the instruction to move the program counter to the right place.

![1558181328270](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558181328270.png)

As is mentioned above, conditional jumps read from registers and memory, or only registers, for efficiency.

![1558181854202](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558181854202.png)

#### MIPS Branch

Instead of using rt as a destination operand, rs and rt are both used as source operands and the immediate is sign extended and added to the PC to calculate the address of the instruction to jump to if the branch is taken. The instructions are all I-type. The +4 is not always necessary.

```assembly
BEQ rt, rs, IMM
```

Branch if rs and rt are equal.

```pseudocode
if rs == rt
	PC = PC + 4 + IMM
else
	PC = PC + 4
```

```assembly
BNE rt, rs, IMM
```

Branch if rs and rt are not equal.

```pseudocode
if rs != rt
	PC = PC + 4 + IMM
else
	PC = PC + 4
```

```assembly
BGEZ rt, rs, IMM
```

Branch if rs is greater or equal to 0.

```pseudocode
if rs >= 0
	PC = PC + 4 + IMM
else
	PC = PC + 4
```

```assembly
BLEZ rt, rs, IMM
```

Branch if rs is less than or equal to 0.

```pseudocode
if rs <= 0
	PC = PC + 4 + IMM
else
	PC = PC + 4
```

```assembly
BGTZ rt, rs, IMM
```

Branch if rs is greater than 0.

```pseudocode
if rx > 0
	PC + PC + 4 + IMM
else
	PC = PC + 4
```

```assembly
BLTZ rt, rs, IMM
```

Branch if rs is less than 0.

```pseudocode
if rs < 0
	PC = PC + 4 + IMM
else
	PC = PC + 4
```

#### Conditional Branch Instructions

The implementation of BEQ:

![1558188861786](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558188861786.png)

### Decode Control Signal

#### Single-Cycle Hardwired Control

- As combinational function of Inst = MEM[PC]

![1558189584129](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558189584129.png)

- Consider
  - All R-type and I-type ALU instructions
  - LW, SW
  - BEQ, BNE, BLEZ, BGTZ
  - J, JR, JAL, JALR

#### Single-Bit Control Signals

|          | When Destination-asserted                           | When asserted                                        | Equation                                               |
| -------- | --------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------ |
| RegDest  | GPR write select according to rt, i.e., inst[20:16] | GPR write select according to rd, i.e., inst[15:11]  | opcode==0                                              |
| ALUSrc   | second ALU input from second GPR read port          | second ALU input from sign-extended 16-bit immediate | (opcode!=0)&&(opcode!=BEQ)&&(opcode!=BNE)              |
| MemtoReg | Steer ALU result to GPR write port                  | steer memory load to GPR wr. port                    | opcode==LW                                             |
| RegWrite | GPR write disabled                                  | GPR write enabled                                    | (opcode!=SW)&&(opcode!=Bxx)&&(opcode!=J)&&(opcode!=JR) |
| MemRead  | Memory read disabled                                | Memory read port return load value                   | opcode==LW                                             |
| MemWrite | Memory write disabled                               | Memory write enabled                                 | opcode==SW                                             |
| PCSrc1   | According to PCSrc2                                 | next PC is based on 26-bit immediate jump target     | (opcode==J)                                            |
| PCSrc2   | next PC = PC + 4                                    | next PC is based on 16-bit immediate branch target   | (opcode==Bxx)&&“bcond is satisfied”                    |

### ALU Control

- case opcode
  - ‘0’  select operation according to funct
  - ‘ALUi’  selection operation according to opcode
  - ‘LW’  select addition
  - ‘SW’  select addition
  - ‘Bxx’  select bcond generation function
  - __  don’t care
- Example ALU operations
  - ADD, SUB, AND, OR, XOR, NOR, etc.
  - bcond on equal, not equal, LE zero, GT zero, etc.

R-type:

![1558257391210](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558257391210.png)

#### Control Box

- Combinational Logic $\rightarrow$ Hardware Control
  - Idea: Control signals generated combinationally based on instruction
  - Necessary in a single-cycle microarchitecture…
- Sequential Logic $\rightarrow$ Sequential/Microprogrammed Control
  - Idea: A memory structure contains the control signals associated with an instruction
  - Control Store

### Microarchitecture

#### Analysis

- Every instruction takes 1 cycle to execute
  - CPI (Cycles per instruction) is only 1
- How long each instruction takes is determined by how long the slowest instruction takes to execute
  - Even though many instructions do not need that long to execute
- Clock cycle time of the microarchitecture is determined by how long it takes to complete the slowest instrucion
  - Critical path of the design is determined by the processing time of the slowest instruction

What is the slowest instruction to process?

- Memory is not magic
- What if memory sometimes takes 100ms to access?
- Does it make sense to have a simple register to register add or jump to take {100ms + all else} to do a memory operation?
- And, what if you need to access memory more than once to process an instruction?

#### Single Cycle uArch Complexity

- Contrived
  - All instructions run as slow as the slowest instruction
- Inefficient
  - All instructions run as slow as the slowest instruction
  - Must provide worst-case combinational resources in parallel as required by any instruction
  - Need to replicate a resource if it is needed more than once by an instruction during different parts of the instruction processing cycle
- Not necessarily the simplest way to implement an ISA
  - Single-cycle implementation of REP MOVS (x86) or INDEX (VAX)?
- Not easy to optimize/improve performance
  - Optimizing the common case does not work (e.g. common instructions)

#### Microarchitecture Design Principles

- Critical path design
  - Find the maximum combinational logic delay and decrease it
  - Break a path into multiple cycles if it takes too long

The power consumed by a microprocessor may be expressed in this way:
$$
P\propto CV^2f
$$
where frequency is more or less correlated to the voltage, causing a cubic relation between power and frequency of clock. By minimizing these factors, or the proportion factor, which is determined by the property of the wires, we can lower the power consumption, thus achieve critical design.

- Bread and butter (common case) design
  - Spend time and resources on where it matters
    - i.e., improve what the machine is really designed to do
  - Common case vs. uncommon case
    - Common cases are what the system mostly do, therefore matters
- Balanced design
  - Balanced instruction/data flow through hardware components
  - Design to eliminate bottlenecks: balance the hardware for the work
    - The clock cycle cannot be too fast so that the slow operations may be fully done

## Multi-Cycle

### Introduction

![1558267573535](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558267573535.png)

- Goal: Let each instruction take (close to) only as much time it really needs
- Idea
  - Determine clock cycle time independently of instruction processing time
  - Each instruction takes as many clock cycles as it needs to take
    - Multiple state transitions per instruction
    - The states followed by each instruction is different
- ISA specifies abstractly what AS’ should be, given an instruction and AS
  - It defines an abstract finite state machine where
    - State = programmer-visible state
    - Next-state logic = instruction execution specification
  - From ISA point of view, there are no “intermediate states” between AS and AS’ during instruction execution
    - One state transition per instruction
- Microarchitecture implements how AS is transformed to AS’
  - There are many choices in implementation
  - We can have programmer-invisible state to optimize the speed of instruction execution: multiple state transitions per instruction
    - Choice 1: AS $\rightarrow$ AS’ (transform AS to AS’ in a single clock cycle)
    - Choice 2: AS $\rightarrow$ AS+MS1 $\rightarrow$ AS+MS2 $\rightarrow$ AS+MS3 $\rightarrow$ AS’ (take multiple clock cycles to transform AS to AS’)

$$
\text{AS}=\text{Architectural (programmer visible) state at the beginning of an instruction}\\\Downarrow\\
\text{Step1: Process part of instruction in one clock cycle}\\\Downarrow\\
\text{Step2: Process part of instruction the next clock cycle}\\\Downarrow\\\vdots\\\Downarrow\\\text{AS'}=\text{
Architectural (programmer visible) state at the end of a clock cycle
}
$$

#### Benefits of Multi-Cycle Design

- Critical path design
  - Can keep reducing the critical path independently of the worst-case processing time of any instruction
- Bread and butter (common case) design
  - Can optimize the number of states it takes to execute “important” instructions that make up much of the execution time
- Balanced design
  - No need to provide more capability or resources than really needed
    - An instruction that needs resource X multiple times does not require multiple X’s to be implemented
    - Leads to more efficient hardware: Can reuse hardware components needed multiple times for an instruction

#### Microprogrammed Multi-Cycle uArch

- Key Idea for Realization
  - One can implement the “process instruction” step as a finite state machine that sequences between states and eventually returns back to the “fetch instruction” state
  - A state is defined by the control signals asserted in it
  - Control signals for the next state are determined in current state
- Instruction Processing Cycle
  - Fetch
  - Decode
  - Evaluate Address
  - Fetch Operands
  - Execute
  - Store Result
  - Repeat them all

#### A Basic Multi-Cycle Microarchitecture

- Instruction processing cycle divided into “states”
  - A stage in the instruction processing cycle can take multiple states
- A multi-cycle microarchitecture sequences from state to state process an instruction
  - The behavior of the machine in a state is completely determined by control signals in that state
- The behavior of the entire processor is specified fully by a finite state machine
- In a state (clock cycle), control signals control 2 things:
  - How the datapath should process the data
  - How to generate the control signals for the next clock cycle

#### Terminology

- Control signals associated with the current state
  - Microinstruction
- Act of transitioning from one state to another
  - Determining the next state and the microinstruction for the next state
  - Microsequencing
- Control store stores control signals for every possible state
  - Store for microinstructions for the entire FSM
- Microsequencer determines which set of control signals will be used in the next clock cycle (i.e., next state)

#### What Happens in a Clock Cycle?

- The control signals (microinstruction) for the current state control 2 things
  - Processing in the data path
  - Generation of control signals (microinstruction) for the next cycle
- Datapath and microsequencer operate concurrently
- Question: why not generate control signals for the current cycle in the current cycle?
  - This will lengthen the clock cycle
  - Why would it lengthen the clock cycle?
  - It cannot use the advantage of datapath and microsequencer operating concurrently, as is shown below:

![1558274143435](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558274143435.png)

![1558274214943](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558274214943.png)

#### A Simple LC-3b Control and Datapath

![1558274650534](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558274650534.png)

- R for ready flag. If the memory is not ready, the system waits.
- BEN for branch enabled.

What determines next-state control signals?

- What is happening in the current clock cycle
  - See the 9 control signals coming form “Control” block
- The instruction that is being executed
  - IR[15:11] coming from the Data Path
- Whether the condition of a branch is met, if the instruction being processed is a branch

The state machine for multi-cycle processing:

- The behavior of the LC-3b uarch is completely determined by
  - the 35 control signals and
  - additional 7 bits that go into the control logic from the datapath
- 32 control signals completely describes the state of the control structure
- We can completely describe the behavior of the LC-3b as a state machine, i.e., a directed graph of 
  - Nodes (one corresponding to each state)
  - Arcs (showing flow from each state to the next state(s))

