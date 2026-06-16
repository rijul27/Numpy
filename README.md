# Numpy
Numpy Basics to Advance

------------

# NumPy Broadcasting

**Broadcasting** is NumPy's mechanism for performing operations on arrays of **different shapes** without explicitly copying data.

It allows NumPy to "stretch" the smaller array logically to match the larger array.

---

## Why Broadcasting?

Without broadcasting:

```python
a = np.array([1, 2, 3])

b = 10

a + b
```

How can an array and a single number be added?

NumPy internally treats it as:

```text
[1 2 3]
+
[10 10 10]
```

Result:

```python
[11 12 13]
```

No actual copy of `[10,10,10]` is created.

---

## Example 1: Scalar + Array

```python
import numpy as np

a = np.array([1,2,3])

print(a + 5)
```

Output:

```python
[6 7 8]
```

Broadcasting expands `5` across all elements.

---

## Example 2: 1D Array + 1D Array

```python
a = np.array([1,2,3])
b = np.array([10,20,30])

print(a + b)
```

Output:

```python
[11 22 33]
```

Shapes are the same:

```text
(3,) + (3,)
```

No broadcasting needed.

---

## Example 3: 2D Array + Scalar

```python
a = np.array([
    [1,2,3],
    [4,5,6]
])

print(a + 10)
```

Output:

```python
[[11 12 13]
 [14 15 16]]
```

Shape:

```text
(2,3) + scalar
```

---

## Example 4: Row Broadcasting

```python
a = np.array([
    [1,2,3],
    [4,5,6]
])

b = np.array([10,20,30])

print(a + b)
```

Shapes:

```text
a = (2,3)
b = (3,)
```

NumPy treats `b` as:

```text
[[10 20 30]
 [10 20 30]]
```

Result:

```python
[[11 22 33]
 [14 25 36]]
```

---

## Example 5: Column Broadcasting

```python
a = np.array([
    [1,2,3],
    [4,5,6]
])

b = np.array([
    [10],
    [20]
])

print(a + b)
```

Shapes:

```text
a = (2,3)
b = (2,1)
```

NumPy stretches columns:

```text
[[10 10 10]
 [20 20 20]]
```

Result:

```python
[[11 12 13]
 [24 25 26]]
```

---

# Broadcasting Rules

NumPy compares shapes from **right to left**.

Two dimensions are compatible if:

1. They are equal
2. One of them is 1

---

## Valid Example

Shapes:

```text
(2,3)
(1,3)
```

Comparison:

```text
3 = 3 ✔
2 vs 1 ✔
```

Broadcasting works.

---

## Another Valid Example

Shapes:

```text
(4,1)
(1,5)
```

Comparison:

```text
1 vs 5 ✔
4 vs 1 ✔
```

Result shape:

```text
(4,5)
```

---

## Invalid Example

Shapes:

```text
(2,3)
(2,2)
```

Comparison:

```text
3 vs 2 ❌
```

Error:

```python
ValueError:
operands could not be broadcast together
```

---

## Visual Example

```python
a = np.array([
    [1],
    [2],
    [3]
])

b = np.array([10,20,30])
```

Shapes:

```text
a = (3,1)
b = (3,)
```

Think of `b` as `(1,3)`.

```text
a:

[[1]
 [2]
 [3]]

b:

[10 20 30]
```

Broadcasted:

```text
[[1 1 1]
 [2 2 2]
 [3 3 3]]

[[10 20 30]
 [10 20 30]
 [10 20 30]]
```

Result:

```python
[[11 21 31]
 [12 22 32]
 [13 23 33]]
```

---

# Checking Shapes

```python
a.shape
b.shape
```

Always check shapes first when debugging broadcasting errors.

---

# Real-World Example

Normalize data:

```python
data = np.array([
    [10,20,30],
    [40,50,60]
])

mean = np.array([25,35,45])

normalized = data - mean
```

Broadcasting automatically subtracts the mean from each row.

Result:

```python
[[-15 -15 -15]
 [ 15  15  15]]
```

---

# Interview Questions

### Q1. What is broadcasting?

A mechanism that allows NumPy to perform operations on arrays of different shapes by virtually expanding smaller arrays.

---

### Q2. When does broadcasting work?

When dimensions are:

* Equal, or
* One of them is 1

---

### Q3. From which side are shapes compared?

**Right to left**.

---

### Q4. What is the result shape?

Example:

```text
(4,1)
(1,5)
```

Result:

```text
(4,5)
```

---

### Q5. Why is broadcasting fast?

