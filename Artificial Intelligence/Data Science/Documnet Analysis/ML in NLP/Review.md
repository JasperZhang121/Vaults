### Regression Model

- $y = \sigma(\mathbf{w}^\top \mathbf{x} + b)$

- $\sigma(z) = (1 + \exp(-z))^{-1}$

- $\mathbf{w} = [0.1, -0.2, 0.4]^\top$

If the data is: $[0.2, 0.5, -0.3]^\top$


```python
w = np.array([0.1,-0.2,0.4])
x = np.array([0.2,0.5,-0.3])
b = 0.6
z = np.dot(w,x)+b
y = pow((1+math.exp(-z)),-1)
print("predicted possibility: ", y)
```

### Pointwise mutual information 

|       | drive | run | fluffy | broken |
|-------|-------|-----|--------|--------|
| car   | 27    | 35   | 15      | 8     |
| bus   | 2    | 1   | 12      | 17     |
| cat   | 10     | 23  | 15     | 4      |
| dog   | 1     | 31  | 18     | 2      |
$$\text{PMI}(X, Y) = \log_2 \left( \frac{p(X, Y)}{p(X) \times p(Y)} \right)$$
### Purity of the clustering

$$Purity = \frac{1}{N} \sum_{k} \max_{j} |c_k \cap t_j|$$
Where:
- N is the total number of documents
- ck​ is the set of documents in cluster k
- tj​ is the set of documents of the true class j
- ∣ck ​∩ tj​∣ is the number of documents of class j in cluster k

