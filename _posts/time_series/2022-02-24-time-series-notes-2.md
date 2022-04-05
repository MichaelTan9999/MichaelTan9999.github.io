---
title: Time Series Notes (2) - Stationary linear time series models
date: 2022-02-24 10:00:00 +0800
categories: 时间序列分析
math: true
---

# General linear processes

## Definition

### Linear time series

A time series $\{Z_t\}$ is linear if the value of $Z_t$ is a linear function of a white noise sequence.

### Causal time series

A time series $\{Z_t\}$ is causal if the value of $Z_t$ is affected only by the information up to now.

### General linear process

A linear, causal and stationary time series is also called a general linear process.

The general linear process has the form of


$$
Z_t=\sum_{j=0}^{+\infty}\psi_ja_{t-j}=\psi_0a_t+\psi_1a_{t-1}+\psi_2a_{t-2}+\cdots
$$


where $\{a_t\}\sim WN(0,\sigma_a^2)$ and $\sum_{j=0}^{\infty}\psi_j^2<\infty$.

## Some statistics

$$
\begin{align}
& \mu=\text{E}(Z_t)=0 \\
& \gamma_0=\text{Var}(Z_t)=\sigma_a^2\sum_{j=0}^{\infty}\psi_j^2 \\
& \gamma_k=\text{Cov}(Z_t,Z_{t-k}),\space k\ge0 \\
& \rho_k=\frac{\text{Cov}(Z_t,Z_{t-k})}{\text{Var}(Z_t)}=\frac{\sum_{j=0}^\infty\psi_j\psi_{j+k}}{\sum_{j=0}^\infty\psi_j^2},\space k>0
\end{align}
$$

# Moving average (MA) processes

## Definition

A moving average process of order $q$, and abbreviated as $\text{MA}(q)$, is defined as:


$$
Z_t=\theta_0+a_t+\theta_1a_{t-1}+\theta_2a_{t-2}+\cdots+\theta_qa_{t-q}
$$


where $q\ge0$ is an integer and $\{a_t\}\sim WN(0,\sigma_a^2)$.

The MA process is also called the MA model.

Note that MA process is a special general linear process. Hence, it is <font color=red>linear, causal and stationary</font>.

## Back-shift operator $B$

The operator $B$ is a symbol that denotes the back-shift of the time series:


$$
(B^kZ)_t:=B^{k-1}(BZ_t):=Z_{t-k}
$$


The MA process $Z_t=a_t+\theta_1a_{t-1}+\theta_2a_{t-2}+\cdots+\theta_qa_{t-q}$ can be rewritten as


$$
Z_t=(1+\theta_1B+\theta_2B^2+\cdots+\theta_pB^p)a_t=\theta(B)a_t
$$


where $\theta(x)=1+\theta_1x+\cdots+\theta_qx^q$ is the **MA characteristic polynomial**.

## Some statistics

### General form MA(q)

For a $\text{MA}(q)$ process where $Z_t=\theta_0+a_t+\theta_1a_{t-1}+\theta_2a_{t-2}+\cdots+\theta_qa_{t-q},\space\space\space\{a_t\}\sim WN(0,\sigma_a^2)$, we have


