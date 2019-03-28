# Design and Analysis of Algorithms

## Interval Scheduling

We have resources and requests. Each of the requests corresponds to an interval of time, $$s(i)$$ as start time and $$f(i)$$ as finish time, where $$s(i)<f(i)$$. The two requests are compatible means that they do not overlap, $$f(i)\le s(j)$$ or $$f(j)\le s(i)$$. The goal is to select a compatible subset of requests/intervals of maximum size from a given set of requests/intervals. The size, namely, is to total time of request that is fulfilled.

### Greedy Strategy

The strategy is to maximize the profit at each step without apparent look ahead.

1. Use a simple rule to select a request $$i​$$ .
2. Reject all requests incompatible with $$i$$ .
3. Repeat until all requests are processed.

Wrong answers:

* Find the subset with the largest number of requests.
* For each request, find the number of incompatible requests and select the one with the minimum number of incompatibles.

![](.gitbook/assets/image%20%281%29.png)

As can be seen from the picture, the 4 requests at the top is the answer while it cannot be found by the methods above.

The correct answer should be that to scan the $$f(i)​$$s associated with the list of requests that we have and pick the one that is minimum, which also signifies that find the request associated with the earliest finish time, and eliminate the ones that do not satisfy the condition, at each step.

### Proof of Greedy Strategy by Induction

#### Claim

Given a list of intervals L, greedy algorithm with earliest finish time produces k intervals, where k is the maximum.

#### Introduction on $k^\star$ 

Base case: $$k^\star=1$$ any interval would work.

Suppose claim holds for $$k^\star$$ and we are given a list of intervals whose optimal schedule has $$k^\star+1$$ intervals. The final optimal result can be signified as:

$$
S^\star[1,2,...,k^\star+1]=<s(j_1),f(j_1)>,...,<s(j_{k^\star+1}),f(j_{k^\star+1})>
$$

If a incomplete solution is:



$$
S[1,2,...,k]=<s(i_1),f(i_1)>,...,<s(i_{k}),f(i_{k})>
$$

there is:

$$
f(i_1)\le f(j_1)
$$

Replace the final result's first item to be the first item of the incomplete solution:

$$
S^{\star\star}=<s(i_1),f(i_1)>,<s(j_2),f(j_2)>,...,<s(j_{k^\star+1}),f(j_{k^\star+1})>
$$

Define $L'$ as the set of intervals with $s(i)\ge f(i_1)$. Since $S^{\star\star}$ is optimal for $L$, $S^{\star\star}[2,...,k^\star+1]$ is optimal for $L'$. Therefore, optimal schedule for $L'$ has $k^\star$ size. By the inductive hypothesis, run the greedy algorithm on $L'$ should produce a schedule of size $k^\star$.

## Divide and Conquer: Convex Hull, Median Finding

Given a problem of size n, divide it into $$a,a\ge 1$$ subproblems of size $$\frac{n}{b},b>1$$. Solve each subproblem recursively. Combine all of the results.

$$
T(n)=aT(\frac{n}{b})+T_{merge}
$$

### Convex Hull
For a given set of points, find the set of edges connecting the points that would form a convex shape. A shape with one point stretching out and forming a line is still a convex hull.

Given n points in a plane,
$$
S=\{(x_i,y_i)|_{i=1,2,...}\}
$$
Assume no 2 have the same x and y coordinate and no 3 in a line. A convex hull is the smallest convex polygon containing all points in S, and we are going to call that $CH(S)​$.

![1550826046400](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1550826046400.png)

To check whether a line between the 2 points is a segment, which is the out most edge of the convex polygon, we simply check whether there are points at both sides of the line. If there is no side of the line that has no point, it is not a segment.

The simplest idea is to try every line for every points. There are $O(n)$ points to test and $O(n)$ lines to test for each point. With test complexity $O(n)$, the total complexity is $O(n^3)$.

The divide and conquer idea is to find the convex hull separately and merge them into one.

1. Sort the points by x coordinate.
2. For input set S, divide into left-half A and right-half B by x coordinate.
3. Compute $CH(A)$ and $CH(B)$.
4. Combine.

