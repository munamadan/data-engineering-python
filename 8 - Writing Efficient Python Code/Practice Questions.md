
## Section A: Multiple Choice Questions
**50 questions | 2 points each = 100 points**

### Chapter 1: Foundations (12 questions)

**Q1.** What are the two key aspects of efficient Python code?
- [ ] A) Short code and fast typing
- [ ] B) Minimal completion time and minimal resource consumption
- [ ] C) Readability and comments
- [ ] D) Using loops and functions

> [!success]- Answer
> **Correct Answer: B) Minimal completion time and minimal resource consumption**
> 
> Efficient code has fast runtime (minimal completion time) and small memory footprint (minimal resource consumption).

**Q2.** What does `range(2, 11, 2)` produce?
- [ ] A) [2, 4, 6, 8, 10]
- [ ] B) [2, 4, 6, 8, 10, 12]
- [ ] C) [0, 2, 4, 6, 8, 10]
- [ ] D) [2, 3, 4, 5, 6, 7, 8, 9, 10]

> [!success]- Answer
> **Correct Answer: A) [2, 4, 6, 8, 10]**
> 
> `range(start, stop, step)` with step=2 creates even numbers from 2 to 10 (11 is excluded).

**Q3.** What does `enumerate(['a', 'b', 'c'])` return?
- [ ] A) ['a', 'b', 'c']
- [ ] B) [('a', 0), ('b', 1), ('c', 2)]
- [ ] C) [(0, 'a'), (1, 'b'), (2, 'c')]
- [ ] D) {'a': 0, 'b': 1, 'c': 2}

> [!success]- Answer
> **Correct Answer: C) [(0, 'a'), (1, 'b'), (2, 'c')]**
> 
> `enumerate()` returns tuples of (index, value), with index first.

**Q4.** What does broadcasting mean in NumPy?
- [ ] A) Sending data over a network
- [ ] B) Applying operations to entire arrays without explicit loops
- [ ] C) Copying arrays
- [ ] D) Converting arrays to lists

> [!success]- Answer
> **Correct Answer: B) Applying operations to entire arrays without explicit loops**
> 
> Broadcasting allows element-wise operations on entire arrays at once.

**Q5.** What happens when you try `[1, 2, 3] ** 2` with a Python list?
- [ ] A) Returns [1, 4, 9]
- [ ] B) Returns [1, 2, 3, 1, 2, 3]
- [ ] C) TypeError
- [ ] D) Returns [[1, 2, 3], [1, 2, 3]]

> [!success]- Answer
> **Correct Answer: C) TypeError**
> 
> Lists don't support broadcasting. You get: "unsupported operand type(s) for ** or pow(): 'list' and 'int'".

**Q6.** How do you get the first column of a 2-D NumPy array?
- [ ] A) `arr[0]`
- [ ] B) `arr[:, 0]`
- [ ] C) `arr[0, :]`
- [ ] D) `[row[0] for row in arr]`

> [!success]- Answer
> **Correct Answer: B) arr[:, 0]**
> 
> `:` means "all rows", `0` means "first column". Result: all elements in first column.

**Q7.** What does `map(lambda x: x ** 2, [1, 2, 3])` return?
- [ ] A) [1, 4, 9]
- [ ] B) A map object
- [ ] C) [2, 4, 6]
- [ ] D) 14

> [!success]- Answer
> **Correct Answer: B) A map object**
> 
> `map()` returns a map object. Use `list()` to convert: `list(map(lambda x: x**2, [1, 2, 3]))` gives [1, 4, 9].

**Q8.** Which is faster for creating a dictionary?
- [ ] A) formal_dict = dict()
- [ ] B) literal_dict = {}
- [ ] C) Both are the same speed
- [ ] D) Depends on dictionary size

> [!success]- Answer
> **Correct Answer: B) literal_dict = {}**
> 
> Literal syntax `{}` is faster (~52 ns faster) than formal syntax `dict()`.

**Q9.** What is a key advantage of NumPy arrays over Python lists?
- [ ] A) Arrays can store strings, lists cannot
- [ ] B) Arrays are homogeneous and support broadcasting
- [ ] C) Lists are faster than arrays
- [ ] D) Arrays can be indexed, lists cannot

> [!success]- Answer
> **Correct Answer: B) Arrays are homogeneous and support broadcasting**
> 
> NumPy arrays must have one data type and support element-wise operations without loops.

**Q10.** Which Zen of Python principle applies to nested code?
- [ ] A) Sparse is better than dense
- [ ] B) Flat is better than nested
- [ ] C) Simple is better than complex
- [ ] D) Explicit is better than implicit

> [!success]- Answer
> **Correct Answer: B) Flat is better than nested**
> 
> "Flat is better than nested" encourages avoiding deeply nested code structures.

**Q11.** What does `arr[arr > 0]` do in NumPy (boolean indexing)?
- [ ] A) Returns True/False values
- [ ] B) Returns only elements greater than 0
- [ ] C) Returns indices of elements > 0
- [ ] D) Modifies arr to remove values <= 0

> [!success]- Answer
> **Correct Answer: B) Returns only elements greater than 0**
> 
> Boolean indexing filters the array, returning only elements where the condition is True.

**Q12.** Which uses less memory for 1000 numbers?
- [ ] A) Python list
- [ ] B) NumPy array
- [ ] C) Both use the same
- [ ] D) Depends on the numbers

> [!success]- Answer
> **Correct Answer: B) NumPy array**
> 
> NumPy arrays use less memory due to homogeneous data types and efficient storage.

### Chapter 2: Timing & Profiling (13 questions)

**Q13.** What character prefixes IPython magic commands?
- [ ] A) #
- [ ] B) %
- [ ] C) $
- [ ] D) @

> [!success]- Answer
> **Correct Answer: B) %**
> 
> Magic commands are prefixed by `%`. Single `%` for line magic, double `%%` for cell magic.

**Q14.** In `%timeit` output `8.61 μs ± 69.1 ns per loop`, what does `8.61 μs` represent?
- [ ] A) Total time for all runs
- [ ] B) Average time per loop
- [ ] C) Fastest time recorded
- [ ] D) Standard deviation

> [!success]- Answer
> **Correct Answer: B) Average time per loop**
> 
> `8.61 μs` is the mean (average) time per loop execution.

**Q15.** How do you save %timeit output to a variable?
- [ ] A) %timeit code > var
- [ ] B) var = %timeit -o code
- [ ] C) %timeit -s var code
- [ ] D) %timeit code --save var

> [!success]- Answer
> **Correct Answer: B) var = %timeit -o code**
> 
> The `-o` flag saves output: `times = %timeit -o code`

**Q16.** What's the difference between `%timeit` and `%%timeit`?
- [ ] A) No difference
- [ ] B) % is for single lines, %% is for multiple lines (cell)
- [ ] C) % is faster than %%
- [ ] D) %% is deprecated

> [!success]- Answer
> **Correct Answer: B) % is for single lines, %% is for multiple lines (cell)**
> 
> Single `%` (line magic) times one line, double `%%` (cell magic) times multiple lines.

**Q17.** What package provides line-by-line runtime profiling?
- [ ] A) time_profiler
- [ ] B) line_profiler
- [ ] C) runtime_profiler
- [ ] D) code_profiler

> [!success]- Answer
> **Correct Answer: B) line_profiler**
> 
> Install with: `pip install line_profiler`

