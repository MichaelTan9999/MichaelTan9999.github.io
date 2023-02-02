---
title: Algorithms Notes (2) - Recurrence Relations
date: 2023-01-14 10:00:00 +0800
categories: 算法分析
math: true
---
## Master Theorem

### Basic assumptions

The master theorem would describe a recursive problem into a following procedures:

1. We have a procedure $P$ and an input $x$ of size $n$.
2. If $n <$ some constant $k$, solve $x$ directly without recursion.
3. Else, create $a$ subproblems of $x$, each having size of $n/b$. Call $P$ on each subproblems and mergee the resuts from the subproblems.

Thus, the runtime of an algorithm can be expressed as follows:

$$
T(n)=a\cdot T(\frac{n}{b})+f(n),
$$

where $f(n)$ is the time to create and combine their results in the above procedure. Crucially, $a$ and $b$ must not be depended on input size $n$. The theorem below also assumes that, as a base case for the recurrence, $T(n)=O(1)$ when $n$ is less than some bound $\kappa>0$. By using the Master Theorem, we do not have to expand the recursive relations many times but convert the recursive relation directly into a $\Theta$-notation.

### Generic form

The Master Theorem has three regimes, based on the efficiency of the recurrence reduction. That is, whether the split and combination is going to be much more than size of subproblems. In other word, if we compare the work on $f(n)$ and the work of total subproblems. To simplify, we name critical component as

$$
c_{crit}=\log_ba=\log(\text{number of subproblems})/\log(\text{relative subproblem size})
$$

| Case | Description                                                                | Condition                                                                                                                                                | Master Theorem Bound                                                                                                                                                                                                                         |
| :--- | :------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Work to split/recombine<br />a problem is dwarfed <br />by subproblems.   | $f(n)=O(n^c)$, where $c<c_{crit}$<br />upper-bounded by <br />a lesser exponent polynomial                                                           | $T(n)=\Theta(n^{c_{crit}})$<br />The splitting term <br />does not appear; <br />the recursive tree <br />structure dominates.                                                                                                            |
| 2    | Work to split/recombine<br />a problem is comparable <br />to subproblems. | $f(n)=\Theta(n^{c_{crit}}\log^kn)$, where $k\ge0$<br />rangebound by <br />the critical-exponent polynomial, <br />times zero or more optional logs | $T(n)=\Theta(n^{c_{crit}}\log^{k+1}n)$<br />The bound is the <br />splitting term, where <br />the log is augmented <br />by a single power.                                                                                               |
| 3    | Work to split/recombine<br /> a problem dominates <br />subproblems.       | $f(n)=\Omega(n^c)$, where $c>c_{crit}$<br />lower-bounded by <br />a greater-exponent polynomial                                                     | Doesn't yield anything.<br />Further more, if<br />$af(\frac{n}{b})\le kf(n)$,<br />for some constant $k<1$<br />and sufficiently large $n$,<br />then the total is dominated<br /> by the splitting term.<br />$T(n)=\Theta(f(n))$ |

However, not all recursive problems can be solved via the Master Theorem, such as $T(n)=T(n-1)+O(n/2)$.

## Substitution Method

The main idea of the Substitution Method is quite simple - just using the mathematical induction. It has two steps:

1. Guess the answer
2. Prove it by induction

The key point here is that we'd better have a **good** guess. We can expand the expression a few more times to see if there is a potential pattern. And another fashion way is to draw a graph with the help of computers - whether the line is linear, polynomial or exponential.

Here I will record an example on the sildes.

### Example one

> $T(n)=T(n/5)+7T(n/10)+n$, where $T(n)=1$ for all $n\in[1,10]$.

First we use computer to draw a rough graph, and find that the line looks like a straght line. So we guess $T(n)=O(n)$.

However, in our induction hypothesis, we need to write it as

> There is some $n_0>0$ and some $C>0$ such that for all $n>n_0$, $T(n)\le C\cdot n$.

Here we will use a placeholder $C$ in the following proof. It is similar to the placeholder in indefinite integral.

> 1. Inductive hypothesis: $T(n)<Cn$;
> 2. Base cases: IH holds when $n\in[1,10]$ if $C\ge1$.
> 3. Inductive steps:
>    1. Let $k>10$, assume that IH holds for $n\in[1,k]$.
>    2. Expand the expression.

1. Inductive hypothesis: $T(n)\le 10n$
2. Base case: the inductive hypothesis holds for $1\le n\le 10$. $T(n)=1\le 10n$.
3. Inductive step:
   1. Let $k\le 10$. Assume that the IH holds for all $n$ such that $1\le n\le k$.
   2. $$
      \begin{align*}
      T(k)
      &= k + T(k/5) + T(7k/10) \\
      &\le k + 10\cdot(k/5) + 10\cdot(7k/10) \\
      &= k + 2k + 7k \\
      &= 10k
      \end{align*}
      $$
   3. Thus, the inductive hypothesis holds for $n=k$.
4. Conclusion: with $C=10$ and $n_0=1$, $T(n)\le Cn$ for all $n\ge n_0$. By the big-O definition, $T(n)=O(n)$.

### Example two

> $T(n)=4T(n/2)+100n$, with $T(1)=\Theta(1)$.

Guess $T(n)=O(n^3)$.

The base case $T(1)=\Theta(1)\le O(1)$.

Assume that $T(n)\le c\cdot n^3$ for $k<n$. Prove $T(n)\le c\cdot n^3$ by induction.

$$
\begin{align*}
T(n)
&= 4T(n/2) + 100n \\
&= 4c(n/2)^3 + 100n \\
&= (c/2)n^3 + 100n \\
&= cn^3 - ((c/2)n^3 - 100n) \\
&\le c\cdot n^3
\end{align*}
$$

If $(c/2)\cdot n^3 - 100n \ge 0$, the inequation always holds. For example, $c=2, n=10$.

Although we prove that $T(n)=O(n^3)$, it is not tight enough. What if we have a guess $T(n)=O(n^2)$?

Assume that $T(k)\le c\cdot k^2$.

$$
\begin{align*}
T(n)
&= 4T(n/2) + 100n \\
&\le cn^2 + 100n \\
&\le cn^2
\end{align*}
$$

However, there is no choice of $c$ where $c>0$ make the above inequation true.

But we can add lower order terms to strengthen the inductive hypothesis.

Inductive hypothesis $T(k)=c_1 \cdot k^2 - c_2 \cdot k$ for $k<n$.

$$
\begin{align*}
T(n)
&= 4T(n/2) + 100n \\
&\le 4(c_1(n/2)^2-c_2(n/2)) + 100n \\
&= c_1n^2 - 2c_2n + 100n \\
&= c_1n^2 -c_2n - (c_2n - 100n) \\
&\le c_1n^2 -c_2n
\end{align*}
$$

Here we have $c_2 > 100$. The initial condition is that $T(1)=c_1-c_2$. We can have $c_1=200, c_2=150$.
