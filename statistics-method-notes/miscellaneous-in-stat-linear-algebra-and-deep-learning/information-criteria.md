# Information Criteria

## Akaike Information Criterion

### AIC Summary

The short version: (AIC lower is better)&#x20;

$$
\begin{equation*}
AIC = -2 * l(\hat{\theta}|y_{1:T}) + 2k + \frac{2k(k+1)}{T-K-1}
\end{equation*}
$$

### AIC Derivation&#x20;

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

AIC is asymptotically efficient. If the true model is not in the set of models under consideration, AIC will asymptotically select the model that minimizes the MSE of prediction.  AIC is not consistent.&#x20;

## Bayesian Information Criterion&#x20;

### BIC Summary

$$
\begin{align*}
BIC = -2 * \log f(y_{1:T}|\hat{\theta}, M) + k * \log T
\end{align*}
$$

### BIC Derivation

BIC aim to select the model with the highest posterior probability. Let $$M_i = \{ f(Y|\theta_i)|\theta_i\in\Theta_i \}$$be a candidate model family. We want to focus on posterior probability.&#x20;

$$
\begin{align*}
\log P(M_i|y_{1:T})
& = \log f(y_{1:T}|M_i) * P(M_i) / p(y_{1:T})\\
& \propto \log P(M_i) * f(y_{1:T}|M_i) \\
& =  \log P(M_i) \int f(y_{1:T}|\theta_i, M_i) f(\theta_i | M_i) \partial \theta_i \\
& = \log p(M_i) + \log \int f(y_{1:T}|\theta_i, M_i) f(\theta_i | M_i) \partial \theta_i \\
\end{align*}
$$

Now if we have no prior information, we can assume that all models have the same prior probability. So the key term is the marginal / integrated likelihood:&#x20;

$$
\begin{equation*}
\log f(y_{1:T}|M_i) = \log \int f(y_{1:T}|\theta_i, M_i) f(\theta_i | M_i) \partial \theta_i
\end{equation*}
$$

The following notes are taken from the paper written by Adrian E. Raftery's 1995 paper in [Bayesian model selection](https://www.jstor.org/stable/271063) on approximation of a single model $$M$$ estimated at $$\theta^*$$

$$
\begin{align*}
g(\theta|M) 
& = \log p(y_{1:T}|\theta, M) * p(\theta|M) \\
& \approx g(\theta^*) + (\theta - \theta^*)^T g'(\theta^*) + \frac{1}{2}(\theta - \theta^*)^Tg''(\theta^*)(\theta - \theta^*) + o(||\theta - \theta^*||^2) \\
&  \approx g(\theta^*)  + \frac{1}{2}(\theta - \theta^*)^Tg''(\theta^*)(\theta - \theta^*)
\end{align*}
$$

* Here $$g'$$ is the vector of first partial derivatives and $$g''$$ is the Hessian matrix
* $$g'(\theta^*)=0$$ given $$\theta^*$$ is chosen to maximize the $$g(\theta)$$function
* Note from now on we skip writing conditioned on $$M$$ but it should still exists&#x20;

We can then use the function to approximate the log marginal likelihood&#x20;

$$
\begin{align*}
p(y_{1:T}|M) 
& = \int \exp g(\theta)\partial \theta \\
& \approx \exp(g(\theta^*)) * \int \exp (\frac{1}{2}(\theta - \theta^*)^Tg''(\theta^*)(\theta - \theta^*) \partial \theta \\
& \approx \exp(g(\theta^*))  \left( (2\pi)^{d/2} |-g''(\theta^*)|^{-\frac{1}{2}} + O(n^{-1}) \right)
\end{align*}
$$

* The integral is approximated through multivariate normal density (Laplace method for integrals)&#x20;

Adding log we can get&#x20;

$$
\begin{align*}
\log p(y_{1:T}|M) 
& = \log  \exp(g(\theta^*))  \left( (2\pi)^{d/2} |-g''(\theta^*)|^{-\frac{1}{2}} + O(T^{-1}) \right)\\
& = g(\theta^*) + \frac{d}{2} \log(2\pi) - \frac{1}{2}\log |-g''(\theta^*)| + O(T^{-1}) \\
& = \log p(y_{1:T}|\theta^*, M)  + \log  p(\theta^*|M) + \frac{d}{2} \log(2\pi) - \frac{1}{2}\log |-g''(\theta^*)| + O(T^{-1}) \\
& \approx  \log p(y_{1:T}|\theta^*, M)  + \log  p(\theta^*|M) + \frac{d}{2} \log(2\pi) - \frac{d}{2}\log T - \frac{1}{2}\log |I| + O(T^{-\frac{1}{2}}) \\
& \approx  \log p(y_{1:T}|\theta^*, M)  - \frac{d}{2}\log T  + O(1)
\end{align*}
$$

* At large sample, $$\theta^*$$ is approximately equal to the MLE estimate. So $$-g''(\theta^*)\approx T*I$$ , where $$I$$ is the expected [Fisher information matrix](fisher-information.md) for one observation. The expectation being taken over the data, with parameter held fixed. So we have $$|T*I|\approx T^d|I|$$. Those two expectation add the $$O(T^{-\frac{1}{2}})$$ error term&#x20;
* The last step is to ignoring all terms that are not scale with number of observation
*   $$d$$ is the number of free parameters



The convention is to use the above estimate and multiplying by minus two, hence&#x20;

$$
\begin{align*}
BIC = -2 * \log f(y_{1:T}|\hat{\theta}, M) + k * \log T
\end{align*}
$$

We can obtain approximated posterior model probability&#x20;

$$
\begin{align*}
P(M_i|y_{1:T}) \approx \frac{\exp(-\frac{1}{2} BIC_i) P(M_i)}{\sum_{j=1}^N \exp(-\frac{1}{2} BIC_j) P(M_j)}
\end{align*}
$$

If we assume all models have the same prior, then we can remove the model prior probability. The result relative model probabilities are called BIC weights.&#x20;

Notice in previous estimate we ignored the term $$\log p(\theta^*|M)$$. We can further improve the criteria by assuming a unit-information prior, i.e., assuming the pdf is a multivariate Normal distribution with mean $$\hat{\theta}$$ and covariance matrix $$I(\hat{\theta})^{-1}$$. The result just ended up being the same approximator.&#x20;

BIC is asymptotically consistent. Means it will asymptotically choose the true model (if the true model is under consideration) as number of observation goes up. BIC is not asymptotically efficient.&#x20;



Citation

1. Raftery, A. E. (1995). Bayesian Model Selection in Social Research. _Sociological Methodology_, _25_, 111–163. https://doi.org/10.2307/271063
2. Visser, I., & Maarten Speekenbrink. (2022). _Mixture and Hidden Markov Models with R_. Springer Nature.
