# Design and Analysis of Algorithms

## Interval Scheduling

We have resources and requests. Each of the requests corresponds to an interval of time, $$s(i)$$ as start time and $$f(i)$$ as finish time, where $$s(i)<f(i)$$. The two requests are compatible means that they do not overlap, $$f(i)\le s(j)$$ or $$f(j)\le s(i)$$. The goal is to select a compatible subset of requests/intervals of maximum size from a given set of requests/intervals. The size, namely, is to total time of request that is fulfilled.

### Greedy Strategy

The strategy is to maximize the profit at each step without apparent look ahead.

1. Use a simple rule to select a request $$i$$ .
2. Refect all requests incompatible with $$i$$ .
3. Repeat until all requests are processed.

Wrong answers:

* Find the subset with the largest number of requests.
* For each request, find the number of incompatible requests and select the one with the minimum number of incompatibles.

![](.gitbook/assets/image%20%281%29.png)

As can be seen from the picture, the 4 requests at the top is the answer while it cannot be found by the methods above.

The correct answer should be that to scan the $$f(i)$$s associated with the list of requests that we have and pick the one that is minimum, which also signifies that find the request associated with the earliest finish time, and eliminate the ones that do not satisfy the condition, at each step.

### Proof of Greedy Strategy by Induction

#### Claim

Given a list of intervals L, greedy algorithm with earliest finish time produces k intervals, where k is the maximum.

#### Introduction on $$k^\star$$ 

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

Define $$L'$$ as the set of intervals with $$s(i)\ge f(i_1)$$. Since $$S^{\star\star}$$ is optimal for $$L$$, $$S^{\star\star}[2,...,k^\star+1]$$ is optimal for $$L'$$. Therefore, optimal schedule for $$L'$$ has $$k^\star$$ size. By the inductive hypothesis, run the greedy algorithm on $$L'$$ should produce a schedule of size $$k^\star$$.

## Divide and Conquer: Convex Hull, Median Finding

Given a problem of size n, divide it into $$a,a\ge 1$$ subproblems of size $$\frac{n}{b},b>1$$. Solve each subproblem recursively. Combine all of the results.

$$
T(n)=aT(\frac{n}{b})+T_{merge}
$$

### Convex Hull

