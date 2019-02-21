# Introduction to Algorithm

## Algorithmic Thinking and Peak Finding

### Course Overview

* Efficient procedures for solving large scale problems
* Scalability
* Static Data Structures

### Toy: Peak Finding

* The problem is about finding peaks if exists in an array with a certain length. Peak is defined as:

  ```text
    For array a[], position i is a peak if and only if $a[i-1] \le a[i]$ and $a[i+1]\le a[i]$. For the cases of edges, only compare the single side.
  ```

  By looking at the previous and latter position, peaks can be detected. 



If we only want to tell the existence of a peak, or find only one peak, we might apply the divide and conquer method. First decide whether the middle position satisfies to be a peak, then apply the function upon the left half and the right half.

Time complexity:

$$
T(n) = T(\frac{n}{2}) + \theta (1) = \theta (\log_2n)
$$

#### 2D version

Under the 2-D case, we might have empty buck. Namely, some position does not contain a number. Peak under 2-Dimension is defined as following: For position \(x,y\) to be a peak, x,y must satisfy

$$
\forall pi \in \{f(x-1,y),f(x,y-1),f(x+1,y),f(x,y+1), f(x,y)\}, pi \le f(x,y)
$$

The approach is known as the Greedy Ascent algorithm.

* Chose a starting point arbitrarily. Check if it satisfies the terms. 
* If not, move to the position having a larger number than the original number, and so forth. 

The worst case goes through all positions. In most cases, we prefer the starting position to be the center. However, such algorithm fails when at a certain column there is no peak.

#### A Better 2D Version

* Pick the middle column $j = \frac{m}{2}$. Find global max on column $j$ at $\(i,j\)$.
* Pick left columns if $a\(i,j-1\)&gt;a\(i,j\)$. If $a\(i,j\)\le a\(i,j-1\),a\(i,j+1\)$, we have a peak.
* When having a single column, find the max and done.

Time complexity:

$$
T(n,m) = T(n,\frac{m}{2}) + \theta (n) = \theta (n\log_2 m)
$$

#### Argue that peaks have to exist.

Suppose that a certain array, 1D or 2D, does not have a peak, which means there is no $$i,j$$ for $$\forall pi \in {f(i-1,j),f(i,j-1),f(i+1,j),f(i,j+1), f(i,j)}, pi \le f(i,j)$$ .

## Models of Computation, Document Distance

### What is an algorithm?

* A computation procedure for solving a problem.

|  | Program | Algorithm |
| :---: | :--- | :--- |
| Language | Programming language | Structured English |
| Work Place | Computer | model of computation |

### Module of Computation

* What operations an algorithm is allowed?
* Cost\(time, space, ...\) of each operation.
* Random Access Machine \(RAM\)
* Has random access memory. Modeled by a big array, every parts of which can be access in constant time.
* in constant time, a program can
  * load constant amount of words;
  * do constant amount of operations;
  * store constant amount of words;
  * ...
* Has constant amount of registers.
* Pointer Machine
* dynamically allocated objects;
* object has constant fields;
* field = word \(int, float, ...\) or pointer
* Python Model
* "list" = array, Python list is not an actual list but an array/vector.
* Object with constant amount of attributes.

#### Document Distance

* Document: sequence of words.
* Word: string of alphanumeric characters A-Z, 0-9.
* Idea: sharing words
  * Think of a document as a vector $D\[w\] =$ \#occurrences of $w$ in D, where $w$ stands for words in the document. Each word has frequency of itself. Then define distance of the documents to be: $$d(D_1,D_2) = \frac{D_1 \cdot D_2}{|D_1||D_2|}$$ , as an angle between the documents.
* Algorithm:
  * Split documents into words;
  * Complete word frequencies;
  * Dot product.

## Insertion Sort, Merge Sort

### Binary Search and Insertion Sort

* To find a given object in a certain position.
* Check middle position, if not, continue.
* Check left and right recursively.
*  $$T(n)=\log_2n$$ 

  For each position, move the object to the "right" place by swapping terms.

