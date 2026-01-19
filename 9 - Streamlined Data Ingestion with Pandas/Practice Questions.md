# Streamlined Data Ingestion with pandas - Comprehensive Final Exam

**Course:** Streamlined Data Ingestion with pandas  
**Chapters:** 1-4 (All Chapters)  
**Total Points:** 150  
**Passing Score:** 105/150 (70%)  
**Time Limit:** 90 minutes

---

## Section A: Multiple Choice Questions (50 questions × 2 points = 100 points)

### Part 1: Flat Files (Chapters 1 & 2)

**Q1.** Which function is used to load CSV files in pandas?
- [ ] A) pd.load_csv()
- [ ] B) pd.read_csv()
- [ ] C) pd.import_csv()
- [ ] D) pd.csv_read()

> [!success]- Answer
> **Correct Answer: B) pd.read_csv()**
> 
> `pd.read_csv()` is the pandas function for loading flat files, including CSV and TSV files.

**Q2.** What parameter specifies a different delimiter in `read_csv()`?
- [ ] A) delimiter
- [ ] B) sep
- [ ] C) separator
- [ ] D) delim

> [!success]- Answer
> **Correct Answer: B) sep**
> 
> Use `sep="\t"` for tab-separated files, or any other delimiter character.

**Q3.** Which parameter limits the number of rows loaded?
- [ ] A) limit
- [ ] B) max_rows
- [ ] C) nrows
- [ ] D) row_limit

> [!success]- Answer
> **Correct Answer: C) nrows**
> 
> `nrows` specifies the maximum number of rows to load.

**Q4.** What must you set when using `skiprows` to skip past the header row?
- [ ] A) header=0
- [ ] B) header=None
- [ ] C) header=False
- [ ] D) no_header=True

> [!success]- Answer
> **Correct Answer: B) header=None**
> 
> Set `header=None` to tell pandas there are no column names in the data being read.

**Q5.** Which function is used to load Excel files?
- [ ] A) pd.read_csv()
- [ ] B) pd.load_excel()
- [ ] C) pd.read_excel()
- [ ] D) pd.excel_read()

> [!success]- Answer
> **Correct Answer: C) pd.read_excel()**
> 
> `pd.read_excel()` is specifically for spreadsheet/Excel files.

**Q6.** What does `sheet_name=None` do in `read_excel()`?
- [ ] A) Loads the first sheet
- [ ] B) Loads no sheets
- [ ] C) Loads all sheets in the workbook
- [ ] D) Raises an error

> [!success]- Answer
> **Correct Answer: C) Loads all sheets in the workbook**
> 
> Returns an OrderedDict with sheet names as keys and DataFrames as values.

**Q7.** How does pandas load Boolean columns by default?
- [ ] A) As bool type
- [ ] B) As float data
- [ ] C) As int data
- [ ] D) As string/object

> [!success]- Answer
> **Correct Answer: B) As float data**
> 
> pandas loads True/False columns as float by default; use `dtype` to specify bool.

**Q8.** What happens to NA values in Boolean columns?
- [ ] A) They remain as NA
- [ ] B) They are changed to True
- [ ] C) They are changed to False
- [ ] D) They cause an error

> [!success]- Answer
> **Correct Answer: B) They are changed to True**
> 
> This is a critical issue when working with Boolean data in pandas.

**Q9.** What parameter is used to parse datetime columns during import?
- [ ] A) dtype
- [ ] B) datetime_cols
- [ ] C) parse_dates
- [ ] D) date_format

> [!success]- Answer
> **Correct Answer: C) parse_dates**
> 
> Use `parse_dates`, NOT `dtype`, for datetime columns.

**Q10.** Which function is used for non-standard datetime formats after loading?
- [ ] A) pd.parse_datetime()
- [ ] B) pd.to_datetime()
- [ ] C) pd.datetime_parse()
- [ ] D) pd.convert_datetime()

> [!success]- Answer
> **Correct Answer: B) pd.to_datetime()**
> 
> Use `pd.to_datetime()` with a `format` parameter for non-standard dates.

### Part 2: Databases (Chapter 3)

**Q11.** What library provides the `create_engine()` function?
- [ ] A) pandas
- [ ] B) sqlalchemy
- [ ] C) sqlite3
- [ ] D) sql

> [!success]- Answer
> **Correct Answer: B) sqlalchemy**
> 
> `sqlalchemy`'s `create_engine()` creates database connection engines.

**Q12.** What is the correct SQLite URL format?
- [ ] A) sqlite://filename.db
- [ ] B) sqlite:///filename.db
- [ ] C) sqlite:/filename.db
- [ ] D) db://sqlite/filename

> [!success]- Answer
> **Correct Answer: B) sqlite:///filename.db**
> 
> Note the three forward slashes for SQLite file databases.

**Q13.** What are the two arguments for `pd.read_sql()`?
- [ ] A) filename and engine
- [ ] B) query and engine
- [ ] C) database and table
- [ ] D) sql and connection

> [!success]- Answer
> **Correct Answer: B) query and engine**
> 
> `pd.read_sql(query, engine)` where query can be a SQL string or table name.

**Q14.** What does `SELECT * FROM table` do?
- [ ] A) Selects the first row
- [ ] B) Gets all data in the table
- [ ] C) Counts all rows
- [ ] D) Deletes the table

> [!success]- Answer
> **Correct Answer: B) Gets all data in the table**
> 
> The `*` wildcard selects all columns.

