---
description: 'Visser: Sec 2.3'
---

# Mixture Model Parameter Estimation

## Maximum Likelihood Estimator

MLE is defined as the parameters that maximizes the [likelihood function](../mixture-model-setup.md#likelihood) defined in previous chapter given the observation (treating observation as fixed)

$$
\begin{equation*}
\hat{\theta} = \argmax_\theta L(\theta | y_{1:T})
\end{equation*}
$$

### Label Switching

In a mixture model, the components can permute and still has the same distribution. (e.g.: If there are two normal distribution state, just swap the paramters for state 1 with parameters for state 2). It has more severe consequences when using Bayesian paramter estimation and inference techniques, and bootstrapping. In that cause, relabeling components to make them consistent over iterations becomes necessary.&#x20;

## Method 1: Numerical Optimization of the Likelihood&#x20;

Numerical optimization can iteratively find the minimum of a function. Look at below trivial example

{% code overflow="wrap" %}
```r
GaussMix2 <- function(par, y){
# set parameter values 
p1 <- par[1]; p2 <- 1 - par[1]
m1 <- par[2]; m2 <- par[3]; s1 <- par[4]; s2 <- par[5]
# return negative log-likelihood 
-sum(log(p1 * dnorm(y, mean = m1, sd = s1) + p2 * dnorm(y, mean = m2, sd = s2)))

}

# define starting values 
pstart <- c("p1"=.5,"m1"=1,"m2"=2,"s1"=.5,"s2"=.5)

# create a fake mixture Gaussian 
y_obs <- c(rnorm(1000,0,1), rnorm(1000,5,2))

# run optim 
opt <- optim(par=pstart,fn=GaussMix2,y=y_obs,lower=c(0,-Inf,-Inf,0,0),upper=c(1,rep(Inf,4)),method="L-BFGS-B")

# read output 
opt$par 

#        p1         m1         m2         s1         s2 
# 0.50854228 0.03480772 5.02649332 0.98800874 2.03568627 

```
{% endcode %}

* _GaussMix2_ is a function that takes in parameters and observation y. It then finds the minimum of the negative log likelihood:$$\sum_{t=1}^N (-1) * (\pi_1f_1(y_t) + \pi_2f_2(y_t))$$
* _optim_ arguments passed are&#x20;
  * _par = pstart_: Initial values for the parameters to be optimized over&#x20;
  * _fn = GaussMix2_: The function toe be minimized. First argument is the vector of parameters that needs to be minized&#x20;
  * _y=y\_obs:_ the additional argument required for _fn_
  * The requirement of mixing probabilities have to be between 0 and 1 and positive standard deviation is enforced through _lower_ and _upper_ arguments&#x20;

## Expectation Maximization (EM) Algorithm&#x20;

### EM Goal and Use Canse:&#x20;

Parameter estimation for problems with missing or latent data and maximum likelihood estimation would be easy if we know the missing or latent values.

### EM Idea&#x20;

Impute expected values for the latent data / states and then estimate parameters by optimizing the joint likelihood of the parameters given the observation and imputed latent states. Then we can separately perform the following step&#x20;

* Expectation step: (Re)compute expectation of the complete-data log likelihood (The likelihood of observated data + hidden states).&#x20;
* Maximization step: Expected complete-data log likelihood is a function of parameters $$\theta$$, but not the unobserved state $$S$$. So we can find the optimal values of $$\theta$$ by maximizing expected complete-data log likelihood.&#x20;

### EM in Mixture Model&#x20;

The joint log-likelihood function (complete-data log likelihood) can be decomposed into two parts:&#x20;

$$
\begin{equation*}
\log f(y_{1:T}, s_{1:T}|\theta) = \sum_{t=1}^T \log P(s_t|\theta_{pr}) + \sum_{t=1}^T \log f(y_t|s_t, \theta_{obs})
\end{equation*}
$$

* The first part is the probability of $$S_t$$ (state at time $$t$$) given the mixing probability parameter $$\theta_{pr}$$. The second part is the probability of $$Y_t$$ (observation at time $$t$$) given the state at that time and the density parameters $$\theta_{obs}$$.&#x20;

The expectation of complete-data log likelihood:&#x20;

$$
\begin{align*}
Q(\theta, \theta') 
& \equiv E_{s_{1:T}|y_{1:T}, \theta'}\left[ \log f(y_{1:T}, s_{1:T}|\theta)\right] \\
& = \sum_{s_{1:T}\in S^T} P(s_{1:T} | y_{1:T},\theta')\log f(y_{1:T}, s_{1:T}|\theta)\\
& = \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i) \log P(s_t = i|\theta_{pr}) + \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log f(y_t | s_t = i, \theta_{obs}) \\
\gamma_t(i) & = p(s_t=i|y_t, \theta')
\end{align*}
$$

* In the definition, the expectation is w.r.t hidden state across each observation ($$s_{1:T}$$) given the observations and initial / previous guess parameter $$\theta'$$.&#x20;
* The expectation is a function of the true parameter and the initial / guess parameters&#x20;
* The second equal sign is only for mixture model. It applies the defnition of expectation. Notice $$s_{1:T}\in S^T$$ is just a short hand for all possible permutation of hidden states from time 1 to T.&#x20;
* The third equation can be also think of as directly applying the definition of expectation over the previous listed complete-data log likelihood equation.&#x20;
* $$\gamma$$ function is the posterior probabilities of the state given the initial / previous guess parameters.&#x20;

So overall, the EM algorithm for mixture model can be described as&#x20;

1. Start with an initial set of parameters $$\theta'$$
2. Repeat until convergence:&#x20;
   1. Compute the [posterior probabilities](../mixture-model-setup.md#posterior-probabilities) $$\gamma$$and the [log-likelihood](../mixture-model-setup.md#likelihood) $$l(\theta'|y_{1:T})$$as described in previous chapter.&#x20;
   2. Obtain new estimates&#x20;
      * $$\begin{equation*} \hat{\theta}_{pr} = \overset{\text{argmax}}{\theta_{pr}} \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log P(s_t = i|\theta_{pr}) \end{equation*}$$
      *   $$\begin{equation*} \hat{\theta}_{obs} = \overset{\text{argmax}}{\theta_{obs}} \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log f(y_t|s_t = i,\theta_{obs}) \end{equation*}$$


   3. Set $$\theta'=(\hat{\theta}{pr}, \hat{\theta}{obs})$$

The convergence can be checked by 1) the norm of the parameter guesses or 2) relative increase in the log-likelihood. This can be done in R through package _depmixS4._&#x20;

In summary, EM algorithm for Misture Class Model alternative between&#x20;

1. computing the expected component membership probabilities or the posterior state probabilities
2. optimizing the response model parameters conditional on these expected component memberships

## Handling Constraint

Linear (in)equality constraint can be represented as&#x20;

$$
I \leq A\theta\leq U
$$

And numerical optimization can be performed using packages such as _Rsolnp, Rdonlp2, depmixS4_ &#x20;

## Starting Values

We have two choices, especially concerning EM algorithm.&#x20;

1. Start with density parameter values for components (obtained through methods such as K-means) and equal value for mixing probability&#x20;
2. Start with posterior probability (such as random assignment of each case to a component, resulting in 1 hot vector for posterior probability) and directly go to maximization step in the first iteration&#x20;
