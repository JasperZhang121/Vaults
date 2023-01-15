```python
import pandas as pd
```

Read:
```python
readExcel = pd.read_excel("temp.xlsx")
readtxt = pd.read_csv('text.txt',sep='\t',header=None, names=['one'])
```

Porperty checks:
```python
readExcel.head()
readExcel.columns
readExcel.index
readExcel.dtypes
```

DataFrame & Series
```python
# 1. Series
# direct
S1 = pd.Series([1,'a',3,4])
S1.index
S1.values
# initialize index
s2 = pd.Series([1,'a',3,4],index=['a','b','c','d'])
# dict to series
dic = {'a':1,'b':2}
s3 = pd.Series(dic)

# 2. DataFrame
# dic to dataFrame
data = {  
    'a':[1,2,3,4],
    'b':[5,6,7,8],
    'c':[1,4,5,9]
}
df =pd.DataFrame(data)

# 3. Search
df['a']
df.loc[1:3] #including 3

```