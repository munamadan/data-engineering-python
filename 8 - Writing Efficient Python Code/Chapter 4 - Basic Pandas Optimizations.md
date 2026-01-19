
## Overview

This chapter focuses on best practices for iterating over pandas DataFrames, comparing different iteration methods, and leveraging pandas' built-in capabilities for efficient data manipulation.

---

## 1. pandas Recap

### What is pandas?

- **Library used for data analysis**
- **Main data structure**: DataFrame
- **DataFrame characteristics**:
    - Tabular data with labeled rows and columns
    - Built on top of the NumPy array structure

> [!important] Chapter Objective Learn best practices for iterating over a pandas DataFrame

### Sample Baseball Dataset

```python
import pandas as pd
baseball_df = pd.read_csv('baseball_stats.csv')
print(baseball_df.head())
```

|Index|Team|League|Year|RS|RA|W|G|Playoffs|
|---|---|---|---|---|---|---|---|---|
|0|ARI|NL|2012|734|688|81|162|0|
|1|ATL|NL|2012|700|600|94|162|1|
|2|BAL|AL|2012|712|705|93|162|1|
|3|BOS|AL|2012|734|806|69|162|0|
|4|CHC|NL|2012|613|759|61|162|0|

**Column Definitions:**

- RS: Runs Scored
- RA: Runs Allowed
- W: Wins
- G: Games Played

---

## 2. Iterating with `.iloc[]` (Slowest Method)

### Using `.iloc[]` for Row Access

```python
import numpy as np

def calc_win_perc(wins, games_played):
    win_perc = wins / games_played
    return np.round(win_perc, 2)

# Iterating with .iloc
win_perc_list = []
for i in range(len(baseball_df)):
    row = baseball_df.iloc[i]
    wins = row['W']
    games_played = row['G']
    win_perc = calc_win_perc(wins, games_played)
    win_perc_list.append(win_perc)

baseball_df['WP'] = win_perc_list
```

**Performance:** 183 ms ± 1.73 ms

> [!warning] Not Recommended Using `.iloc[]` with `range(len(df))` is the slowest iteration method.

---

## 3. Iterating with `.iterrows()` (Slow Method)

### Using `.iterrows()`

```python
win_perc_list = []
for i, row in baseball_df.iterrows():
    wins = row['W']
    games_played = row['G']
    win_perc = calc_win_perc(wins, games_played)
    win_perc_list.append(win_perc)

baseball_df['WP'] = win_perc_list
```

**Performance:** 95.3 ms ± 3.57 ms

### What `.iterrows()` Returns

```python
for row_tuple in team_wins_df.iterrows():
    print(row_tuple)
    print(type(row_tuple[1]))

# Output:
# (0, Team    ARI
#      Year   2012
#      W        81
#      Name: 0, dtype: object)
# <class 'pandas.core.series.Series'>
```

- Returns a tuple: `(index, Series)`
- The row data is a pandas Series object
- Access values using: `row_tuple[1]['column_name']` or `row['column_name']`

**Performance Improvement:** ~2x faster than `.iloc[]`

---

## 4. Iterating with `.itertuples()` (Faster Method)

### Using `.itertuples()`

```python
for row_namedtuple in team_wins_df.itertuples():
    print(row_namedtuple)

# Output:
# Pandas(Index=0, Team='ARI', Year=2012, W=81)
# Pandas(Index=1, Team='ATL', Year=2012, W=94)
```

### Accessing Data from Named Tuples

```python
# Correct: Access with dot notation
print(row_namedtuple.Index)  # 0
print(row_namedtuple.Team)   # 'ARI'
print(row_namedtuple.W)      # 81

# Incorrect: Cannot use bracket notation
print(row_namedtuple['Team'])  # TypeError!
```

### Performance Comparison

|Method|Time|Type Returned|
|---|---|---|
|`.iterrows()`|527 ms|pandas Series|
|`.itertuples()`|7.48 ms|Named tuple|

**Performance Improvement:** ~70x faster than `.iterrows()`!

### Key Differences

```python
# .iterrows() - use bracket notation
for i, row in team_wins_df.iterrows():
    print(row['Team'])  # Works

# .itertuples() - use dot notation
for row_namedtuple in team_wins_df.itertuples():
    print(row_namedtuple.Team)  # Works
    print(row_namedtuple['Team'])  # TypeError!
```

---

## 5. pandas `.apply()` Method (Good Alternative)

### What is `.apply()`?

- Takes a function and applies it to a DataFrame
- **Must specify an axis**:
    - `axis=0` for columns
    - `axis=1` for rows
- Can be used with **lambda functions** (anonymous functions)

### Example: Calculating Run Differential

