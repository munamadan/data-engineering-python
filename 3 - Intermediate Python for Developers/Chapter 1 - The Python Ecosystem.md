
## 📚 Topics Covered

- Built-in Functions
- Custom Functions
- Modules
- Packages

---

## 🔧 Built-in Functions

### Core Functions Overview

#### `max()` and `min()`

- **Purpose:** Find largest/smallest values in data structures
- **Example:**

```python
sales = [125.97, 84.32, 99.78, 154.21, 78.50, 83.67, 111.13]
max(sales)  # Returns: 154.21
min(sales)  # Returns: 78.5
```

#### `sum()` and `round()`

- **`sum()`:** Adds all elements in a data structure
- **`round()`:** Trims float to specified decimal places

```python
sum(sales)  # Returns: 737.5799999999999
total_sales = sum(sales)
round(total_sales, 2)  # Returns: 737.58
```

#### Nested Functions

- Can call functions within other functions

```python
# Instead of:
total_sales = sum(sales)
round(total_sales, 2)

# You can write:
total_sales = round(sum(sales), 2)
```

#### `len()`

- **Purpose:** Counts number of elements
- **Works with:** strings, lists, dictionaries, sets, tuples
- **Does NOT work with:** floats, integers, booleans

```python
len(sales)  # Returns: 7
len("Introduction to Programming")  # Returns: 30
len({"a": 1, "b": 2, "c": 3})  # Returns: 3

# Calculate average
sum(sales) / len(sales)  # Returns: 105.36857142857141
```

#### `sorted()`

- **Purpose:** Sort elements in ascending order
- **Works with:** int, str, lists, dictionaries, etc.

```python
sorted(sales)  # Returns: [78.5, 83.67, 84.32, 99.78, 111.13, 125.97, 154.21]
sorted("George")  # Returns: ['G', 'e', 'e', 'g', 'o', 'r']
```

#### `help()`

- **Purpose:** Get documentation about functions, variables, or values

```python
help(sorted)
# Returns documentation with signature:
# sorted(iterable, /, *, key=None, reverse=False)
```

### Benefits of Functions

- **Reusable:** Write once, use many times
- **Shorter:** Less code to accomplish tasks
- **Cleaner:** More readable and maintainable
- **Less error-prone:** Tested and optimized

### Function Reference Table

|Function|Returns|
|---|---|
|`print()`|Display output (variable values)|
|`max()`|Largest value in data structure|
|`min()`|Smallest value in data structure|
|`sum()`|Sum of all elements|
|`round()`|Float trimmed to specified decimals|
|`len()`|Count of elements|
|`sorted()`|Elements sorted in ascending order|
|`help()`|Information about function/variable/value|

📖 **Reference:** [Python Built-in Functions Documentation](https://docs.python.org/3/library/functions.html)

---

## 📦 Modules

### What are Modules?

- Python scripts (files ending with `.py`)
- Contain functions and attributes
- Can contain other modules
- ~200 built-in modules available
- Help avoid rewriting existing code

### Popular Built-in Modules

- **`os`** - Interacting with operating system
- **`collections`** - Advanced data structures
- **`string`** - String operations
- **`logging`** - Logging for testing/debugging
- **`subprocess`** - Run terminal commands

📖 **Reference:** [Python Module Index](https://docs.python.org/3/py-modindex.html)

### Importing Modules

#### Basic Import

```python
import os
type(os)  # Returns: <class 'module'>
```

#### Getting Module Documentation

```python
help(os)  # Returns extensive documentation
```

### Working with the `os` Module

#### Get Current Working Directory

```python
os.getcwd()  # Returns: '/home/georgeboorman/intermediate_python_for_developers'

# Store for repeated use
work_dir = os.getcwd()
```

#### Change Directory

```python
os.chdir("/home/georgeboorman")
os.getcwd()  # Returns: '/home/georgeboorman'

# Note: work_dir variable remains unchanged
work_dir  # Still: '/home/georgeboorman/intermediate_python_for_developers'
```

### Module Attributes vs Functions

**Key Difference:**

- **Attributes:** Have values (no parentheses)
- **Functions:** Perform tasks (use parentheses)

```python
# Attribute (no parentheses)
os.environ  # Returns environment variables dictionary

# Function (with parentheses)
os.getcwd()  # Returns current directory
```

### Selective Imports

#### Import Single Function

```python
from os import chdir
# Now use chdir() directly without os. prefix
```

#### Import Multiple Functions

```python
from os import chdir, getcwd
getcwd()  # No need for os.getcwd()
```

⚠️ **Important:** Importing whole modules requires more memory. Import specific functions when possible.

---

## 📚 Packages

### What are Packages?

- **Package = Collection of modules**
- Also called **libraries**
- Publicly available and free
- Downloaded from [PyPI](https://pypi.org/)
- Must be installed before use

### Installing Packages

#### Using Terminal/Command Prompt

```bash
python3 -m pip install <package_name>
```

**Command Breakdown:**

- `python3` - Execute Python code from terminal
- `pip` - Preferred Installer Program

#### Example: Installing pandas

```bash
python3 -m pip install pandas
```

### Importing with Aliases

```python
# Standard import
import pandas
# Must use: pandas.function_name()

# Import with alias (common practice)
import pandas as pd
# Can use: pd.function_name()
```

### Working with pandas

#### Creating a DataFrame

```python
sales = {"user_id": ["KM37", "PR19", "YU88"],
         "order_value": [197.75, 208.21, 134.99]}

sales_df = pd.DataFrame(sales)
```

**Output:**

```
  user_id  order_value
0    KM37       197.75
1    PR19       208.21
2    YU88       134.99
```

#### Reading CSV Files

```python
sales_df = pd.read_csv("sales.csv")
type(sales_df)  # Returns: pandas.core.frame.DataFrame
```

#### Preview Data

```python
sales_df.head()  # Shows first 5 rows
```

### Functions vs Methods

**Key Distinction:**

- **Function:** Code to perform a task (standalone)
- **Method:** Function specific to a data type

```python
# Built-in function (works on any iterable)
sum([1, 2, 3, 4, 5])  # Returns: 15

# pandas function (creates DataFrame)
sales_df = pd.DataFrame(sales)

# Method (specific to DataFrame)
sales_df.head()  # Only works on DataFrames
```

⚠️ **Note:** `.head()` won't work with lists, dictionaries, or other data types.

---

## 🎯 Key Takeaways

1. **Built-in functions** provide efficient, tested solutions for common tasks
2. **Modules** extend Python's capabilities without writing everything from scratch
3. **Selective imports** save memory and improve code clarity
4. **Packages** require installation but provide powerful specialized functionality
5. **Methods** are functions tied to specific data types (like DataFrame.head())
6. **Aliases** (like `pd` for pandas) make code more concise

---

## 📝 Next Steps

- Practice using built-in functions with different data types
- Explore other built-in modules (collections, string, logging)
- Learn more pandas functionality for data manipulation
- Understand when to use functions vs methods

---

#python #dataengineering #datacamp #intermediate-python


## Section A: Multiple Choice Questions

### 1. Built-in Functions - Basic Concepts

**Q1.** Which of the following data types does `len()` NOT work with?

- [ ] A) Strings
- [ ] B) Lists
- [ ] C) Dictionaries
- [x] D) Floats

