# Distribution Testing

## Testing for Normality&#x20;

### Shapiro-Wilk test

Razali, N. and Wah, Y. (2011) [Power Comparisons of Shapiro-Wilk, Kolmogorov-Smirnov, Lilliefors and Anderson-Darling tests](https://www.nrc.gov/docs/ML1714/ML17143A100.pdf). Journal of Statistical Modeling and Analytics, 2, 21-33.

#### Mathematical Formulation&#x20;

$$
\begin{equation*}
W = \frac{(\sum_{i=1}^n \alpha_i y_{(i)})^2}{\sum_{i=1}^n (y_{(i)} - \bar{y})^2}
\end{equation*}
$$

* $$y_{(i)}$$ is the $$i$$ the order statistics&#x20;
* $$\alpha$$ vector is calculated as $$\frac{m^TV^{-1}}{(m^T V^{-1} V^{-1} m)^{1/2}}$$
* $$m$$ is the expected values of the order statistics&#x20;
* $$V$$ is the covariance matrix of the order statistics&#x20;

#### Note&#x20;

* Has a bias by sample size. The larger the sample, the morel likely to get a statistically significant result. AS R94 version of the test can be used for $$n$$ in the range of 3 to 5000
* Shapiro-Wilk generally has better power for a given significance