**Function:**

```python
def calc_run_diff(runs_scored, runs_allowed):
    run_diff = runs_scored - runs_allowed
    return run_diff
```

**Loop Approach:**

```python
run_diffs_iterrows = []
for i, row in baseball_df.iterrows():
    run_diff = calc_run_diff(row['RS'], row['RA'])
    run_diffs_iterrows.append(run_diff)
baseball_df['RD'] = run_diffs_iterrows
```

**Performance:** 86.8 ms ± 3 ms

**`.apply()` Approach:**

```python
run_diffs_apply = baseball_df.apply(
    lambda row: calc_run_diff(row['RS'], row['RA']),
    axis=1
)
baseball_df['RD'] = run_diffs_apply
```

**Performance:** 30.1 ms ± 1.75 ms

**Performance Improvement:** ~3x faster than `.iterrows()`

### `.apply()` Syntax

```python
df.apply(lambda row: function(row['col1'], row['col2']), axis=1)
```

---

## 6. Optimal pandas Iterating: Vectorization (Fastest!)

### pandas Internals

- **pandas is built on NumPy**
- Take advantage of NumPy array efficiencies
- **Eliminating loops applies to pandas as well**

### Accessing Underlying NumPy Arrays

```python
wins_np = baseball_df['W'].values
print(type(wins_np))  # <class 'numpy.ndarray'>
print(wins_np)  # [ 81  94  93 ...]
```

### The Power of Vectorization (Broadcasting)

```python
# Vectorized operation - no loop needed!
baseball_df['RS'].values - baseball_df['RA'].values
# array([ 46, 100, 7, ..., 188, 110, -117])
```

### Vectorized Approach for Run Differential

```python
run_diffs_np = baseball_df['RS'].values - baseball_df['RA'].values
baseball_df['RD'] = run_diffs_np
```

**Performance:** 124 μs ± 1.47 μs

> [!tip] Broadcasting is Extremely Efficient! Vectorization leverages NumPy's optimized C implementations for array operations.

---

## 7. Performance Comparison Summary

### All Methods Compared

|Method|Time|Speed Relative to .iloc|
|---|---|---|
|`.iloc[]` with loop|183 ms|1x (baseline)|
|`.iterrows()`|95.3 ms|~2x faster|
|`.apply()` with lambda|30.1 ms|~6x faster|
|NumPy vectorization|0.124 ms (124 μs)|~1,476x faster!|

### Specific Comparisons

**For Win Percentage Calculation:**

- `.iloc[]`: 183 ms
- `.iterrows()`: 95.3 ms (**~2x faster**)

**For Run Differential Calculation:**

- `.iterrows()`: 86.8 ms
- `.apply()`: 30.1 ms (**~3x faster**)
- Vectorization: 0.124 ms (**~700x faster than .iterrows()!**)

**For Printing Rows:**

- `.iterrows()`: 527 ms
- `.itertuples()`: 7.48 ms (**~70x faster**)

---

## 8. Best Practices Summary

### Iteration Method Hierarchy (Best to Worst)

1. **🥇 Vectorization (NumPy arrays)** - Use whenever possible
    
    ```python
    df['new_col'] = df['col1'].values - df['col2'].values
    ```
    
2. **🥈 `.apply()` method** - When vectorization isn't straightforward
    
    ```python
    df['new_col'] = df.apply(lambda row: func(row['col1']), axis=1)
    ```
    
3. **🥉 `.itertuples()`** - When iteration is necessary
    
    ```python
    for row in df.itertuples():
        print(row.column_name)
    ```
    
4. **❌ `.iterrows()`** - Avoid when possible (slow)
    
    ```python
    for i, row in df.iterrows():
        print(row['column_name'])
    ```
    
5. **❌❌ `.iloc[]` with range** - Avoid (slowest)
    
    ```python
    for i in range(len(df)):
        row = df.iloc[i]
    ```
    

### Key Principles

|Principle|Recommendation|
|---|---|
|**First choice**|Use vectorization with `.values`|
|**Need custom function**|Use `.apply()`|
|**Must iterate**|Use `.itertuples()` not `.iterrows()`|
|**Accessing values**|`.itertuples()` → dot notation, `.iterrows()` → bracket notation|
|**Performance priority**|Vectorization >> `.apply()` >> `.itertuples()` >> `.iterrows()` >> `.iloc[]`|

---

## 9. Course Wrap-Up: What You Learned

Throughout this course, you learned:

