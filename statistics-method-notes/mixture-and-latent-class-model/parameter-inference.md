---
description: 'Visser: Sec 2.4, 2.5'
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