Because NumPy usually does **not create actual copies** of the repeated values. It performs operations using optimized C code and shared memory views.

---

# Memory Trick

```text
Rule 1:
Compare shapes from RIGHT → LEFT

Rule 2:
Dimensions must be:
Equal OR 1

Rule 3:
Result shape = Maximum dimension
```

Example:

```text
(3,1)
(1,4)
------
(3,4)
```

# NumPy `reshape()`

`reshape()` is used to **change the shape (dimensions)** of an array **without changing its data**.

---

## Syntax

```python
arr.reshape(new_shape)
```

or

```python
np.reshape(arr, new_shape)
```

---

## Example 1: 1D → 2D

```python
import numpy as np

arr = np.array([1,2,3,4,5,6])

new_arr = arr.reshape(2,3)

print(new_arr)
```

Output:

```python
[[1 2 3]
 [4 5 6]]
```

Shape:

```python
print(arr.shape)      # (6,)
print(new_arr.shape)  # (2,3)
```

---

## Important Rule

The total number of elements must remain the same.

```text
Old array = 6 elements

New shape must also contain 6 elements
```

Valid:

```python
arr.reshape(2,3)
arr.reshape(3,2)
arr.reshape(1,6)
arr.reshape(6,1)
```

Invalid:

```python
arr.reshape(4,2)
```

Because:

```text
4 × 2 = 8 elements
```

Error:

```python
ValueError: cannot reshape array
```

---

## Example 2: 2D → 1D

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

print(arr.reshape(6))
```

Output:

```python
[1 2 3 4 5 6]
```

---

## Example 3: 2D → 3D

```python
arr = np.arange(12)

new_arr = arr.reshape(2,2,3)

print(new_arr)
```

Output:

```python
[[[ 0  1  2]
  [ 3  4  5]]

 [[ 6  7  8]
  [ 9 10 11]]]
```

Shape:

```python
(2,2,3)
```

Check:

```text
2 × 2 × 3 = 12
```

---

# Using `-1` (Very Important)

NumPy can automatically calculate one dimension.

```python
arr = np.arange(12)

arr.reshape(3, -1)
```

Output:

```python
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
```

NumPy calculates:

```text
12 ÷ 3 = 4
```

So shape becomes:

```text
(3,4)
```

---

## More Examples

```python
arr.reshape(-1,2)
```

Output shape:

```text
(6,2)
```

Because:

```text
12 ÷ 2 = 6
```

---

### Only One `-1` Allowed

Correct:

```python
arr.reshape(3,-1)
```

Wrong:

```python
arr.reshape(-1,-1)
```

Error:

```python
ValueError
```

NumPy cannot calculate two unknown dimensions.

---

# Reshape and Memory

A common interview question:

### Does `reshape()` create a copy?

Usually:

```text
No
```

It returns a **view** (shares memory).

Example:

```python
a = np.arange(6)

b = a.reshape(2,3)

np.shares_memory(a,b)
```

Output:

```python
True
```

---

## Proof

```python
a = np.arange(6)

b = a.reshape(2,3)

b[0,0] = 100

print(a)
```

Output:

```python
[100   1   2   3   4   5]
```

Original array changed because memory is shared.

---

# Row-Major Order (Default)

NumPy fills rows first.

```python
arr = np.arange(6)

arr.reshape(2,3)
```

Output:

```python
[[0 1 2]
 [3 4 5]]
```

Filled row by row.

---

# `order='F'` (Column Major)

```python
arr.reshape(2,3, order='F')
```

Output:

```python
[[0 2 4]
 [1 3 5]]
