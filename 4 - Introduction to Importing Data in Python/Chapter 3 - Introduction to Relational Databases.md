
## 📚 What is a Relational Database?

**Definition:** Database based on the relational model of data

**History:** First described by Edgar "Ted" Codd

### Key Characteristics

**Relational Model:**

- Widely adopted standard
- Governed by **Codd's 12 Rules/Commandments**
    - Actually consists of 13 rules (zero-indexed!)
    - Describes what a Relational Database Management System (RDBMS) should adhere to

---

## 🗃️ Database Structure

### Tables (Relations)

**Example: Northwind Database**

Tables include:

- **Orders** - Customer orders
- **Customers** - Customer information
- **Employees** - Employee details
- **Products** - Product catalog
- **Suppliers** - Supplier information
- **Categories** - Product categories
- **Shippers** - Shipping companies

### How Tables are Linked

**Key Concept:** Tables are connected through common fields

**Example:**

- `Orders` table has `CustomerID` field
- `Customers` table has `CustomerID` field
- These can be **joined** to get customer details for each order

---

## 🔧 Relational Database Management Systems (RDBMS)

### Popular RDBMS

Common systems include:

- **SQLite** - Lightweight, file-based
- **PostgreSQL** - Open-source, powerful
- **MySQL** - Popular web database
- **Microsoft SQL Server** - Enterprise database
- **Oracle Database** - Enterprise database

---

## 🐍 Creating a Database Engine in Python

### Using SQLAlchemy

**Why SQLAlchemy?**

- Works with many different RDBMS
- Provides unified interface
- Handles connection management

### Basic Setup

```python
from sqlalchemy import create_engine

# Create engine for SQLite database
engine = create_engine('sqlite:///Northwind.sqlite')
```

**Connection String Format:** `sqlite:///database_name.sqlite`

### Getting Table Names

```python
from sqlalchemy import create_engine

engine = create_engine('sqlite:///Northwind.sqlite')
table_names = engine.table_names()

print(table_names)
```

**Output:**

```python
['Categories', 'Customers', 'EmployeeTerritories',
 'Employees', 'Order Details', 'Orders', 'Products',
 'Region', 'Shippers', 'Suppliers', 'Territories']
```

---

## 📊 Querying Databases

### Basic SQL Query

**SELECT Statement:**

```sql
SELECT * FROM Table_Name
```

**Meaning:** Returns all columns of all rows from the table

**Example:**

```sql
SELECT * FROM Orders
```

### SQL Querying Workflow

**Standard Process:**

1. Import packages and functions
2. Create the database engine
3. Connect to the engine
4. Query the database
5. Save query results to DataFrame
6. Close the connection

---

## 💻 Your First SQL Query

### Method 1: Manual Connection Management

```python
from sqlalchemy import create_engine
import pandas as pd

# Create engine
engine = create_engine('sqlite:///Northwind.sqlite')

# Connect
con = engine.connect()

# Execute query
rs = con.execute("SELECT * FROM Orders")

# Fetch results into DataFrame
df = pd.DataFrame(rs.fetchall())

# Close connection
con.close()
```

**Output (without column names):**

```
    0      1    2                    3                    4
0  10248  VINET  5  7/4/1996 12:00:00 AM  8/1/1996 12:00:00 AM
1  10251  VICTE  3  7/8/1996 12:00:00 AM  8/5/1996 12:00:00 AM
```

### Setting Column Names

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///Northwind.sqlite')
con = engine.connect()
rs = con.execute("SELECT * FROM Orders")
df = pd.DataFrame(rs.fetchall())

# Set column names from query result
df.columns = rs.keys()

con.close()
```

**Output (with column names):**

```
   OrderID CustomerID  EmployeeID              OrderDate
