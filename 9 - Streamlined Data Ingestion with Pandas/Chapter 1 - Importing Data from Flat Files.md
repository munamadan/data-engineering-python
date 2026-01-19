## Overview
This chapter introduces flat file imports using pandas, focusing on CSV files and customizing the import process.

## Key Concepts

### pandas DataFrames
- **DataFrame**: pandas-specific structure for two-dimensional data
- Primary data structure used throughout pandas operations

### Flat Files
- Simple, easy-to-produce format
- Data stored as plain text (no formatting)
- One row per line
- Values separated by a delimiter
- **Most common type**: Comma-Separated Values (CSV)
- **One function to rule them all**: `pd.read_csv()`

## Basic Import Operations

### Loading CSV Files
```python
import pandas as pd

# Basic CSV import
tax_data = pd.read_csv("us_tax_data_2016.csv")
tax_data.head(4)  # Display first 4 rows
```

### Loading Other Flat Files
Use the `sep` parameter to specify different delimiters:

```python
# Tab-separated values (TSV)
tax_data = pd.read_csv("us_tax_data_2016.tsv", sep="\t")
```

## Modifying Flat File Imports

### Limiting Columns with `usecols`
Choose which columns to load (reduces memory usage):

```python
# Method 1: By column names
col_names = ['STATEFIPS', 'STATE', 'zipcode', 'agi_stub', 'N1']
tax_data_v1 = pd.read_csv('us_tax_data_2016.csv', usecols=col_names)

# Method 2: By column numbers
col_nums = [0, 1, 2, 3, 4]
tax_data_v2 = pd.read_csv('us_tax_data_2016.csv', usecols=col_nums)

# Both methods produce identical results
print(tax_data_v1.equals(tax_data_v2))  # True
```

> [!info] `usecols` accepts:
> - List of column names
> - List of column numbers
> - Function to filter column names

### Limiting Rows

#### Load First N Rows with `nrows`
```python
# Load only first 1000 rows
tax_data_first1000 = pd.read_csv('us_tax_data_2016.csv', nrows=1000)
print(tax_data_first1000.shape)  # (1000, 147)
```

#### Processing Files in Chunks
Combine `nrows` and `skiprows` to process specific row ranges:

```python
# Skip first 1000 rows, then load next 500
tax_data_next500 = pd.read_csv('us_tax_data_2016.csv',
                                nrows=500,
                                skiprows=1000,
                                header=None)
```

> [!warning] When using `skiprows`
> Set `header=None` so pandas knows there are no column names in the data

**`skiprows` accepts:**
- List of row numbers
- Number of rows to skip
- Function to filter rows

### Assigning Column Names with `names`
Supply column names using the `names` parameter:

```python
col_names = list(tax_data_first1000)  # Get column names from another df

tax_data_next500 = pd.read_csv('us_tax_data_2016.csv',
                                nrows=500,
                                skiprows=1000,
                                header=None,
                                names=col_names)
```

> [!important] Column Names Requirement
> The list passed to `names` MUST have a name for every column in your data.
> If you only need to rename a few columns, do it after the import!

## Handling Errors and Missing Data

### Common Flat File Import Issues
1. Column data types are wrong
2. Values are missing
3. Records that cannot be read by pandas

### Specifying Data Types with `dtype`
pandas automatically infers column data types, but you can override:

```python
# Check inferred data types
print(tax_data.dtypes)

# Specify data types manually
tax_data = pd.read_csv("us_tax_data_2016.csv", 
                       dtype={"zipcode": str})
```

**Format:** `dtype` takes a dictionary of `{column_name: data_type}`

### Customizing Missing Data Values

#### Default Behavior
pandas automatically interprets some values as missing/NA

#### Using `na_values`
Set custom values to be treated as missing:

```python
# Treat 0 as missing in zipcode column
tax_data = pd.read_csv("us_tax_data_2016.csv",
                       na_values={"zipcode": 0})

# Check for missing values
print(tax_data[tax_data.zipcode.isna()])
```

**`na_values` accepts:**
- Single value
- List of values
- Dictionary of columns and values

### Handling Lines with Errors

