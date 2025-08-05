# Reliability

## Source

* [https://courses.lumenlearning.com/suny-hccc-research-methods/chapter/chapter-7-scale-reliability-and-validity/](https://courses.lumenlearning.com/suny-hccc-research-methods/chapter/chapter-7-scale-reliability-and-validity/)
*

## Primer:&#x20;

These notes briefly go over the validity and reliability concept within psychometrics, the common measures in the single & repeated measure paradigms. Validity refers to whether the scale measures the unobservable construct that we wanted to measure (i.e., accuracy). Reliability refers to whether the scale measures the intended construct consistently and precisely (i.e., precision).&#x20;



## Reliability Measures

### Inter-rater Reliability

A measure of consistency between two or more independent raters of the same construct. Here are some common options:&#x20;

#### Joint Probability of agreement&#x20;

Simplest approach for nominal rating. The percentage of options that two raters agree with each other.&#x20;

#### Correlation Coefficients

Simplest approach for ordinal / interval ratings, with Pearson's r, Kendall's T, or Spearman's rho.&#x20;

#### Kappa Statistics

Cohen's kappa (for two raters) & [Fleiss's kappa](https://en.wikipedia.org/wiki/Fleiss's_kappa) (for multiple raters) are more robust measures for nominal ratings.&#x20;

$$
\kappa=\frac{p_o-p_e}{1-p_e}=1-\frac{1-p_o}{1-p_e}
$$

where $$p_o$$ is the relative observed agreement among raters, and $$p_e$$ is the hypothetical probability of chance agreement. For $$k$$ categories with $$N$$ observations to categorize and $$n_{ki}$$ the number of times the rater $$i$$ predicted the category $$k$$, we then have $$p_e = \frac{1}{N^2}\sum_kn_{k1}n_{k2}$$. It is also possible to get the [estimate](https://en.wikipedia.org/wiki/Cohen's_kappa) from binary classification confusion matrix for a binary classification task.&#x20;

### Split-half reliability&#x20;

Split-half reliability is a measure of consistency between two halves of a construct measure. Is often just randomly splitting the inventory into two halves, then measuring the correlation. It is not suitable for inventory such as Big Five, where the intended construct is non-unidimensional.&#x20;

### Internal consistency reliability

Measuring consistency between different items of the same construct. One common measure is Cronbach's alpha.&#x20;

$$
\begin{align*}
& \alpha = \frac{k}{k-1}( 1 - \frac{\sum_{i=1}^k \sigma_{y_i}^2}{\sigma^2_y} )\\
& y=\sum_{i=1}^k y_i
\end{align*}
$$
