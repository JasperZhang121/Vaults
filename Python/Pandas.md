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

```