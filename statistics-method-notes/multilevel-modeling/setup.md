# Bayesian Multivarate Regression with Regularization

In the Bayesian context, regularization is performed by applying appropriate prior distribution to the regression coefficients. Some of the common priors include

* Laplace prior&#x20;
* Spike-and-slab&#x20;
* Horseshoe and hierarchical shrinkage&#x20;

## [Example 1: Horshoe prior ](https://www.pymc.io/projects/docs/en/latest/learn/core\_notebooks/pymc\_overview.html)&#x20;

Traditionally, horse-shoe prior is defined as&#x20;

$$
\theta_j|\lambda_j, \tau \sim N(0, \lambda_j^2\tau^2) \\
\lambda_j\sim C^+(0,1)
$$

where $$\tau$$is the global shrinkage parameter and $$\lambda_j$$is the local shrinkage parameter.&#x20;

<pre class="language-python"><code class="lang-python"><strong># Data preparation 
</strong>import pytensor.tensor as at
import arviz as az
import pandas as pd

test_scores = pd.read_csv(pm.get_data("test_scores.csv"), index_col=0)
X = test_scores.dropna().astype(float)
y = X.pop("score")
# Standardization 
X -= X.mean()
X /= X.std()
N, D = X.shape
D0 = int(D / 2)
</code></pre>

The horseshoe prior for each regression coefficient $$\beta_i$$is&#x20;

$$
\begin{align*}
&\beta_i \sim N(0, \tau^2*\tilde{\lambda}_i^2) \\
&\tau\sim \text{Half-StudentT}_2(\frac{D_0}{D-D_0}*\frac{\sigma}{\sqrt{N}})\\
&\tilde{\lambda}_i^2=\frac{c^2\lambda_i^2}{c^2+\tau^2\lambda_i^2} \\
& \lambda_i \sim \text{Half-StudentT}_5(1) \\
& c^2 \sim \text{InverseGamma(1,1)}
\end{align*}
$$

* $$\sigma$$is the prior on the error standard deviation&#x20;
* $$\tau$$ follows a Half-StudentT distribution (just one option as a heavy-tailed distribution with non-zero probability
* $$D_0$$ represents the true number of non-zero coefficients. We guess it to be half of the overall parameters. This doesn't have to be a super good guess.&#x20;
* $$c^2$$ need to be strictly positive, but not necessarily long-tail. hence the inverse gamma prior.&#x20;

Finally, to make the sampling process more efficient in PyMC, we re-parameterize the coefficient as&#x20;

$$
z_i \sim N(0,1)\\
\beta_i = z_i * \tau * \tilde{\lambda}_i
$$

<pre class="language-python"><code class="lang-python">// Some code
with pm.Model(coords={"predictors": X.columns.values}) as test_score_model:
    sigma = pm.HalfNormal("sigma", 25)
    tau = pm.HalfStudentT("tau", 2, D0 / (D - D0) * sigma / np.sqrt(N))
    # Local shrinkage prior
    lam = pm.HalfStudentT("lam", 5, dims="predictors") # function default scale to 1
    c2 = pm.InverseGamma("c2", 1, 1)
    z = pm.Normal("z", 0.0, 1.0, dims="predictors")
    # Shrunken coefficients
    beta = pm.Deterministic(
        "beta", z * tau * lam * at.sqrt(c2 / (c2 + tau**2 * lam**2)), dims="predictors"
    ) # here we can pass dims="predictors" cause we specified the coordinates in pm.Model call
    # No shrinkage on intercept
    beta0 = pm.Normal("beta0", 100, 25.0)

    scores = pm.Normal("scores", beta0 + at.dot(X.values, beta), sigma, observed=y.values)
    
    
<strong>pm.model_to_graphviz(test_score_model) # check model graph 
</strong>
with test_score_model:# check whether sampled prior, specifically the scores, matches to reality 
    prior_samples = pm.sample_prior_predictive(100)

az.plot_dist(
    test_scores["score"].values,
    kind="hist",
    color="C1",
    hist_kwargs=dict(alpha=0.6),
    label="observed",
)
az.plot_dist(
    prior_samples.prior_predictive["scores"],
    kind="hist",
    hist_kwargs=dict(alpha=0.6),
    label="simulated",
)
plt.xticks(rotation=45);

# fitting model 
with test_score_model:
    idata = pm.sample(1000, tune=2000, random_seed=42)

# checking energy plot 
az.plot_energy(idata)

# checking trace plot 
az.plot_trace(idata, var_names=["tau", "sigma", "c2"])

# plot coefficient
az.plot_forest(idata, var_names=["beta"], combined=True, hdi_prob=0.95, r_hat=True)
</code></pre>





```
// Some code
basic_model = pm.Model()
with basic_model:
    # Priors for unknown model parameters
    alpha = pm.Normal("alpha", mu=0, sigma=10)
    beta = pm.Normal("beta", mu=0, sigma=10, shape=2)
    sigma = pm.HalfNormal("sigma", sigma=1)

    # Expected value of outcome
    mu = alpha + beta[0] * X1 + beta[1] * X2

    # Likelihood (sampling distribution) of observations
    Y_obs = pm.Normal("Y_obs", mu=mu, sigma=sigma, observed=Y)
```

*



