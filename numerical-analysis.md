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
- Backward substitution. For each row $i=m,m-1,...,1$,
  - For each $j<i$, zero out rest of the column.

$$
T(n)=\theta(m^2n)
$$

#### Numerical Error

When the first number of the first row is 0, we cannot apply Gaussian elimination right away, but have to find a non-zero number in the first column. A worse case is, the first number of the first row is a very small number, due to the double numerical error from the way of representation of floating points.

To avoid the errors of such kinds, we may let the row containing tiny numbers go down to the bottom of the matrix, thus avoid dividing them. Or set some rules treating the small numbers as 0.

#### Reasonable Use Case

Suppose we have a set of $\vec{b}$ corresponding to a set of linear systems with identical $A$ waiting to be solved. Instead of apply Gaussian elimination every time, we memorize the step of Gaussian elimination of $A$ and apply it separately to the $\vec{b}$s.

It is obvious that the time complexity is much less for triangular $A$. The following procedure tries to construct triangular matrices.

The process of Gaussian elimination can be represented as:
$$
M_k...M_1A\vec{x}=M_k...M_1\vec{b}
$$
Define:
$$
U=M_k...M_1A,A=(M_1^{-1}...M_k^{-1})U=LU
$$
where $U$ is a upper triangular matrix. Therefore:
$$
A\vec{x}=\vec{b}\Rightarrow LU\vec{x}=\vec{b}
$$
Solve $L\vec{y}=\vec{b}$ using forward substitution and $U\vec{x}=\vec{y}$ using backward substitution, for $L$ is triangular and help save time. The total complexity becomes $O(mn)$.

### Factorization

The main idea of Gaussian elimination mentioned above is to separate $A$ into a product of 2 triangular matrices.
$$
A=LU
$$
However, for every $A$ there may not be such factorization. 

As is done before, to avoid tiny numbers caused by double numerical errors, we have to do pivoting by swapping columns, which is to swap the tiny numbers to the right most column. The process of swapping rows and columns becomes:
$$
A=LUP
$$

For all $A$, we can find $L,U,P$ for such kind of factorization.

### Norm

$$
||x||_p=(\sum_{i=1}^n|x_i|^p)^{-\frac{1}{p}},||x||_\infty=\max\{|x_1|,...,|x_n|\}
$$

It is very import to decide what is “small” when using notations like $d,\delta,\Delta$.

![1555048561057](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1555048561057.png)

One way of understanding norm is to draw a unit circle of the norm, $||x||_p=1$, where $p$ is defined to be the norm size.

- $p=1$, the unit circle is a rhombus.
- $p=\infty$, the unit circle is a square.
- In other cases, it is something in between, expending like a star.

When solving $A\vec x=\vec b$, if take the equation as a line in the surface, the solution tries to minimize the distance between the line and the origin. Hence, it is equivalence to take all different kinds of norms as distance.

For matrices, we may “unroll” a matrix so as to form a “long vector” consisting of all columns of the matrix.
$$
A\in\R_{m\times n}\leftrightarrow a(:)\in\R,||A||_{Fro}=\sqrt{\sum_{ij}a^2_{ij}}
$$
We may also use the induced norm, which is finding the greatest norm of multiplication between the matrix and a unit vector with corresponding dimensions and norm size. It is the furthest distance that the matrix covers.
$$
||A||=\sup\{||A\vec x||,||\vec x||=1\}=\sup_{\vec x\ne\vec 0}\{\frac{||A\vec x||}{||\vec x||}\}
$$
Some examples for induced norms:
$$
||A||_1=\max_i\sum|a_{ij}||,||A||_\infty=\max_i\sum_j|a_{ij}|
$$

### Error