#### Error Scenario
When a file has unparseable records (wrong number of fields):

```python
# This will raise a ParserError
tax_data = pd.read_csv("us_tax_data_2016_corrupt.csv")
# pandas.errors.ParserError: Error tokenizing data. 
# C error: Expected 147 fields in line 3, saw 148
```

#### Solution: Skip Bad Lines
```python
tax_data = pd.read_csv("us_tax_data_2016_corrupt.csv",
                       error_bad_lines=False,
                       warn_bad_lines=True)
# Output: b'Skipping line 3: expected 147 fields, saw 148\n'
```

| Parameter | Purpose |
|-----------|---------|
| `error_bad_lines=False` | Skip unparseable records |
| `warn_bad_lines=True` | Display messages when records are skipped |

## Key Parameters Summary

| Parameter | Purpose | Accepts |
|-----------|---------|---------|
| `sep` | Specify delimiter | String (e.g., `"\t"`, `","`, `";"`) |
| `usecols` | Select columns to load | List of names, numbers, or function |
| `nrows` | Limit number of rows | Integer |
| `skiprows` | Skip rows | List, integer, or function |
| `header` | Row number(s) to use as column names | Integer or `None` |
| `names` | Assign column names | List of strings |
| `dtype` | Specify data types | Dictionary `{col: type}` |
| `na_values` | Custom missing value indicators | Value, list, or dictionary |
| `error_bad_lines` | Skip unparseable lines | Boolean |
| `warn_bad_lines` | Show warnings for bad lines | Boolean |

---

#datacamp #pandas #data-ingestion #flat-files #csv


## Section A: Multiple Choice Questions (30 questions × 2 points = 60 points)

### Topic 1: Flat Files and Basic Imports (10 questions)

**Q1.** What is a flat file?

- [ ] A) A compressed binary file format
- [ ] B) A plain text file with one row per line and values separated by delimiters
- [ ] C) A proprietary Excel format
- [ ] D) A database backup file

> [!success]- Answer **Correct Answer: B) A plain text file with one row per line and values separated by delimiters**
> 
> Flat files are simple, easy-to-produce formats where data is stored as plain text without formatting. Each row represents one line, and values are separated by delimiters like commas or tabs.

**Q2.** What is the most common type of flat file?

- [ ] A) Tab-separated values (TSV)
- [ ] B) Pipe-separated values (PSV)
- [ ] C) Comma-separated values (CSV)
- [ ] D) Space-separated values (SSV)

> [!success]- Answer **Correct Answer: C) Comma-separated values (CSV)**
> 
> CSV is explicitly stated as the most common flat file type in the chapter.

**Q3.** Which pandas function is used to load flat files?

- [ ] A) pd.load_csv()
- [ ] B) pd.read_file()
- [ ] C) pd.import_csv()
- [ ] D) pd.read_csv()

> [!success]- Answer **Correct Answer: D) pd.read_csv()**
> 
> The chapter states "One pandas function to load them all: read_csv()" - this single function handles all flat file types.

**Q4.** What is a pandas DataFrame?

- [ ] A) A one-dimensional array
- [ ] B) A pandas-specific structure for two-dimensional data
- [ ] C) A Python dictionary
- [ ] D) A NumPy array

> [!success]- Answer **Correct Answer: B) A pandas-specific structure for two-dimensional data**
> 
> DataFrames are the fundamental pandas structure for working with two-dimensional tabular data.

**Q5.** What parameter do you use to specify a different delimiter when reading flat files?

- [ ] A) delimiter
- [ ] B) sep
- [ ] C) separator
- [ ] D) delim

> [!success]- Answer **Correct Answer: B) sep**
> 
> The `sep` parameter is used to specify delimiters other than commas.
> 
> ```python
> tax_data = pd.read_csv("file.tsv", sep="\t")
> ```

**Q6.** How do you load a tab-separated values (TSV) file?

- [ ] A) pd.read_csv("file.tsv")
- [ ] B) pd.read_tsv("file.tsv")
- [ ] C) pd.read_csv("file.tsv", sep="\t")
- [ ] D) pd.read_csv("file.tsv", delimiter="tab")

