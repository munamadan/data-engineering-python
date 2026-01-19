	
## Section A: Multiple Choice (30 Questions)

### Flat Files (1-10)

**Q1.** What is a flat file?

- [ ] A) Compressed file
- [x] B) Text file containing records (table data)
- [ ] C) Binary file
- [ ] D) Image file

**Q2.** Which function in pandas reads CSV files?

- [ ] A) pd.load_csv()
- [x] B) pd.read_csv()
- [ ] C) pd.import_csv()
- [ ] D) pd.get_csv()

**Q3.** What does `skiprows=1` do in `np.loadtxt()`?

- [ ] A) Skips first column
- [x] B) Skips first row (typically header)
- [ ] C) Skips last row
- [ ] D) Skips empty rows

**Q4.** Why use context manager (`with` statement) for files?

- [ ] A) It's faster
- [x] B) Automatically closes the file
- [ ] C) Required by Python
- [ ] D) Compresses the file

**Q5.** What limitation does NumPy have for importing?

- [ ] A) Can't handle large files
- [ ] B) Too slow
- [x] C) Struggles with mixed datatypes
- [ ] D) No delimiter support

**Q6.** What does `usecols=[0, 2]` do?

- [ ] A) Skips columns 0 and 2
- [x] B) Keeps only columns at index 0 and 2
- [ ] C) Uses columns as index
- [ ] D) Sorts by columns 0 and 2

**Q7.** Which is the standard for data analysis in Python?

- [ ] A) NumPy arrays only
- [ ] B) Lists
- [x] C) pandas DataFrames
- [ ] D) Dictionaries

**Q8.** What mode opens a file for reading?

- [ ] A) 'w'
- [x] B) 'r'
- [ ] C) 'a'
- [ ] D) 'x'

**Q9.** What is the delimiter in CSV files?

- [ ] A) Tab
- [ ] B) Space
- [x] C) Comma
- [ ] D) Semicolon

**Q10.** How do you convert pandas DataFrame to NumPy array?

- [ ] A) df.to_array()
- [ ] B) df.to_numpy()
- [x] C) np.array(df)
- [ ] D) df.array()

### Other File Types (11-20)

**Q11.** What file mode is used for pickled files?

- [ ] A) 'r'
- [ ] B) 'w'
- [ ] C) 'rb'
- [x] D) 'rp'

**Q12.** What does HDF5 stand for?

- [ ] A) High Data Format 5
- [x] B) Hierarchical Data Format version 5
- [ ] C) Header Data File 5
- [ ] D) Hybrid Data Format 5

**Q13.** What does `scipy.io.loadmat()` return?

- [ ] A) List
- [ ] B) DataFrame
- [ ] C) Dictionary
- [x] D) NumPy array

**Q14.** Which file type is native to Python?

- [ ] A) Excel
- [x] B) Pickled files
- [ ] C) MATLAB
- [ ] D) HDF5

**Q15.** How do you access an Excel sheet by index?

- [x] A) xl.parse('0')
- [ ] B) xl.parse(0)
- [ ] C) xl.sheet(0)
- [ ] D) xl[0]

**Q16.** What is Stata primarily used for?

- [x] A) Engineering
- [ ] B) Academic social sciences research
- [ ] C) Web development
- [ ] D) Game development

**Q17.** HDF5 files can scale to:

- [ ] A) Megabytes
- [x] B) Gigabytes
- [ ] C) Terabytes
- [ ] D) Exabytes

**Q18.** Which library imports MATLAB files?

- [x] A) matlab
- [ ] B) scipy
- [ ] C) h5py
- [ ] D) pandas

**Q19.** What is serialization?

- [x] A) Sorting data
- [ ] B) Converting object to bytestream
- [ ] C) Compressing files
- [ ] D) Encrypting data

**Q20.** What does MATLAB stand for?

- [ ] A) Math Laboratory
- [x] B) Matrix Laboratory
- [ ] C) Mathematical Lab
- [ ] D) Matrix Library

### Relational Databases (21-30)

**Q21.** Who described the relational model?

- [ ] A) Bill Gates
- [x] B) Edgar "Ted" Codd
- [ ] C) Dennis Ritchie
- [ ] D) Linus Torvalds

**Q22.** What library creates database engines?

- [ ] A) pandas
- [x] B) sqlite3
- [ ] C) SQLAlchemy
- [ ] D) dbengine

**Q23.** What does `SELECT * FROM Orders` do?

- [ ] A) Deletes orders
- [x] B) Returns all columns and rows from Orders
- [ ] C) Creates Orders table
- [ ] D) Counts orders

**Q24.** What does INNER JOIN return?

- [ ] A) All records from both tables
- [x] B) Records with matching values in both tables
- [ ] C) Only first table records
- [ ] D) Only second table records

**Q25.** What is the SQLite connection string format?

- [x] A) `'sqlite://db.sqlite'`
- [ ] B) `'sqlite:///db.sqlite'`
- [ ] C) `'db://sqlite'`
- [ ] D) `'sqlite:/db.sqlite'`

**Q26.** Which pandas function queries databases?

- [ ] A) pd.sql_query()
- [ ] B) pd.query_db()
- [x] C) pd.read_sql_query()
- [ ] D) pd.read_db()

**Q27.** What does `engine.table_names()` return?

- [ ] A) First table
- [x] B) List of all table names
- [ ] C) Number of tables
- [ ] D) Table contents

**Q28.** What does WHERE clause do?

- [ ] A) Joins tables
- [x] B) Filters rows by condition
- [ ] C) Selects columns
- [ ] D) Sorts results

**Q29.** What does `rs.fetchall()` do?

- [ ] A) Fetches one row
- [x] B) Fetches all remaining rows
- [ ] C) Fetches column names
- [ ] D) Closes connection

**Q30.** How are tables linked in relational databases?

- [ ] A) File names
- [x] B) Primary and foreign keys
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