1. ✅ The definition of **efficient and Pythonic code**
2. ✅ How to use **Python's powerful built-in library**
3. ✅ The advantages of **NumPy arrays**
4. ✅ Some handy **magic commands to profile code**
5. ✅ How to deploy efficient solutions with **zip(), itertools, collections, and set theory**
6. ✅ The **cost of looping** and how to **eliminate loops**
7. ✅ **Best practices for iterating with pandas DataFrames**

---

## Quick Reference

### Code Patterns

```python
# Vectorization (FASTEST)
df['result'] = df['col1'].values - df['col2'].values

# .apply() method
df['result'] = df.apply(lambda row: func(row['col1'], row['col2']), axis=1)

# .itertuples() (when iteration needed)
for row in df.itertuples():
    value = row.column_name  # Dot notation

# .iterrows() (avoid if possible)
for idx, row in df.iterrows():
    value = row['column_name']  # Bracket notation
```

### Performance Cheat Sheet

- **Vectorization**: ~1,476x faster than `.iloc[]`
- **`.apply()`**: ~6x faster than `.iloc[]`
- **`.itertuples()`**: ~70x faster than `.iterrows()`
- **`.iterrows()`**: ~2x faster than `.iloc[]`

---

#python #pandas #dataframes #iteration #vectorization #optimization #data-analysis #writing-efficient-python-code

## Section A: Multiple Choice Questions
**20 questions | 2 points each = 40 points**

### Topic 1: pandas Basics & Iteration Methods (10 questions)

**Q1.** What is the main data structure in pandas?
- [ ] A) Array
- [ ] B) DataFrame
- [ ] C) Series
- [ ] D) Matrix

> [!success]- Answer
> **Correct Answer: B) DataFrame**
> 
> The DataFrame is pandas' main data structure. It represents tabular data with labeled rows and columns, and is built on top of the NumPy array structure.

**Q2.** What underlying structure is the pandas DataFrame built on?
- [ ] A) Python lists
- [ ] B) Dictionaries
- [ ] C) NumPy arrays
- [ ] D) Tuples

> [!success]- Answer
> **Correct Answer: C) NumPy arrays**
> 
> pandas DataFrames are built on top of NumPy array structures, which is why we can leverage NumPy's efficiency for operations.

**Q3.** Which iteration method is the slowest for pandas DataFrames?
- [ ] A) .itertuples()
- [ ] B) .apply()
- [ ] C) .iloc[] with range(len(df))
- [ ] D) .iterrows()

> [!success]- Answer
> **Correct Answer: C) .iloc[] with range(len(df))**
> 
> Using `.iloc[]` with a range loop is the slowest method at 183 ms, compared to other iteration methods.

**Q4.** What does `.iterrows()` return for each iteration?
- [ ] A) A named tuple
- [ ] B) A tuple of (index, Series)
- [ ] C) A NumPy array
- [ ] D) A dictionary

> [!success]- Answer
> **Correct Answer: B) A tuple of (index, Series)**
> 
> `.iterrows()` returns a tuple where the first element is the index and the second element is a pandas Series containing the row data.
> 
> ```python
> for i, row in df.iterrows():
>     # i is the index, row is a Series
>     value = row['column_name']
> ```

**Q5.** What does `.itertuples()` return for each iteration?
- [ ] A) A pandas Series
- [ ] B) A dictionary
- [ ] C) A named tuple
- [ ] D) A list

> [!success]- Answer
> **Correct Answer: C) A named tuple**
> 
> `.itertuples()` returns named tuples (specifically, Pandas named tuples) that allow you to access columns using dot notation.
> 
> ```python
> for row in df.itertuples():
>     print(row.column_name)  # Dot notation
> ```

**Q6.** How do you access a column value when using `.iterrows()`?
- [ ] A) row.column_name
- [ ] B) row['column_name']
- [ ] C) row(column_name)
- [ ] D) row->column_name

> [!success]- Answer
> **Correct Answer: B) row['column_name']**
> 
> With `.iterrows()`, the row is a Series object, so you use bracket notation to access values.

**Q7.** How do you access a column value when using `.itertuples()`?
- [ ] A) row['column_name']
- [ ] B) row.get('column_name')
- [ ] C) row.column_name
- [ ] D) row[column_name]

> [!success]- Answer
> **Correct Answer: C) row.column_name**
> 
> With `.itertuples()`, the row is a named tuple, so you use dot notation to access values. Using bracket notation like `row['column_name']` will cause a TypeError.

**Q8.** Approximately how much faster is `.itertuples()` compared to `.iterrows()`?
- [ ] A) 2x faster
- [ ] B) 10x faster
- [ ] C) 70x faster
- [ ] D) Same speed

> [!success]- Answer
> **Correct Answer: C) 70x faster**
> 
> The chapter shows `.itertuples()` (7.48 ms) is approximately 70x faster than `.iterrows()` (527 ms) for printing rows.