In the process of solving a linear system, as we draw nearer to the solution, the distance between the current state and the target is defined to be $\epsilon$.
$$
(A+\epsilon\cdot\delta A)\vec x(\epsilon)=\vec b+\epsilon\cdot\delta\vec b
$$
Simplifications:
$$
\frac{d\vec x}{d\epsilon}\Big |_{\epsilon=0}=A^{-1}(\delta\vec b-\delta A\cdot\vec x(0)),\frac{||\vec x(\epsilon)-\vec x(0)||}{||\vec x(0)||}\le||A^{-1}||\cdot||A||(\frac{||\delta\vec b||}{||\vec b||}+\frac{||\delta\vec A||}{||\vec A||})+o(\epsilon^2)
$$
The condition number of $A$, $1/f'(x^*)$ is the norm of $A$ times norm of $A$ inverse:
$$
\bold{cond}A=||A||\cdot||A^{-1}||=(\sup_{\vec x\ne\vec 0}\{\frac{||A\vec x||}{||\vec x||}\})(\inf_{\vec y\ne\vec 0}\{\frac{||A\vec y||}{||\vec y||}\})^{-1}
$$
There is a potential approximation given that $||A^{-1}\vec x||\le||A^{-1}||||\vec x||$:
$$
\bold{cond}A\ge\frac{||A||||A^{-1}\vec x||}{||\vec x||}
$$

### Parametric Regression

Find $a_1,...,a_n$, for:
$$
f(\vec x)=a_1x_1+a_2x_2+...+a_nx_n
$$
The process is by doing $n$ experiments to make an approximation to the coefficients.
$$
y^{(1)}=f(\vec x^{(1)})\\\vdots\\y^{(n)}=f(\vec x^{(n)})
$$
Matrix form of representation:
$$
\begin{pmatrix}\vec x^{(1)T}\\\vdots\\\vec x^{(n)T}\end{pmatrix}\begin{pmatrix}a_1\\\vdots\\a_n\end{pmatrix}=\begin{pmatrix}y^{(1)}\\\vdots\\y^{(n)}\end{pmatrix}
$$
In general, we can deal with functions instead of simple numbers.
$$
f(\vec x)=a_1f_1(x_1)+a_2f_2(x_2)+...+a_nf_n(x_n)
$$
where $f_i$ can be nonlinear. For example, $f_i(x)=x^i$ forms a famous case that is the polynomial approximation or “Vandermonde system”.
$$
f(x)=a_0+a_1x+a_2x^2+...+a_nx^n
$$
The sample matrix maybe constructed by simple production.

#### Solution

$$
A=\begin{pmatrix}\vec x^{(1)T}\\\vdots\\\vec x^{(n)T}\end{pmatrix},\vec x=\vec a,\vec b=\begin{pmatrix}y^{(1)}\\\vdots\\y^{(n)}\end{pmatrix}
$$

The process of finding solution is to let $A\vec x\approx b$, which is:
$$
A\vec x\approx b\Leftrightarrow\min_\vec{x}||A\vec x-\vec b||_2\Leftrightarrow \min_{\vec x}||A\vec x-\vec b||_2^2
$$

$$
||A\vec x-\vec b||_2^2=(A\vec x-\vec b)^T(A\vec x-\vec b)=||A\vec{x}||^2-2(A^T\vec{b})\cdot\vec{x}+||\vec{b}||^2\Rightarrow A^TA\vec x^*=A^T\vec b\Rightarrow\vec x^*=(A^TA)^{-1}A^T\vec b
$$

Suppose, $f(\vec x)=||A\vec{x}||^2-2(A^T\vec{b})\cdot\vec{x}+||\vec{b}||^2$. By taking the gradient, we can find the minimum to the function, as is done above.
$$
\nabla f=-2A^T\vec b+2A^TA\vec x=\vec0
$$


#### Regularization

When $A^TA$ is not invertible, we add a regularization term to the target function.

Tikhonov regularization, Ridge regression, Gaussian priror:
$$
\min_{vec x}||A\vec x-\vec b||+\alpha||\vec x||_2^2
$$
Lasso, Laplace prior:
$$
\min_{\vec x}||A\vec x-\vec b||_2^2+\alpha||\vec x||_1
$$
Elastic net:
$$
\min_{\vec x}||A\vec x-\vec b||_2^2+\alpha||\vec x||_2^2+\beta||\vec x||_1
$$

#### $A^TA$ Property