The simplest idea of merging is to look at all pairs in turn for all points on both sides with complexity $\theta(n^2)$. A clever-looking idea may be to connect the points with the largest and smallest y coordinate in pair, but incorrect. 

![1550896139397](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1550896139397.png)

If connect $a_4​$ to $b_1​$, the problem is not solved. Sometimes, it is not the line between the highest points that is the highest line. The factor showing the height of the line should be told by a intersection between the connecting line and a vertical line between the 2 hulls.

![1550898175718](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1550898175718.png)

The complexity is linear to the number of points, $\theta(n)$. The total complexity of the whole algorithm:
$$
T(n)=2T(\frac{n}{2})+\theta(n)=\theta(n\log_2n)
$$

### Median Finding

We want to do it better than sort-and-find approach. And not only to find the median value, but also to find any value for a given rank.

0. For a given array of numbers $S$, make a clever choice of $x\in S$.
1. Compute $k=rank(x)​$.
2. Split the array into 2 parts. $B=\{y\in S|y<x\},C=\{y\in S|y>x\}​$
3. If $k=desired\_rank​$, return. Else if $k>i​$, run $B​$, else run $C​$.

For a worst case scenario, we choose x badly every time, which makes B and C extremely unbalanced. The complexity would be $\theta(n^2)​$. The idea of choosing x cleverly is to split the array into subarrays with size $\left\lceil\frac{n}{5}\right\rceil​$. Sort each subarray with complexity $\theta(\left\lceil\frac{n}{5}\right\rceil)​$. Choose the median of the medians of the subarrays to be x.

![1550900613561](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1550900613561.png)

Then B and C can be evenly generated. Final time complexity:
$$
T(n)=\begin{cases}
O(1) & n\le some\_constant(140)\\
T(\left\lceil\frac{n}{5}\right\rceil)+T(\frac{7n+6}{10})+\theta(n)=\theta(n) & otherwise
\end{cases}
$$

## Divide and Conquer: Matrix Multiplication Strassen Algorithm

When calculated by hand, time complexity of matrix multiplication is $\theta(mn^2)$ for $A_{n\times m}\times B_{m\times n}$. It can be improved by divide and conquer idea.

### Naive Idea

1. Evenly split the matrices into 4 parts. $A=\left[\begin{array}\\
   A_{11} & A_{12}\\
   A_{21} & A_{22}
   \end{array}\right]​$
2. Compute. $C_{11}=A_{11}B_{11}+A_{12}B_{21}$
3. Recursively compute.

Time complexity:
$$
T(n)=8T(\frac{n}{2})+O(n^2)
$$

### Strassen

Instead of computing the multiplication directly, compute some of these terms as parts.
$$
\begin{cases}
M_1=(A_{11}+A_{22})(B_{11}+B_{22})\\
M_2=(A_{21}+A_{22})B_{11}\\
M_3=A_{11}(B_{12}-B_{22})\\
M_4=A_{22}(B_{21}-B_{11})\\
M_5=(A_{11}+A_{12})B_{22}\\
M_6=(A_{21}-A_{11})(B_{11}+B_{12})\\
M_7=(A_{12}-A_{22})(B_{21}+B_{22})
\end{cases},\begin{cases}
C_{11}=M_1+M_4-M_5+M_7\\
C_{12}=M_3+M_5\\
C_{21}=M_2+M_4\\
C_{22}=M_1-M_2+M_3+M_6
\end{cases}
$$
A man must work on a certain area for 10 years to come up with this algorithm, oh my god! Each of $C_{xx}​$ only requires a single time of multiplication. Time complexity:
$$
T(n)=7T(\frac{n}{2})+O(n^2)
$$

## Master Theorem

$$
\begin{align*}  
T(n)&=aT(\frac{n}{b})+f(n)\\
&=\sum_{i=0}^ta^if(\frac{n}{b^i})\\
&=\begin{cases}
\theta(n^{\log_ba}) & f(n)=O(n^c),c<\log_ba\\
\theta(n^c\log_2^{k+1}n) & f(n)=\theta(n^c\log_2^kn),c=\log_ba\\
\theta(f(n)) & f(n)=\Omega(n^c),c>\log_ba
\end{cases}  \\  
\end{align*}
$$

