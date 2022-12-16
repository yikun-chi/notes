---
description: https://www.coursera.org/learn/classification-vector-spaces-in-nlp
---

# Classification Methods and Vector Space

## Sentiment Analysis

### Bag of Word and Sparse Representation&#x20;

**Features:** Bag of Word approach to generate features that are 1 or 0 to indicate whether each word are in the specific training example.&#x20;

**Algorithm:** Logistic regression on the feature to classify as positive or negative sentiment&#x20;

**Problem:** Sparse features. Forces logistic regression to have large number of parameters.&#x20;

### Negative and Positive Frequency Representation&#x20;

**Features:**  For each word, identify its positive and negative frequency by counting how many times the word appear in positive cases and negative cases. The final feature for an example is the a vector of length three: a bias unit (normally 1), the sum of dedupped words’ positive frequency, and the sum of dedupped words’ negative frequency

**Algorithm:** Logistic regression on the features to classify as positive or negative sentiment.

### Naive Bayes Frequency Ratio Representation&#x20;

**Features:** For each word, derive its positive and negative frequency

$$
\begin{align*}
& p(w_i | pos) = \frac{freq(w_i,pos)}{freq(pos)}\\
& freq(w_i,pos): \text{Total count of word i in all positive text} \\
& freq(pos): \text{Total count of all words in all positive text} \\ 
\end{align*}
$$

**Algorithms:** Naive Bayes&#x20;

$$
\begin{equation*}
    \mathbb{I} \left(\frac{p(pos)}{p(neg)}\prod_{i=1}^n \frac{p(w_i|pos)}{p(w_i|neg)} \geq 1 \right) 
\end{equation*}
$$





**Laplacian Smoothing**: The aforementioned feature suffers the problem of multiplying / dividing by 0 if certain word only shows up in one class. Laplacian smoothing replaces the the conditional probability for word i with the following definition&#x20;

$$
\begin{align*}
& p(w_i | pos) = \frac{freq(w_i,pos)+1}{N_{pos} + V}\\
& N_{pos}: \text{Sum of word frequency in positive class}\\
& V: \text{Number of unique word in the entire text (both classes)} \\ 
\end{align*}
$$

**Log sum Trick**: Direct likelihood generally is a very small number and multiplying them risk of underflow. So we use log of the number to transform multiplication into addition for safer calculation.

$$
\begin{align*}
\log\left( \frac{p(pos)}{p(neg)}\prod_{i=1}^n \frac{p(w_i|pos)}{p(w_i|neg)} \right) = \log \frac{p(pos)}{p(neg)} + \left( \sum_{i=1}^n \log \frac{p(w_i|pos)}{p(w_i|neg)} \right)
\end{align*}
$$

Notice now the threshold for indicator function becomes 0 instead of 1. &#x20;

## Vector Space Model&#x20;

Skipped Content: Adding and subtracting word embeddings, Euclidian distance, cosine similarity&#x20;

### Word to Word Design&#x20;

A matrix with words on the rows and columns. Cell value are the number of times two word (row & col) co-occurance within $$k$$ distance away from each other.&#x20;

### Word to Doc Design&#x20;

A matrix with words on the rows and document types as columns. The cell value are the number of times certain words appear in certain genre of documents.

### PCA

1. Deriving uncorrelated features through eigenvector&#x20;
   1. Mean Normalize data&#x20;
   2. Get covariance matrix&#x20;
   3. Perform SVD to get eigenvector matrix (first matrix) and eigenvalue (diagonal value of second matrix)&#x20;
   4. Eigenvector should be organized according to descending eigenvalue&#x20;
2. Dot product to project data matrix $$X$$to the first $$k$$column of eigenvector matrix $$U$$through $$X' = XU[:k]$$
3. Calculate percentage of variance explained via $$\frac{\sum_{i=1}^{d} S_{ii}}{\sum_{j=1}^k S_{jj}}$$

[PCA Deepdive](../../miscellaneous/svd-and-pca-deep-dive.md)

## Machine Translation with K nearest Neighbor and Locality Sensitive Hashing&#x20;

### Machine Translation Key Ida

Let $$X$$ be a matrix $$m × n$$ with each row as an English word, let $$Y$$ be a matrix with each row as the corresponding French word. The translation matrix $$R$$ is the matrix that minimizes the squared Frobenius norm of the difference: $$\lVert XR - Y \rVert ^2_F$$

$$R$$ can be obtained by iterative update via gradient:&#x20;

$$
\begin{align*}
        & R = R - \alpha * g \\
        & g = \frac{2}{m}(X^T(XR-Y))
    \end{align*}
$$



### Locality Sensitive Hashing

We can define $$n$$ hyperplanes with normal vector matrix $$V$$. Each word can be represented with $$w_i=0$$ or $$w_i = 1$$as below or above the hyperlane $$i$$. The final has value of each word can be calcualted as $$\sum_{i=0}^n 2^i * w_i$$

So overall, we can first determine the number of hash bucket $$h$$ (i.e.: the hash value), then generate number of hyperplanes $$n$$ based on the determined number of bucket $$2^n = h$$. For each set of hyperplanes, if two items have the same has value, they are in the same bucket. We can repeat this process for many times to improve accuracy.

### Approximate K-nearest Neighborhood&#x20;

Use mutliple sets of hyperplanes to determine final set of vectors that are potential candidate of nearest neighbors.&#x20;

Code Example&#x20;

{% code overflow="wrap" %}
```python
// Some code
num_dimensions = 2
num_planes = 3
random_panes_matrix = np.random.normal(size = (num_palnes, num_dimensions ))
def side_of_plane_matrix(P,v): 
    dotproduct = np.dot(P,v.T)
    sign_of_dot_product = np.sign(dotproduct)
    return sign_of_dot_product 
    
num_planes_matrix = side_of_plane_matrix(random_panes_matrix , v) 
```
{% endcode %}

## Code Example

### Visualization - Confidence Ellipse ([Link](https://matplotlib.org/stable/gallery/statistics/confidence\_ellipse.html))

{% code overflow="wrap" %}
```python
confidence_ellipse(data_pos.positive, data_pos.negative, ax, n_std=2, edgecolor='black', label=r'$2\sigma$' )
# data_pos.positive is a sequence of x-axis coordinate
```
{% endcode %}

## Completed Notebook&#x20;

[Logistic Regression Sentiment Analysis Notebook](https://drive.google.com/file/d/1VjBPjnc84eIcTmzQi-KuPAQk1LbJKl93/view?usp=sharing)

* Stemming, Removing Stopwords
* Manual Implementation of Logistic Regression and lossderivation

[Naive Bayes Sentiment Analysis Notebook](https://drive.google.com/file/d/10B7u3PMDpUxS3i6jWMwQK5OBupr800Ar/view?usp=sharing)

* Naive Bayes Sentiment Analysis

[Vector Space Model Notebook](https://drive.google.com/file/d/16qyWv2DkFhi2hK2U0YPvJ8cKGvTjL3Dr/view?usp=share\_link)

* Manual PCA

[Machine Translation and Nearest Neighbor Search Notebook](https://drive.google.com/file/d/1iHHK93QHkHoNfAtuktrJW6vbWmEN7pT3/view?usp=share\_link)

* LSH and approximate neighborhood
* Manual Gradient Descent









