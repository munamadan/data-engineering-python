## Overview

### Why Time Our Code?
- Allows us to pick the optimal coding approach
- **Faster code = More efficient code!**

---

## 1. IPython Magic Commands

### What Are Magic Commands?

**Magic commands** are enhancements on top of normal Python syntax:
- Prefixed by the `%` character
- Provide special functionality for interactive development
- Available in IPython and Jupyter notebooks

**View all magic commands:**
```python
%lsmagic
```

---

## 2. Using %timeit

### Basic Syntax

**Line magic (single line):**
```python
%timeit code_to_time
```

**Cell magic (multiple lines):**
```python
%%timeit
line1
line2
line3
```

### Example

```python
import numpy as np

# Time a single line
%timeit rand_nums = np.random.rand(1000)
# Output: 8.61 μs ± 69.1 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

---

## 3. Understanding %timeit Output

### Output Format

```
8.61 μs ± 69.1 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

**Breaking it down:**

| Component | Meaning |
|-----------|---------|
| `8.61 μs` | **Average time per loop** (mean) |
| `± 69.1 ns` | **Standard deviation** (variability) |
| `7 runs` | Number of separate runs |
| `100000 loops` | Number of loops per run |

### How %timeit Works

1. **Runs the code multiple times** (loops)
2. **Repeats this process** (runs)
3. **Calculates statistics** (mean and std dev)
4. **Reports the best results**

> [!info] Why Multiple Runs?
> Multiple runs provide more reliable timing by accounting for system variations and other processes.

---

## 4. Customizing %timeit

### Specifying Runs and Loops

**Syntax:**
- `-r` : Number of runs
- `-n` : Number of loops

```python
# Set 2 runs and 10 loops
%timeit -r2 -n10 rand_nums = np.random.rand(1000)
# Output: 16.9 μs ± 5.14 μs per loop (mean ± std. dev. of 2 runs, 10 loops each)
```

**When to customize:**
- Very slow code: Reduce loops/runs
- Very fast code: Increase loops for accuracy

---

## 5. Line Magic vs Cell Magic

### Line Magic (`%timeit`)

**Single line of code:**
```python
%timeit nums = [x for x in range(10)]
# Output: 914 ns ± 7.33 ns per loop
```

### Cell Magic (`%%timeit`)

**Multiple lines of code:**
```python
%%timeit
nums = []
for x in range(10):
    nums.append(x)
# Output: 1.17 μs ± 3.26 ns per loop
```

> [!tip] Line vs Cell
> Use `%` for single lines, `%%` for multiple lines (must be first line in cell)

---

## 6. Saving %timeit Output

### Using the `-o` Flag

```python
# Save output to variable
times = %timeit -o rand_nums = np.random.rand(1000)
# Output: 8.69 μs ± 91.4 ns per loop
```

### Accessing Saved Results

```python
# All timing results
times.timings
# [8.697e-06, 8.651e-06, 8.634e-06, ...]

# Best time
times.best
# 8.619398139999247e-06

# Worst time
times.worst
# 8.902550710008654e-06

# Average time
times.average
```

---

## 7. Comparing Approaches

### Example: Formal vs Literal Syntax

**Python data structures can be created two ways:**

**Formal name:**
```python
formal_list = list()
formal_dict = dict()
formal_tuple = tuple()
```

**Literal syntax:**
```python
literal_list = []
literal_dict = {}
literal_tuple = ()
```

### Timing Comparison

```python
# Time formal syntax
f_time = %timeit -o formal_dict = dict()
# Output: 145 ns ± 1.5 ns per loop

# Time literal syntax
l_time = %timeit -o literal_dict = {}
# Output: 93.3 ns ± 1.88 ns per loop

# Calculate difference
diff = (f_time.average - l_time.average) * (10**9)
print(f'l_time better than f_time by {diff} ns')
# Output: l_time better than f_time by 51.9 ns
```

> [!success] Winner: Literal Syntax
> Literal syntax (`{}`) is faster than formal syntax (`dict()`)

---

## 8. Code Profiling for Runtime

### What is Code Profiling?

**Provides detailed stats on:**
- Frequency of function calls
- Duration of function calls
- **Line-by-line analysis**

### Installing line_profiler

```bash
pip install line_profiler
```

---

## 9. Using line_profiler

### Setup

```python
# Load the extension
%load_ext line_profiler
```

### Syntax

```python
%lprun -f function_name function_call
```

**Components:**
- `%lprun`: Line profiler magic command
- `-f function_name`: Function to profile
- `function_call`: Actual function execution

### Example Function

```python
heroes = ['Batman', 'Superman', 'Wonder Woman']
hts = np.array([188.0, 191.0, 183.0])
wts = np.array([95.0, 101.0, 74.0])

def convert_units(heroes, heights, weights):
    new_hts = [ht * 0.39370 for ht in heights]
    new_wts = [wt * 2.20462 for wt in weights]
    
    hero_data = {}
    for i, hero in enumerate(heroes):
        hero_data[hero] = (new_hts[i], new_wts[i])
    
    return hero_data

# Profile the function
%lprun -f convert_units convert_units(heroes, hts, wts)
```

---

## 10. Understanding %lprun Output

### Output Format

```
Line #  Hits  Time      Per Hit   % Time  Line Contents
===========================================================
     1                                     def convert_units(...):
     2     1  500.0     500.0      35.0       new_hts = [...]
     3     1  450.0     450.0      31.5       new_wts = [...]
     4     1   10.0      10.0       0.7       hero_data = {}
     5     3  450.0     150.0      31.5       for i,hero...
     6                                            hero_data[...] 
     7     1   20.0      20.0       1.4       return hero_data
```

**Column Meanings:**

| Column | Description |
|--------|-------------|
| **Line #** | Line number in the function |
| **Hits** | Number of times line was executed |
| **Time** | Total time spent on this line (microseconds) |
| **Per Hit** | Time per execution (Time / Hits) |
| **% Time** | Percentage of total function time |
| **Line Contents** | Actual code |

### Key Insights

- **Identify bottlenecks**: Lines with highest % Time
- **Loop iterations**: See Hits count
- **Per-call cost**: Check Per Hit values

---

## 11. Comparing %timeit and %lprun

### %timeit

**Purpose:** Overall function timing

```python
%timeit convert_units(heroes, hts, wts)
# Output: 3 μs ± 32 ns per loop
```

**Pros:**
- Quick overall benchmark
- Multiple runs for accuracy
- Good for comparing approaches

### %lprun

**Purpose:** Line-by-line analysis

```python
%lprun -f convert_units convert_units(heroes, hts, wts)
# Shows detailed breakdown per line
```

