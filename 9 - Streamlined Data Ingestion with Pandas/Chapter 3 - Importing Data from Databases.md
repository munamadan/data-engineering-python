
## Overview
This chapter covers working with relational databases using SQL queries in pandas, including connecting to databases, querying data, filtering, aggregating, and joining tables.

## Key Concepts

### Relational Databases
**Structure:**
- Data about entities is organized into **tables**
- Each **row** or **record** is an instance of an entity
- Each **column** has information about an attribute
- Tables can be **linked** to each other via unique keys

**Advantages:**
- Support more data
- Multiple simultaneous users
- Data quality controls
- Data types are specified for each column

**Interaction:**
- **SQL (Structured Query Language)** to interact with databases

### Common Relational Databases
- **SQLite**: Databases are computer files (simple, file-based)

## Connecting to Databases

### Two-Step Process
1. Create way to connect to database
2. Query database

### Creating a Database Engine

```python
from sqlalchemy import create_engine

# Create engine to handle database connections
engine = create_engine("sqlite:///filename.db")
```

**Components:**
- **Function**: `sqlalchemy`'s `create_engine()` makes an engine to handle database connections
- **Parameter**: Needs string URL of database to connect to
- **SQLite URL format**: `sqlite:///filename.db`

### Querying Databases

```python
pd.read_sql(query, engine)
```

**Arguments:**
- **`query`**: String containing SQL query to run or table to load
- **`engine`**: Connection/database engine object

## SQL Review: SELECT

### Basic SELECT Statement
**Purpose**: Used to query data from a database

**Basic syntax:**
```sql
SELECT [column_names] FROM [table_name];
```

**Get all data in a table:**
```sql
SELECT * FROM [table_name];
```

**Code style:**
- Keywords in ALL CAPS
- Semicolon (;) to end a statement

### Getting Data from a Database

#### Method 1: Load by Table Name
```python
import pandas as pd
from sqlalchemy import create_engine

# Create database engine to manage connections
engine = create_engine("sqlite:///data.db")

# Load entire weather table by table name
weather = pd.read_sql("weather", engine)
```

#### Method 2: Load with SQL Query
```python
# Load entire weather table with SQL
weather = pd.read_sql("SELECT * FROM weather", engine)
```

> [!info] Both Methods Work
> You can pass either a table name or a SQL query to `pd.read_sql()`

## Refining Imports with SQL Queries

### SELECTing Specific Columns

**Syntax:**
```sql
SELECT [column names] FROM [table name];
```

**Example:**
```sql
SELECT date, tavg
FROM weather;
```

### WHERE Clauses
**Purpose**: Use a WHERE clause to selectively import records

**Syntax:**
```sql
SELECT [column_names]
FROM [table_name]
WHERE [condition];
```

### Filtering by Numbers

**Mathematical operators:**
- `=` (equal to)
- `>` and `>=` (greater than, greater than or equal)
- `<` and `<=` (less than, less than or equal)
- `<>` (not equal to)

**Example:**
```sql
SELECT *
FROM weather
WHERE tmax > 32;
```

### Filtering Text

**Rules:**
- Match exact strings with the `=` sign and the text to match
- **String matching is case-sensitive**

**Example:**
```sql
/* Get records about incidents in Brooklyn */
SELECT *
FROM hpd311calls
WHERE borough = 'BROOKLYN';
```

### SQL and pandas Integration

```python
# Load libraries
import pandas as pd
from sqlalchemy import create_engine

# Create database engine
engine = create_engine("sqlite:///data.db")

# Write query to get records from Brooklyn
query = """SELECT *
FROM hpd311calls
WHERE borough = 'BROOKLYN';"""

# Query the database
brooklyn_calls = pd.read_sql(query, engine)
print(brooklyn_calls.borough.unique())
# ['BROOKLYN']
```

### Combining Conditions: AND

**Purpose**: WHERE clauses with AND return records that meet **all** conditions

**Example:**
```python
# Write query to get records about plumbing in the Bronx
and_query = """SELECT *
FROM hpd311calls
WHERE borough = 'BRONX'
AND complaint_type = 'PLUMBING';"""

# Get calls about plumbing issues in the Bronx
bx_plumbing_calls = pd.read_sql(and_query, engine)
print(bx_plumbing_calls.shape)
# (2016, 8)
```

### Combining Conditions: OR

**Purpose**: WHERE clauses with OR return records that meet **at least one** condition

**Example:**
```python
# Write query to get records about water leaks or plumbing
or_query = """SELECT *
FROM hpd311calls
WHERE complaint_type = 'WATER LEAK'
OR complaint_type = 'PLUMBING';"""

# Get calls that are about plumbing or water leaks
leaks_or_plumbing = pd.read_sql(or_query, engine)
print(leaks_or_plumbing.shape)
# (10684, 8)
```

## More Complex SQL Queries

### Getting DISTINCT Values

**Purpose**: Get unique values for one or more columns

**Syntax:**
```sql
SELECT DISTINCT [column names] FROM [table];
```