### Case 1

$$
T(n)=\sum_{i=0}^ta^if(\frac{n}{b^i})
=\sum_{i=0}^ta^i(\frac{n}{b^i})^c
=\theta(a^t)=\theta(n^{\log_ba}),
t=\log_bn
$$

The other cases follows the same idea.

## Divide and Conquer: Fast Fourier Transform (FFT)

### Operations on Polynomials

#### Representation

##### Coefficient Vector

$$
A(x,n)=\sum_{i=0}^{n-1}a_ix_i=a_0+a_1x+a_2x^2+...+a_{n-1}x^{n-1}
$$

##### Roots

$$
A(x)=\prod_{i=0}^{n-1}(x-r_i)
$$

According to some basic theorem of algebra, every polynomial can be represented by a series of roots.

##### Samples

Also according to some basic theorem of algebra, every polynomial can be represented by a series of 2-d points.

#### Evaluation

Compute $A(x_0)​$.

The naive idea may be to separately compute each term and add them up with complexity $\theta(n^2)$. A slightly smarter idea is to time a $x_0$ each time when computing a new term with complexity $\theta(n)​$. A slightly smarter way than that is the Horner’s Rule:
$$
A(x)=a_0+x(a_1+x(a_2+...+x(a_{n-1})...)),\theta(n)
$$

#### Addition

Compute $A(x)+B(x)$. Simply adding corresponding coefficients.

#### Multiplication

Compute $A(x)\times B(x)$. When doing this by hand:
$$
c_k=\sum_{j=0}^ka_jb_{k-j},\theta(n^2)
$$

|                | Coefficient Vector | Roots    | samples  |
| -------------- | :----------------: | -------- | -------- |
| Evaluation     |      $ O(n)$       | $ O(n) $ | $O(n^2)$ |
| Addition       |       $O(n)$       | $\infty$ | $ O(n) $ |
| Multiplication |      $O(n^2)$      | $ O(n) $ | $ O(n) $ |

### Converting between Coefficient Vector and Samples

$$
V_n=\begin{pmatrix}
1 & x_0 & x_0^2 & ... & x_0^{n-1}\\
1 & x_1 & x_1^2 & ... & x_1^{n-1}\\
1 & x_2 & x_2^2 & ... & x_2^{n-1}\\
\vdots &\vdots &\vdots &\ddots &\vdots\\
1 & x_{n-1} & x_{n-1}^2 & ... & x_{n-1}^{n-1}\\
\end{pmatrix},A=\begin{pmatrix}
a_0\\
a_1\\
\vdots\\
a_{n-1}
\end{pmatrix}
$$

For a transform from coefficient to samples:
$$
\begin{pmatrix}
y_0\\
y_1\\
\vdots\\
y_{n-1}
\end{pmatrix}=V_n\times A
$$


#### Divide and Conquer Algorithm

Goal: To compute all $A(x)$ for $x\in X​$.

1. Divide the coefficients into even and odd parts.
   $$
   A_{even}(x)=\sum_{k=0}^{\frac{1}{2}n-1}a_{2k}x^k,
   A_{odd}(x)=\sum_{k=0}^{\frac{1}{2}n}a_{2k+1}x^k
   $$

2. Conquer. Recursively compute  $A_{even}(y)$ and $A_{odd}(y)$ for $y\in X^2$.

3. Combine.
   $$
   A(x)=A_{even}(x^2)+xA_{odd}(x^2)
   $$

