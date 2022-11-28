---
description: >-
  Notes are primarily taken from the textbook Mathematical Statistics and Data
  Analysis by John Rice 3rd edition
---

# power

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