> [!success]- Answer **Correct Answer: C) pd.read_csv("file.tsv", sep="\t")**
> 
> Use `read_csv()` with `sep="\t"` to specify tab as the delimiter. The `\t` represents a tab character.

**Q7.** What does the `.head(4)` method do?

- [ ] A) Returns the last 4 rows
- [ ] B) Returns the first 4 rows
- [ ] C) Returns every 4th row
- [ ] D) Returns 4 random rows

> [!success]- Answer **Correct Answer: B) Returns the first 4 rows**
> 
> The `.head()` method displays the first n rows of a DataFrame. By default it shows 5 rows, but you can specify any number.

**Q8.** Which of the following is NOT a characteristic of flat files?

- [ ] A) Simple and easy-to-produce
- [ ] B) Data stored as plain text
- [ ] C) One row per line
- [ ] D) Contains formatting and styling information

> [!success]- Answer **Correct Answer: D) Contains formatting and styling information**
> 
> Flat files explicitly do NOT contain formatting - they store data as plain text without any styling or formatting information.

**Q9.** What is the basic syntax for importing a CSV file named "data.csv"?

- [ ] A) pd.load("data.csv")
- [ ] B) pandas.read_csv("data.csv")
- [ ] C) pd.read_csv("data.csv")
- [ ] D) pd.import_csv("data.csv")

> [!success]- Answer **Correct Answer: C) pd.read_csv("data.csv")**
> 
> After importing pandas as pd, use `pd.read_csv()` with the filename as a string argument.
> 
> ```python
> import pandas as pd
> data = pd.read_csv("data.csv")
> ```

**Q10.** In a flat file, how are values for different fields separated?

- [ ] A) By spaces only
- [ ] B) By a delimiter
- [ ] C) By line breaks
- [ ] D) By brackets

> [!success]- Answer **Correct Answer: B) By a delimiter**
> 
> Flat files use delimiters (like commas, tabs, pipes) to separate values for different fields within a row.

### Topic 2: Modifying Imports - Limiting Data (10 questions)

**Q11.** Which parameter is used to select specific columns to load?

- [ ] A) columns
- [ ] B) select_cols
- [ ] C) usecols
- [ ] D) cols

> [!success]- Answer **Correct Answer: C) usecols**
> 
> The `usecols` parameter allows you to choose which columns to load, reducing memory usage.

**Q12.** What types of values can `usecols` accept?

- [ ] A) Only column names
- [ ] B) Only column numbers
- [ ] C) Column names, column numbers, or a function
- [ ] D) Only a function

> [!success]- Answer **Correct Answer: C) Column names, column numbers, or a function**
> 
> The chapter explicitly states that `usecols` accepts:
> 
> - A list of column numbers
> - A list of column names
> - A function to filter column names

**Q13.** Which two methods of selecting columns with `usecols` are shown in the chapter?

- [ ] A) By name and by index
- [ ] B) By name and by data type
- [ ] C) By name and by number
- [ ] D) By number and by function

> [!success]- Answer **Correct Answer: C) By name and by number**
> 
> The chapter demonstrates selecting columns both by their names (strings) and by their position numbers (integers).

**Q14.** What parameter limits the number of rows loaded?

- [ ] A) limit
- [ ] B) nrows
- [ ] C) max_rows
- [ ] D) row_limit

> [!success]- Answer **Correct Answer: B) nrows**
> 
> The `nrows` parameter specifies the maximum number of rows to load from the file.
> 
> ```python
> tax_data = pd.read_csv('file.csv', nrows=1000)
> ```

**Q15.** What does this code do: `pd.read_csv('file.csv', nrows=1000)`?

- [ ] A) Loads rows 0-999
- [ ] B) Loads every 1000th row
- [ ] C) Loads the first 1000 rows
- [ ] D) Skips the first 1000 rows

> [!success]- Answer **Correct Answer: C) Loads the first 1000 rows**
> 
> The `nrows` parameter loads the specified number of rows from the beginning of the file.

**Q16.** Which parameter is used to skip rows when loading a file?

- [ ] A) skip
- [ ] B) ignore_rows
- [ ] C) skiprows
- [ ] D) omit_rows