Time complexity:
$$
T(n,|X|)=2T(\frac{1}{2}n,|X|)+O(n+|X|)=O(n^2)
$$
If, some how, $T(n,|X|)=2T(\frac{1}{2}n,\frac{1}{2}|X|)+O(n+|X|)​$, where $|X|​$ changes in the same way as $n​$, the result can be $O(n\log_2n)​$. To achieve such goal, we construct $X​$ by the property of negative numbers and imaginary numbers, for the square of negative numbers are positive numbers and the square of imaginary numbers are negative numbers, so that when taking the square of the set, the set squeezes and becomes small. For some special cases, we take X as following, taking even-sect points from a unit circle on a complex plane, also known as the nth root of unity:
$$
\begin{align*}
&|X|=1,X=\left\{1\right\}\\
&|X|=2,X=\left\{1,-1\right\}\\
&|X|=4,X=\left\{1,-1,i,-i\right\}\\
&|X|=8,X=\left\{1,-1,i,-i,\pm\frac{1}{\sqrt{2}}(1+i),\pm\frac{1}{\sqrt{2}}(1-i)\right\}\\
\end{align*}
$$
In angular form:
$$
(\cos\theta,\sin\theta)=\cos\theta+i\sin\theta=e^{i\theta},\theta\in\left\{0,\frac{1}{n}\tau,\frac{2}{n}\tau,...,\frac{n-1}{n}\tau,\right\},\tau=2\pi
$$
In general:
$$
x_k=e^{\frac{ik\tau}{n}}
$$

#### Discrete Fourier Transform

$$
y=V\times A,V_{jk}=x_j^k=e^{\frac{ijk\tau}{n}}
$$

The FFT is the divide and conquer version of DFT.

### Fast Polynomials Multiplication

Notations:
$$
A^\star=FFT(A),B^\star=FFT(B),C^\star_k=A^\star_kB^\star_k,\forall k
$$
For V, the inverse matrix:
$$
V^{-1}=\frac{\bar{V}}{n},\bar{V}_{jk}=\frac{1}{V_{jk}}=e^{-\frac{ijk\tau}{n}}
$$
Proof for that:
$$
V\bar{V}=P,P_{jk}=\sum_{m=0}^{n-1}e^{i\tau jm/n}e^{-i\tau mk/n}=\sum_{m=0}^{n-1}e^{i\tau(j-k)m/n}
$$
Obviously, when $j=k$, $P_{jk}=1$. For $j\ne k​$:
$$
\sum_{m=0}^{n-1}(e^{i\tau(j-k)/n)^m}=\frac{e^{i\tau(j-k)}-1}{e^{i\tau(j-k)/n}-1}=0
$$
QED.

## Data Structure: B-Tree

A B-Tree has nodes with n elements and n+1 children between each of the elements. The node which is the children between some other nodes means that the node contains elements holding comparative properties saying it is between the 2 sizes of the elements above. 

### Properties

$B$ is the number of elements on each node.

1. $B\le​$ #children $\le 2B​$
2. $B-1\le$ #keys $< 2B-1​$
3. All leaves are at same depth

### Insertion

When there is a potential overflow in a node, insert the element into the rightful place and split the node by the center and move the central element up to the parent node, with the split result to be 2 new nodes. If the parent also encounters an overflow after the split process, apply the split process upon the parent. If we reach the root and root has an overflow, create a new root according to the same process.

### Deletion

When deleting on a leaf node, just delete. Under other more complicated cases, we try to move the deletion down to a leaf node. Swap the aimed element with the right most element of its left child or the left most element of its right child until the aim is at the bottom and become a leaf.

## Divide and Conquer: van Emde Boas Trees

The goal is to maintain n elements among $\left\{0,1,...,u-1\right\}​$, a set of continuous integers,with operations of insertion and deletion with complexity $\theta(\log_2\log_2n)​$. When doing binary search on the levels of binary trees, we get:
$$
T(k)=T(\frac{1}{2}k)+O(1),k=\log_2u
$$
Actually plug in u:
$$
T'(u)=T'(\sqrt{u})+O(1)
$$

### Bit Vector

A bit vector is an array of size u, with 1 for presence and 0 fir absence. Given that the set is continuous, we can take advantage of that and maintain an array of bits, showing whether a certain integer is in the set. Based on such idea, we build up a simple tree whose upper level signifies the Or result of the children.![1550923500760](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1550923500760.png)

The top bits consist the summary vector. We hereby split the universe into clusters, in the picture here, separated by red lines. In this version, the total size is limited down to the square root of the origin, $\sqrt{u}$.

Insert complexity is constant. Successor operation is to:

1. Look in x’s cluster. If is not there, go on.

2. Look for the next 1 in summary vector.

