---
title: Time Series Notes (5) - Parameter estimation
date: 2022-04-02 22:30:00 +0800
categories: 时间序列分析
math: true
---

# Introduction

The hyper parameters of a time series model, for example, $p,d,q$ for an $\text{ARIMA}(p,d,q)$ model is already known. And the model will be completely determined if the values of parameters like


$$
\phi_1,\dots,\phi_p,\theta_1,\dots,\theta_p,\theta_0\text{ and }\sigma_a^2
$$


are further known.

Then we have to use methods to estimate these values based on observed values from $\{Z_t\}$.

# Conditional least squares (CLS)

The conditional least square is derived from the basic linear regression, where we can see $Z_{t+1}$ as $Y$ in the linear regression and the other $Z_t,\dots,Z_{t-p}$ as the $X_i$ In the linear regression.

Consider an $\text{AR}(p)$ model $Z_t=\theta_0+\phi_1Z_{t-1}+\phi_2Z_{t-2}+\cdots+\phi_pZ_{t-p}+a_t$, the conditional least square estimates are 


$$
(\hat\theta_0,\hat\theta_1,\dots,\hat\theta_p)=\underset{\theta_0,\phi_1,\dots,\phi_p}{\text{argmin}}\sum_{t=p+1}^n(Z_t-\theta_0-\phi_1Z_{t-1}-\cdots-\phi_pZ_{t-p})^2
$$


and


$$
\hat\sigma_a^2=\frac{1}{n-p}\sum_{t=p+1}^n(Z_t-\hat\theta_0-\hat\phi_1Z_{t-1}-\cdots-\hat\phi_pZ_{t-p})^2
$$


Notice there is a common term in above two expressions. We call the term


$$
S_C(\theta_0,\phi_1,\dots,\phi_p)=\sum_{t=p+1}^n(Z_t-\theta_0-\phi_1Z_{t-1}-\cdots-\phi_pZ_{t-p})^2
$$


the **conditional sum of squares function**. In this way, we can rewrite the formulas into the following styles:


$$
\begin{align}
& (\hat\theta_0,\hat\theta_1,\dots,\hat\theta_p)=\text{argmin}\space S_C(\theta_0,\phi_1,\dots,\phi_p) \\
& \hat\sigma_a^2=\frac{1}{n-p}S_C(\hat\theta_0,\hat\phi_1,\dots,\hat\phi_p)
\end{align}
$$

## CLS on $\text{AR}(1)$ process

The conditional sum of squares function for $\text{AR}(1)$ process is


$$
S_C(\mu,\theta_0)=\sum_{t=2}^n[Z_t-\theta_0-\phi Z_{t-1}]^2
$$


Therefore, the CLS estimates $(\hat\theta_0,\hat\phi_0)$ satisfy


$$
\begin{align}
& \hat\phi=\frac{\sum_{t=2}^n(Z_t-\bar y)(Z_{t-1}-\bar x)}{\sum_{t=2}^n(Z_{t-1}-\bar x)^2} \\
& \bar y=\hat\theta_0+\hat\phi\bar x
\end{align}
$$



where


$$
\bar y=\frac{1}{n-1}\sum_{t=2}^nZ_t,\space\space\space\space\bar x=\frac{1}{n-1}\sum_{t=2}^nZ_{t-1}
$$


Especially, for large $n$,


$$
\bar y\approx\bar Z=\frac{1}{n}\sum_{t=1}^nZ_t,\space\space\space\space\bar  x\approx\bar Z
$$


Therefore,


$$
\hat\theta_0\approx(1-\hat\phi)\bar Z,\space\space\space\space\hat\mu=\frac{\hat\theta_0}{1-\hat\phi}\approx\bar Z
$$


and


$$
\hat\phi=\frac{\sum_{t=2}^n(Z_t-\bar y)(Z_{t-1}-\bar x)}{\sum_{t=2}^n(Z_{t-1}-\bar x)^2}\approx\frac{\sum_{t=2}^n(Z_t-{\color{red}\bar Z})(Z_{t-1}-{\color{red}\bar Z})}{\sum_{t=2}^n(Z_{t-1}-{\color{red}\bar Z})^2}\approx\frac{\sum_{t=2}^n(Z_t-{\color{red}\bar Z})(Z_{t-1}-{\color{red}\bar Z})}{\sum_{\color{green}t=1}^n(Z_{t-1}-{\color{red}\bar Z})^2}=r_1(\hat\rho_1)
$$

## CLS on $\text{MA}(1)$ process



Suppose we have an invertible process $Z_t=a_t+\theta a_{t-1}$, where $\vert\theta\vert<1$, the conditional sum of squares function is



$$
S_C(\theta)=\sum_{t=1}^n(a_t)^2
$$



By invertibility and truncate at $0=Z_0=Z_{-1}=Z_{-2}=\cdots$,  the above sum of squares can be written as



$$
S_C(\theta)=\sum_{t=1}^n[Z_t-\theta Z_{t-1}+\theta^2Z_{t-2}-\cdots+(-\theta)^{t-1}Z_1]^2
$$



Note that if $a_0=0$, then we have

$$
a_1=Z_1,\space\space\space\space a_2=Z_2-Z_1=Z_2-\theta a_1,\space\space\space\space a_3=Z_3-\theta Z_2+\theta^2Z_1=Z_3-\theta a_2,\space\space\cdots\space\space a_n=Z_n-\theta a_{n-1}
$$

> It is **impossible to directly calculate** the conditional least square estimates for MA and ARMA models. Some numerical optimization methods, such as Gaussian Newton,  are usually used to search the estimates.
{: .prompt-warning }