```

Uses column-wise filling (Fortran style).

---

# Related Functions

### Flatten

Returns a copy.

```python
arr.flatten()
```

---

### Ravel

Usually returns a view.

```python
arr.ravel()
```

---

### Resize

Changes shape and can modify size.

```python
np.resize(arr, (3,4))
```

---

# Interview Questions

### Q1. What is `reshape()`?

Changes the dimensions of an array without changing the data.

---

### Q2. What condition must be satisfied?

Total number of elements must remain the same.

```text
Old Elements = New Elements
```

---

### Q3. What does `-1` mean?

NumPy automatically calculates that dimension.

Example:

```python
arr.reshape(2,-1)
```

---

### Q4. Does `reshape()` create a copy?

Usually no.

It returns a **view** and shares memory.

---

### Q5. Difference between `reshape()` and `resize()`?

| reshape()                 | resize()                     |
| ------------------------- | ---------------------------- |
| Changes shape only        | Can change shape and size    |
| Elements count must match | Can repeat/truncate elements |
| Usually view              | May allocate new memory      |

---

# Visualization

Original:

```text
[1 2 3 4 5 6]
```

Reshape:

```python
reshape(2,3)
```

Result:

```text
1 2 3
4 5 6
```

Reshape:

```python
reshape(3,2)
```

Result:

```text
1 2
3 4
5 6
```

Same data, different arrangement.

---

# Quick Revision

```python
arr.reshape(2,3)
```

➡ 2 rows, 3 columns

```python
arr.reshape(3,2)
```

➡ 3 rows, 2 columns

```python
arr.reshape(-1,2)
```

➡ NumPy calculates rows

```python
arr.reshape(2,-1)
```

➡ NumPy calculates columns

```python
np.shares_memory(a,b)
```

➡ Check if reshape shares memory

### Memory Trick

```text
reshape()
↓
Change Shape
Not Data

Total Elements
Must Stay Same

-1
↓
Auto Calculate
```

# NumPy Array Manipulation

Array manipulation means **changing the shape, structure, orientation, or arrangement of arrays** without changing the underlying data values.

This is one of the most important NumPy topics for Data Science, Machine Learning, and interviews.

---

# 1. `reshape()`

Changes array dimensions.

```python
import numpy as np

arr = np.arange(6)

arr.reshape(2,3)
```

Output:

```text
[[0 1 2]
 [3 4 5]]
```

Use when converting:

* 1D → 2D
* 2D → 3D
* ML input preparation

---

# 2. `flatten()`

Converts any array into a 1D array.

Creates a **copy**.

```python
arr = np.array([
    [1,2],
    [3,4]
])

arr.flatten()
```

Output:

```python
array([1,2,3,4])
```

---

# 3. `ravel()`

Also converts to 1D.

Usually returns a **view**.

```python
arr.ravel()
```

Interview question:

| Function  | Copy/View      |
| --------- | -------------- |
| flatten() | Copy           |
| ravel()   | View (usually) |

---

# 4. `transpose()`

Rows become columns.

```python
arr = np.array([
    [1,2,3],
    [4,5,6]
])

arr.T
```

Output:

```text
[[1 4]
 [2 5]
 [3 6]]
```

Shape:

```text
(2,3) → (3,2)
```

---

# 5. `swapaxes()`

Swaps two axes.

```python
arr.swapaxes(0,1)
```

Mostly used with 3D arrays.

---

# 6. `moveaxis()`

Moves an axis to a new position.

```python
np.moveaxis(arr, 0, 2)
```

Very common in:

* Deep Learning
* Image processing

---

# 7. `expand_dims()`

Adds a dimension.

```python
arr = np.array([1,2,3])

np.expand_dims(arr, axis=0)
```

Output:

```text
[[1 2 3]]
```

Shape:

```text
(3,) → (1,3)
```

---

# 8. `squeeze()`

Removes dimensions of size 1.

```python
arr = np.array([[[1,2,3]]])

np.squeeze(arr)
```

Output:

```python
array([1,2,3])
```

---

# 9. `resize()`

Changes shape and size.

```python
np.resize(np.array([1,2,3]), (2,4))
```

Output:

```text
[[1 2 3 1]
 [2 3 1 2]]
```

Notice values repeat.

---

# Joining Arrays

---

# 10. `concatenate()`

Most important joining function.

```python
a = np.array([1,2,3])
b = np.array([4,5,6])

np.concatenate((a,b))
```

Output:

```python
array([1,2,3,4,5,6])
```

---

# 11. `vstack()`

Vertical stacking.

```python
a = np.array([1,2,3])
b = np.array([4,5,6])

np.vstack((a,b))
```

Output:

```text
[[1 2 3]
 [4 5 6]]
```

---

# 12. `hstack()`

Horizontal stacking.

```python
np.hstack((a,b))
```

Output:

```python
array([1,2,3,4,5,6])
```

---

# 13. `dstack()`

Stack along depth axis.

```python
np.dstack((a,b))
```

Creates 3D array.

---

# 14. `stack()`

General stacking function.

```python
np.stack((a,b))
```

Output:

```text
[[1 2 3]
 [4 5 6]]
```

---

# Splitting Arrays

---

# 15. `split()`

Split into equal parts.

```python
arr = np.arange(10)

np.split(arr,[3,7])
```

Output:

```text
[array([0,1,2]),
 array([3,4,5,6]),
 array([7,8,9])]
