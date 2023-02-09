---
description: >-
  Notes taken from Mixture and Hidden Markov Models with R by Ingmar Visser and
  Maarten Speekenbrink
---

# Information Criteria

## Akaike Information Criterion

The short version: (AIC lower is better)&#x20;

$$
\begin{equation*}
AIC = -2 * l(\hat{\theta}|y_{1:T}) + 2k + \frac{2k(k+1)}{T-K-1}
\end{equation*}
$$

The long version: model should be selected based on maximizes the expected log likelihood with respect to the true distribution. This subsequently means minimizes the [KL divergence](measuring-difference-in-probability-distributions.md#kullback-leibler-kl-divergence) between the true distribution and the approximated model.&#x20;

$$
\begin{equation*}
D(f^*||f) = \int f^*(y) \log \frac{f^*(y)}{f(y|\theta)} \partial y = E_{f^*}[\log f^*(Y)] - E_{f^*}[\log f(Y|\theta)]
\end{equation*}
$$

Notice the first term is constant. So the goal is to maximize the second term. The expectation of the log likelihood under true distribution is estimated through the observed log likelihood&#x20;

$$
\begin{equation*}
E_{f^*}[\log f(Y|\theta)] \approx \frac{1}{T} \sum_{t=1}^T \log f(y_t | \theta)
\end{equation*}
$$

But this estimation is biased because here the data are used to estimate the parameters as well as the expectation. The bias is approximately $$k / T$$ where $$k$$ is the number of freely estimated parameters.&#x20;

So AIC is the bias corrected average log likelihood estimate with a multiplier&#x20;

$$
\begin{equation*}
AIC = -2 * T * \frac{1}{T} \sum_{t=1}^T \log f(y_t|\theta) + 2k = -2 * l(\hat{\theta}|y_{1:T}) + 2k
\end{equation*}
$$

If the number of observation is small compare to the maximum number of estimated parameters, we add a second-order bias correction term&#x20;

$$
\begin{equation*}
AIC = -2 * l(\hat{\theta}|y_{1:T}) + 2k + \frac{2k(k+1)}{T-K-1}
\end{equation*}
$$



## Bayesian Information Criterion&#x20;

BIC aim to select the model with the highest posterior probability. Let $$M_i = \{ f(Y|\theta_i)|\theta_i\in\Theta_i \}$$be a candidate model family. The posterior probability of the model family is computed as the marginal probability over all possible parameter values:&#x20;

$$
\begin{align*}
& P(M_i|y_{1:T}) \propto P(M_i) \int f(y_{1:T}|\theta_i, M_i) f(\theta_i | M_i) \partial \theta_i \\
& \log P(M_i|y_{1:T}) \propto \log P(M_i) + \log f(y_{1:T}|M_i) \\
& \log f(y_{1:T}|M_i) = \log \int f(y_{1:T}|\theta_i, M_i) f(\theta_i | M_i) \partial \theta_i
\end{align*}
$$



* $$P(M_i)$$is the prior probability of model family&#x20;
* $$f(y_{1:T}|\theta_i, M_i)$$is the likelihood of data given model family and parameters&#x20;
* $$f(\theta_i|M_i)$$ is the prior density over the model parameters&#x20;
* Taking integral we get log marginal likelihood $$f(y_{1:T}|M_i)$$
* Take log to decouple two terms&#x20;

Consider a single model $$M$$, we can approximate the posterior probaility by using Taylor series expansion&#x20;

$$
\begin{equation*}
\log f(y_{1:T}|M) = \log f(y_{1:T}|\hat{\theta}, M) + \log f(\hat{\theta}|M) + \frac{k}{2}\log(2\pi) - \frac{k}{2}\log(T) - \frac{1}{2}|I(\hat{\theta})| + O(n^{-\frac{1}{2}})
\end{equation*}
$$