0    10248      VINET           5  7/4/1996 12:00:00 AM
1    10251      VICTE           3  7/8/1996 12:00:00 AM
```

---

## 🔒 Using Context Manager

### Best Practice: Automatic Connection Closing

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///Northwind.sqlite')

with engine.connect() as con:
    rs = con.execute("SELECT OrderID, OrderDate, ShipName FROM Orders")
    df = pd.DataFrame(rs.fetchmany(size=5))
    df.columns = rs.keys()
    
# Connection automatically closed after with block
```

**Advantages:**

- Automatically closes connection
- Exception-safe
- Cleaner code

### Fetching Methods

|Method|Purpose|
|---|---|
|`rs.fetchall()`|Fetch all remaining rows|
|`rs.fetchmany(size=n)`|Fetch next n rows|
|`rs.fetchone()`|Fetch next single row|

---

## 🐼 The pandas Way

### Simplified Query with pandas

**Direct query method:**

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///Northwind.sqlite')

# Query directly to DataFrame
df = pd.read_sql_query("SELECT * FROM Orders", engine)
```

**Benefits:**

- One-line query
- Automatic column naming
- No need to manually manage connections
- Returns DataFrame directly

### Comparison: Traditional vs pandas Way

**Traditional Method:**

```python
with engine.connect() as con:
    rs = con.execute("SELECT * FROM Orders")
    df = pd.DataFrame(rs.fetchall())
    df.columns = rs.keys()
```

**pandas Method:**

```python
df = pd.read_sql_query("SELECT * FROM Orders", engine)
```

**Recommendation:** Use `pd.read_sql_query()` for simplicity

---

## 🔗 Advanced Querying: Table Relationships

### Understanding JOINs

**Purpose:** Combine data from multiple tables based on related columns

### INNER JOIN

**Definition:** Returns records that have matching values in both tables

**Syntax:**

```sql
SELECT column1, column2
FROM table1
INNER JOIN table2
ON table1.common_field = table2.common_field
```

### INNER JOIN Example in Python

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///Northwind.sqlite')

query = """
SELECT OrderID, CompanyName 
FROM Orders
INNER JOIN Customers 
ON Orders.CustomerID = Customers.CustomerID
"""

df = pd.read_sql_query(query, engine)
print(df.head())
```

**Output:**

```
   OrderID                  CompanyName
0    10248  Vins et alcools Chevalier
1    10251     Victuailles en stock
2    10254       Chop-suey Chinese
3    10256    Wellington Importadora
4    10258             Ernst Handel
```

**What happened:**

- Joined `Orders` table with `Customers` table
- Matched on `CustomerID` field
- Retrieved `OrderID` from Orders and `CompanyName` from Customers

---

## 📝 SQL Query Components

### SELECT Statement

**Purpose:** Specify which columns to retrieve

```sql
-- All columns
SELECT * FROM Orders

-- Specific columns
SELECT OrderID, CustomerID, OrderDate FROM Orders
```

### WHERE Clause

**Purpose:** Filter rows based on conditions

```sql
SELECT * FROM Orders
WHERE CustomerID = 'VINET'
```

### JOIN Clause

**Purpose:** Combine tables

```sql
SELECT Orders.OrderID, Customers.CompanyName
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID = Customers.CustomerID
```

---

## 🎯 Key Concepts Summary

### Relational Database Components

|Component|Description|
|---|---|
|**Table**|Collection of related data (rows and columns)|
|**Row**|Single record in a table|
|**Column**|Attribute or field|
|**Primary Key**|Unique identifier for each row|
|**Foreign Key**|References primary key in another table|
|**Relationship**|Connection between tables via keys|

### Query Methods Comparison

|Method|Code Complexity|Use Case|
|---|---|---|
|**Manual**|High|Learning, fine control|
|**Context Manager**|Medium|Better than manual|
|**pandas**|Low|Production, simplicity|

### INNER JOIN Concept

```
Table A          Table B          Result (INNER JOIN)
-------          -------          -------------------
ID  Name         ID  Value        ID  Name  Value
1   Alice        1   100          1   Alice  100
2   Bob          3   300          3   Carol  300
3   Carol        3   300
```

Only matching IDs appear in result

---

## 💡 Best Practices