* Steps
  * Begin at 0th position;
  * Look at the position next to the right bound of the sorted part. Move the object on the position to its right place in the sorted part. Hence, the sorted part gets larger by a single position.
  * Continues to the end.
* Worst case: change a descending array into a ascending array, or backward.

  $$
  T(n) = \theta (n^2);
  $$

* Be cautious that the different ways of performing a swap operation.
  * Swap references;
  * Swap copies;
  * Swap pointers;
  * ...
* Given that compare cost is much larger than swap cost, the worst case scenario is as mentioned above. To actually make the given assumption happen, instead of swapping the object through the sorted array, do a binary search on the sorted array and find the position quickly. After finding the position, in a array structure where memory is stored continuously, the insert operation is not constant time, going back to the case of $$\theta (n^2)$$ . There is nothing more that we can do to improve this algorithm.

### Merge Sort

* Separate the array into objects and combine them altogether.

  $$
  Time = Divide + Recursion + Merge\\
  T(n) = C_1 + 2T(\frac{n}{2}) + C_2n = \theta(n\log_2n)
  $$

* Merge Sort algorithm needs $\theta \(n\)$ auxiliary space, which makes it not a in-place sorting algorithm. It is possible that merge sort could happen in-place but not practical.

  \*\*\*\*

### **Heap**

* Implements a set S of elements, each of which are associated with a key.
  * insert
  * max/min
  * pop\_max/pop\_min
  * increase key to a new value
* Use numeric relations between nodes instead of actual relation in space.
  * $$parent(i)=\frac{i}{2}$$ ;
  * $$left(i)=2i$$ ;
  * $$right(i)=2i+1$$ .
* Max Heap property: The key of a node is greater than or equal to the keys of its children. Same thing goes for min heap.

#### **Operations**

* build\_max\_heap: produce a max heap from an unordered array. The way of achieving such an operation is through a method called Max/min-heapify. Namely, Correct a single violation of a heap property.
  * Assume that the tree rooted at $left\(i\)$ and $right\(i\)$ are max heaps;
  * In order to correct the single mistake, swap the roots with their largest child along the tree. 

    $$
    T(n) = Number\text{ } of\text{ } Levels = \theta(\log_2n)
    $$

  * In every insert operation throughout the build process, put the node to the bottom and heapify to correct the mistake.
  * Time complexity:
    * Observe max\_heapify takes constant time for nodes that are above the leaves and in general $O\(l\)$ time for nodes that are $l$ levels above the leaves.
    * $\frac{n}{4}$ nodes with level 1, $\frac{n}{8}$ with level 2, ..., 1 node in level $\log\_2n$.

#### **Problem: Airport with a Single Runway**

* Total work:

  $$
  (\lim_{n\rightarrow +\infty})T(n) = \theta(1)\sum_{i=1}^{\log_2n} i\frac{n}{2^{i+1}} = \theta(n\log_2n)
  $$

### **Binary Search Tree \(BST\)**

* Reservations for future landings;
* Reserve request for landings, landing time t specified;
* Add t to the set T if no other landing are scheduled with k mins;
* Remove from set R after plane lands;
* All of these operations in $\theta\(\log\_2n\)$ time.

#### **Abstraction**

The problem can be stated as following:

* Build a data structure containing sorted numbers;
* The operation of inserting a new number into the structure and holding the numbers to be still sorted is valid and take $\theta\(\log\_2n\)$ time.

#### **Invariant**

For all nodes x, if y is in the left subtree of x, $key\(y\)\le key\(x\)$. If y is in the right subtree of x, $key\(y\)\ge key\(x\)$.

Insert time complexity:

$$
T(n) = O(height)
$$

* find\_min: goto the left most leaf;
* next\_larger\(x\): binary search;

#### **Subtree Size**

Add another attribute to every node, which starts at 0 and add by 1 each time an insert operation happens.

**Sort and Tell the Rank of a Node**

## AVL Trees, AVL Sort

### Heights

Note: $$Height = Longest\text{ } Path\text{ } to\text{ } Bottom$$ If:

$$
Height = \log_2n
$$

then, the tree satisfies to be a Balanced Tree.

