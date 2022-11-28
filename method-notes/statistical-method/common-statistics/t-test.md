---
description: >-
  Notes are primarily taken from the textbook Mathematical Statistics and Data
  Analysis by John Rice 3rd edition
---

# t-test

### Usage:&#x20;

Test if two samples $$X_1, ..., X_n$$and $$Y_1, ..., Y_m$$ have the same mean. The assumption is that $$X$$ and $$Y$$ are iid samples from a normal distribution with the same variance $$\sigma^2$$. (There is variation method for non-homogeneous variance assumption). If the underlying distributions are not normal but the sample size are large, the use of t distribution can be justified by the CLT. But at the same time, a high degree of freedom t distribution is similar to normal distribution.

### Motivation:&#x20;

Notice the MLE estiamte for $$\mu_x - \mu_y$$is $$\bar{X} - \bar{Y}$$. Since $$X, Y$$ are normally distributed, $$\bar{X} - \bar{Y}$$ can be expressed as a linear combination of independent normally distributed random variables with the distribution:&#x20;

$$
\bar{X} - \bar{Y} \sim N\left(\mu_x - \mu_y,  \sigma^2 + (\frac{1}{n} + \frac{1}{m})\right)
$$

So then if we know the variance $$\sigma^2$$, we can construct the confidence interval for $$\mu_x - \mu_y$$based on standard normal&#x20;

$$
Z = \frac{(\bar{X} - \bar{Y}) - (\mu_x - \mu_y)}{\sigma \sqrt{\frac{1}{n} + \frac{1}{m}}}
$$

However, generally $$\sigma^2$$is unknown, and estimated from the pooled sample variance&#x20;

$$
\begin{align*}
        & s_p^2 = \frac{(n-1)s_x^2 + (m-1)s_y^2}{m+n-2} \\
        & s_x^2 = \frac{1}{(n-1)}\sum_{i=1}^n (x_i - \bar{X})^2
    \end{align*}
$$

### Formal Definition:&#x20;

The following statistics follow a $$t$$ distribution with $$m+n-2$$ degrees of freedom&#x20;

$$
t = \frac{(\bar{X}-\bar{Y}) - (\mu_x - \mu_y)}{s_p \sqrt{\frac{1}{n} + \frac{1}{m}}}
$$



The confidence interval of $$100 * (1-\alpha)$$ for the difference in mean can be constructed as&#x20;

$$
\begin{align*}
        & (\bar{X} - \bar{Y}) \pm t_{m+n-2} (\alpha / 2)s_{\bar{X}- \bar{Y}}\\
        & s_{\bar{X}- \bar{Y}} = s_p \sqrt{\frac{1}{n} + \frac{1}{m}}
    \end{align*}
$$

The alternative hypothesis can be expressed as:&#x20;

$$
\begin{align*}
        & \text{Two-sided}: |t| > t_{n+m-2}(\alpha / 2) \\
        & \text{One sided greater}: t > t_{n+m-2}(\alpha)
    \end{align*}
$$



## Power

### Definition

The power of a test is the probability of rejecting the null hypothesis when it is false.&#x20;

### Case of two-sample t test and using normal distribution approximation

For a two-sample $$t$$ test, power depends on&#x20;

1. The real difference $$\triangle = \left|{\mu_x - \mu_y}\right|$$
2. $$\alpha$$
3. Population standard deviation $$\sigma$$
4. Sample size $$n, m$$

Suppose that $$\sigma, \alpha, \triangle$$ are all given and both samples are size $$n$$, then we can get the varaince of mean difference as&#x20;

$$
Var(\bar{X} - \bar{Y}) = \sigma^2 (\frac{1}{n} + \frac{1}{n}) = \frac{2\sigma^2}{n}
$$

The two tailed test at level $$\alpha$$is now based on the standard normal (approximating t with normal distribution):&#x20;

$$
Z = \frac{\bar{X} -\bar{Y}}{\sigma \sqrt{2/n}}
$$



So the rejection region is $$\left| Z \right| > z(\alpha/2)$$ or $$\left|\bar{X} - \bar{Y}\right| > z(\alpha/2) \sigma \sqrt{2/n}$$

The power of this test is the probability that the test statistics falls in the rejection region under null hypothesis, which is :&#x20;

$$
\begin{align*}
        & P\left( \left|\frac{\bar{X} -\bar{Y}}{\sigma \sqrt{2/n}}\right| > z(\alpha/2) \right)\\
        & = P\left(  \left|\bar{X} - \bar{Y}\right| > z(\alpha/2) \sigma \sqrt{2/n} \right) \\
        & = P\left(\bar{X} - \bar{Y} >  z(\alpha/2) \sigma \sqrt{2/n} \right) + P\left( \bar{X} - \bar{Y} <  -z(\alpha/2) \sigma \sqrt{2/n} \right) \\
        & = P\left(\frac{(\bar{X} - \bar{Y})-\triangle}{\sigma \sqrt{2/n}} >  \frac{z(\alpha/2) \sigma \sqrt{2/n}-\triangle}{\sigma\sqrt{2/n}} \right) + P\left( \bar{X} - \bar{Y} <  -z(\alpha/2) \sigma \sqrt{2/n} \right)\\
        & = 1 - \Phi\left( z(\alpha/2) - \frac{\triangle}{\sigma}\sqrt{n/2}\right) +  P\left( \bar{X} - \bar{Y} <  -z(\alpha/2) \sigma \sqrt{2/n} \right)\\
        & =  1 - \Phi\left( z(\alpha/2) - \frac{\triangle}{\sigma}\sqrt{n/2}\right) + \Phi\left( -z(\alpha/2) - \frac{\triangle}{\sigma}\sqrt{n/2}\right)
\end{align*}
$$







