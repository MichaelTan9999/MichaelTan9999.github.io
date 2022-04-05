---
title: Time Series Notes (3) - Non-stationary linear time series models
date: 2022-03-15 23:10:00 +0800
categories: 时间序列分析
math: true
---

# Stationarity through differencing

## Non-stationarity in mean

Any time series with a mean function that evolves over time is non-stationary. Consider a series of the form


$$
Z_t=\mu_t+X_t
$$


where $\mu_t$ is a nonconstant mean function and $X_t$ is a zero-mean stationary series.

## Differencing operation

### Definition of differencing

The differencing operation on an arbitrary time series $\{Z_t\}$ is defined as 


$$
\nabla Z_t=Z_t-Z_{t-1},\space t\in\mathbb{Z}
$$


Here $\nabla=(I-B)$ is called the difference operator,


$$
\nabla Z=(I-B)Z=Z-BZ,\space\space(\nabla Z_t)=Z_t-(BZ)_t=Z_t-Z_{t-1}
$$


and the new time series $\nabla Z=\{\nabla Z_t\}$ is called the first difference of $Z=\{Z_t\}$ or the differenced sequence.

The iteration of differencing: for any $k,\nabla^kZ=\nabla^{k-1}(\nabla Z)$. For example, 


$$
(\nabla^2Z)_t=\nabla(\nabla Z_t)=(\nabla Z)_t-(\nabla Z)_{t-1}=(Z_t-Z_{t-1})-(Z_{t-1}-Z_{t-2})=Z_t-2Z_{t-1}+Z_{t-2}
$$


### Properties of differencing

1. It is a linear operator on sequences / time series, namely for any two sequences $X=(X)_t$ and $Y=(Y)_t$, and numbers $a,b$, $\nabla(aX+bY)=a\nabla X+b\nabla Y$ which means for any $t$, $\nabla(aX+bY)_t=a\nabla X_t+b\nabla Y_t$.

2. For a linear sequence $X_t=a+bt,\nabla X_t\equiv b$.

   More generally, for a polynomial sequence $X_t=a_0+a_1t+\cdots+a_pt^p$ for degree $p\ge1$, $\nabla^pX_t=a_p$.

   In particular, $\nabla^{p+1}X\equiv0$.

3. If $X=(X_t)$ is a stationary time series, $\nabla X$ is also a stationary time series.

For the third assertion: let $Y=\nabla X$ and $Y_t=X_t-X_{t-1}$

The mean function of $Y$: $\mu_Y=\text{E}(X_t)-\text{E}(X_{t-1})=\mu_X-\mu_X=0$.

The covariance function of $Y$: $\text{Cov}(Y_t,Y_{t-k})=\text{Cov}(\nabla X,\nabla X_{t-k})=\text{Cov}(X_t-X_{t-1},X_{t-k}-X_{t-k-1})=\gamma_k-\gamma_{k-1}-\gamma_{k+1}+\gamma_k$, where $\gamma_k=\text{Cov}(X_t,X_{t-k})$ and $k=0,1,2,\dots$ So this covariance of $Y$ depends only on the lag $k$.

# ARIMA models

## Motivation

After taking differencing operations, the resulting sequence might well be considered as arising from a stationary time series model. Note that the ARMA model is the most general model among what we have learned. Combin- ing these two facts, we reach the general class of integrated autoregressive-moving average (ARIMA) models.

## Definition of ARIMA model

A time series $\{Z_t\}$ is said to follow an integrated autoregressive-moving average (ARIMA) process if the $d$-th order difference


$$
W_t=\nabla^dZ_t
$$


is a stationary ARMA process. If $W_t$ is $\text{ARMA}(p,q)$, we say that $Z_t$ is $\text{ARIMA}(p,d,q)$.

Note that when $d=0$, the $\text{ARIMA}(p,d,q)$ process reduces to $\text{ARMA}(p,q)$. 

# Log transformation

## Motivation

In words, if the standard deviation of the series is proportional to the level of the series, then transforming to logarithms will produce a series with approximately constant variance.

Also, if the level of the series $\mu_t$ is changing roughly exponentially, the log-transformed series will exhibit a linear trend.

## Non-stationarity in variance

Time series where increased dispersion seems to be be associated with increased levels of the series — the larger the level of the series, the more variation there is around that level and conversely.

For series with non-stationarity in variance, we usually perform logarithm transformation before taking differencing or fitting a model.

# Power transformation or Box-Cox transformations

$$
g(x)=\begin{cases}\frac{x^\lambda-1}{\lambda},&\lambda\ne0\\\log(x),&\lambda=0\end{cases},\space x>0
$$



where $\lambda$ is a parameter. Note that the power function part in $g(x)$ converges to $\log(x)$ when the exponent $\lambda\rightarrow0$.

1. $\lambda=\frac{1}{2}$ produces a square root transformation useful with Poisson like data;
2. $\lambda=1$ corresponds to a reciprocal transformation;
3. $\lambda=0$ yields the log transformation.

