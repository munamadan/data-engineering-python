## 1. Defining Efficient Code

### Two Key Aspects of Efficiency

**1. Minimal Completion Time (Fast Runtime)**
- Code executes quickly
- Reduces waiting time for results

**2. Minimal Resource Consumption (Small Memory Footprint)**
- Uses less memory
- More efficient resource utilization

---

## 2. Defining Pythonic Code

### What is Pythonic?

**Pythonic code:**
- Focuses on readability
- Uses Python's constructs as intended
- Follows Python's design philosophy

### Example: Non-Pythonic vs Pythonic

**Non-Pythonic:**
```python
# Inefficient and verbose
doubled_numbers = []
for i in range(len(numbers)):
    doubled_numbers.append(numbers[i] * 2)
```

**Pythonic:**
```python
# Clean and readable
doubled_numbers = [x * 2 for x in numbers]
```

---

## 3. The Zen of Python

Key principles by Tim Peters:

- **Beautiful is better than ugly**
- **Explicit is better than implicit**
- **Simple is better than complex**
- **Complex is better than complicated**
- **Flat is better than nested**
- **Sparse is better than dense**
- **Readability counts**

> [!tip] Access the Zen of Python
> Type `import this` in Python to see the full list of principles

---

## 4. The Python Standard Library

### What's Included

Part of every standard Python installation:

**Built-in Types:**
- `list`, `tuple`, `set`, `dict`, and others

**Built-in Functions:**
- `print()`, `len()`, `range()`, `round()`, `enumerate()`, `map()`, `zip()`, and others

**Built-in Modules:**
- `os`, `sys`, `itertools`, `collections`, `math`, and others

---

## 5. Built-in Function: range()

### Basic Usage

**Create a range of numbers:**

```python
# Explicitly typing (inefficient)
nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Using range(stop)
nums = range(11)
nums_list = list(nums)
print(nums_list)
# Output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Using range(start, stop)
nums = range(0, 11)
nums_list = list(nums)
print(nums_list)
# Output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### Using Step Values

```python
# range(start, stop, step)
even_nums = range(2, 11, 2)
even_nums_list = list(even_nums)
print(even_nums_list)
# Output: [2, 4, 6, 8, 10]
```

**Syntax:** `range(start, stop, step)`
- **start**: Starting number (inclusive, default 0)
- **stop**: Ending number (exclusive)
- **step**: Increment value (default 1)

---

## 6. Built-in Function: enumerate()

### Basic Usage

**Creates an indexed list of objects:**

```python
letters = ['a', 'b', 'c', 'd']
indexed_letters = enumerate(letters)
indexed_letters_list = list(indexed_letters)
print(indexed_letters_list)
# Output: [(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd')]
```

### Custom Start Index

```python
letters = ['a', 'b', 'c', 'd']
indexed_letters2 = enumerate(letters, start=5)
indexed_letters2_list = list(indexed_letters2)
print(indexed_letters2_list)
# Output: [(5, 'a'), (6, 'b'), (7, 'c'), (8, 'd')]
```

**Use Case:** When you need both the index and value in a loop

---

## 7. Built-in Function: map()

### Basic Usage

**Applies a function over an object:**

```python
nums = [1.5, 2.3, 3.4, 4.6, 5.0]
rnd_nums = map(round, nums)
print(list(rnd_nums))
# Output: [2, 2, 3, 5, 5]
```

### Using map() with Lambda

**Lambda (anonymous function):**

```python
nums = [1, 2, 3, 4, 5]
sqrd_nums = map(lambda x: x ** 2, nums)
print(list(sqrd_nums))
# Output: [1, 4, 9, 16, 25]
```

**Syntax:** `map(function, iterable)`
- Applies function to each element in iterable
- Returns a map object (needs conversion to list)

---

## 8. NumPy Arrays Overview

### NumPy vs Lists

**Python List:**
```python
nums_list = list(range(5))
# Output: [0, 1, 2, 3, 4]
```

**NumPy Array:**
```python
import numpy as np
nums_np = np.array(range(5))
# Output: array([0, 1, 2, 3, 4])
```

### Key Advantage: Homogeneity

**NumPy arrays have a single data type:**

```python
# Integer array
nums_np_ints = np.array([1, 2, 3])
# Output: array([1, 2, 3])
nums_np_ints.dtype
# Output: dtype('int64')

# Float array (automatic conversion)
nums_np_floats = np.array([1, 2.5, 3])
# Output: array([1., 2.5, 3.])
nums_np_floats.dtype
# Output: dtype('float64')
```

> [!info] Type Coercion
> NumPy automatically converts all elements to the same type (typically the most general type needed)

---

## 9. NumPy Array Broadcasting

### The Problem with Lists

**Lists don't support broadcasting:**

```python
nums = [-2, -1, 0, 1, 2]
nums ** 2
# TypeError: unsupported operand type(s) for ** or pow(): 'list' and 'int'
```

### List Workarounds (Inefficient)

**Option 1: For Loop**
```python
sqrd_nums = []
for num in nums:
    sqrd_nums.append(num ** 2)
print(sqrd_nums)
# Output: [4, 1, 0, 1, 4]
```

**Option 2: List Comprehension (Better but not best)**
```python
sqrd_nums = [num ** 2 for num in nums]
print(sqrd_nums)
# Output: [4, 1, 0, 1, 4]
```

### NumPy Solution (Best!)

**Broadcasting for the win:**

```python
nums_np = np.array([-2, -1, 0, 1, 2])
nums_np ** 2
# Output: array([4, 1, 0, 1, 4])
```

**Broadcasting:** Apply operations to entire arrays at once without explicit loops

---

## 10. Basic Indexing

### 1-D Indexing

**Lists vs NumPy Arrays (same syntax):**

```python
# Lists
nums = [-2, -1, 0, 1, 2]
nums[2]      # 0
nums[-1]     # 2
nums[1:4]    # [-1, 0, 1]