**Q15.** Is string matching in SQL WHERE clauses case-sensitive?
- [ ] A) No, always case-insensitive
- [ ] B) Yes, it's case-sensitive
- [ ] C) Only for numbers
- [ ] D) Depends on the database

> [!success]- Answer
> **Correct Answer: B) Yes, it's case-sensitive**
> 
> String matching with `=` is case-sensitive in SQL.

**Q16.** Which SQL operator means "not equal to"?
- [ ] A) !=
- [ ] B) <>
- [ ] C) NOT
- [ ] D) ~=

> [!success]- Answer
> **Correct Answer: B) <>**
> 
> SQL uses `<>` for "not equal to".

**Q17.** What does AND do in WHERE clauses?
- [ ] A) Returns records meeting at least one condition
- [ ] B) Returns records meeting all conditions
- [ ] C) Returns no records
- [ ] D) Orders results

> [!success]- Answer
> **Correct Answer: B) Returns records meeting all conditions**
> 
> AND is more restrictive - all conditions must be true.

**Q18.** What does `SELECT COUNT(*) FROM table` return?
- [ ] A) Number of columns
- [ ] B) Number of unique values
- [ ] C) Number of rows in the table
- [ ] D) NULL values count

> [!success]- Answer
> **Correct Answer: C) Number of rows in the table**
> 
> `COUNT(*)` counts all rows.

**Q19.** When using GROUP BY, what must you remember?
- [ ] A) Use only aggregate functions
- [ ] B) Also select the column you're grouping by
- [ ] C) Include a WHERE clause
- [ ] D) Use DISTINCT

> [!success]- Answer
> **Correct Answer: B) Also select the column you're grouping by**
> 
> Always include the grouped column in your SELECT statement.

**Q20.** What notation is used with multiple tables in SQL?
- [ ] A) Bracket notation
- [ ] B) Dot notation table.column
- [ ] C) Underscore notation
- [ ] D) Arrow notation

> [!success]- Answer
> **Correct Answer: B) Dot notation table.column**
> 
> Use `table.column` to specify which table a column belongs to.

### Part 3: JSON & APIs (Chapter 4)

**Q21.** What does JSON stand for?
- [ ] A) Java Standard Object Notation
- [ ] B) JavaScript Object Notation
- [ ] C) Just Simple Object Notation
- [ ] D) Java Serialized Object Network

> [!success]- Answer
> **Correct Answer: B) JavaScript Object Notation**
> 
> JSON is JavaScript Object Notation.

**Q22.** Is JSON a tabular data format?
- [ ] A) Yes, like CSV
- [ ] B) No, it's not tabular
- [ ] C) Only if formatted correctly
- [ ] D) Only with record orientation

> [!success]- Answer
> **Correct Answer: B) No, it's not tabular**
> 
> JSON is explicitly not tabular, unlike CSV or Excel.

**Q23.** Which JSON orientation is most common?
- [ ] A) Column
- [ ] B) Split
- [ ] C) Record
- [ ] D) Index

> [!success]- Answer
> **Correct Answer: C) Record**
> 
> Record orientation (list of objects) is most common.

**Q24.** What does an API provide?
- [ ] A) Data compression
- [ ] B) Way to get data without knowing database details
- [ ] C) Data encryption
- [ ] D) Data storage

> [!success]- Answer
> **Correct Answer: B) Way to get data without knowing database details**
> 
> APIs define how applications communicate and provide data access.

**Q25.** Which function gets data from a URL?
- [ ] A) requests.fetch()
- [ ] B) requests.download()
- [ ] C) requests.get()
- [ ] D) requests.retrieve()

> [!success]- Answer
> **Correct Answer: C) requests.get()**
> 
> `requests.get()` retrieves data from URLs/APIs.

**Q26.** What does `response.json()` return?
- [ ] A) A string
- [ ] B) A DataFrame
- [ ] C) A dictionary
- [ ] D) A list

> [!success]- Answer
> **Correct Answer: C) A dictionary**
> 
> `response.json()` returns a dictionary, not a string.

**Q27.** Can you use `pd.read_json()` with `response.json()` output?
- [ ] A) Yes, always
- [ ] B) No, it expects strings not dictionaries
- [ ] C) Yes, if you specify orient
- [ ] D) Only for record orientation

> [!success]- Answer
> **Correct Answer: B) No, it expects strings not dictionaries**
> 
> Use `pd.DataFrame()` for API response dictionaries, not `read_json()`.

**Q28.** Where is `json_normalize()` located?
- [ ] A) Main pandas module
- [ ] B) pandas.io.json submodule
- [ ] C) requests library
- [ ] D) json module

> [!success]- Answer
> **Correct Answer: B) pandas.io.json submodule**
> 
> Must import: `from pandas.io.json import json_normalize`

**Q29.** What is the default column name separator in `json_normalize()`?
- [ ] A) Underscore (_)
- [ ] B) Hyphen (-)
- [ ] C) Period (.)
- [ ] D) Colon (:)

> [!success]- Answer
> **Correct Answer: C) Period (.)**
> 
> Default pattern is `attribute.nestedattribute`; change with `sep`.

**Q30.** What is the use case for `pd.concat()`?
- [ ] A) Adding columns
- [ ] B) Adding rows
- [ ] C) Joining tables
- [ ] D) Sorting data

> [!success]- Answer
> **Correct Answer: B) Adding rows**
> 
> `concat()` stacks DataFrames vertically (adds rows).

### Part 4: Cross-Chapter Concepts

**Q31.** Which parameter is common to both `read_csv()` and `read_excel()`?
- [ ] A) sheet_name
- [ ] B) orient
- [ ] C) nrows
- [ ] D) All of the above

