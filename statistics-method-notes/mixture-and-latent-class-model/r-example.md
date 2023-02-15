---
description: 'Visser & Speekenbrink: Sec 3'
---

# R Example

## S\&P 500

Objective

1. Easy fitting Gaussian mixture using _lca_ function&#x20;
2. Obtain posterior probability&#x20;

{% code overflow="wrap" %}
```r
# load package 
require(hmmr)
data('sp500')

# fit data using lca, default is Gaussian distribution. 
m1 <- lca(sp500$logret, nclasses = 1)
m2 <- lca(sp500$logret, nclasses = 2)
m3 <- lca(sp500$logret, nclasses = 3)

# obtain posterior probability 
# method for obtain posterior probability, such as vertibit, smoothing 
# smoothing in mixture model gives exact posterior probability defined previously 
pst1 <- posterior(m2, type = 'smoothing')

# local method directly gives component based on maximum posterior probability  
pstState <- posterior(m2, type = 'local')

# actual posterior probability is very different from the estimated probability
# indicates components not well separated
table(pstState) / sum(table(pstState))
```
{% endcode %}

## Conservation data&#x20;

1. Use BIC weights to get relative likelihood of model&#x20;

```r
// Some code
require(hmmr)
data('conservation')
m1 <- lca(conservation$r1, nclasses = 1)
m2 <- lca(conservation$r1, nclasses = 2)
m3_1 <- lca(conservation$r1, nclasses = 3)
m3_2 <- lca(conservation$r1, nclasses = 3)
m3_3 <- lca(conservation$r1, nclasses = 3)

bics <- c(sapply(list(m1, m2, m3_1, m3_2, m3_3) ,BIC))
bicws <- exp(-bics / 2) / sum(exp(-bics / 2))
```
