# Measuring Difference in Probability Distributions

## Kullback-Leibler (KL) divergence&#x20;

$$
\begin{equation*}
D_{KL}(P||Q)=\sum_{s\in S}p_p(s)\log\left(\frac{p_p(s)}{p_q(s)}\right) = E_{s\sim P}\left[\log\frac{p(x)}{q(x)}\right]
\end{equation*}
$$

Key properties&#x20;

* non-symmetric&#x20;
*   $$D_{KL}(P||Q)\geq 0$$, and only is 0 when two distributions are the same&#x20;