**Q18.** What's the correct syntax for using line profiler?
- [ ] A) %lprun function_name
- [ ] B) %lprun -f function_name function_call
- [ ] C) %lprun function_call
- [ ] D) %profile function_name

> [!success]- Answer
> **Correct Answer: B) %lprun -f function_name function_call**
> 
> Syntax: `%lprun -f function_to_profile actual_function_call()`

**Q19.** In %lprun output, what does "% Time" show?
- [ ] A) Time in seconds
- [ ] B) Percentage of total function time spent on that line
- [ ] C) Time accuracy percentage
- [ ] D) Loop completion percentage

> [!success]- Answer
> **Correct Answer: B) Percentage of total function time spent on that line**
> 
> "% Time" helps identify bottlenecks - lines taking most time.

**Q20.** What's a quick way to check object size in bytes?
- [ ] A) len()
- [ ] B) sys.getsizeof()
- [ ] C) size()
- [ ] D) memory()

> [!success]- Answer
> **Correct Answer: B) sys.getsizeof()**
> 
> `sys.getsizeof(obj)` returns size in bytes.

**Q21.** What's special about using %mprun?
- [ ] A) Nothing special
- [ ] B) Functions must be imported from external files
- [ ] C) It only works in Python 2
- [ ] D) It requires NumPy

> [!success]- Answer
> **Correct Answer: B) Functions must be imported from external files**
> 
> Unlike %lprun, %mprun requires: `from file import function`

**Q22.** In %mprun output, what does "Increment" show?
- [ ] A) Total memory used
- [ ] B) Memory added by that specific line
- [ ] C) Memory freed
- [ ] D) Memory percentage

> [!success]- Answer
> **Correct Answer: B) Memory added by that specific line**
> 
> "Increment" shows new memory allocated. Key for finding memory hogs.

**Q23.** When should you use %lprun vs %timeit?
- [ ] A) Always use %timeit
- [ ] B) Use %timeit for overall comparison, %lprun to find bottlenecks
- [ ] C) Always use %lprun
- [ ] D) They do the same thing

> [!success]- Answer
> **Correct Answer: B) Use %timeit for overall comparison, %lprun to find bottlenecks**
> 
> %timeit: overall benchmark. %lprun: line-by-line detail for optimization.

**Q24.** What property gives the best time from saved %timeit output?
- [ ] A) times.fastest
- [ ] B) times.min
- [ ] C) times.best
- [ ] D) times.minimum

> [!success]- Answer
> **Correct Answer: C) times.best**
> 
> Access best time with `times.best`, worst with `times.worst`.

**Q25.** Why might %mprun show 0.0 MiB for small datasets?
- [ ] A) It's broken
- [ ] B) Small allocations may not register due to limited precision
- [ ] C) Memory wasn't used
- [ ] D) Wrong units

> [!success]- Answer
> **Correct Answer: B) Small allocations may not register due to limited precision**
> 
> Memory profiler has limited precision. Use larger datasets for meaningful results.

### Chapter 3: Collections, Itertools & Sets (13 questions)

**Q26.** What does the `zip()` function return?
- [ ] A) A list of tuples
- [ ] B) A zip object (iterator)
- [ ] C) A dictionary
- [ ] D) A set of tuples

> [!success]- Answer
> **Correct Answer: B) A zip object (iterator)**
> 
> `zip()` returns an iterator. Use `[*zip_obj]` or `list()` to convert to list.

**Q27.** Which module contains the `Counter` class?
- [ ] A) itertools
- [ ] B) collections
- [ ] C) functools
- [ ] D) typing

> [!success]- Answer
> **Correct Answer: B) collections**
> 
> `Counter` is part of the `collections` module for counting hashable objects.

**Q28.** Which module contains the `combinations()` function?
- [ ] A) collections
- [ ] B) functools
- [ ] C) itertools
- [ ] D) math

> [!success]- Answer
> **Correct Answer: C) itertools**
> 
> `combinations()` is part of the `itertools` module.

**Q29.** Which set method returns elements in both sets?
- [ ] A) union()
- [ ] B) difference()
- [ ] C) intersection()
- [ ] D) symmetric_difference()

> [!success]- Answer
> **Correct Answer: C) intersection()**
> 
> `intersection()` returns elements that exist in both sets.

**Q30.** What does `symmetric_difference()` return?
- [ ] A) Elements in both sets
- [ ] B) Elements in the first set only
- [ ] C) Elements in exactly one set
- [ ] D) All elements from both sets

> [!success]- Answer
> **Correct Answer: C) Elements in exactly one set**
> 
> `symmetric_difference()` returns elements in either set but not both.

**Q31.** Which data structure provides fastest membership testing?
- [ ] A) List
- [ ] B) Tuple
- [ ] C) Set
- [ ] D) Dictionary

> [!success]- Answer
> **Correct Answer: C) Set**
> 
> Sets use hash tables, making membership testing ~200x faster than lists/tuples.

**Q32.** What are the three categories of itertools functions?
- [ ] A) Fast, slow, and medium iterators
- [ ] B) Infinite, finite, and combination generators
- [ ] C) Simple, complex, and nested iterators
- [ ] D) Sequential, parallel, and recursive iterators

> [!success]- Answer
> **Correct Answer: B) Infinite, finite, and combination generators**
> 
> Categories: infinite (count, cycle, repeat), finite (accumulate, chain), combinations (product, permutations, combinations).

**Q33.** Which approach is fastest for calculating row sums?
- [ ] A) For loop with sum()
- [ ] B) List comprehension
- [ ] C) Built-in map() function
- [ ] D) All equally fast

> [!success]- Answer
> **Correct Answer: C) Built-in map() function**
> 
> Performance: For loop (140 μs), List comp (114 μs), map() (95 μs - fastest).

**Q34.** How much faster is NumPy's `.mean(axis=1)` than looping?
- [ ] A) 2x faster
- [ ] B) 10x faster
- [ ] C) 50x faster
- [ ] D) 240x faster

> [!success]- Answer
> **Correct Answer: D) 240x faster**
> 
> Loop: 5.54 ms, NumPy: 23.1 μs. Approximately 240x faster!

**Q35.** What does "holistic conversion" mean?
- [ ] A) Converting data types one at a time inside loop
- [ ] B) Converting all data at once outside the loop
- [ ] C) Using multiple conversion functions
- [ ] D) Converting data before program starts

> [!success]- Answer
> **Correct Answer: B) Converting all data at once outside the loop**
> 
> Collect data first, then convert all items using `map(list, tuples)`.

**Q36.** What is the main principle for writing better loops?
- [ ] A) Make loops as complex as possible
- [ ] B) Anything done once should be outside the loop
- [ ] C) Always use nested loops
- [ ] D) Calculate everything inside loop for clarity

> [!success]- Answer
> **Correct Answer: B) Anything done once should be outside the loop**
> 
> Move one-time calculations above loop, holistic conversions below loop.

**Q37.** What are the five notable collections datatypes?
- [ ] A) list, tuple, set, dict, array
- [ ] B) namedtuple, deque, Counter, OrderedDict, defaultdict
- [ ] C) count, cycle, repeat, accumulate, chain
- [ ] D) map, filter, reduce, zip, enumerate

> [!success]- Answer
> **Correct Answer: B) namedtuple, deque, Counter, OrderedDict, defaultdict**
> 
> These are the specialized datatypes in the `collections` module.