# NumPy arrays
nums_np = np.array(nums)
nums_np[2]   # 0
nums_np[-1]  # 2
nums_np[1:4] # array([-1, 0, 1])
```

### 2-D Indexing

**Lists (clunky syntax):**
```python
nums2 = [[1, 2, 3],
         [4, 5, 6]]

# Access element
nums2[0][1]  # 2

# Get column (requires list comprehension)
[row[0] for row in nums2]  # [1, 4]
```

**NumPy Arrays (elegant syntax):**
```python
nums2_np = np.array(nums2)

# Access element
nums2_np[0, 1]  # 2

# Get column (simple!)
nums2_np[:, 0]  # array([1, 4])
```

> [!tip] NumPy Advantage
> Cleaner, more intuitive syntax for multi-dimensional indexing

---

## 11. Boolean Indexing

### NumPy Boolean Indexing

**Filter arrays based on conditions:**

```python
nums = [-2, -1, 0, 1, 2]
nums_np = np.array(nums)

# Create boolean mask
nums_np > 0
# Output: array([False, False, False, True, True])

# Apply mask to filter
nums_np[nums_np > 0]
# Output: array([1, 2])
```

### List Alternatives (Less Efficient)

**Option 1: For Loop**
```python
pos = []
for num in nums:
    if num > 0:
        pos.append(num)
print(pos)
# Output: [1, 2]
```

**Option 2: List Comprehension**
```python
pos = [num for num in nums if num > 0]
print(pos)
# Output: [1, 2]
```

> [!success] NumPy Wins
> Boolean indexing is more concise and faster than list alternatives

---

## 12. Key Comparisons

### Lists vs NumPy Arrays

| Feature | Lists | NumPy Arrays |
|---------|-------|--------------|
| **Data Types** | Mixed types allowed | Homogeneous (single type) |
| **Broadcasting** | ❌ Not supported | ✅ Supported |
| **2-D Indexing** | `list[row][col]` | `array[row, col]` |
| **Boolean Indexing** | ❌ Not available | ✅ Available |
| **Performance** | Slower for numerical ops | Faster for numerical ops |
| **Memory** | More memory per element | Less memory (fixed type) |

---

## 13. Best Practices

### When to Use Built-in Functions

✅ **Use `range()`** instead of typing out number lists  
✅ **Use `enumerate()`** when you need index and value  
✅ **Use `map()`** to apply functions to iterables  
✅ **Use list comprehensions** over explicit for loops

### When to Use NumPy

✅ **Numerical computations** on large datasets  
✅ **Broadcasting operations** (element-wise operations)  
✅ **Multi-dimensional data** (matrices, tensors)  
✅ **Boolean filtering** of data  
✅ **Performance-critical** numerical code

---

## 14. Code Examples Summary

### Built-in Functions

```python
# range()
even_nums = range(2, 11, 2)  # [2, 4, 6, 8, 10]

# enumerate()
for i, letter in enumerate(['a', 'b', 'c']):
    print(i, letter)

# map()
squared = map(lambda x: x**2, [1, 2, 3])
```

### NumPy Operations

```python
import numpy as np

# Create array
arr = np.array([1, 2, 3, 4, 5])

# Broadcasting
arr * 2  # array([2, 4, 6, 8, 10])

# Boolean indexing
arr[arr > 3]  # array([4, 5])