> [!success]- Answer **Correct Answer: C) skiprows**
> 
> The `skiprows` parameter allows you to skip specified rows when reading the file.

**Q17.** What types of values can `skiprows` accept?

- [ ] A) Only a list of row numbers
- [ ] B) Only a number of rows
- [ ] C) A list of row numbers, a number of rows, or a function
- [ ] D) Only a function

> [!success]- Answer **Correct Answer: C) A list of row numbers, a number of rows, or a function**
> 
> Like `usecols`, `skiprows` is flexible and accepts multiple input types for different use cases.

**Q18.** When using `skiprows` to process file chunks, what should you set `header` to?

- [ ] A) header=0
- [ ] B) header=1
- [ ] C) header=True
- [ ] D) header=None

> [!success]- Answer **Correct Answer: D) header=None**
> 
> The chapter emphasizes: "Set header=None so pandas knows there are no column names" when you're skipping rows that include the header.

**Q19.** What happens if you load two DataFrames using column names and column numbers with `usecols`, using the same columns?

- [ ] A) They will be different DataFrames
- [ ] B) They will be identical DataFrames
- [ ] C) An error will occur
- [ ] D) Only one will load successfully

> [!success]- Answer **Correct Answer: B) They will be identical DataFrames**
> 
> The chapter demonstrates this with `.equals()` returning `True`:
> 
> ```python
> print(tax_data_v1.equals(tax_data_v2))  # True
> ```

**Q20.** What is the purpose of combining `nrows` and `skiprows`?

- [ ] A) To load all data faster
- [ ] B) To process a file in chunks
- [ ] C) To remove duplicate rows
- [ ] D) To sort the data

> [!success]- Answer **Correct Answer: B) To process a file in chunks**
> 
> The chapter states: "Use nrows and skiprows together to process a file in chunks" - useful for large files that don't fit in memory.

### Topic 3: Column Names, Data Types, and Error Handling (10 questions)

**Q21.** Which parameter is used to assign custom column names during import?

- [ ] A) column_names
- [ ] B) names
- [ ] C) cols
- [ ] D) headers

> [!success]- Answer **Correct Answer: B) names**
> 
> The `names` parameter allows you to supply custom column names when reading a file.

**Q22.** What is a critical requirement when using the `names` parameter?

- [ ] A) Names must be in alphabetical order
- [ ] B) The list must have a name for every column in your data
- [ ] C) Names must be uppercase
- [ ] D) Names must be unique

> [!success]- Answer **Correct Answer: B) The list must have a name for every column in your data**
> 
> The chapter explicitly states: "The list MUST have a name for every column in your data" (emphasis in original).

**Q23.** According to the chapter, when should you rename columns after import instead of using `names`?

- [ ] A) When you need to rename all columns
- [ ] B) When you only need to rename a few columns
- [ ] C) When columns have numbers as names
- [ ] D) Never

> [!success]- Answer **Correct Answer: B) When you only need to rename a few columns**
> 
> The chapter advises: "If you only need to rename a few columns, do it after the import!"

**Q24.** Does pandas automatically infer column data types?

- [ ] A) No, you must always specify them
- [ ] B) Yes, but you can override with dtype
- [ ] C) Only for numeric columns
- [ ] D) Only if you set auto_dtype=True

> [!success]- Answer **Correct Answer: B) Yes, but you can override with dtype**
> 
> The chapter states: "pandas automatically infers column data types" but shows you can use `dtype` to specify them manually.

**Q25.** What parameter is used to specify column data types during import?

- [ ] A) types
- [ ] B) dtypes
- [ ] C) dtype
- [ ] D) data_types

> [!success]- Answer **Correct Answer: C) dtype**
> 
> Use the `dtype` parameter to specify data types for columns.

**Q26.** What format does the `dtype` parameter accept?

- [ ] A) A list of data types
- [ ] B) A single data type for all columns
- [ ] C) A dictionary of column names and data types
- [ ] D) A tuple of (column, type) pairs