**Remove duplicate records:**
```sql
SELECT DISTINCT * FROM [table];
```

**Example:**
```sql
/* Get unique street addresses and boroughs */
SELECT DISTINCT incident_address, borough
FROM hpd311calls;
```

### Aggregate Functions

**Purpose**: Query a database directly for descriptive statistics

**Available functions:**
- `SUM`
- `AVG`
- `MAX`
- `MIN`
- `COUNT`

#### SUM, AVG, MAX, MIN
Each takes a single column name:

```sql
SELECT AVG(tmax) FROM weather;
```

#### COUNT
**Get number of rows:**
```sql
SELECT COUNT(*) FROM [table_name];
```

**Get number of unique values in a column:**
```sql
SELECT COUNT(DISTINCT [column_names]) FROM [table_name];
```

### GROUP BY

**Purpose**: Aggregate functions calculate a single summary statistic by default. Summarize data by categories with GROUP BY statements.

> [!important] Remember
> Also select the column you're grouping by!

**Example:**
```sql
/* Get counts of plumbing calls by borough */
SELECT borough, COUNT(*)
FROM hpd311calls
WHERE complaint_type = 'PLUMBING'
GROUP BY borough;
```

### Counting by Groups Example

```python
# Create database engine
engine = create_engine("sqlite:///data.db")

# Write query to get plumbing call counts by borough
query = """SELECT borough, COUNT(*)
FROM hpd311calls
WHERE complaint_type = 'PLUMBING'
GROUP BY borough;"""

# Query database and create dataframe
plumbing_call_counts = pd.read_sql(query, engine)

print(plumbing_call_counts)
# borough       COUNT(*)
# 0  BRONX          2016
# 1  BROOKLYN       2702
# 2  MANHATTAN      1413
# 3  QUEENS          808
# 4  STATEN ISLAND   178
```

## Loading Multiple Tables with Joins

### Keys
**Definition**: Database records have unique identifiers, or **keys**

**Purpose**: Keys allow tables to be linked together

### Joining Tables

**Basic JOIN syntax:**
```sql
SELECT *
FROM hpd311calls
JOIN weather
ON hpd311calls.created_date = weather.date;
```

**Key points:**
- Use **dot notation** (`table.column`) when working with multiple tables
- **Default join** only returns records whose key values appear in both tables
- **Make sure join keys are the same data type** or nothing will match

### Joining and Filtering

```sql
/* Get only heat/hot water calls and join in weather data */
SELECT *
FROM hpd311calls
JOIN weather
ON hpd311calls.created_date = weather.date
WHERE hpd311calls.complaint_type = 'HEAT/HOT WATER';
```

### Joining and Aggregating

**Example: Get call counts by borough with census data**

```sql
/* Get call counts by borough
and join in population and housing counts */
SELECT hpd311calls.borough,
       COUNT(*),
       boro_census.total_population,
       boro_census.housing_units
FROM hpd311calls
JOIN boro_census
ON hpd311calls.borough = boro_census.borough
GROUP BY hpd311calls.borough;
```

**In pandas:**
```python
query = """SELECT hpd311calls.borough,
       COUNT(*),
       boro_census.total_population,
       boro_census.housing_units
FROM hpd311calls
JOIN boro_census
ON hpd311calls.borough = boro_census.borough
GROUP BY hpd311calls.borough;"""

call_counts = pd.read_sql(query, engine)
print(call_counts)
# borough       COUNT(*)  total_population  housing_units
# 0  BRONX         29874        1455846         524488
# 1  BROOKLYN      31722        2635121        1028383
# ...
```

## SQL Keyword Order

**Standard SQL order of keywords:**
1. `SELECT`
2. `FROM`
3. `JOIN`
4. `WHERE`
5. `GROUP BY`

> [!important] SQL Query Structure
> Always follow this order when writing SQL queries

## Key Concepts Summary

| Component | Purpose | Example |
|-----------|---------|---------|
| `create_engine()` | Connect to database | `create_engine("sqlite:///data.db")` |
| `pd.read_sql()` | Query database | `pd.read_sql(query, engine)` |
| `SELECT` | Choose columns | `SELECT col1, col2 FROM table` |
| `WHERE` | Filter records | `WHERE tmax > 32` |
| `AND` | All conditions must be true | `WHERE a = 1 AND b = 2` |
| `OR` | At least one condition true | `WHERE a = 1 OR b = 2` |
| `DISTINCT` | Get unique values | `SELECT DISTINCT borough` |
| `COUNT()` | Count rows | `SELECT COUNT(*) FROM table` |
| `GROUP BY` | Aggregate by category | `GROUP BY borough` |
| `JOIN` | Combine tables | `JOIN table2 ON key1 = key2` |

## Operators Reference

### Comparison Operators
| Operator | Meaning |
|----------|---------|
| `=` | Equal to |
| `>` | Greater than |
| `>=` | Greater than or equal to |
| `<` | Less than |
| `<=` | Less than or equal to |
| `<>` | Not equal to |

