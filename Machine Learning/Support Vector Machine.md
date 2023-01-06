In [machine learning](https://en.wikipedia.org/wiki/Machine_learning "Machine learning"), **support vector machines** (**SVMs**, also **support vector networks**[[1]](https://en.wikipedia.org/wiki/Support_vector_machine#cite_note-CorinnaCortes-1)) are [supervised learning](https://en.wikipedia.org/wiki/Supervised_learning "Supervised learning") models with associated learning [algorithms](https://en.wikipedia.org/wiki/Algorithm "Algorithm") that analyze data for [classification](https://en.wikipedia.org/wiki/Statistical_classification "Statistical classification") and [regression analysis](https://en.wikipedia.org/wiki/Regression_analysis "Regression analysis").

---

Any [hyperplane](https://en.wikipedia.org/wiki/Hyperplane "Hyperplane") can be written as the set of points x satisfying:  W<sup>T</sup>-b = 0

If the training data is [linearly separable](https://en.wikipedia.org/wiki/Linearly_separable "Linearly separable"), we can select two parallel hyperplanes that separate the two classes of data, so that the distance between them is as large as possible. The region bounded by these two hyperplanes is called the "margin", and the maximum-margin hyperplane is the hyperplane that lies halfway between them. With a normalized or standardized dataset, these hyperplanes can be described by the equations:
- W<sup>T</sup>-b = 1 (anything on or above this boundary is of one class, with label 1)
- W<sup>T</sup>-b = -1 (anything on or below this boundary is of the other class, with label −1)

<br/>

Margin = 2/ ||W||
- Hard-margin
- Soft-margin