$$
\begin{align}
& \mu=\text{E}(Z_t)=\theta_0 \\
& \gamma_0=(1+\theta_1^2+\theta_2^2+\cdots+\theta_q^2)\sigma_a^2 \\
& \rho_k=\left\{\begin{matrix}\frac{\theta_k+\theta_1\theta_{k+1}+\theta_2\theta_{k+2}+\cdots+\theta_{q-k}\theta_q}{1+\theta_1^2+\theta_2^2+\cdots+\theta_q^2},&q=1,2,\cdots,q\\0,&k\ge q+1\end{matrix}\right.
\end{align}
$$

### Special form of MA(1)

For $MA(1)$: $Z_t=\theta_0+a_t+\theta_1a_{t-1}$, the statistics are


$$
\begin{align}
& \mu=\text{E}(Z_t)=\theta_0 \\
& \gamma_0=\text{Var}(Z_t)=\sigma_a^2(1+\theta_1^2) \\
& \gamma_1=\text{Cov}(Z_t,Z_{t-1})=\text{Cov}(a_t+\theta_1a_{t-1},a_{t-1}+\theta_1a_{t-2})=\sigma_a^2\theta_1 \\
& \rho_1=\frac{\gamma_1}{\gamma_0}=\frac{\sigma_a^2\theta_1}{\sigma_a^2(1+\theta_1^2)}=\frac{\theta_1}{1+\theta_1^2} \\
& \gamma_2=\text{Cov}(a_t+\theta_1a_{t-1},a_{t-2}+\theta_1a_{t-3})=0 \\
& \rho_2=0
\end{align}
$$

# Autoregressive (AR) process

## Definition

A $p$-th order autoregressive model (or, for short, an $\text{AR}(p)$ model) $\{Z_t\}$ satisfies the equation


$$
Z_t=\theta_0+\phi_1Z_{t-1}+\phi_2Z_{t-2}+\cdots+\phi_pZ_{t-p}+a_t
$$


where $p\ge0$ is an integer, $\{\phi_i\}$ are real numbers, and $\{a_t\}\sim WN(0,\sigma_a^2)$.

The model can be rewritten as $\phi(B)Z_t=\theta_0+a_t$, where $\phi(x)=1-\phi_1x-\phi_2x^2-\cdots-\phi_px^p$ is **AR characteristic polynomial**. If all roots of the corresponding $\phi(x)=0$ are outside the unit circle, the $\text{AR}(p)$ model is stationary, and the unique solution is called $\text{AR}(p)$ process. The condition above is called stationarity condition.

## Some statistics

### Yule-Walker equations (matrix form)

Consider a $\text{AR}(1)$ model, where $Z_t=\theta_0+\phi Z_{t-1}+a_t$. Suppose it is a stationary process, we can substitue the $Z_t$ terms with its stationary condition that $Z_t=\mu+\sum_{j=0}^\infty\psi_ja_{t-j}$. By doing so, we have that


$$
\mu+\sum_{j=0}^\infty\psi_ja_{t-j}=\theta_0+\phi\mu+\phi\sum_{j=0}^\infty\psi_ja_{t-1-j}+a_t
$$


multiply the equation by $Z_{t-k}$, take expectations and divide by $\gamma_0$, we have the recursive relationship


$$
\rho_k=\phi_1\rho_{k-1}+\phi_2\rho_{k-2}+\cdots+\phi_p\rho_{k-p},\space k\ge 1
$$


By choosing different $k=1,2,3,\cdots,p$, we have equations below


$$
\begin{pmatrix}

1 & \rho_1 & \cdots & \rho_{p-1} \\

\rho_1 & 1 & \cdots & \rho_{p-2} \\

\vdots & \vdots & \ddots & \vdots \\

\rho_{p-1} & \rho_{p-2} & \cdots & 1

\end{pmatrix} \begin{pmatrix} \phi_1 \\ \phi_2 \\ \vdots \\ \phi_p \end{pmatrix} 

= \begin{pmatrix} \rho_1 \\ \rho_2 \\ \vdots \\ \rho_p \end{pmatrix}
$$


Given the values $\phi_1,\phi_2,\cdots,\phi_p$, the Yule-Walker equations can be solved for $\rho_1,\rho_2,\cdots,\rho_p$.

### General form AR(p)

$$
\begin{align}
& \mu=\frac{1}{1-\phi_1-\phi_2-\cdots-\phi_p} \\
& \rho_k=\phi_1\rho_{k-1}+\phi_2\rho_{k-2}+\cdots+\phi_p\rho_{k-p},\space k\ge1 \\
& \gamma_0=\frac{\sigma_a^2}{1-\phi_1-\phi_2-\cdots-\phi_p} \\
& \gamma_k=\gamma_0\times\rho_k
\end{align}
$$

### Special form of AR(1)

For $\text{AR}(1)$: $Z_t=\theta_0+\phi Z_{t-1}+a_t$


$$
\begin{align}
& \mu=\frac{\theta_0}{1-\phi} \\
& \psi_0=1,\space\space\psi_k=\phi^k \\
& Z_t=\frac{\theta_0}{1-\phi}+\sum_{j=0}^\infty\phi^ja_{t-j}
\end{align}
$$

# The mixed autoregressive-moving average model

## Definition

In general, if a time series has the form below,


$$
Z_t=\theta_0+\phi_1Z_{t-1}+\phi_2Z_{t-2}+\cdots+\phi_pZ_{t-p}+a_t+\theta_1a_{t-1}+\theta_2a_{t-2}+\cdots+\theta_qa_{t-q}
$$


we say that $\{Z_t\}$ is a mixed autoregressive-moving average model with abbreviation $\text{ARMA}(p,q)$.

The model will reduce to an AR model if $q=0$, and to an MA process if $p=0$. Then the AR and MA models are special cases of ARMA models.

For convenience, we may rewrite the above equation as


$$
\phi(B)Z_t=\theta_0+\theta(B)a_t
$$


where $\phi(x)$ and $\theta(x)$ are the AR and MA characteristic polynomials, respectively. That is,


$$
\begin{align}
& \phi(x)=1-\phi_1x-\phi_2x^2-\cdots-\phi_px^p \\
& \theta(x)=1+\theta_1x+\cdots+\theta_qx^q
\end{align}
$$

There exists a unique stationary solution to the $\text{ARMA}(p,q)$ model if all roots of the AR characteristic equation $\phi(x)=0$ are outside the unit circle.

This unique stationary solution is called the **$\text{ARMA}(p,q)$ process**, and has the form of


$$
Z_t=\mu+\sum_{j=0}^\infty\psi_ja_{t-j}
$$

## Statistics

$$
\begin{align}
& \mu=\frac{\theta_0}{1-\phi_1-\phi_2-\cdots-\phi_p} \\
& \gamma_k=\phi_1\gamma_{k-1}+\phi_2\gamma_{k-2}+\cdots+\phi_p\gamma_{k-p} \\
& \rho_k=\phi_1\rho_{k-1}+\phi_2\rho_{k-2}+\cdots+\phi_p\rho_{k-p} \\
& \rho_k=\Phi\rho_q,\space k>q+1
\end{align}
$$

# Invertbility

## Definition

A time series $\{Z_t\}$ is invertible if


$$
a_t=\pi_0Z_t+\pi_1Z_{t-1}+\pi_2Z_{t-2}+\cdots
$$


This property ensures that we can recover the information sequence based on the past values of the time series. So, any AR process is always invertible. Without loss of generality, we can set $\pi_0=1$.

A general MA (or ARMA) process is invertible if all roots of its MA characteristic polynomial are outside the unit circle.

## AR and MR representation

If a time series $\{Z_t\}$ is invertible, then its autoregressive (AR) representation is:


$$
Z_t=\pi_1Z_{t-1}+\pi_2Z_{t-2}+\cdots+a_t
$$


It looks like a AR model, but with infinite order.

Correspondingly, the unique stationary solution to an AR (or ARMA) model, expressed as a general linear process of the form, is also called the MA representation as below:


$$
Z_t=\sum_{j=0}^\infty\psi_ja_{t-j}
$$