> [!success]- Answer
> **Correct Answer: C) nrows**
> 
> `nrows`, `skiprows`, and `usecols` are shared; `sheet_name` is Excel-only, `orient` is JSON-only.

**Q32.** Which uses the `dtype` parameter to specify data types?
- [ ] A) Only read_csv()
- [ ] B) Only read_excel()
- [ ] C) Only read_sql()
- [ ] D) Both read_csv() and read_excel()

> [!success]- Answer
> **Correct Answer: D) Both read_csv() and read_excel()**
> 
> `dtype` works with both CSV and Excel imports.

**Q33.** Which function can accept either a string or a dictionary as input?
- [ ] A) pd.read_csv()
- [ ] B) pd.read_json()
- [ ] C) pd.DataFrame()
- [ ] D) Both B and C

> [!success]- Answer
> **Correct Answer: D) Both B and C**
> 
> `read_json()` accepts strings; `DataFrame()` accepts dictionaries; use appropriately for APIs.

**Q34.** What do SQL JOINs and pandas merge() have in common?
- [ ] A) Nothing, completely different
- [ ] B) Both combine tables/DataFrames by matching key values
- [ ] C) Both only work with numeric keys
- [ ] D) Both require sorted data

> [!success]- Answer
> **Correct Answer: B) Both combine tables/DataFrames by matching key values**
> 
> `merge()` is the pandas version of SQL joins.

**Q35.** Which format requires keys to be the same data type for joining?
- [ ] A) Only SQL databases
- [ ] B) Only pandas DataFrames
- [ ] C) Both SQL and pandas
- [ ] D) Neither

> [!success]- Answer
> **Correct Answer: C) Both SQL and pandas**
> 
> Both SQL JOINs and pandas merge() require matching key data types.

**Q36.** What happens with missing values when using `na_values` parameter?
- [ ] A) Only works in read_csv()
- [ ] B) Works in both read_csv() and read_excel()
- [ ] C) Only works with numeric columns
- [ ] D) Deletes the rows

> [!success]- Answer
> **Correct Answer: B) Works in both read_csv() and read_excel()**
> 
> `na_values` is available for both flat file and Excel imports.

**Q37.** Which data source is not tabular?
- [ ] A) CSV files
- [ ] B) Excel spreadsheets
- [ ] C) SQL databases
- [ ] D) JSON data

> [!success]- Answer
> **Correct Answer: D) JSON data**
> 
> JSON is not tabular; all others are structured as tables.

**Q38.** Which function would you use to combine results from multiple API calls?
- [ ] A) pd.merge()
- [ ] B) pd.concat()
- [ ] C) pd.join()
- [ ] D) pd.combine()

> [!success]- Answer
> **Correct Answer: B) pd.concat()**
> 
> Use `concat()` to stack multiple API result pages vertically.

**Q39.** What is required when loading the second chunk of a CSV file with `skiprows` and `nrows`?
- [ ] A) sheet_name=1
- [ ] B) header=None
- [ ] C) orient="split"
- [ ] D) sep="\t"

> [!success]- Answer
> **Correct Answer: B) header=None**
> 
> Set `header=None` when skipping past the header row in chunk processing.

**Q40.** Which function creates database connection engines?
- [ ] A) pd.connect()
- [ ] B) create_engine()
- [ ] C) db.connect()
- [ ] D) sql.engine()

> [!success]- Answer
> **Correct Answer: B) create_engine()**
> 
> From sqlalchemy: `create_engine("sqlite:///file.db")`

**Q41.** What's the difference between `usecols` in `read_csv()` and `read_excel()`?
- [ ] A) No difference
- [ ] B) Excel can use column letters like "A:F"
- [ ] C) CSV can use column letters
- [ ] D) Excel doesn't have usecols

> [!success]- Answer
> **Correct Answer: B) Excel can use column letters like "A:F"**
> 
> `read_excel()` can use Excel-style column letters; `read_csv()` cannot.

**Q42.** Which correctly describes nested data structures?
- [ ] A) Only found in JSON
- [ ] B) Found in JSON; Excel also has nested structures in cells
- [ ] C) Not supported by pandas
- [ ] D) Found in all data formats

> [!success]- Answer
> **Correct Answer: B) Found in JSON; Excel also has nested structures in cells**
> 
> Nested data is primarily a JSON feature, but can appear in Excel cells.

**Q43.** What must match for a merge/join to work correctly?
- [ ] A) Number of rows
- [ ] B) Column names
- [ ] C) Key column data types
- [ ] D) Number of columns

> [!success]- Answer
> **Correct Answer: C) Key column data types**
> 
> Keys must be the same data type in both SQL and pandas.

**Q44.** Which parameter adds a prefix to column names?
- [ ] A) prefix in read_csv()
- [ ] B) meta_prefix in json_normalize()
- [ ] C) col_prefix in read_excel()
- [ ] D) name_prefix in read_sql()

> [!success]- Answer
> **Correct Answer: B) meta_prefix in json_normalize()**
> 
> `meta_prefix` adds prefixes to meta columns from parent-level data.

**Q45.** What do `true_values` and `false_values` parameters require to work?
- [ ] A) parse_dates to be set
- [ ] B) Columns set as bool in dtype
- [ ] C) sheet_name to be specified
- [ ] D) orient to be "record"

> [!success]- Answer
> **Correct Answer: B) Columns set as bool in dtype**
> 
> Custom True/False values only apply to Boolean columns.