### Logical Operators
| Operator | Meaning |
|----------|---------|
| `AND` | All conditions must be true |
| `OR` | At least one condition must be true |

## Aggregate Functions Reference

| Function | Purpose | Example |
|----------|---------|---------|
| `SUM(column)` | Sum of values | `SELECT SUM(sales) FROM table` |
| `AVG(column)` | Average of values | `SELECT AVG(temperature) FROM weather` |
| `MAX(column)` | Maximum value | `SELECT MAX(price) FROM products` |
| `MIN(column)` | Minimum value | `SELECT MIN(age) FROM users` |
| `COUNT(*)` | Count all rows | `SELECT COUNT(*) FROM table` |
| `COUNT(DISTINCT col)` | Count unique values | `SELECT COUNT(DISTINCT city) FROM table` |

---

#datacamp #pandas #sql #databases #sqlalchemy #data-ingestion


## Section A: Multiple Choice Questions (30 questions × 2 points = 60 points)

### Topic 1: Database Basics and Connections (10 questions)

**Q1.** In relational databases, what does each row or record represent?
- [ ] A) An attribute of an entity
- [ ] B) A column header
- [ ] C) An instance of an entity
- [ ] D) A table name

> [!success]- Answer
> **Correct Answer: C) An instance of an entity**
> 
> Each row/record represents one instance of an entity (e.g., one person, one transaction, one weather observation).

**Q2.** What language is used to interact with databases?
- [ ] A) Python
- [ ] B) SQL (Structured Query Language)
- [ ] C) R
- [ ] D) HTML

> [!success]- Answer
> **Correct Answer: B) SQL (Structured Query Language)**
> 
> The chapter states: "SQL (Structured Query Language) to interact with databases"

**Q3.** What is a characteristic of SQLite databases?
- [ ] A) They require a server to run
- [ ] B) They are computer files
- [ ] C) They only work on Linux
- [ ] D) They can't be used with pandas

> [!success]- Answer
> **Correct Answer: B) They are computer files**
> 
> The chapter explicitly states: "SQLite databases are computer files"

**Q4.** What are the two steps in the database connection process?
- [ ] A) Download and upload data
- [ ] B) Create way to connect to database and query database
- [ ] C) Read and write data
- [ ] D) Open and close connection

> [!success]- Answer
> **Correct Answer: B) Create way to connect to database and query database**
> 
> The chapter outlines this as a "Two-step process"

**Q5.** Which function creates an engine to handle database connections?
- [ ] A) pd.create_engine()
- [ ] B) sqlalchemy.connect()
- [ ] C) create_engine()
- [ ] D) engine_create()

> [!success]- Answer
> **Correct Answer: C) create_engine()**
> 
> The function is `sqlalchemy`'s `create_engine()`

**Q6.** What is the SQLite URL format?
- [ ] A) sqlite://filename.db
- [ ] B) sqlite:///filename.db
- [ ] C) sqlite:/filename.db
- [ ] D) db://sqlite/filename

> [!success]- Answer
> **Correct Answer: B) sqlite:///filename.db**
> 
> Note the three slashes: `sqlite:///filename.db`

**Q7.** What function is used to query databases in pandas?
- [ ] A) pd.query_sql()
- [ ] B) pd.sql_read()
- [ ] C) pd.read_sql()
- [ ] D) pd.database_query()

> [!success]- Answer
> **Correct Answer: C) pd.read_sql()**
> 
> This is the pandas function for querying databases

**Q8.** What are the two arguments for `pd.read_sql()`?
- [ ] A) filename and engine
- [ ] B) query and engine
- [ ] C) database and table
- [ ] D) sql and connection

> [!success]- Answer
> **Correct Answer: B) query and engine**
> 
> `pd.read_sql(query, engine)` - query is the SQL string or table name, engine is the connection object

**Q9.** What can you pass as the `query` argument in `pd.read_sql()`?
- [ ] A) Only SQL queries
- [ ] B) Only table names
- [ ] C) String containing SQL query to run or table to load
- [ ] D) Only SELECT statements

> [!success]- Answer
> **Correct Answer: C) String containing SQL query to run or table to load**
> 
> You can pass either a SQL query or a table name

**Q10.** Which of the following is NOT an advantage of relational databases mentioned in the chapter?
- [ ] A) Support more data
- [ ] B) Multiple simultaneous users
- [ ] C) Faster than flat files
- [ ] D) Data quality controls

> [!success]- Answer
> **Correct Answer: C) Faster than flat files**
> 
> The advantages listed are: support more data, multiple simultaneous users, and data quality controls. Speed comparison wasn't mentioned.

### Topic 2: SQL SELECT and WHERE Clauses (10 questions)

**Q11.** What SQL keyword is used to query data from a database?
- [ ] A) GET
- [ ] B) QUERY
- [ ] C) SELECT
- [ ] D) FETCH

> [!success]- Answer
> **Correct Answer: C) SELECT**
> 
> SELECT is the fundamental SQL keyword for querying data