The height of a node is defined as length of longest path from it down to a leaf. For convenience, define $nullptr$ to have height $-1$.

$$
height(node) = max\{height(node.left),height(node.right)\} + 1
$$

### AVL

* Require heights of left and right children of every node to differ by at most $\pm 1$. Therefore AVL Trees are always balanced.
* The worst case is when right subtree has height 1 more than left for every node.

#### **Property to be Maintained**

Define: $N\_h$ = min nodes in an AVL tree of height $h$.

$$
N_{O(1)} = O(1),\qquad N_h = 1 + N_{h-1} + N_{h-\alpha}
$$

$N$ should be the sum of the height of its children. Given that the tree is AVL, the formula goes as above, which is almost Fibonacci.

$$
N_h > F_H = \frac{1}{\sqrt{5}}\psi^h, where\quad \psi = \frac{\sqrt{5} + 1}{2}\\
N_h > 1 + 2N_{h-\alpha} > 2N_{h-\alpha} = \theta(2^{\frac{h}{2}}),
h < 2\log_2N
$$

**How to Maintain when Inserting**

* Do simple BST insert;
* Fix AVL property.

### **Rotation**

The operation can only be applied to AVL trees and stated as follows. For a given AVL tree, take x as the up-most root and y to be x's right child. X's left subtree, y's left subtree, y's right subtree are relatively noted as A, B, and C. After the operation, the tree is going to be like as follows. Y is the up-most root. Y's left child is x, whose left and right subtrees are A and B. Y's right subtree is C. The operation mentioned here is called left rotation. Same thing goes for right rotation. By rotating the tree, we can adjust the positions of the nodes so that the tree remains balanced. 

## Counting Sort, Radix Sort, Lower Bounds for Sorting and Searching

### Comparison Model

* All input items are black boxes \(ADT\);
* Only operations allowed are comparisons;
* Time cost = times of comparisons.

#### Decision Tree

