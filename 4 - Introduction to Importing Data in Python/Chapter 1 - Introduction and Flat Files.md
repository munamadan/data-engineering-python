
## 📚 Chapter Overview

**What We'll Import:**

- Flat files (.txt, .csv)
- Files from other software
- Relational databases

---

## 📄 Plain Text Files

### Reading a Text File

**Basic Approach:**

```python
filename = 'huck_finn.txt'
file = open(filename, mode='r')  # 'r' is to read
text = file.read()
file.close()

print(text)
```

**Important:** Always close the file after reading!

### Writing to a File

```python
filename = 'huck_finn.txt'
file = open(filename, mode='w')  # 'w' is to write
file.close()
```

### Context Manager (Best Practice)

**Using `with` statement:**

```python
with open('huck_finn.txt', 'r') as file:
    print(file.read())
```

**Advantages:**

- Automatically closes the file
- Cleaner code
- Exception-safe
- No need to call `file.close()`

---

## 📊 Flat Files

### What are Flat Files?

**Definition:** Text files containing records (table data)

**Key Concepts:**

- **Record:** Row of fields or attributes
- **Column:** Feature or attribute
- **Header:** First row containing column names

### Common File Extensions

|Extension|Type|Delimiter|
|---|---|---|
|`.csv`|Comma Separated Values|Comma (`,`)|
|`.txt`|Text file|Various (comma, tab, etc.)|

### Example: CSV File

**titanic.csv:**

```
PassengerId,Survived,Pclass,Name,Gender,Age,SibSp,Parch,Ticket,Fare,Cabin,Embarked
1,0,3,"Braund, Mr. Owen Harris",male,22,1,0,A/5 21171,7.25,,S
2,1,1,"Cumings, Mrs. John Bradley",female,38,1,0,PC 17599,71.2833,C85,C
3,1,3,"Heikkinen, Miss. Laina",female,26,0,0,STON/O2.3101282,7.925,,S
```

**Structure:**

- **Header:** Column names (PassengerId, Survived, etc.)
- **Records:** Data rows
- **Delimiter:** Comma separates values

### Tab-Delimited Files

**MNIST.txt:**

```
pixel149    pixel150    pixel151    pixel152    pixel153
0           0           0           0           0
86          250         254         254         254
0           0           0           9           254
```

**Delimiter:** Tab character separates values

---

## 🔢 Importing with NumPy

### Why NumPy?

**Reasons:**

1. **NumPy arrays** are the standard for storing numerical data
2. **Essential** for other packages (e.g., scikit-learn)
3. Efficient for numerical operations

**Key Functions:**

- `loadtxt()` - Load data from text file
- `genfromtxt()` - More flexible loading

### Basic Import

```python
import numpy as np

filename = 'MNIST.txt'
data = np.loadtxt(filename, delimiter=',')

print(data)
```

**Output:**

```
[[ 0.   0.   0.   0.   0.]
 [ 86. 250. 254. 254. 254.]
 [ 0.   0.   0.   9. 254.]
 ...
 [ 0.   0.   0.   0.   0.]]
```

### Customizing NumPy Import

#### Skip Rows (Header)

```python
filename = 'MNIST_header.txt'
data = np.loadtxt(filename, delimiter=',', skiprows=1)
```

**`skiprows`:** Number of rows to skip from the beginning

#### Select Specific Columns

```python
data = np.loadtxt(filename, delimiter=',', skiprows=1, usecols=[0, 2])
```

**`usecols`:** List of column indices to keep

**Output:**

```
[[ 0.   0.]
 [ 86. 254.]
 [ 0.   0.]
 ...]
```

#### Specify Data Type

```python
data = np.loadtxt(filename, delimiter=',', dtype=str)
```

**`dtype`:** Data type for the array (str, float, int, etc.)

### NumPy Parameters Summary

|Parameter|Purpose|Example|
|---|---|---|
|`filename`|File to load|`'data.csv'`|
|`delimiter`|Character separating values|`','` or `'\t'`|
|`skiprows`|Number of rows to skip|`skiprows=1`|
|`usecols`|Columns to keep (by index)|`usecols=[0, 2, 5]`|
|`dtype`|Data type|`dtype=str` or `dtype=float`|

