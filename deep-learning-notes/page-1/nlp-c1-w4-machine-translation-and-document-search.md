---
description: https://www.coursera.org/learn/classification-vector-spaces-in-nlp/home/week/4
---

# NLP-C1-W4: Machine Translation and Document Search

## Machine Translation Key Ida

Let $$X$$ be a matrix $$m × n$$ with each row as an English word, let $$Y$$ be a matrix with each row as the corresponding French word. The translation matrix $$R$$ is the matrix that minimizes the squared Frobenius norm of the difference: $$\lVert XR - Y \rVert ^2_F$$

$$R$$ can be obtained by iterative update via gradient:&#x20;

$$
\begin{align*}
        & Loss = \lVert XR - Y \rVert_F \\
        & g - \frac{d}{dR}Loss = \frac{2}{m}(X^T(XR-Y)) \\
        & R = R - \alpha * g \\
    \end{align*}
$$

## Using K-nearest Neighborhood for Translation&#x20;

Once we have the matrix, we can transform an English word into a French word vector space. However, we need to use the K-nearest neighbor search to identify exactly which word to translate.&#x20;

### Locality Sensitive Hashing for Faster Search&#x20;

We can define $$n$$ hyperplanes with a normal vector matrix $$V$$. Each word can be represented with $$w_i=0$$ or $$w_i = 1$$as below or above the hyperlane $$i$$. The final has value of each word can be calculated as $$\sum_{i=0}^n 2^i * w_i$$

So, overall, we can first determine the number of hash buckets $$h$$. This can be decided by considering how many items we want in each bucket and dividing the total number of items by the desired item count per bucket. Then,  generate  $$n$$  hyperplanes based on the determined number of bucket $$2^n = h$$. For each set of hyperplanes, if two items have the same hash value, they are in the same bucket. We can repeat this process many times to improve accuracy.

### Approximate K-nearest Neighborhood&#x20;

Use multiple sets of hyperplanes to determine the final set of vectors that are potential candidates of nearest neighbors.&#x20;

Flow:&#x20;

1. Generate multiple hash tables
2. Given a target vector, hash it, and grab all items that are in the hash tables across all hash tables.&#x20;
3. Run the nearest neighbor search on the reduced candidate list.&#x20;

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
# then use \sum_{i=0}^n 2^i * w_i to calculate the hash value 
```
{% endcode %}



## Completed Notebook&#x20;

[Vector Space Model Notebook](https://drive.google.com/file/d/16qyWv2DkFhi2hK2U0YPvJ8cKGvTjL3Dr/view?usp=share\_link)

* Manual PCA

[Machine Translation and Nearest Neighbor Search Notebook](https://drive.google.com/file/d/1iHHK93QHkHoNfAtuktrJW6vbWmEN7pT3/view?usp=share\_link)

* LSH and approximate neighborhood
* Manual Gradient Descent