**Q9.** What is the primary library that pandas is built on top of?
- [ ] A) TensorFlow
- [ ] B) NumPy
- [ ] C) SciPy
- [ ] D) Matplotlib

> [!success]- Answer
> **Correct Answer: B) NumPy**
> 
> pandas is built on top of NumPy, which allows it to leverage NumPy's efficient array operations and is why vectorization works so well with pandas.

**Q10.** Which method would you use to iterate when you need the row index?
- [ ] A) .itertuples() only
- [ ] B) .iterrows() only
- [ ] C) Both .itertuples() and .iterrows()
- [ ] D) Neither method provides index

> [!success]- Answer
> **Correct Answer: C) Both .itertuples() and .iterrows()**
> 
> Both methods provide access to the index:
> - `.iterrows()`: `for i, row in df.iterrows()` - i is the index
> - `.itertuples()`: `row.Index` gives you the index from the named tuple

### Topic 2: .apply() Method & Vectorization (10 questions)

**Q11.** What does the `axis` parameter in `.apply()` specify?
- [ ] A) The data type to return
- [ ] B) Whether to apply to columns (0) or rows (1)
- [ ] C) The number of iterations
- [ ] D) The sorting order

> [!success]- Answer
> **Correct Answer: B) Whether to apply to columns (0) or rows (1)**
> 
> The axis parameter specifies the direction:
> - `axis=0`: apply function to each column
> - `axis=1`: apply function to each row

**Q12.** What type of function can be used with `.apply()`?
- [ ] A) Only named functions
- [ ] B) Only lambda functions
- [ ] C) Both named functions and lambda functions
- [ ] D) Only built-in functions

> [!success]- Answer
> **Correct Answer: C) Both named functions and lambda functions**
> 
> `.apply()` can work with any function - named functions, lambda functions, or built-in functions.
> 
> ```python
> # Lambda function
> df.apply(lambda row: row['A'] + row['B'], axis=1)
> 
> # Named function
> df.apply(my_function, axis=1)
> ```

**Q13.** In the baseball example, how much faster was `.apply()` compared to `.iterrows()`?
- [ ] A) No difference
- [ ] B) ~3x faster
- [ ] C) ~10x faster
- [ ] D) ~100x faster

> [!success]- Answer
> **Correct Answer: B) ~3x faster**
> 
> For calculating run differential:
> - `.iterrows()`: 86.8 ms
> - `.apply()`: 30.1 ms
> - Approximately 3x faster

**Q14.** What does "vectorization" mean in the context of pandas?
- [ ] A) Converting data to vectors
- [ ] B) Performing operations on entire arrays at once without explicit loops
- [ ] C) Creating vector graphics
- [ ] D) Sorting data in vectors

> [!success]- Answer
> **Correct Answer: B) Performing operations on entire arrays at once without explicit loops**
> 
> Vectorization (also called broadcasting) means applying operations to entire arrays simultaneously, leveraging NumPy's optimized C implementations rather than Python loops.

**Q15.** How do you access the underlying NumPy array from a pandas DataFrame column?
- [ ] A) df['column'].array
- [ ] B) df['column'].to_numpy()
- [ ] C) df['column'].values
- [ ] D) df['column'].get_array()

> [!success]- Answer
> **Correct Answer: C) df['column'].values**
> 
> The `.values` attribute returns the underlying NumPy array of a pandas Series/column.
> 
> ```python
> wins_np = baseball_df['W'].values
> print(type(wins_np))  # <class 'numpy.ndarray'>
> ```

**Q16.** What is the performance of vectorization compared to `.iterrows()` for run differential calculation?
- [ ] A) ~10x faster
- [ ] B) ~50x faster
- [ ] C) ~700x faster
- [ ] D) Same speed

> [!success]- Answer
> **Correct Answer: C) ~700x faster**
> 
> Performance comparison:
> - `.iterrows()`: 86.8 ms
> - Vectorization: 0.124 ms (124 μs)
> - Approximately 700x faster!

**Q17.** Which code correctly uses `.apply()` to calculate run differential?
- [ ] A) `df.apply(lambda row: row['RS'] - row['RA'])`
- [ ] B) `df.apply(lambda row: row['RS'] - row['RA'], axis=1)`
- [ ] C) `df.apply(row['RS'] - row['RA'], axis=1)`
- [ ] D) `df.apply(lambda: row['RS'] - row['RA'], axis=0)`

> [!success]- Answer
> **Correct Answer: B) df.apply(lambda row: row['RS'] - row['RA'], axis=1)**
> 
> For row-wise operations, you need:
> 1. A lambda function that takes a row parameter
> 2. `axis=1` to apply across rows
> 
> ```python
> df.apply(lambda row: row['RS'] - row['RA'], axis=1)
> ```

