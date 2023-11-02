### Querying

| |quick | brown | fox|
|---|---|---|---|
|Doc1| 3 |0 |2|
|Doc2| 0 |1 |1 |
|Doc3| 0| 3| 6|

#### Calculate the tf-idf score

$$\text{IDF} = \log\left(\frac{\text{total number of documents}}{\text{number of documents containing the term}}\right)$$
$$\text{TF-IDF}(term, document) = \text{TF}(term, document) \times \text{IDF}(term)
$$

| |quick | brown | fox|
|---|---|---|---|
|df| 1 |2 |3|
|idf| log3 |log3/2 |log3/3=0|

Res:

| |quick | brown | fox|
|---|---|---|---|
|Doc1| 3log3 |0 |0|
|Doc2| 0 |log3/2 |0 |
|Doc3| 0| 3log3/2| 0|


#### Cosine similarity 
##### Suppose the query "quick fox"

- term frequency = [1,0,1]
- idf:

|quick | brown | fox|
|---|---|---|
| log3 |log3/2 |0|

- tf * idf:

| |quick | brown | fox|
|---|---|---|---|
|query| log3 |0 |0|

$$cosine_similarity(A, B) = \frac{A \cdot B}{\|A\| \cdot \|B\|}
$$
<<<<<<< HEAD
=======

>>>>>>> origin/main
- sim(query,Doc1) = sim([log 3, 0, 0], [3 log 3, 0, 0]) = $\text{sim}(query, Doc1) = \frac{3 \cdot (\log 3)^2}{\log 3 \cdot 3 \cdot \log 3} = 1$
- sim(query, Doc2) = sim([log 3, 0, 0], [0, log 3/2 , 0]) = 0
- sim(query, Doc3) = sim([log 3, 0, 0], [0, 3 log 3 2 , 0]) = 0


```python
tf = np.array([
    [3,0,2],
    [0,1,1],
    [0,3,6],
])
q = np.array([1,0,1])

idf = [math.log(3),math.log(3/2),0]

tf_idf = tf*idf
tf_idf_q= q*idf
res = []

for x in tf_idf:
    sim = 1 - cosine(x,tf_idf_q)
    res.append(sim)
print(res)
```
---
### Evaluation
Suppose that we are evaluating our IR system. For a given query, our system retrieves 10 documents, which are marked as being relevant (R) or irrelevant (I) in the following order: R, I, R, R, I, R, R, R, R, R The list is ordered left to right, so the leftmost R is the relevance of the first retrieved document. There are 12 relevant documents in the entire collection.
#### Recall at 5 documents retrieved 
- 3/12 = 0.25

#### Interpolated precision
The list of (precision, recall) after k, k ∈ {1, . . . , 10} documents are retrieved are [(1, 1/12 ), ( 1/2 , 1/12 ), ( 2/3 , 2 /12 ), ( 3 /4 , 3 /12 ), ( 3 /5 , 3 /12 ), ( 4 /6 , 4 /12 ), ( 5 /7 , 5 /12 ), ( 6 /8 , 6 /12 ), ( 7 /9 , 7 /12 ), ( 8 /10 , 8 /12 )].
After drawing the precision-recall curve we can see the interpolated precision at 20% recall is 8/10 .

```python
docs = ['R', 'I', 'R', 'R', 'I', 'R', 'R', 'R', 'R', 'R']
total_relevant = 12
target_recall = 0.2

relevant = 12
target_recall = 0.2

precisions_recalls= []
count = 0

for i, doc in enumerate(docs):
    if doc=='R':
        count+=1
    precision = count / (i + 1)
    recall = count / relevant 
    precisions_recalls.append((precision,recall))

interpolated_precision = max(precision for precision,r in precisions_recalls if r >= target_recall)
print("interpolated: ", interpolated_precision)
```

#### F1-score at 5 documents retrieved

$$F1 \text{ score} = \frac{2 \times \text{precision} \times \text{recall}}{\text{precision} + \text{recall}}$$

The number of relevant documents is 3 when 5 documents are retrieved, so the precision is 3/5 = 0.6. The recall is 0.25 as calculated above. Therefore the F1-score is 2×0.6×0.25 / (0.6+0.25) = 6/7 ≈ 0.35.


If there is alpha, using this formula: $F = \frac{1}{\alpha \frac{1}{P} + (1 - \alpha) \frac{1}{R}}$
