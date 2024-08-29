# Principal Components Analysis

Content from:

* &#x20;[Jonathan Hui medium post](https://jonathan-hui.medium.com/machine-learning-singular-value-decomposition-svd-principal-component-analysis-pca-1d45e885e491)&#x20;
* [Coursera workbook of the NLP module](broken-reference)
* [Stanford CS 233 PCA module](https://drive.google.com/file/d/1b7HLvYZ\_cF8q2LGwpvDhR\_SNI-FQVcsT/view?usp=drive\_link)&#x20;
* [Lecture Notes on PCA by Laurenz Wiskott](https://drive.google.com/file/d/1aVzEZjn-POBPfD6VmwJvQVQGNu5\_kwGj/view?usp=drive\_link)&#x20;

## PCA Intuition&#x20;

PCA looks for a single lower-dimensional linear subspace that captures most of the variation in the data. In other words, PCA aims to minimize the error introduced by projecting the data into this linear subspace.&#x20;

## PCA Flow&#x20;

The overflow of PCA involves rotating the data to obtain uncorrelated features and then performing dimensionality reduction through projection.&#x20;

* Eigenvectors give the direction of uncorrelated features&#x20;
* Eigenvalues are the variance of the new features&#x20;
* Dot product gives the projection of uncorrelated features&#x20;

1. Deriving uncorrelated features through eigenvector&#x20;
   1. Mean Normalize data&#x20;
   2. Get covariance matrix&#x20;
   3. Perform SVD to get the eigenvector matrix (first matrix) and eigenvalue (diagonal value of second matrix)&#x20;
   4. The eigenvector should be organized according to descending eigenvalue&#x20;
2. Dot product to project data matrix $$X$$to the first $$k$$column of eigenvector matrix $$U$$through $$X' = XU[:k]$$
3. Calculate the percentage of variance explained via $$\frac{\sum_{i=0}^{k} S_{ii}}{\sum_{j=0}^d S_{jj}}$$

## [A Simple 2D Example](svd-and-pca-deep-dive.md)

A simple 2D PCA walkthrough motivating variance minimization and PCA as rotation problem

## PCA as a Linear Combination

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

&#x20;