**Q38.** What's the most efficient way to find unique values in a list?
- [ ] A) Loop and check if item already in result
- [ ] B) Convert list to a set
- [ ] C) Sort and remove adjacent duplicates
- [ ] D) Use nested loop

> [!success]- Answer
> **Correct Answer: B) Convert the list to a set**
> 
> `unique_types_set = set(primary_types)` is fastest and simplest.

### Chapter 4: pandas DataFrame Iteration (12 questions)

**Q39.** What is pandas' main data structure?
- [ ] A) Array
- [ ] B) DataFrame
- [ ] C) Series
- [ ] D) Matrix

> [!success]- Answer
> **Correct Answer: B) DataFrame**
> 
> DataFrame is tabular data with labeled rows and columns, built on NumPy arrays.

**Q40.** Which iteration method is slowest for pandas DataFrames?
- [ ] A) .itertuples()
- [ ] B) .apply()
- [ ] C) .iloc[] with range(len(df))
- [ ] D) .iterrows()

> [!success]- Answer
> **Correct Answer: C) .iloc[] with range(len(df))**
> 
> `.iloc[]` with range is slowest at 183 ms.

**Q41.** What does `.iterrows()` return for each iteration?
- [ ] A) A named tuple
- [ ] B) A tuple of (index, Series)
- [ ] C) A NumPy array
- [ ] D) A dictionary

> [!success]- Answer
> **Correct Answer: B) A tuple of (index, Series)**
> 
> Returns tuple where first element is index, second is a pandas Series.

**Q42.** How do you access column values with `.itertuples()`?
- [ ] A) row['column_name']
- [ ] B) row.get('column_name')
- [ ] C) row.column_name
- [ ] D) row[column_name]

> [!success]- Answer
> **Correct Answer: C) row.column_name**
> 
> `.itertuples()` returns named tuples, use dot notation.

**Q43.** How much faster is `.itertuples()` than `.iterrows()`?
- [ ] A) 2x faster
- [ ] B) 10x faster
- [ ] C) 70x faster
- [ ] D) Same speed

> [!success]- Answer
> **Correct Answer: C) 70x faster**
> 
> `.itertuples()` (7.48 ms) is ~70x faster than `.iterrows()` (527 ms).

**Q44.** What does `axis=1` mean in `.apply()`?
- [ ] A) Apply to first column
- [ ] B) Apply function row-wise (across columns)
- [ ] C) Apply to first row
- [ ] D) Apply function column-wise (down rows)

> [!success]- Answer
> **Correct Answer: B) Apply function row-wise (across columns)**
> 
> `axis=1`: row-wise operations. `axis=0`: column-wise operations.

**Q45.** How much faster is `.apply()` than `.iterrows()`?
- [ ] A) No difference
- [ ] B) ~3x faster
- [ ] C) ~10x faster
- [ ] D) ~100x faster

> [!success]- Answer
> **Correct Answer: B) ~3x faster**
> 
> `.iterrows()`: 86.8 ms, `.apply()`: 30.1 ms. ~3x faster.

**Q46.** How do you access underlying NumPy array from DataFrame column?
- [ ] A) df['column'].array
- [ ] B) df['column'].to_numpy()
- [ ] C) df['column'].values
- [ ] D) df['column'].get_array()

> [!success]- Answer
> **Correct Answer: C) df['column'].values**
> 
> `.values` returns underlying NumPy ndarray.

**Q47.** What is vectorization performance vs `.iterrows()`?
- [ ] A) ~10x faster
- [ ] B) ~50x faster
- [ ] C) ~700x faster
- [ ] D) Same speed

> [!success]- Answer
> **Correct Answer: C) ~700x faster**
> 
> `.iterrows()`: 86.8 ms, Vectorization: 0.124 ms. ~700x faster!

**Q48.** What is the fastest method for DataFrame column operations?
- [ ] A) Using .iterrows()
- [ ] B) Using .apply()
- [ ] C) Using vectorization with .values
- [ ] D) Using .itertuples()

> [!success]- Answer
> **Correct Answer: C) Using vectorization with .values**
> 
> Vectorization is ~1,476x faster than `.iloc[]` loops.

**Q49.** Why is vectorization faster than looping?
- [ ] A) Uses less memory
- [ ] B) Leverages NumPy's optimized C implementations
- [ ] C) Runs on multiple processors
- [ ] D) Compresses data

> [!success]- Answer
> **Correct Answer: B) Leverages NumPy's optimized C implementations**
> 
> Uses C-level operations, eliminates Python loop overhead.

**Q50.** What is pandas built on top of?
- [ ] A) TensorFlow
- [ ] B) NumPy
- [ ] C) SciPy
- [ ] D) Matplotlib

> [!success]- Answer
> **Correct Answer: B) NumPy**
> 
> pandas DataFrames are built on NumPy arrays, enabling efficient operations.

---

## Section B: True/False Questions
**30 questions | 1 point each = 30 points**

### Chapters 1-2 (15 questions)

**Q51.** Efficient code means code that is both fast and uses minimal memory. **T/F**

> [!success]- Answer
> **True** - Efficiency has two aspects: minimal completion time and minimal resource consumption.

**Q52.** `range(5)` generates [1, 2, 3, 4, 5]. **T/F**

> [!success]- Answer
> **False** - `range(5)` generates [0, 1, 2, 3, 4]. Starts at 0, stops before 5.

**Q53.** `enumerate()` returns tuples of (value, index). **T/F**

> [!success]- Answer
> **False** - Returns tuples of (index, value), with index first.

**Q54.** NumPy arrays can contain mixed data types like lists. **T/F**

> [!success]- Answer
> **False** - NumPy arrays are homogeneous with single data type.

**Q55.** Broadcasting allows operations on entire NumPy arrays without loops. **T/F**

> [!success]- Answer
> **True** - Broadcasting enables element-wise operations without explicit loops.

**Q56.** Python lists support broadcasting like NumPy arrays. **T/F**

> [!success]- Answer
> **False** - Lists don't support broadcasting. `[1,2,3] ** 2` raises TypeError.

**Q57.** Literal syntax `{}` is faster than formal syntax `dict()`. **T/F**

> [!success]- Answer
> **True** - Literal syntax is ~52 nanoseconds faster.

**Q58.** Magic commands in IPython are prefixed with %. **T/F**

> [!success]- Answer
> **True** - Single `%` for line magic, double `%%` for cell magic.

**Q59.** %timeit runs code only once to measure time. **T/F**

> [!success]- Answer
> **False** - %timeit runs multiple times (loops and runs) for accurate statistics.

**Q60.** The `-o` flag in %timeit saves output to a variable. **T/F**

> [!success]- Answer
> **True** - `times = %timeit -o code` saves results to `times`.

**Q61.** line_profiler requires `%load_ext line_profiler` before use. **T/F**

> [!success]- Answer
> **True** - Must load extension first.

**Q62.** sys.getsizeof() provides line-by-line memory analysis. **T/F**

> [!success]- Answer
> **False** - Only gives total object size. Use %mprun for line-by-line.

**Q63.** NumPy arrays generally use more memory than Python lists. **T/F**

> [!success]- Answer
> **False** - NumPy arrays use less memory due to homogeneous types.

**Q64.** %mprun requires functions to be imported from external files. **T/F**