**Pros:**
- Identifies specific bottlenecks
- Shows execution counts
- Pinpoints optimization targets

> [!tip] Use Both
> Use `%timeit` for overall comparison, `%lprun` to find bottlenecks

---

## 12. Code Profiling for Memory Usage

### Quick Approach: sys.getsizeof()

```python
import sys

# List memory
nums_list = [*range(1000)]
sys.getsizeof(nums_list)
# Output: 9112 bytes

# NumPy array memory
import numpy as np
nums_np = np.array(range(1000))
sys.getsizeof(nums_np)
# Output: 8096 bytes
```

> [!info] Memory Efficiency
> NumPy arrays generally use less memory than lists for numerical data

---

## 13. Using memory_profiler

### Installation

```bash
pip install memory_profiler
```

### Setup

```python
%load_ext memory_profiler
```

### Important Requirement

**Functions must be imported from a file:**

```python
# hero_funcs.py
def convert_units(heroes, heights, weights):
    # function code here
    pass

# In notebook/script
from hero_funcs import convert_units
%mprun -f convert_units convert_units(heroes, hts, wts)
```

> [!warning] Import Requirement
> Unlike `%lprun`, `%mprun` requires functions to be imported from external files

---

## 14. Understanding %mprun Output

### Output Format

```
Line #  Mem usage  Increment  Occurrences  Line Contents
==========================================================
     1   50.0 MiB   50.0 MiB            1   def convert_units(...):
     2   50.2 MiB    0.2 MiB            1       new_hts = [...]
     3   50.4 MiB    0.2 MiB            1       new_wts = [...]
     4   50.4 MiB    0.0 MiB            1       hero_data = {}
     5   50.6 MiB    0.2 MiB            4       for i,hero...
     6   50.6 MiB    0.0 MiB            1       return hero_data
```

**Column Meanings:**

| Column | Description |
|--------|-------------|
| **Line #** | Line number in the function |
| **Mem usage** | Total memory used at this line |
| **Increment** | Memory added by this line |
| **Occurrences** | Number of times line executed |
| **Line Contents** | Actual code |

### Key Metrics

- **Mem usage**: Cumulative memory at each step
- **Increment**: New memory allocated (key for finding memory hogs)
- **Occurrences**: How many times line ran

---

## 15. %mprun Output Caveats

### Small Allocations

```python
# Small datasets may show 0.0 MiB
%mprun -f convert_units convert_units(heroes, hts, wts)
# With 480 heroes: might show 0.0 MiB increments
```

**Why?**
- Memory profiler has limited precision
- Small allocations may not register
- Use larger datasets for meaningful results

### Platform Differences

- **Inspects memory by querying OS**
- Results may differ between platforms
- Results may vary between runs
- **Still useful for relative comparisons**

> [!tip] Use for Comparison
> Focus on comparing lines within same run, not absolute values

---

## 16. Best Practices Summary

### Timing Code

**Use %timeit when:**
- ✅ Comparing different approaches
- ✅ Benchmarking overall performance
- ✅ Need statistical reliability (multiple runs)

**Use %lprun when:**
- ✅ Finding bottlenecks in functions
- ✅ Need line-by-line timing
- ✅ Optimizing specific code sections

### Profiling Memory

**Use sys.getsizeof() when:**
- ✅ Quick object size check
- ✅ Comparing data structure sizes

**Use %mprun when:**
- ✅ Line-by-line memory analysis
- ✅ Finding memory leaks
- ✅ Optimizing memory usage

---

## 17. Complete Workflow Example

### Step 1: Time Overall

```python
%timeit convert_units(heroes, hts, wts)
# 3 μs ± 32 ns per loop
```

### Step 2: Profile Runtime

```python
%load_ext line_profiler
%lprun -f convert_units convert_units(heroes, hts, wts)
# Identifies bottleneck lines
```

### Step 3: Profile Memory

```python
# Save function to hero_funcs.py first
from hero_funcs import convert_units
%load_ext memory_profiler
%mprun -f convert_units convert_units(heroes, hts, wts)
# Shows memory usage per line
```

### Step 4: Optimize

```python
# Improve bottleneck lines
# Re-run profilers to verify improvement
```

---

## 18. Key Commands Reference

### Magic Commands

| Command | Purpose | Usage |
|---------|---------|-------|
| `%timeit` | Time single line | `%timeit code` |
| `%%timeit` | Time multiple lines | First line of cell |
| `%lprun` | Profile runtime | `%lprun -f func func()` |
| `%mprun` | Profile memory | `%mprun -f func func()` |

### %timeit Options

| Option | Purpose | Example |
|--------|---------|---------|
| `-r` | Number of runs | `%timeit -r2` |
| `-n` | Number of loops | `%timeit -n10` |
| `-o` | Save output | `%timeit -o` |

---

## 19. Time Units Reference

| Unit | Symbol | Value |
|------|--------|-------|
| Seconds | s | 1 |
| Milliseconds | ms | 10⁻³ (0.001 s) |
| Microseconds | μs | 10⁻⁶ (0.000001 s) |
| Nanoseconds | ns | 10⁻⁹ (0.000000001 s) |

---

## Key Takeaways

✅ **%timeit** provides reliable timing with multiple runs and loops

✅ **Line magic** (`%`) for single lines, **cell magic** (`%%`) for multiple lines

✅ **Save output** with `-o` flag for programmatic comparison

✅ **Literal syntax** (`[]`, `{}`) faster than formal (`list()`, `dict()`)

✅ **%lprun** gives line-by-line runtime analysis (requires line_profiler)

✅ **%mprun** gives line-by-line memory analysis (requires memory_profiler + import)

✅ **Use profilers** to identify bottlenecks before optimizing

✅ **Combine tools**: %timeit for overall, %lprun/%mprun for details

---

#python #efficiency #profiling #timing #ipython #optimization #performance #datacamp

## Section A: Multiple Choice Questions (60 points)
*2 points each | 30 questions*

### Magic Commands & %timeit Basics (10 questions)

**Q1.** What character prefixes IPython magic commands?

- [ ] A) #
- [ ] B) %
- [ ] C) $
- [ ] D) @

> [!success]- Answer
> **Correct Answer: B) %**
> 
> Magic commands are prefixed by the `%` character. Single `%` for line magic, double `%%` for cell magic.

**Q2.** Why should we time our code?

- [ ] A) To make code look professional
- [ ] B) To pick the optimal coding approach and ensure faster code
- [ ] C) Because it's required
- [ ] D) To count lines of code

