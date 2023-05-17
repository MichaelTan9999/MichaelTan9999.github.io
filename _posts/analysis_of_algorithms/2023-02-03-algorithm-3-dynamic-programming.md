---
title: Algorithms Notes (3) - Dynamic Programming
date: 2023-02-05 09:30:00 +0800
categories: 算法分析
math: true
---
## Overview of dynamic programming

Dynamic programming is both a mathematical optimization method and a computer programming method. The method was developed by Richard Bellman in the 1950s and has found applications in numerous fields.

In both contexts it refers to simplifying a complicated problem by breaking it down into simpler sub-problems in a recursive manner. While some decision problems cannot be taken apart this way, decisions that span several points in time do often break apart recursively. Likewise, in computer science, if a problem can be solved optimally by breaking it into sub-problems and then recursively finding the optimal solutions to the sub-problems, then it is said to have optimal substructure.

If sub-problems can be nested recursively inside larger problems, so that dynamic programming methods are applicable, then there is a relation between the value of the larger problem and the values of the sub-problems. In the optimization literature this relationship is called the Bellman equation.

**There are two key attributes that a problem must have in order for dynamic programming to be applicable: optimal substructure and overlapping sub-problems. If a problem can be solved by combining optimal solutions to non-overlapping sub-problems, the strategy is called "divide and conquer" instead.** This is why merge sort and quick sort are not classified as dynamic programming problems.

If $r$ represents the cost of a solution composed of subproblems $x_1, x_2, \dots, x_n$, then $r$ can be written as

$$
r = g(f(x_1),f(x_2),\dots,f(x_n))
$$

where $g$ is the composition function, or the so-called state transition equation.

### Optimal substructure

Optimal substructure means that the solution to a given optimization problem can be obtained by the combination of optimal solutions to its sub-problems. Such optimal substructures are usually described by means of recursion.

### Overlapping sub-problems

*Overlapping* sub-problems means that the space of sub-problems must be small, that is, any recursive algorithm solving the problem should solve the same sub-problems over and over, rather than generating new sub-problems.

## Longest common subsequence problem

### LCS problem definition

A sequence $Z$ is a subsequence of $X$ if $Z$ can be obtained from $X$ by deleting symbols.

> BDFH is a subsequence of ABCDEFGH.
>
> ABCDEFGH is a subsequence of ABCDEFGH.

A sequence $Z$ is a longest common subsequence (LCS) of $X$ and $Y$ if $Z$ is a subsequence of both $X$ and $Y$, and any sequence longer than $Z$ is not a subsequence of at least one of $X$ or $Y$.

LCS problem task is to find the length of LCS $Z$ of given sequences $X$ and $Y$.

### LCS recursive solution

We define $X_i, Y_i$ to be the prefixes of $X$ and $Y$ of length $i$ and $j$ respectively, and $c[i,j]$ to be the length of LCS of $X_i$ and $Y_i$. If $X$ has $m$ characters and $Y$ has $n$ characters, then the length of LCS of $X$ and $Y$ will be $c[m,n]$.

Here we give the state transition equation directly.

$$
c[i,j]=
\begin{cases}
c[i-1,j-1] + 1 & \text{if } x_i=y_i\\
\max(c[i,j-1],c[i-1,j]) &\text{otherwise}
\end{cases}
$$

When we calculate $c[i,j]$, we consider two cases:

1. $x_i=y_j$: one more symbol in substring $X_i$ and $Y_j$ matches, so the length of LCS $X_i$ and $Y_j$ equals to the length of LCS of shorter substrings $X_{i-1}$ and $Y_{j-1}$ plus one.
2. $x_i \ne y_j$: As the current symbols don't match, our solution will not be updated, and LCS .  and $Y_j$ equals to the maximum in the. length of LCS of $X_i$ and $Y_{j-1}$ and the length of LCS of $X_{i-1}$ and $Y_j$.

> Find the length of LCS of string $ABCB$ and $BDCAB$.

Here we give the dynamic programming tables directly. It starts from the upper left part and ends in bottom right.

|   | j    | 0    | 1 | 2 | 3 | 4 | 5 |
| - | ---- | ---- | - | - | - | - | - |
| i |      | null | B | D | C | A | B |
| 0 | null | 0    | 0 | 0 | 0 | 0 | 0 |
| 1 | A    | 0    | 0 | 0 | 0 | 1 | 1 |
| 2 | B    | 0    | 1 | 1 | 1 | 1 | 2 |
| 3 | C    | 0    | 1 | 1 | 2 | 2 | 2 |
| 4 | B    | 0    | 1 | 1 | 2 | 2 | 3 |

This table directly yields the time complexity: $O(mn)$, where $m,n$ are the lengths of the two arrays.

So far we only find the length of the LCS but not the actual LCS itself. To find the actual LCS we go from the bottom right and trace to upper left. We mark the position where the number changes.

If we only cares the length, we can just have two rows in the memory to store. Since the algorithm goes iteratedly, the maximum is always in the most fresh row.

## Knapsack problem

### Unbounded knapsack problem

We have a knapsack with capacity $W$, $n$ items with infinite copies. And we want the knapsack to store as much as possible.

Here we define $B[W]$ as the optimal value that we can find for a capacity $W$. If we put an item $i$ in the knapsack, the best value we can achieve is the optimal value with a capacity of $W-w_i$ plus the value $b_i$ of item $i$. So we derive the state transition equation as below:

$$
B[W]=\begin{cases}0 & w_i>W \\ \max_i(B[W-w_i]+b_i) & \text{otherwise}\end{cases}
$$

Here the $\max_i$ means the find-maximum operation will go through all items for each knapsack size. If we want to memory the items set, we can simply use a two-dimension array to record the what items are in the knapsack for each different size.

### 0-1 knapsack problem

Here we still give the state transition equation directly. Since each item only has one copy, we need to update the formula before, inder to demonstrate rejecting the item or accepting the item under each knapsack size.

$$
B[k,W]=\begin{cases}B[k-1,w] & w_k>W \\ \max(B[k-1,w],B[k-1,w-w_k]+b_k) & \text{otherwise}\end{cases}
$$

The key point that why we need two parameters to demonstrate the status is because we cannot define the sub-problems only based on the size of the knapsack. We have to use a pair of parameters to denote the current knapsack value and the items in the knapsack. Remember in the unbounded knapsack problem, if we want to record what the items in the knapsack, we still need another table.