> [!success]- Answer
> **True** - Unlike %lprun, %mprun needs `from file import function`.

**Q65.** List comprehensions are generally slower than for loops. **T/F**

> [!success]- Answer
> **False** - List comprehensions are typically faster than for loops.

### Chapters 3-4 (15 questions)

**Q66.** `zip()` immediately returns a list of tuples. **T/F**

> [!success]- Answer
> **False** - Returns zip object (iterator). Must unpack with `[*zip_obj]`.

**Q67.** `Counter` is part of the `itertools` module. **T/F**

> [!success]- Answer
> **False** - `Counter` is part of `collections`, not `itertools`.

**Q68.** Sets provide much faster membership testing than lists. **T/F**

> [!success]- Answer
> **True** - Sets are ~200x faster for membership testing.

**Q69.** `intersection()` returns elements in exactly one set. **T/F**

> [!success]- Answer
> **False** - `intersection()` returns elements in BOTH sets. `symmetric_difference()` returns elements in exactly one.

**Q70.** Moving one-time calculations outside a loop can improve performance. **T/F**

> [!success]- Answer
> **True** - Key principle: calculations that don't depend on iteration should be done once outside.

**Q71.** NumPy operations on arrays are generally slower than Python loops. **T/F**

> [!success]- Answer
> **False** - NumPy is ~240x faster. Uses optimized C implementations.

**Q72.** `combinations()` generates pairs including duplicates like (A, A). **T/F**

> [!success]- Answer
> **False** - `combinations()` generates unique combinations without duplicates.

**Q73.** Holistic conversions mean converting one item at a time inside loop. **T/F**

> [!success]- Answer
> **False** - Holistic = collect first, then convert all at once outside loop.

**Q74.** `.iloc[]` with range is faster than `.iterrows()`. **T/F**

> [!success]- Answer
> **False** - `.iloc[]` (183 ms) is slower than `.iterrows()` (95.3 ms).

**Q75.** With `.itertuples()`, use bracket notation like `row['column']`. **T/F**

> [!success]- Answer
> **False** - `.itertuples()` returns named tuples, requires dot notation `row.column`.

**Q76.** `.apply()` can only be used with lambda functions. **T/F**

> [!success]- Answer
> **False** - Can use named functions, lambda functions, or built-in functions.

**Q77.** Vectorization is ~1,476x faster than `.iloc[]` loops. **T/F**

> [!success]- Answer
> **True** - Vectorization (124 μs) vs `.iloc[]` (183 ms). ~1,476x faster!

**Q78.** pandas DataFrames are built on Python lists. **T/F**

> [!success]- Answer
> **False** - Built on NumPy arrays, not lists.

**Q79.** `.values` returns underlying NumPy array of DataFrame column. **T/F**

> [!success]- Answer
> **True** - `.values` attribute returns NumPy ndarray.

**Q80.** Broadcasting and vectorization are the same concept. **T/F**

> [!success]- Answer
> **True** - Both refer to applying operations to entire arrays at once.

---

## Section C: Short Answer Questions
**14 questions | 3 points each = 42 points**

### Chapter 1 (4 questions)

**Q81.** Explain the two key aspects of efficient Python code with examples.

> [!success]- Answer
> **1. Minimal Completion Time (Fast Runtime):**
> - Code executes quickly
> - Reduces waiting time
> - Example: Using `set.intersection()` (137 ns) vs nested loops (601 ns)
> 
> **2. Minimal Resource Consumption (Small Memory Footprint):**
> - Uses less memory
> - More efficient storage
> - Example: NumPy array (8096 bytes) vs list (9112 bytes) for 1000 numbers
> 
> **Goal:** Code should be a tool for insights, not something that leaves you waiting.

**Q82.** Compare Python lists and NumPy arrays. List 4 key differences.

> [!success]- Answer
> **Key Differences:**
> 
> 1. **Data Types:**
>    - Lists: Can mix types
>    - Arrays: Homogeneous (single type)
> 
> 2. **Broadcasting:**
>    - Lists: Not supported (`[1,2,3] ** 2` → TypeError)
>    - Arrays: Supported (`arr ** 2` works)
> 
> 3. **2-D Indexing:**
>    - Lists: `list[row][col]` (clunky)
>    - Arrays: `arr[row, col]` (elegant)
> 
> 4. **Performance:**
>    - Lists: Slower for numerical ops
>    - Arrays: Faster, optimized

**Q83.** Describe the three built-in functions: `range()`, `enumerate()`, and `map()` with syntax and examples.

> [!success]- Answer
> **1. range(start, stop, step):**
> ```python
> range(2, 11, 2)  # [2, 4, 6, 8, 10]
> # Start: 2, Stop: 11 (exclusive), Step: 2
> ```
> 
> **2. enumerate(iterable, start=0):**
> ```python
> list(enumerate(['a', 'b', 'c']))
> # [(0, 'a'), (1, 'b'), (2, 'c')]
> # Returns (index, value) tuples
> ```
> 
> **3. map(function, iterable):**
> ```python
> list(map(lambda x: x**2, [1, 2, 3]))
> # [1, 4, 9]
> # Applies function to each element
> ```

**Q84.** Explain NumPy broadcasting with an example comparing lists vs arrays.

> [!success]- Answer
> **Broadcasting:** Applying operations to entire arrays without explicit loops.
> 
> **Lists - Don't Support Broadcasting:**
> ```python
> nums = [-2, -1, 0, 1, 2]
> nums ** 2  # TypeError!
> 
> # Need loop or comprehension:
> [num ** 2 for num in nums]  # [4, 1, 0, 1, 4]
> ```
> 
> **NumPy Arrays - Support Broadcasting:**
> ```python
> import numpy as np
> nums_np = np.array([-2, -1, 0, 1, 2])
> nums_np ** 2  # array([4, 1, 0, 1, 4])
> # Single operation, no loop!
> ```
> 
> **Advantages:** Most concise, fastest execution, no explicit loops.

### Chapter 2 (3 questions)

**Q85.** Explain %timeit output format and what each component means.

> [!success]- Answer
> **Output Format:**
> ```
> 8.61 μs ± 69.1 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
> ```
> 
> **Components:**
> 
> | Part | Meaning |
> |------|---------|
> | `8.61 μs` | Average time per loop (mean) |
> | `± 69.1 ns` | Standard deviation (variability) |
> | `7 runs` | Separate run executions |
> | `100000 loops` | Loops per run |
> 
> **How it works:**
> 1. Executes code 100,000 times (loops)
> 2. Repeats 7 times (runs)
> 3. Calculates mean and std dev
> 4. Reports average per loop
> 
> **Why:** Multiple runs provide reliable timing, account for system variations.

**Q86.** Compare %lprun and %mprun. What do they measure and what are their key differences?

> [!success]- Answer
> **%lprun - Runtime Profiler:**
> - **Measures:** Time spent on each line
> - **Package:** line_profiler
> - **Command:** `%lprun -f func func()`
> - **Import needed:** No
> - **Shows:** Line #, Hits, Time, Per Hit, % Time
> 
> **%mprun - Memory Profiler:**
> - **Measures:** Memory usage per line
> - **Package:** memory_profiler
> - **Command:** `%mprun -f func func()`
> - **Import needed:** YES (from external file)
> - **Shows:** Line #, Mem usage, Increment, Occurrences
> 
> **Key Difference:** %mprun REQUIRES function imported from file. %lprun doesn't.
> 
> **Use cases:**
> - %lprun: Find time bottlenecks
> - %mprun: Find memory hogs

