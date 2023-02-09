# Model Covariates Effect on Prior Probability

Given observations $$z_{1:T}$$, and assume that we can divide covariates into the ones for prior ($$z_t^{(pr)}$$) and ones for model ($$z_t^{(obs)}$$) . The joint log-likelihood is&#x20;

$$
\begin{equation*}
\log f(Y_{1:T}, S_{1:T}|\theta, z_{1:T}) = \sum_{t=1}^T \log P(S_t|\theta_{pr}, z_t^{(pr)}) + \sum_{t=1}^T\log f(Y_t|S_t, \theta_{obs}, z_t^{(obs)})
\end{equation*}
$$

Generally, we can model the effects of covariates on initial probabilities as multinomial logistic regression.

$$
\begin{align*}
& \log \frac{P(S_t = i|\theta_{pr}, z_t^{(pr)})}{P(S_t = N|\theta_{pr}, z_t^{(pr)})} = z_t^{(pr)}\beta_i \\
& P(S_t = i|\theta_{pr},  z_t^{(pr)}) = \frac{\exp(z_t^{(pr)}\beta_i)}{\sum_{j=1}^N \exp(z_t^{(pr)}\beta_j)}
\end{align*}
$$

* Let the parameters for the baseline state (e.g.: State N) to be 0

This means when applying the EM algorithm, we need to find values $$\theta_{pr}$$ to maximize: &#x20;

$$
\begin{equation*}
\sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log P(S_t=i|\theta_{pr}, z_t^{(pr)})
\end{equation*}
$$
