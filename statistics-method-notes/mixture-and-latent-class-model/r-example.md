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

Context:&#x20;

Response variables at 5 different trials, _r1_ to _r5_. At each trial, there is a correct strategy and incorrect strategy. &#x20;

1. Use BIC weights to get relative likelihood of model&#x20;
2. Fitting multivariate distribution&#x20;
3. Debugging on issue around singularity&#x20;
4. Fitting independent Gaussian

### BIC weights and relative probability&#x20;

```r
# BIC Weights 
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

### Fitting multivariate normal distribution response model&#x20;

{% code overflow="wrap" lineNumbers="true" %}
```r
# Fitting Multivariate Distribution 
require(hmmr)
data('conservation')

dat <- conservation[,c("r1", "r2")]
dat <- dat[rowSums(is.na(dat))==0,]

fModels <- list()

for (nstate in 2:6){
  rModels <- list()
  for (i in 1:nstate){
    # For each state, create a MVN response object stored in a list 
    rModels[[i]] <- list(MVNresponse(cbind(r1,r2) ~ 1, data = dat))
  }
  
  priorMod <- transInit(~1, ns = nstate, data = dat,family = multinomial("identity"))

  mod <- makeMix(response = rModels, prior = priorMod)
  
  fModels[[paste0(nstate)]] <- fit(mod)
}

# obtain log likelihood 
sapply(fModels, logLik)

# notice sometime the log likelihood for model with 3 components is lower than 2 component. This is due to reaching local optimum.
From theory we know this is not correct. 
fit(fModels[["3"]])
```
{% endcode %}

1. _MVNresponse_ creates default multivariate normal distributed response.&#x20;
2. _transInit_ is specified as no response. The response in our case is the latent component probability.&#x20;
3. _transition_ argument for _makeMix_ is skipped since they are used for HMM model.&#x20;

### Degenerate solutions due to singular covariance matrix&#x20;

```r
# Singularity issue 

set.seed(2)
rModels <- list()
for (i in 1:6){
  rModels[[i]] <- list(MVNresponse(cbind(r1,r2)~1, data = dat))
}
priorMod <- transInit(~1, ns = nstate, data = dat,family = multinomial("identity"))
mod <- makeMix(response = rModels, prior = priorMod)
res <- fit(mod)

# We would get below warning message: 
#Warning message:
#In em.mix(object = object, maxit = emcontrol$maxit, tol = emcontrol$tol,  :
#  Log likelihood decreased on iteration 13 from 56.6121503690555 to 5.52464472624439


# we essentailly now use different starting point until the method: 
# 1. did not error out, 2. converges, and 3. have higher likelihood then 5 components

ok <- FALSE
while(!ok){
  tmp <- try(fit(fModels[["6"]]), silent = TRUE)
  if ((!inherits(tmp, "try-error")) && 
    startsWith(tmp@message, "Log Likelihood converged") &&
    logLik(tmp) >= logLik(tModels[["5"]])) ok <- TRUE
}
# Use compact argument to get full info 
summary(fModels[["4"]], compact = FALSE)

```

This is because our estimated covariance matrix for one component is singular. The covariance matrix transform to correlation matrix result in a 2 by 2 matrix of ones. This suggests perfect collinearity between two dimensions. So our bivariate Gaussian ellipses is just a line in 2D space. This often is a sign of mis-specification. This can also be seen from the relative small estimated state probability, singling not too much population in that component.&#x20;

### Fitting Independent Gaussian Distribution&#x20;

<pre><code><strong># Fitting independent Gaussian distribution 
</strong>gm4indep &#x3C;- fit(mix(
  list(r1 ~ 1, r2 ~ 1),  # each element is formula describing the GLM
  nstates = 4, 
  family = list(gaussian(), gaussian()), # specify distribution for each response
  data = dat
))

# obtain AIC and BIC
c("AIC" = AIC(gm4indep), "BIC" = BIC(gm4indep))

# get likelihood ratio 
llratio(fModels[["4"]], gm4indep)

# result suggest independent Gaussian is enough 
</code></pre>