**Q46.** Which SQL keyword must come first in a query?
- [ ] A) FROM
- [ ] B) WHERE
- [ ] C) SELECT
- [ ] D) GROUP BY

> [!success]- Answer
> **Correct Answer: C) SELECT**
> 
> SQL order: SELECT, FROM, JOIN, WHERE, GROUP BY.

**Q47.** What does `ignore_index=True` do in `pd.concat()`?
- [ ] A) Ignores the index column
- [ ] B) Renumbers rows starting from 0
- [ ] C) Removes duplicate indices
- [ ] D) Sorts by index

> [!success]- Answer
> **Correct Answer: B) Renumbers rows starting from 0**
> 
> Creates a new sequential index for the combined DataFrame.

**Q48.** Which datetime code represents a 4-digit year?
- [ ] A) %y
- [ ] B) %Y
- [ ] C) %YYYY
- [ ] D) %year

> [!success]- Answer
> **Correct Answer: B) %Y**
> 
> `%Y` is 4-digit year (1999), `%y` is 2-digit year (99).

**Q49.** What type of object does `sheet_name=None` return in `read_excel()`?
- [ ] A) DataFrame
- [ ] B) List
- [ ] C) OrderedDict
- [ ] D) Dictionary

> [!success]- Answer
> **Correct Answer: C) OrderedDict**
> 
> Returns OrderedDict with sheet names as keys and DataFrames as values.

**Q50.** Which function flattens nested JSON structures?
- [ ] A) pd.flatten()
- [ ] B) json_normalize()
- [ ] C) pd.unnest()
- [ ] D) flatten_json()

> [!success]- Answer
> **Correct Answer: B) json_normalize()**
> 
> `json_normalize()` from `pandas.io.json` flattens nested JSON.

---

## Section B: True/False Questions (20 questions × 1 point = 20 points)

**Q51.** The `read_csv()` function can only read comma-separated files. **T/F**

> [!success]- Answer
> **False** - Can read any delimiter with `sep` parameter.

**Q52.** You must always provide column names when using the `names` parameter. **T/F**

> [!success]- Answer
> **True** - The list must have a name for every column.

**Q53.** pandas automatically infers column data types when loading data. **T/F**

> [!success]- Answer
> **True** - But you can override with `dtype` parameter.

**Q54.** By default, `read_excel()` loads all sheets in a workbook. **T/F**

> [!success]- Answer
> **False** - It loads only the first sheet by default.

**Q55.** Boolean columns can contain True, False, and NA values without conversion. **T/F**

> [!success]- Answer
> **False** - NA values are converted to True in Boolean columns.

**Q56.** Use `dtype` to specify datetime columns during import. **T/F**

> [!success]- Answer
> **False** - Use `parse_dates`, not `dtype`, for datetimes.

**Q57.** SQLite databases are computer files. **T/F**

> [!success]- Answer
> **True** - SQLite uses file-based storage.

**Q58.** String matching in SQL WHERE clauses is case-insensitive. **T/F**

> [!success]- Answer
> **False** - String matching is case-sensitive in SQL.

**Q59.** Aggregate functions calculate a single summary statistic by default without GROUP BY. **T/F**

> [!success]- Answer
> **True** - GROUP BY is needed for category-level summaries.

**Q60.** Default JOIN behavior returns all records from both tables. **T/F**

> [!success]- Answer
> **False** - Default returns only matching records (inner join).

**Q61.** JSON records must all have the same set of attributes. **T/F**

> [!success]- Answer
> **False** - JSON records can have different attributes (flexibility).

**Q62.** Record orientation is the most common JSON arrangement. **T/F**

> [!success]- Answer
> **True** - Most common format for JSON data.

**Q63.** The `requests` library is tied to specific APIs. **T/F**

> [!success]- Answer
> **False** - Not tied to any particular API.

**Q64.** You can use `pd.read_json()` directly with `response.json()` output. **T/F**

> [!success]- Answer
> **False** - `read_json()` expects strings; `response.json()` returns dicts.

**Q65.** `json_normalize()` needs its own import statement. **T/F**

> [!success]- Answer
> **True** - Must import from `pandas.io.json`.

**Q66.** The default separator in `json_normalize()` is an underscore. **T/F**

> [!success]- Answer
> **False** - Default is a period (`.`), not underscore.

**Q67.** `pd.concat()` combines DataFrames horizontally by adding columns. **T/F**

> [!success]- Answer
> **False** - `concat()` stacks vertically (adds rows); `merge()` adds columns.

**Q68.** Merge keys must be the same data type to work properly. **T/F**

> [!success]- Answer
> **True** - Same requirement as SQL joins.

**Q69.** The `sep` parameter works identically in `read_csv()` and `json_normalize()`. **T/F**

> [!success]- Answer
> **False** - In `read_csv()` it specifies delimiter; in `json_normalize()` it's for flattened column name separator.

**Q70.** Arguments passed to `read_excel()` apply to all sheets when loading multiple sheets. **T/F**

> [!success]- Answer
> **True** - Parameters like `nrows` apply to all sheets read.

---

## Section C: Short Answer Questions (10 questions × 3 points = 30 points)

**Q71.** Explain the difference between `nrows` and `skiprows` parameters, and describe when you would use them together.

