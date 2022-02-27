---
title: Time Series Notes (1) - Fundamental Concepts
date: 2022-02-04 11:11:00 +0800
categories: 时间序列分析
---

# What is time series?

## Defintion of times series

A time series is stochastic process in discrete time, that is a sequence of random variables $\{Z_t\}$, which are ordered by a time index $t$.

## Two characteristics

- Infinite
- Ordered by time

## Two times series models

For times series, the available information includes the time index $t$, and the information in the past.

### Deterministic model

$$Z_t=a+bt+\varepsilon_t$$

$$Z_t=a+b_1t+b_2t^2+\cdots+b_pt^p+\varepsilon_t$$

Both are polynomial regression in time $t$.

### Stochastic model

$$Z_t=\theta_0+\phi Z_{t-1}+\varepsilon_t$$

$$Z_t=a+b_1Z_{t-1}+b_2Z_{t-2}+\cdots+b_pZ_{t-p}+\varepsilon_t$$

They are also called autoregressive model.

# Descriptive statistics of time series

## The mean function

For a times series $\{Z_t,t\in\mathbb{Z}\}$, the mean function (or mean sequence) is defined by

$$\mu_t=\text{E}(Z_t),\space\space t\in\mathbb{Z}$$

That is to say, $\mu_t$ is just the expected value of the process at time $t$. In general, $\mu_t$ can be different at each time $t$.

## The auto-covariance function (ACVF)

 The auto-covariance function is defined as

$$\gamma(t,s)=\text{Cov}(Z_t,Z_s)=\text{Cov}{(Z_t,Z_s)=\text{E}[(Z_t-\mu_t)(Z_s-\mu_s)]=\text{E}(Z_tZ_s)-\mu_t\mu_s}$$

## The variance function

Especially, when $s=t$ in the above ACVF equation, we have the variance function of $\{Z_t\}$, which is defined as

$$\gamma(t,t)=\text{Cov}(Z_t,Z_t)=\text{Var}(Z_t)$$

## The auto-correlation function (ACF)

The auto-correlation function is given by

$$\rho(t,s)=\text{Corr}(Z_t,Z_s)=\frac{\text{Cov}(Z_t,Z_s)}{\sqrt{\text{Var}(Z_t)\text{Var}(Z_s)}}=\frac{\gamma(t,s)}{\sqrt{\gamma(t,t)\gamma(s,s)}}$$

## Some useful formula

### ACVF and ACF

$$\gamma(t,t)=\text{Var}(Z_t)\space\space\space\space\space\rho(t,t)=1$$

$$\gamma(t,s)=\gamma(s,t)\space\space\space\space\space\rho(t,s)=\rho(s,t)$$

$$\vert\gamma(t,s)\vert\le\sqrt{\gamma(t,t)\gamma(s,s)}\space\space\space\space\space\vert\rho(t,s)\vert\le1$$

The first two results are directly from the definitions, and the third one can be derived by Cauchy-Schwarz inequality.

### Variance function

$$\text{Cov}(aX,Y)=a\text{Cov}(X,Y)$$

$$\text{Cov}(aX+bY,Z)=a\text{Cov}(X,Z)+b\text{Cov}(Y,Z)$$



$$\text{Cov}[\sum_{i=1}^m c_iY_i,\sum_{j=1}^n d_jZ_j]=\sum_{i=1}^m\sum_{j=1}^n c_jd_j\text{Cov}(Y_i,Z_i)$$

# Stationarity

## Weak stationarity

A time series $\{Z_t\}$ is said to be **weakly** (second-order, or covariance) stationary if

1. the mean function $\mu_t=\text{E}(Z_t)$ is independent of time; and
2. $\gamma(t,t-k)=\gamma(0,-k)$ for all times $t$ and lags $k$.

## Strict stationary

A time series $\{Z_t\}$ is said to be strictly stationary if the joint distribution of $Z_{t1},Z_{t2},\cdots,Z_{tn}$ is the same as that of $Z_{t1−k},Z_{t2−k},\cdots,Z_{tn−k}$ for all choices of natural number $n$, all choices of time points $t_1,t_2,\cdots,t_n$ and all choices of time lag $k$.

When the first moment and second moment exist, weak stationarity and strong stationarity are exactly the same.

## White noise

White noise is a sequence of $i.i.d.$ random variables, denoted by $\{a_t\}$, with zero mean and finite variance $\sigma_a^2$, abbreviated as $WN(0,\sigma_a^2)$.

### Characteristics of white noise

$$\gamma(t,t-k)=\left\{\begin{matrix}\sigma_a^2,&k=0\\0,&k\ne0\end{matrix}\right.$$

$$\rho_k=\left\{\begin{matrix}1,&k=0\\0,&k\ne0\end{matrix}\right.$$