**Q12.** What does this SQL query do: `SELECT * FROM weather;`?
- [ ] A) Selects only the first row
- [ ] B) Gets all data in the weather table
- [ ] C) Deletes the weather table
- [ ] D) Creates a new table

> [!success]- Answer
> **Correct Answer: B) Gets all data in the weather table**
> 
> The `*` means "all columns", so this retrieves everything from the table

**Q13.** According to SQL code style shown in the chapter, how should keywords be written?
- [ ] A) In lowercase
- [ ] B) In ALL CAPS
- [ ] C) In camelCase
- [ ] D) Mixed case

> [!success]- Answer
> **Correct Answer: B) In ALL CAPS**
> 
> Code style: "keywords in ALL CAPS"

**Q14.** What punctuation marks the end of a SQL statement?
- [ ] A) Period (.)
- [ ] B) Comma (,)
- [ ] C) Semicolon (;)
- [ ] D) Colon (:)

> [!success]- Answer
> **Correct Answer: C) Semicolon (;)**
> 
> Code style: "semicolon (;) to end a statement"

**Q15.** What is the purpose of a WHERE clause?
- [ ] A) To create new columns
- [ ] B) To selectively import records
- [ ] C) To delete data
- [ ] D) To join tables

> [!success]- Answer
> **Correct Answer: B) To selectively import records**
> 
> WHERE clauses filter which records are returned based on conditions

**Q16.** Which operator means "not equal to" in SQL?
- [ ] A) !=
- [ ] B) <>
- [ ] C) NOT
- [ ] D) ~=

> [!success]- Answer
> **Correct Answer: B) <>**
> 
> SQL uses `<>` for "not equal to"

**Q17.** Is string matching in SQL WHERE clauses case-sensitive?
- [ ] A) No, it's case-insensitive
- [ ] B) Yes, it's case-sensitive
- [ ] C) Only for numeric values
- [ ] D) Depends on the database

> [!success]- Answer
> **Correct Answer: B) Yes, it's case-sensitive**
> 
> The chapter emphasizes: "String matching is case-sensitive"

**Q18.** What does `WHERE borough = 'BROOKLYN'` filter for?
- [ ] A) Any borough containing "BROOKLYN"
- [ ] B) Exactly "BROOKLYN" (case-sensitive)
- [ ] C) "Brooklyn" or "BROOKLYN"
- [ ] D) Any text starting with "B"

> [!success]- Answer
> **Correct Answer: B) Exactly "BROOKLYN" (case-sensitive)**
> 
> The `=` matches exact strings and is case-sensitive

**Q19.** What does AND do when combining conditions in WHERE clauses?
- [ ] A) Returns records that meet at least one condition
- [ ] B) Returns records that meet all conditions
- [ ] C) Returns records that meet none of the conditions
- [ ] D) Averages the results

> [!success]- Answer
> **Correct Answer: B) Returns records that meet all conditions**
> 
> The chapter states: "WHERE clauses with AND return records that meet all conditions"

**Q20.** What does OR do when combining conditions in WHERE clauses?
- [ ] A) Returns records that meet all conditions
- [ ] B) Returns records that meet at least one condition
- [ ] C) Returns records that meet none of the conditions
- [ ] D) Orders the results

> [!success]- Answer
> **Correct Answer: B) Returns records that meet at least one condition**
> 
> The chapter states: "WHERE clauses with OR return records that meet at least one condition"

### Topic 3: Advanced SQL - DISTINCT, Aggregates, and JOINs (10 questions)

**Q21.** What does SELECT DISTINCT do?
- [ ] A) Selects the first record
- [ ] B) Gets unique values for one or more columns
- [ ] C) Counts all records
- [ ] D) Deletes duplicate records

> [!success]- Answer
> **Correct Answer: B) Gets unique values for one or more columns**
> 
> DISTINCT returns only unique values, removing duplicates

**Q22.** Which of the following is NOT an aggregate function mentioned in the chapter?
- [ ] A) SUM
- [ ] B) AVG
- [ ] C) MEDIAN
- [ ] D) COUNT

> [!success]- Answer
> **Correct Answer: C) MEDIAN**
> 
> The aggregate functions listed are: SUM, AVG, MAX, MIN, COUNT. MEDIAN was not mentioned.

**Q23.** What does `SELECT COUNT(*) FROM table_name;` do?
- [ ] A) Counts the number of columns
- [ ] B) Counts unique values
- [ ] C) Gets the number of rows in the table
- [ ] D) Counts NULL values

> [!success]- Answer
> **Correct Answer: C) Gets the number of rows in the table**
> 
> `COUNT(*)` counts all rows that meet the query conditions

**Q24.** How do you count the number of unique values in a column?
- [ ] A) COUNT(column_name)
- [ ] B) UNIQUE(column_name)
- [ ] C) COUNT DISTINCT column_name
- [ ] D) COUNT(DISTINCT column_name)

> [!success]- Answer
> **Correct Answer: D) COUNT(DISTINCT column_name)**
> 
> Syntax: `SELECT COUNT(DISTINCT [column_names]) FROM [table_name];`