### 1. Always Use Context Managers or pandas

**Good:**

```python
with engine.connect() as con:
    # query here
```

**Better:**

```python
df = pd.read_sql_query(query, engine)
```

### 2. Set Column Names

```python
df.columns = rs.keys()  # If using manual method
```

### 3. Use Parameterized Queries

**Avoid:**

```python
query = f"SELECT * FROM Users WHERE name = '{user_input}'"  # SQL injection risk!
```

**Prefer:**

```python
query = "SELECT * FROM Users WHERE name = ?"
df = pd.read_sql_query(query, engine, params=(user_input,))
```

### 4. Limit Results for Testing

```python
# Test query with limited results
df = pd.read_sql_query("SELECT * FROM Orders LIMIT 10", engine)
```

---

## 🔧 Common Patterns

### Basic Query

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///database.sqlite')
df = pd.read_sql_query("SELECT * FROM table_name", engine)
```

### Query with WHERE

```python
query = "SELECT * FROM Orders WHERE CustomerID = 'VINET'"
df = pd.read_sql_query(query, engine)
```

### Query with JOIN

```python
query = """
SELECT t1.col1, t2.col2
FROM table1 t1
INNER JOIN table2 t2
ON t1.id = t2.id
"""
df = pd.read_sql_query(query, engine)
```

### List All Tables

```python
engine = create_engine('sqlite:///database.sqlite')
tables = engine.table_names()
print(tables)
```

---

## 📚 What You've Learned

**Topics Covered:**

- Relational databases and their structure
- Creating database engines with SQLAlchemy
- Basic SQL queries
    - `SELECT` - Choose columns
    - `WHERE` - Filter rows
    - `JOIN` - Combine tables
- Querying with pandas (`pd.read_sql_query()`)
- Table relationships and INNER JOIN

**Next Course:** Scraping data from the web and interacting with APIs

---

## 📖 Quick Reference

### SQLAlchemy Engine

```python
from sqlalchemy import create_engine
engine = create_engine('sqlite:///database.sqlite')
```

### Query Methods

```python
# Method 1: Manual
con = engine.connect()
rs = con.execute("SELECT * FROM table")
df = pd.DataFrame(rs.fetchall())
df.columns = rs.keys()
con.close()

# Method 2: Context manager
with engine.connect() as con:
    rs = con.execute("SELECT * FROM table")
    df = pd.DataFrame(rs.fetchall())
    df.columns = rs.keys()

# Method 3: pandas (recommended)
df = pd.read_sql_query("SELECT * FROM table", engine)
```

### SQL Basics

```sql
-- Select all
SELECT * FROM table_name

-- Select specific columns
SELECT col1, col2 FROM table_name

-- Filter rows
SELECT * FROM table_name WHERE condition

