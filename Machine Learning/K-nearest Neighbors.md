In [statistics](https://en.wikipedia.org/wiki/Statistics "Statistics"), the **_k_-nearest neighbors algorithm** (**_k_-NN**) is a [non-parametric](https://en.wikipedia.org/wiki/Non-parametric_statistics "Non-parametric statistics") [supervised learning](https://en.wikipedia.org/wiki/Supervised_learning "Supervised learning") method first developed by [Evelyn Fix](https://en.wikipedia.org/wiki/Evelyn_Fix "Evelyn Fix") and [Joseph Hodges](https://en.wikipedia.org/wiki/Joseph_Lawson_Hodges_Jr. "Joseph Lawson Hodges Jr.") in 1951, and later expanded by [Thomas Cover](https://en.wikipedia.org/wiki/Thomas_M._Cover "Thomas M. Cover"). It is used for [classification](https://en.wikipedia.org/wiki/Statistical_classification "Statistical classification") and [regression](https://en.wikipedia.org/wiki/Regression_analysis "Regression analysis"). In both cases, the input consists of the _k_ closest training examples in a [data set](https://en.wikipedia.org/wiki/Data_set "Data set").

---

The training examples are vectors in a multidimensional feature space, each with a class label. The training phase of the algorithm consists only of storing the [feature vectors](https://en.wikipedia.org/wiki/Feature_vector "Feature vector") and class labels of the training samples.

In the classification phase, _k_ is a user-defined constant, and an unlabeled vector (a query or test point) is classified by assigning the label which is most frequent among the _k_ training samples nearest to that query point.

A commonly used distance metric for [continuous variables](https://en.wikipedia.org/wiki/Continuous_variable "Continuous variable") is [Euclidean distance](https://en.wikipedia.org/wiki/Euclidean_distance "Euclidean distance"). For discrete variables, such as for text classification, another metric can be used, such as the **overlap metric** (or [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance "Hamming distance")). In the context of gene expression microarray data, for example, _k_-NN has been employed with correlation coefficients, such as Pearson and Spearman, as a metric. Often, the classification accuracy of _k_-NN can be improved significantly if the distance metric is learned with specialized algorithms such as [Large Margin Nearest Neighbor](https://en.wikipedia.org/wiki/Large_Margin_Nearest_Neighbor "Large Margin Nearest Neighbor") or [Neighbourhood components analysis](https://en.wikipedia.org/wiki/Neighbourhood_components_analysis "Neighbourhood components analysis").

A drawback of the basic "majority voting" classification occurs when the class distribution is skewed. That is, examples of a more frequent class tend to dominate the prediction of the new example, because they tend to be common among the _k_ nearest neighbors due to their large number. One way to overcome this problem is to weight the classification, taking into account the distance from the test point to each of its _k_ nearest neighbors. The class (or value, in regression problems) of each of the _k_ nearest points is multiplied by a weight proportional to the inverse of the distance from that point to the test point. Another way to overcome skew is by abstraction in data representation. For example, in a [self-organizing map](https://en.wikipedia.org/wiki/Self-organizing_map "Self-organizing map") (SOM), each node is a representative (a center) of a cluster of similar points, regardless of their density in the original training data. _K_-NN can then be applied to the SOM.

----
Example

```python
X = [[0], [1], [2], [3]]
y = [0, 0, 1, 1]
from sklearn.neighbors import KNeighborsClassifier
neigh = KNeighborsClassifier(n_neighbors=3)
neigh.fit(X, y)
neigh.predict([[1.1]])
```