> [!success]- Answer
> **Correct Answer: B) To pick the optimal coding approach and ensure faster code**
> 
> Timing allows us to compare approaches and choose the fastest. Faster code = more efficient code!

**Q3.** What command shows all available magic commands?

- [ ] A) %showmagic
- [ ] B) %listmagic
- [ ] C) %lsmagic
- [ ] D) %magiclist

> [!success]- Answer
> **Correct Answer: C) %lsmagic**
> 
> `%lsmagic` displays all available magic commands in IPython.

**Q4.** What does `%timeit` do?

- [ ] A) Prints the current time
- [ ] B) Times code execution with multiple runs and loops
- [ ] C) Creates a timer object
- [ ] D) Delays code execution

> [!success]- Answer
> **Correct Answer: B) Times code execution with multiple runs and loops**
> 
> `%timeit` runs code multiple times to get reliable timing statistics.

**Q5.** In the output `8.61 μs ± 69.1 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)`, what does `8.61 μs` represent?

- [ ] A) Total time for all runs
- [ ] B) Average time per loop
- [ ] C) Fastest time recorded
- [ ] D) Standard deviation

> [!success]- Answer
> **Correct Answer: B) Average time per loop**
> 
> `8.61 μs` is the mean (average) time per loop execution.

**Q6.** What does `± 69.1 ns` represent in %timeit output?

- [ ] A) The fastest time
- [ ] B) The slowest time
- [ ] C) The standard deviation (variability)
- [ ] D) The total time

> [!success]- Answer
> **Correct Answer: C) The standard deviation (variability)**
> 
> Standard deviation shows how much the timing varies across runs.

**Q7.** What's the difference between `%timeit` and `%%timeit`?

- [ ] A) No difference
- [ ] B) % is for single lines, %% is for multiple lines (cell)
- [ ] C) % is faster than %%
- [ ] D) %% is deprecated

> [!success]- Answer
> **Correct Answer: B) % is for single lines, %% is for multiple lines (cell)**
> 
> Single `%` (line magic) times one line, double `%%` (cell magic) times multiple lines.

**Q8.** How do you set 2 runs and 10 loops in %timeit?

- [ ] A) %timeit -r2 -n10
- [ ] B) %timeit runs=2 loops=10
- [ ] C) %timeit (2, 10)
- [ ] D) %timeit --runs 2 --loops 10

> [!success]- Answer
> **Correct Answer: A) %timeit -r2 -n10**
> 
> `-r` sets runs, `-n` sets loops. Example: `%timeit -r2 -n10 code`

**Q9.** How do you save %timeit output to a variable?

- [ ] A) %timeit code > var
- [ ] B) var = %timeit -o code
- [ ] C) %timeit -s var code
- [ ] D) %timeit code --save var

> [!success]- Answer
> **Correct Answer: B) var = %timeit -o code**
> 
> The `-o` flag saves output: `times = %timeit -o code`

**Q10.** What property gives the best time from saved %timeit output?

- [ ] A) times.fastest
- [ ] B) times.min
- [ ] C) times.best
- [ ] D) times.minimum

> [!success]- Answer
> **Correct Answer: C) times.best**
> 
> Access best time with `times.best`, worst with `times.worst`, average with `times.average`.

---

### Comparing Approaches (5 questions)

**Q11.** Which is faster for creating a dictionary?

- [ ] A) formal_dict = dict()
- [ ] B) literal_dict = {}
- [ ] C) Both are the same speed
- [ ] D) Depends on the dictionary size

> [!success]- Answer
> **Correct Answer: B) literal_dict = {}**
> 
> Literal syntax `{}` is faster (~52 ns faster) than formal syntax `dict()`.

**Q12.** What's the formal syntax for creating a list?

- [ ] A) []
- [ ] B) list()
- [ ] C) List()
- [ ] D) new list

> [!success]- Answer
> **Correct Answer: B) list()**
> 
> Formal: `list()`, Literal: `[]`. Literal is faster.

**Q13.** In the comparison `diff = (f_time.average - l_time.average) * (10**9)`, why multiply by 10⁹?

- [ ] A) To convert to nanoseconds
- [ ] B) To make numbers bigger
- [ ] C) To convert to milliseconds
- [ ] D) It's a mistake

> [!success]- Answer
> **Correct Answer: A) To convert to nanoseconds**
> 
> Multiplying by 10⁹ converts seconds to nanoseconds for easier reading.

**Q14.** Which syntax is generally recommended?

- [ ] A) Formal syntax (dict(), list())
- [ ] B) Literal syntax ({}, [])
- [ ] C) Both are equally good
- [ ] D) Neither, use alternatives

> [!success]- Answer
> **Correct Answer: B) Literal syntax ({}, [])**
> 
> Literal syntax is both faster and more Pythonic.

**Q15.** What's the output of comparing a list comprehension vs for loop with append?

- [ ] A) For loop is faster
- [ ] B) List comprehension is faster
- [ ] C) They're the same speed
- [ ] D) Depends on list size

> [!success]- Answer
> **Correct Answer: B) List comprehension is faster**
> 
> Example showed: list comprehension (914 ns) vs for loop (1.17 μs). List comprehension wins.

---

### Code Profiling: Runtime (8 questions)

**Q16.** What does code profiling provide?

- [ ] A) Code syntax checking
- [ ] B) Detailed stats on frequency and duration of function calls
- [ ] C) Code formatting
- [ ] D) Variable names

> [!success]- Answer
> **Correct Answer: B) Detailed stats on frequency and duration of function calls**
> 
> Profiling gives line-by-line analysis of function execution time and frequency.

**Q17.** What package is used for line-by-line runtime profiling?

- [ ] A) time_profiler
- [ ] B) line_profiler
- [ ] C) runtime_profiler
- [ ] D) code_profiler

> [!success]- Answer
> **Correct Answer: B) line_profiler**
> 
> Install with: `pip install line_profiler`

**Q18.** How do you load the line_profiler extension?

- [ ] A) import line_profiler
- [ ] B) %load_ext line_profiler
- [ ] C) %import line_profiler
- [ ] D) load line_profiler

> [!success]- Answer
> **Correct Answer: B) %load_ext line_profiler**
> 
> Magic command to load: `%load_ext line_profiler`

**Q19.** What's the correct syntax for using line profiler?

- [ ] A) %lprun function_name
- [ ] B) %lprun -f function_name function_call
- [ ] C) %lprun function_call
- [ ] D) %profile function_name

> [!success]- Answer
> **Correct Answer: B) %lprun -f function_name function_call**
> 
> Syntax: `%lprun -f function_to_profile actual_function_call()`

**Q20.** In %lprun output, what does the "Hits" column show?