-- Join tables
SELECT t1.col1, t2.col2
FROM table1 t1
INNER JOIN table2 t2
ON t1.key = t2.key
```

---

#python #sql #databases #sqlalchemy #pandas #relational-databases #datacamp

## Section A: Multiple Choice (30 Questions)

### Flat Files (1-10)

**Q1.** What is a flat file?

- [ ] A) Compressed file
- [ ] B) Text file containing records (table data)
- [ ] C) Binary file
- [ ] D) Image file

**Q2.** Which function in pandas reads CSV files?

- [ ] A) pd.load_csv()
- [ ] B) pd.read_csv()
- [ ] C) pd.import_csv()
- [ ] D) pd.get_csv()

**Q3.** What does `skiprows=1` do in `np.loadtxt()`?

- [ ] A) Skips first column
- [ ] B) Skips first row (typically header)
- [ ] C) Skips last row
- [ ] D) Skips empty rows

**Q4.** Why use context manager (`with` statement) for files?

- [ ] A) It's faster
- [ ] B) Automatically closes the file
- [ ] C) Required by Python
- [ ] D) Compresses the file

**Q5.** What limitation does NumPy have for importing?

- [ ] A) Can't handle large files
- [ ] B) Too slow
- [ ] C) Struggles with mixed datatypes
- [ ] D) No delimiter support

**Q6.** What does `usecols=[0, 2]` do?

- [ ] A) Skips columns 0 and 2
- [ ] B) Keeps only columns at index 0 and 2
- [ ] C) Uses columns as index
- [ ] D) Sorts by columns 0 and 2

**Q7.** Which is the standard for data analysis in Python?

- [ ] A) NumPy arrays only
- [ ] B) Lists
- [ ] C) pandas DataFrames
- [ ] D) Dictionaries

**Q8.** What mode opens a file for reading?

- [ ] A) 'w'
- [ ] B) 'r'
- [ ] C) 'a'
- [ ] D) 'x'

**Q9.** What is the delimiter in CSV files?

- [ ] A) Tab
- [ ] B) Space
- [ ] C) Comma
- [ ] D) Semicolon

**Q10.** How do you convert pandas DataFrame to NumPy array?

- [ ] A) df.to_array()
- [ ] B) df.to_numpy()
- [ ] C) np.array(df)
- [ ] D) df.array()

### Other File Types (11-20)

**Q11.** What file mode is used for pickled files?

- [ ] A) 'r'
- [ ] B) 'w'
- [ ] C) 'rb'
- [ ] D) 'rp'

**Q12.** What does HDF5 stand for?

- [ ] A) High Data Format 5
- [ ] B) Hierarchical Data Format version 5
- [ ] C) Header Data File 5
- [ ] D) Hybrid Data Format 5

**Q13.** What does `scipy.io.loadmat()` return?

- [ ] A) List
- [ ] B) DataFrame
- [ ] C) Dictionary
- [ ] D) NumPy array

**Q14.** Which file type is native to Python?

- [ ] A) Excel
- [ ] B) Pickled files
- [ ] C) MATLAB
- [ ] D) HDF5

**Q15.** How do you access an Excel sheet by index?

- [ ] A) xl.parse('0')
- [ ] B) xl.parse(0)
- [ ] C) xl.sheet(0)
- [ ] D) xl[0]

**Q16.** What is Stata primarily used for?

- [ ] A) Engineering
- [ ] B) Academic social sciences research
- [ ] C) Web development
- [ ] D) Game development

**Q17.** HDF5 files can scale to:

- [ ] A) Megabytes
- [ ] B) Gigabytes
- [ ] C) Terabytes
- [ ] D) Exabytes

**Q18.** Which library imports MATLAB files?

- [ ] A) matlab
- [ ] B) scipy
- [ ] C) h5py
- [ ] D) pandas

**Q19.** What is serialization?

- [ ] A) Sorting data
- [ ] B) Converting object to bytestream
- [ ] C) Compressing files
- [ ] D) Encrypting data

**Q20.** What does MATLAB stand for?

- [ ] A) Math Laboratory
- [ ] B) Matrix Laboratory
- [ ] C) Mathematical Lab
- [ ] D) Matrix Library

### Relational Databases (21-30)

**Q21.** Who described the relational model?

- [ ] A) Bill Gates
- [ ] B) Edgar "Ted" Codd
- [ ] C) Dennis Ritchie
- [ ] D) Linus Torvalds

**Q22.** What library creates database engines?

- [ ] A) pandas
- [ ] B) sqlite3
- [ ] C) SQLAlchemy
- [ ] D) dbengine

**Q23.** What does `SELECT * FROM Orders` do?

- [ ] A) Deletes orders
- [ ] B) Returns all columns and rows from Orders
- [ ] C) Creates Orders table
- [ ] D) Counts orders

**Q24.** What does INNER JOIN return?

- [ ] A) All records from both tables
- [ ] B) Records with matching values in both tables
- [ ] C) Only first table records
- [ ] D) Only second table records

**Q25.** What is the SQLite connection string format?

- [ ] A) `'sqlite://db.sqlite'`
- [ ] B) `'sqlite:///db.sqlite'`
- [ ] C) `'db://sqlite'`
- [ ] D) `'sqlite:/db.sqlite'`

**Q26.** Which pandas function queries databases?

- [ ] A) pd.sql_query()
- [ ] B) pd.query_db()
- [ ] C) pd.read_sql_query()
- [ ] D) pd.read_db()

**Q27.** What does `engine.table_names()` return?

- [ ] A) First table
- [ ] B) List of all table names
- [ ] C) Number of tables
- [ ] D) Table contents

**Q28.** What does WHERE clause do?

- [ ] A) Joins tables
- [ ] B) Filters rows by condition
- [ ] C) Selects columns
- [ ] D) Sorts results

**Q29.** What does `rs.fetchall()` do?

- [ ] A) Fetches one row
- [ ] B) Fetches all remaining rows
- [ ] C) Fetches column names
- [ ] D) Closes connection

**Q30.** How are tables linked in relational databases?

- [ ] A) File names
- [ ] B) Primary and foreign keys
- [ ] C) Row numbers
- [ ] D) Alphabetically

---

## Section B: True/False (15 Questions)

**Q31.** NumPy arrays are standard for numerical data storage. **T/F**

**Q32.** pandas can handle mixed datatypes easily. **T/F**

**Q33.** Context managers automatically close files. **T/F**

**Q34.** Pickled files use text mode 'r'. **T/F**

**Q35.** Excel files can contain multiple sheets. **T/F**

**Q36.** HDF5 has a flat, non-hierarchical structure. **T/F**

**Q37.** MATLAB is standard in engineering. **T/F**

**Q38.** SAS is commonly used in biostatistics. **T/F**

**Q39.** pd.read_sql_query() automatically sets column names. **T/F**

**Q40.** INNER JOIN returns all records from both tables. **T/F**

**Q41.** SQLAlchemy works with multiple RDBMS types. **T/F**

**Q42.** `skiprows=1` skips the first column. **T/F**

**Q43.** `.dta` is the file extension for Stata files. **T/F**

**Q44.** `with` statement is considered best practice for database connections. **T/F**

**Q45.** CSV stands for Comma Separated Values. **T/F**

---

## Section C: Short Answer (10 Questions)

**Q46.** List three methods to query a database and identify which is recommended.

**Q47.** Explain the difference between NumPy and pandas for importing data.

**Q48.** What are the components of a flat file structure?

**Q49.** Name the file extension and primary use case for: Excel, Pickled, MATLAB, SAS, Stata, HDF5.

**Q50.** What is the SQL workflow for querying databases?

**Q51.** How does serialization work in pickled files?

**Q52.** Explain what INNER JOIN does with an example.

**Q53.** What parameters customize `np.loadtxt()` and what does each do?

**Q54.** Why is HDF5 used for large datasets?

**Q55.** What is the relational model and who described it?

---

## Section D: Code Analysis & Scenarios (5 Questions)

**Q56.** Fix all issues in this code:

```python
import pandas as pd
file = open('data.csv', 'r')
data = pd.read_csv(file)
```

**Q57.** What's the best way to import this Excel file with 3 sheets?

```python
# file: sales_data.xlsx
# sheets: 'Q1', 'Q2', 'Q3'
# Requirement: Load all sheets into separate DataFrames
```

**Q58.** Complete this database query to join Orders and Customers:

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///shop.sqlite')
query = """
SELECT Orders.OrderID, Customers.CompanyName
FROM Orders
_____ Customers
ON _____
"""
df = pd.read_sql_query(query, engine)
```

