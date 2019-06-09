# Operating Systems

## Basics of Information

### Entropy

For any kind of information, it can be described by a certain discrete random variable $X$.

| $x_1$ | $x_2$ | …    | $x_N$ |
| ----- | ----- | ---- | ----- |
| $p_1$ | $p_2$ | …    | $p_N$ |

Hereby, we define uncertainty to be 1 over probability, so that higher the probability, less the uncertainty is. Furthermore, in order to measure uncertainty in bits, we take a logarithm based 2.
$$
I(x_i)=-\log_2p_i
$$
When facing $N$ choices and receiving data to narrow down the choices into $M$, say by telling you the symbol of the card is the heart, we measure the magnitude of such data in the form of:
$$
I(data)=\log_2\frac{1}{M\cdot(1/N)}=\log_2\frac{N}{M}
$$
In an example of flipping coins, $N=2,M=1$ , the magnitude of information content is $2$, which in binary, $\log_22=1​$ bit.

Entropy is the average amount of information contained in each piece of data received about the value of $X$.
$$
H(X)=E(I(X))=-\sum_ip_i\log_2p_i
$$
Suppose we have a data sequence describing the values of the random variable $X$, the average number of bits used to transmit our choices/data had better be equal to the entropy of the variable $X$. A large difference between entropy and average number of bits may imply inconsistency between the coding mechanism and the real world and may cause low efficiency or errors.

### Ambiguousity

Typically, when some way of coding may suggest ambiguous result, it is a bad way. For example, when we try to encode text with only 4 possibility of characters A, B, C, D, if use the following law, “0110” is ambiguous.

| A    | B    | C    | D    |
| ---- | ---- | ---- | ---- |
| 0    | 1    | 10   | 11   |

0110 may suggest “ABBA” or “ABC” or “ADA”.

To check whether the encoding is ambiguous, we draw a binary tree/decision tree to show the possible outcomes of decoding. 

![1553938967130](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1553938967130.png)

An unambiguous encoding is a binary tree with children with same depth. This is a typical ambiguous encoding.

### Variable-length Encodings

When encoding data we would like to match the length of the encoding to the information content of the data. For objects with higher probability of appearance, we give them shorter encodings, while longer for lower ones. This may save entropy and efficiency in the real world. Ideally, a perfect encoding would suggest encoding entropy equals real entropy, namely, the number of states of encoding equals the number of states of the real objects.

### Huffman’s Algorithm

The algorithm is for a given set of symbols and their probabilities, construct an optimal variable-lengths encoding.

- Build subtree using 2 symbols with lowest $p_i$.
- At each step, choose 2 symbols/subtrees with lowest $p_i$ combine to form new subtree.
- And it repeats.

The result is a optimal tree built from the bottom up.

![1553940340173](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1553940340173.png)

By the properties of probability, we can do better by encoding combinations of the symbols, instead of encoding single symbols.

### Hamming Distance

The factor describes the number of positions in which the corresponding digits differ in 2 encodings of the length.

### Parity Check to Detect Single(odd)-bit Errors

A parity bit can be added to any length message and is chosen to make the total number of 1 bits even (make a even parity). To check for a single-bit error/odd number of errors, count the number of 1s in the received message and if it is add, there has been an error.

To detect $E$ errors, we need a minimum Hamming Distance of $E+1$ between code words.

## Digital Abstraction

### Representing Information with Voltage

Suppose we can reliably distinguish voltages that differ by $1/2^N$ volts, we can represent $N$ bits of information by voltages in the range $0V$ to $1V$. In the real world, when transmitting analog signal between distant places, there is some error accumulating by distance. In order to build a perfect signal transmitting system, we introduce the method of digital abstraction.

By a intuitive way, voltages lower than a certain threshold signifies 0, while larger signifies 1. However, such an implementation is troubled by the condition at the threshold. Instead of having a single threshold value, we take 2 values to determine whether 0 or 1, and the problem is solved.

![1554008173351](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1554008173351.png)

In the red zone, it is forbidden to ask the device.

### Combinational Device

A combinational device is a circuit element follows the “static discipline”.

- Have digital inputs, which means the input follow the rules of digital abstraction.
- Have digital outputs.
- Have functional relation between all possible inputs and outputs.

A set of interconnected elements is a combinational device if:

- each circuit element is combinational.
- every input is connected to exactly one output or to some vast supply of constant 0’s and 1’s.
- the circuit contains no directed cycles.

### Noise Margins