**Q2.** What does the `sorted()` function return when sorting a string?

- [ ] A) A sorted string
- [x] B) A list of characters sorted alphabetically
- [ ] C) A tuple of characters
- [ ] D) An error

**Q3.** What is the correct syntax for rounding 123.456789 to 2 decimal places?

- [x] A) `round(123.456789, 2)`
- [ ] B) `round(2, 123.456789)`
- [ ] C) `123.456789.round(2)`
- [ ] D) `rounded(123.456789, 2)`

**Q4.** Which function would you use to get documentation about a Python function?

- [ ] A) `doc()`
- [ ] B) `info()`
- [x] C) `help()`
- [ ] D) `describe()`

### 2. Modules

**Q5.** What is the file extension for Python modules?

- [ ] A) `.python`
- [ ] B) `.mod`
- [x] C) `.py`
- [ ] D) `.module`

**Q6.** Which os module function returns the current working directory?

- [ ] A) `os.getdir()`
- [x] B) `os.getcwd()`
- [ ] C) `os.current_dir()`
- [ ] D) `os.pwd()`

**Q7.** What is the main difference between module attributes and functions?

- [ ] A) Attributes use parentheses, functions don't
- [x] B) Attributes have values, functions perform tasks
- [ ] C) There is no difference
- [ ] D) Attributes are faster than functions

**Q8.** Which import statement is most memory efficient?

- [ ] A) `import os`
- [ ] B) `from os import *`
- [x] C) `from os import getcwd`
- [ ] D) All are equally efficient

### 3. Packages

**Q9.** What does PyPI stand for?

- [x] A) Python Package Installer
- [ ] B) Python Package Index
- [ ] C) Python Programming Interface
- [ ] D) Preferred Python Installer

**Q10.** What is the command to install the pandas package?

- [ ] A) `python install pandas`
- [ ] B) `pip install pandas`
- [ ] C) `python3 -m pip install pandas`
- [x] D) Both B and C are correct

**Q11.** Why would you import pandas with an alias like `import pandas as pd`?

- [ ] A) It's required for pandas to work
- [x] B) To make code more concise
- [ ] C) To save memory
- [ ] D) To improve performance

**Q12.** What is the difference between a function and a method?

- [ ] A) Functions are faster than methods
- [x] B) Methods are specific to a data type, functions are not
- [ ] C) There is no difference
- [ ] D) Methods can only be used with strings

---

## Section B: Code Analysis Questions

### Q13. What will be the output?

```python
sales = [125.97, 84.32, 99.78, 154.21, 78.50]
result = round(sum(sales) / len(sales), 2)
print(result)
```

- [ ] A) 108.56
- [ ] B) 542.78
- [ ] C) 108.556
- [ ] D) 542.8

**Q14.** What is wrong with this code?