# 2-D indexing
arr_2d = np.array([[1, 2], [3, 4]])
arr_2d[:, 0]  # array([1, 3]) - first column
```

---

## Key Takeaways

✅ **Efficient code** = Fast runtime + Small memory footprint

✅ **Pythonic code** = Readable + Uses Python's constructs properly

✅ **Use built-ins** like `range()`, `enumerate()`, `map()` for cleaner code

✅ **NumPy arrays** are faster and more powerful than lists for numerical work

✅ **Broadcasting** allows operations on entire arrays without loops

✅ **Boolean indexing** provides elegant filtering in NumPy

✅ **The Zen of Python** guides design decisions

---

#python #efficiency #numpy #built-ins #pythonic #datacamp #performance #optimization

## Section A: Multiple Choice Questions (60 points)

_2 points each | 30 questions_

### Defining Efficient & Pythonic Code (6 questions)

**Q1.** What are the two key aspects of writing efficient Python code?

- [ ] A) Readability and simplicity
- [ ] B) Minimal completion time and minimal resource consumption
- [ ] C) Using loops and functions
- [ ] D) Short code and fast typing

> [!success]- Answer **Correct Answer: B) Minimal completion time and minimal resource consumption**
> 
> Efficient code has fast runtime (minimal completion time) and small memory footprint (minimal resource consumption).

**Q2.** What does "Pythonic" code mean?

- [ ] A) Code written in Python
- [ ] B) Code that focuses on readability and uses Python's constructs as intended
- [ ] C) Code with many comments
- [ ] D) Code that runs fast

> [!success]- Answer **Correct Answer: B) Code that focuses on readability and uses Python's constructs as intended**
> 
> Pythonic code emphasizes readability and proper use of Python's features and design patterns.

**Q3.** Which is more Pythonic?

- [ ] A) `doubled = []; for i in range(len(nums)): doubled.append(nums[i] * 2)`
- [ ] B) `doubled = [x * 2 for x in nums]`
- [ ] C) Both are equally Pythonic
- [ ] D) Neither is Pythonic

> [!success]- Answer **Correct Answer: B) `doubled = [x * 2 for x in nums]`**
> 
> List comprehensions are more Pythonic than explicit for loops with indexing. They're more readable and concise.

**Q4.** According to The Zen of Python, which is better?

- [ ] A) Implicit is better than explicit
- [ ] B) Complex is better than simple
- [ ] C) Readability counts
- [ ] D) Ugly is better than beautiful

> [!success]- Answer **Correct Answer: C) Readability counts**
> 
> The Zen of Python emphasizes: "Explicit is better than implicit", "Simple is better than complex", "Beautiful is better than ugly", and "Readability counts".

**Q5.** What is the goal of writing efficient Python code?

- [ ] A) To write the shortest code possible
- [ ] B) To make code a tool for insights, not something that leaves you waiting
- [ ] C) To use the most advanced Python features
- [ ] D) To avoid using any libraries

> [!success]- Answer **Correct Answer: B) To make code a tool for insights, not something that leaves you waiting**
> 
> The course goal is that code should be a tool for gaining insights, not something that wastes time.

**Q6.** Which Zen of Python principle applies to nested code structures?

- [ ] A) Sparse is better than dense
- [ ] B) Flat is better than nested
- [ ] C) Simple is better than complex
- [ ] D) Explicit is better than implicit

> [!success]- Answer **Correct Answer: B) Flat is better than nested**
> 
> "Flat is better than nested" encourages avoiding deeply nested code structures.

---

### Built-in Functions (12 questions)

**Q7.** What does `range(5)` produce?

- [ ] A) [1, 2, 3, 4, 5]
- [ ] B) [0, 1, 2, 3, 4]
- [ ] C) [0, 1, 2, 3, 4, 5]
- [ ] D) [5]

> [!success]- Answer **Correct Answer: B) [0, 1, 2, 3, 4]**
> 
> `range(stop)` generates numbers from 0 up to (but not including) stop. Range is exclusive of the stop value.

**Q8.** What does `range(2, 11, 2)` produce?

- [ ] A) [2, 4, 6, 8, 10]
- [ ] B) [2, 4, 6, 8, 10, 12]
- [ ] C) [0, 2, 4, 6, 8, 10]
- [ ] D) [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

> [!success]- Answer **Correct Answer: A) [2, 4, 6, 8, 10]**
> 
> `range(start, stop, step)` with step=2 creates even numbers from 2 to 10 (11 is excluded).

**Q9.** What is the syntax for range()?

- [ ] A) range(start, stop, step)
- [ ] B) range(stop, start, step)
- [ ] C) range(step, start, stop)
- [ ] D) range(stop, step, start)

> [!success]- Answer **Correct Answer: A) range(start, stop, step)**
> 
> Correct order: start (inclusive, default 0), stop (exclusive), step (increment, default 1).

**Q10.** What does `enumerate(['a', 'b', 'c'])` return?

- [ ] A) ['a', 'b', 'c']
- [ ] B) [('a', 0), ('b', 1), ('c', 2)]
- [ ] C) [(0, 'a'), (1, 'b'), (2, 'c')]
- [ ] D) {'a': 0, 'b': 1, 'c': 2}

> [!success]- Answer **Correct Answer: C) [(0, 'a'), (1, 'b'), (2, 'c')]**
> 
> `enumerate()` returns tuples of (index, value), with index first.

**Q11.** What does the `start` parameter do in `enumerate(letters, start=5)`?

- [ ] A) Starts enumerating from the 5th element
- [ ] B) Sets the starting index to 5
- [ ] C) Limits enumeration to 5 elements
- [ ] D) Repeats enumeration 5 times

> [!success]- Answer **Correct Answer: B) Sets the starting index to 5**
> 
> `start` parameter changes the starting index value. Default is 0.

**Q12.** What does `map(round, [1.5, 2.3, 3.4])` return?

- [ ] A) [1.5, 2.3, 3.4]
- [ ] B) [2, 2, 3]
- [ ] C) [1, 2, 3]
- [ ] D) A map object (needs conversion to list)

> [!success]- Answer **Correct Answer: D) A map object (needs conversion to list)**
> 
> `map()` returns a map object. Use `list()` to convert: `list(map(round, [1.5, 2.3, 3.4]))` gives [2, 2, 3].

**Q13.** What does `map(lambda x: x ** 2, [1, 2, 3])` do?

- [ ] A) Multiplies each element by 2
- [ ] B) Squares each element
- [ ] C) Raises 2 to each element's power
- [ ] D) Returns [1, 2, 3]

> [!success]- Answer **Correct Answer: B) Squares each element**
> 
> Lambda function `x ** 2` squares the input. Result: [1, 4, 9].

**Q14.** What's the syntax for map()?

- [ ] A) map(iterable, function)
- [ ] B) map(function, iterable)
- [ ] C) map(function)(iterable)
- [ ] D) map.function(iterable)

> [!success]- Answer **Correct Answer: B) map(function, iterable)**
> 
> Correct syntax: function first, then iterable(s).

**Q15.** Which is part of Python's Standard Library?

- [ ] A) numpy
- [ ] B) pandas
- [ ] C) itertools
- [ ] D) tensorflow

> [!success]- Answer **Correct Answer: C) itertools**
> 
> `itertools` is a built-in module. NumPy, pandas, and TensorFlow require separate installation.

**Q16.** What type of object does range() return?

- [ ] A) list
- [ ] B) tuple
- [ ] C) range object
- [ ] D) generator

> [!success]- Answer **Correct Answer: C) range object**
> 
> `range()` returns a range object, not a list. Convert with `list()` if needed.

**Q17.** What's the best use case for enumerate()?

- [ ] A) When you need to count elements
- [ ] B) When you need both index and value in a loop
- [ ] C) When you want to reverse a list
- [ ] D) When you need to sort a list

> [!success]- Answer **Correct Answer: B) When you need both index and value in a loop**
> 
> `enumerate()` is perfect for accessing both the index and value simultaneously.

**Q18.** What's the output of `list(range(0, 11))`?

- [ ] A) [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
- [ ] B) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
- [ ] C) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
- [ ] D) [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

> [!success]- Answer **Correct Answer: B) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]**
> 
> Starts at 0 (inclusive), ends before 11 (exclusive), giving 0 through 10.

---

### NumPy Arrays (12 questions)

**Q19.** What is a key difference between Python lists and NumPy arrays?

- [ ] A) Arrays can store numbers, lists cannot
- [ ] B) Arrays are homogeneous (single data type), lists can be mixed
- [ ] C) Lists are faster than arrays
- [ ] D) Arrays cannot be indexed

> [!success]- Answer **Correct Answer: B) Arrays are homogeneous (single data type), lists can be mixed**
> 
> NumPy arrays must have one data type for all elements. Lists can mix types.

**Q20.** What does broadcasting mean in NumPy?

- [ ] A) Sending data over a network
- [ ] B) Applying operations to entire arrays without explicit loops
- [ ] C) Copying arrays
- [ ] D) Converting arrays to lists

> [!success]- Answer **Correct Answer: B) Applying operations to entire arrays without explicit loops**
> 
> Broadcasting allows element-wise operations on entire arrays at once.

**Q21.** What happens with `np.array([1, 2.5, 3])`?

- [ ] A) TypeError
- [ ] B) All elements become integers
- [ ] C) All elements become floats: array([1., 2.5, 3.])
- [ ] D) Mixed types are preserved

> [!success]- Answer **Correct Answer: C) All elements become floats: array([1., 2.5, 3.])**
> 
> NumPy automatically converts to the most general type needed (float) to maintain homogeneity.

**Q22.** What happens when you try `[1, 2, 3] ** 2` with a Python list?

- [ ] A) Returns [1, 4, 9]
- [ ] B) Returns [1, 2, 3, 1, 2, 3]
- [ ] C) TypeError
- [ ] D) Returns [[1, 2, 3], [1, 2, 3]]

> [!success]- Answer **Correct Answer: C) TypeError**
> 
> Lists don't support broadcasting. You get: "unsupported operand type(s) for ** or pow(): 'list' and 'int'".

**Q23.** What's the NumPy way to square all elements in an array?

- [ ] A) `[x ** 2 for x in arr]`
- [ ] B) `arr ** 2`
- [ ] C) `map(lambda x: x**2, arr)`
- [ ] D) `for x in arr: x ** 2`

> [!success]- Answer **Correct Answer: B) `arr ** 2`**
> 
> NumPy broadcasting allows direct operation on the entire array: `arr ** 2`.

**Q24.** How do you access the element at row 0, column 1 in a 2-D NumPy array?

- [ ] A) `arr[0][1]`
- [ ] B) `arr[0, 1]`
- [ ] C) `arr(0, 1)`
- [ ] D) Both A and B work

> [!success]- Answer **Correct Answer: D) Both A and B work**
> 
> Both syntaxes work, but `arr[0, 1]` is the preferred NumPy style.

**Q25.** How do you get the first column of a 2-D NumPy array?

- [ ] A) `arr[0]`
- [ ] B) `arr[:, 0]`
- [ ] C) `arr[0, :]`
- [ ] D) `[row[0] for row in arr]`

> [!success]- Answer **Correct Answer: B) `arr[:, 0]`**
> 
> `:` means "all rows", `0` means "first column". Result: all elements in first column.

**Q26.** What does `arr[arr > 0]` do (boolean indexing)?

- [ ] A) Returns True/False values
- [ ] B) Returns only elements greater than 0
- [ ] C) Returns indices of elements > 0
- [ ] D) Modifies arr to remove values <= 0

> [!success]- Answer **Correct Answer: B) Returns only elements greater than 0**
> 
> Boolean indexing filters the array, returning only elements where the condition is True.

**Q27.** What does `np.array([-2, -1, 0, 1, 2]) > 0` return?

- [ ] A) array([1, 2])
- [ ] B) array([False, False, False, True, True])
- [ ] C) array([3, 4])
- [ ] D) True

> [!success]- Answer **Correct Answer: B) array([False, False, False, True, True])**
> 
> Comparison creates a boolean array. To filter, use: `arr[arr > 0]`.

**Q28.** Can Python lists use boolean indexing like `list[list > 0]`?

- [ ] A) Yes, it works the same as NumPy
- [ ] B) No, lists don't support boolean indexing
- [ ] C) Yes, but only with integers
- [ ] D) Yes, but it's slower

> [!success]- Answer **Correct Answer: B) No, lists don't support boolean indexing**
> 
> Boolean indexing is a NumPy feature. For lists, use list comprehensions: `[x for x in list if x > 0]`.

**Q29.** What attribute shows the data type of a NumPy array?

- [ ] A) .type
- [ ] B) .datatype
- [ ] C) .dtype
- [ ] D) .typeof

> [!success]- Answer **Correct Answer: C) .dtype**
> 
> Use `.dtype` to check the data type: `arr.dtype` returns dtype('int64'), dtype('float64'), etc.

**Q30.** Which is more efficient for numerical operations on large datasets?

- [ ] A) Python lists with for loops
- [ ] B) Python lists with list comprehensions
- [ ] C) NumPy arrays with broadcasting
- [ ] D) All are equally efficient

> [!success]- Answer **Correct Answer: C) NumPy arrays with broadcasting**
> 
> NumPy is optimized for numerical operations and is significantly faster than lists.

---

## Section B: True/False Questions (15 points)

_1 point each_

**Q31.** Efficient code means code that is both fast and uses minimal memory. **T/F**

> [!success]- Answer **True** - Efficiency has two aspects: minimal completion time and minimal resource consumption.

**Q32.** Pythonic code prioritizes brevity over readability. **T/F**

> [!success]- Answer **False** - Pythonic code focuses on readability. "Readability counts" is a key Zen of Python principle.

**Q33.** `range(5)` generates [1, 2, 3, 4, 5]. **T/F**

> [!success]- Answer **False** - `range(5)` generates [0, 1, 2, 3, 4]. It starts at 0 and stops before 5.

**Q34.** `range(stop)` is exclusive of the stop value. **T/F**

> [!success]- Answer **True** - `range(10)` generates 0 through 9, not including 10.

**Q35.** `enumerate()` returns tuples of (value, index). **T/F**

> [!success]- Answer **False** - `enumerate()` returns tuples of (index, value), with index first.

**Q36.** `map()` returns a list by default. **T/F**

> [!success]- Answer **False** - `map()` returns a map object. Use `list()` to convert to a list.

**Q37.** NumPy arrays can contain mixed data types like lists. **T/F**

> [!success]- Answer **False** - NumPy arrays are homogeneous and must have a single data type.

**Q38.** Broadcasting allows operations on entire NumPy arrays without explicit loops. **T/F**

> [!success]- Answer **True** - Broadcasting is a key NumPy feature that enables element-wise operations without loops.

**Q39.** Python lists support broadcasting like NumPy arrays. **T/F**

> [!success]- Answer **False** - Lists don't support broadcasting. Trying `[1,2,3] ** 2` raises a TypeError.

**Q40.** `arr[:, 0]` gets the first row of a 2-D NumPy array. **T/F**

> [!success]- Answer **False** - `arr[:, 0]` gets the first column (all rows, column 0). `arr[0, :]` gets the first row.

**Q41.** Boolean indexing is available for both lists and NumPy arrays. **T/F**

> [!success]- Answer **False** - Boolean indexing is a NumPy feature, not available for Python lists.

**Q42.** `np.array([1, 2, 3]).dtype` returns 'int'. **T/F**

> [!success]- Answer **False** - Returns `dtype('int64')` or similar, not just 'int'.

**Q43.** According to The Zen of Python, "Explicit is better than implicit". **T/F**

> [!success]- Answer **True** - This is one of the core principles from The Zen of Python.

**Q44.** List comprehensions are more Pythonic than explicit for loops. **T/F**

> [!success]- Answer **True** - List comprehensions are generally preferred for their readability and conciseness.

**Q45.** NumPy arrays use more memory per element than Python lists. **T/F**

> [!success]- Answer **False** - NumPy arrays use less memory because they have a fixed data type, whereas lists store type information for each element.

---

## Section C: Short Answer Questions (30 points)

_3 points each_

**Q46.** Explain the two key aspects of efficient Python code.

> [!success]- Answer **1. Minimal Completion Time (Fast Runtime):**
> 
> - Code executes quickly
> - Reduces waiting time for results
> - Users can get insights faster
> 
> **2. Minimal Resource Consumption (Small Memory Footprint):**
> 
> - Uses less memory
> - More efficient resource utilization
> - Allows handling larger datasets
> - Better scalability
> 
> **Goal:** Your code should be a tool for gaining insights, not something that leaves you waiting for results.

**Q47.** What makes code "Pythonic"? Provide an example comparing non-Pythonic and Pythonic code.

> [!success]- Answer **Pythonic code:**
> 
> - Focuses on readability
> - Uses Python's constructs as intended
> - Follows Python's design philosophy (Zen of Python)
> 
> **Example - Doubling numbers:**
> 
> **Non-Pythonic:**
> 
> ```python
> doubled_numbers = []
> for i in range(len(numbers)):
>     doubled_numbers.append(numbers[i] * 2)
> ```
> 
> **Pythonic:**
> 
> ```python
> doubled_numbers = [x * 2 for x in numbers]
> ```
> 
> **Why Pythonic is better:**
> 
> - More concise
> - More readable
> - No manual index management
> - Uses list comprehension (Python's intended construct)

**Q48.** Describe `range()` function syntax and provide examples for creating: (a) numbers 0-10, (b) even numbers 2-10.

> [!success]- Answer **Syntax:** `range(start, stop, step)`
> 
> - `start`: Starting number (inclusive, default 0)
> - `stop`: Ending number (exclusive)
> - `step`: Increment value (default 1)
> 
> **(a) Numbers 0-10:**
> 
> ```python
> # Method 1
> nums = list(range(11))
> # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
> 
> # Method 2
> nums = list(range(0, 11))
> # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
> ```
> 
> **(b) Even numbers 2-10:**
> 
> ```python
> even_nums = list(range(2, 11, 2))
> # [2, 4, 6, 8, 10]
> ```
> 
> **Note:** Stop value (11) is exclusive!

**Q49.** Explain `enumerate()` and provide an example showing both default indexing and custom start value.

> [!success]- Answer **Purpose:** Creates an indexed list of objects, returning tuples of (index, value).
> 
> **Default indexing (starts at 0):**
> 
> ```python
> letters = ['a', 'b', 'c', 'd']
> indexed = list(enumerate(letters))
> print(indexed)
> # [(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd')]
> ```
> 
> **Custom start value:**
> 
> ```python
> letters = ['a', 'b', 'c', 'd']
> indexed = list(enumerate(letters, start=5))
> print(indexed)
> # [(5, 'a'), (6, 'b'), (7, 'c'), (8, 'd')]
> ```
> 
> **Use case:** Perfect for loops where you need both index and value:
> 
> ```python
> for i, letter in enumerate(letters):
>     print(f"Index {i}: {letter}")
> ```

**Q50.** Describe `map()` function and provide examples with both a built-in function and a lambda expression.

> [!success]- Answer **Purpose:** Applies a function to each element of an iterable.
> 
> **Syntax:** `map(function, iterable)`
> 
> **Example 1 - Built-in function:**
> 
> ```python
> nums = [1.5, 2.3, 3.4, 4.6, 5.0]
> rnd_nums = map(round, nums)
> print(list(rnd_nums))
> # [2, 2, 3, 5, 5]
> ```
> 
> **Example 2 - Lambda function:**
> 
> ```python
> nums = [1, 2, 3, 4, 5]
> sqrd_nums = map(lambda x: x ** 2, nums)
> print(list(sqrd_nums))
> # [1, 4, 9, 16, 25]
> ```
> 
> **Important:** `map()` returns a map object, not a list. Convert with `list()` to see results.

**Q51.** What is NumPy array homogeneity? Explain with an example showing automatic type conversion.

> [!success]- Answer **Homogeneity:** All elements in a NumPy array must have the same data type.
> 
> **Example - Automatic Type Conversion:**
> 
> ```python
> import numpy as np
> 
> # Integer array
> nums_int = np.array([1, 2, 3])
> print(nums_int)         # array([1, 2, 3])
> print(nums_int.dtype)   # dtype('int64')
> 
> # Mixed input → automatic conversion to float
> nums_mixed = np.array([1, 2.5, 3])
> print(nums_mixed)       # array([1., 2.5, 3.])
> print(nums_mixed.dtype) # dtype('float64')
> ```
> 
> **What happened:**
> 
> - Mixed input had int (1, 3) and float (2.5)
> - NumPy converted all to float (most general type)
> - Result: All elements are floats
> 
> **Why it matters:** Homogeneity allows NumPy to be memory-efficient and fast.

**Q52.** Explain NumPy broadcasting and compare it to list alternatives for squaring numbers.

> [!success]- Answer **Broadcasting:** Applying operations to entire arrays without explicit loops.
> 
> **Problem: Lists don't support broadcasting**
> 
> ```python
> nums = [-2, -1, 0, 1, 2]
> nums ** 2  # TypeError!
> ```
> 
> **List Alternative 1 - For loop (inefficient):**
> 
> ```python
> sqrd = []
> for num in nums:
>     sqrd.append(num ** 2)
> # [4, 1, 0, 1, 4]
> ```
> 
> **List Alternative 2 - List comprehension (better but not best):**
> 
> ```python
> sqrd = [num ** 2 for num in nums]
> # [4, 1, 0, 1, 4]
> ```
> 
> **NumPy Broadcasting (best!):**
> 
> ```python
> import numpy as np
> nums_np = np.array([-2, -1, 0, 1, 2])
> nums_np ** 2
> # array([4, 1, 0, 1, 4])
> ```
> 
> **Advantages:**
> 
> - Most concise
> - Fastest execution
> - Most memory efficient
> - No explicit loops needed

**Q53.** Compare 2-D indexing syntax between lists and NumPy arrays. Which is cleaner and why?

> [!success]- Answer **Lists - Clunky syntax:**
> 
> ```python
> nums_list = [[1, 2, 3],
>              [4, 5, 6]]
> 
> # Access element at row 0, column 1
> nums_list[0][1]  # 2
> 
> # Get first column (requires list comprehension)
> [row[0] for row in nums_list]  # [1, 4]
> ```
> 
> **NumPy Arrays - Elegant syntax:**
> 
> ```python
> import numpy as np
> nums_np = np.array([[1, 2, 3],
>                     [4, 5, 6]])
> 
> # Access element at row 0, column 1
> nums_np[0, 1]  # 2
> 
> # Get first column (simple!)
> nums_np[:, 0]  # array([1, 4])
> ```
> 
> **NumPy is cleaner because:**
> 
> - Single bracket notation: `[row, col]` vs `[row][col]`
> - Slice syntax for columns: `[:, 0]` vs list comprehension
> - More intuitive and readable
> - Matches mathematical notation

**Q54.** Explain boolean indexing in NumPy. How does it compare to filtering lists?

> [!success]- Answer **Boolean Indexing:** Filtering NumPy arrays using boolean conditions.
> 
> **NumPy Boolean Indexing:**
> 
> ```python
> import numpy as np
> nums_np = np.array([-2, -1, 0, 1, 2])
> 
> # Create boolean mask
> mask = nums_np > 0
> # array([False, False, False, True, True])
> 
> # Apply mask to filter
> positive = nums_np[nums_np > 0]
> # array([1, 2])
> ```
> 
> **List Alternative 1 - For loop:**
> 
> ```python
> nums = [-2, -1, 0, 1, 2]
> pos = []
> for num in nums:
>     if num > 0:
>         pos.append(num)
> # [1, 2]
> ```
> 
> **List Alternative 2 - List comprehension:**
> 
> ```python
> pos = [num for num in nums if num > 0]
> # [1, 2]
> ```
> 
> **NumPy advantages:**
> 
> - More concise: `arr[arr > 0]`
> - Faster for large arrays
> - Can combine conditions: `arr[(arr > 0) & (arr < 5)]`
> - More readable for complex filters

**Q55.** List 5 key differences between Python lists and NumPy arrays.

> [!success]- Answer **1. Data Types:**
> 
> - **Lists:** Can contain mixed types
> - **Arrays:** Homogeneous (single type only)
> 
> **2. Broadcasting:**
> 
> - **Lists:** Not supported (`[1,2,3] ** 2` → TypeError)
> - **Arrays:** Supported (`arr ** 2` works)
> 
> **3. 2-D Indexing:**
> 
> - **Lists:** `list[row][col]` - clunky
> - **Arrays:** `arr[row, col]` - elegant
> 
> **4. Boolean Indexing:**
> 
> - **Lists:** Not available
> - **Arrays:** Available (`arr[arr > 0]`)
> 
> **5. Performance:**
> 
> - **Lists:** Slower for numerical operations
> - **Arrays:** Faster, optimized for numerical work
> 
> **Bonus - Memory:**
> 
> - **Lists:** More memory (stores type per element)
> - **Arrays:** Less memory (fixed type)

---

## Section D: Code Analysis & Scenarios (25 points)

_2.5 points each_

**Q56.** What does this code output?

```python
nums = list(range(1, 10, 2))
print(nums)
```

> [!success]- Answer **Output:** `[1, 3, 5, 7, 9]`
> 
> **Explanation:**
> 
> - Start: 1
> - Stop: 10 (exclusive)
> - Step: 2
> - Result: Odd numbers from 1 to 9

**Q57.** What does this code output?

```python
letters = ['x', 'y', 'z']
result = list(enumerate(letters, start=10))
print(result)
```

> [!success]- Answer **Output:** `[(10, 'x'), (11, 'y'), (12, 'z')]`
> 
> **Explanation:**
> 
> - `enumerate()` creates (index, value) tuples
> - `start=10` makes first index 10
> - Indices increment: 10, 11, 12

**Q58.** What's the output?

```python
nums = [2, 4, 6, 8]
result = list(map(lambda x: x / 2, nums))
print(result)
```

> [!success]- Answer **Output:** `[1.0, 2.0, 3.0, 4.0]`
> 
> **Explanation:**
> 
> - Lambda divides each element by 2
> - `map()` applies to all elements
> - Division creates floats

**Q59.** What happens when you run this code?

```python
nums_list = [1, 2, 3]
result = nums_list * 2
print(result)
```

> [!success]- Answer **Output:** `[1, 2, 3, 1, 2, 3]`
> 
> **Explanation:**
> 
> - List multiplication repeats the list
> - NOT element-wise multiplication
> - To multiply elements, use list comprehension or NumPy

**Q60.** What does this NumPy code output?

```python
import numpy as np
arr = np.array([1, 2, 3])
result = arr * 2
print(result)
```

> [!success]- Answer **Output:** `array([2, 4, 6])`
> 
> **Explanation:**
> 
> - NumPy supports broadcasting
> - Multiplies each element by 2
> - Element-wise operation (unlike lists)

**Q61.** What's the output?

```python
import numpy as np
arr = np.array([1, 2.5, 3])
print(arr.dtype)
```

> [!success]- Answer **Output:** `dtype('float64')`
> 
> **Explanation:**
> 
> - Mixed input: integers (1, 3) and float (2.5)
> - NumPy converts all to most general type (float)
> - Maintains homogeneity

**Q62.** What does this code do and what's the output?

```python
import numpy as np
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
result = arr_2d[:, 1]
print(result)
```

> [!success]- Answer **Output:** `array([2, 5])`
> 
> **What it does:** Gets the second column (index 1)
> 
> **Explanation:**
> 
> - `:` means "all rows"
> - `1` means "column index 1"
> - Result: [2, 5] (second column values)

**Q63.** What's the output?

```python
import numpy as np
arr = np.array([-3, -1, 0, 2, 5])
result = arr[arr > 0]
print(result)
```

> [!success]- Answer **Output:** `array([2, 5])`
> 
> **Explanation:**
> 
> - `arr > 0` creates boolean mask: [False, False, False, True, True]
> - Boolean indexing returns only True elements
> - Result: positive values only

**Q64.** Debug this code - what's wrong and how do you fix it?

```python
nums = [1, 2, 3, 4, 5]
squared = nums ** 2
print(squared)
```

> [!success]- Answer **Problem:** Lists don't support broadcasting. This raises TypeError.
> 
> **Fix Option 1 - List comprehension:**
> 
> ```python
> nums = [1, 2, 3, 4, 5]
> squared = [x ** 2 for x in nums]
> print(squared)
> # [1, 4, 9, 16, 25]
> ```
> 
> **Fix Option 2 - NumPy (best):**
> 
> ```python
> import numpy as np
> nums = np.array([1, 2, 3, 4, 5])
> squared = nums ** 2
> print(squared)
> # array([1, 4, 9, 16, 25])
> ```

**Q65.** Complete this code to get all even numbers from the array:

```python
import numpy as np
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
evens = arr[______]
print(evens)
```

> [!success]- Answer **Completed code:**
> 
> ```python
> import numpy as np
> arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
> evens = arr[arr % 2 == 0]
> print(evens)
> # Output: array([2, 4, 6, 8, 10])
> ```
> 
> **Explanation:**
> 
> - `arr % 2 == 0` creates boolean mask for even numbers
> - Boolean indexing filters the array
> - Returns only even values

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)

1. B | 2. B | 3. B | 4. C | 5. B | 6. B | 7. B | 8. A | 9. A | 10. C
2. B | 12. D | 13. B | 14. B | 15. C | 16. C | 17. B | 18. B | 19. B | 20. B
3. C | 22. C | 23. B | 24. D | 25. B | 26. B | 27. B | 28. B | 29. C | 30. C

### Section B: True/False (15 points)

31. T | 32. F | 33. F | 34. T | 35. F | 36. F | 37. F | 38. T | 39. F | 40. F
32. F | 42. F | 43. T | 44. T | 45. F

### Section C: Short Answer (30 points)

Questions 46-55: Award 3 points each for complete, accurate responses with code examples.

### Section D: Code Analysis (25 points)

Questions 56-65: Award 2.5 points each for correct analysis and explanations.

**Total: 130 points** **Passing Score: 91 points (70%)**

---

## Quick Study Guide

### Built-in Functions Reference

```python
# range(start, stop, step)
range(5)           # [0, 1, 2, 3, 4]
range(1, 6)        # [1, 2, 3, 4, 5]
range(2, 11, 2)    # [2, 4, 6, 8, 10]