We set 2 different rules for voltage transforming to digits for inputs and outputs
$$
digit(V)=\begin{cases}
1 & V\ge V_{IH}\\
0 & V\le V_{IL}
\end{cases},
digit(V)=\begin{cases}
1 & V\ge V_{OH}\\
0 & V\le V_{OL}
\end{cases}
$$
The noise margin is defined to be $V_{IL}-V_{OL}$ or $V_{OR}-V_{IR}$. It simply says how much noise added to the input signal could be tolerated for the output signal to remain unchanged.

![1554110223583](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1554110223583.png)

## CMOS Technology



## Synthesis of Combinational Logic

### Functional Specifications

There are many ways of specifying the function of a combinational device, for example: If C is 1, copy B to Y, otherwise copy A to Y. We want a way of precise implementation instead of descriptions in words. Therefore, we construct Truth Table for a boolean function. Concise alternatives:

- Truth Tables are a concise description of the combinational system’s function.
- Boolean expressions form an algebra whose operations are AND (multiplication), OR (addition), and inversion (overbar).

To construct a boolean function based on a truth table, we simply take every row with output 1 of the truth table and sum(or) them together. For example:

| C    | B    | A    | Y    |
| ---- | ---- | ---- | ---- |
| 1    | 1    | 0    | 1    |

$$
Y=CB\bar A
$$

The whole table:

| C    | B    | A    | Y    |
| ---- | ---- | ---- | ---- |
| 0    | 0    | 0    | 0    |
| 0    | 0    | 1    | 1    |
| 0    | 1    | 0    | 0    |
| 0    | 1    | 1    | 1    |
| 1    | 0    | 0    | 0    |
| 1    | 0    | 1    | 0    |
| 1    | 1    | 0    | 1    |
| 1    | 1    | 1    | 1    |

$$
Y=\bar C\bar BA+\bar C BA+CB\bar A+CBA
$$

When building circuits implementing boolean functions, we use multiple gates to construct a larger gate with more inputs.
$$
Z=A\cdot B\cdot C=(A\cdot B)\cdot C
$$
![1555144824018](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555144824018.png)

And the tree structure is better.

## Introduction

- Software Engineering Problem
  - Turn hardware/software quirks to get what programmers want/need
  - Optimize for convenience, utilization, security, reliability, etc…
- For any OS area (e.g. file systems, virtual memory, networking, scheduling)
  - What is the hardware interface? (physical reality)
  - What is the application interface? (nicer abstraction)
- Problem: Run multiple applications in such a way that they are protected from one another.
- Goal
  - Keep User Programs from Crashing OS
  - Keep User Programs from Crashing each other
  - Keep Parts of OS from crashing other parts
- Mechanisms
  - Address Translation
  - Dual Mode Operation
- Simple Policy
  - Programs are not allowed to read/write memory of other Programs or of Operating System.
- Address Space
  - A group of memory address usable by something.
  - Each program (process) and kernel has potentially different address spaces.
- Address Translation
  - Translate from Virtual Address (emitted by CPU) into Physical Address (of memory).
  - Mapping often performed in Hardware by Memory Management Unit (MMU).

![1555145669595](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555145669595.png)

![1555145711912](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555145711912.png)

A vulnerability exists when the map can be written by the program.

### Dual Mode Operation

Nowadays, hardware provides at least 2 modes:

- Kernel mode (or “supervisor” or “protected”)
- User mode: Normal programs executed

Some instructions are prohibited in user mode, such as modifying translation maps, or exceptions may be generated. In order to get into kernel mode, the process has to call a system call and run a special process “trap”, setting mode bit to 0, and the system call will be executed.

## Concurrency

- “Thread” of execution
  - Independent Fetch/Decode/Execute loop
  - Operating in some Address space
- Uniprogramming: one thread at a time
  - MS/DOS, early Macintosh, Batch processing
  - Easier for operating system builder
  - Get rid of concurrency by defining it away
  - Does this make sense for personal computers?
- Multiprogramming: more than one thread at a time
  - Multicis, Unix/Linux, OS/2, Windows NT/2000/XP, Mac OS X
  - Often called “multitasking”, but multitasking has other meanings
- The basic problem of concurrency involves resources:
  - Hardware: single CPU, single DRAM, single I/O devices
  - Multiprogramming API: users think they have exclusive access to shared resources
- OS Has to coordinate all activity
  - Multiple users, I/O interrupts,…
  - How can it keep all these things straight?
- Basic Idea: Use Virtual Machine abstraction
  - Decompose hard problems into simpler ones
  - Abstract the notion of an executing program
  - Then, worry about multiplexing these abstract machines
- Dijkstra did this for the “THE system”
  - Few thousand lines vs 1 million lines in OS 360 (1K bugs)

What happens during execution?