**Q25.** What is the default behavior of aggregate functions?
- [ ] A) Calculate statistics for each row
- [ ] B) Calculate a single summary statistic
- [ ] C) Group by all columns
- [ ] D) Return NULL

> [!success]- Answer
> **Correct Answer: B) Calculate a single summary statistic**
> 
> The chapter states: "Aggregate functions calculate a single summary statistic by default"

**Q26.** What does GROUP BY do?
- [ ] A) Sorts the data
- [ ] B) Summarizes data by categories
- [ ] C) Joins tables together
- [ ] D) Filters rows

> [!success]- Answer
> **Correct Answer: B) Summarizes data by categories**
> 
> GROUP BY allows you to "Summarize data by categories with GROUP BY statements"

**Q27.** When using GROUP BY, what must you remember to do?
- [ ] A) Use only aggregate functions
- [ ] B) Also select the column you're grouping by
- [ ] C) Include a WHERE clause
- [ ] D) Use DISTINCT

> [!success]- Answer
> **Correct Answer: B) Also select the column you're grouping by**
> 
> The chapter emphasizes: "Remember to also select the column you're grouping by!"

**Q28.** What notation is used when working with multiple tables in SQL?
- [ ] A) Bracket notation [table][column]
- [ ] B) Dot notation table.column
- [ ] C) Underscore notation table_column
- [ ] D) Arrow notation table->column

> [!success]- Answer
> **Correct Answer: B) Dot notation table.column**
> 
> "Use dot notation (table.column) when working with multiple tables"

**Q29.** What does a default JOIN return?
- [ ] A) All records from both tables
- [ ] B) Only records whose key values appear in both tables
- [ ] C) Only records from the first table
- [ ] D) All possible combinations

> [!success]- Answer
> **Correct Answer: B) Only records whose key values appear in both tables**
> 
> "Default join only returns records whose key values appear in both tables"

**Q30.** What is critical when joining tables?
- [ ] A) Tables must have the same number of rows
- [ ] B) Join keys must be the same data type or nothing will match
- [ ] C) Tables must be in alphabetical order
- [ ] D) Both tables must have the same columns

> [!success]- Answer
> **Correct Answer: B) Join keys must be the same data type or nothing will match**
> 
> The chapter warns: "Make sure join keys are the same data type or nothing will match"

---

## Section B: True/False Questions (15 questions × 1 point = 15 points)

**Q31.** In relational databases, each column contains information about an attribute. **T/F**

> [!success]- Answer
> **True** - Each column represents an attribute (characteristic) of the entity.

**Q32.** SQLite databases require a separate server to run. **T/F**

> [!success]- Answer
> **False** - SQLite databases are computer files, not requiring a separate server.

**Q33.** The `create_engine()` function is part of pandas. **T/F**

> [!success]- Answer
> **False** - It's part of sqlalchemy, not pandas.

**Q34.** You can pass either a SQL query or a table name to `pd.read_sql()`. **T/F**

> [!success]- Answer
> **True** - The query argument accepts either "String containing SQL query to run or table to load"

**Q35.** SQL keywords should be written in lowercase according to the style guide in the chapter. **T/F**

> [!success]- Answer
> **False** - Keywords should be in ALL CAPS according to the code style shown.

**Q36.** The asterisk (*) in `SELECT * FROM table` means "all columns". **T/F**

> [!success]- Answer
> **True** - The `*` is a wildcard that selects all columns.

**Q37.** String matching in SQL WHERE clauses is case-insensitive. **T/F**

> [!success]- Answer
> **False** - The chapter explicitly states: "String matching is case-sensitive"

**Q38.** The `<>` operator in SQL means "greater than or less than". **T/F**

> [!success]- Answer
> **False** - `<>` means "not equal to"

**Q39.** WHERE clauses with OR require all conditions to be true. **T/F**

