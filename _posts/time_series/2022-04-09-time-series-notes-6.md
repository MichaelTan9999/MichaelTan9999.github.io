---
title: Time Series Notes (6) - Parameter estimation
date: 2022-04-09 16:00:00 +0800
categories: 时间序列分析
math: true
---

# Residual Analysis

## Definition of residuals

Consider an $\text{AR}(1)$ model with a constant term: $Z_t=\phi Z_{t-1}+\theta_0+a_t$, having estimated $\phi$ and $\theta_0$, the residuals are defined as

$$
\hat a_t=Z_t-\hat\phi Z_{t-1}-\hat\theta_0
$$

If the model is correctly specified and the parameter estimates are reasonably close to the true values, then the residuals should have nearly the properties of a white noise: $i.i.d.$ with zero mean and common variances. Deviations from these properties would indicate an indequacy of the fit and we will search for a more appropriate model.

## Calculation of residuals

Consider an $\text{ARMA}(1,1)$ model

$$
Z_t-\hat\mu=\hat\phi_1(Z_{t-1}-\hat\mu)+\cdots+\hat\phi_p(Z_{t-p}-\hat\mu)+\hat a_t-\hat\theta_1\hat a_{t-1}-\cdots-\hat\theta_q\hat a_{t-q}
$$

The residuals can be calculated as follows,

$$
\hat a_t=\sum_{j=0}^\infty\hat\pi_j(Z_{t-j}-\hat\mu)
$$

Where $\hat\pi_j$s are functions of $\hat\phi_1,\dots,\hat\phi_p$, and $\hat\theta_1,\dots,\hat\theta_q$, and the initial values $Z_s=\hat\mu$ for $s\le0$.

> The invertibility is necessary to calculate the residuals.
{: .prompt-warning }

Standardized residuals: $\{a_t/s\}$, where $s^2$ is the sample variance of the residual sequence.

## Ways of residual analysis

| Method                                                       | Target                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| (1) the time plot of the residual sequence;                  | to check whether or not there still exist some patterns not yet explained by the fitted model |
| (2) the histogram; (3) the quantile-quantile plot; (4) some formal normality test; | to check the possible normality of the residuals             |
| (5) the correlogram; (6) Ljung-Box test;                     | to check for the autocorrelations                            |

1. If the time plot of the residuals is like that of a white noise, we can say that there is no more patterns explained by the fitted model, though it is very difficult to see something by this way.
2. The histogram and the Q-Q plot always give a very direct graph to show the normality while the normality test such as Shapiro-Wilk normality test gives a numerical result.
3. The upper and lower bounds of the correlogram, the sample autocorrelation function (ACF) of the residuals, can be calculated through The Bartlett’s approximation.

### Ljung-Box test

For a fitted $\text{ARMA}(p,q)$ model, the Ljung-Box test is modified on Box & Pierce test statistic. Ljung-Box test statistic is defined as


$$
Q_*=n(n+2)(\frac{\hat\rho_1^2}{n-1}+\frac{\hat\rho_2^2}{n-2}+\cdots+\frac{\hat\rho_K^2}{n-K})\sim\chi_{K-m}^2,
$$


where $K$ is predetermined integer, $\hat\rho_j^2$ is the sample ACF of the residuals, and $m=p+q$. The sample size $n$ is that of the corresponding ARMA model.

> The degrees of freedom is unchanged with or without the intercept.
{: .prompt-info }

# Analysis of over-parameterized models

We usually overfit a time series to confirm the selected model. For example, if we want to pick an $\text{AR}(2)$ model, we will use an $\text{AR}(3)$ model to overfit. The original $\text{AR}(2)$ model would be confirmed if:

1. the estimate of the additional parameter, $\phi_3$, is not significantly different from zero, and
2. the estimates for the parameters in common, $\phi_1$ and $\phi_2$, (and $\theta_0$ if it exists,) do not change significantly from their original estimates.

Some guidelines:

1. Check simple models before trying complicated ones.
2. When overfitting, do not increase the orders of the $\text{AR}$ and $\text{MA}$ parts of the model simultaneously.
3. Extend the model in directions suggested by an analysis of the residuals. If after fitting an $\text{MA}(1)$ model, substantial correlation remains at lag <font color = 'red'>2</font> in the residuals, try an $\text{MA}(2)$, not an $\text{ARMA}(1,1)$.