> [!success]- Answer **Correct Answer: C) A dictionary of column names and data types**
> 
> The chapter specifies: "dtype takes a dictionary of column names and data types"
> 
> ```python
> dtype={"zipcode": str}
> ```

**Q27.** What parameter is used to set custom missing values?

- [ ] A) missing_values
- [ ] B) na_values
- [ ] C) null_values
- [ ] D) nan_values

> [!success]- Answer **Correct Answer: B) na_values**
> 
> The `na_values` parameter allows you to specify which values should be treated as missing/NA.

**Q28.** What types of values can `na_values` accept?

- [ ] A) Only a single value
- [ ] B) Only a list
- [ ] C) Only a dictionary
- [ ] D) A single value, list, or dictionary of columns and values

> [!success]- Answer **Correct Answer: D) A single value, list, or dictionary of columns and values**
> 
> The chapter states `na_values` "can pass a single value, list, or dictionary of columns and values".

**Q29.** What parameter do you set to `False` to skip unparseable records?

- [ ] A) skip_bad_lines
- [ ] B) ignore_errors
- [ ] C) error_bad_lines
- [ ] D) skip_errors

> [!success]- Answer **Correct Answer: C) error_bad_lines**
> 
> Set `error_bad_lines=False` to skip records that cannot be parsed correctly.

**Q30.** What parameter shows messages when records are skipped due to errors?

- [ ] A) show_warnings
- [ ] B) warn_bad_lines
- [ ] C) display_errors
- [ ] D) verbose

> [!success]- Answer **Correct Answer: B) warn_bad_lines**
> 
> Set `warn_bad_lines=True` to see messages when records are skipped.
> 
> ```python
> pd.read_csv("file.csv", 
>             error_bad_lines=False,
>             warn_bad_lines=True)
> ```

---

## Section B: True/False Questions (15 questions × 1 point = 15 points)

**Q31.** Flat files can contain formatting and styling information. **T/F**

> [!success]- Answer **False** - Flat files store data as plain text without any formatting.

**Q32.** The `read_csv()` function can only read comma-separated files. **T/F**

> [!success]- Answer **False** - `read_csv()` can read any flat file by specifying different delimiters with the `sep` parameter.

**Q33.** You can use either column names or column numbers with the `usecols` parameter. **T/F**

> [!success]- Answer **True** - Both methods are demonstrated in the chapter and produce identical results.

**Q34.** The `nrows` parameter specifies which specific rows to load by row number. **T/F**

> [!success]- Answer **False** - `nrows` specifies how many rows to load from the beginning (or after skiprows), not which specific rows.

**Q35.** When using `skiprows`, you must always set `header=None`. **T/F**

> [!success]- Answer **False** - You should set `header=None` when the skipped rows include the header, but it's not always required in every skiprows scenario.

**Q36.** The list passed to the `names` parameter must have a name for every column. **T/F**

> [!success]- Answer **True** - This is explicitly stated as a requirement in the chapter.

**Q37.** pandas never automatically infers data types for columns. **T/F**

> [!success]- Answer **False** - pandas automatically infers column data types, though you can override this with `dtype`.

**Q38.** The `dtype` parameter accepts a dictionary mapping column names to data types. **T/F**

> [!success]- Answer **True** - This is the correct format for specifying data types with `dtype`.

**Q39.** pandas automatically interprets some values as missing or NA. **T/F**

> [!success]- Answer **True** - The chapter states this explicitly before showing how to customize with `na_values`.

**Q40.** You can pass a dictionary to `na_values` to specify different missing values for different columns. **T/F**

> [!success]- Answer **True** - The chapter demonstrates using a dictionary: `na_values={"zipcode": 0}`.

**Q41.** If a file has unparseable records, pandas will automatically skip them without any parameters set. **T/F**

> [!success]- Answer **False** - By default, pandas raises a ParserError. You must set `error_bad_lines=False` to skip them.

**Q42.** The `warn_bad_lines` parameter must be set to `True` to see messages about skipped records. **T/F**

> [!success]- Answer **True** - This parameter controls whether warning messages are displayed.

**Q43.** CSV is the most common type of flat file. **T/F**

> [!success]- Answer **True** - The chapter explicitly states this.

