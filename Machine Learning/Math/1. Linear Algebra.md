
- Vectors
- Matrices
- Inverse and Transpose
- Row-echelon form (REF)
- Moore-Penrose pseudo-inverse
- Gaussian elimination
------

<span style="color: yellow;">**Example**</span>

Given:
$$A = 
\begin{bmatrix}
2&3\\
0&1\\
\end{bmatrix}
\\
x = 
\begin{bmatrix}
1\\
3\\
\end{bmatrix}
$$
```python
A = np.array([[2, 3], [0, 1]])
x = np.array([[1], [3]])
```

<br/>

**Basic matrix calculations:**

Remember the matrix multiplication uses @ in python:
```python
#Matrix Multiplication Example
b = A @ x
print('\nMatrix Multiplication')
print(b)
```

Other basics:
```python
#Matrix Addition Example
b = A + x
print('\nMatrix Addition')
print(b)

#Elementwise Multiplication Example
b = A * x
print('\nElementwise Matrix Multiplication')
print(b)

#Extract a single element of a matrix:
print('\nSingle Element Extraction')
b = A[0, 0]
print(b)

#Extract an entire column of a matrix:
print('\nColumn Extraction')
b = A[:, 0]
print(b)

#Extract an entire row of a matrix:
print('\nRow Extraction')
b = A[0, :]
print(b)

#Transpose of a matrix:
print('\nTranspose')
A_Transpose = A.T
print(A_Transpose)
```

<br/>
<br/>

**Linear equations**:

$$a_{11}x_1+a_{12}x_2 \dots a_{1d}x_d=b_1$$
$$a_{21}x_1+a_{22}x_2 \dots a_{2d}x_d=b_2$$
$$\vdots$$
$$a_{n1}x_1+a_{n2}x_2 \dots a_{nd}x_d=b_n$$

The above system of linear equations can also be written down in a compact matrix form as follows:

$$AX = B$$

where,
$$A = \begin{bmatrix}
a_{11} & \dots & a_{1d}\\
\vdots & \ddots & \vdots \\
a_{n1} & \dots & a_{nd}
\end{bmatrix}, \quad
B = \begin{bmatrix}
b_1 \\ \vdots \\ b_n
\end{bmatrix}, \quad
X = \begin{bmatrix}
x_1 \\ \vdots \\ x_d
\end{bmatrix}
$$


Solution for X by using numpy:

```python
A = np.array([[2, 0, 3], [1, -1, 2], [1, -3, 4]])
B = np.array([[2], [1], [2]])

def solve_with_numpy(A,B):
    x = np.linalg.solve(A, B)
    return x
res = solve_with_numpy(A,B)
```


Inverse of matrix: 
```python
A = np.array([[-3, -2], [1, 1]])
from numpy.linalg import inv
A_inv = inv(A)
```

Use inverse solve **Linear equations**:

```python
A = np.array([[-3, -2], [1, 1]])
B = np.array([[4], [6]])
X = A_inv @ B 
```


Moore-Penrose pseudo inverse:  (A<sup>T</sup>A)<sup>-1</sup>A<sup>T</sup>

```python
A = np.array([[1, 3], [2, 7], [5, 1]])
B = np.array([[13], [30], [9]])

A_pseudo_inverse = inv(A.T @ A)@(A.T) 
X = A_pseudo_inverse @ B 
# another methond
print(np.linalg.pinv(A)@ B)
```