> [!success]- Answer
> **`nrows`:**
> - Limits the NUMBER of rows to load
> - Loads from the current position (beginning or after skiprows)
> - Example: `nrows=1000` loads 1000 rows
> 
> **`skiprows`:**
> - Skips specified rows before loading
> - Can accept a number, list of row numbers, or function
> - Example: `skiprows=500` skips first 500 rows
> 
> **Using together:**
> - Enables **chunk processing** of large files
> - Load file in manageable pieces
> - Example: `skiprows=1000, nrows=500` skips first 1000 rows, then loads next 500
> - Important: Set `header=None` when skipping past the header row
> - Useful when files are too large to fit in memory

**Q72.** Compare and contrast flat files, spreadsheets, databases, and JSON in terms of structure and capabilities.

> [!success]- Answer
> **Flat Files (CSV/TSV):**
> - Tabular structure
> - Plain text, no formatting
> - One row per line
> - Simple, lightweight
> - No data type enforcement
> 
> **Spreadsheets (Excel):**
> - Tabular structure
> - CAN have formatting and formulas
> - Multiple sheets in one workbook
> - More complex than flat files
> 
> **Databases (SQL):**
> - Tabular structure (tables)
> - Data types specified for columns
> - Tables linked via keys
> - Supports multiple users, data quality controls
> - Requires SQL queries
> 
> **JSON:**
> - NOT tabular
> - Objects with attribute-value pairs
> - Records can have different attributes
> - Supports nested structures (objects within objects)
> - Common web data format

**Q73.** Describe the complete process of connecting to and querying a database using pandas, including all necessary imports and steps.

> [!success]- Answer
> **Complete database access process:**
> 
> **Step 1: Imports**
> ```python
> import pandas as pd
> from sqlalchemy import create_engine
> ```
> 
> **Step 2: Create database engine**
> ```python
> engine = create_engine("sqlite:///database.db")
> ```
> - Note: Three slashes for SQLite
> - URL format varies by database type
> 
> **Step 3: Write SQL query**
> ```python
> query = """SELECT column1, column2
> FROM table_name
> WHERE condition
> GROUP BY column1;"""
> ```
> 
> **Step 4: Query database**
> ```python
> df = pd.read_sql(query, engine)
> ```
> - Can also pass table name: `pd.read_sql("table_name", engine)`
> 
> **Result:** Returns a pandas DataFrame

**Q74.** Explain the issue with Boolean columns in pandas and how to handle them correctly, including the problems that arise and their solutions.

> [!success]- Answer
> **The Issue:**
> - pandas loads True/False columns as **float data by default**
> - When you set columns to Boolean type:
>   1. **NA/missing values are changed to True** (not preserved)
>   2. **Unrecognized values are also changed to True** (silent errors)
> 
> **How to Handle Correctly:**
> 
> 1. **Explicitly set dtype to bool:**
> ```python
> pd.read_excel("file.xlsx", dtype={"ColName": bool})
> ```
> 
> 2. **Use custom True/False values:**
> ```python
> pd.read_excel("file.xlsx",
>               dtype={"ColName": bool},
>               true_values=["Yes", "Y"],
>               false_values=["No", "N"])
> ```
> 
> 3. **Consider alternatives:**
> - If missing values are important, use float or int instead
> - If "unknown" is different from False, keep as object/string
> 
> **Boolean Considerations:**
> - Are there missing values?
> - What if a value is incorrectly coded as True?
> - Could data be modeled another way?

**Q75.** Describe three ways to specify datetime columns in pandas, including when each method should be used.

> [!success]- Answer
> **Method 1: Simple list with `parse_dates`**
> ```python
> pd.read_excel("file.xlsx", parse_dates=["DateCol1", "DateCol2"])
> ```
> - When: Columns already contain complete datetime values
> - Use: Standard date formats that pandas recognizes
> 
> **Method 2: Combining columns with nested list**
> ```python
> parse_dates=[["StartDate", "StartTime"]]
> ```
> - When: Date and time are in separate columns
> - Result: Creates combined column with underscore-joined name
> 
> **Method 3: Dictionary with custom names**
> ```python
> parse_dates={"Start": ["StartDate", "StartTime"],
>              "End": ["EndDate", "EndTime"]}
> ```
> - When: Combining columns AND want custom names
> - Result: Creates columns with specified names
> 
> **Method 4: pd.to_datetime() (after loading)**
> ```python
> df["DateCol"] = pd.to_datetime(df["DateCol"], format="%m%d%Y")
> ```
> - When: Non-standard formats that `parse_dates` can't handle
> - Requires specifying format string with codes (%Y, %m, %d, etc.)

**Q76.** Compare SQL aggregate functions with pandas operations, explaining how GROUP BY relates to pandas grouping.

> [!success]- Answer
> **SQL Aggregate Functions:**
> - SUM, AVG, MAX, MIN, COUNT
> - Calculate summary statistics
> - Default: One value for entire table
> - GROUP BY: Summary per category
> 
> **SQL Example:**
> ```sql
> SELECT borough, COUNT(*)
> FROM calls
> GROUP BY borough;
> ```
> Returns one count per borough
> 
> **pandas Equivalent:**
> - Can perform similar operations after loading data
> - `df.groupby('borough').size()` similar to SQL GROUP BY + COUNT
> - `df['column'].sum()` similar to SQL SUM
> 
> **Key Similarity:**
> - Both require specifying grouping column
> - SQL: Must SELECT the grouped column
> - pandas: Use `.groupby('column')`
> 
> **When to use SQL vs pandas:**
> - SQL aggregation: Better for large datasets (only returns summary)
> - pandas aggregation: Better when you need full data + summaries
> - Best practice: Aggregate in SQL when possible to reduce data transfer

**Q77.** Explain why you can't use `pd.read_json()` with API response data and describe the correct workflow for loading API data.