**Q44.** The `.head()` method displays the last few rows of a DataFrame. **T/F**

> [!success]- Answer **False** - `.head()` displays the first few rows. `.tail()` would show the last rows.

**Q45.** When loading columns by name and by number using the same columns, the resulting DataFrames will be different. **T/F**

> [!success]- Answer **False** - The chapter demonstrates with `.equals()` that they produce identical DataFrames.

---

## Section C: Short Answer Questions (8 questions × 3 points = 24 points)

**Q46.** What are the three common flat file import issues mentioned in the chapter?

> [!success]- Answer The three common flat file import issues are:
> 
> 1. **Column data types are wrong** - pandas may infer incorrect types
> 2. **Values are missing** - data may have missing or NA values
> 3. **Records that cannot be read by pandas** - unparseable or corrupt lines
> 
> These issues can be addressed using parameters like `dtype`, `na_values`, and `error_bad_lines`.

**Q47.** Explain the purpose of combining `nrows` and `skiprows` parameters together.

> [!success]- Answer Combining `nrows` and `skiprows` allows you to **process a file in chunks**, which is useful for large files that don't fit into memory.
> 
> For example, you could:
> 
> - First load rows 0-999 with `nrows=1000`
> - Then load rows 1000-1499 with `nrows=500, skiprows=1000`
> - Continue processing the file in manageable chunks
> 
> When doing this, you should set `header=None` so pandas knows there are no column names in the chunk.

**Q48.** List three types of input that the `usecols` parameter can accept.

> [!success]- Answer The `usecols` parameter can accept:
> 
> 1. **A list of column names** - e.g., `['STATEFIPS', 'STATE', 'zipcode']`
> 2. **A list of column numbers** - e.g., `[0, 1, 2, 3, 4]`
> 3. **A function to filter column names** - allows programmatic column selection
> 
> All three methods can be used to select which columns to load from a file.

**Q49.** When should you rename columns after import instead of using the `names` parameter?

> [!success]- Answer You should rename columns after import when **you only need to rename a few columns** rather than all of them.
> 
> The `names` parameter requires a name for every column in the dataset, making it cumbersome if you only want to change one or two column names. In that case, it's easier to load the file normally and then use DataFrame methods like `.rename()` to change specific columns.

**Q50.** Describe what happens when you try to read a corrupt file without using error handling parameters.

> [!success]- Answer When you try to read a corrupt file (one with unparseable records or wrong number of fields), pandas will **raise a `ParserError`** and stop execution.
> 
> The error message will indicate the problem, such as:
> 
> ```
> pandas.errors.ParserError: Error tokenizing data. 
> C error: Expected 147 fields in line 3, saw 148
> ```
> 
> To handle this, you need to set `error_bad_lines=False` to skip problematic lines and optionally `warn_bad_lines=True` to see which lines were skipped.

**Q51.** What is the difference between how `na_values` can be specified as a single value versus as a dictionary?

> [!success]- Answer
> 
> - **Single value**: Treats that value as missing across ALL columns
>     - Example: `na_values="N/A"` would treat all "N/A" entries as missing
> - **Dictionary**: Allows you to specify different missing values for specific columns
>     - Example: `na_values={"zipcode": 0}` treats 0 as missing only in the zipcode column
>     - Different columns can have different missing value indicators
> 
> The dictionary approach provides more granular control when different columns use different conventions for missing data.

**Q52.** Explain what the `header` parameter does and when you should set it to `None`.

> [!success]- Answer The `header` parameter specifies which row number(s) to use as column names.
> 
> You should set `header=None` when:
> 
> - There are no column names in the data you're reading
> - You've used `skiprows` and have skipped past the header row
> - The data chunk you're reading doesn't contain a header
> 
> When `header=None`, pandas assigns default numeric column names (0, 1, 2, ...) instead of trying to use the first row as column names. You can then assign proper names using the `names` parameter.

**Q53.** What is a DataFrame and why is it important in pandas?