```

---

# 16. `array_split()`

Works even if division isn't equal.

```python
np.array_split(arr,3)
```

Output:

```text
[array([0,1,2,3]),
 array([4,5,6]),
 array([7,8,9])]
```

---

# 17. `hsplit()`

Horizontal split.

```python
np.hsplit(arr,2)
```

---

# 18. `vsplit()`

Vertical split.

```python
np.vsplit(arr,2)
```

---

# Adding / Removing Elements

---

# 19. `append()`

```python
arr = np.array([1,2,3])

np.append(arr,4)
```

Output:

```python
array([1,2,3,4])
```

---

# 20. `insert()`

```python
np.insert(arr,1,100)
```

Output:

```python
array([1,100,2,3])
```

---

# 21. `delete()`

```python
np.delete(arr,1)
```

Output:

```python
array([1,3])
```

---

# Repeating Arrays

---

# 22. `repeat()`

Repeats elements.

```python
np.repeat([1,2,3],2)
```

Output:

```python
array([1,1,2,2,3,3])
```

---

# 23. `tile()`

Repeats entire array.

```python
np.tile([1,2,3],3)
```

Output:

```python
array([1,2,3,1,2,3,1,2,3])
```

---

# Rearranging Arrays

---

# 24. `flip()`

Reverse elements.

```python
np.flip([1,2,3,4])
```

Output:

```python
array([4,3,2,1])
```

---

# 25. `rot90()`

Rotate matrix by 90°.

```python
np.rot90(arr)
```

---

# 26. `roll()`

Circular shift.

```python
np.roll([1,2,3,4],1)
```

Output:

```python
array([4,1,2,3])
```

---

# Most Important Interview Functions

If you're preparing for interviews, focus on these first:

| Category         | Important Functions                                |
| ---------------- | -------------------------------------------------- |
| Shape Change     | `reshape()`, `flatten()`, `ravel()`                |
| Axis Change      | `transpose()`, `.T`, `swapaxes()`                  |
| Dimension Change | `expand_dims()`, `squeeze()`                       |
| Join Arrays      | `concatenate()`, `stack()`, `vstack()`, `hstack()` |
| Split Arrays     | `split()`, `array_split()`                         |
| Modify Arrays    | `append()`, `insert()`, `delete()`                 |
| Repeat Arrays    | `repeat()`, `tile()`                               |
| Rearrange Arrays | `flip()`, `roll()`, `rot90()`                      |

---
# NumPy Binary Operations

**Binary operations** are operations that involve **two operands (two arrays, or an array and a scalar)**.

Examples:

```python
a + b
a - b
a * b
a / b
```

NumPy performs these operations **element-wise** by default.

---

# 1. Addition (`add`)

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

a + b
```

Output:

```python
array([5,7,9])
```

Equivalent function:

```python
np.add(a, b)
```

---

# 2. Subtraction (`subtract`)

```python
a - b
```

Output:

```python
array([-3,-3,-3])
```

Equivalent:

```python
np.subtract(a, b)
```

---

# 3. Multiplication (`multiply`)

Element-wise multiplication:

```python
a * b
```

Output:

```python
array([ 4,10,18])
```

Equivalent:

```python
np.multiply(a, b)
```

---

# 4. Division (`divide`)

```python
a = np.array([10,20,30])
b = np.array([2,4,5])

a / b
```

Output:

```python
array([5.,5.,6.])
```

Equivalent:

```python
np.divide(a, b)
```

---

# 5. Power (`power`)

```python
a = np.array([2,3,4])

a ** 2
```

Output:

```python
array([ 4, 9,16])
```

Equivalent:

```python
np.power(a, 2)
```

---

# 6. Modulus (`mod`)

Remainder after division.

```python
a = np.array([10,20,30])

a % 3
```

Output:

```python
array([1,2,0])
```

Equivalent:

```python
np.mod(a, 3)
```

---

# 7. Floor Division

```python
a = np.array([10,20,30])

a // 4
```

Output:

```python
array([2,5,7])
```

Equivalent:

```python
np.floor_divide(a, 4)
```

---

# Comparison Operations

These return Boolean arrays.

---

## Equal

```python
a = np.array([1,2,3])
b = np.array([1,5,3])

a == b
```

Output:

```python
array([ True, False, True])
```

Equivalent:

```python
np.equal(a,b)
```

---

## Not Equal

```python
np.not_equal(a,b)
```

Output:

```python
array([False, True, False])
```

---

## Greater Than

```python
np.greater(a,b)
```