**Q87.** Describe the workflow for optimizing code using timing and profiling tools.

> [!success]- Answer
> **Complete Optimization Workflow:**
> 
> **Step 1: Time Overall Performance**
> ```python
> %timeit my_function()
> # Get baseline: 3 μs ± 32 ns
> ```
> 
> **Step 2: Profile Runtime (if slow)**
> ```python
> %load_ext line_profiler
> %lprun -f my_function my_function()
> # Identify bottleneck lines (highest % Time)
> ```
> 
> **Step 3: Profile Memory (if needed)**
> ```python
> # Save function to file.py first
> from file import my_function
> %load_ext memory_profiler
> %mprun -f my_function my_function()
> # Find memory-intensive lines (highest Increment)
> ```
> 
> **Step 4: Optimize**
> - Fix bottleneck lines
> - Use faster alternatives
> - Eliminate loops, use vectorization
> 
> **Step 5: Re-measure**
> - Run %timeit again
> - Confirm improvement
> - Document speedup

### Chapter 3 (4 questions)

**Q88.** Explain all four set methods with examples showing what each returns.

> [!success]- Answer
> **Set Methods with Examples:**
> 
> ```python
> set_a = {'A', 'B', 'C'}
> set_b = {'B', 'C', 'D'}
> 
> # 1. intersection() - Elements in BOTH
> set_a.intersection(set_b)
> # {'B', 'C'}
> 
> # 2. difference() - Elements in first, NOT second
> set_a.difference(set_b)  # {'A'}
> set_b.difference(set_a)  # {'D'}  # Order matters!
> 
> # 3. symmetric_difference() - Elements in EXACTLY ONE
> set_a.symmetric_difference(set_b)
> # {'A', 'D'}  # Excludes common B, C
> 
> # 4. union() - ALL elements from both
> set_a.union(set_b)
> # {'A', 'B', 'C', 'D'}
> ```
> 
> **Key:** 
> - intersection = both
> - difference = directional  
> - symmetric_difference = one not both
> - union = everything

**Q89.** List the three itertools categories with example functions and explain what combinations() does.

> [!success]- Answer
> **Three itertools Categories:**
> 
> **1. Infinite Iterators:**
> - Produce values indefinitely
> - Examples: `count`, `cycle`, `repeat`
> 
> **2. Finite Iterators:**
> - Work with finite sequences
> - Examples: `accumulate`, `chain`, `zip_longest`
> 
> **3. Combination Generators:**
> - Generate combinations/permutations
> - Examples: `product`, `permutations`, `combinations`
> 
> **combinations() Explained:**
> ```python
> from itertools import combinations
> 
> items = ['A', 'B', 'C', 'D']
> combos = [*combinations(items, 2)]
> # [('A', 'B'), ('A', 'C'), ('A', 'D'),
> #  ('B', 'C'), ('B', 'D'), ('C', 'D')]
> 
> # Generates all unique 2-element pairs
> # No duplicates (A, A)
> # No repeats: (A, B) appears once, not (B, A)
> ```

**Q90.** Explain the principles of writing better loops with examples.

> [!success]- Answer
> **Key Principles:**
> 
> **1. Move one-time calculations ABOVE loop:**
> ```python
> # BAD - recalculates every iteration
> for item in items:
>     avg = calculate_average(data)  # Same each time!
>     if item > avg:
>         process(item)
> 
> # GOOD - calculate once
> avg = calculate_average(data)  # Before loop
> for item in items:
>     if item > avg:
>         process(item)
> # ~2x faster!
> ```
> 
> **2. Use holistic conversions BELOW loop:**
> ```python
> # BAD - convert individually
> results = []
> for item in items:
>     results.append(list(item))  # Convert each
> 
> # GOOD - convert all at once
> temp = []
> for item in items:
>     temp.append(item)
> results = [*map(list, temp)]  # Convert in bulk
> # ~1.2x faster!
> ```
> 
> **Main Principle:** Anything done once should be OUTSIDE the loop!

**Q91.** Compare performance of different methods for eliminating loops with specific numbers from the chapter.

> [!success]- Answer
> **Performance Comparisons:**
> 
> **1. Row Sums:**
> - For loop: 140 μs
> - List comprehension: 114 μs (1.2x faster)
> - Built-in map(): 95 μs (1.5x faster)
> 
> **2. NumPy Operations:**
> - Loop with np.mean(): 5.54 ms
> - NumPy .mean(axis=1): 23.1 μs
> - **240x faster!**
> 
> **3. Set Operations:**
> - Nested loops: 601 ns
> - Set intersection: 137 ns
> - **4.4x faster!**
> 
> **4. Membership Testing:**
> - Lists/tuples: 7.6 μs
> - Sets: 37.5 ns
> - **200x faster!**
> 
> **5. Moving Calculations Out:**
> - Inside loop: 74.9 μs
> - Outside loop: 37.5 μs
> - **2x faster!**
> 
> **Takeaway:** NumPy and sets provide dramatic speedups!

### Chapter 4 (3 questions)

**Q92.** List all five DataFrame iteration methods from fastest to slowest with approximate times.

> [!success]- Answer
> **From Fastest to Slowest:**
> 
> **1. Vectorization (NumPy arrays):** 124 μs (0.124 ms)
> ```python
> df['result'] = df['col1'].values - df['col2'].values
> ```
> 
> **2. `.apply()` method:** 30.1 ms
> ```python
> df['result'] = df.apply(lambda row: func(row['col']), axis=1)
> ```
> 
> **3. `.itertuples()`:** 7.48 ms (for printing)
> ```python
> for row in df.itertuples():
>     print(row.column)  # Dot notation
> ```
> 
> **4. `.iterrows()`:** 95.3 ms (win perc) / 86.8 ms (run diff)
> ```python
> for i, row in df.iterrows():
>     value = row['column']  # Bracket notation
> ```
> 
> **5. `.iloc[]` with range:** 183 ms
> ```python
> for i in range(len(df)):
>     row = df.iloc[i]  # AVOID - slowest!
> ```
> 
> **Speed ratios:**
> - Vectorization vs .iloc: ~1,476x faster
> - Vectorization vs .iterrows: ~700x faster
> - .itertuples vs .iterrows: ~70x faster

**Q93.** Explain the difference between accessing values with .iterrows() vs .itertuples() with code examples.

> [!success]- Answer
> **Key Difference: Return Type**
> 
> **`.iterrows()` - Returns Series:**
> ```python
> for i, row in df.iterrows():
>     # row is a pandas Series
>     team = row['Team']      # Bracket notation ✓
>     wins = row['W']
>     # row.Team              # Won't work naturally
> ```
> 
> **`.itertuples()` - Returns Named Tuple:**
> ```python
> for row in df.itertuples():
>     # row is a named tuple
>     team = row.Team         # Dot notation ✓
>     wins = row.W
>     index = row.Index
>     # team = row['Team']    # TypeError! ✗
> ```
> 
> **Critical Points:**
> - `.iterrows()`: Use **brackets** `row['col']`
> - `.itertuples()`: Use **dots** `row.col`
> - `.itertuples()` is **~70x faster**
> - Wrong notation = **Error!**