**Q18.** What is the fastest method for performing simple arithmetic operations on DataFrame columns?
- [ ] A) Using .iterrows()
- [ ] B) Using .apply()
- [ ] C) Using vectorization with .values
- [ ] D) Using .itertuples()

> [!success]- Answer
> **Correct Answer: C) Using vectorization with .values**
> 
> Vectorization is by far the fastest method, approximately 1,476x faster than `.iloc[]` loops.
> 
> ```python
> df['result'] = df['col1'].values - df['col2'].values
> ```

**Q19.** Why is vectorization so much faster than looping?
- [ ] A) It uses less memory
- [ ] B) It leverages NumPy's optimized C implementations
- [ ] C) It runs on multiple processors
- [ ] D) It compresses the data

> [!success]- Answer
> **Correct Answer: B) It leverages NumPy's optimized C implementations**
> 
> Vectorization is fast because it uses NumPy's underlying C implementations for array operations, avoiding the overhead of Python loops and leveraging efficient low-level operations.

**Q20.** In the syntax `df.apply(func, axis=1)`, what does `axis=1` mean?
- [ ] A) Apply to the first column
- [ ] B) Apply function row-wise (across columns)
- [ ] C) Apply to the first row
- [ ] D) Apply function column-wise (down rows)

> [!success]- Answer
> **Correct Answer: B) Apply function row-wise (across columns)**
> 
> - `axis=1`: apply function to each row (operate across columns)
> - `axis=0`: apply function to each column (operate down rows)

---

## Section B: True/False Questions
**10 questions | 1 point each = 10 points**

**Q21.** The `.iloc[]` method with range is faster than `.iterrows()`. **T/F**

> [!success]- Answer
> **False** - `.iloc[]` with range (183 ms) is slower than `.iterrows()` (95.3 ms). `.iloc[]` is actually the slowest method covered.

**Q22.** When using `.itertuples()`, you can access column values using bracket notation like `row['column']`. **T/F**

> [!success]- Answer
> **False** - `.itertuples()` returns named tuples, which require dot notation (`row.column`). Using bracket notation will cause a TypeError.

**Q23.** The `.apply()` method can only be used with lambda functions. **T/F**

> [!success]- Answer
> **False** - `.apply()` can be used with any function: named functions, lambda functions, or built-in functions.

**Q24.** Vectorization is approximately 1,476x faster than using `.iloc[]` with a loop. **T/F**

> [!success]- Answer
> **True** - The chapter demonstrates that vectorization (124 μs) is approximately 1,476x faster than `.iloc[]` looping (183 ms).

**Q25.** `.iterrows()` returns each row as a pandas Series object. **T/F**

> [!success]- Answer
> **True** - `.iterrows()` returns a tuple of (index, Series), where the Series contains the row data.

**Q26.** pandas DataFrames are built on top of Python lists. **T/F**

> [!success]- Answer
> **False** - pandas DataFrames are built on top of NumPy arrays, not Python lists.

**Q27.** The `.values` attribute returns the underlying NumPy array of a DataFrame column. **T/F**

> [!success]- Answer
> **True** - Using `.values` on a DataFrame column or Series returns the underlying NumPy ndarray.

**Q28.** `.itertuples()` is approximately 70x faster than `.iterrows()` for printing rows. **T/F**

> [!success]- Answer
> **True** - The chapter shows `.itertuples()` (7.48 ms) is approximately 70x faster than `.iterrows()` (527 ms).

**Q29.** You should always use `.iterrows()` when iterating over a DataFrame. **T/F**

> [!success]- Answer
> **False** - You should prefer vectorization first, then `.apply()`, then `.itertuples()` if you must iterate. `.iterrows()` should be avoided when possible as it's slow.

**Q30.** Broadcasting and vectorization are the same concept in pandas. **T/F**

> [!success]- Answer
> **True** - The chapter explicitly states "Broadcasting (vectorizing) is extremely efficient!" They refer to the same concept of applying operations to entire arrays at once.

---

## Section C: Short Answer Questions
**7 questions | 3 points each = 21 points**

**Q31.** List the five iteration methods covered in the chapter from fastest to slowest, and provide the approximate time for each from the examples given.

> [!success]- Answer
> **Answer:**
> 
> From fastest to slowest:
> 
> 1. **Vectorization (NumPy arrays)**: 124 μs (0.124 ms)
> 2. **`.apply()` method**: 30.1 ms
> 3. **`.itertuples()`**: 7.48 ms (for printing) / not timed for calculations
> 4. **`.iterrows()`**: 95.3 ms (win percentage) / 86.8 ms (run diff)
> 5. **`.iloc[]` with range**: 183 ms
> 
> **Note:** The times vary by operation, but the relative ranking remains consistent.