# enumerate(iterable, start=0)
list(enumerate(['a', 'b', 'c']))
# [(0, 'a'), (1, 'b'), (2, 'c')]

list(enumerate(['a', 'b'], start=5))
# [(5, 'a'), (6, 'b')]

# map(function, iterable)
list(map(round, [1.5, 2.3, 3.4]))
# [2, 2, 3]

list(map(lambda x: x**2, [1, 2, 3]))
# [1, 4, 9]
```

### NumPy Operations Reference

```python
import numpy as np

# Create arrays
arr = np.array([1, 2, 3])

# Check data type
arr.dtype  # dtype('int64')

# Broadcasting
arr * 2    # array([2, 4, 6])
arr ** 2   # array([1, 4, 9])

# 2-D indexing
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
arr_2d[0, 1]   # 2 (row 0, col 1)
arr_2d[:, 0]   # array([1, 4]) (all rows, col 0)
arr_2d[0, :]   # array([1, 2, 3]) (row 0, all cols)

# Boolean indexing
arr = np.array([-2, -1, 0, 1, 2])
arr > 0        # array([False, False, False, True, True])
arr[arr > 0]   # array([1, 2])
```

---

## Key Concepts Summary

### Efficiency Definition

|Aspect|Meaning|
|---|---|
|**Fast Runtime**|Minimal completion time|
|**Small Memory**|Minimal resource consumption|

### Pythonic vs Non-Pythonic

|Approach|Example|
|---|---|
|**Non-Pythonic**|`for i in range(len(nums)): result.append(nums[i]*2)`|
|**Pythonic**|`result = [x * 2 for x in nums]`|

### Lists vs NumPy Arrays

|Feature|Lists|NumPy Arrays|
|---|---|---|
|**Data Types**|Mixed|Homogeneous|
|**Broadcasting**|❌|✅|
|**2-D Indexing**|`[row][col]`|`[row, col]`|
|**Boolean Index**|❌|✅|
|**Performance**|Slower|Faster|

### Built-in Functions

|Function|Purpose|Returns|
|---|---|---|
|`range()`|Generate number sequences|range object|
|`enumerate()`|Add indices to iterable|enumerate object|
|`map()`|Apply function to iterable|map object|

---

## Common Mistakes to Avoid

❌ **Forgetting range() is exclusive of stop value**

```python
# WRONG assumption
range(10)  # Does NOT include 10!

