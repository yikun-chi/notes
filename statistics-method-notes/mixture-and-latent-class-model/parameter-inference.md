---
description: 'Visser & Speekenbrink: Sec 2.4, 2.5'
---

# Mixture Model Parameter Inference

## Likelihood Ratio Test&#x20;

The likelihood ratio test is used to test constraints on the parameter space. The statistics is defined as&#x20;

$$
\begin{align*}
& \Lambda = -2 * \left( l(\hat{\theta}_u|y) - l(\hat{\theta}_r|y) \right) \\
& \Lambda \sim \Chi^2(v) \\
& v = dim(\hat{\theta}_u) - dim(\hat{\theta}_r)
\end{align*}
$$

* $$\hat{\theta}_u$$ is the MLE parameters of the unconstrained model, and $$\hat{\theta}_r$$ is the MLE parameters of the constrained model.&#x20;
* The distribution assumes&#x20;
  * Constrained model estimates are the true parameters&#x20;
  * Under some regularity conditions&#x20;
  * True parameter vectors is an interior point of the parameter space (cannot lie on the boundary, such as standard deviation or mixing probability equals 0)&#x20;

If the constrained model estimates are the true parameters, then under some regularity conditions, the statistics is asymptotically chi-squared distributed with degrees of freedom&#x20;

### Code Example

In _depmixS4_, the likelihood ratio can be performed using _llratio_ function

```r
// Some code

# Create and fit mixture model 
spmix2 <- mix(RT~1, data = speed1, nstates = 2)
fspmix2 <- fit(spmix2, verbose = FALSE)

pst <- getpars(fspmix2)
pst[c(4,6)] <- 0.25 # manual set some parameters to create a constrained model 
spm2 <- setpars(spmix2, pst)
fsmix2c <- fit(spm2, equal = c(1,1,1,2,1,2), method = "rsolnp")
# The equal argument is used to specify equality constraints: 
## parameters with same integer number in this vector are estimated to be equal
## except 0 and 1, which indicates free and fixed parameter. 
llratio(fspmix2, fspmix2c)
```

## Standard Errors and Hessian Matrix

Under regularity conditions, the MLE estimates asymptotically follow a multivariate Gaussian distribution&#x20;

$$
\begin{align*}
& \hat{\theta} \sim Normal(\theta^*, \Sigma_{\hat{\theta}}) \\
& \Sigma_{\hat{\theta}} = I(\theta^*)^{-1} / T \\
& J(\theta) = -\frac{1}{T} \left[ \frac{\partial^2}{\partial \theta_i \theta_j} l(\theta|y_{1:T}) | \theta \right] \\
& \hat{\Sigma}_{\hat{\theta}} = J(\theta)^{-1} / T = \left[ \frac{\partial^2}{\partial \theta_i \theta_j} -1 * l(\theta|y_{1:T}) | \hat{\theta} \right]^{-1}=H^{-1}
\end{align*}
$$

* $$T$$ is the number of observation&#x20;
* $$I(\theta^*)$$ is the [Fisher information matrix](../miscellaneous-in-stat-linear-algebra-and-deep-learning/fisher-information.md). This can be approximated with the observed Fisher information matrix $$J(\theta)$$
* The last line shows that the actual variance matrix of the multivariate Gaussain can be approximated with the inverse of the Hessian matrix $$H$$ of the minus log-likelihood evaluated at estimated parameters $$\hat{\theta}$$

This means that if we know the Hessian matrix $$H$$, the standard error of the parameters estimate can be obtained via the square root of the diagonal elements of the inverse.&#x20;

### Obtain Hessian as part of numerical optimization&#x20;

If the log-likelihood is optimized using direct numerical optimization, the Hessian can be estimated during the optimization.  For example:&#x20;

{% code overflow="wrap" lineNumbers="true" %}
```r
# estimate 
nlmEst <- nlm(GaussMix2, pstart, y = y, hessian = TRUE) 
# GaussMix2 is a function that takes in parameter value and y, and then reutrn the negative log-likelihood 

# obtain hessian 
hess1<- nlmEst$hessian 
# get square-root of the diagonal of the inverse 
nlmSes <- sqrt(diag(solve(hess1)))
```
{% endcode %}

### Obtain Hessian via Finite Difference Approximation&#x20;

{% code overflow="wrap" lineNumbers="true" %}
```r
require(numDeriv)
opt <- optim(pstart, fn = GaussMix2, y = speed1$RT, lower = c(0, - Inf, -Inf, 0, 0), upper = c(1, rep(Inf, 4)), method = "L-BFGS-B")
hess2 <- hessian(GaussMix2, opt$par, y=y)

require(nlme)
hess3 <- fdHess(opt$par, GaussMix2, y = y)$Hessian 
# here par can be the estimated parameter from optim 
```
{% endcode %}

_depmixS4_ provides _vcov(), confit(), standardError()_ methods for variance-covarince, confidence interval, and standard error.&#x20;

### Obtain Hessia via Parametric Bootstrap&#x20;

If it is possible to simulate data from the fitted mixture model, the variance - covariance matrix can be obtained via&#x20;

1. Fit a mixture model to the data and obtain estimates&#x20;
2. Repeat&#x20;
   1. Use the fitted model to simulate data by first draw a component distribution and then draw a data point from the component distribution
   2. Fit the mixture model to the simulated data and obtain estimate&#x20;
3. Order the estimate from simulation process to obtain sample quantiles.&#x20;

{% code overflow="wrap" lineNumbers="true" %}
```r
require(boot) 

speed.rg <- function(data, mle) {
    simulate(data)
} 

speed.fun <- function(data){
    getpars(fit(data, verbose = FALSE, emcontrol = em.control(random.start = FALSE))) }
    
speed_boot_par <- boot(fspmix2, speed.fun, R = 1000, sim = "parametric", ran.gen = speed.rg) 
```
{% endcode %}

* _boot_ first argument is observed data, but in this case we pass the fitted _fspmix2_ object.&#x20;
* _boot_ second argument is a function that takes in data and return a vector of parameters&#x20;
* boot _ran.gen_ argument requires a function of two argument, first argument should be the observed data and second argument consist of any other information. The function should return the simulated dataset. In this case the "data" being passed is the fitted model instead of actual observed data. So we don't need to pass any actual _mle_ from the boot function call.&#x20;

### Linear Constraint Correction of Hessian&#x20;

$$
\begin{align*}
& \hat{C} = \hat{D}^{-1} -\hat{D}^{-1} A^T [A\hat{D}^{-1}A^T]^{-1} A\hat{D}^{-1} \\
& \hat{D} = \hat{H} + A^T A
\end{align*}
$$

* $$\hat{H}$$ is approximated Hessian&#x20;
*   $$A$$ is a $$c \times p$$ matrix of the Jacobian of the linear constraints ($$c$$ is the number of contraints)&#x20;