**Q94.** Explain vectorization in pandas: what it is, why it's fast, and when to use alternatives.

> [!success]- Answer
> **What is Vectorization:**
> Performing operations on entire arrays at once without explicit Python loops, leveraging NumPy's optimized C implementations.
> 
> **Why It's Fast:**
> 1. **C-level operations:** NumPy uses optimized C code
> 2. **No Python loop overhead:** Eliminates repeated bytecode execution
> 3. **Operates on entire arrays:** All elements processed simultaneously
> 4. **Optimized algorithms:** Highly efficient low-level implementations
> 
> **Example:**
> ```python
> # Vectorized - 124 μs (FAST!)
> df['run_diff'] = df['RS'].values - df['RA'].values
> 
> # Loop - 86.8 ms (SLOW!)
> for i, row in df.iterrows():
>     df.at[i, 'run_diff'] = row['RS'] - row['RA']
> # 700x slower!
> ```
> 
> **When to Use Alternatives:**
> 
> **Use `.apply()` when:**
> - Complex conditional logic
> - Custom functions needed
> - Multi-column operations with conditions
> 
> ```python
> def categorize(row):
>     if row['W'] > 90 and row['Playoffs'] == 1:
>         return 'Elite'
>     elif row['W'] > 75:
>         return 'Good'
>     return 'Rebuilding'
> 
> df['Category'] = df.apply(categorize, axis=1)
> # Can't easily vectorize this logic
> ```
> 
> **Use `.itertuples()` when:**
> - Absolutely must iterate
> - No vectorization possible
> - Need row-by-row processing
> - Still 70x faster than `.iterrows()`!

---

## Section D: Code Analysis & Debugging
**14 questions | 2 points each = 28 points**

### Mixed Concepts (14 questions)

**Q95.** What's wrong with this code? Fix it.
```python
nums = [1, 2, 3, 4, 5]
squared = nums ** 2
```

> [!success]- Answer
> **Problem:** Lists don't support broadcasting.
> 
> **Fix Option 1 - List Comprehension:**
> ```python
> squared = [x ** 2 for x in nums]
> ```
> 
> **Fix Option 2 - NumPy (Best):**
> ```python
> import numpy as np
> nums = np.array([1, 2, 3, 4, 5])
> squared = nums ** 2
> ```

**Q96.** Debug and fix this code:
```python
def slow_function():
    result = []
    for i in range(1000):
        result.append(i ** 2)
    return result

%lprun -f slow_function slow_function()
```

> [!success]- Answer
> **Problem:** Forgot to load line_profiler extension.
> 
> **Fixed:**
> ```python
> %load_ext line_profiler  # Load extension first!
> 
> def slow_function():
>     result = []
>     for i in range(1000):
>         result.append(i ** 2)
>     return result
> 
> %lprun -f slow_function slow_function()
> ```

**Q97.** Optimize this code using the fastest method:
```python
prices = [10, 20, 30, 40]
for price in prices:
    average = sum(prices) / len(prices)
    if price > average:
        print(f"${price} is above average")
```

> [!success]- Answer
> **Optimized:**
> ```python
> prices = [10, 20, 30, 40]
> average = sum(prices) / len(prices)  # Calculate once!
> for price in prices:
>     if price > average:
>         print(f"${price} is above average")
> # ~2x faster by moving calculation outside loop
> ```

**Q98.** What's the issue? Fix it.
```python
for row in df.itertuples():
    team = row['Team']
    wins = row['W']
```

> [!success]- Answer
> **Issue:** `.itertuples()` returns named tuples, can't use brackets.
> 
> **Fixed:**
> ```python
> for row in df.itertuples():
>     team = row.Team  # Dot notation
>     wins = row.W
> ```

**Q99.** Complete this to find common elements using the fastest method:
```python
list_a = ['A', 'B', 'C']
list_b = ['B', 'C', 'D']
# Complete:
common = _______
```

> [!success]- Answer
> **Completed:**
> ```python
> common = set(list_a).intersection(set(list_b))
> # Result: {'B', 'C'}
> 
> # Alternative:
> common = set(list_a) & set(list_b)
> 
> # ~4.4x faster than nested loops!
> ```

**Q100.** What will this output?
```python
import numpy as np
arr = np.array([1, 2.5, 3])
print(arr.dtype)
```

> [!success]- Answer
> **Output:** `dtype('float64')`
> 
> **Explanation:**
> - Mixed input: integers (1, 3) and float (2.5)
> - NumPy converts all to most general type (float)
> - Maintains homogeneity
> - Array becomes: `array([1., 2.5, 3.])`

**Q101.** Rewrite using vectorization (fastest method):
```python
results = []
for i, row in df.iterrows():
    result = row['A'] * 2 + row['B']
    results.append(result)
df['Result'] = results
```

> [!success]- Answer
> **Vectorized:**
> ```python
> df['Result'] = df['A'].values * 2 + df['B'].values
> 
> # Or (pandas handles vectorization):
> df['Result'] = df['A'] * 2 + df['B']
> 
> # ~1,476x faster than .iloc loops!
> ```

**Q102.** Complete using combinations:
```python
items = ['W', 'X', 'Y', 'Z']
from itertools import _______
pairs = _______
print([*pairs])
```

> [!success]- Answer
> **Completed:**
> ```python
> from itertools import combinations
> pairs = combinations(items, 2)
> print([*pairs])
> 
> # Output:
> # [('W', 'X'), ('W', 'Y'), ('W', 'Z'),
> #  ('X', 'Y'), ('X', 'Z'), ('Y', 'Z')]
> ```

**Q103.** What's the output?
```python
from collections import Counter
items = ['A', 'B', 'A', 'C', 'B', 'A']
count = Counter(items)
print(count['A'])
print(count.most_common(1))
```

> [!success]- Answer
> **Output:**
> ```python
> 3
> [('A', 3)]
> ```
> 
> **Explanation:**
> - `count['A']` returns count of 'A' = 3
> - `most_common(1)` returns list of [(element, count)] for top 1
> - 'A' appears most (3 times)

**Q104.** Fix this slow DataFrame code:
```python
total_runs = []
for i in range(len(baseball_df)):
    row = baseball_df.iloc[i]
    total = row['RS'] + row['RA']
    total_runs.append(total)
baseball_df['Total'] = total_runs
```

> [!success]- Answer
> **Optimized (vectorization):**
> ```python
> baseball_df['Total'] = baseball_df['RS'].values + baseball_df['RA'].values
> 
> # Or:
> baseball_df['Total'] = baseball_df['RS'] + baseball_df['RA']
> 
> # ~1,476x faster than .iloc[] approach!
> ```

**Q105.** What does this do? Explain each set operation.
```python
set_a = {'X', 'Y', 'Z'}
set_b = {'Y', 'Z', 'W'}

r1 = set_a.union(set_b)
r2 = set_a.difference(set_b)
r3 = set_a.symmetric_difference(set_b)
```

> [!success]- Answer
> **Results:**
> ```python
> r1 = {'X', 'Y', 'Z', 'W'}     # union
> r2 = {'X'}                      # difference
> r3 = {'X', 'W'}                 # symmetric_difference
> ```
> 
> **Explanations:**
> - **union()**: ALL elements from both = everything
> - **difference()**: In set_a but NOT in set_b = only 'X'
> - **symmetric_difference()**: In exactly ONE set = 'X' and 'W' (excludes common 'Y', 'Z')

