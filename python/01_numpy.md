# NumPy

Note: Some outputs may vary (random functions, platform-dependent dtypes).

NumPy is the core library for fast numerical computing with arrays in Python. Use it for vectorized math, linear algebra, simulations, and as the foundation for scientific and data workflows.

## Creating Arrays

Create arrays quickly with common constructors and random generators.

```python
>>> np.zeros(5)
array([0., 0., 0., 0., 0.])
```

```python
>>> np.ones((2, 3))
array([[1., 1., 1.],
       [1., 1., 1.]])
```

```python
>>> np.arange(0, 5)
array([0, 1, 2, 3, 4])
```

```python
>>> np.linspace(0, 5, 6)
array([0., 1., 2., 3., 4., 5.])
```

```python
>>> np.eye(3)  # Identity matrix
array([[1., 0., 0.],
       [0., 1., 0.],
       [0., 0., 1.]])
```

```python
>>> np.random.seed(0)
>>> np.random.rand(3, 2)  # Random values [0, 1)
array([[0.5488135 , 0.71518937],
       [0.60276338, 0.54488318],
       [0.4236548 , 0.64589411]])
```

```python
>>> np.random.seed(0)
>>> np.random.randn(3)  # Standard normal distribution
array([ 1.76405235,  0.40015721,  0.97873798])
```

```python
>>> np.random.seed(0)
>>> np.random.randint(0, 10, 5)  # Random integers
array([5, 0, 3, 3, 7])
```

```python
>>> np.full((2, 3), 7)  # Fill with specific value
array([[7, 7, 7],
       [7, 7, 7]])
```

---

## Array Attributes

Inspect shape, data type, size, and dimensionality of arrays.

```python
>>> arr = np.array([[1, 2, 3], [4, 5, 6]])
>>> arr.shape
(2, 3)
```

```python
>>> arr.dtype
dtype('int64')  # May be int32 on some platforms
```

```python
>>> arr.ndim  # Number of dimensions
2
```

```python
>>> arr.size  # Total number of elements
6
```

---

## Indexing and Slicing

Select elements, rows, and subarrays using indices and slices.

```python
>>> arr = np.array([10, 20, 30, 40, 50])
>>> arr[0]  # First element
10
```

```python
>>> arr[1:4]  # Slice
array([20, 30, 40])
```

```python
>>> arr[-1]  # Last element
50
```

```python
>>> arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
>>> arr2d[1, 2]  # Row 1, Column 2
6
```

```python
>>> arr2d[:2, 1:]  # First 2 rows, columns from index 1
array([[2, 3],
       [5, 6]])
```

```python
>>> arr2d[0]  # First row
array([1, 2, 3])
```

---

## Boolean Filtering

Filter arrays with boolean masks and combined conditions.

```python
>>> arr = np.array([1, 3, 5, 7])
>>> arr[arr > 3]
array([5, 7])
```

```python
>>> arr[(arr > 2) & (arr < 7)]  # Multiple conditions (use & for AND, | for OR)
array([3, 5])
```

---

## Array Operations (Element-wise)

Apply element-wise arithmetic across arrays and scalars.

```python
>>> arr = np.array([1, 2, 3, 4])
>>> arr + 10
array([11, 12, 13, 14])
```

```python
>>> arr * 2
array([2, 4, 6, 8])
```

```python
>>> arr ** 2  # Square
array([ 1,  4,  9, 16])
```

```python
>>> arr1 = np.array([1, 2, 3])
>>> arr2 = np.array([4, 5, 6])
>>> arr1 + arr2
array([5, 7, 9])
```

```python
>>> arr1 * arr2
array([ 4, 10, 18])
```

---

## Universal Functions (ufuncs)

Use fast vectorized math functions on entire arrays.

```python
>>> arr = np.array([0, 1, 4, 9, 16])
>>> np.sqrt(arr)
array([0., 1., 2., 3., 4.])
```

```python
>>> np.exp(np.array([1, 2, 3]))
array([ 2.71828183,  7.3890561 , 20.08553692])
```

```python
>>> np.sin(np.array([0, np.pi/2, np.pi]))
array([0.0000000e+00, 1.0000000e+00, 1.2246468e-16])
```

```python
>>> np.log(np.array([1, np.e, np.e**2]))
array([0., 1., 2.])
```

```python
>>> np.abs(np.array([-1, -2, 3]))
array([1, 2, 3])
```

---

## Statistical Functions

Compute summary statistics along arrays or axes.