# CORRECT
range(10)  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

❌ **Assuming map() returns a list**

```python
# WRONG
result = map(round, [1.5, 2.5])
print(result)  # <map object> not helpful!

# CORRECT
result = list(map(round, [1.5, 2.5]))
print(result)  # [2, 2]
```

❌ **Trying to broadcast with lists**

```python
# WRONG
nums = [1, 2, 3]
nums ** 2  # TypeError!

# CORRECT - Use NumPy
import numpy as np
nums = np.array([1, 2, 3])
nums ** 2  # array([1, 4, 9])
```

❌ **Confusing list multiplication with broadcasting**

```python
# List multiplication repeats list
[1, 2, 3] * 2  # [1, 2, 3, 1, 2, 3]

# NumPy multiplies elements
np.array([1, 2, 3]) * 2  # array([2, 4, 6])
```

❌ **Wrong enumerate() tuple order**

```python
# WRONG assumption
enumerate(['a', 'b'])  # Returns (value, index)? NO!

# CORRECT
enumerate(['a', 'b'])  # Returns (index, value)
# [(0, 'a'), (1, 'b')]
```

---

## Study Strategy

### Day 1: Foundations

- [ ] Understand efficiency definition
- [ ] Learn Pythonic principles
- [ ] Read The Zen of Python (`import this`)
- [ ] Practice identifying Pythonic vs non-Pythonic code