**Q106.** Complete to calculate win percentage using fastest method:
```python
baseball_df = pd.read_csv('baseball.csv')
# Calculate: wins / games
baseball_df['WP'] = _______
```

> [!success]- Answer
> **Completed (vectorization):**
> ```python
> baseball_df['WP'] = baseball_df['W'].values / baseball_df['G'].values
> 
> # Alternative:
> baseball_df['WP'] = baseball_df['W'] / baseball_df['G']
> 
> # Both use vectorization
> # ~1,476x faster than .iloc[] loops
> # ~700x faster than .iterrows()
> ```

**Q107.** Analyze this %timeit output and compare:
```python
# Method 1
%timeit list_comp = [x**2 for x in range(100)]
# 3.5 μs ± 50 ns

# Method 2
%timeit map_result = list(map(lambda x: x**2, range(100)))
# 4.2 μs ± 60 ns
```

Which is faster and by how much?

> [!success]- Answer
> **Analysis:**
> 
> **Faster Method:** List comprehension (3.5 μs)
> 
> **Comparison:**
> - List comp: 3.5 μs
> - Map: 4.2 μs
> - Difference: 0.7 μs (700 ns)
> - **List comp is ~1.2x faster (20% faster)**
> 
> **Why:**
> - List comprehensions optimized in CPython
> - map() has lambda function call overhead
> - For simple operations, list comp often wins
> 
> **Note:** For built-in functions (without lambda), map() can be faster!

**Q108.** Debug this memory profiling code:
```python
def process_data():
    data = [i for i in range(1000)]
    return data

%mprun -f process_data process_data()
```

> [!success]- Answer
> **Problem:** Function not imported from external file!
> 
> **Fixed:**
> 
> **Step 1 - Save to file (data_funcs.py):**
> ```python
> # data_funcs.py
> def process_data():
>     data = [i for i in range(1000)]
>     return data
> ```
> 
> **Step 2 - Import and profile:**
> ```python
> from data_funcs import process_data
> %load_ext memory_profiler
> %mprun -f process_data process_data()
> ```
> 
> **Key:** %mprun requires import from file (unlike %lprun)

---

## Answer Key & Scoring

### Section A: Multiple Choice (100 points)
Q1-Q12 (Ch1): B,A,C,B,C,B,B,B,B,B,B,B
Q13-Q25 (Ch2): B,B,B,B,B,B,B,B,B,B,B,C,B
Q26-Q38 (Ch3): B,B,C,C,C,C,B,C,D,B,B,B,B
Q39-Q50 (Ch4): B,C,B,C,C,B,B,C,C,C,B,B

### Section B: True/False (30 points)
Q51-Q65: T,F,F,F,T,F,T,T,F,T,T,F,F,T,F
Q66-Q80: F,F,T,F,T,F,F,F,F,F,F,T,F,T,T

### Section C: Short Answer (42 points)
Q81-Q94: See detailed answers (3 points each)

### Section D: Code Analysis (28 points)
Q95-Q108: See detailed answers (2 points each)

**Total: 200 points**
**Passing Score: 140+ points (70%)**

---

## Comprehensive Study Guide

### Performance Benchmarks Summary

| Operation | Slow Method | Fast Method | Speedup |
|-----------|-------------|-------------|---------|
| **Dict creation** | dict() (145 ns) | {} (93 ns) | 1.6x |
| **List operations** | For loop (140 μs) | map() (95 μs) | 1.5x |
| **Set operations** | Nested loops (601 ns) | intersection() (137 ns) | 4.4x |
| **Membership test** | List (7.6 μs) | Set (37.5 ns) | 200x |
| **NumPy operations** | Loop (5.54 ms) | .mean(axis=1) (23.1 μs) | 240x |
| **DataFrame iter** | .iloc[] (183 ms) | Vectorization (124 μs) | 1,476x |
| **Pandas operations** | .iterrows() (86.8 ms) | Vectorization (124 μs) | 700x |
| **Pandas iteration** | .iterrows() (527 ms) | .itertuples() (7.48 ms) | 70x |
| **Pandas functions** | .iterrows() (86.8 ms) | .apply() (30.1 ms) | 3x |
| **Loop optimization** | Calc inside (74.9 μs) | Calc outside (37.5 μs) | 2x |

### Essential Code Patterns

```python
# CHAPTER 1: FOUNDATIONS
# Range
range(2, 11, 2)  # [2, 4, 6, 8, 10]

# Enumerate
list(enumerate(['a', 'b']))  # [(0, 'a'), (1, 'b')]

# Map
list(map(lambda x: x**2, [1,2,3]))  # [1, 4, 9]

# NumPy broadcasting
arr = np.array([1,2,3])
arr ** 2  # array([1, 4, 9])

# Boolean indexing
arr[arr > 0]  # Filter positive values

# CHAPTER 2: TIMING & PROFILING
# Basic timing
%timeit code

# Cell timing
%%timeit
line1
line2

# Save output
times = %timeit -o code
times.best, times.worst, times.average

# Runtime profiling
%load_ext line_profiler
%lprun -f func func()

# Memory profiling
from file import func
%load_ext memory_profiler
%mprun -f func func()

# CHAPTER 3: COLLECTIONS & ITERTOOLS
# Zip
[*zip(list1, list2)]

# Counter
from collections import Counter
Counter(items)

# Combinations
from itertools import combinations
[*combinations(items, 2)]

# Set operations
set_a.intersection(set_b)      # Both
set_a.difference(set_b)        # A not B
set_a.symmetric_difference(set_b)  # One not both
set_a.union(set_b)             # All

# CHAPTER 4: PANDAS
# Vectorization (FASTEST)
df['result'] = df['col1'].values - df['col2'].values

# Apply
df['result'] = df.apply(lambda row: func(row['col']), axis=1)

# Itertuples
for row in df.itertuples():
    value = row.column_name  # Dot notation

# Iterrows (avoid)
for i, row in df.iterrows():
    value = row['column_name']  # Bracket notation
```

### Key Concepts Checklist

**Chapter 1:**
- [ ] Two aspects of efficiency
- [ ] Pythonic code principles
- [ ] range(), enumerate(), map()
- [ ] NumPy broadcasting
- [ ] Boolean indexing
- [ ] Lists vs arrays

**Chapter 2:**
- [ ] Magic commands (% vs %%)
- [ ] %timeit output format
- [ ] Saving with -o flag
- [ ] %lprun for runtime
- [ ] %mprun for memory (needs import)
- [ ] Literal vs formal syntax

**Chapter 3:**
- [ ] zip() returns iterator
- [ ] Counter in collections
- [ ] combinations in itertools
- [ ] Four set methods
- [ ] Sets 200x faster for membership
- [ ] Move calculations outside loops
- [ ] Holistic conversions

**Chapter 4:**
- [ ] Five iteration methods & speeds
- [ ] .itertuples() vs .iterrows() notation
- [ ] axis=1 means row-wise
- [ ] .values returns NumPy array
- [ ] Vectorization ~1,476x faster
- [ ] When to use .apply()

### Common Mistakes Reference

❌ **Lists don't broadcast:** `[1,2,3] ** 2` → TypeError
✅ **Use NumPy:** `np.array([1,2,3]) ** 2`

❌ **Forgetting %load_ext:** Before %lprun/%mprun
✅ **Load first:** `%load_ext line_profiler`