- [ ] A) Number of errors
- [ ] B) Number of times the line was executed
- [ ] C) Time in hits
- [ ] D) Hit rate percentage

> [!success]- Answer
> **Correct Answer: B) Number of times the line was executed**
> 
> "Hits" shows how many times each line ran (useful for loops).

**Q21.** In %lprun output, what does "% Time" show?

- [ ] A) Time in seconds
- [ ] B) Percentage of total function time spent on that line
- [ ] C) Time accuracy percentage
- [ ] D) Loop completion percentage

> [!success]- Answer
> **Correct Answer: B) Percentage of total function time spent on that line**
> 
> "% Time" helps identify bottlenecks - lines taking most time.

**Q22.** What does "Per Hit" show in %lprun output?

- [ ] A) Total time
- [ ] B) Average time per execution (Time / Hits)
- [ ] C) Number of hits
- [ ] D) Time percentage

> [!success]- Answer
> **Correct Answer: B) Average time per execution (Time / Hits)**
> 
> "Per Hit" = Time ÷ Hits, showing average time per execution.

**Q23.** When should you use %lprun vs %timeit?

- [ ] A) Always use %timeit
- [ ] B) Use %timeit for overall comparison, %lprun to find bottlenecks
- [ ] C) Always use %lprun
- [ ] D) They do the same thing

> [!success]- Answer
> **Correct Answer: B) Use %timeit for overall comparison, %lprun to find bottlenecks**
> 
> %timeit: overall benchmark. %lprun: line-by-line detail for optimization.

---

### Code Profiling: Memory (7 questions)

**Q24.** What's a quick way to check object size in bytes?

- [ ] A) len()
- [ ] B) sys.getsizeof()
- [ ] C) size()
- [ ] D) memory()

> [!success]- Answer
> **Correct Answer: B) sys.getsizeof()**
> 
> `sys.getsizeof(obj)` returns size in bytes. Quick but not line-by-line.

**Q25.** Which uses less memory for 1000 numbers?

- [ ] A) Python list
- [ ] B) NumPy array
- [ ] C) Both use the same
- [ ] D) Depends on the numbers

> [!success]- Answer
> **Correct Answer: B) NumPy array**
> 
> Example: list = 9112 bytes, array = 8096 bytes. Arrays more memory-efficient.

**Q26.** What package provides line-by-line memory profiling?

- [ ] A) mem_profiler
- [ ] B) memory_profiler
- [ ] C) line_memory
- [ ] D) memory_line

> [!success]- Answer
> **Correct Answer: B) memory_profiler**
> 
> Install with: `pip install memory_profiler`

**Q27.** What's special about using %mprun?

- [ ] A) Nothing special
- [ ] B) Functions must be imported from external files
- [ ] C) It only works in Python 2
- [ ] D) It requires NumPy

> [!success]- Answer
> **Correct Answer: B) Functions must be imported from external files**
> 
> Unlike %lprun, %mprun requires: `from file import function`

**Q28.** In %mprun output, what does "Increment" show?

- [ ] A) Total memory used
- [ ] B) Memory added by that specific line
- [ ] C) Memory freed
- [ ] D) Memory percentage

> [!success]- Answer
> **Correct Answer: B) Memory added by that specific line**
> 
> "Increment" shows new memory allocated. Key for finding memory hogs.

**Q29.** What does "Mem usage" show in %mprun output?

- [ ] A) Memory added by line
- [ ] B) Cumulative total memory at that line
- [ ] C) Memory freed
- [ ] D) Memory percentage

> [!success]- Answer
> **Correct Answer: B) Cumulative total memory at that line**
> 
> "Mem usage" is cumulative total. "Increment" is what that line added.

**Q30.** Why might %mprun show 0.0 MiB for small datasets?

- [ ] A) It's broken
- [ ] B) Small allocations may not register due to limited precision
- [ ] C) Memory wasn't used
- [ ] D) Wrong units

> [!success]- Answer
> **Correct Answer: B) Small allocations may not register due to limited precision**
> 
> Memory profiler has limited precision. Use larger datasets for meaningful results.

---

## Section B: True/False Questions (15 points)
*1 point each*

**Q31.** Magic commands in IPython are prefixed with the % character. **T/F**

> [!success]- Answer
> **True** - Single `%` for line magic, double `%%` for cell magic.

**Q32.** %timeit runs code only once to measure time. **T/F**

> [!success]- Answer
> **False** - %timeit runs code multiple times (loops and runs) for accurate statistics.

**Q33.** In %timeit output, the first number is the average time per loop. **T/F**

> [!success]- Answer
> **True** - Example: `8.61 μs` is the mean time per loop.

**Q34.** `%%timeit` must be the first line in a cell. **T/F**

> [!success]- Answer
> **True** - Cell magic (`%%`) must be the first line of the cell.

**Q35.** The `-o` flag in %timeit saves output to a variable. **T/F**

> [!success]- Answer
> **True** - `times = %timeit -o code` saves results to `times`.

**Q36.** Literal syntax `{}` is faster than formal syntax `dict()`. **T/F**

> [!success]- Answer
> **True** - Literal syntax is approximately 52 nanoseconds faster.

**Q37.** List comprehensions are generally slower than for loops with append(). **T/F**

> [!success]- Answer
> **False** - List comprehensions are faster. Example: 914 ns vs 1.17 μs.

**Q38.** line_profiler requires `%load_ext line_profiler` before use. **T/F**

> [!success]- Answer
> **True** - Must load the extension first.

**Q39.** %lprun syntax is: `%lprun -f function_name function_call()`. **T/F**

> [!success]- Answer
> **True** - `-f` specifies the function to profile, followed by the actual call.

**Q40.** In %lprun output, "Hits" shows the number of times a line executed. **T/F**

> [!success]- Answer
> **True** - Useful for seeing loop iterations.

**Q41.** sys.getsizeof() provides line-by-line memory analysis. **T/F**

> [!success]- Answer
> **False** - sys.getsizeof() only gives total object size. Use %mprun for line-by-line.

**Q42.** NumPy arrays generally use more memory than Python lists. **T/F**

> [!success]- Answer
> **False** - NumPy arrays use less memory due to homogeneous data types.

**Q43.** %mprun requires functions to be imported from external files. **T/F**

> [!success]- Answer
> **True** - Unlike %lprun, %mprun needs: `from file import function`.

**Q44.** In %mprun output, "Increment" shows memory added by each line. **T/F**

> [!success]- Answer
> **True** - "Increment" is the key metric for finding memory-intensive lines.

**Q45.** %mprun results are identical across all platforms and runs. **T/F**