3. Look for the first 1 in that cluster.

   Time complexity $O(\sqrt{u})$.

To translate between actual index and the cluster the index belongs, $x=i\sqrt{u}+j$, where $i$ is the index of cluster and j is the index within cluster. More specifically:
$$
i=high(x)=\left\lfloor\frac{x}{\sqrt{u}}\right\rfloor,j=low(x)=x\mod \sqrt{u}
$$
To be even more specific, high and low simply means the high half of the binary representation of x and low half means likewise.

![1550927105924](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1550927105924.png)

## Recurse

The data structure is represented by $V$, consisting with 2 parts.

```
V = size - u
V.cluster[i] = size - \sqrt{u}
V.summary = size - sqrt{u}
0\le i\le \sqrt{u}
```

#### Insert

```
Insert(V,x) = Insert(V.cluster[high(x)],low(x))+Insert(V.summary,high(x))
```

$$
T(u)=2T(\sqrt{u})+O(1)=O(\log_2u)
$$



#### Successor

```
Successor(V,x):
	i = high(x)
	j = Successor(V.cluster[i],low(x))
	if j == \infty:
		i = Successor(V.summary,i)
		j = Successor(V.cluster[i],-\infty)
	return index(i,j)
```

$$
T(u)=O((\log_2u)^{\log_23})
$$

These are both bad algorithm for they both call themselves for multiple times.

#### Do Less Recursion

The way to reduce recursion is to store the current minimum value. In the insert operation, if the inserting x is smaller than the current minimum, the minimum should be refreshed and 

## Amortization: Amortized Analysis

Amortized cost in hash table is always 0, though for some it might not be so. This reflects a certain kind of Amortization, called the Aggregate method. In general, the cost of each step is not necessarily small, while on average, it gives a amortized cost of a smaller result.

Amortized bounds are to assign amortized cost for each operation such that it “preserve the sum”. 
$$
\sum_{op}amortized\_cost\ge\sum_{op}actual\_cost
$$
Under this model, there may be some occasionally larger complexity, while it does not matter, for it is fine amortizedly. For example, in 2-3 Trees, when the tree needs to grow, the complexity grows, while the amortized cost is not so great.

### Example: Accounting Method

- Allow an operation to store credit in bank account.
- Allow an operation to pay for time using credit in bank balance.

## Randomization

The algorithm generate a random number or a vector based on a certain range and the process depends on the random variable. On the same input on different executions, there may be different number of steps or outputs. Sometimes the output is probably incorrect according to a probability, in which case, the result must be checked and decide if it should run again. 

### Matrix Product: Freivald’s Algorithm

Choose a random binary vector r[1,…,n], such that the probability of the choice of value of the binary vector element is equal, namely, $P(r[i]=1)=\frac{1}{2}$ independently.

```
for i = 1,...,n:
	if A*Br=Cr:
		output yes
	else:
		output no
```

### Quick Sort

For a n-element array A,

- Divide
  - Pick a pivot element x in A
  - Partition the array into subarrays
- Conqure
  - Recursively sort subarrays the less and the greater part
- Combine
  - Trivial

#### Banic

Pick the pivot x to be the first of the last element of the array, with equal probability. Do partition based on that given x on $O(n)​$ time. The worst case, as before, is to sort a reversely arranged array and one part of the partition result has no element, where the time complexity is:
$$
T(n)=T(0)+T(n-1)+\theta(n)=\theta(n^2)
$$
The randomized process ensures a general balance case for the partition.

#### Median Selection

The median finding process has $\theta( n)$ complexity, while the whole process becomes:
$$
T(n)=2T(\frac{n}{2})+\theta(n)
$$

#### Randomized and Paranoid Quick Sort

The pivot x is chosen at random from array A. Expected time complexity is $O(n\log_2n)$. Furthermore, choose the pivot to be the random element from A repeatedly, until the result of partition is balanced enough:
$$
|L|\le\frac{3}{4}|A|,|G|\le\frac{3}{4}|A|
$$
The partition is good with 50% probability.

### Skip List

