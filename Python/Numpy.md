
Import:
```
import numpy as np
```

1-D arrays:

```python
# 1. convert list
L = [1,2,3]
arr1 = np.array(L)

# 2. convert tuple
T = (4,5,6)
arr2 = np.array(T)

# 3. claim data type
arr3 = np.array([1,2,3,4],dtype = float)

# 4. arange
arr4 = np.arange(1,10,2)

# 5. linespace 
arr5 = np.linspace(1,10,5) #(arithmetric)

# 6. logspace 
arr6 = np.logspace(0,9,10,base=3) #(geometric)


```


DataType conversion:
```python
arr3 = arr3.astype(np.int32)
```


N-D array

2-D
```python
# 1. convert 2-D list
LL= [[1,2,3],[4,5,6],[7,8,9]]
arr7 = np.array(LL)

# 2. ones
arr9 = np.ones((4,4), dtype = 'float')

# 3. zeros
arr10 = np.zeros((4,4), dtype = 'float')

# 4. empty
arr11 = np.empty((2,2))

# 5. full
arr12 = np.full((2,5),5)

# 6. identity
arr13 = np.identity(3,dtype="float")

# 7. eye
arr14 = np.eye(3,6,1)

```


Shape:

```python
arr8 = np.arange(1,10)
arr8 = arr8.reshape(3,3)
arr8
```


Property
```python
arr14.ndim
arr14.shape
arr14.size
arr14.itemsize
arr14.nbytes
```

Transpose
```python
arr15 = arr14.T
```

Search
```python
# 1. search by boolean list
arr16 = np.linspace(1,10,5)
boolist = [True, False,True,False,True]
arr17 = np.array(boolist)
arr18 = arr16[arr17]

# 2. search by index list
arr19 = np.arange(1,16,2)
i = np.array([[1,1],[3,4]])
arr20 = arr19[i]

# search by multi index list
arr21 = np.arange(12).reshape(3,4)
m = np.array([[1,1],[2,0]])
n = np.array([[2,0],[1,1]])
arr22 = arr21[m,n]
```


Iterate:

```python
# one loop get all elements
for x in np.nditer(arr21):
    print(x,end=",")
```


concatenate

```python
# 1. concatenate
arr23 = np.array([[1,2,3],[1,2,3]])
arr24 = np.array([[4,5,6],[4,5,6]])
arr25 = np.concatenate((arr23,arr24),0) # along row 

# 2. hstack
arr26 = np.hstack((arr23,arr24)) # always axis = 1 (col)

# 3. vstack
arr27 = np.vstack((arr23,arr24)) # always axis = 0 (row)
```


split

```python
a = np.linspace(1,20,9)
b = a.reshape(3,3)

# 1. split
arr28 = np.split(a,3)
arr29 = np.split(a,[4,7])
arr30 = np.split(b,3,axis=1)

# 2. hsplit
arr31 = np.hsplit(b,3)

# 3. vsplit
arr31 = np.vsplit(b,3)


```