### Day 2: Built-in Functions

- [ ] Master `range()` with all parameters
- [ ] Practice `enumerate()` with default and custom start
- [ ] Learn `map()` with functions and lambdas
- [ ] Remember: these return objects, not lists!

### Day 3: NumPy Basics

- [ ] Understand array homogeneity
- [ ] Learn array creation
- [ ] Practice checking `.dtype`
- [ ] Understand automatic type conversion

### Day 4: NumPy Operations

- [ ] Master broadcasting concept
- [ ] Practice element-wise operations
- [ ] Compare with list alternatives
- [ ] Understand performance benefits

### Day 5: NumPy Indexing

- [ ] Practice 1-D indexing
- [ ] Master 2-D indexing syntax
- [ ] Learn slicing with `:` notation
- [ ] Understand row vs column selection

### Day 6: Boolean Indexing

- [ ] Create boolean masks
- [ ] Apply masks for filtering
- [ ] Combine multiple conditions
- [ ] Compare with list comprehensions

### Day 7: Review & Practice

- [ ] Complete all practice problems
- [ ] Review common mistakes
- [ ] Take this practice quiz
- [ ] Focus on weak areas

---

## Final Exam Checklist

### Before the Exam ✓

**Efficiency & Pythonic Code:**

- [ ] Know two aspects of efficiency
- [ ] Can identify Pythonic vs non-Pythonic code
- [ ] Understand Zen of Python principles

