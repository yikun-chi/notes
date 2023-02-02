# Fisher Information

## Score

The score is defined as the partial derivative of the likelihood function with respect to the parameters&#x20;

$$
\begin{equation*}
\frac{\partial}{\partial \theta} \log f(X; \theta)
\end{equation*}
$$

Under certain regularity conditions, we can show that the expectation of score evaluated at true parameters equals to 0.&#x20;

## Fisher Information&#x20;

Fisher information is defined to to be the variance of the score

$$
\begin{equation*}
I(\theta) := Var_\theta\left(\frac{\partial}{\partial \theta} \log f(X|\theta) \right)  = -E_\theta\left[\frac{\partial^2}{\partial \theta^2} \log f(X|\theta) \right]
\end{equation*}
$$



In matrix form, Fisher information matrix for $$k$$ parameters is a $$k \times k$$ matrix where the $$i, j$$ entry of the matrix is&#x20;

$$
\begin{equation*}
I(\theta)_{ij} := Cov_\theta\left(\frac{\partial}{\partial \theta_i} \log f(X|\theta), \frac{\partial}{\partial \theta_j} \log f(X|\theta) \right)  = -E_\theta\left[\frac{\partial^2}{\partial \theta_i \partial \theta_j} \log f(X|\theta) \right]
\end{equation*}
$$





