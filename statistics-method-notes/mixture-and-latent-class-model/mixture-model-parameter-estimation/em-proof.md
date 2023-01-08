# EM Proof

The proof relies on [KL divergence](../../miscellaneous-in-stat-linear-algebra-and-deep-learning/measuring-difference-in-probability-distributions.md#kullback-leibler-kl-divergence)

Given parameters $$\theta$$ (arbitrary parameter) and $$\theta'$$ (previous guess),  and observation, we can define two posterior state sequence (states of the sequence of observation) distributions:&#x20;

$$
\begin{align*}
P_1(s_{1:T}) &= \frac{f(s_{1:T}, y_{1:T}|\theta')}{f(y_{1:T}|\theta')} \\
P_2(s_{1:T}) &= \frac{f(s_{1:T}, y_{1:T}|\theta)}{f(y_{1:T}|\theta)}
\end{align*}
$$

Now recall the expected complete-data log-likelihood and apply the definition of posterior sequence probability above

$$
\begin{align*}
Q(\theta, \theta') 
& \equiv E_{s_{1:T}|y_{1:T}, \theta'}\left[ \log f(y_{1:T}, s_{1:T}|\theta)\right] \\
& = \sum_{s_{1:T}\in S^T} P_1(s_{1:T}) * \log f(y_{1:T}, s_{1:T}|\theta)\\
& = \frac{1}{f(y_{1:T}|\theta')} * \sum_{s_{1:T}\in S^T} f(s_{1:T}, y_{1:T}|\theta') * \log f(y_{1:T}, s_{1:T}|\theta)\\
\end{align*}
$$

So the KL divergence of two posterior probabilities are&#x20;

$$
\begin{align*}
D_{KL}(P_1||P_2) 
& = \sum_{s_{1:T}\in S^T} \frac{f(s_{1:T},y_{1:T}|\theta')}{f(y_{1:T}|\theta')} 
\log \left( \frac{f(s_{1:T},y_{1:T}|\theta')f(y_{1:T}|\theta)}{f(s_{1:T},y_{1:T}|\theta)f(y_{1:T}|\theta')} \right) \\
& = \log \frac{f(y_{1:T}|\theta)}{f(y_{1:T}|\theta')} + \sum_{s_{1:T}\in S^T} \frac{f(s_{1:T},y_{1:T}|\theta')}{f(y_{1:T}|\theta')} 
\log \left( \frac{f(s_{1:T},y_{1:T}|\theta')}{f(s_{1:T},y_{1:T}|\theta)} \right) \\
& = \log \frac{f(y_{1:T}|\theta)}{f(y_{1:T}|\theta')} + Q(\theta', \theta') - Q(\theta, \theta') \\
\end{align*}
$$

Since KL divergence is greater than 0, we have&#x20;

$$
\begin{align*}
\log f(y_{1:T}|\theta) - \log f(y_{1:T}|\theta') \geq Q(\theta, \theta') - Q(\theta', \theta')
\end{align*}
$$

&#x20;This implies that if the arbitrary parameter $$\theta$$ is chosen such that $$Q(\theta, \theta')\geq Q(\theta', \theta')$$, we will increase the log likelihood of the data. At maximization step, $$\theta$$ is chosen by maximizing $$Q(\theta, \theta')$$, so the greater or equal criteria is satisfied by definition and thus guarantee log likelihood to be increasing (hence local maximum).&#x20;

Note that technically, the maximization doesn't need to pick $$\theta$$ to maximize $$Q(\theta, \theta')$$. As long as the new $$\theta$$ satisfies the greater inequality, we can gurantee log likelihood to be always increasing.This version is called generalized EM algorithm &#x20;