```python
from os import getcwd
result = os.getcwd()
```

- [ ] A) Nothing, it will work fine
- [ ] B) Should use `getcwd()` not `os.getcwd()`
- [ ] C) Need to import os module first
- [ ] D) getcwd requires an argument

**Q15.** Which code snippet will successfully read a CSV file?

```python
# Option A
import pandas
df = pandas.read_csv("data.csv")

# Option B
import pandas as pd
df = pd.read_csv("data.csv")

# Option C
from pandas import read_csv
df = read_csv("data.csv")
```

- [ ] A) Only A
- [ ] B) Only B
- [ ] C) Only A and B
- [ ] D) All options work

**Q16.** What will this return?

```python
sorted("Python")
```

- [ ] A) `"Phnoty"`
- [ ] B) `['P', 'h', 'n', 'o', 't', 'y']`
- [ ] C) `['P', 'y', 't', 'h', 'o', 'n']`
- [ ] D) An error

**Q17.** Identify the error:

```python
data = {"name": "John", "age": 30}
os.environ()
```

- [ ] A) Missing import statement
- [ ] B) `os.environ` should not have parentheses
- [ ] C) Both A and B
- [ ] D) No error

---

## Section C: Short Answer Questions

**Q18.** Explain the difference between nested function calls and sequential function calls. Provide an example of each.

**Q19.** List three benefits of using built-in functions instead of writing custom code.

**Q20.** What are the approximately 200 built-in modules in Python designed to help with? Name three popular modules and their purposes.

**Q21.** When you change directories using `os.chdir()`, does it affect variables that previously stored the old directory path? Explain.

**Q22.** What is the difference between a package and a module in Python?

---

## Section D: Code Writing Questions

**Q23.** Write code to:

- Create a list of test scores: [78, 92, 85, 88, 95, 73, 90]
- Find the highest and lowest scores
- Calculate the average score rounded to 2 decimal places
- Sort the scores in ascending order

**Q24.** Write code to import only the `chdir` and `getcwd` functions from the os module, then use them to:

- Get the current directory
- Change to a new directory "/home/user/documents"
- Verify the directory change

**Q25.** Create a pandas DataFrame from the following dictionary and display the first 3 rows:

```python
employees = {
    "emp_id": ["E001", "E002", "E003", "E004", "E005"],
    "salary": [50000, 62000, 58000, 71000, 49000]
}
```

**Q26.** Write code that demonstrates:

- Using `help()` to get information about the `max()` function
- Finding the length of the string "Data Engineering"
- Sorting a list `[5, 2, 8, 1, 9]` in ascending order

---

## Section E: Scenario-Based Questions

**Q27.** You need to analyze sales data stored in a CSV file. What are the steps you need to take before you can use pandas to read the file? Include installation and import steps.

**Q28.** You're working on a project that requires frequently checking and changing directories. Your teammate imports the entire os module, but you want to be more memory-efficient. How would you approach this differently?

**Q29.** You have a list of 1000 prices and need to:

- Find the total sum
- Round it to 2 decimal places
- Find the average Show how to do this efficiently using built-in functions.

**Q30.** Explain why `sales_df.head()` is a method and not a function. Can you use `.head()` on a regular Python list? Why or why not?

---

## Section F: True/False Questions

**Q31.** `len()` can be used with floats, integers, and booleans.

- [ ] True
- [ ] False

**Q32.** The `sorted()` function can accept parameters like `key` and `reverse`.

- [ ] True
- [ ] False

**Q33.** When you import a specific function using `from module import function`, you need to use the module name as a prefix.

- [ ] True
- [ ] False

**Q34.** Packages and libraries are different things in Python.

- [ ] True
- [ ] False

**Q35.** Module attributes should be called with parentheses, just like functions.

- [ ] True
- [ ] False

---

## Section G: Advanced Application

**Q36.** Compare these two approaches and explain which is better and why:

```python
# Approach 1
import os
current = os.getcwd()
os.chdir("/new/path")
os.listdir()

# Approach 2
from os import getcwd, chdir, listdir
current = getcwd()
chdir("/new/path")
listdir()
```

**Q37.** You receive this error: `AttributeError: module 'pandas' has no attribute 'DataFrame'` What are three possible causes and solutions?

**Q38.** Write a nested function call that:

- Calculates the sum of [10, 20, 30, 40, 50]
- Rounds the result to 1 decimal place
- Prints the result All in one line of code.

**Q39.** Explain the memory implications of `import pandas` vs `from pandas import DataFrame`. When would you use each?

**Q40.** Create a comparison table showing the differences between:

- Built-in functions
- Module functions
- Package functions
- Methods

---

## Answer Key Location

> Answers are provided in a separate document: `Chapter_1_Answer_Key.md`

---

## Study Tips

1. **Practice coding** each example from the chapter
2. **Experiment** with different data types for each function
3. **Read documentation** using `help()` for functions you're unsure about
4. **Try importing** different modules and explore their functions
5. **Install pandas** and practice DataFrame operations
6. **Understand the why** - don't just memorize syntax

---

#python #quiz #dataengineering #certification-prep