- Execution sequence
  - Fetch Instruction at PC
  - Decode
  - Execute (possibly using registers)
  - Write results to registers/memory
  - PC = Next Instruction(PC)
  - Repeat

How can we give the illusion of multiple processors?

- Assume a single processor is all we have. How do we provide illusion of multiple processors?
  - Multiplex in time!
- Each virtual “CPU” needs a structure to hold:
  - Program Counter (PC), Stack Pointer (SP)
  - Registers (Integer, Floating point, others…?)
- How to switch from one CPU to the next?
  - Save PC, SP, and registers in current state block
  - Load PC, SP, and registers from new state block
- What triggers switch?
  - Timer, voluntary yield, I/O, other things

![1555213760529](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555213760529.png)

Properties of this simple multiprogramming technique

- All virtual CPUs share same non-CPU resources
  - I/O
  - Memory
- Consequence of sharing
  - Each thread can access the data of every other thread (good for sharing, bad for protection)
  - Threads can share instructions (good for sharing, bad for protection)
  - Can threads overwrite OS functions?
- This (unprotected) model common in:
  - Embedded applications
  - Windows 3.1/Machintosh (switch only with yield)
  - Windows 95-ME (switch with both yield and timer)

Modern Technique: SMT/Hyperthreading

- Hardware technique
  - Exploit natural properties of superscalar processors multiple processors
  - Higher utilization of processor resources

![1555214244614](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555214244614.png)

Each slot represents a cycle and each block represents a unit. a colored block says the unit is currently busy. As is shown is the picture, a hyper threading processor can somehow merge the 2 threads together and make use of the blank units, thus improves time efficiency. We can therefore load 2 different threads into the processor at the same time and the processor runs a interweaving process.

- Can schedule each thread as if were separated CPU
  - However, not linear speedup
  - If have multiprocessor, should schedule each processor first
- Original technique called “Simultaneous Multithreading”
  - Alpha, SPARC, Pentium 4, Power 5

Program’s Address Space

![1555216152804](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555216152804.png)

- What happens when you read or write to an address?
  - Perhaps nothing
  - Perhaps acts like regular memory
  - Perhaps ignores writes (read only)
  - Perhaps causes I/O operation
  - Perhaps causes exceptions

Traditional UNIX Process

- Process: Operating system’s abstraction to represent what is needed to run a single program
  - Often called a “Heavy Weight Process”
  - Formally: a single, sequential stream of execution in its own address space
- Two parts:
  - Sequential Program Execution Stream
    - Code executed as a single, sequential stream of execution
    - Includes State of CPU registers
  - Protected Resources
    - Main Memory State (contents of Address Space)
    - I/O state (i.e. file descriptors)
- Important: There is no concurrency in a heavyweight process

How do we multiplex processes?

- The current state of process held in a process control block (PCB)
  - This is a “snapshot” of the execution and protection environment
  - Only one PCB active at a time
- Give out CPU time to different processes (Scheduling):
  - Only one process “running” at a time
  - Give more time to important processes
- Give pieces of resources to different processes (Protection):
  - Controlled access to non-CPU resources
  - Sample mechanisms:
    - Memory Mapping: Give each process their own address space
    - Kernel/User duality: Arbitrary multiplexing of I/O through system calls

CPU Switching from Process to Process

![1555220842447](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555220842447.png)

- This is also called a “context switch”
- Code executed in kernel above is overhead
  - Overhead sets minimum practical switching time
  - Less overhead with SMT/hyperthreading, but contention for resources instead

Diagram of Process State

![1555221100893](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555221100893.png)

- As a process executes, it changes state
  - new: The process is being created
  - ready: The process is waiting to run
  - running: Instructions are being executed
  - waiting: Process waiting for some event to occur
  - terminated: The process has finished execution

Process Schduling

![1555221370221](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555221370221.png)

- PCBs move from queue to queue as they change state
  - Decisions about which order to remove from queues are Scheduling decisions
  - Many algorithms possible (few weeks from now)

What does it take to create a process?

- Must construct new PCB
  - Inexpensive
- Must set up new page tables for address space
  - More expensive
- Copy data from parent process? (Unix fork())
  - Semantics of Unix fork() are that the child process gets a complete copy of the parent memory and I/O state
  - Originally very expensive
  - Much less expensive with “copy on write”
- Copy I/O state (file handles, etc)
  - Medium expense

Process =? Program

- More to a process than just a program:
  - Program is just part of the process state
  - I run emacs on lectures.txt, you run it on homework.java. Same program, different processes
- Less to a process than a program:
  - A program can invoke more than one process
  - cc starts up cpp, cc1, cc2, as, and ld

Multiple Process Collaborate on a Task

