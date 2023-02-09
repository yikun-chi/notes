---
description: 'Visser & Speekenbrink: Sec 2.6'
---

# Mixture Model Model Selection

## Method 1: Likelihood Ratio Test

[Likelihood ratio test](parameter-inference.md#likelihood-ratio-test) can be used when regularity conditions and models have the same number of mixture components.&#x20;

Alternatively, we can use bootstrapping method instead of the actual chi-square distribution to test  $$k$$ components vs. $$k + l$$ components. The process are&#x20;

1. Fit the $$k, k+l$$ component mixture models to obtain MLE estimates $$\hat{\theta}_k, \hat{\theta}_{k+l}$$ and compute the likelihood ratio $$\Lambda_{obs}$$
2. For $$i=1,...,M$$:&#x20;
   1. Use the fitted $$k$$ component model to simulate a bootstrap sample&#x20;
   2. Fit the $$k, k+l$$ component mixture models to the bootstrap data and calculate the likelihood ratio&#x20;
3. Calculate the proportion of bootstrap likelihood ratio which are equal or larger than observed.&#x20;

## Method 2: Information Criteria&#x20;

[See information criteria](../miscellaneous-in-stat-linear-algebra-and-deep-learning/information-criteria.md)



















