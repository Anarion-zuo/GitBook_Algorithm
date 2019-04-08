# Numerical Analysis

Compute $A\vec{x}=\vec{b}$. Transform this problem into minimize $||A\vec{x}-\vec{b}||^2$.
$$
\begin{align*}
||A\vec{x}-\vec{b}||^2&=(A\vec{x}-\vec{b})^T(A\vec{x}-\vec{b})\\
&=(\vec{x}^TA^T-\vec{b}^T)(A\vec{x}-\vec{b})\\
&=||A\vec{x}||^2-2(A^T\vec{b})\cdot\vec{x}+||\vec{b}||^2
\end{align*}
$$

## Introduction

### Double Numerical Errors

```cpp
double x = 1.0;
double y = x / 3.0;
cout << x == y * 3.0;
// The result is not going to be equal
```

This kind of things are a bit annoying, for it comes from nowhere, and catches us by unawares. Therefore,
$$
Mathematically\text{ }correct \ne Numerically\text{ }sound
$$
When some comparison between floating numbers has to exist, we use some tolerance.

```cpp
return (x - y) <= n * numeric_limits<double>::epsilon;
```

Due to the way representing floating points in computers, the precision of long rational numbers cannot remain unchanged. Moreover, the multiplication might cause severe loss in precision. For example,
$$
0.1_2\times0.1_2=0.01_2
$$
When the number of precision is limited down to 2, the result becomes $0.0_2$.

Other kinds of losing precision operations include overflow. When taking the norm of a vector, the square operation may produce a large number and cause overflow. To avoid overflow, we firstly divide the vector with the largest component of the vector, then take the square.

### Finding Roots

For equations and computation of complicated expressions, we can always apply the minimization method.
$$
\sqrt{x}\Rightarrow\min_y||y^2-x||^2
$$

When we are trying to find the root of a function, a larger derivative around the root gives a better result.

As for error of a root finding or general optimization problem, we define the forward and backward error to be:
$$
e_{forward}=x-x^*,e_{backward}=f(x)-f(x^*)
$$
Therefore, their ratio is $\frac{1}{f'(x^*)}$. It proves the conclusion above.

## Linear System

Solving linear systems.
$$
A\vec{x}=\vec{b},A\in\R^{m\times n},\vec{x}\in\R^n,\vec{b}\in\R^m
$$

### Solvability with Shape

Solvability can depend on $\vec{b}$. It also depends on the shape of $A$. If $A$ is tall, $m>n$, there may be no solution, unless the matrix contains dependent row vectors. If $A$ is wide, $m<n$, there may be infinite solutions.

### Inverting Matrices

Solving the equation does not mean finding inverse matrices. When we solve linear systems by hand, we do the process of Gaussian Elimination, consisting of permutation and making more 0s in the matrix.

### Gaussian Elimination

1. Row scaling. Make the first number of the first row 1 by dividing the first row with that number.
2. Forward substitution. Make the first column 0 except the first row, with the 1 of the first row.
3. Move to the next column. Form a upper triangular form.

#### Formal Expression

- Forward substitution. For each row $i=1,2,...,m$,
  - Scale row to get pivot 1.
  - For each $j>i$, subtract multiple of row $i$ from row $j$ to zero out pivot column.
- Backward substitution. For each row $i=m,m-1,...,1​$,
  - For each $j<i​$, zero out rest of the column.

$$
T(n)=\theta(m^2n)
$$

#### Numerical Error

When the first number of the first row is 0, we cannot apply Gaussian elimination right away, but have to find a non-zero number in the first column. A worse case is, the first number of the first row is a very small number, due to the double numerical error from the way of representation of floating points.

To avoid the errors of such kinds, we may let the row containing tiny numbers go down to the bottom of the matrix, thus avoid dividing them.

### #### 

