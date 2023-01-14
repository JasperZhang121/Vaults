
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



```



