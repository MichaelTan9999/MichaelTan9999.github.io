---
title: Algorithms Notes (1) - Asymptotic Notations
date: 2023-01-04 15:00:00 +0800
categories: 算法分析
math: true
---
## Basic assumption

Before we dive into the special notations, we first have some critical assumptions for algorithm complexity analysis. The following operations are defined as **basic** operations:

1. Arithmetic operations: `a * b + c`
2. Assigning a value to a variable: `x = 3`
3. Indexing into an array \ accessing value of objects or structs: `arr[i]`, `arr.length`, `student.name`
4. Call a method: `x = factorial(n)`
5. Returning from a method: `return y`
6. Comparison: `x==y`, `x>=y`

And we have a reasonable assumption that **each basic operation takes constant time**.

## Big-oh and small-oh notation

### Big-oh definition

We say $g(n)=O(f(n))$ if and only if there exists positive constant $c$ and $n_0$ such that $g(n)\le c\cdot f(n)$ for all $n>n_0$.

This definition implies two points:

* $n > n_0$ means that it cares a large size of the input data - larger than some given number $n_0$
* $g(n)\le c\cdot f(n)$ means that $g(n)$ grows no faster than a constant $c$ time of the simplified function $f(n)$

The constant coefficients and the base of logarithm functions do not affect the $O$ notation.

### The arithmetic rules

* Polynomial

If $g(n)$ is a non-negative polynomial of degree $d$, i.e., $g(n)=a_d\cdot n^d+a_{d-1}\cdot n^{d-1}+\cdots+a_1\cdot n+a_0$, then $g(n)=O(n^d)$.

The biggest eats all.

An example: $g(n)=1+100000n^{0.3}+n^{0.5}=O(n^{0.5})$

* Product

If $g_1(n)=O(f_1(n)),g_2(n)=O(f_2(n))$, then $g_1(n)\cdot g_2(n)=O(f_1(n)\cdot f_2(n))$.

The big multiplies the big.

An example: $g(n)=(n^3+n^2+1)(n^7+n^{12})=O(n^3\cdot n^{12})=O(n^{15})$

* Sum

If $g_1(n)=O(f_1(n)),g_2(n)=O(f_2(n))$, then $g_1(n)+g_2(n)=O(\max(f_1(n),f_2(n)))$.

The bigger of the two big.

A wrong example: $f(n)=\sum_{i=1}^n1$, set $g_1=g_2=\cdots=g_n=1$, then $O(f(n))=O(\max(1,1,\dots,1))=1$.

However, $O(f(n))=n$

> The sum property only applies when the number of functions are constants.
{: .prompt-danger }

* Logarithmic

Log functions grow slower than power functions.

If $g(n)=(\log n)^a+n^b$, where $b>0, a>0$, then $g(n)=O(n^b)$

An example: $g(n)=(\log n)^2+n^{0.0001}=O(n^{0.0001})$

> Log only beats constant.
{: .prompt-tip }

* Exponential

Exponential functions grow faster than power functions.

If $g(n)=a^n+n^b$, then $g(n)=O(a^n)$.

An example: $g(n)=2^n+3^n=O(3^n)$

> The asymptotic analysis of exponential functions involves the base.
{: .prompt-tip }

### Small-oh defintion

We say $g(n)=o(f(n))$ if for **any** constant $c>0$, there exists a constant $n_0>0$ such that $0<g(n)<c\cdot f(n)$ for all $n>n_0$.

## Big-omega and small omega

### Big-omega definition

We say $g(n)=\Omega(f(n))$ if and only if there exists a positive constant $c>0$ and $n_0>0$ such that $g(n)\ge c\cdot f(n)$ for all $n>n_0$.

The definition implies two points:

* $c\cdot f(n)$ grows slower than $g(n)$
* $g(n)$ will surpass $c\cdot f(n)$ at some point, which is denoted as $n_0$

### Small-omega definition

We say $g(n)=\omega(f(n))$ if for **any** constant $c>0$, there exists a constant $n_0$ such that $g(n)>c\cdot f(n)\ge 0$ for all $n\ge n_0$.

In a word, big-oh and small-oh controls the upper bound while big-omega and small-omega controls the lower bound in asymptotic analysis. And small characters have more strict conditon since they require the coefficients are any positive numbers.

## Big-Theta

If $g(n)=O(f(n))$ and $g(n)=\Omega(f(n))$, then $g(n)=\Theta(n)$.

Some examples:

* $g(n)=3n+4=\Theta(n)$
* $g(n)=n\cdot\log n+2n^2=\Theta(n^2)$
* $g(n)=(3n^2+2\sqrt n)\cdot(n\log n+n)=\Theta(n^3\cdot\log n)$

If two algorithms have the same big-theta function, they can be considered as equally good.