* Any comparison algorithm can be viewed as a tree of all possible comparisons and their outcomes, and resulting answer.

  ![Decision Tree](https://github.com/NirnaethA/Algorithm_Note/tree/4cc7f7edba300880d624e5cd45a9b35ff0e92447/pic/c2cec3fdfc0392456a6ac4258694a4c27d1e2538.png)

* An operation can be represented in a binary decision tree, the worst case time complexity is the height of the tree and the best case time complexity is $\theta\(\log\_2n\)$ which is the min possible height of the tree, because the tree is binary and must have more than or equal to n leaves.
* Similarly, a sorting algorithm's time complexity has to be $\Omega\(n\log\_2n\)$. Because decision tree is binary and the amount of leaves has to be larger or equal to the amount of all possible outcome, which is $n!$.

  $$
  height\ge \log_2n! = \sum_{i=1}^n \log_2i\ge \int_0^n \log_2xdx\Rightarrow height = \theta(n\log_2n)
  $$

* Proof of lower bound is always what we care about.

#### Counting Sort

* For a given array, count the frequency of each number in the array and store the result in another array whose indexes correspond to the numbers in the original array.
* ```cpp
    '''
    for (int i = 0; i < A.size(); ++i){
        Count[A[i]]++;
    }
    '''
  ```

  \*\*\*\*

* Imagine each integer as base b. The number of digits, d, is $\log\_bk$, as k is the digit's index required.
* Sort integers by least significant digit,
* ...
* Sort integers by most significant digit.

#### Radix Sort

* Imagine each integer as base b. The number of digits, d, is $\log\_bk$, as k is the digit's index required.
* Sort integers by least significant digit,
* ...
* Sort integers by most significant digit.

### Hash with Chaining

#### Dictionary ADT

* maintain set of items, each with a key
* insert\(item\): overwrite any existing key.
* delete\(item\)
* search\(key\): return item with given key or report does not exist.

  With a AVL tree, all of the operations take $O\(\log\_2n$\)$. Here, we want it to be constant time like an integer indexes based array.

#### Direct Access Table

* Store items in array indexed by keys.
* Badness:
  * Keys may not be integers.
  * Possible collision.
  * Gigantic memory hog.

#### Solution to non-Integer Indexes: Prehash

* Maps keys to unsigned integers.
* In theory, keys are finite and discrete\(string of bits\).
* In Python, hash\(x\) is the prehash of x.
* Ideally, $hash\(x\)=hash\(y\)$ only when $x=y$.

#### Reducing Space: Hashing

* Reduce the universe of all keys\(integers\) down to reasonalbe size for table. Namely, make the key space smaller to be a hash space. The space size may be close to the amount of the keys.
* There has to be 2 keys corresponding to the same output, which is called collision.

#### Solve Collision: Chaining

* Linked list of colliding elements in each slot of hash table.

#### Simple Uniform Hashing

* Each key is equally likely to be hashed to any slot of the table, independent of where other keys hashing.
* Analysis:
  * Expected length of chain for n keys, m slots is $\frac{n}{m}$. We name this to be $\alpha$, the load factor of the table. As long as, $m=\theta\(n\)$, $\frac{n}{m} = O\(1\)$.
  * Hence, the running time is the length of the chain. The worst case is we have to search through the whole list with time complexity $O\(\frac{n}{m} + 1\)$ or $O\(\alpha + 1\)$.

#### Hash Function

1.  Division Method: $h\(k\) = k \mod m$, where m is mentioned above.
2. Multiplication Method: $h\(k\) = \(\(ak\)\mod 2^w\) &gt;&gt; \(w - r\)$, where $k$ is w-bit. This method can be understood as a division method under binary case, or taking the certain binary bits of the input.
3. Universal Hashing: $h\(k\) = \(\(ak+b\)\mod p\)\mod m$

### Table Doubling, Karp-Rabin

#### Grow the Table

* In order to keep $\frac{n}{m}$ to be constant. 
  * Make table of size $m'$.
  * Build new hash function $f'$ - Rehash objects in the old table and insert the results into the new table. 
  * Time complexity $\theta\(n + m + m'\)$.
* The operation had better be doubling the size. Each double operation is $\theta\(n\)$.
* Operation takes "$T\(n\)$ amortized", if k operations take less than or equal to $kT\(n\)$ time. Namely, on average, each insert operation take constant time.
* Deletion 
* 1.  If $m=\frac{n}{2}$, shrink the table. Delete-insert operation at position $2^k$ time takes linear time. Too slow. 
  2. If $m = \frac{n}{2^k},k\ge 2$, shrink the table to half of the size.

#### String Matching

The string or other things can be huge. Hash them before comparing can make things faster. In the string case, we need all of the substrings of certain length. Computing the substrings' hash value takes linear time, and if so, we are not saving any time here. Therefore, we introduce an ADT to compute the hash value faster.

## Open Addressing, Cryptographic Hashing

## Integer Arithmetic, Karatsuba Multiplication

### High Precision Multiplication

* Have 2 n-digit numbers and the radix \(2,10,...\). $$0\le x,y\le r^n$$ 
* Divide and conquer. Have $$x_1$$ as high half, $$x_0$$ as low half.

$$
x=x_1r^{\frac{n}{2}}+x_0,y=y_1r^{\frac{n}{2}}+y_0,\quad 0\le x_0,x_1,y_0,y_1<r^{\frac{n}{2}}
$$

* Combine

$$
z_0=x_0y_0,z_1=x_0y_1+x_1y_0,z_2=x_1y_1
$$

$$
z=xy=z_0+z_1r^{\frac{n}{2}}+z_2r^n
$$

We need 4 multiplies for n/2-digit numbers. Total time complexity is $$\theta(n^2)$$ 

### Karatsuba Algorithm

Better $$z_1$$ :

$$
z_1=(x_0+x_1)(y_0+y_1)-z_0-z_2
$$

1. Only 3 multiplications.

$$
T(n)=3T(\frac{n}{2})+\theta(n)=\theta(n^{\log_23})=\theta(n^{1.5848625...})
$$

## Square Roots, Newton's Method

* Want millionth digit of $$\sqrt{2}$$ .

$$
x=\sqrt{a}\Rightarrow y=x^2-a
$$

* Using Newton's method, one time of division.

$$
x_0=1,x_{n+1}+1=\frac{x_n+\frac{a}{x_n}}{2}
$$

* Error analysis.

$$
x_n=\sqrt{a}(1+\epsilon_n)\Rightarrow x_{n+1}=\frac{\sqrt{a}}{2}((1+\epsilon_n)+\frac{1}{1+\epsilon_n})=\sqrt{a}(1+\frac{\epsilon_n^2}{2(1+\epsilon_n)})
$$

$$
\Rightarrow \epsilon_{n+1}=\frac{\epsilon_n^2}{2(1+\epsilon_n)}
$$

### Toom-Cook

Generize Karatsuba Algorithm and separate the number into 3 parts each time.

$$
T(n)=5T(\frac{n}{3})+\theta(n)=\theta(n^{\log_35})
$$

### High-precision Division

* Want a high-precision of $$\frac{a}{b}$$ , first compute high-precision of $$\frac{1}{b}$$.
* Compute $$\lfloor\frac{R}{b}\rfloor$$ , where R is a large enough number $$2^k$$ and makes the division easy. To do that, we set up a function and find its zero using Newton's method.

$$
f(x)=\frac{1}{x}-\frac{b}{R},f'(x)=-\frac{1}{x^2},x_{n+1}=x_n-\frac{f(x_n)}{f'(x_n)}=2x_n-\frac{b}{R}x_n^2
$$

## Breadth-First Search \(BFS\)

### Graph Features

A graph consists of a set of vertices and a set of edges. The edges can be represented as ordered or unordered pairs, corresponding to directed and undirected graph.

$$
G=(V,E),E=\{\{v,w\}...\}or\{(v,w)...\}
$$

#### Applications

* Web crawling
* Social networking
* Netword broadcast
* Garbage collection
* Modeling checking
* Checking mathematical conjection
* Solving puzzles and games

### Adjacency List

Each vertices has a linked list pointing towards the linked/neighbour/able to reach by a single move vertices. Memory complexity is $$\theta(|V|+|E|)$$ .

### BFS

* Goal: visit all nodes reachable from given $$s\in V$$ 
* Time complexity: $$O(|V|+|E|)$$ 
* Implementation: look at nodes reachable in 0 move, 1 move, 2moves, ...
* Be careful to avoid duplicates with a set.

## Dynamic Programming \(DP\)

### Basic Idea

1. Take a problem
2. Split it into subproblems
3. Reuse the solution

### Example: Fibonacci Numbers

#### Defination



$$
F_1=F_2=1,F_n=F_{n-1}+F_{n-2}
$$

#### Naive Recursive Algorithm

```cpp
unsigned int fib(unsigned int n){
    if (n == 1 || n == 2)
        return 1;
    return fib(n - 1) + fib(n - 2);
}
```

Exponential time complexity:

$$
T(n)=T(n-1)+T(n-2)+O(1)
$$

#### Memorized DP

Compute each Fibonacci term and store it for later computation.

#### Bottom Up DP

$$
DP=recursion+memorization
$$

$$
time=\#subproblems\times time/subproblem
$$

```cpp
unsigned int fib(unsigned int n){
    if (n == 1 || n == 2)
        return 1;
    unsigned int n1 = 1, n2 = 1;
    for (unsigned int i = 2; i <= n; ++i){
        n1 = n1 + n2;
        n2 = n2 + n1;
    }
    return n1;
}
```

This code takes constant memory and linear time. It does the same thing as Memory version and takes less memory. It topologically sort the subproblem dependency DAG.

### Example: Shortest Path

### Example: Text Justification

In some text-editing software or a pdf-printer, the characters that the user enters into the screen must be presented in a good-looking way. These softwares do not want the texts to be too loose in each lines. 

$$
badness(i,j)=\begin{cases}
(page\_width-total\_width)^N & N=\text{2 or 3 or others}\\
\infty & \text{if don't fit}
\end{cases}
$$

1. Subproblems:
   * Try all 

## Computational Complexity