> [!success]- Answer
> **False** - Results may vary between platforms/runs as it queries the OS. Use for relative comparisons.

---

## Section C: Short Answer Questions (30 points)
*3 points each*

**Q46.** Explain what magic commands are and how to identify them in IPython.

> [!success]- Answer
> **Magic Commands:**
> - Enhancements on top of normal Python syntax
> - Provide special functionality for interactive development
> - Available in IPython and Jupyter notebooks
> 
> **Identification:**
> - Prefixed by the `%` character
> - Single `%` for line magic (one line)
> - Double `%%` for cell magic (multiple lines)
> 
> **Examples:**
> ```python
> %timeit code              # Line magic
> %%timeit                  # Cell magic
> line1
> line2
> ```
> 
> **View all magic commands:**
> ```python
> %lsmagic
> ```

**Q47.** Explain the %timeit output format. What does each component mean?

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
> | `8.61 μs` | **Average time per loop** (mean) |
> | `± 69.1 ns` | **Standard deviation** (variability) |
> | `7 runs` | Number of separate runs executed |
> | `100000 loops` | Number of loops per run |
> 
> **How it works:**
> 1. Executes code 100,000 times (loops)
> 2. Repeats this process 7 times (runs)
> 3. Calculates mean and standard deviation
> 4. Reports average time per loop
> 
> **Why multiple runs?** More reliable timing, accounts for system variations.

**Q48.** Describe how to customize %timeit runs and loops. When would you adjust these values?

> [!success]- Answer
> **Syntax for customization:**
> ```python
> %timeit -r<runs> -n<loops> code
> ```
> 
> **Options:**
> - `-r` : Number of runs (default 7)
> - `-n` : Number of loops (auto-determined)
> 
> **Example:**
> ```python
> %timeit -r2 -n10 code
> # 2 runs, 10 loops each
> ```
> 
> **When to adjust:**
> 
> **Reduce loops/runs when:**
> - Code is very slow
> - Each execution takes significant time
> - Example: `-r2 -n5` for slow operations
> 
> **Increase loops when:**
> - Code is very fast
> - Need more accuracy
> - Default might not be enough
> 
> **Default behavior:**
> - %timeit automatically determines appropriate loop count
> - Usually don't need to customize

**Q49.** Compare formal vs literal syntax for creating Python data structures. Which is faster and why?

> [!success]- Answer
> **Two ways to create data structures:**
> 
> **Formal syntax (using constructor):**
> ```python
> formal_list = list()
> formal_dict = dict()
> formal_tuple = tuple()
> ```
> 
> **Literal syntax (direct notation):**
> ```python
> literal_list = []
> literal_dict = {}
> literal_tuple = ()
> ```
> 
> **Timing comparison:**
> ```python
> %timeit formal_dict = dict()
> # 145 ns ± 1.5 ns
> 
> %timeit literal_dict = {}
> # 93.3 ns ± 1.88 ns
> 
> # Literal is ~52 ns faster!
> ```
> 
> **Why literal is faster:**
> - Direct bytecode operation
> - No function call overhead
> - More optimized at Python interpreter level
> 
> **Winner:** Literal syntax `{}`, `[]`, `()` - faster AND more Pythonic!

**Q50.** Explain how to save %timeit output and what information you can extract from it.

> [!success]- Answer
> **Saving output:**
> ```python
> # Use -o flag
> times = %timeit -o rand_nums = np.random.rand(1000)
> # Output: 8.69 μs ± 91.4 ns per loop
> ```
> 
> **Available information:**
> 
> **1. All timing results:**
> ```python
> times.timings
> # [8.697e-06, 8.651e-06, 8.634e-06, ...]
> # List of all individual run times
> ```
> 
> **2. Best time:**
> ```python
> times.best
> # 8.619398139999247e-06
> # Fastest execution time
> ```
> 
> **3. Worst time:**
> ```python
> times.worst
> # 8.902550710008654e-06
> # Slowest execution time
> ```
> 
> **4. Average time:**
> ```python
> times.average
> # Mean of all runs
> ```
> 
> **Use case - comparing approaches:**
> ```python
> t1 = %timeit -o approach1()
> t2 = %timeit -o approach2()
> diff = (t1.average - t2.average) * 1e9
> print(f"Difference: {diff} ns")
> ```

**Q51.** What is code profiling? Compare %lprun and %timeit.

> [!success]- Answer
> **Code Profiling:**
> Detailed analysis providing:
> - Frequency of function calls
> - Duration of function calls
> - **Line-by-line breakdown**
> 
> **%timeit - Overall timing:**
> ```python
> %timeit convert_units(heroes, hts, wts)
> # Output: 3 μs ± 32 ns per loop
> ```
> 
> **Pros:**
> - Quick overall benchmark
> - Multiple runs for statistical reliability
> - Good for comparing different approaches
> - Easy to use
> 
> **%lprun - Line-by-line profiling:**
> ```python
> %load_ext line_profiler
> %lprun -f convert_units convert_units(heroes, hts, wts)
> # Shows time per line
> ```
> 
> **Pros:**
> - Identifies specific bottlenecks
> - Shows execution count per line
> - Pinpoints exactly what to optimize
> - Shows time distribution
> 
> **When to use each:**
> - **%timeit**: Compare approaches, overall performance
> - **%lprun**: Find bottlenecks, detailed optimization
> 
> **Best practice:** Use both! %timeit first, then %lprun on slow functions.

**Q52.** Describe the %lprun output format and what each column means.

> [!success]- Answer
> **Output Format:**
> ```
> Line #  Hits  Time      Per Hit   % Time  Line Contents
> ===========================================================
>      1     1  500.0     500.0      35.0   def convert_units(...):
>      2     1  450.0     450.0      31.5       new_hts = [...]
>      3     3  450.0     150.0      31.5       for i,hero...
> ```
> 
> **Column Meanings:**
> 
> | Column | Description |
> |--------|-------------|
> | **Line #** | Line number in the function |
> | **Hits** | Times the line executed |
> | **Time** | Total time on this line (μs) |
> | **Per Hit** | Time per execution (Time/Hits) |
> | **% Time** | % of total function time |
> | **Line Contents** | The actual code |
> 
> **Key insights:**
> 
> **Find bottlenecks:**
> - Look for highest **% Time**
> - These lines need optimization
> 
> **Loop analysis:**
> - **Hits** > 1 indicates loops
> - **Per Hit** shows per-iteration cost
> 
> **Example interpretation:**
> - Line 2: 35% of time, ran once → slow operation
> - Line 3: 31.5% time, 3 hits → loop overhead