---

## Less Than

```python
np.less(a,b)
```

---

## Greater Equal

```python
np.greater_equal(a,b)
```

---

## Less Equal

```python
np.less_equal(a,b)
```

---

# Logical Operations

Work with Boolean arrays.

---

## Logical AND

```python
a = np.array([True,True,False])
b = np.array([True,False,True])

np.logical_and(a,b)
```

Output:

```python
array([ True, False, False])
```

---

## Logical OR

```python
np.logical_or(a,b)
```

Output:

```python
array([ True, True, True])
```

---

## Logical NOT

```python
np.logical_not(a)
```

Output:

```python
array([False,False, True])
```

---

## Logical XOR

```python
np.logical_xor(a,b)
```

Output:

```python
array([False, True, True])
```

---

# Bitwise Operations

Used on integers.

---

## Bitwise AND

```python
a = np.array([5])
b = np.array([3])

np.bitwise_and(a,b)
```

Binary:

```text
5 = 101
3 = 011
---------
    001
```

Output:

```python
array([1])
```

---

## Bitwise OR

```python
np.bitwise_or(a,b)
```

Output:

```python
array([7])
```

---

## Bitwise XOR

```python
np.bitwise_xor(a,b)
```

Output:

```python
array([6])
```

---

## Bitwise NOT

```python
np.bitwise_not(a)
```

---

# Matrix Binary Operations

A very common interview question.

---

## Element-wise Multiplication

```python
A = np.array([[1,2],
              [3,4]])

B = np.array([[5,6],
              [7,8]])

A * B
```

Output:

```python
[[ 5 12]
 [21 32]]
```

---

## Matrix Multiplication

```python
A @ B
```

or

```python
np.matmul(A,B)
```

Output:

```python
[[19 22]
 [43 50]]
```

### Interview Trap

```python
A * B
```

❌ Not matrix multiplication.

```python
A @ B
```

✅ Matrix multiplication.

---

# Broadcasting with Binary Operations

```python
a = np.array([1,2,3])

a + 10
```

Output:

```python
array([11,12,13])
```

Broadcasting applies `10` to every element.

---

## Example

```python
a = np.array([
    [1,2,3],
    [4,5,6]
])

b = np.array([10,20,30])

a + b
```

Output:

```python
[[11 22 33]
 [14 25 36]]
```

---

# Universal Functions (ufuncs)

Most binary operations are implemented as **ufuncs**.

Examples:

```python
np.add()
np.subtract()
np.multiply()
np.divide()
np.power()
```

Advantages:

* Fast (C implementation)
* Vectorized
* Support broadcasting

---

# Most Important Binary Functions

| Function            | Operator              |
| ------------------- | --------------------- |
| `np.add()`          | `+`                   |
| `np.subtract()`     | `-`                   |
| `np.multiply()`     | `*`                   |
| `np.divide()`       | `/`                   |
| `np.power()`        | `**`                  |
| `np.mod()`          | `%`                   |
| `np.floor_divide()` | `//`                  |
| `np.equal()`        | `==`                  |
| `np.greater()`      | `>`                   |
| `np.less()`         | `<`                   |
| `np.logical_and()`  | AND                   |
| `np.logical_or()`   | OR                    |
| `np.bitwise_and()`  | Bitwise AND           |
| `np.matmul()`       | Matrix Multiplication |

---

# Interview Questions

### Q1. What are binary operations?

Operations involving two operands, performed element-wise by NumPy.

---

### Q2. Difference between `*` and `@`?

```python
A * B
```

➡ Element-wise multiplication

```python
A @ B
```

➡ Matrix multiplication

---

### Q3. What are ufuncs?

Universal Functions that perform vectorized operations efficiently on NumPy arrays.

---

### Q4. Can binary operations use broadcasting?

Yes.

```python
array + scalar
array + array_of_different_shape
```

works when broadcasting rules are satisfied.

---

# Quick Revision

```python
np.add(a,b)          # +
np.subtract(a,b)     # -
np.multiply(a,b)     # *
np.divide(a,b)       # /
np.power(a,b)        # **
np.mod(a,b)          # %
np.equal(a,b)        # ==
np.logical_and(a,b)
np.bitwise_and(a,b)
np.matmul(A,B)       # Matrix multiplication
```

### Memory Trick

```text
Arithmetic:
+  -  *  /  %

Comparison:
==  !=  >  <

Logical:
AND OR NOT XOR

Bitwise:
&  |  ^  ~

Matrix:
@
```