**Q32.** Explain the difference between how you access column values when using `.iterrows()` versus `.itertuples()`. Provide code examples.

> [!success]- Answer
> **Answer:**
> 
> **`.iterrows()`** returns a Series, so you use **bracket notation**:
> ```python
> for i, row in df.iterrows():
>     value = row['Team']  # Bracket notation
>     wins = row['W']
> ```
> 
> **`.itertuples()`** returns a named tuple, so you use **dot notation**:
> ```python
> for row in df.itertuples():
>     value = row.Team  # Dot notation
>     wins = row.W
>     
> # This will cause TypeError:
> # value = row['Team']  # Wrong!
> ```
> 
> Using the wrong notation will result in errors.

**Q33.** What does `axis=1` mean in the context of the `.apply()` method? Provide an example showing how to use it.

> [!success]- Answer
> **Answer:**
> 
> **`axis=1`** means to apply the function **row-wise** (across columns). The function receives each row as input and should return a result for that row.
> 
> **Example:**
> ```python
> # Calculate run differential for each row
> baseball_df.apply(
>     lambda row: row['RS'] - row['RA'],
>     axis=1
> )
> ```
> 
> The function operates on each row, accessing columns within that row. In contrast, `axis=0` would apply the function column-wise (down the rows).

**Q34.** Why is vectorization so much faster than other iteration methods? Explain the underlying reason.

> [!success]- Answer
> **Answer:**
> 
> Vectorization is faster because:
> 
> 1. **Uses NumPy's C implementations**: Operations are performed at the C level rather than Python level, which is much faster
> 
> 2. **Eliminates Python loop overhead**: No repeated Python bytecode execution for each iteration
> 
> 3. **Operates on entire arrays at once**: Broadcasting applies operations to all elements simultaneously rather than one at a time
> 
> 4. **Leverages optimized algorithms**: NumPy uses highly optimized algorithms for array operations
> 
> Example: `df['RS'].values - df['RA'].values` performs the subtraction on all elements in one optimized operation rather than looping through each row.

**Q35.** When should you use `.apply()` instead of vectorization? Provide a scenario where `.apply()` would be necessary.

> [!success]- Answer
> **Answer:**
> 
> Use `.apply()` when:
> 
> 1. **Complex custom logic** that cannot be vectorized directly
> 2. **Conditional operations** that depend on row-specific logic
> 3. **Function calls** to custom or external functions
> 4. **Operations requiring multiple columns** with complex logic
> 
> **Scenario example:**
> ```python
> def categorize_team(row):
>     if row['W'] > 90 and row['Playoffs'] == 1:
>         return 'Elite'
>     elif row['W'] > 75:
>         return 'Good'
>     else:
>         return 'Rebuilding'
> 
> df['Category'] = df.apply(categorize_team, axis=1)
> ```
> 
> This conditional logic based on multiple columns cannot be easily vectorized, making `.apply()` the appropriate choice (though still prefer vectorization when possible).

**Q36.** Describe what the `.values` attribute does and why it's important for performance optimization in pandas.

> [!success]- Answer
> **Answer:**
> 
> **What `.values` does:**
> The `.values` attribute returns the underlying NumPy ndarray from a pandas Series or DataFrame column.
> 
> ```python
> wins_np = baseball_df['W'].values
> print(type(wins_np))  # <class 'numpy.ndarray'>
> ```
> 
> **Why it's important for performance:**
> 1. Allows direct access to NumPy arrays for vectorized operations
> 2. Bypasses pandas overhead for simple operations
> 3. Enables extremely fast array arithmetic (e.g., ~1,476x faster than loops)
> 4. Leverages NumPy's optimized C implementations
> 
> **Example:**
> ```python
> # Fast vectorized operation
> run_diff = df['RS'].values - df['RA'].values  # 124 μs
> ```

**Q37.** What are the three best practices for iterating over pandas DataFrames in order of preference?

> [!success]- Answer
> **Answer:**
> 
> **Order of preference:**
> 
> 1. **🥇 Vectorization (First Choice)**
>    - Use `.values` to access NumPy arrays
>    - Perform operations on entire columns at once
>    - Example: `df['new'] = df['col1'].values - df['col2'].values`
> 
> 2. **🥈 `.apply()` Method (When Vectorization Not Possible)**
>    - Use for custom functions or complex logic
>    - Still much faster than explicit loops
>    - Example: `df.apply(lambda row: func(row['col']), axis=1)`
> 
> 3. **🥉 `.itertuples()` (When Iteration Is Absolutely Necessary)**
>    - Much faster than `.iterrows()`
>    - Use dot notation to access values
>    - Example: `for row in df.itertuples(): print(row.column)`
> 
> **Avoid:** `.iterrows()` and especially `.iloc[]` with range loops.