> [!success]- Answer A **DataFrame** is a pandas-specific structure for two-dimensional data. It's the fundamental data structure in pandas for working with tabular data.
> 
> DataFrames are important because:
> 
> - They organize data in rows and columns (like a spreadsheet or SQL table)
> - They're the result of reading flat files with `read_csv()`
> - They provide methods for data manipulation, analysis, and transformation
> - Most pandas operations work with DataFrames as input and output

---

## Section D: Code Analysis & Scenarios (5 questions × 2 points = 10 points)

**Q54.** What will the shape of the resulting DataFrame be?

```python
tax_data = pd.read_csv('file.csv')  # Original file has 179796 rows and 147 columns
tax_data_subset = pd.read_csv('file.csv', nrows=1000)
print(tax_data_subset.shape)
```

> [!success]- Answer **Output: (1000, 147)**
> 
> The `nrows=1000` parameter limits the number of rows loaded to 1000, but all 147 columns are still loaded since no column filtering was applied. The shape shows (rows, columns).

**Q55.** Identify the issue in this code and explain how to fix it:

```python
tax_data = pd.read_csv('file.csv',
                       nrows=500,
                       skiprows=1000)
# Results in columns: 0, 1, 2, 3, ... (numeric names)
```

> [!success]- Answer **Issue**: The code is missing `header=None`, so pandas tries to use the first row of the chunk as column names, but since we skipped past the actual header, it uses data values as column names.
> 
> **Fix**: Add `header=None` to tell pandas there's no header row:
> 
> ```python
> tax_data = pd.read_csv('file.csv',
>                        nrows=500,
>                        skiprows=1000,
>                        header=None)
> ```
> 
> This will properly assign numeric column names (0, 1, 2, ...) that you can then rename using the `names` parameter.

**Q56.** What does this code accomplish?

```python
col_names = ['STATEFIPS', 'STATE', 'zipcode']
col_nums = [0, 1, 2]

df1 = pd.read_csv('file.csv', usecols=col_names)
df2 = pd.read_csv('file.csv', usecols=col_nums)

print(df1.equals(df2))
```

> [!success]- Answer **Output: True**
> 
> This code demonstrates that selecting columns by name and by number produces identical DataFrames.
> 
> - `df1` loads columns by their names
> - `df2` loads columns by their position numbers (0, 1, 2 correspond to the first three columns)
> - Both DataFrames contain the same data, so `.equals()` returns `True`
> 
> This shows that `usecols` can accept either format and produce equivalent results.

**Q57.** Complete this code to treat the value 0 as missing ONLY in the zipcode column:

```python
tax_data = pd.read_csv("us_tax_data_2016.csv",
                       na_values=_______)
```

> [!success]- Answer
> 
> ```python
> tax_data = pd.read_csv("us_tax_data_2016.csv",
>                        na_values={"zipcode": 0})
> ```
> 
> Use a dictionary with the column name as the key and the missing value indicator as the value. This tells pandas to treat 0 as NA/missing only in the zipcode column, while leaving 0 as a valid value in other columns.

**Q58.** What will happen when this code runs?

```python
import pandas as pd
data = pd.read_csv("corrupt_file.csv")
# corrupt_file.csv has a line with 148 fields when 147 are expected
```

> [!success]- Answer **Result**: The code will raise a `ParserError` and stop execution.
> 
> **Error message**:
> 
> ```
> pandas.errors.ParserError: Error tokenizing data. 
> C error: Expected 147 fields in line X, saw 148
> ```
> 
> **Solution**: To handle this gracefully, add error handling parameters:
> 
> ```python
> data = pd.read_csv("corrupt_file.csv",
>                    error_bad_lines=False,
>                    warn_bad_lines=True)
> ```
> 
> This will skip the problematic line and display a warning message.

---

## Answer Key & Scoring

### Section A: Multiple Choice (60 points)

1. B | 2. C | 3. D | 4. B | 5. B
2. C | 7. B | 8. D | 9. C | 10. B
3. C | 12. C | 13. C | 14. B | 15. C
4. C | 17. C | 18. D | 19. B | 20. B
5. B | 22. B | 23. B | 24. B | 25. C
6. C | 27. B | 28. D | 29. C | 30. B

### Section B: True/False (15 points)