**Q59.** A colleague gives you a file with mixed text and numerical data. Which import method should you use and why?

- File: employee_data.csv
- Contains: Name (string), Age (int), Salary (float), Department (string)

**Q60.** You need to store a complex Python dictionary with nested lists for later use. Which file format should you use and how would you save/load it?

---

## Answer Key & Scoring

### Section A (1-30): 1 point each = 30 points

1. B 2. B 3. B 4. B 5. C 6. B 7. C 8. B 9. C 10. B
2. C 12. B 13. C 14. B 15. B 16. B 17. D 18. B 19. B 20. B
3. B 22. C 23. B 24. B 25. B 26. C 27. B 28. B 29. B 30. B

### Section B (31-45): 1 point each = 15 points

31. T 32. T 33. T 34. F 35. T 36. F 37. T 38. T 39. T 40. F
32. T 42. F 43. T 44. T 45. T

### Section C (46-55): 3 points each = 30 points

**Q46:**

- Manual: con.execute() + DataFrame
- Context manager: with engine.connect()
- pandas: pd.read_sql_query() (recommended)

**Q47:**

- NumPy: Numerical data only, arrays
- pandas: Mixed types, DataFrames, labeled columns

**Q48:** Records (rows), Columns (attributes), Header (column names), Delimiter