> [!success]- Answer
> **Why pd.read_json() Doesn't Work:**
> - `response.json()` returns a **dictionary**
> - `pd.read_json()` expects **strings** (file paths or JSON strings)
> - Type mismatch causes an error
> 
> **Correct API Workflow:**
> 
> **Step 1: Import libraries**
> ```python
> import requests
> import pandas as pd
> ```
> 
> **Step 2: Make API call**
> ```python
> response = requests.get(api_url,
>                        params={"key": "value"},
>                        headers={"Authorization": "Bearer TOKEN"})
> ```
> 
> **Step 3: Extract JSON data**
> ```python
> data = response.json()  # Returns dictionary
> ```
> 
> **Step 4: Create DataFrame**
> ```python
> df = pd.DataFrame(data["key"])  # Use DataFrame(), not read_json()
> ```
> 
> **For nested data:**
> ```python
> from pandas.io.json import json_normalize
> df = json_normalize(data["businesses"], sep="_")
> ```

**Q78.** Describe the three advanced parameters of `json_normalize()` and provide a scenario where each would be used.

> [!success]- Answer
> **1. `record_path`** (string or list)
> - Points to nested data to flatten
> - **Scenario:** API returns businesses with nested categories array
> ```python
> json_normalize(data["businesses"], record_path="categories")
> ```
> - Creates one row per category per business
> 
> **2. `meta`** (list of attributes)
> - Preserves parent-level attributes alongside nested data
> - Can use nested paths: `["name", ["coords", "lat"]]`
> - **Scenario:** Want category details WITH business info
> ```python
> json_normalize(data["businesses"],
>               record_path="categories",
>               meta=["name", "rating"])
> ```
> - Each category row includes business name and rating
> 
> **3. `meta_prefix`** (string)
> - Adds prefix to meta columns
> - Distinguishes parent vs nested data
> - **Scenario:** Avoid name conflicts, clarify data source
> ```python
> json_normalize(data["businesses"],
>               record_path="categories",
>               meta=["name", "rating"],
>               meta_prefix="biz_")
> ```
> - Creates: `biz_name`, `biz_rating` (from parent) + `alias`, `title` (from categories)

**Q79.** Explain when to use `pd.concat()` vs `df.merge()`, including the parameters each function uses and requirements for successful execution.

> [!success]- Answer
> **`pd.concat()` - Adding Rows (Vertical)**
> 
> **When to use:**
> - Stacking DataFrames with same columns
> - Combining multiple API result pages
> - Appending new records to existing data
> 
> **Syntax:**
> ```python
> pd.concat([df1, df2, df3], ignore_index=True)
> ```
> 
> **Key parameter:**
> - `ignore_index=True` - Renumbers rows from 0
> 
> **Requirements:**
> - DataFrames should have same/similar columns
> - No matching keys needed
> 
> **Result:** More rows, same columns
> 
> **`df.merge()` - Adding Columns (Horizontal)**
> 
> **When to use:**
> - Joining related data from different sources
> - Adding supplementary information
> - Similar to SQL JOIN
> 
> **Syntax:**
> ```python
> df1.merge(df2, on="key_column")
> # OR
> df1.merge(df2, left_on="key1", right_on="key2")
> ```
> 
> **Key parameters:**
> - `on` - Column name if same in both
> - `left_on`, `right_on` - Different key names
> 
> **Requirements:**
> - Key columns with common values
> - Keys must be same data type
> 
> **Result:** Same/fewer rows, more columns

**Q80.** Describe the SQL keyword order and explain why it matters. Then provide an example query demonstrating all five keywords.

> [!success]- Answer
> **SQL Keyword Order:**
> 1. SELECT
> 2. FROM
> 3. JOIN
> 4. WHERE
> 5. GROUP BY
> 
> **Why it matters:**
> - SQL queries MUST follow this order to execute
> - Out-of-order keywords cause syntax errors
> - Reflects logical query processing flow
> 
> **Complete Example:**
> ```sql
> SELECT hpd311calls.borough,          -- 1. SELECT (what to return)
>        COUNT(*) AS call_count,
>        boro_census.total_population
> FROM hpd311calls                     -- 2. FROM (main table)
> JOIN boro_census                     -- 3. JOIN (add related table)
> ON hpd311calls.borough = boro_census.borough
> WHERE hpd311calls.complaint_type = 'PLUMBING'  -- 4. WHERE (filter)
> GROUP BY hpd311calls.borough;        -- 5. GROUP BY (aggregate)
> ```
> 
> **What it does:**
> - Joins call data with census data
> - Filters for plumbing complaints only
> - Groups by borough
> - Returns borough, call count, and population for each borough
> 
> **Note:** Not all keywords required in every query, but those used must follow this order.

---

## Answer Key & Scoring

### Section A: Multiple Choice (100 points)
**Part 1 (Q1-10):** 1.B | 2.B | 3.C | 4.B | 5.C | 6.C | 7.B | 8.B | 9.C | 10.B  
**Part 2 (Q11-20):** 11.B | 12.B | 13.B | 14.B | 15.B | 16.B | 17.B | 18.C | 19.B | 20.B  
**Part 3 (Q21-30):** 21.B | 22.B | 23.C | 24.B | 25.C | 26.C | 27.B | 28.B | 29.C | 30.B  
**Part 4 (Q31-50):** 31.C | 32.D | 33.D | 34.B | 35.C | 36.B | 37.D | 38.B | 39.B | 40.B | 41.B | 42.B | 43.C | 44.B | 45.B | 46.C | 47.B | 48.B | 49.C | 50.B