```python
>>> arr = np.array([1, 2, 3, 4, 5])
>>> arr.sum()
15
```

```python
>>> arr.mean()
3.0
```

```python
>>> arr.std()  # Standard deviation
1.4142135623730951
```

```python
>>> arr.var()  # Variance
2.0
```

```python
>>> arr.min()
1
```

```python
>>> arr.max()
5
```

```python
>>> arr.argmin()  # Index of minimum
0
```

```python
>>> arr.argmax()  # Index of maximum
4
```

```python
>>> arr2d = np.array([[1, 2, 3], [4, 5, 6]])
>>> arr2d.sum(axis=0)  # Sum along columns
array([5, 7, 9])
```

```python
>>> arr2d.sum(axis=1)  # Sum along rows
array([ 6, 15])
```

---

## Reshaping Arrays

Reshape, transpose, and flatten arrays without changing data.

```python
>>> arr = np.arange(12)
>>> arr.reshape(3, 4)
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
```

```python
>>> arr.reshape(2, 6)
array([[ 0,  1,  2,  3,  4,  5],
       [ 6,  7,  8,  9, 10, 11]])
```

```python
>>> arr2d = np.array([[1, 2, 3], [4, 5, 6]])
>>> arr2d.T  # Transpose
array([[1, 4],
       [2, 5],
       [3, 6]])
```

```python
>>> arr.reshape(3, 4).flatten()  # Convert to 1D
array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
```

---

## Stacking Arrays

Combine arrays vertically, horizontally, or by concatenation.

```python
>>> arr1 = np.array([1, 2, 3])
>>> arr2 = np.array([4, 5, 6])
>>> np.vstack((arr1, arr2))  # Vertical stack
array([[1, 2, 3],
       [4, 5, 6]])
```

```python
>>> np.hstack((arr1, arr2))  # Horizontal stack
array([1, 2, 3, 4, 5, 6])
```

```python
>>> np.concatenate((arr1, arr2))
array([1, 2, 3, 4, 5, 6])
```

---

## Broadcasting

Perform operations on arrays with compatible shapes.

```python
>>> arr = np.array([[1, 2, 3], [4, 5, 6]])
>>> arr + np.array([10, 20, 30])  # Adds to each row
array([[11, 22, 33],
       [14, 25, 36]])
```

```python
>>> arr * 10  # Scalar broadcasting
array([[10, 20, 30],
       [40, 50, 60]])
```

---

## Matrix Operations

Run linear algebra operations like dot products and eigendecomposition.

```python
>>> A = np.array([[1, 2], [3, 4]])
>>> B = np.array([[5, 6], [7, 8]])
>>> np.dot(A, B)  # Matrix multiplication
array([[19, 22],
       [43, 50]])
```

```python
>>> A @ B  # Alternative matrix multiplication (Python 3.5+)
array([[19, 22],
       [43, 50]])
```

```python
>>> np.linalg.det(A)  # Determinant
-2.0
```

```python
>>> np.linalg.inv(A)  # Inverse matrix (requires non-singular matrix)
array([[-2. ,  1. ],
       [ 1.5, -0.5]])
```

```python
>>> eigenvalues, eigenvectors = np.linalg.eig(A)
>>> eigenvalues
array([-0.37228132,  5.37228132])
```

---

## Copying Arrays

Understand views vs copies and how to make independent data.

```python
>>> arr = np.array([1, 2, 3])
>>> arr_view = arr  # Not a copy! Same reference
>>> arr_copy = arr.copy()  # True copy
```

**Important:** Views share data with the original array, while copies are independent.

---

## Useful Tips

Handy utilities for uniqueness, selection, clipping, and rounding.

```python
>>> np.unique(np.array([1, 1, 2, 3, 3, 4]))  # Get unique values
array([1, 2, 3, 4])
```

```python
>>> np.where(np.array([1, 2, 3, 4]) > 2)  # Returns a tuple of index arrays
(array([2, 3]),)
```

```python
>>> np.clip(np.array([1, 5, 10, 15]), 2, 12)  # Clip values
array([ 2,  5, 10, 12])
```

```python
>>> np.round(np.array([1.234, 5.678]), 2)  # Round to decimals
array([1.23, 5.68])
```

---

## Key Ideas

Core takeaways to remember when working with NumPy.
- **NumPy = math on arrays.** Operations apply to the whole array.
- **Broadcasting** allows operations on arrays of different shapes.
- **Use `.copy()`** when you need an independent copy of an array.
- **Axis parameter:** axis=0 operates on columns, axis=1 on rows.





