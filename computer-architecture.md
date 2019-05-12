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

### ISA

![1557643152536](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1557643152536.png)

ISA is agreed upon interface between software and hardware. It is what the software writer needs to know to write and debug system/user programs. Microarchitecture is a specific implementation of an ISA, not visible to the software. A microprocessor is ISA plus uarch plus circuits. Its architecture is ISA plus microachitecture.

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
- Multi-treading support, Multiprocessor support

Microarchitecture content:

- Implementation of the ISA under specific design constraints and goals
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

Do we need an instruction pointer in the ISA?

- Yes: Control-driven, sequential execution
  - An instruction is executed when IP points to it.
  - IP automatically changes sequentially (except for control flow instructions).
- No: Data-driven, parallel execution
  - An instruction is executed when all its operand values are available (data flow).

Tradeoffs: many high-level ones

- Ease of programming (for average programmers)?
- Ease for compilation?
- Performance: Extraction of parallelism?
- Hardware complexity?