Symmetric:
$$
(A^TA)^T=A^TA
$$
Positive Semi-definite:
$$
\vec x^TA^TA\vec x\ge0
$$

### Pivoting for SPD $C$

Solve $C\vec x=\vec d$ for symmetric positive definite $C$.
$$
C=\begin{pmatrix}c_{11}&\vec v^T\\\vec v&\tilde C\end{pmatrix}
$$

#### Cholesky Factorization

$$
E=\begin{pmatrix}1/c_{11}&\vec 0^T\\\vec r&I_{(n-1)\times(n-1)}\end{pmatrix}
$$

$$
EC=\begin{pmatrix}\sqrt{c_{11}}&\vec v^T/\sqrt{c_{11}}\\\vec 0&D\end{pmatrix}
$$

$$
ECE^T=\begin{pmatrix}1&\vec 0\\\vec 0&\tilde D\end{pmatrix}
$$

Therefore, for any $C$, $\exist E$
$$
C=(E_n...E_1)I(E_1^T...E_n^T)\equiv LL^T,L=\prod_{i=n}^1E_i
$$
where $L$ is certainly a lower triangular matrix, as $E$.

#### Observation

Suppose $L$ is represented in the form of:
$$
L=\begin{pmatrix}
L_{11}&0&0\\
\vec l_k^T&l_{kk}&0\\
L_{31}&L_{32}&L_{33}
\end{pmatrix}\Rightarrow LL^T=\begin{pmatrix}
\times&\times&\times\\
\vec l_k^TL_{11}^T&\vec l_k^T\vec l_k+l_{kk}^2&\times\\
\times&\times&\times
\end{pmatrix}
$$
Hence, we have the way of computing Cholesky factorization.
$$
C_{kk}=\vec l_k^T\vec l_k+l_{kk}^2,\vec v=\vec l_k^TL_{11}^T
$$
Substitute $L$ into the definition of positive definite matrix:
$$
\vec x^TC\vec x=\vec x^TL^TL\vec x=||L\vec x||_2^2\ge0
$$

### Column Space and QR

## Nonlinear Problems

### Root

Given: $f:\R^n\rightarrow\R^m$, Find: $\vec x^*$ with $f(\vec x^*)=\vec0$.

#### Typical Regularizing Assumptions

1. Continuous: $f(\vec x)\rightarrow f(\vec y)$ as $\vec x\rightarrow\vec y$.
2. Lipschitz: $||f(\vec x)-f(\vec y)||\le C||\vec x-\vec y||$.
3. Differentiable.
4. $C^k$: $k$ derivatives exist and are continuous.

#### Single-Variable $\R^1\rightarrow\R^1$

##### Bisection Algorithm

Intermediate Value Theorem:

> Suppose $f:[a,b]\rightarrow\R$ is continuous. Suppose $f(x)<u<f(y)$. Then, there exists $z$ between $x$ and $y$ such that $f(z)=u$.

Thus, if:
$$
f(l)f(r)<0
$$
then:
$$
\exists x_0\in[l,r],f(x_0)=0
$$
Processes:

1. Compute $c=(l+r)/2$.
2. If $f(c)=0$, return $x^*=c$.
3. If $f(l)f(c)<0$, take $r=c$. Otherwise take $l=c$.
4. Return to step 1 until $|r-l|<\epsilon$; then return $c$.

The algorithm must converge unconditionally.

To examine the convergence of the algorithm, we introduce a factor $E_k>|x_k-x^*|$ as the convergence criterion, such that when $E_k$ is bounded, the algorithm is convergent. In the case of bisection, there is:
$$
E_k=|r_k-l_k|,E_{k+1}\le\frac{1}{2}E_k
$$

##### Fixed Points

Find $x^*$ for:
$$
x^*=g(x^*)
$$
Any equation can be transformed into the fixed points form:
$$
f(x)=0\Rightarrow f(x)+x=x
$$
The strategy is to find the convergent sequence such that:
$$
x_{k+1}=g(x_k)
$$
Compute along the sequence. If it converges, the solution can be found.