# Maximum likelihood (ML) and unconditional least squares (ULS)

For any set of observations $Z_1,Z_2,\cdots,Z_n$ (time series or otherwise), the likelihood function $L$ is defined to be the probability (density) value of obtaining the data actually observed. However, it is considered as a function of the parameters in the model.

**Advantage**: First, all of the information in the data is used rather than just the first and second moments, as is the case with least squares. Second, many large-sample results are known under very general conditions.

**Disadvantage**: The method needs a specific joint probability density function of the process, which is sometimes complex.

## ULS and ML on $\text{AR}(1)$ model

Consider an $\text{AR}(1)$ model $Z_t-\mu=\phi(Z_{t-1}-\mu)+a_t$, where the white noise $\{a_t\}\sim i.i.d.N(0,\sigma_a^2)$, and the unknown parameters are $\mu,\phi,\sigma_a^2$.

The random variable $Z_t$, conditional on $\{Z_{t-1},Z_{t-2},\dots\}$, will follow the normal distribution $N\{\mu+\phi(Z_{t−1} −\mu),\sigma_a^2\}$, and hence has the density


$$
f(z|Z_{t-1},Z_{t-2},\dots)=f(z|Z_{t-1})=(2\pi\sigma_a^2)^{-\frac{1}{2}}\exp\{-\frac{[z-\mu-\phi(Z_{t-1}-\mu)]^2}{2\sigma_a^2}\}
$$


Replace $Z_t$ as its MA presentation, we have the likelihood function for $Z_1,\dots,Z_n$ is


$$
\begin{split}
L(\phi,\mu,\sigma_a^2)&=f(z_n,z_{n-1},\dots,z_1) = f(z_n|z_{n-1},\dots,z_1)f(z_{n-1},\dots,z_1) \\
&=f(z_n|z_{n-1})f(z_{n-1},\dots,z_1)=\cdots \\
&=f(z_n|z_{n-1})f(z_{n-1}|z_{n-2})\cdots f(z_2|z_1)f(z_1) \\
&=(2\pi\sigma_a^2)^{-\frac{n}{2}}(1-\phi^2)^{\frac{1}{2}}\exp\{-\frac{1}{2\sigma_a^2}S(\phi,\mu)\}
\end{split}
$$


where


$$
\begin{split}
{\color{green}S(\phi,\mu)}&=\sum_{t=2}^n[(Z_t-\mu)-\phi(Z_{t-1}-\mu)]^2+(1-\phi^2)(Z_1-\mu)^2 \\
&={\color{red}S_C(\phi,\mu)}+(1-\phi^2)(Z_1-\mu)^2
\end{split}
$$


is called the **unconditional sum of squares function**.

> When $\mu=0$, we let $Z_1=0$, the ML method is exactly the same as the CLS method.
{: .prompt-tip }

The  ML estimates for $\mu,\phi,\sigma_a^2$ will minimize the log-likelihood function


$$
-\frac{n}{2}\log(2\pi)-\frac{n}{2}\log(\sigma_a^2)+\frac{1}{2}\log(1-\phi^2)-\frac{1}{2\sigma_a^2}S(\phi,\mu)
$$


As a compromise between conditional least squares (CLS) estimates and full maximum likelihood (ML) estimates, the **unconditional least squares (ULS) estimates** are


$$
\begin{align}
& (\hat\phi,\hat\mu)=\text{argmin}\space S(\phi,\mu)=\text{argmin}\{S_C(\phi,\mu)+(1-\phi^2)(Z_1-\mu)\} \\
& \hat\sigma_a^2=\frac{1}{n-1}S(\hat\phi,\hat\mu)
\end{align}
$$

# Properties of the estimates

The CLS, ULS and ML estimates have the same large-sample properties.

Asymptotic variances of the estimates for a few low order ARMA models are as follows:

$$
\begin{align}
\text{AR}(1)&:\text{Var}(\hat\phi)\approx\frac{1}{n}(1-\phi^2) \\
\text{AR}(2)&:\left\{\begin{matrix}\text{Var}(\hat\phi_1)\approx\text{Var}(\hat\phi_2)\approx\frac{1}{n}(1-\phi_2^2)\\\text{corr}(\hat\phi_1,\hat\phi_2)\approx-\frac{\phi_1}{1-\phi_2}=-\rho_1\end{matrix}\right. \\
\text{MA}(1)&:\text{Var}(\hat\theta)\approx\frac{1}{n}(1-\theta^2) \\
\text{MA}(2)&:\left\{\begin{matrix}\text{Var}(\hat\theta_1)\approx\text{Var}(\hat\theta_2)\approx\frac{1}{n}(1-\theta_2^2)\\\text{corr}(\hat\theta_1,\hat\theta_2)\approx\frac{\theta_1}{1+\theta_2}\end{matrix}\right. \\
\text{ARMA}(1,1)&:\left\{\begin{matrix}\text{Var}(\hat\phi)\approx\frac{1}{n}(1-\phi^2)(\frac{1+\phi\theta}{\phi+\theta})^2\\\text{Var}(\hat\theta)\approx\frac{1}{n}(1-\theta^2)(\frac{1+\phi\theta}{\phi+\theta})^2\\\text{corr}(\hat\phi,\hat\theta)\approx\frac{\sqrt{(1-\phi^2)(1-\theta^2)}}{1+\phi\theta}\end{matrix}\right.
\end{align}
$$


When estimating an $\text{AR}(1)$ process with the $\text{AR}(2)$ model, the variance of the estimates will increase. It is the same for the $\text{MA}$ models.

For the $\text{ARMA}(1,1)$ process with $\phi+\theta\sim0$, the variance may be very large.

