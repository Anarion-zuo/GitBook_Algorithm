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

When a and b is ready, the “plus” and “multiply” fires, then “minus” and “plus” fires, then “multiply” fires.