> [!success]- Answer
> **False** - OR returns records that meet at least one condition, not all conditions (that's AND).

**Q40.** SELECT DISTINCT can be used to remove duplicate records. **T/F**

> [!success]- Answer
> **True** - `SELECT DISTINCT * FROM [table]` removes duplicate records.

**Q41.** Aggregate functions automatically group data by categories without using GROUP BY. **T/F**

> [!success]- Answer
> **False** - By default, aggregate functions calculate a single summary statistic. You need GROUP BY to summarize by categories.

**Q42.** When using GROUP BY, you don't need to select the column you're grouping by. **T/F**

> [!success]- Answer
> **False** - The chapter emphasizes: "Remember to also select the column you're grouping by!"

**Q43.** A default JOIN returns all records from both tables regardless of matches. **T/F**

> [!success]- Answer
> **False** - Default join only returns records whose key values appear in both tables.

**Q44.** Join keys must be the same data type for the join to work properly. **T/F**

> [!success]- Answer
> **True** - "Make sure join keys are the same data type or nothing will match"

**Q45.** The correct SQL keyword order is: SELECT, FROM, JOIN, WHERE, GROUP BY. **T/F**

> [!success]- Answer
> **True** - This is the standard SQL order of keywords shown in the chapter.

---

## Section C: Short Answer Questions (8 questions × 3 points = 24 points)

**Q46.** List four advantages of relational databases mentioned in the chapter.

> [!success]- Answer
> The four advantages of relational databases are:
> 1. **Support more data** - Can handle larger datasets
> 2. **Multiple simultaneous users** - Allow concurrent access
> 3. **Data quality controls** - Built-in validation and constraints
> 4. **Data types are specified for each column** - Enforces data type consistency
> 
> Additionally, tables can be linked via unique keys for relational structure.

**Q47.** Explain the two-step process for connecting to and querying databases.

> [!success]- Answer
> The two-step process is:
> 
> **Step 1: Create way to connect to database**
> - Use `create_engine()` from sqlalchemy
> - Provide database URL (e.g., `sqlite:///filename.db`)
> - Creates an engine object to manage connections
> 
> **Step 2: Query database**
> - Use `pd.read_sql(query, engine)`
> - Pass SQL query or table name
> - Pass the engine object created in step 1
> - Returns a pandas DataFrame

**Q48.** What are the five aggregate functions mentioned in the chapter and what do they do?

> [!success]- Answer
> The five aggregate functions are:
> 
> 1. **SUM** - Calculates the sum of values in a column
> 2. **AVG** - Calculates the average of values in a column
> 3. **MAX** - Finds the maximum value in a column
> 4. **MIN** - Finds the minimum value in a column
> 5. **COUNT** - Counts the number of rows (or unique values with DISTINCT)
> 
> SUM, AVG, MAX, MIN each take a single column name. COUNT can use `*` to count all rows or `DISTINCT column` to count unique values.

**Q49.** Explain the difference between AND and OR when combining conditions in WHERE clauses.

> [!success]- Answer
> **AND:**
> - Returns records that meet **all** conditions
> - All conditions must be true for a record to be included
> - More restrictive (fewer results)
> - Example: `WHERE borough = 'BRONX' AND complaint_type = 'PLUMBING'`
>   - Only returns records that are BOTH in Bronx AND about plumbing
> 
> **OR:**
> - Returns records that meet **at least one** condition
> - Any condition being true includes the record
> - Less restrictive (more results)
> - Example: `WHERE complaint_type = 'WATER LEAK' OR complaint_type = 'PLUMBING'`
>   - Returns records about water leaks OR plumbing (or both)

**Q50.** What is the purpose of GROUP BY and what must you remember when using it?

> [!success]- Answer
> **Purpose of GROUP BY:**
> - Aggregate functions calculate a single summary statistic by default
> - GROUP BY allows you to **summarize data by categories**
> - Breaks down aggregate calculations by distinct values in specified column(s)
> 
> **What to remember:**
> - **Always select the column you're grouping by** in your SELECT statement
> - Without GROUP BY, aggregates return one value for the entire table
> - With GROUP BY, aggregates return one value per category
> 
> Example:
> ```sql
> SELECT borough, COUNT(*)  -- Must select borough!
> FROM hpd311calls
> GROUP BY borough;  -- Grouping by borough
> ```

**Q51.** Explain what dot notation is and when you must use it in SQL.

> [!success]- Answer
> **Dot notation** is the format `table.column` used to specify which table a column belongs to.
> 
> **When to use it:**
> - **When working with multiple tables** (especially with JOINs)
> - Prevents ambiguity when multiple tables have columns with the same name
> - Required in JOIN conditions to specify which table each key comes from
> 
> **Examples:**
> ```sql
> -- In JOIN condition
> ON hpd311calls.created_date = weather.date
> 
> -- In SELECT with multiple tables
> SELECT hpd311calls.borough, boro_census.total_population
> 
> -- In WHERE with multiple tables
> WHERE hpd311calls.complaint_type = 'PLUMBING'
> ```

**Q52.** List the five SQL keywords in their correct order as shown in the chapter.

> [!success]- Answer
> The standard SQL order of keywords is:
> 
> 1. **SELECT** - Choose which columns to return
> 2. **FROM** - Specify which table(s) to query
> 3. **JOIN** - Combine multiple tables (if needed)
> 4. **WHERE** - Filter which records to include
> 5. **GROUP BY** - Aggregate by categories (if using aggregate functions)
> 
> This order must be followed for SQL queries to execute properly. You don't need all keywords in every query, but those you use must appear in this order.

**Q53.** Explain what keys are in databases and what critical requirement must be met when joining tables.

> [!success]- Answer
> **Keys:**
> - Database records have unique identifiers called **keys**
> - Allow tables to be linked together
> - Enable relational structure between different tables
> 
> **Critical requirement when joining:**
> - **Join keys must be the same data type or nothing will match**
> - If one key is a string and the other is a number, the join will fail
> - Data types must match exactly for the join condition to work
> - The default join only returns records whose key values appear in both tables
> 
> Example: If `created_date` is a date type in one table and a string in another, they won't match even if the values look the same.

---

## Section D: Code Analysis & Scenarios (5 questions × 2 points = 10 points)

**Q54.** What does this code do?
```python
from sqlalchemy import create_engine
engine = create_engine("sqlite:///weather_data.db")
weather = pd.read_sql("weather", engine)
```

> [!success]- Answer
> This code:
> 1. **Imports** the `create_engine` function from sqlalchemy
> 2. **Creates a database engine** that connects to the SQLite database file "weather_data.db"
> 3. **Loads the entire "weather" table** from the database into a pandas DataFrame
> 
> Note: `pd.read_sql("weather", engine)` passes a table name (not a SQL query), which loads all data from that table - equivalent to `SELECT * FROM weather`.

**Q55.** Write the SQL query to get only the `borough` and `complaint_type` columns from the `hpd311calls` table where the borough is 'MANHATTAN'.

> [!success]- Answer
> ```sql
> SELECT borough, complaint_type
> FROM hpd311calls
> WHERE borough = 'MANHATTAN';
> ```
> 
> **Explanation:**
> - `SELECT borough, complaint_type` - specifies exactly which columns to return
> - `FROM hpd311calls` - specifies the table
> - `WHERE borough = 'MANHATTAN'` - filters for only Manhattan records (case-sensitive exact match)

**Q56.** What will this query return?
```sql
SELECT COUNT(DISTINCT borough)
FROM hpd311calls;
```

> [!success]- Answer
> **Returns:** The number of unique boroughs in the hpd311calls table (a single number).
> 
> **Explanation:**
> - `DISTINCT borough` gets unique borough values
> - `COUNT()` counts how many unique values there are
> - Results in one row with one column containing the count of distinct boroughs
> 
> For NYC data, this would likely return 5 (Bronx, Brooklyn, Manhattan, Queens, Staten Island).

**Q57.** Identify the issue in this SQL query:
```sql
SELECT AVG(temperature)
FROM weather
GROUP BY station;
```

> [!success]- Answer
> **Issue:** The query is grouping by `station` but not selecting the `station` column.
> 
> **Problem:** While this query will technically run, you won't know which average temperature belongs to which station because `station` isn't in the SELECT clause.
> 
> **Fix:**
> ```sql
> SELECT station, AVG(temperature)
> FROM weather
> GROUP BY station;
> ```
> 
> The chapter emphasizes: "Remember to also select the column you're grouping by!"

**Q58.** Complete this SQL query to join the `hpd311calls` and `boro_census` tables on the `borough` column and get all columns:
```sql
SELECT *
FROM hpd311calls
_____ boro_census
_____ hpd311calls.borough = boro_census.borough;
```

> [!success]- Answer
> ```sql
> SELECT *
> FROM hpd311calls
> JOIN boro_census
> ON hpd311calls.borough = boro_census.borough;
> ```
> 
> **Explanation:**
> - `JOIN boro_census` - specifies which table to join with
> - `ON hpd311calls.borough = boro_census.borough` - defines the join condition
> - Uses dot notation to specify which table each `borough` column comes from
> - This will return only records where the borough exists in both tables (default inner join)

---

## Answer Key & Scoring

### Section A: Multiple Choice (60 points)
1. C  |  2. B  |  3. B  |  4. B  |  5. C  
2. B  |  7. C  |  8. B  |  9. C  |  10. C  
3. C  |  12. B  |  13. B  |  14. C  |  15. B  
4. B  |  17. B  |  18. B  |  19. B  |  20. B  
5. B  |  22. C  |  23. C  |  24. D  |  25. B  
6. B  |  27. B  |  28. B  |  29. B  |  30. B  

### Section B: True/False (15 points)
31. T  |  32. F  |  33. F  |  34. T  |  35. F  
32. T  |  37. F  |  38. F  |  39. F  |  40. T  
33. F  |  42. F  |  43. F  |  44. T  |  45. T  

### Section C: Short Answer (24 points)
- Q46-Q53: See detailed answers in collapsible sections (3 points each)

### Section D: Code Analysis (10 points)
- Q54-Q58: See detailed answers in collapsible sections (2 points each)

---

## Quick Reference Guide

### Database Connection Pattern
```python
from sqlalchemy import create_engine
import pandas as pd

# Step 1: Create engine
engine = create_engine("sqlite:///database.db")

# Step 2: Query database
df = pd.read_sql(query, engine)
# OR
df = pd.read_sql("table_name", engine)
```

### Essential SQL Patterns

```sql
-- Basic SELECT
SELECT column1, column2 FROM table_name;
SELECT * FROM table_name;

-- WHERE with conditions
SELECT * FROM table WHERE column > 100;
SELECT * FROM table WHERE city = 'NYC';  -- Case-sensitive!

-- Combining conditions
SELECT * FROM table WHERE condition1 AND condition2;
SELECT * FROM table WHERE condition1 OR condition2;

-- DISTINCT
SELECT DISTINCT column FROM table;

-- Aggregate functions
SELECT COUNT(*) FROM table;
SELECT AVG(column) FROM table;
SELECT SUM(column) FROM table;
SELECT MAX(column) FROM table;
SELECT MIN(column) FROM table;
SELECT COUNT(DISTINCT column) FROM table;

-- GROUP BY
SELECT category, COUNT(*)
FROM table
GROUP BY category;

-- JOIN
SELECT *
FROM table1
JOIN table2
ON table1.key = table2.key;

-- Complete query
SELECT table1.column1, table2.column2, COUNT(*)
FROM table1
JOIN table2
ON table1.key = table2.key
WHERE table1.value > 100
GROUP BY table1.column1, table2.column2;
```

---

## Key Concepts Summary

| Concept | Key Points |
|---------|-----------|
| **Relational Databases** | Tables, rows=entities, columns=attributes, linked by keys |
| **SQLite** | File-based database system |
| **create_engine()** | From sqlalchemy; creates connection engine |
| **URL Format** | `sqlite:///filename.db` (three slashes!) |
| **pd.read_sql()** | Takes query/table name and engine |
| **SELECT** | Query data; `*` means all columns |
| **WHERE** | Filter records; string matching is case-sensitive |
| **AND/OR** | AND=all conditions; OR=at least one |
| **DISTINCT** | Get unique values |
| **Aggregates** | SUM, AVG, MAX, MIN, COUNT |
| **GROUP BY** | Summarize by categories; must select grouped column |
| **JOIN** | Combine tables; use dot notation; keys must match types |
| **SQL Order** | SELECT, FROM, JOIN, WHERE, GROUP BY |

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal to |
| `>` | Greater than |
| `>=` | Greater than or equal |
| `<` | Less than |
| `<=` | Less than or equal |
| `<>` | Not equal to |

---

## Common Mistakes to Avoid

❌ Forgetting three slashes in SQLite URL: `sqlite:///` not `sqlite://`  
❌ Not importing both pandas and create_engine from sqlalchemy  
❌ Using lowercase for SQL keywords instead of ALL CAPS  
❌ Forgetting semicolon at end of SQL statements  
❌ Assuming string matching is case-insensitive (it's case-sensitive!)  
❌ Confusing `<>` (not equal) with other operators  
❌ Using AND when you mean OR, or vice versa  
❌ Forgetting to select the column you're grouping by  
❌ Not using dot notation when working with multiple tables  
❌ Joining tables with different data types for keys  
❌ Writing SQL keywords in wrong order  
❌ Using `COUNT(column)` when you mean `COUNT(DISTINCT column)`  

---

## Study Strategy (3-Day Plan)

### Day 1: Database Basics and SELECT
- Review relational database concepts
- Practice creating database engines with `create_engine()`
- Work with `pd.read_sql()` using table names
- Write basic SELECT queries
- Practice WHERE clauses with numbers and text
- Remember: string matching is case-sensitive!

### Day 2: Filtering and Aggregating
- Practice AND vs OR conditions
- Work with comparison operators
- Use SELECT DISTINCT for unique values
- Practice all five aggregate functions (SUM, AVG, MAX, MIN, COUNT)
- Master GROUP BY with aggregates
- Remember to select grouped columns!

### Day 3: JOINs and Complex Queries
- Understand keys in databases
- Practice JOIN syntax with dot notation
- Combine JOINs with WHERE clauses
- Use JOINs with GROUP BY and aggregates
- Review SQL keyword order
- Ensure join keys have matching data types
- Complete practice quiz

---

## Pre-Exam Checklist

✅ Know the two-step connection process  
✅ Remember SQLite URL format: `sqlite:///filename.db`  
✅ Understand `pd.read_sql(query, engine)` arguments  
✅ Know SQL keywords should be ALL CAPS  
✅ Remember string matching is case-sensitive  
✅ Understand AND (all) vs OR (at least one)  
✅ Know all comparison operators, especially `<>` for not equal  
✅ Memorize five aggregate functions: SUM, AVG, MAX, MIN, COUNT  
✅ Always select columns you're grouping by  
✅ Use dot notation with multiple tables  
✅ Remember join keys must be same data type  
✅ Know SQL keyword order: SELECT, FROM, JOIN, WHERE, GROUP BY  
✅ Understand `COUNT(*)` vs `COUNT(DISTINCT column)`  

---

## During the Exam

💡 **SQLite URL** - Three slashes: `sqlite:///`  
💡 **Case sensitivity** - String matching IS case-sensitive  
💡 **AND vs OR** - AND is more restrictive (all conditions), OR is less restrictive  
💡 **Aggregate defaults** - Calculate single statistic unless using GROUP BY  
💡 **GROUP BY rule** - Must select the grouped column!  
💡 **Dot notation** - Required when working with multiple tables  
💡 **JOIN requirement** - Keys must be same data type  
💡 **SQL order** - SELECT, FROM, JOIN, WHERE, GROUP BY (in that order!)  
💡 **COUNT variations** - `COUNT(*)` vs `COUNT(DISTINCT col)` are different  

---

#datacamp #pandas #sql #databases #sqlalchemy #data-ingestion #certification #exam-prep