**Built-in Functions:**

- [ ] Know `range()` syntax: (start, stop, step)
- [ ] Remember range is exclusive of stop
- [ ] Know `enumerate()` returns (index, value)
- [ ] Understand `map()` returns map object
- [ ] Can use lambda functions

**NumPy Arrays:**

- [ ] Understand homogeneity
- [ ] Know broadcasting concept
- [ ] Can use 2-D indexing: [row, col]
- [ ] Master boolean indexing
- [ ] Know when to use NumPy vs lists

### During the Exam ✓

**Quick Checks:**

- [ ] `range(10)` stops at 9, not 10
- [ ] `enumerate()` gives (index, value), not (value, index)
- [ ] `map()` needs `list()` conversion
- [ ] Lists can't broadcast: `[1,2,3] ** 2` = TypeError
- [ ] NumPy can broadcast: `np.array([1,2,3]) ** 2` = works
- [ ] `arr[:, 0]` = first column, `arr[0, :]` = first row
- [ ] Boolean indexing: `arr[arr > 0]` filters array

**Common Traps:**

- [ ] Range exclusive of stop
- [ ] Map returns object, not list
- [ ] Enumerate tuple order
- [ ] List vs array multiplication
- [ ] 2-D indexing syntax differences

---

## Zen of Python Highlights

> Beautiful is better than ugly. Explicit is better than implicit. Simple is better than complex. Flat is better than nested. Readability counts.

**Remember:** Type `import this` in Python to see the full Zen!

---

## Congratulations! 🎉

You've mastered:

- ✅ Defining efficient and Pythonic code
- ✅ Using built-in functions: range(), enumerate(), map()
- ✅ Creating and manipulating NumPy arrays
- ✅ Broadcasting for element-wise operations
- ✅ Advanced indexing techniques
- ✅ Boolean filtering in NumPy

**You're ready to write efficient Python code!** 🐍⚡

---

#python #efficiency #numpy #built-ins #pythonic #datacamp #performance #optimization #foundations