❌ **%mprun without import:** Function must be from file
✅ **Import first:** `from file import func`

❌ **Wrong %timeit syntax:** `times = %timeit code` (no -o)
✅ **Save with -o:** `times = %timeit -o code`

❌ **Indexing zip:** `zip(a,b)[0]` → TypeError
✅ **Unpack first:** `[*zip(a,b)][0]`

❌ **Counter in itertools:** Wrong module
✅ **Counter in collections:** `from collections import Counter`

❌ **combinations in collections:** Wrong module
✅ **combinations in itertools:** `from itertools import combinations`

❌ **Bracket with .itertuples():** `row['col']` → TypeError
✅ **Dot notation:** `row.col`

❌ **Dot with .iterrows():** `row.col` doesn't work naturally
✅ **Bracket notation:** `row['col']`

❌ **Forgetting axis:** `df.apply(lambda row: ...)` defaults to axis=0
✅ **Specify axis=1:** `df.apply(lambda row: ..., axis=1)`

❌ **Calc inside loop:** Recalculating same value every iteration
✅ **Move outside:** Calculate once before loop

---

## Final Exam Strategy

### Study Schedule (7 Days)

**Day 1: Chapter 1 Foundations**
- Review efficiency definition
- Master range(), enumerate(), map()
- Practice NumPy broadcasting
- Understand boolean indexing

**Day 2: Chapter 2 Timing (Part 1)**
- Learn magic commands
- Practice %timeit
- Understand output format
- Save results with -o

**Day 3: Chapter 2 Profiling (Part 2)**
- Install line_profiler & memory_profiler
- Practice %lprun and %mprun
- Understand import requirements
- Interpret profiler output

**Day 4: Chapter 3 Collections & Itertools**
- Master zip(), Counter, combinations
- Learn all 4 set methods
- Practice set operations
- Understand performance benefits

**Day 5: Chapter 3 Loop Optimization**
- Eliminate loops techniques
- Move calculations outside
- Holistic conversions
- NumPy vectorization

**Day 6: Chapter 4 pandas**
- Five iteration methods
- .iterrows() vs .itertuples()
- .apply() with axis parameter
- Vectorization with .values

**Day 7: Review & Practice**
- Take this comprehensive exam
- Review wrong answers
- Practice weak areas
- Memorize key performance numbers

### Pre-Exam Checklist

**Knowledge Check:**
- [ ] Can explain two aspects of efficiency
- [ ] Know Zen of Python principles
- [ ] Understand broadcasting concept
- [ ] Can use all profiling tools
- [ ] Know all set methods
- [ ] Can identify bottlenecks
- [ ] Know DataFrame iteration hierarchy
- [ ] Understand vectorization

**Performance Numbers:**
- [ ] Literal syntax ~52 ns faster
- [ ] Sets ~200x faster for membership
- [ ] NumPy operations ~240x faster
- [ ] Set intersection ~4.4x faster
- [ ] .itertuples() ~70x faster than .iterrows()
- [ ] Vectorization ~700x faster than .iterrows()
- [ ] Vectorization ~1,476x faster than .iloc[]
- [ ] Moving calcs out ~2x faster

**Syntax Mastery:**
- [ ] range(start, stop, step)
- [ ] enumerate(iterable, start=0)
- [ ] map(function, iterable)
- [ ] %timeit vs %%timeit
- [ ] %lprun -f func func()
- [ ] from collections import Counter
- [ ] from itertools import combinations
- [ ] df.apply(func, axis=1)

### During the Exam

**Quick Reference Card:**

```
EFFICIENCY: Fast runtime + Small memory
PYTHONIC: Readable, uses Python constructs

BUILT-INS:
range(start, stop, step) - exclusive stop
enumerate() - returns (index, value)
map() - returns iterator, needs conversion

NUMPY:
Broadcasting - operations on entire arrays
Boolean indexing - arr[arr > 0]
Homogeneous - single data type

TIMING:
% = line, %% = cell
-o saves output
.best, .worst, .average

PROFILING:
%lprun - runtime (no import needed)
%mprun - memory (MUST import from file)
% Time - find bottlenecks
Increment - memory added

COLLECTIONS:
Counter - collections module
combinations - itertools module
zip() - returns iterator

SETS:
intersection() - both sets
difference() - first not second
symmetric_difference() - exactly one
union() - all elements
Membership: 200x faster than lists

PANDAS:
.values - get NumPy array
axis=1 - row-wise operations
.itertuples() - dot notation
.iterrows() - bracket notation
Vectorization - ALWAYS fastest
```

**Answer Strategy:**
1. Read question carefully
2. Identify chapter/topic
3. Recall key concepts
4. Check for common traps
5. Use process of elimination
6. Trust performance hierarchy

**Time Management:**
- Section A (50 MC): 60 minutes (1.2 min each)
- Section B (30 T/F): 15 minutes (0.5 min each)
- Section C (14 SA): 45 minutes (3 min each)
- Section D (14 Code): 30 minutes (2 min each)
- Review: 10 minutes
- **Total: 160 minutes (2h 40min)**

---

## Course Summary

### What You've Mastered

**Chapter 1: Foundations**
✅ Defining efficient and Pythonic code
✅ Using built-in functions effectively
✅ Understanding NumPy array advantages
✅ Broadcasting and boolean indexing

**Chapter 2: Timing & Profiling**
✅ Using magic commands
✅ Timing code with %timeit
✅ Profiling runtime with %lprun
✅ Profiling memory with %mprun
✅ Identifying performance bottlenecks

**Chapter 3: Efficient Techniques**
✅ Combining objects with zip()
✅ Counting with Counter
✅ Generating combinations with itertools
✅ Applying set theory
✅ Eliminating loops
✅ Writing better loops

**Chapter 4: pandas Optimization**
✅ Understanding iteration methods
✅ Using .apply() effectively
✅ Leveraging vectorization
✅ Choosing optimal approaches

### Key Takeaways

**Performance Hierarchy:**
1. Vectorization (NumPy/pandas)
2. Built-in functions (map, zip)
3. List comprehensions
4. Optimized loops
5. Standard loops

**Always Remember:**
- Efficiency = Speed + Memory
- Pythonic = Readable + Idiomatic
- Profile before optimizing
- Vectorize when possible
- Sets for membership testing
- NumPy for numerical operations
- pandas .values for speed

**The Golden Rules:**
1. **Don't guess, measure!** Use %timeit and profilers
2. **Vectorize first** - Always try NumPy/pandas vectorization
3. **Use built-ins** - They're optimized in C
4. **Sets for membership** - 200x faster than lists
5. **Move calculations out** - Don't repeat work in loops
6. **Import for %mprun** - Memory profiler needs external files
7. **Dot vs bracket** - .itertuples() vs .iterrows()

---

## Final Thoughts

You've learned to write efficient Python code by:
- Understanding what makes code efficient
- Using powerful built-in tools
- Leveraging NumPy and pandas
- Profiling to find bottlenecks
- Applying optimization techniques

**Remember:** The goal isn't to optimize everything, but to:
1. Write clean, readable code first
2. Profile to find actual bottlenecks
3. Optimize the slow parts
4. Measure the improvement

**Good luck on your exam!** 🎉🐍⚡

---

#python #efficiency #numpy #pandas #profiling #optimization #writing-efficient-python-code #final-exam #comprehensive #certification