---

## Section D: Code Analysis & Scenarios
**7 questions | 2 points each = 14 points**

**Q38.** What is wrong with this code? Fix it.
```python
for row in df.itertuples():
    team_name = row['Team']
    wins = row['W']
```

> [!success]- Answer
> **Issue:** `.itertuples()` returns named tuples, which don't support bracket notation.
> 
> **Fixed code:**
> ```python
> for row in df.itertuples():
>     team_name = row.Team  # Dot notation
>     wins = row.W  # Dot notation
> ```

**Q39.** Complete this code to calculate win percentage using the fastest method (vectorization):
```python
baseball_df = pd.read_csv('baseball_stats.csv')
# Complete: Calculate win percentage (wins / games)
baseball_df['WP'] = _______
```

> [!success]- Answer
> **Completed code:**
> ```python
> baseball_df['WP'] = baseball_df['W'].values / baseball_df['G'].values
> 
> # Alternative (also vectorized, pandas handles it):
> baseball_df['WP'] = baseball_df['W'] / baseball_df['G']
> ```
> 
> Both approaches use vectorization, but explicitly using `.values` ensures you're working directly with NumPy arrays.

**Q40.** Rewrite this inefficient code using `.apply()`:
```python
results = []
for i, row in df.iterrows():
    result = row['A'] * 2 + row['B']
    results.append(result)
df['Result'] = results
```

> [!success]- Answer
> **Rewritten code:**
> ```python
> df['Result'] = df.apply(lambda row: row['A'] * 2 + row['B'], axis=1)
> 
> # Even better - use vectorization:
> df['Result'] = df['A'].values * 2 + df['B'].values
> ```

**Q41.** What will this code output?
```python
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
result = df['A'].values + df['B'].values
print(type(result))
print(result)
```

> [!success]- Answer
> **Output:**
> ```python
> <class 'numpy.ndarray'>
> [5 7 9]
> ```
> 
> **Explanation:**
> - `.values` returns a NumPy array
> - Adding two NumPy arrays performs element-wise addition
> - Result is a NumPy array `[1+4, 2+5, 3+6]` = `[5, 7, 9]`

**Q42.** This code is slow. Identify the issue and rewrite it using the fastest method:
```python
total_runs = []
for i in range(len(baseball_df)):
    row = baseball_df.iloc[i]
    total = row['RS'] + row['RA']
    total_runs.append(total)
baseball_df['Total_Runs'] = total_runs
```

> [!success]- Answer
> **Issue:** Uses the slowest iteration method (`.iloc[]` with range)
> 
> **Optimized code (vectorization):**
> ```python
> baseball_df['Total_Runs'] = baseball_df['RS'].values + baseball_df['RA'].values
> 
> # Or simply (pandas handles vectorization):
> baseball_df['Total_Runs'] = baseball_df['RS'] + baseball_df['RA']
> ```
> 
> This will be approximately 1,476x faster!

**Q43.** Complete this code to iterate over a DataFrame and print team names using the faster iteration method:
```python
for _______ in team_df._______:
    print(_______)
```

> [!success]- Answer
> **Completed code:**
> ```python
> for row in team_df.itertuples():
>     print(row.Team)
> 
> # If you need the index:
> for row in team_df.itertuples():
>     print(row.Index, row.Team)
> ```
> 
> `.itertuples()` is approximately 70x faster than `.iterrows()` for iteration.

**Q44.** You need to apply a complex function that takes three column values and returns a category based on multiple conditions. Which method should you use and why? Write example code.

> [!success]- Answer
> **Method: Use `.apply()` with a custom function**
> 
> **Why:** The logic is too complex for simple vectorization, but `.apply()` is still much faster than explicit loops.
> 
> **Example code:**
> ```python
> def classify_team(row):
>     if row['W'] > 90 and row['RS'] > 750:
>         return 'Powerhouse'
>     elif row['W'] > 80 and row['RS'] > row['RA']:
>         return 'Contender'
>     elif row['W'] < 70:
>         return 'Rebuilding'
>     else:
>         return 'Average'
> 
> baseball_df['Classification'] = baseball_df.apply(classify_team, axis=1)
> ```
> 
> This is ~3x faster than using `.iterrows()` and much cleaner than manual loops.

---

## Answer Key & Scoring