- High Creation/memory Overhead
- (Relatively) High Context-Switch Overhead
- Need Communication mechanism
  - Separate Address Spaces Isolates Processes
  - Shared-Memory Mapping
    - Accomplished by mapping address to common DRAM
    - Read and Write through memory
  - Message Passing
    - send() and receive() messages
    - Works across network

Shared Memory Communication

![1555222950559](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555222950559.png)

- Communication occurs by “simply” reading/writing to shared address page
  - Really low overhead communication
  - Introduces complex synchronization problems

Modern “Lightweight” Process with Threads

- Thread: a sequential execution stream within process (Sometimes called a “Lightweight process”)
  - Process still contains a single Address Space
  - No protection between threads
- Multithreading: a single program made up of a number of different concurrent activities
  - Sometimes called multitasking, as in Ada…
- Why separate the concept of a thread from that of a process?
  - Discuss the “thread” part of a process (concurrency)
  - Separate from the “address space” (Protection)
  - Heavyweight Process is the same thing as Process with one thread

Single and Multithread Process

![1555224505094](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555224505094.png)

- Threads encapsulate concurrency: “Active” component
- Address spaces encapsulate protection: “Passive” part
  - Keeps buggy program from trashing the system
- Why have multiple threads per address space?
  - They would crash each other
- Embedded systems
  - Elevators, Planes, Medical systems, Wistwathced
  - Single Program, concurrent operations
- Most modern OS kernels
  - Internally concurrent because have to deal with concurrent requests by multiple users
  - But no protection needed within kernel
- Network Servers
  - Concurrent requests from network
  - Again, single program, multiple concurrent operations
  - File server, Web server, and airline reservation systems

Thread State

- State shared by all threads in process/address space
  - Contents of memory (global variables, heap)
  - I/O state (file system, network connections, etc)
- State “private” to each thread
  - Kept in TCB (Thread Control Block)
  - CPU registers (including, program counter)
  - Execution stack

## Thread

Classification

![1555226171789](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555226171789.png)

- Real operating systems have either
  - One or many address spaces
  - One or many threads per address space

Thread State

- State shared by all threads in process/address space
  - Contents of memory (global variables, heap)
  - I/O state (file system, network connections, etc)
- State “private” to each thread
  - Kept in TCB (Thread Control Block)
  - CPU registers (including, program counter)
  - Execution stack
    - Parameters, Temporary variables
    - return PCs are kept while called procedures are executing

MIPS: Software conventions for Registers

![1555227338123](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555227338123.png)

- Before calling procedure
  - Save caller-saves registers
  - Save v0, v1
  - Save ra
- After return, assume
  - Callee-saves registers OK
  - gp, sp, fp OK (restored)
  - Other things trashed

Use of Threads

- A bad Example

  - ```
    main(){
        ComputePI("pi.txt");
        PrintClassList("clist.txt");
    }
    ```

  - The second function is never executed, because computing pi is a infinite process.

- Version of program with threads

  - ```
    main(){
        CreateThread(ComputePI("pi.txt"));
        CreateThread(PrintClassList("clist.txt"))
    }
    ```

- CreateThread()

  - Start independent thread running given procedure

- What is the behavior here?

  - Now, you should actually see the class list
  - This should behave as if there are 2 separate CPUs.

Memory Footprint of Two-Thread Example

- If we stopped this program and examined it with a debugger, we would see
  - Two sets of CPU registers
  - Two sets of Stacks

![1555228354027](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555228354027.png)

- How do we position stacks relative to each other?
- What maximum size should we choose for the stacks?
- What happens if threads violate this?
- How might you catch violations?

Per Thread State

- Each Thread has a Thread Control Block (TCB)
  - Execution State: CPU registers, program counter, pointer to stack
  - Scheduling info: State (more later), priority, CPU time
  - Accounting Info
  - Various Pointers (for implementing scheduling queues)
  - Pointer to enclosing process? (PCB)?
  - Etc (add stuff as you find a need)
- In Nachos: “Thread” is a class that includes TCB
- OS Keeps track of TCBs in protected meory

Lifecycle of Thread (or Process)

![1555229844803](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555229844803.png)

- As a thread executes, it changes state:
  - new: The thread is being created
  - ready: The thread is waiting to run
  - running: Instructions are being executed
  - waiting: Thread waiting for some event to occur
- “Active” threads are represented by their TCBs
  - TCBs organized into queues based on their state

Ready Queue And Various I/O Device Queues

- Thread not running. TCB is in some scheduler queue
  - Separate queue for each device/sigmal/condition
  - Each queue can have a different scheduler policy

![1555230244813](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555230244813.png)