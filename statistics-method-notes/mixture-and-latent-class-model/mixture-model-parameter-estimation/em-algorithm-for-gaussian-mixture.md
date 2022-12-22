---
description: A Detailed Example of EM Algorithm for Mixture model
---

# EM Algorithm for Gaussian Mixture

Given a mixture model with all Gaussian mixture, each of the observation density is defined as:&#x20;

$$
\begin{equation*}
f_i(y_t) = \frac{1}{\sqrt{2\pi}\sigma_i} \exp \left( - \frac{(y_t - \mu_i)^2}{2\sigma_i^2} \right)
\end{equation*}
$$

The parameters $$\theta_{pr}=(\pi_1, ..., \pi_N)$$ are the mixing probabilities and $$\theta_{obs} = (\mu_1, ..., \sigma_N)$$ are the parameters for normal density function.&#x20;

The [expected complete-data log-likelihood](./#em-in-mixture-model) now can be defined as:&#x20;

$$
\begin{align*}
Q(\theta, \theta') 
& = \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i) \log P(s_t = i|\theta_{pr}) + \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log f(y_t | s_t = i, \theta_{obs}) \\
& = \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i) \log \pi_i + \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i)\log \frac{1}{\sqrt{2\pi}\sigma_i} \exp\left(-\frac{(y_t-\mu_i)^2}{2\sigma_i^2}\right)\\
& = \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i) \log \pi_i + \sum_{t=1}^T \sum_{i=1}^N \gamma_t(i) \left[\log(\frac{1}{\sqrt{2\pi}\sigma_i}) -  \frac{(y_t-\mu_i)^2}{2\sigma_i^2} \right]\\
\gamma_t(i) & = p(s_t=i|y_t, \theta')
\end{align*}
$$

Recall during the maximization step we need to find the parameter that maximizes the expected complete-data log likelihood. So we take partial derivative and set it to 0

$$
\begin{align*}
& 0 = \frac{\partial Q}{\partial \mu_i} = \sum_{t=1}^T \gamma_t(i) * \frac{(y_t - \mu_i)}{\sigma_i^2} \\
& 0 = \frac{\partial Q}{\partial \sigma_i} = \sum_{t=1}^T \gamma_t(i)\left(-\frac{1}{\sigma_i} + \frac{(y_t - \mu_i)^2}{\sigma_i^3} \right)
\end{align*}
$$

For the mixing probability the process is a bit more involved. We can re-express the component probabilities in multinomial logit form and use the fact that mixing probabilities sums to 1:&#x20;

$$
\begin{align*}
& \pi_i = \frac{\exp(k_i)}{\sum_{j=1}^N \exp(k_j)} \\
& \frac{\partial \pi_i}{\partial k_j} = \begin{cases} \pi_i - \pi_i^2 & i =j \\ -\pi_i\pi_j & i \neq j \\
\end{cases}\\
& \frac{\partial Q}{\partial{\pi_i}} = \sum_{t=1}^T  \frac{\gamma_t(i)}{\pi_i} \\
& \frac{\partial Q}{\partial{k_j}} = \sum_{i=1}^N \frac{\partial Q}{\partial{\pi_i}} * \frac{\partial \pi_i}{\partial k_j} = \sum_{t=1}^T (\gamma_t(j) - \pi_j)
\end{align*}
$$

Finally we can solve for all of them and obtain the following estimator:&#x20;

$$
\begin{align*}
& \hat{\mu}_i = \frac{\sum_{t=1}^T \gamma_t(i) y_t}{\sum_{t=1}^T \gamma_t(i)} \\
& \hat{\sigma}_i =  \sqrt{\frac{\sum_{t=1}^T \gamma_t(i)( y_t - \hat{\mu}_i)^2}{\sum_{t=1}^T \gamma_t(i)}} \\
& \hat{\pi}_i = \frac{1}{T} \sum_{t=1}^T \gamma_t(i)
\end{align*}
$$

```r
EM_GaussMix2 <- function(par, y) {
    # read the parameter 
    ps <- par[1:2]; means <- par[3:4]; sds <- par[5:6]
    # compute a matrix with component density values (likelihood) 
    B <- cbind(dnorm(y,mean=means[1],sd=sds[1]),
                dnorm(y,mean=means[2],sd=sds[2]))
    # Log likelihood to check convergence 
    prevLL <- sum(log(colSums(apply(B,1,"*",ps))))
    converge <- FALSE; i <- 1
    while(!converge) {
        # compute posterior state probabilities 
        gamma <- t(apply(B,1,"*",ps))
        gamma <- gamma/rowSums(gamma)

        # parameter estimates
        for (j in 1:2) {
            means[j] <- sum(gamma[,j]*y)/sum(gamma[,j])
            sds[j] <- sqrt(sum(gamma[,j]*(y-means[j])^2)/ sum(gamma[,j]))
        }

        ps <- colMeans(gamma)

        # component density values with new parameters
        B <- cbind(dnorm(y,mean=means[1],sd=sds[1]), 
                    dnorm(y,mean=means[2],sd=sds[2]))
        
        # Compute Log-likelihood and check for convergence 
        LL <- sum(log(colSums(apply(B,1,"*",ps))))
        if(LL - prevLL < 10e-8) converge <- TRUE
        i <-i+1; prevLL <- LL
    }
    return(list(par = c(p1=ps[1], m1=means[1], m2=means[2], s1=sds[1], s2=sds[2]), niter = i, logL = LL))
}
```