**Q49:**

- .xlsx (Excel): Spreadsheets
- .pkl (Pickle): Python objects
- .mat (MATLAB): Engineering/science
- .sas7bdat (SAS): Business analytics
- .dta (Stata): Social sciences
- .hdf5 (HDF5): Large numerical data

**Q50:** Import → Create engine → Connect → Query → Save to DataFrame → Close

**Q51:** Converting Python object to bytestream for storage/transmission

**Q52:** Returns only records with matching values in both tables. Example: Orders.CustomerID = Customers.CustomerID

**Q53:**

- delimiter: character separating values
- skiprows: rows to skip from start
- usecols: column indices to keep
- dtype: data type

**Q54:** Scales to exabytes, hierarchical structure, efficient for TB-scale datasets

**Q55:** Database structure based on relations (tables); Edgar "Ted" Codd

### Section D (56-60): 5 points each = 25 points

**Q56:**

```python
import pandas as pd
with open('data.csv', 'r') as file:
    data = pd.read_csv(file)
# OR simply: data = pd.read_csv('data.csv')
```

**Q57:**

```python
import pandas as pd
xl_file = pd.ExcelFile('sales_data.xlsx')
q1 = xl_file.parse('Q1')
q2 = xl_file.parse('Q2')
q3 = xl_file.parse('Q3')
# OR using loop
dfs = {sheet: xl_file.parse(sheet) for sheet in xl_file.sheet_names}
```

**Q58:**

```python
query = """
SELECT Orders.OrderID, Customers.CompanyName
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID = Customers.CustomerID
"""
```

**Q59:** Use pandas (pd.read_csv()) because it handles mixed datatypes easily, while NumPy struggles with non-numerical data.

**Q60:** Use pickled files (.pkl):

```python
import pickle
# Save
with open('data.pkl', 'wb') as file:
    pickle.dump(my_dict, file)
# Load
with open('data.pkl', 'rb') as file:
    my_dict = pickle.load(file)
```

**Total: 100 points** **Pass: 70+ points**

---

## Quick Study Guide

### File Import Decision Tree

```
Is it a flat file?
├─ YES: Numerical only? → NumPy (loadtxt)
│       Mixed types? → pandas (read_csv)
│
├─ NO: What type?
    ├─ Excel → pd.ExcelFile().parse()
    ├─ Python object → pickle.load()
    ├─ MATLAB → scipy.io.loadmat()
    ├─ SAS → SAS7BDAT().to_data_frame()
    ├─ Stata → pd.read_stata()
    ├─ HDF5 → h5py.File()
    └─ Database → pd.read_sql_query()
```

### Essential Code Patterns

**Flat Files:**

```python
# NumPy
import numpy as np
data = np.loadtxt('file.csv', delimiter=',', skiprows=1)

# pandas
import pandas as pd
df = pd.read_csv('file.csv')
```

**Other Files:**

```python
# Pickle
import pickle
with open('file.pkl', 'rb') as f:
    data = pickle.load(f)

# Excel
xl = pd.ExcelFile('file.xlsx')
df = xl.parse('Sheet1')

# MATLAB
import scipy.io
mat = scipy.io.loadmat('file.mat')

# HDF5
import h5py
data = h5py.File('file.hdf5', 'r')
```