### Limitation: Mixed Datatypes

**Problem:** NumPy struggles with mixed data types

**Example: titanic.csv**

```
Name                        Sex      Cabin    Fare
Braund, Mr. Owen Harris     male     NaN      7.3
Cumings, Mrs. John Bradley  female   C85      71.3
```

- **Strings:** Name, Sex, Cabin
- **Floats:** Fare

**Solution:** Use pandas for mixed datatypes!

---

## 🐼 Importing with pandas

### What Data Scientists Need

**Requirements:**

1. Two-dimensional labeled data structures
2. Columns of potentially different types
3. Ability to manipulate, slice, reshape
4. Support for groupby, join, merge operations
5. Statistical functions
6. Time series capabilities

### The pandas DataFrame

**Definition:** Pythonic analog of R's data frame

**Key Features:**

- Labeled rows and columns
- Mixed data types per column
- Industry standard for data analysis
- Essential for exploratory data analysis (EDA)
- Used for data wrangling and preprocessing

### Basic Import with pandas

```python
import pandas as pd

filename = 'winequality-red.csv'
data = pd.read_csv(filename)

# View first 5 rows
data.head()
```

**Output:**

```
   volatile acidity  citric acid  residual sugar
0              0.70         0.00             1.9
1              0.88         0.00             2.6
2              0.76         0.04             2.3
3              0.28         0.56             1.9
4              0.70         0.00             1.9
```

### Converting to NumPy Array

```python
data_array = data.to_numpy()
```

**Use case:** When you need NumPy array for scikit-learn or other libraries

### pandas Use Cases

**Common Applications:**

- **Exploratory data analysis (EDA)**
- **Data wrangling** - Cleaning and transforming data
- **Data preprocessing** - Preparing data for models
- **Building models** - Integration with ML libraries
- **Visualization** - Plotting and graphing

**Status:** Standard and best practice in data science

---

## 📊 NumPy vs pandas Comparison

|Feature|NumPy|pandas|
|---|---|---|
|**Best For**|Numerical data only|Mixed datatypes|
|**Data Structure**|ndarray|DataFrame|
|**Headers**|Not supported natively|Labeled columns|
|**Use Case**|Mathematical operations|Data analysis & wrangling|
|**Mixed Types**|❌ Difficult|✅ Easy|
|**Statistical Functions**|Basic|Comprehensive|
|**Industry Standard**|Numerical computing|Data analysis|

---

## 🎯 Key Takeaways

### File Reading Methods

1. **Plain text files:**
    
    - `open()` with `file.read()`
    - Always use `with` statement (context manager)
2. **Numerical flat files:**
    
    - Use `np.loadtxt()` for pure numerical data
    - Customize with `delimiter`, `skiprows`, `usecols`, `dtype`
3. **Mixed datatype flat files:**
    
    - Use `pd.read_csv()` for versatility
    - Returns DataFrame with labeled columns
    - Convert to NumPy with `.to_numpy()` if needed

### Best Practices

✅ **Use context managers** (`with` statement) for file handling ✅ **Use NumPy** for numerical-only data and mathematical operations ✅ **Use pandas** for data analysis, mixed types, and data wrangling ✅ **Check file structure** (delimiter, header) before importing ✅ **Preview data** with `.head()` after importing with pandas

### Common File Issues

You'll learn to handle:

- Files with comments
- Files with missing values
- Files without headers
- Different delimiters
- Mixed data types

---

## 🔜 Next Topics

### This Course:

- Import other file types (Excel, SAS, Stata)
- Interact with relational databases

### Future Courses:

- Scrape data from the web
- Interact with APIs

---

## 💡 Quick Reference

### Context Manager Pattern

```python
with open('file.txt', 'r') as file:
    content = file.read()
    # File automatically closed after this block
```

### NumPy Import Pattern

```python
import numpy as np
data = np.loadtxt('file.csv', 
                  delimiter=',', 
                  skiprows=1, 
                  usecols=[0, 2])
```