For a doubly directed linked list, the complexity of searching is $O(n)$ and the sorting complexity is $O(n)$. If we have 2 sorted linked list, some elements of one of which is directly linked to the other on the top.

![1553775891967](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1553775891967.png)

Consider this as a transport map of streets. If it is previously known to me that I am going to distant places, I might make decisions to travel by express, namely, the linked list on the top, instead of taking buses, stopping at every station. To view this special linked list as a data structure of its own being, we analyze the complexity of searching operation.

#### Searching

- Walk right in the top linked list L1, until going right would go too far.
- Walk down to the bottom list L0.
- Walk right in L0 until the target is found.

There are some obvious properties:

1. Cost: $|L_1|+\frac{|L_0|}{|L_1|}$
2. The cost is minimized when equals: $|L_1|^2=|L_0|=n$.

Push the logic to extreme, the complexity may be simpler. If the number of linked lists is 3, the complexity  becomes $3n^\frac{1}{3}$, and so on, $kn^\frac{1}{k}$. When we take $k$ to be $\log_2n$, the complexity becomes $2\log_2n$, when the data structure gets to look like a tree. Thus, a skip list is formed. By the way, it is crucial that the linked lists are sorted.![1553777945493](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1553777945493.png)

#### Insertion

- Search for the proper position of the inserting element x.
- Always insert into the bottom list.
- At each insertion, flip the coin (take one half of probability) to decide whether duplicate the element to the next level. Stop if encounters negative in a flip of a coin. If continue to get positive all the way up to the top of the lists, create a new list if necessary.

The worst case is infinite complexity, in which case we keep flipping coins every time, which must be prevented by some mechanism.

#### Deletion

A deletion has to happen in all levels for the same element.

#### Warm up Lemma

The number of levels in a n-element skip list is $O(\log_2n)$ or $O(c\log_2n)$ with high probability. The probability is in a form of $1-\frac{1}{n^\alpha}$, where $\alpha$ is related to the $c$ mentioned above. Here we prove this lemma.

In order to find the probability of correctness, we first look at the probability of failure, which says the number levels of n-element skip list is not less or equal to $c\log_2n$, which is the probability that some elements get promoted in time complexity greater than $c\log_2n$ times. By using the union bound, we claim the probability is bounded by the probability of some certain element gets promoted times n is greater than $c\log_2n$ times.
$$
P\{\text{some elemets get promoted > }c\log_2n\}\le n\times P\{\text{element gets promoted }>c\log_2n\}
$$
The probability of some certain element gets promoted greater than $\log_2n$ times is simply $(\frac{1}{2})^{c\log_2n}$, hereby the proof is finished. The probability of … is bounded by $\frac{1}{n^{c-1}}$.

#### Theorem of Searching

Any search in an n-element skip list costs $O(\log_2n)$ w.h.p. (with high probability). Here we apply the technical procedure of analyzing search backwards.

- Backwards search starts or ends at a node in the bottom list.
- At each node visited,
  - If the node was not promoted higher (tails here), we go to (came from) the left side.
  - If the node was promoted higher (heads here), we go (came from) up side.
- Stop (start) when we reach the top (bottom) level.

As we may see from the process illustrated above, backwards search makes “up” moves and “left” moves each with probability $\frac{1}{2}$. The number of moves going “up” is less or equal to $c\log_2n$ with high probability. The total number of moves correspond to the number of moves until getting $c\log_2n$ “up” moves. In other words, the process of flipping coins does not stop until $c\log_2n$ heads are obtained, with high probability.

The theorem is hereby proved by Chevnoff’s Theorem. Let $X$ be the sum of $X_i$s, where:
$$
P\{X_i=1\}=p,P\{X_i=0\}=1-p,1\le i\le m
$$
Then, for all $r>0$:
$$
P\{X\ge E[X]+r\}\le e^{-\frac{2r^2}{m}}
$$

### Perfect Hash

#### Dictionary Problem

Dictionary is a kind of abstract data type. It maintains a set of items, each with a key subject to:

1. insert(item): assume the key is not already in the table.
2. delete(item)
3. search(item): find item with that key.

A easy and good enough way of implementation is hashing with chain, which has amortizingly constant time complexity.