**Databases:**

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('sqlite:///db.sqlite')
df = pd.read_sql_query("SELECT * FROM table", engine)
```

### File Extensions Reference

|Extension|Type|Library|Use Case|
|---|---|---|---|
|`.csv`, `.txt`|Flat file|NumPy/pandas|Tabular data|
|`.pkl`|Pickled|pickle|Python objects|
|`.xlsx`|Excel|pandas|Spreadsheets|
|`.mat`|MATLAB|scipy|Engineering|
|`.sas7bdat`|SAS|sas7bdat|Statistics|
|`.dta`|Stata|pandas|Social science|
|`.hdf5`, `.h5`|HDF5|h5py|Large data|
|`.sqlite`|Database|SQLAlchemy|Relational data|

### Key Functions Summary

**NumPy:**

- `np.loadtxt(filename, delimiter, skiprows, usecols, dtype)`

**pandas:**

- `pd.read_csv(filename)`
- `pd.ExcelFile(filename).parse(sheet)`
- `pd.read_stata(filename)`
- `pd.read_sql_query(query, engine)`

**Other:**

- `pickle.load(file)`
- `scipy.io.loadmat(filename)`
- `h5py.File(filename, 'r')`
- `create_engine('sqlite:///db.sqlite')`

### SQL Basics

**SELECT:** Choose columns

```sql
SELECT col1, col2 FROM table
SELECT * FROM table
```

**WHERE:** Filter rows

```sql
SELECT * FROM table WHERE condition
```

**JOIN:** Combine tables

```sql
SELECT t1.col, t2.col
FROM table1 t1
INNER JOIN table2 t2
ON t1.key = t2.key
```

---

## Common Mistakes to Avoid

❌ Forgetting to close files (use context managers!) ❌ Using NumPy for mixed datatypes ❌ Wrong file mode for pickled files ('rb' not 'r') ❌ Not setting column names after SQL query ❌ Forgetting triple slash in SQLite connection string ❌ Using SELECT without FROM clause ❌ Wrong JOIN syntax (missing ON clause) ❌ Not converting map objects to lists ❌ Mixing up sheet name (string) vs index (int) in Excel

---

## Study Strategy

### Day 1-2: Flat Files

- Practice with CSV and text files
- Master NumPy loadtxt parameters
- Understand pandas read_csv
- Know when to use each

### Day 3-4: Other File Types

- Memorize file extensions
- Practice each import method
- Understand use cases
- Focus on pickle, Excel, MATLAB

### Day 5-6: Databases

- Learn SQL basics (SELECT, WHERE, JOIN)
- Practice with SQLAlchemy
- Master pd.read_sql_query()
- Understand table relationships

### Day 7: Review & Practice

- Take this comprehensive exam
- Review wrong answers
- Practice weak areas
- Do hands-on coding

---

## Final Checklist

### Before Exam

- [ ] Can import CSV with NumPy and pandas
- [ ] Know all file extensions
- [ ] Can write basic SQL queries
- [ ] Understand INNER JOIN
- [ ] Know when to use each import method
- [ ] Memorized key functions
- [ ] Practiced with context managers
- [ ] Understand database workflow

### During Exam

- [ ] Read questions carefully
- [ ] Eliminate wrong answers
- [ ] Check file extensions
- [ ] Remember: pandas for mixed types, NumPy for numbers
- [ ] SQLite uses triple slash: `sqlite:///`
- [ ] Context managers auto-close
- [ ] pd.read_sql_query() is simplest for databases

---

## Congratulations!

You've covered comprehensive data importing in Python:

- ✅ Flat files (CSV, TXT)
- ✅ Various file formats (Excel, MATLAB, SAS, Stata, HDF5, Pickle)
- ✅ Relational databases (SQL, SQLAlchemy)
- ✅ Best practices and patterns

**You're ready to import any data source!** 📊🐍

---

#python #data-import #comprehensive-exam #numpy #pandas #sql #databases #certification