**Q53.** Explain sys.getsizeof() and compare list vs NumPy array memory usage.

> [!success]- Answer
> **sys.getsizeof() - Quick memory check:**
> 
> Returns object size in bytes:
> ```python
> import sys
> 
> # Check list size
> nums_list = [*range(1000)]
> sys.getsizeof(nums_list)
> # Output: 9112 bytes
> ```
> 
> **List vs NumPy array comparison:**
> 
> ```python
> import numpy as np
> 
> # List
> nums_list = [*range(1000)]
> size_list = sys.getsizeof(nums_list)
> # 9112 bytes
> 
> # NumPy array
> nums_np = np.array(range(1000))
> size_np = sys.getsizeof(nums_np)
> # 8096 bytes
> ```
> 
> **NumPy uses ~1000 bytes less (11% savings)**
> 
> **Why NumPy is more efficient:**
> - Homogeneous data type (no type info per element)
> - Contiguous memory layout
> - Fixed-size elements
> - Less overhead per element
> 
> **Lists use more because:**
> - Each element stores type information
> - Pointer overhead for each element
> - More flexible but less memory-efficient
> 
> **When it matters:** Large datasets, memory-constrained environments

**Q54.** What is memory_profiler and how does it differ from line_profiler? Include the import requirement.

> [!success]- Answer
> **memory_profiler - Line-by-line memory analysis:**
> 
> **Installation:**
> ```bash
> pip install memory_profiler
> ```
> 
> **Setup:**
> ```python
> %load_ext memory_profiler
> ```
> 
> **CRITICAL Difference - Import Requirement:**
> 
> ```python
> # Must save function to file first!
> # hero_funcs.py
> def convert_units(heroes, heights, weights):
>     # function code
>     pass
> 
> # Then import and profile
> from hero_funcs import convert_units
> %mprun -f convert_units convert_units(heroes, hts, wts)
> ```
> 
> **line_profiler vs memory_profiler:**
> 
> | Aspect | line_profiler | memory_profiler |
> |--------|---------------|-----------------|
> | **Measures** | Runtime | Memory usage |
> | **Command** | `%lprun` | `%mprun` |
> | **Import needed** | ❌ No | ✅ YES (from file) |
> | **Shows** | Time per line | Memory per line |
> 
> **Why import requirement?**
> - memory_profiler needs to reload function
> - Must be in separate module
> - Can't profile inline-defined functions

**Q55.** Explain the %mprun output format, including caveats about small datasets.

> [!success]- Answer
> **Output Format:**
> ```
> Line #  Mem usage  Increment  Occurrences  Line Contents
> ==========================================================
>      1   50.0 MiB   50.0 MiB            1   def convert_units(...):
>      2   50.2 MiB    0.2 MiB            1       new_hts = [...]
>      3   50.4 MiB    0.2 MiB            1       new_wts = [...]
> ```
> 
> **Column Meanings:**
> 
> | Column | Description |
> |--------|-------------|
> | **Line #** | Line number |
> | **Mem usage** | **Cumulative** memory at line |
> | **Increment** | Memory **added** by this line |
> | **Occurrences** | Times line executed |
> | **Line Contents** | The code |
> 
> **Key metric: Increment**
> - Shows memory added by each line
> - Find memory hogs by highest increment
> 
> **CAVEATS:**
> 
> **1. Small datasets show 0.0 MiB:**
> ```python
> # With 480 heroes: may show 0.0 MiB increments
> # Memory profiler has limited precision
> ```
> 
> **Solution:** Use larger datasets (35,000+ rows) for meaningful results
> 
> **2. Platform differences:**
> - Queries OS for memory info
> - Results vary between platforms
> - Results may differ between runs
> 
> **3. Use for relative comparison:**
> - Don't rely on absolute values
> - Compare lines within same run
> - Focus on which lines use MORE memory
> 
> **Best practice:** Look for patterns, not exact values

---

## Section D: Code Analysis & Scenarios (25 points)
*2.5 points each*

**Q56.** What's the output and what does it tell you?

```python
%timeit nums = [x for x in range(10)]
```

Output: `914 ns ± 7.33 ns per loop`

> [!success]- Answer
> **Output interpretation:**
> 
> **Average time:** 914 nanoseconds per loop
> **Variability:** ± 7.33 ns standard deviation
> 
> **What it tells us:**
> - List comprehension for 10 elements takes ~914 ns
> - Very low variability (consistent performance)
> - Fast operation (nanoseconds)
> 
> **Context:** This is faster than the for loop with append() which took 1.17 μs (1170 ns).

**Q57.** Compare these two outputs. Which approach is better?

```python
%timeit formal_list = list()
# 120 ns ± 2 ns

%timeit literal_list = []
# 75 ns ± 1.5 ns
```

> [!success]- Answer
> **Better approach:** `literal_list = []`
> 
> **Analysis:**
> - Literal syntax: 75 ns
> - Formal syntax: 120 ns
> - **Difference:** 45 ns (literal is 60% faster)
> 
> **Why literal wins:**
> - Direct bytecode operation
> - No function call overhead
> - More Pythonic
> 
> **Recommendation:** Always use literal syntax `[]`, `{}`, `()` for creating data structures.

**Q58.** Analyze this %lprun output. Which line is the bottleneck?

```
Line #  Hits  Time      Per Hit   % Time  Line Contents
===========================================================
     2     1  1200.0    1200.0     45.0   new_hts = [...]
     3     1   800.0     800.0     30.0   new_wts = [...]
     4     1    50.0      50.0      2.0   hero_data = {}
     5     3   600.0     200.0     23.0   for i,hero...
```

> [!success]- Answer
> **Bottleneck: Line 2 (45% of time)**
> 
> **Analysis by line:**
> - **Line 2:** 45% time, 1200.0 μs → **Main bottleneck**
> - **Line 3:** 30% time, 800.0 μs → Secondary bottleneck
> - **Line 5:** 23% time, 600.0 μs, 3 hits → Loop overhead
> - **Line 4:** 2% time, 50.0 μs → Not significant
> 
> **Optimization priority:**
> 1. Focus on Line 2 first (biggest impact)
> 2. Then Line 3
> 3. Line 5 acceptable (loop necessary)
> 
> **Combined Lines 2+3:** 75% of total time → optimize these first!

**Q59.** What does this code do? What information does it provide?

```python
times = %timeit -o -r3 -n100 code_to_test
print(times.best)
print(times.worst)
print(times.average)
```