### Section B: True/False (20 points)
51.F | 52.T | 53.T | 54.F | 55.F | 56.F | 57.T | 58.F | 59.T | 60.F  
61.F | 62.T | 63.F | 64.F | 65.T | 66.F | 67.F | 68.T | 69.F | 70.T

### Section C: Short Answer (30 points)
Q71-Q80: See detailed answers in collapsible sections (3 points each)

---

## Master Reference Guide - All Data Formats

### Complete Import Pattern Matrix

| Format | Function | Key Parameters | Returns |
|--------|----------|----------------|---------|
| **CSV/TSV** | `pd.read_csv()` | `sep`, `nrows`, `skiprows`, `usecols`, `dtype`, `na_values` | DataFrame |
| **Excel** | `pd.read_excel()` | `sheet_name`, `nrows`, `skiprows`, `usecols`, `dtype`, `true_values`, `false_values`, `parse_dates` | DataFrame or OrderedDict |
| **SQL** | `pd.read_sql()` | `query`, `engine` | DataFrame |
| **JSON file** | `pd.read_json()` | `orient`, `dtype` | DataFrame |
| **API/JSON dict** | `pd.DataFrame()` | Dictionary data | DataFrame |
| **Nested JSON** | `json_normalize()` | `sep`, `record_path`, `meta`, `meta_prefix` | DataFrame |

### Common Parameters Across Functions

| Parameter | Works With | Purpose |
|-----------|------------|---------|
| `dtype` | read_csv(), read_excel() | Specify column data types |
| `nrows` | read_csv(), read_excel() | Limit rows loaded |
| `skiprows` | read_csv(), read_excel() | Skip specified rows |
| `usecols` | read_csv(), read_excel() | Select specific columns |
| `na_values` | read_csv(), read_excel() | Custom missing value indicators |
| `parse_dates` | read_csv(), read_excel() | Parse datetime columns |

### Function-Specific Parameters

| Parameter | Function | Purpose |
|-----------|----------|---------|
| `sep` | read_csv() | Specify delimiter |
| `sheet_name` | read_excel() | Select sheet(s) |
| `true_values` | read_excel() | Custom True values for bool |
| `false_values` | read_excel() | Custom False values for bool |
| `orient` | read_json() | JSON data layout |
| `record_path` | json_normalize() | Path to nested data |
| `meta` | json_normalize() | Parent attributes to keep |
| `meta_prefix` | json_normalize() | Prefix for meta columns |
| `on` | merge() | Key column (same name) |
| `left_on` / `right_on` | merge() | Key columns (different names) |
| `ignore_index` | concat() | Renumber rows |

### Database Connection & Query Pattern

```python
# Imports
from sqlalchemy import create_engine
import pandas as pd

# Create engine
engine = create_engine("sqlite:///database.db")

# Query (method 1: table name)
df = pd.read_sql("table_name", engine)

# Query (method 2: SQL query)
query = """
SELECT column1, column2
FROM table
WHERE condition
GROUP BY column1;
"""
df = pd.read_sql(query, engine)
```

### API Access & JSON Pattern

```python
# Imports
import requests
import pandas as pd
from pandas.io.json import json_normalize

# API call
response = requests.get(api_url,
                       params={"key": "value"},
                       headers={"Authorization": "Bearer TOKEN"})

# Extract data
data = response.json()

# Simple DataFrame
df = pd.DataFrame(data["items"])

# Flatten nested
df = json_normalize(data["items"], sep="_")

# Deeply nested
df = json_normalize(data["items"],
                   sep="_",
                   record_path="nested_array",
                   meta=["parent_attr"],
                   meta_prefix="parent_")
```

### Combining DataFrames Pattern

```python
# Concatenate (add rows)
combined = pd.concat([df1, df2, df3], ignore_index=True)

# Merge (add columns) - same key name
merged = df1.merge(df2, on="key_col")

# Merge - different key names
merged = df1.merge(df2, left_on="key1", right_on="key2")
```

---

## Critical Concepts - Master Checklist

### Chapter 1: Flat Files
- [ ] `pd.read_csv()` is the function for flat files
- [ ] `sep` parameter specifies delimiter (e.g., `"\t"` for TSV)
- [ ] `nrows` limits number of rows, `skiprows` skips rows
- [ ] `usecols` selects columns (by name or number)
- [ ] Set `header=None` when skipping past header
- [ ] `names` parameter requires name for every column
- [ ] `dtype` specifies column data types
- [ ] `na_values` sets custom missing values
- [ ] `error_bad_lines=False` skips unparseable records

### Chapter 2: Spreadsheets
- [ ] `pd.read_excel()` for Excel files
- [ ] Spreadsheets can have formatting/formulas (unlike flat files)
- [ ] Default loads first sheet only
- [ ] `sheet_name=None` loads all sheets → returns OrderedDict
- [ ] `sheet_name` accepts name, number, or list
- [ ] `usecols` can use Excel letters: "A:F"
- [ ] Boolean columns load as float by default
- [ ] NA in Boolean columns → converted to True
- [ ] `dtype={"col": bool}` sets Boolean type
- [ ] `true_values`/`false_values` only work with bool dtype
- [ ] `parse_dates` (NOT dtype!) for datetime columns
- [ ] `pd.to_datetime()` for non-standard date formats
- [ ] Datetime codes: %Y, %m, %d, %H, %M, %S

