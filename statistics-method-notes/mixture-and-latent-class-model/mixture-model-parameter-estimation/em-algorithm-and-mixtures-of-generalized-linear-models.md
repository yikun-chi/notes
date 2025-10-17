# EM Algorithm and Mixtures of Generalized Linear Models

## Generalized Linear Models (GLM)

### Component of GLM&#x20;

GLM assumes the observations follows an exponential distribution (for simplicity, here is a good [Stack Exchange](https://stats.stackexchange.com/questions/284260/why-do-we-assume-the-exponential-family-in-the-glm-context) post about this, and a good notes from [Ryan Tibshirani](https://www.stat.cmu.edu/~ryantibs/advmethods/notes/glm.pdf) on GLM) &#x20;

$$
\begin{equation*}
f(y_t) = \exp \left(w_t \frac{y_t\theta_t - b(\theta_t)}{\theta} + c(y_t, \phi / w_t)\right)
\end{equation*}
$$

* $$\theta_t, \phi$$ : location and scale parameters&#x20;
  * often $$\phi$$ is a known constant, such as for Binomial and Poisson distribution it equals 1. If it is unkown, it is estimated by method of moments. Note that $$\theta_t$$ and $$\phi$$ are orthogonal parameters, so their estimation does not affect each other
* $$w_t$$: known weights
* $$b, c$$: known functions&#x20;

The mean and variance are&#x20;

$$
\begin{align*}
& E[y_t] = \mu_t = b'(\theta_t) & Var(y_t) = \frac{\phi}{w_t}b''(\theta_t)
\end{align*}
$$

The distribution can also depend on predictors $$x_j$$through a linear function such that the mean is assumed to be a smooth function of the linear combination of predictors&#x20;

$$
\begin{align*}
& \mu_t = g(\eta_t) = g(\beta_0 + \beta_1x_{1t} + \cdots + \beta_k x_{kt}) \\
& \eta_t = g^{-1}(\mu_t) = h(\mu_t)
\end{align*}
$$

* $$h$$ is also called the link function
* If $$h=(b')^{-1}$$, we call this the canonical link function and $$\theta_t = \eta_t$$

### Model Likelihood

The log-likelihood for $$T$$ independent observations from an exponential family distribution is&#x20;

$$
\begin{equation*}
l(\theta_{1:T}, \phi | y_{1:T}) = \sum_{t=1}^T w_t \frac{y_t\theta_t - b(\theta_t)}{\phi} + \sum_{t=1}^Tc(y_t, \phi/w_t)
\end{equation*}
$$

* to note again, likelihood function is not pdf. It is a function w.r.t parameters.&#x20;

### Estimate GLM Through Iteratively Weighted Least Squares&#x20;

GLM model beta coefficients can be estimated through Iteratively Weighted Least Squares, and it can be shown that this is equivalent to maximum likelihood estimates.&#x20;

Start with an initial guess of $$\hat{\eta_t}$$ and $$\hat{\mu_t} = h(\hat{\eta_t})$$

1. Obtain working response $$z_t = \hat{\eta}_t + \frac{y_t - \hat{\mu}_t}{\frac{\partial\mu_t}{\partial\eta_t}}$$
2. Obtain working weight $$W_t = \frac{w_t}{b''(\hat{\theta}_t)}(\frac{\partial\mu_t}{\partial\eta_t})^2$$
3. Estimate $$\hat{\beta} = (X^TWX)^{-1}X^TWz$$
   * $$X$$: model matrix&#x20;
   * $$W$$: diagonal matrix with the working weights&#x20;
   * $$z$$: vector of working response
4. Obtain new estimate $$\hat{\eta}=X\hat{\beta}$$

Repeat the steps until convergence&#x20;

### Connection to EM Algorithm and Mixture Model&#x20;

Summary: We can use IWLS to maximize individual component density numerically if each component density of mixture model is in GLM family.&#x20;

Recall in mixture model, we can define joint log-likelihood as a function of the data, hidden states, and parameters. The expectation of complete data log-likelihood over all possible hidden states is&#x20;

$$
\begin{align*}
Q(\theta, \theta') & =\sum_{t=1}^T \sum_{i=1}^N \gamma_t(i) \log P(s_t = i|\theta_{pr}) + \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log f(y_t | s_t = i, \theta_{obs}) \\
\gamma_t(i) & = p(s_t=i|y_t, \theta')
\end{align*}
$$

Part of the EM process is then to obtain the parameters estiamte that maximizes this expected complete data log likelihood.&#x20;

$$
\begin{equation*} \hat{\theta}_{obs} = \overset{\text{argmax}}{\theta_{obs}} \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log f(y_t|s_t = i,\theta_{obs}) \end{equation*}
$$

If each component density has a separate set of parameters, then they can be estimated separately. So the parameter for component $$i$$ can be estimated via maximizing the weighted likelihood:&#x20;

$$
\begin{equation*} \sum_{t=1}^T \gamma_t(i)\log f_i(y_t|s_t = i,\theta_{obs}^{(i)}) \end{equation*}
$$

Notice the tricky part of the EM for Mixture model is that it is not always possible to analytically obtain the actual maximum. But if the density function for component $$i$$ is from the generalized linear model family, we can rewrite the formula as&#x20;

$$
\begin{align*} 
& \sum_{t=1}^T w'_t \frac{y_t\theta_t - b(\theta_t)}{\phi} + \sum_{t=1}^T\gamma_t(i)c(y_t, \phi/w_t)\\
& w'_t =\gamma_t(i)w_t
\end{align*}
$$

Note that only the first part impacts the estimation of $$\beta$$, and subsequently $$\theta$$, we can thus rely on IWLS routine using weights $$w'_t$$.&#x20;

Further note that we cannot use IWLS to estimate $$\phi$$ since the wieghts are not the same ($$w'_t$$ vs. $$w_t$$). But generally $$\phi$$ is estimated through a method of meoments anyway.&#x20;