### pandas Import Pattern

```python
import pandas as pd
df = pd.read_csv('file.csv')
df.head()  # Preview
```

---

#python #data-import #numpy #pandas #flat-files #csv #datacamp

## Section A: Multiple Choice (20 Questions)

**Q1.** What does the 'r' mode mean in `open(filename, mode='r')`?

- [ ] A) Run
- [ ] B) Read
- [ ] C) Retrieve
- [ ] D) Return

**Q2.** What is a flat file?

- [ ] A) Compressed data file
- [ ] B) Text file containing records (table data)
- [ ] C) Binary file
- [ ] D) Encrypted file

**Q3.** What delimiter is used in CSV files?

- [ ] A) Tab
- [ ] B) Semicolon
- [ ] C) Comma
- [ ] D) Space

**Q4.** What is a record in a flat file?

- [ ] A) A column
- [ ] B) A row of fields or attributes
- [ ] C) The file extension
- [ ] D) The delimiter

**Q5.** What is the header in a flat file?

- [ ] A) The last row
- [ ] B) The first row containing column names
- [ ] C) The file metadata
- [ ] D) The delimiter type

**Q6.** Which NumPy function is used to load data from text files?

- [ ] A) np.load()
- [ ] B) np.read()
- [ ] C) np.loadtxt()
- [ ] D) np.import()

**Q7.** What does the `skiprows` parameter do in `np.loadtxt()`?

- [ ] A) Skips columns
- [ ] B) Skips the specified number of rows from the beginning
- [ ] C) Skips empty rows only
- [ ] D) Skips the last rows

**Q8.** What does the `usecols` parameter accept in `np.loadtxt()`?

- [ ] A) Column names as strings
- [ ] B) List of column indices to keep
- [ ] C) Number of columns
- [ ] D) Boolean values

**Q9.** Why is NumPy preferred for numerical data?

- [ ] A) It's faster for text processing
- [ ] B) NumPy arrays are standard for storing numerical data
- [ ] C) It handles strings better
- [ ] D) It's easier to learn

**Q10.** What is a limitation of NumPy for importing data?

- [ ] A) Can't handle large files
- [ ] B) Too slow
- [ ] C) Struggles with mixed datatypes
- [ ] D) No customization options

**Q11.** What function in pandas is used to read CSV files?

- [ ] A) pd.load_csv()
- [ ] B) pd.read_csv()
- [ ] C) pd.import_csv()
- [ ] D) pd.open_csv()

**Q12.** What does `data.head()` do in pandas?

- [ ] A) Returns the last 5 rows
- [ ] B) Returns the first 5 rows
- [ ] C) Returns column headers only
- [ ] D) Returns data types

**Q13.** How do you convert a pandas DataFrame to a NumPy array?

- [ ] A) data.to_array()
- [ ] B) data.to_numpy()
- [ ] C) data.array()
- [ ] D) np.array(data)

**Q14.** What is a DataFrame?

- [ ] A) NumPy array with labels
- [ ] B) Pythonic analog of R's data frame
- [ ] C) A type of list
- [ ] D) A database table

**Q15.** Why use a context manager (`with` statement) for file operations?

- [ ] A) It's faster
- [ ] B) It automatically closes the file
- [ ] C) It compresses the file
- [ ] D) It's required by Python

**Q16.** What character is typically used as a delimiter in tab-delimited files?

- [ ] A) Space
- [ ] B) Comma
- [ ] C) Tab (`\t`)
- [ ] D) Pipe (`|`)

**Q17.** Which is standard practice for data analysis in Python?

- [ ] A) Using only NumPy
- [ ] B) Using only base Python
- [ ] C) Using pandas for DataFrames
- [ ] D) Writing custom parsers

**Q18.** What does `dtype=str` do in `np.loadtxt()`?

- [ ] A) Filters string columns
- [ ] B) Converts all data to strings
- [ ] C) Removes strings
- [ ] D) Counts strings

**Q19.** What capability does pandas provide that NumPy doesn't?