31. F | 32. F | 33. T | 34. F | 35. F
32. T | 37. F | 38. T | 39. T | 40. T
33. F | 42. T | 43. T | 44. F | 45. F

### Section C: Short Answer (24 points)

- Q46-Q53: See detailed answers in collapsible sections (3 points each)

### Section D: Code Analysis (10 points)

- Q54-Q58: See detailed answers in collapsible sections (2 points each)

---

## Quick Reference Guide

### Essential Parameters

```python
# Basic import
pd.read_csv("file.csv")

# Different delimiter
pd.read_csv("file.tsv", sep="\t")

# Limit columns
pd.read_csv("file.csv", usecols=['col1', 'col2'])
pd.read_csv("file.csv", usecols=[0, 1, 2])

# Limit rows
pd.read_csv("file.csv", nrows=1000)
pd.read_csv("file.csv", nrows=500, skiprows=1000, header=None)

# Assign column names
pd.read_csv("file.csv", names=['col1', 'col2', 'col3'])

# Specify data types
pd.read_csv("file.csv", dtype={"zipcode": str})

# Custom missing values
pd.read_csv("file.csv", na_values={"col1": 0})

# Handle errors
pd.read_csv("file.csv", error_bad_lines=False, warn_bad_lines=True)
```

---

## Key Concepts Summary

|Concept|Description|
|---|---|
|**Flat File**|Plain text file with one row per line, values separated by delimiter|
|**CSV**|Comma-separated values - most common flat file type|
|**DataFrame**|pandas structure for two-dimensional data|
|**Delimiter**|Character that separates values (comma, tab, pipe, etc.)|
|**Chunk Processing**|Loading large files in pieces using nrows + skiprows|

---

## Common Mistakes to Avoid

❌ Forgetting to import pandas before using `pd.read_csv()`  
❌ Using `read_csv()` without the `sep` parameter for non-CSV files  
❌ Not setting `header=None` when using `skiprows` with chunks  
❌ Providing incomplete column lists to the `names` parameter  
❌ Expecting pandas to skip bad lines automatically without `error_bad_lines=False`  
❌ Confusing `nrows` (how many to load) with `skiprows` (which ones to skip)  
❌ Using `.tail()` when you meant `.head()` or vice versa  
❌ Forgetting that `usecols` can accept both names and numbers

---

## Study Strategy (3-Day Plan)

### Day 1: Basics

- Review flat file concepts and DataFrame basics
- Practice basic CSV imports with `read_csv()`
- Experiment with different delimiters using `sep`
- Use `.head()` and `.shape` to examine DataFrames

### Day 2: Limiting and Selecting Data

- Practice using `usecols` with both names and numbers
- Work with `nrows` to limit rows
- Combine `nrows` and `skiprows` for chunk processing
- Practice assigning column names with `names` parameter

### Day 3: Data Types and Error Handling

- Review how pandas infers data types
- Practice using `dtype` to specify types
- Work with `na_values` to customize missing data
- Handle corrupt files with `error_bad_lines` and `warn_bad_lines`
- Complete practice quiz and review wrong answers

---

## Pre-Exam Checklist

✅ Know the basic syntax: `pd.read_csv("filename")`  
✅ Remember `sep` parameter for non-CSV delimiters  
✅ Understand difference between `nrows` and `skiprows`  
✅ Know when to set `header=None`  
✅ Remember that `names` requires a name for EVERY column  
✅ Know the three input types for `usecols`, `skiprows`, and `na_values`  
✅ Understand `dtype` uses a dictionary format  
✅ Know both error handling parameters: `error_bad_lines` and `warn_bad_lines`  
✅ Review all code examples from chapter

---

## During the Exam

💡 **Read questions carefully** - distinguish between nrows and skiprows  
💡 **Check parameter names** - it's `usecols`, not `select_cols`  
💡 **Remember dictionary formats** - dtype and na_values use `{column: value}`  
💡 **For True/False** - look for absolute words like "always", "never", "only"  
💡 **Code analysis** - trace through line by line  
💡 **Short answer** - be specific and use terminology from the chapter

---

#datacamp #pandas #data-ingestion #flat-files #csv #certification #exam-prep