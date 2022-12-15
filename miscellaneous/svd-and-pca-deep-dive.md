# SVD and PCA Deep Dive

Content from: [Jonathan Hui medium post](https://jonathan-hui.medium.com/machine-learning-singular-value-decomposition-svd-principal-component-analysis-pca-1d45e885e491) and Coursera workbook of the NLP module

## PCA as Linear Combination

### SVD

From SVD decomposition we know that any matrix $$A_{m \times n}$$can be factorized: $$A = U_{m\times m}S_{m\times n}V_{n\times n}^T$$ where $$U$$ and $$V$$ are orthogonal matrices with orthonormal eigenvectors chosen from $$AA^T$$ and $$A^TA$$and $$S$$is a diagonal matrix with $$r$$ elements equal to the root of the positive eigenvalues of $$AA^T$$and $$A^TA$$( They have the same positive eigenvalues).&#x20;

### SVD to linear combination of outer product&#x20;

We can rewrite the SVD equations as $$AV = US$$, and then apply the [column view](https://ghenshaw-work.medium.com/3-ways-to-understand-matrix-multiplication-fe8a007d7b26) of matrix multiplication and the fact that $$S$$ is a diagonal matrix, we have:&#x20;

$$
\begin{align*}
& Av_i = \sigma_i u_i \\
& \Longrightarrow A e_i = \sigma_i u_i v_i^T \\
& A_{\text{column i}} = \sigma_i u_i v_i^T
\end{align*}
$$

This means that we can think of matrix $$A$$ as a series of outer product of $$u_i$$and $$v_i$$

$$
A = \sigma_1u_1v_1^T + ...+\sigma_ru_rv_r^T
$$

### Covariance Matrix and SVD

Now in terms of covariance matrix, if $$X$$ is already centered, it can be written as&#x20;

$$
\Sigma=E[(X-\bar{X})(X-\bar{X})]=\frac{XX^T}{n}
$$

We can subsequently do SVD on this sample covariance matrix $$\Sigma = \sigma_1 u_1 v_1^T + ...$$. If the singular value $$\sigma_i$$ is ordered from largest to smallest, this means the impact of this linear combination is also ordered that way.&#x20;

Furthermore

* The total variance of the data equals to the trace of the matrix $$\Sigma$$ which equals to the sum of squares of $$\Sigma$$'s singular values. So we can get ratio of variance lost if we drop smaller singular values&#x20;
* The first eigenvector $$u_1$$of $$\Sigma$$ points to the most important direction of the data
* The error, calculated as the sum of the perpendicular squared distance from each point to $$u_1$$is minimum when SVD is used.

If $$Z=AX$$, then the covariance of $$Z$$ can be calculated as $$V_z = AV_xA^T$$ where $$V_x$$is the covariance of $$X$$

Given the SVD decomposition, apply $$A$$ to a vector $$x$$ can be understood as first performing a rotation $$V^T$$, then a scaling $$S$$, and finally another rotation $$U$$.&#x20;

## Alternative Perspective: Rotation ([Link](https://drive.google.com/file/d/1M7jtWcFcK-eakfO47cCssHSuuiZyS9TK/view?usp=sharing}))

First consider the extreme case of $$y=n x, x\sim \text{Unif}(0,1)$$. We create both variables and mean center it, then fed into a PCA to get 2 principle components. We plot the original data in blue and PCA transformed data (based on the rotation matrix of PCA) in orange.&#x20;

<figure><img src="../.gitbook/assets/y=nx PCA Demo.png" alt=""><figcaption><p>y=nx PCA Demo</p></figcaption></figure>

&#x20;We can see that there is only one direction (horizontal) post transformation, which is the original unfirom sampling on x axis.&#x20;

If we look at the rotation matrix, it is equal to $$R=\begin{bmatrix} \cos(45^\circ) & \sin(45^\circ) \\ -\sin(45^\circ) & \cos(45^\circ) \end{bmatrix}$$, which is a $$45^\circ$$ rotation. The explained variance is $$(0.166,0)$$. So the first principle component has all the explained variance since the original variance is $$Var(x) + Var(y)=0.1666$$ based on uniform sampling.&#x20;

In a more complicated example, we can generate $$X$$and $$Y$$ as two independent normal variables with different varianec, and then apply a rotation matrix to make them correlated. We can then attempt to decouple this through principle component. Here blue is the original post rotation data and organge is the recovered data.&#x20;

<figure><img src="../.gitbook/assets/2 var PCA Demo.png" alt=""><figcaption></figcaption></figure>



&#x20;