The convergence criterion is:
$$
E_k=|x_k-x^*|=|g(x_{k-1})-g(x^*)|\le C|x_{k-1}-x^*|=CE_{k-1}
$$
If $g$ is Lipschitz, the sequence is convergent.

It can also be shown that the convergence criterion is bounded even by quadratic degree, even when $g'(x^*)=0$. Thus, it is quadratically convergent.
$$
E_k=|g(x_{k-1})-g(x^*)|=\frac{1}{2}|g''(x^*)(x_{k-1}-x^*)^2+O((x_{k-1}-x^*)^3)|\le\frac{1}{2}|(g''(x^*)+\epsilon)(x_{k-1}-x^*)^2|
$$
When applying the algorithm, it is better to know a starting point near $x^*$.

##### Newton’s Method

Solve for Taylor’s first expansion:
$$
f(x)=f(x_k)+f'(x_k)(x-x_k)\Rightarrow x_{k+1}=x_k-\frac{f(x_k)}{f'(x_k)}
$$
The solution is the fixed point iteration on:
$$
g(x)=x-\frac{f(x)}{f'(x)}
$$
We have quadratic convergence in this case.
$$
g'(x)=1-\frac{f'(x)^2-f(x)f''(x)}{f'(x)^2}=\frac{f(x)f''(x)}{f'(x)^2},g'(x^*)=0
$$
Taking derivative is hard, therefore, we have the secant method.

##### Secant Method

$$
x_{k+1}=x_k-\frac{f(x_k)(x_k-x_{k-1})}{f(x_k)-f(x_{k-1})}
$$

The convergence rate is $\frac{1+\sqrt5}{2}$.

What we want is a hybrid method of secant/Newton with convergence guarantees of bisection. For example, Dekker’s Method:

> Take secant step if it is in the bracket, bisection step otherwise.

##### Conclusion

- Unlikely to solve exactly, so we settle for iterative methods.
- Must check that method converges at all.
- Convergence rates:
  - Linear: $E_{k+1}\le CE_k$ for some $0\le C<1$
  - Super-linear: $E_{k+1}\le CE_k^r,r>1$
  - Quadratic: $r=2$
  - Cubic: $r=3$
- Time per is iteration also important.

#### Multiple Variables $\R^n\rightarrow\R^m$

##### Usual Assumption

$$
n\ge m
$$

##### Newton’s Method

Find derivatives by Jacobian:
$$
(Df)_{ij}=\frac{\partial f_i}{\partial x_j}
$$
The size of Jacobian matrix is $m\times n$.

Solve Taylor’s expansion as before:
$$
f(\vec x)=f(\vec x_k)+Df(\vec x_k)(\vec x-\vec x_k)\Rightarrow\vec x_{k+1}=\vec x_k-[Df(\vec x_k)]^{-1}f(\vec x_k)
$$
We do not compute the inverse explicitly. Instead, we do the Gaussian elimination to solve for the result.

We can prove that the algorithm is convergent:

1. $\vec x_{k+1}=g(\vec x_k)$ converges when the maximum-magnitude eigenvalue of $Dg$ is less than 1.
2. Extend observation about (quadratic) convergence in multiple dimensions.

We still have 2 problems:

1. Differentiation is hard.
2. $Df(\vec x_k)$ changes at every iteration, which means differentiation is even made harder.

We cannot apply the extended secant method to make things better, for there is no enough data points for us to compute the Jacobian. However, we can make things better by computing only a part of the Jacobian, which is the directional derivative. A directional derivative is a vector.
$$
D_{\vec v}f=Df\times\vec v
$$
From another perspective, we can have something like a secant-like approximation:
$$
J(\vec x_k-\vec x_{k-1})=f(\vec x_k)-f(\vec x_{k-1}),J=Df(\vec x_k)
$$
This is the Broyden’s method. The outline of the steps are:

- Maintain current iterate $\vec x_k$ and approximation $J_k$ of Jacobian near $\vec x_k$.
- Update $\vec x_k$ using Newton-like step.
- Update $J_k$ using secant-like formula.