- [ ] A) Mathematical operations
- [ ] B) Labeled columns with mixed datatypes
- [ ] C) Array indexing
- [ ] D) Numerical computations

**Q20.** Which mode is used to write to a file?

- [ ] A) 'r'
- [ ] B) 'w'
- [ ] C) 'a'
- [ ] D) 'x'

---

## Section B: True/False (10 Questions)

**Q21.** A flat file can only contain numerical data. **T/F**

**Q22.** The `with` statement automatically closes files. **T/F**

**Q23.** CSV stands for Comma Separated Values. **T/F**

**Q24.** NumPy arrays are the standard for storing numerical data in Python. **T/F**

**Q25.** pandas DataFrames can handle mixed datatypes easily. **T/F**

**Q26.** `skiprows=1` skips the first column. **T/F**

**Q27.** The header is the last row of a flat file. **T/F**

**Q28.** pandas is the pythonic analog of R's data frame. **T/F**

**Q29.** You must always call `file.close()` when using context managers. **T/F**

**Q30.** `usecols=[0, 2]` keeps only the first and third columns. **T/F**

---

## Section C: Code Analysis (5 Questions)

**Q31.** What will this code do?

```python
with open('data.txt', 'r') as file:
    content = file.read()
```

- [ ] A) Write to data.txt
- [ ] B) Read data.txt and store in content
- [ ] C) Delete data.txt
- [ ] D) Create data.txt

**Q32.** What's wrong with this code?

```python
file = open('data.txt', 'r')
data = file.read()
```

- [ ] A) Wrong mode
- [ ] B) Missing file.close()
- [ ] C) Syntax error
- [ ] D) Nothing wrong

**Q33.** What will `data` contain?

```python
import numpy as np
data = np.loadtxt('file.csv', delimiter=',', skiprows=1, usecols=[0])
```

- [ ] A) All columns from the file
- [ ] B) Only the first column, skipping the header
- [ ] C) Only the first row
- [ ] D) The entire file

**Q34.** What does this return?

```python
import pandas as pd
df = pd.read_csv('data.csv')
df.head()
```

- [ ] A) Last 5 rows
- [ ] B) First 5 rows
- [ ] C) Column names
- [ ] D) File size

**Q35.** How do you fix this code to skip the header?

```python
data = np.loadtxt('file.csv', delimiter=',')
```

- [ ] A) Add `header=1`
- [ ] B) Add `skiprows=1`
- [ ] C) Add `skip_header=True`
- [ ] D) Add `ignore_header=1`

---

## Section D: Short Answer (5 Questions)

**Q36.** Explain the difference between NumPy and pandas for importing data. When would you use each?

**Q37.** What are the three main components of a flat file structure?

**Q38.** Why is using a context manager (`with` statement) considered best practice?

**Q39.** What parameters can you use to customize `np.loadtxt()` and what does each do?

**Q40.** List three use cases where pandas DataFrames are preferred over NumPy arrays.

---

## Answer Key

### Section A

1. B 2. B 3. C 4. B 5. B 6. C 7. B 8. B 9. B 10. C
2. B 12. B 13. B 14. B 15. B 16. C 17. C 18. B 19. B 20. B

### Section B

21. F 22. T 23. T 24. T 25. T 26. F 27. F 28. T 29. F 30. T

### Section C

31. B 32. B 33. B 34. B 35. B

---

## Quick Reference

### File Operations

```python
# Context manager (best practice)
with open('file.txt', 'r') as file:
    data = file.read()
```

### NumPy Import

```python
import numpy as np
data = np.loadtxt('file.csv', 
                  delimiter=',',
                  skiprows=1,
                  usecols=[0, 2],
                  dtype=float)
```

### pandas Import

```python
import pandas as pd
df = pd.read_csv('file.csv')
df.head()  # Preview first 5 rows
```

### Key Concepts

- **Flat file:** Text file with table data
- **Record:** Row of data
- **Header:** First row with column names
- **Delimiter:** Character separating values (comma, tab)
- **Context manager:** `with` statement for automatic file closing

---

#python #data-import #quiz #numpy #pandas #flat-files