> [!success]- Answer
> **What it does:**
> Times code with custom parameters and saves detailed results.
> 
> **Parameters:**
> - `-o`: Save output to `times` variable
> - `-r3`: Run 3 times
> - `-n100`: 100 loops per run
> 
> **Information extracted:**
> 
> ```python
> times.best      # Fastest execution time
> times.worst     # Slowest execution time
> times.average   # Mean of all runs
> ```
> 
> **Use case:**
> - Compare multiple approaches
> - Calculate performance differences
> - Track best-case vs worst-case performance
> 
> **Example analysis:**
> If `best=100ns`, `worst=150ns`, `average=120ns`:
> - Good consistency (50ns range)
> - Average close to best (stable performance)

**Q60.** Debug this code. What's wrong?

```python
def slow_function():
    result = []
    for i in range(1000):
        result.append(i ** 2)
    return result

%lprun -f slow_function slow_function()
```

> [!success]- Answer
> **Problem:** Need to load line_profiler extension first!
> 
> **Fixed code:**
> ```python
> # Load extension first
> %load_ext line_profiler
> 
> def slow_function():
>     result = []
>     for i in range(1000):
>         result.append(i ** 2)
>     return result
> 
> # Now profiling works
> %lprun -f slow_function slow_function()
> ```
> 
> **Common mistake:** Forgetting `%load_ext line_profiler` before using `%lprun`.
> 
> **Remember:** Extensions must be loaded once per session.

**Q61.** Complete this code to time both approaches and find which is faster:

```python
# Time approach 1
t1 = %timeit ______ nums1 = [x**2 for x in range(100)]

# Time approach 2
t2 = %timeit ______ nums2 = list(map(lambda x: x**2, range(100)))

# Calculate difference in nanoseconds
diff = (______ - ______) * 1e9
print(f"Difference: {diff:.2f} ns")
```

> [!success]- Answer
> **Completed code:**
> ```python
> # Time approach 1
> t1 = %timeit -o nums1 = [x**2 for x in range(100)]
> 
> # Time approach 2
> t2 = %timeit -o nums2 = list(map(lambda x: x**2, range(100)))
> 
> # Calculate difference in nanoseconds
> diff = (t1.average - t2.average) * 1e9
> print(f"Difference: {diff:.2f} ns")
> ```
> 
> **Explanation:**
> - `-o` flag saves output
> - `.average` gets mean time
> - `* 1e9` converts seconds to nanoseconds
> - Result shows which is faster

**Q62.** This memory profile shows a problem. What is it?

```python
from hero_funcs import convert_units
%mprun -f convert_units convert_units(heroes, hts, wts)
```

Output shows all Increment values as 0.0 MiB

> [!success]- Answer
> **Problem:** Dataset is too small for memory_profiler precision.
> 
> **Why this happens:**
> - memory_profiler has limited precision
> - Small memory allocations (< 0.1 MiB) may show as 0.0
> - Original dataset (480 heroes) is too small
> 
> **Solutions:**
> 
> **1. Use larger dataset:**
> ```python
> # Use 35,000+ heroes instead of 480
> large_heroes = heroes * 100  # Repeat data
> %mprun -f convert_units convert_units(large_heroes, hts, wts)
> ```
> 
> **2. Focus on relative comparisons:**
> - Even if showing 0.0, some lines may show more
> - Compare which lines allocate vs don't
> 
> **3. Use sys.getsizeof() for small objects:**
> ```python
> import sys
> print(sys.getsizeof(new_hts))
> ```
> 
> **Note:** This doesn't mean no memory was used - just too small to register!

**Q63.** What's wrong with this code?

```python
def process_data():
    data = [i for i in range(1000)]
    return data

%mprun -f process_data process_data()
```

> [!success]- Answer
> **Problem:** Function not imported from external file!
> 
> **Error message:** Will likely say function needs to be imported
> 
> **Why:** `%mprun` requires functions to be in separate files and imported.
> 
> **Fixed code:**
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
> **Key difference:**
> - `%lprun`: Can profile functions defined in notebook ✅
> - `%mprun`: Must import from file ✅

**Q64.** Write code to compare list comprehension vs map for squaring numbers. Use %timeit and save results.

> [!success]- Answer
> **Complete solution:**
> 
> ```python
> # Time list comprehension
> lc_time = %timeit -o [x**2 for x in range(1000)]
> print(f"List comprehension: {lc_time.average * 1e6:.2f} μs")
> 
> # Time map with lambda
> map_time = %timeit -o list(map(lambda x: x**2, range(1000)))
> print(f"Map with lambda: {map_time.average * 1e6:.2f} μs")
> 
> # Compare
> if lc_time.average < map_time.average:
>     faster = "List comprehension"
>     diff = (map_time.average - lc_time.average) * 1e9
> else:
>     faster = "Map"
>     diff = (lc_time.average - map_time.average) * 1e9
> 
> print(f"\n{faster} is faster by {diff:.2f} ns")
> ```
> 
> **Expected result:** List comprehension typically wins
> 
> **Why:** List comprehensions are optimized in CPython, map has lambda overhead.

**Q65.** Given this %lprun output, what optimization would you suggest?

```
Line #  Hits  Time      Per Hit   % Time  Line Contents
===========================================================
     2     1   100.0     100.0      5.0   data = []
     3  1000  1800.0       1.8     90.0   for i in range(1000):
     4  1000   100.0       0.1      5.0       data.append(i**2)
```

> [!success]- Answer
> **Analysis:**
> - Line 3 (loop header): 90% of time!
> - Loop runs 1000 times
> - Append itself only 5% (efficient)
> 
> **Problem:** Loop overhead is the bottleneck
> 
> **Optimization: Use list comprehension**
> 
> **Before (slow):**
> ```python
> data = []
> for i in range(1000):
>     data.append(i**2)
> ```
> 
> **After (fast):**
> ```python
> data = [i**2 for i in range(1000)]
> ```
> 
> **Why this helps:**
> - Eliminates loop overhead
> - Single optimized operation
> - List comprehensions are implemented in C
> - Should reduce time by ~90%
> 
> **Even better with NumPy:**
> ```python
> import numpy as np
> data = np.arange(1000) ** 2
> # Broadcasting - even faster!
> ```

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)
1. B | 2. B | 3. C | 4. B | 5. B | 6. C | 7. B | 8. A | 9. B | 10. C
2. B | 12. B | 13. A | 14. B | 15. B | 16. B | 17. B | 18. B | 19. B | 20. B
3. B | 22. B | 23. B | 24. B | 25. B | 26. B | 27. B | 28. B | 29. B | 30. B

### Section B: True/False (15 points)
31. T | 32. F | 33. T | 34. T | 35. T | 36. T | 37. F | 38. T | 39. T | 40. T
32. F | 42. F | 43. T | 44. T | 45. F