### Chapter 3: Databases
- [ ] Import: `from sqlalchemy import create_engine`
- [ ] SQLite URL: `sqlite:///filename.db` (three slashes!)
- [ ] `pd.read_sql(query, engine)` queries database
- [ ] Can pass table name or SQL query string
- [ ] `SELECT * FROM table` gets all data
- [ ] String matching is case-sensitive
- [ ] `<>` means "not equal to"
- [ ] AND = all conditions; OR = at least one
- [ ] Five aggregates: SUM, AVG, MAX, MIN, COUNT
- [ ] `COUNT(*)` counts rows; `COUNT(DISTINCT col)` counts unique
- [ ] Must select grouped column with GROUP BY
- [ ] Use dot notation (table.column) with multiple tables
- [ ] JOIN requires same data type keys
- [ ] SQL order: SELECT, FROM, JOIN, WHERE, GROUP BY

### Chapter 4: JSON & APIs
- [ ] JSON is not tabular
- [ ] Records can have different attributes
- [ ] Record orientation most common
- [ ] Column orientation more space-efficient
- [ ] APIs provide data without database details
- [ ] `requests.get(url, params=dict, headers=dict)`
- [ ] `response.json()` returns dictionary
- [ ] Use `pd.DataFrame()` for API responses, NOT `read_json()`
- [ ] `read_json()` expects strings, not dictionaries
- [ ] Nested JSON: value itself is an object
- [ ] Import: `from pandas.io.json import json_normalize`
- [ ] Default separator is `.` (dot), change with `sep`
- [ ] `record_path` points to nested data
- [ ] `meta` preserves parent attributes
- [ ] `meta_prefix` prefixes meta columns
- [ ] `concat()` adds rows (vertical stacking)
- [ ] `merge()` adds columns (horizontal joining)
- [ ] `ignore_index=True` renumbers rows in concat
- [ ] Merge keys must be same data type
- [ ] Same key name: use `on`; Different: use `left_on`/`right_on`

---

## Common Cross-Chapter Pitfalls

❌ **Data Type Issues:**
- Forgetting Boolean columns load as float
- Using `dtype` for datetime (use `parse_dates`!)
- Not matching data types in merge/join keys

❌ **Parameter Confusion:**
- Using `on` when keys have different names
- Forgetting `header=None` with skiprows
- Not setting all columns when using `names`
- Thinking `sep` in read_csv() is same as `sep` in json_normalize()

❌ **API/JSON Mistakes:**
- Using `read_json()` with `response.json()` output
- Forgetting separate import for `json_normalize`
- Thinking default separator is underscore (it's dot!)
- Not extracting data with `response.json()`

❌ **SQL Errors:**
- Wrong URL format (forgot third slash in sqlite:///)
- Keywords in wrong order
- Assuming string matching is case-insensitive
- Not selecting grouped columns in GROUP BY

❌ **Function Misuse:**
- Using `concat()` when you need `merge()`
- Using `merge()` when you need `concat()`
- Forgetting `ignore_index=True` in concat
- Not checking if merge keys exist in both DataFrames

---

## Study Strategy - Final Prep (5-Day Plan)

### Day 1: Flat Files & Spreadsheets Review
- Review Chapters 1 & 2 study notes
- Practice all read_csv() and read_excel() parameters
- Master Boolean and datetime handling
- Focus on: sep, nrows, skiprows, usecols, dtype, parse_dates
- Common mistakes: header=None, names parameter, true_values

### Day 2: Databases & SQL
- Review Chapter 3 study notes
- Practice writing SQL queries
- Master WHERE, AND/OR, GROUP BY, JOIN
- Remember: case-sensitive strings, <> operator, dot notation
- SQL order: SELECT, FROM, JOIN, WHERE, GROUP BY

### Day 3: JSON & APIs
- Review Chapter 4 study notes
- Practice API workflow with requests
- Master json_normalize() with all parameters
- Remember: DataFrame() not read_json() for APIs
- Separate import needed for json_normalize

### Day 4: Data Combining & Cross-Chapter Concepts
- Practice concat() vs merge()
- Review parameters common across functions
- Study when to use which function for which format
- Focus on data type requirements for joins/merges
- Review all parameter purposes and compatibilities

### Day 5: Practice Exam & Weak Areas
- Take this comprehensive practice exam
- Review all incorrect answers thoroughly
- Create flashcards for persistently difficult concepts
- Quick review of all 4 chapter quizzes
- Rest well before actual exam

---

## Final Exam Day Tips

✅ **Read carefully** - Many questions test subtle distinctions  
✅ **Check data types** - Common source of errors across all chapters  
✅ **Remember defaults** - Know what happens without parameters  
✅ **Watch for format clues** - CSV vs Excel vs SQL vs JSON have different rules  
✅ **SQL keyword order** - Always SELECT, FROM, JOIN, WHERE, GROUP BY  
✅ **API workflow** - requests.get() → response.json() → pd.DataFrame()  
✅ **Boolean pitfalls** - Float default, NA→True conversion  
✅ **Import statements** - Know what needs importing (sqlalchemy, json_normalize)  
✅ **Concat vs Merge** - Rows vs columns, when to use each  
✅ **Parameter pairing** - Some parameters only work together  

---

**🎉 Congratulations on completing your comprehensive review!**

You now have a complete practice exam covering all four chapters of the Streamlined Data Ingestion with pandas course. Use this to assess your readiness and identify any remaining weak areas.

**Good luck on your certification! 🚀**

---

#datacamp #pandas #comprehensive #final-exam #certification #data-ingestion #csv #excel #sql #json #apis