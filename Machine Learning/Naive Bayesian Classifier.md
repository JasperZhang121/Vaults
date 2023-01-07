
In [statistics](https://en.wikipedia.org/wiki/Statistics "Statistics"), **naive Bayes classifiers** are a family of simple "[probabilistic classifiers](https://en.wikipedia.org/wiki/Probabilistic_classification "Probabilistic classification")" based on applying [Bayes' theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem "Bayes' theorem") with strong (naive) [independence](https://en.wikipedia.org/wiki/Statistical_independence "Statistical independence") assumptions between the features (see [Bayes classifier](https://en.wikipedia.org/wiki/Bayes_classifier "Bayes classifier")). They are among the simplest [Bayesian network](https://en.wikipedia.org/wiki/Bayesian_network "Bayesian network") models,[[1]](https://en.wikipedia.org/wiki/Naive_Bayes_classifier#cite_note-1) but coupled with [kernel density estimation](https://en.wikipedia.org/wiki/Kernel_density_estimation "Kernel density estimation"), they can achieve high accuracy levels.

---
Bayes' theorem:

![[Pasted image 20230107203540.png]]

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.5, random_state=0)
gnb = GaussianNB()
y_pred = gnb.fit(X_train, y_train).predict(X_test)
```