### Section C: Short Answer (30 points)
Questions 46-55: Award 3 points each for complete, accurate responses with code examples.

### Section D: Code Analysis (25 points)
Questions 56-65: Award 2.5 points each for correct analysis and explanations.

**Total: 130 points**
**Passing Score: 91 points (70%)**

---

## Quick Study Guide

### Essential Commands

```python
# Magic commands
%lsmagic                    # List all magic commands

# Timing
%timeit code               # Time single line
%%timeit                   # Time cell (multiple lines)
%timeit -r2 -n10 code     # Custom runs/loops
times = %timeit -o code    # Save output

# Runtime profiling
%load_ext line_profiler
%lprun -f func func()     # Line-by-line time

# Memory profiling
%load_ext memory_profiler
from file import func     # Must import!
%mprun -f func func()     # Line-by-line memory

# Quick memory check
import sys
sys.getsizeof(obj)        # Object size in bytes
```

### Saved %timeit Output

```python
times = %timeit -o code

times.timings    # List of all times
times.best       # Fastest time
times.worst      # Slowest time
times.average    # Mean time
```

### Time Units

| Unit | Symbol | Value |
|------|--------|-------|
| Seconds | s | 1 |
| Milliseconds | ms | 10⁻³ |
| Microseconds | μs | 10⁻⁶ |
| Nanoseconds | ns | 10⁻⁹ |

---

## Key Concepts Summary

### When to Use Each Tool

| Tool | Purpose | Use When |
|------|---------|----------|
| `%timeit` | Overall timing | Comparing approaches |
| `%lprun` | Runtime profiling | Finding bottlenecks |
| `sys.getsizeof()` | Quick memory | Checking object size |
| `%mprun` | Memory profiling | Line-by-line memory |

### %lprun vs %mprun

| Aspect | %lprun | %mprun |
|--------|--------|--------|
| **Measures** | Runtime | Memory |
| **Import needed** | No | Yes (from file) |
| **Command** | `%lprun -f func func()` | `%mprun -f func func()` |
| **Package** | line_profiler | memory_profiler |

### Comparison Results

| Test | Winner | Time |
|------|--------|------|
| **Literal vs Formal** | Literal `{}` | ~52 ns faster |
| **List Comp vs Loop** | List Comp | ~256 ns faster |
| **List vs NumPy** | NumPy (memory) | ~1000 bytes less |

---

## Common Mistakes to Avoid

❌ **Forgetting to load extensions**
```python
# WRONG
%lprun -f func func()  # Error!

# CORRECT
%load_ext line_profiler
%lprun -f func func()
```

❌ **Not importing for %mprun**
```python
# WRONG
def func():
    pass
%mprun -f func func()  # Error!

# CORRECT
# Save to file.py
from file import func
%mprun -f func func()
```

❌ **Confusing line vs cell magic**
```python
# WRONG
%timeit
code1
code2  # Only times first line!

# CORRECT
%%timeit
code1
code2  # Times entire cell
```

❌ **Not saving output for comparison**
```python
# WRONG
%timeit code1
%timeit code2
# Can't compare programmatically

# CORRECT
t1 = %timeit -o code1
t2 = %timeit -o code2
diff = t1.average - t2.average
```

❌ **Using %mprun with small datasets**
```python
# Might show all 0.0 MiB
%mprun -f func func(small_data)

# Use larger datasets
%mprun -f func func(large_data)
```

---

## Study Strategy

### Day 1: Magic Commands & %timeit
- [ ] Understand magic commands (% prefix)
- [ ] Learn %timeit basics
- [ ] Practice line vs cell magic
- [ ] Understand output format
- [ ] Learn -r and -n flags

### Day 2: Advanced %timeit
- [ ] Practice saving output with -o
- [ ] Access .best, .worst, .average
- [ ] Compare formal vs literal syntax
- [ ] Calculate time differences
- [ ] Compare multiple approaches

### Day 3: Runtime Profiling
- [ ] Install line_profiler
- [ ] Learn %load_ext line_profiler
- [ ] Practice %lprun syntax
- [ ] Understand output columns
- [ ] Identify bottlenecks

### Day 4: Memory Profiling - Part 1
- [ ] Learn sys.getsizeof()
- [ ] Compare list vs array memory
- [ ] Install memory_profiler
- [ ] Understand import requirement

### Day 5: Memory Profiling - Part 2
- [ ] Practice %mprun workflow
- [ ] Understand output format
- [ ] Learn about caveats
- [ ] Practice with large datasets

### Day 6: Integration
- [ ] Combine all tools
- [ ] Complete optimization workflow
- [ ] Practice real examples
- [ ] Compare before/after

### Day 7: Review & Practice
- [ ] Review all commands
- [ ] Complete practice quiz
- [ ] Focus on weak areas
- [ ] Practice common patterns

---

## Final Exam Checklist

### Before the Exam ✓

**Magic Commands:**
- [ ] Know % prefix for magic commands
- [ ] Understand line (%) vs cell (%%) magic
- [ ] Know %lsmagic command

**%timeit:**
- [ ] Know basic syntax
- [ ] Understand output format
- [ ] Can use -r and -n flags
- [ ] Can save output with -o
- [ ] Know .best, .worst, .average

**Comparisons:**
- [ ] Know literal faster than formal
- [ ] List comp faster than for loop
- [ ] NumPy uses less memory than list

**Profiling:**
- [ ] Know line_profiler for runtime
- [ ] Know memory_profiler for memory
- [ ] Understand %lprun syntax
- [ ] Know %mprun needs import
- [ ] Can interpret profiler output

### During the Exam ✓

**Quick Checks:**
- [ ] % for line, %% for cell
- [ ] -o to save output
- [ ] Literal `{}` faster than `dict()`
- [ ] %lprun needs %load_ext
- [ ] %mprun needs import from file
- [ ] Focus on "% Time" for bottlenecks
- [ ] Check "Increment" for memory

**Common Traps:**
- [ ] Forgetting %load_ext
- [ ] Using %mprun without import
- [ ] Confusing line vs cell magic
- [ ] Not using -o to save
- [ ] Misinterpreting profiler output

---

## Congratulations! 🎉

You've mastered:
- ✅ Using IPython magic commands
- ✅ Timing code with %timeit
- ✅ Comparing different approaches
- ✅ Runtime profiling with %lprun
- ✅ Memory profiling with %mprun
- ✅ Identifying performance bottlenecks

**You're ready to profile and optimize Python code!** ⚡🐍

---

#python #efficiency #profiling #timing #ipython #optimization #performance #datacamp #magic-commands