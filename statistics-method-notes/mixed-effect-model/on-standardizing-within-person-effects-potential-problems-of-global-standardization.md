---
description: By Lijuan Wang, Qian Zhang, Scott E Maxwell and C S Bergeman
---

# On Standardizing within-person effects: Potential problems of global standardization

[Annotated Paper](https://drive.google.com/file/d/12n3Evc8B9FUUqmaxR27d1sa_6RwceIBf/view?usp=drive_link)

## Abstract

Person-mean centering has been recommended for disaggregating between-person and within-person effects when modeling time-varying predictors. Multilevel modeling textbooks recommended global standardization for standardizing fixed effects. An aim of this study is to evaluate whether and when person-mean centering followed by global standardization can accurately estimate fixed-effects within-person relations (the estimand of interest in this study) in multilevel modeling. We analytically derived that global standardization generally yields inconsistent (asymptotically biased) estimates for the estimand when between-person differences in within-person standard deviations exist and the average within-person relation is nonzero. Alternatively, a person-mean-SD standardization (P-S) approach yields consistent estimates. Our simulation results further revealed (1) how misleading the results from global standardization were under various circumstances and (2) the P-S approach had accurate estimates and satisfactory coverage rates of fixed-effects within-person relations when the number of occasions is 30 or more (in many conditions, performance was satisfactory with 10 or 20 occasions). A daily diary data example, focused on emotional complexity, was used to empirically illustrate the approaches. Researchers should choose standardization approaches based on theoretical considerations and should clearly describe the purpose and procedure of standardization in research articles.

## Primer

Issues about raw units&#x20;

* The interpretation of a raw within-person coefficient depends on the scale of two time-varying variables (so not meaningful if the scale itself is not meaningful)
* Raw coefficients are generally not good effect size measures because they are not scale/unit invariant
* Raw coefficient usually is less useful for comparing the strengths of within-person relations across different pairs of time-varying variables.&#x20;

## Formulation&#x20;

### Set-up

Given variable $$X$$ and $$Y$$, the true within-person correlation is&#x20;

$$
\begin{align*}
\rho_{w,i}  &=\frac{E_i\left[(X_{it}-\mu_{X_i})(Y_{it}-\mu_{Y_i})|i\right]}{\sigma_{X_i}\sigma_{Y_i}} \\
\mu_{\rho w} &= E[\rho_{w,i}]
\end{align*}
$$

* $$\mu_{X_i} \& \mu_{Y_i}$$: Population within-person means
* $$\sigma_{X_i} \&  \sigma_{Y_i}$$: Individual within-person standard deviations
* $$\mu_{\rho w}$$: is the average within-person correlation, generally the true parameter of interests.&#x20;

### Consistent Estimators

Let $$r_{w,i} =\frac{\sum_t(x_{ti}-x_i)(y_{ti}-y_i)}{T*s_{xi}*s_{yi}}$$ be the person-specific correlation estimators using the sample mean and within-person sample standard deviation, then generally we have the following multilevel models:&#x20;

$$
\begin{align*}
r_{w,i} &= \rho_{w,i} + e_{it} \\
\rho_{w,i} &= \mu_{\rho w} + u_{1i}
\end{align*}
$$

So the estimator is a combination of true correlation and error, and true correlation is a combination of population mean and then person-specific variations.&#x20;

### Person-mean centering followed by global standardization formulation

The basic multilevel model (C1) is&#x20;

$$
\begin{align*}
y_{it} &= \gamma_{0i}^{C1} + \gamma_{1i}^{C1}(x_{it}-x_i) + e_{it}^{C1} \\
\gamma_{0i}^{C1} &= \gamma_{00}^{C1} + \gamma_{01}^{C1}x_i + u_{0i}^{C1} \\
\gamma_{1i}^{C1} &= \gamma_{10}^{C1} + u_{1i}^{C1}
\end{align*}
$$

* Without allowing variation to within-person effect $$u_{1i}^{C1}$$, the estimation of within-person effect yielded inflated Type 1 error (false positive).&#x20;
* Including the term when there are no between-person variations of the within-person effect, the model has a higher non-convergence rate.&#x20;
* The person-mean centering is also conducted for the outcome variable implicitly. We can see that if we move the $$\gamma_{0i}^{C1}$$ to the other side.&#x20;

A typical multilevel model with explicitly mean centering (C2): Notice the outcome variable is mean-centered with a sample instead of a latent estimate, and PC annotes as person-specific centering.&#x20;

$$
\begin{align*}
y_{it}^{PC} &= \gamma_{1i}^{C2}*x_{it}^{PC}+e_{it}^{C2} \\
\gamma_{1i}^{C2} &= \gamma_{10}^{C2} + u_{1i}^{C2}\\
y_{it}^{PC} &=y_{it} - y_i\\
x_{it}^{PC} &=x_{it} - y_i\\
\end{align*}
$$

* Empirically, the estimated within-person mean $$\gamma_{10}^{C2}$$ often has high reliability even when the number of time points is as low as 5 or 10.&#x20;

The estimators are then standardized using global standard deviations. We have

* G1 method: $$\hat{\gamma}_{10}^{G1} = \hat{\gamma}_{10}^{C1}*\frac{s_{cx}}{s_{cy}}$$ (using the person-mean centered IV and DVs)
* G2 method: $$\hat{\gamma}_{10}^{G2} = \hat{\gamma}_{10}^{C1}*\frac{s_{cx}}{s_{y}}$$(using the person-mean centered IV and raw DV sd)
* G3 method: $$\hat{\gamma}_{10}^{G3} = \hat{\gamma}_{10}^{C2}*\frac{s_{cx}}{s_{cy}}$$(using the person-mean centered IV and DVs with explicitly mean centering)&#x20;

The issue is that between-person difference (variance) in within-person variance is involved in all three estimators, so they are all inconsistent estimators of $$\mu_{\rho w}$$

* In G1 and G3, the estimator is a linear combination of the average with-person SD and variance in within-person SDs
* In G2, the estimator is a linear combination of the average of within-person SDs, variance in within-person SDs, and variance in within-person means

### Within-person standardization&#x20;

P-S method:  the standard mixed-effect formulation is:&#x20;

$$
\begin{align*}
y_{it}^{PS} &= \gamma_{1i}^{PS}x_{it}^{PS}+e_{it}^{PS} \\
\gamma_{1i}^{PS} &= \gamma_{10} + u_{1i}^{PS}\\
y_{it}^{PS} &= \frac{(y_{it} - y_i)}{s_{yi}}\\
s_{yi} &= \sqrt{\frac{\sum_t (y_{it}-y_i)^2}{T_i-1}}
\end{align*}
$$

* $$y_i$$ is the sample mean&#x20;
* $$s_{yi}$$ is the sample standard deviation

EB method: An alternative within-person standardization is to first estimate $$\gamma_{1i}^{C1}$$ with Bayesian regression, then standardize at the person level using the observed individual within-person standard deviation.&#x20;

## Derivation Result &#x20;

Mean: Let $$\mu_{Xi}=E[X_{it}|i]$$. The between-person population covariance matrix is&#x20;

$$
\begin{bmatrix}
\sigma_{\mu_X}^2 & \sigma_{\mu_X, \mu_Y}\\
\sigma_{\mu_X, \mu_Y} & \sigma_{\mu_Y}^2\\
\end{bmatrix}
$$

* $$\sigma^2_{\mu_X}$$ quantifies between-person differences in the person means of $$X$$
* $$\rho_b=\frac{\sigma_{\mu_X, \mu_Y}}{\sigma_{\mu_x}*\sigma_{\mu_y}}$$ quantifies the population between-person correlation between $$X$$ and $$Y$$

Variability: Let $$\sigma_{X_i}$$ be individual $$i$$'s population intraindividual standard deviation in $$X$$. Let $$E[\sigma_{X}]=\mu_{\sigma_x}$$. The covariance matrix of the person SD variable is&#x20;

$$
\begin{bmatrix} \sigma_{\sigma_X}^2 & \sigma_{\sigma_X, \sigma_Y} \\ \sigma_{\sigma_X, \sigma_Y} & \sigma_{\sigma_Y}^2\ \end{bmatrix}
$$

Findings:&#x20;

1. $$\rho_{X,Y}$$, the population correlation, is a weighted average of the b/t person correlation $$\rho_b$$ and the average w/i person correlation $$\mu_{\rho w}$$. So itreflects neither&#x20;
2. person-mean centering successfully removes the $$\rho_b$$ from the population correlation. But the correlation still not equal to $$\mu_{\rho w}$$ when it is not 0.&#x20;
3. Person-scaled population correlation is equal to $$\mu_{\rho w}$$
4. Global standardization (G1, G2) generally yields inconsistent estimates of the average within-person correlation $$\mu_{\rho w}$$ when it is not 0 and there are between-person differences in the w/i person standard deviations of one or both of the time-varying variables. &#x20;

## Simulation Study and Result&#x20;

1. if $$\mu_{\rho w}=0$$, regardless of whether there are between-person differences in persons SDs of $$X$$ and $$Y$$, the global scaling and person scaling approach all yield consistent results with good coverage rate.&#x20;
2. If $$\mu_{\rho w}=0.5$$, global scaling approach yields inconstent results. The EB method yield results with ignorable bias when $$T> 20$$, and became better when more time points in terms of bias and coverage rates .
3.  When $$\mu_{\rho w}\neq 0$$, the performance of the G1 and G2 method depends on whether there are between-person differences in person SDs.&#x20;





























## Other Minor points&#x20;

* Researcher have found that substantial between-person difference in within-person standard deviations existed in psychological and behavioral variables such as mood, motor performance, and stress.&#x20;
* Between-person differences in intraindividual variability (i.e., between-person diff in within-person sd) have been found to be predictive of important outcomes.&#x20;





