### Section A: Multiple Choice (40 points)
Q1: B | Q2: C | Q3: C | Q4: B | Q5: C | Q6: B | Q7: C | Q8: C | Q9: B | Q10: C
Q11: B | Q12: C | Q13: B | Q14: B | Q15: C | Q16: C | Q17: B | Q18: C | Q19: B | Q20: B

### Section B: True/False (10 points)
Q21: F | Q22: F | Q23: F | Q24: T | Q25: T | Q26: F | Q27: T | Q28: T | Q29: F | Q30: T

### Section C: Short Answer (21 points)
Q31-Q37: See detailed answers above (3 points each)

### Section D: Code Analysis (14 points)
Q38-Q44: See detailed answers above (2 points each)

**Total: 85 points**
**Passing Score: 60+ points (70%)**

---

## Quick Study Guide

### Essential Code Patterns

```python
# Method 1: Vectorization (FASTEST - use whenever possible)
df['result'] = df['col1'].values - df['col2'].values

# Method 2: .apply() (when custom logic needed)
df['result'] = df.apply(lambda row: func(row['col1'], row['col2']), axis=1)

# Method 3: .itertuples() (when iteration necessary)
for row in df.itertuples():
    print(row.column_name)  # Dot notation

# Method 4: .iterrows() (avoid when possible)
for idx, row in df.iterrows():
    print(row['column_name'])  # Bracket notation

# Method 5: .iloc with range (AVOID - slowest)
for i in range(len(df)):
    row = df.iloc[i]
```

---

## Performance Summary Table

| Method | Example Time | Relative Speed | Access Pattern |
|--------|-------------|----------------|----------------|
| **Vectorization** | 124 μs | 1x (baseline - fastest) | `.values` + operations |
| **`.apply()`** | 30.1 ms | ~240x slower | Lambda/function with `axis=1` |
| **`.itertuples()`** | 7.48 ms | ~60x slower | Dot notation: `row.column` |
| **`.iterrows()`** | 95.3 ms | ~768x slower | Bracket: `row['column']` |
| **`.iloc[]` loop** | 183 ms | ~1,476x slower | `df.iloc[i]` |

---

## Key Concepts Summary

### Iteration Methods

| Method | Returns | Access Syntax | Speed | When to Use |
|--------|---------|---------------|-------|-------------|
| `.iloc[i]` | Series | `row['col']` | Slowest | Never |
| `.iterrows()` | (index, Series) | `row['col']` | Slow | Rarely |
| `.itertuples()` | Named tuple | `row.col` | Fast | When iteration needed |
| `.apply()` | Series/DataFrame | `row['col']` in lambda | Faster | Custom functions |
| Vectorization | Array | Direct column ops | Fastest | Always when possible |

### Access Patterns

**`.iterrows()`**: Bracket notation
```python
for i, row in df.iterrows():
    value = row['column_name']
```

**`.itertuples()`**: Dot notation
```python
for row in df.itertuples():
    value = row.column_name
    index = row.Index
```

### Axis Parameter in `.apply()`

- **`axis=0`**: Apply function column-wise (down rows)
- **`axis=1`**: Apply function row-wise (across columns) ← Most common

---

## Common Mistakes to Avoid

❌ **Using bracket notation with `.itertuples()`**
```python
# Wrong
for row in df.itertuples():
    print(row['Team'])  # TypeError!
    
# Right
for row in df.itertuples():
    print(row.Team)
```

❌ **Using `.iloc[]` with range for iteration**
```python
# Slowest method - avoid!
for i in range(len(df)):
    row = df.iloc[i]
```

❌ **Using loops when vectorization is possible**
```python
# Slow
results = []
for i, row in df.iterrows():
    results.append(row['A'] + row['B'])
    
# Fast (1476x faster!)
results = df['A'].values + df['B'].values
```

❌ **Forgetting axis parameter in `.apply()`**
```python
# Wrong - defaults to axis=0 (columns)
df.apply(lambda row: row['A'] + row['B'])

# Right - specify axis=1 for rows
df.apply(lambda row: row['A'] + row['B'], axis=1)
```

❌ **Not accessing underlying NumPy arrays**
```python
# Good but not optimal
df['result'] = df['A'] - df['B']

# Optimal - direct NumPy operations
df['result'] = df['A'].values - df['B'].values
```

---

## Study Strategy

### Day 1-2: Understand Iteration Methods
- Learn what each method returns (Series vs named tuple)
- Practice access patterns (bracket vs dot notation)
- Memorize the speed hierarchy
- Understand `.iloc[]`, `.iterrows()`, `.itertuples()`

### Day 3: Master `.apply()` Method
- Practice `axis` parameter (0 vs 1)
- Write