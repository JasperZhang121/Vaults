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

df.describe()
df['b'].mean()
df.mean()
df.max()
df.min()
df.cov()
df.corr()
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

#basic search 
df['a']
df.loc[1:3] #including 3
df.loc[3,'b']
df.loc[3,['b','c']]
df.loc[[2,3],['b','c']]
df.loc[1:3,['b','c']]

#conditin applied (bool list)
df.loc[df['b']<7,:]

# lambda expression
df.loc[lambda df: (df['b']<7) & (df['c']>1),:]
```


Operations:

```python
# 1. set date as index
df.set_index('a',inplace=True)

# 2. series cal
df1 = df
df1['c'] = df1['b']-df['c']

# 3. increase a new col
df.loc[:,'d'] = df['b']-df['c']
df['e'] = df['b']-df['c']
# df.apply and df.assign

# 3. missing data
df.isnull()
df.fillna({'e':0}, inplace =True)

# 4. sort
df['c'].sort_values()
df.sort_values(by='c',ascending=True)

# 5. str
df['col_name'].str.replace("[abc]","d") #repalce a,b,c to d whenever find them in the df

# 6. drop
df.drop("e",axis=1)
df.drop(1,axis=0)

# 7. concat
dat = {
    'h':[1,2,3,4],
    'j':[6,7,8,9],
    'k':[1,3,4,5]
}
df2 = pd.DataFrame(dat,index=[1,2,3,4])
pd.concat([df,df2],axis = 1)

# index modify
df.index-=1
```


Missingness:

```python
missingness = df[['col1', 'col2', 'col3']].notnull().astype('int')
pattern_counts = missingness.groupby(['col1', 'col2', 'col3']).size().reset_index(name='Counts')

# Completeness for col1 
col1_completeness = (df['col1'].notna().sum() / len(df)) * 100
```

Correlation:

```python
correlation_value = df['col1'].corr(df['col2'])
```

Cross Tabulae:

```python
pd.crosstab(df['col1'], df['col2'])
```

Date Format:

```python
df['birth_date'] = pd.to_datetime(df['birth_date'], format='%d/%m/%Y')
df['birth_year'] = df['birth_date'].dt.year # extract
```


