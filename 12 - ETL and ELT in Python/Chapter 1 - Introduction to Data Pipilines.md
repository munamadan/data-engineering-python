## Data Pipelines

### Definition
**Data pipelines** are responsible for moving data from a source to a destination, and transforming it somewhere along the way.

### Key Components
1. **Source** - Where data originates
2. **Transformation** - Processing/cleaning data
3. **Destination** - Where data ends up

---

## ETL vs ELT

### ETL: Extract, Transform, Load

| Aspect | Description |
|--------|-------------|
| **Pattern** | Traditional data pipeline design pattern |
| **Order** | Extract → Transform → Load |
| **Sources** | May be tabular or non-tabular |
| **Tools** | Leverage Python with pandas |
| **Where Transform Happens** | Outside the destination (using Python/pandas) |

### ELT: Extract, Load, Transform

| Aspect | Description |
|--------|-------------|
| **Pattern** | More recent pattern |
| **Order** | Extract → Load → Transform |
| **Destination** | Data warehouses |
| **Sources** | Typically tabular data |
| **Where Transform Happens** | Inside the data warehouse (using SQL) |

---

## ETL Pipeline Example

### Complete ETL Workflow

```python
def extract(file_name):
    """Extract data from a CSV file"""
    # Read data
    data = pd.read_csv(file_name)
    print(f"Extracting data from {file_name}")
    return data

def transform(data_frame):
    """Transform data to remove null records"""
    # Apply transformations using pandas
    cleaned_data = data_frame.dropna()
    print("Transforming data to remove 'null' records")
    return cleaned_data

def load(data_frame, target_table):
    """Load data to SQL database"""
    # Some custom-built Python logic to load data to SQL
    data_frame.to_sql(name=target_table, con=POSTGRES_CONNECTION)
    print(f"Loading data to the {target_table} table")

# Now, run the data pipeline
extracted_data = extract(file_name="raw_data.csv")
transformed_data = transform(data_frame=extracted_data)
load(data_frame=transformed_data, target_table="cleaned_data")
```

**Output:**
```
Extracting data from raw_data.csv
Transforming data to remove 'null' records
Loading data to the cleaned_data table
```

### Key Points
- Data is **transformed in Python** (using pandas)
- Transformation happens **before** loading to destination
- Uses pandas DataFrame operations

---

## ELT Pipeline Example

### Complete ELT Workflow

```python
def extract(file_name):
    """Extract data from a CSV file"""
    data = pd.read_csv(file_name)
    return data

def load(data_frame, table_name):
    """Load raw data to data warehouse"""
    # Load data to warehouse without transformation
    data_frame.to_sql(name=table_name, con=DATA_WAREHOUSE_CONNECTION)

def transform(source_table, target_table):
    """Transform data using SQL in the data warehouse"""
    data_warehouse.run_sql("""
        CREATE TABLE {target_table} AS
        SELECT
            <field-name>, <field-name>, ...
        FROM {source_table};
    """)

# Similar to ETL pipelines, call the extract, load, and transform functions
extracted_data = extract(file_name="raw_data.csv")
load(data_frame=extracted_data, table_name="raw_data")
transform(source_table="raw_data", target_table="cleaned_data")
```

### Key Points
- Data is **loaded first** to the data warehouse
- Transformation happens **inside** the data warehouse using SQL
- Raw data is stored in the warehouse

---

## Extracting Data from CSV Files

### Using `pd.read_csv()`

```python
import pandas as pd

# Read in the CSV file to a DataFrame
data_frame = pd.read_csv("raw_data.csv")

# Output the first few rows
data_frame.head()
```

**Output:**
```
   name           num_firms  total_income
0  Advertising    58         3892.41
1  Apparel        39         5422.69
...
49 Trucking       35         17324.36
```

### `read_csv()` Function

| Parameter | Purpose |
|-----------|---------|
| **File path** | Path to CSV file (required) |
| `delimiter` | Specify delimiter (default: comma) |
| `header` | Row to use as column names |
| `engine` | Parser engine to use |

**Returns:** DataFrame

### `.head()` Method

**Purpose:** Outputs the first n rows of a DataFrame

```python
data_frame.head()    # First 5 rows (default)
data_frame.head(10)  # First 10 rows
```

---

## Filtering DataFrames

### Using `.loc[]`

The `.loc[]` accessor filters DataFrames by **rows** and **columns**.

**Syntax:** `data_frame.loc[row_filter, column_filter]`

- `:` means "all"

### Filtering by Rows

```python
# Filter rows where name equals "Apparel"
data_frame.loc[data_frame["name"] == "Apparel", :]
```

**Output:**
```
   name     num_firms
1  Apparel  39
37 Apparel  61
```

### Filtering by Columns

```python
# Select only "name" and "num_firms" columns
data_frame.loc[:, ["name", "num_firms"]]
```

**Output:**
```
   name           num_firms
0  Advertising    58
1  Apparel        39
...
```

### Combining Row and Column Filters

```python
# Filter rows AND columns
data_frame.loc[data_frame["name"] == "Apparel", ["name", "num_firms"]]
```

**Output:**
```
   name     num_firms
1  Apparel  39
37 Apparel  61
```

---

## Writing DataFrames to Files

### Using `.to_csv()`

```python
# Write a DataFrame to a .csv file
data_frame.to_csv("cleaned_data.csv")
```

### Parameters

| Parameter | Purpose | Example |
|-----------|---------|---------|
| **Path** | File path to save to | `"output.csv"` |
| `index` | Include row index | `index=False` |
| `sep` | Column separator | `sep='\t'` |
| `header` | Include column names | `header=True` |

### Other Output Methods

```python
# Write to different formats
data_frame.to_json("data.json")    # JSON format
data_frame.to_excel("data.xlsx")   # Excel format
data_frame.to_sql("table_name", con=connection)  # SQL database
```

---

## Running SQL Queries

### Using `.execute()` Method

```python
# Use Python clients or other tools to run SQL queries
data_warehouse.execute("""
    CREATE TABLE total_sales AS
    SELECT
        ds,
        SUM(sales) AS total_sales
    FROM raw_sales_data
    GROUP BY ds;
""")
```

### Purpose
- Execute SQL commands in a data warehouse
- Common in ELT pipelines for transformation
- Tools like `.execute()` or `.run_sql()` are used

---

## Complete Pipeline Example

### ETL Pipeline Implementation

```python
# Define extract function
def extract(file_name):
    """Extract data from CSV file"""
    return pd.read_csv(file_name)

# Define transform function
def transform(data_frame, value):
    """Filter DataFrame by value and select columns"""
    return data_frame.loc[
        data_frame["name"] == value, 
        ["name", "num_firms"]
    ]

# Define load function
def load(data_frame, file_name):
    """Load DataFrame to CSV file"""
    data_frame.to_csv(file_name)
    print(f"Data loaded to {file_name}")

# Execute the pipeline
# First, extract data from a .csv
extracted_data = extract(file_name="raw_data.csv")

# Then, transform the `extracted_data`
transformed_data = transform(data_frame=extracted_data, value="Apparel")

# Finally, load the `transformed_data`
load(data_frame=transformed_data, file_name="cleaned_data.csv")
```

---

## Quick Reference

### ETL vs ELT Summary

| Feature | ETL | ELT |
|---------|-----|-----|
| **Transform Location** | Outside destination (Python) | Inside destination (SQL) |
| **Pattern Age** | Traditional | More recent |
| **Best For** | Non-tabular data, complex Python logic | Tabular data, SQL-friendly transformations |
| **Tools** | pandas, Python libraries | SQL, Data warehouses |
| **Data Storage** | Transformed data loaded | Raw data loaded first |

### Key pandas Functions

| Function | Purpose | Returns |
|----------|---------|---------|
| `pd.read_csv(path)` | Read CSV file | DataFrame |
| `.head(n)` | View first n rows | DataFrame |
| `.loc[rows, cols]` | Filter rows/columns | DataFrame |
| `.to_csv(path)` | Write to CSV | None |
| `.to_sql(name, con)` | Write to SQL | None |

### Pipeline Execution Order

**ETL:**
1. Extract (from source)
2. Transform (using pandas/Python)
3. Load (to destination)

**ELT:**
1. Extract (from source)
2. Load (to data warehouse)
3. Transform (using SQL in warehouse)

---

## Key Concepts

### Data Pipeline Components
- **Extract:** Reading data from source
- **Transform:** Cleaning, filtering, aggregating data
- **Load:** Writing data to destination

### When to Use ETL
- Complex transformations requiring Python logic
- Non-tabular data sources
- Need to transform before storing

### When to Use ELT
- Working with data warehouses
- Tabular data sources
- SQL-based transformations
- Want to keep raw data in warehouse

---

#etl #elt #data-pipelines #pandas #python #data-engineering #csv #sql #dataframes #datacamp #chapter1

# Chapter 1 Practice Quiz

## Introduction to ETL and ELT Pipelines

**Total Points: 100** **Pass Threshold: 70/100 (70%)**

---

## Section A: Multiple Choice Questions (2 points each = 40 points)

### Data Pipeline Basics

**Q1.** What are data pipelines responsible for?

- [ ] A) Only storing data
- [ ] B) Moving data from a source to a destination and transforming it along the way
- [ ] C) Creating databases
- [ ] D) Only transforming data

> [!success]- Answer **Correct Answer: B) Moving data from a source to a destination and transforming it along the way**
> 
> Data pipelines handle the complete flow: extracting from sources, transforming the data, and loading it to destinations.

**Q2.** What does ETL stand for?

- [ ] A) Extract, Transfer, Load
- [ ] B) Extract, Transform, Load
- [ ] C) Execute, Transform, Load
- [ ] D) Extract, Transform, Link

> [!success]- Answer **Correct Answer: B) Extract, Transform, Load**
> 
> ETL is the traditional data pipeline pattern where data is extracted, transformed (using Python/pandas), and then loaded to the destination.

**Q3.** What is the main difference between ETL and ELT?

- [ ] A) ETL is newer than ELT
- [ ] B) ETL transforms before loading; ELT loads before transforming
- [ ] C) They are the same thing
- [ ] D) ETL uses SQL, ELT uses Python

> [!success]- Answer **Correct Answer: B) ETL transforms before loading; ELT loads before transforming**
> 
> The key difference is the order: ETL transforms data before loading (using Python/pandas), while ELT loads raw data first and transforms it in the data warehouse (using SQL).

**Q4.** In an ETL pipeline, where does the transformation typically occur?

- [ ] A) In the data warehouse using SQL
- [ ] B) Outside the destination using Python/pandas
- [ ] C) In the source database
- [ ] D) Transformations don't occur in ETL

> [!success]- Answer **Correct Answer: B) Outside the destination using Python/pandas**
> 
> In ETL, data is transformed before being loaded to the destination, typically using Python tools like pandas.

**Q5.** Which pattern is considered more recent?

- [ ] A) ETL
- [ ] B) ELT
- [ ] C) Both are equally old
- [ ] D) Neither is new

> [!success]- Answer **Correct Answer: B) ELT**
> 
> ELT is the more recent pattern, designed to leverage the power of modern data warehouses for transformation.

### pandas and Data Extraction

**Q6.** What does `pd.read_csv()` return?

- [ ] A) A list
- [ ] B) A dictionary
- [ ] C) A DataFrame
- [ ] D) A string

> [!success]- Answer **Correct Answer: C) A DataFrame**
> 
> The `pd.read_csv()` function reads a CSV file and returns a pandas DataFrame.

**Q7.** What does the `.head()` method do?

- [ ] A) Returns the last rows of a DataFrame
- [ ] B) Returns the first n rows of a DataFrame
- [ ] C) Counts the rows in a DataFrame
- [ ] D) Deletes rows from a DataFrame

> [!success]- Answer **Correct Answer: B) Returns the first n rows of a DataFrame**
> 
> `.head()` displays the first n rows (default is 5) of a DataFrame for quick inspection.
> 
> ```python
> data_frame.head()    # First 5 rows
> data_frame.head(10)  # First 10 rows
> ```

**Q8.** In `.loc[row_filter, column_filter]`, what does `:` mean?

- [ ] A) "None"
- [ ] B) "First"
- [ ] C) "All"
- [ ] D) "Last"

> [!success]- Answer **Correct Answer: C) "All"**
> 
> The colon `:` means "all" when filtering DataFrames. For example:
> 
> - `.loc[:, ["name"]]` - all rows, only "name" column
> - `.loc[filter, :]` - filtered rows, all columns

**Q9.** What pandas function is used to filter a DataFrame by rows and columns?

- [ ] A) `.filter()`
- [ ] B) `.loc[]`
- [ ] C) `.select()`
- [ ] D) `.where()`

> [!success]- Answer **Correct Answer: B) `.loc[]`**
> 
> `.loc[]` is the accessor used to filter DataFrames by rows and columns using labels and boolean conditions.

**Q10.** Which method writes a DataFrame to a CSV file?

- [ ] A) `.write_csv()`
- [ ] B) `.export_csv()`
- [ ] C) `.to_csv()`
- [ ] D) `.save_csv()`

> [!success]- Answer **Correct Answer: C) `.to_csv()`**
> 
> The `.to_csv()` method writes a DataFrame to a CSV file at the specified path.
> 
> ```python
> data_frame.to_csv("output.csv")
> ```

### Pipeline Components

**Q11.** In the code `extracted_data = extract(file_name="raw_data.csv")`, what is the purpose of the `extract()` function?

- [ ] A) To transform data
- [ ] B) To load data to a destination
- [ ] C) To read data from a source
- [ ] D) To delete data

> [!success]- Answer **Correct Answer: C) To read data from a source**
> 
> The `extract()` function is responsible for reading/extracting data from a source (in this case, a CSV file).

**Q12.** What is the correct order of operations in an ETL pipeline?

- [ ] A) Load → Transform → Extract
- [ ] B) Transform → Extract → Load
- [ ] C) Extract → Transform → Load
- [ ] D) Extract → Load → Transform

> [!success]- Answer **Correct Answer: C) Extract → Transform → Load**
> 
> ETL follows this specific order: Extract data from source, Transform it using Python/pandas, then Load it to the destination.

**Q13.** What is the correct order of operations in an ELT pipeline?

- [ ] A) Extract → Transform → Load
- [ ] B) Extract → Load → Transform
- [ ] C) Load → Extract → Transform
- [ ] D) Transform → Load → Extract

> [!success]- Answer **Correct Answer: B) Extract → Load → Transform**
> 
> ELT loads raw data first to the data warehouse, then transforms it there using SQL.

**Q14.** In an ELT pipeline, where does transformation typically occur?

- [ ] A) In Python using pandas
- [ ] B) In the data warehouse using SQL
- [ ] C) In the source system
- [ ] D) Transformations don't occur in ELT

> [!success]- Answer **Correct Answer: B) In the data warehouse using SQL**
> 
> ELT pipelines perform transformations inside the data warehouse using SQL queries after the raw data has been loaded.

### SQL and Data Warehouses

**Q15.** Which method is commonly used to execute SQL queries in a data warehouse?

- [ ] A) `.run()`
- [ ] B) `.execute()`
- [ ] C) `.query()`
- [ ] D) `.sql()`

> [!success]- Answer **Correct Answer: B) `.execute()`**
> 
> The `.execute()` method (or similar like `.run_sql()`) is used to execute SQL commands in data warehouses.
> 
> ```python
> data_warehouse.execute("CREATE TABLE ...")
> ```

**Q16.** What type of data sources does ELT typically work with?

- [ ] A) Only non-tabular data
- [ ] B) Typically tabular data
- [ ] C) Only images
- [ ] D) Only text files

> [!success]- Answer **Correct Answer: B) Typically tabular data**
> 
> ELT pipelines are designed for tabular data that can be loaded into data warehouses and transformed using SQL.

**Q17.** Which pandas method is used to load a DataFrame to a SQL database?

- [ ] A) `.to_database()`
- [ ] B) `.to_sql()`
- [ ] C) `.load_sql()`
- [ ] D) `.write_sql()`

> [!success]- Answer **Correct Answer: B) `.to_sql()`**
> 
> `.to_sql()` writes a DataFrame to a SQL database table.
> 
> ```python
> data_frame.to_sql(name="table_name", con=connection)
> ```

**Q18.** In the example SQL query, what does `GROUP BY ds` do?

```sql
SELECT ds, SUM(sales)
FROM raw_sales_data
GROUP BY ds;
```

- [ ] A) Filters the data
- [ ] B) Sorts the data
- [ ] C) Aggregates data by the ds column
- [ ] D) Deletes data

> [!success]- Answer **Correct Answer: C) Aggregates data by the ds column**
> 
> `GROUP BY` groups rows with the same value in the specified column, allowing aggregate functions like `SUM()` to be applied to each group.

### Output Formats

**Q19.** Besides `.to_csv()`, which other output methods are available for DataFrames?

- [ ] A) Only `.to_txt()`
- [ ] B) `.to_json()`, `.to_excel()`, `.to_sql()`
- [ ] C) Only `.to_sql()`
- [ ] D) DataFrames cannot be exported

> [!success]- Answer **Correct Answer: B) `.to_json()`, `.to_excel()`, `.to_sql()`**
> 
> pandas DataFrames can be exported to multiple formats:
> 
> - `.to_csv()` - CSV files
> - `.to_json()` - JSON files
> - `.to_excel()` - Excel files
> - `.to_sql()` - SQL databases

**Q20.** What happens when you run `data_frame.to_csv("output.csv")`?

- [ ] A) Reads a CSV file
- [ ] B) Deletes a CSV file
- [ ] C) Creates/overwrites a CSV file with the DataFrame data
- [ ] D) Appends to an existing CSV file

> [!success]- Answer **Correct Answer: C) Creates/overwrites a CSV file with the DataFrame data**
> 
> `.to_csv()` creates a new CSV file (or overwrites if it exists) containing the DataFrame data.

---

## Section B: True/False Questions (1 point each = 15 points)

**Q21.** Data pipelines are responsible for moving data from a source to a destination. **T/F**

> [!success]- Answer **True** - Data pipelines handle the movement and transformation of data from sources to destinations.

**Q22.** ETL is a more recent pattern than ELT. **T/F**

> [!success]- Answer **False** - ETL is the traditional pattern. ELT is the more recent approach designed for modern data warehouses.

**Q23.** In an ETL pipeline, data is transformed using SQL in the data warehouse. **T/F**

> [!success]- Answer **False** - In ETL, data is transformed outside the destination using Python/pandas. In ELT, transformation happens in the data warehouse using SQL.

**Q24.** The `pd.read_csv()` function returns a pandas DataFrame. **T/F**

> [!success]- Answer **True** - `pd.read_csv()` reads a CSV file and returns it as a DataFrame.

**Q25.** The `.head()` method displays all rows in a DataFrame. **T/F**

> [!success]- Answer **False** - `.head()` displays only the first n rows (default 5), not all rows.

**Q26.** In `.loc[]`, the colon `:` means "all". **T/F**

> [!success]- Answer **True** - The `:` symbol means "all rows" or "all columns" depending on its position.

**Q27.** `.to_sql()` is used to write a DataFrame to a SQL database. **T/F**

> [!success]- Answer **True** - The `.to_sql()` method loads DataFrame data into a SQL database table.

**Q28.** ELT pipelines load raw data first, then transform it in the data warehouse. **T/F**

> [!success]- Answer **True** - This is the defining characteristic of ELT: load first, transform after.

**Q29.** The `.execute()` method is used to run SQL queries in a data warehouse. **T/F**

> [!success]- Answer **True** - `.execute()` or similar methods (like `.run_sql()`) execute SQL commands in data warehouses.

**Q30.** ETL pipelines can work with both tabular and non-tabular data sources. **T/F**

> [!success]- Answer **True** - ETL is flexible and can handle various data types, including non-tabular sources.

**Q31.** In the expression `data_frame.loc[data_frame["name"] == "Apparel", :]`, the first part filters rows. **T/F**

> [!success]- Answer **True** - `data_frame["name"] == "Apparel"` is the row filter that selects rows where name equals "Apparel".

**Q32.** `.to_json()` and `.to_excel()` are valid DataFrame export methods. **T/F**

> [!success]- Answer **True** - Both are valid methods for exporting DataFrames to different file formats.

**Q33.** In an ETL pipeline, you always transform data before extracting it. **T/F**

> [!success]- Answer **False** - The order is Extract → Transform → Load. You extract first, then transform.

**Q34.** Data warehouses are typically used in ELT pipelines. **T/F**

> [!success]- Answer **True** - ELT is designed to leverage data warehouse capabilities for transformation.

**Q35.** The `delimiter` parameter in `pd.read_csv()` can be used to specify a custom separator. **T/F**

> [!success]- Answer **True** - The `delimiter` parameter allows you to specify separators other than comma (e.g., tabs, pipes).

---

## Section C: Short Answer Questions (3 points each = 21 points)

**Q36.** Explain the three main components of a data pipeline and what each one does.

> [!success]- Answer The three main components of a data pipeline are:
> 
> **1. Extract**
> 
> - Reads data from the source
> - Sources can be CSV files, databases, APIs, etc.
> - Example: `pd.read_csv("raw_data.csv")`
> 
> **2. Transform**
> 
> - Processes and cleans the data
> - Operations include filtering, aggregating, cleaning null values
> - In ETL: done using Python/pandas
> - In ELT: done using SQL in the data warehouse
> 
> **3. Load**
> 
> - Writes data to the destination
> - Destinations can be databases, files, data warehouses
> - Example: `data_frame.to_sql("table_name", con=connection)`
> 
> Together, these components move data from source to destination while ensuring it's in the right format and quality.

**Q37.** Compare ETL and ELT pipelines. What are the key differences in where and how transformation occurs?

> [!success]- Answer **ETL (Extract, Transform, Load):**
> 
> - **Transformation location:** Outside the destination (in Python/pandas)
> - **When it transforms:** After extraction, before loading
> - **Tools used:** Python, pandas, custom logic
> - **Pattern:** Traditional approach
> - **Best for:** Complex transformations, non-tabular data
> 
> **ELT (Extract, Load, Transform):**
> 
> - **Transformation location:** Inside the data warehouse (using SQL)
> - **When it transforms:** After loading raw data
> - **Tools used:** SQL queries in the data warehouse
> - **Pattern:** More recent approach
> - **Best for:** Tabular data, SQL-friendly transformations
> 
> **Key difference:** ETL transforms data before storing it (using Python), while ELT stores raw data first and transforms it later (using SQL in the warehouse). This means ELT keeps raw data accessible in the warehouse, while ETL only stores the transformed result.

**Q38.** Describe how to use `.loc[]` to filter a DataFrame. Provide examples of filtering by rows, columns, and both.

> [!success]- Answer The `.loc[]` accessor filters DataFrames using the syntax: `.loc[row_filter, column_filter]`
> 
> **Filtering by rows (all columns):**
> 
> ```python
> # Select rows where name equals "Apparel", all columns
> data_frame.loc[data_frame["name"] == "Apparel", :]
> ```
> 
> **Filtering by columns (all rows):**
> 
> ```python
> # Select all rows, only "name" and "num_firms" columns
> data_frame.loc[:, ["name", "num_firms"]]
> ```
> 
> **Filtering by both rows AND columns:**
> 
> ```python
> # Select specific rows AND specific columns
> data_frame.loc[
>     data_frame["name"] == "Apparel",
>     ["name", "num_firms"]
> ]
> ```
> 
> **Key points:**
> 
> - `:` means "all"
> - First parameter: row filter
> - Second parameter: column filter
> - Row filters use boolean conditions
> - Column filters use lists of column names

**Q39.** What are the different methods for exporting a DataFrame, and when would you use each?

> [!success]- Answer **DataFrame export methods:**
> 
> **1. `.to_csv("file.csv")`**
> 
> - **Format:** Comma-separated values
> - **When to use:** Simple tabular data, widely compatible, human-readable
> - **Example:** Exporting cleaned data for Excel or other tools
> 
> **2. `.to_json("file.json")`**
> 
> - **Format:** JSON (JavaScript Object Notation)
> - **When to use:** Web applications, APIs, nested data structures
> - **Example:** Sending data to a web service
> 
> **3. `.to_excel("file.xlsx")`**
> 
> - **Format:** Excel spreadsheet
> - **When to use:** Business users, formatted reports, multiple sheets
> - **Example:** Creating reports for non-technical stakeholders
> 
> **4. `.to_sql("table_name", con=connection)`**
> 
> - **Format:** SQL database table
> - **When to use:** Storing in databases, enabling SQL queries
> - **Example:** Loading data into PostgreSQL, MySQL, etc.
> 
> **Parameters to customize:**
> 
> - `index=False` - Don't include row index
> - `header=True/False` - Include/exclude column names
> - `sep='\t'` - Use different separator (for CSV)

**Q40.** Explain what happens in each step of this ETL pipeline:

```python
extracted_data = extract(file_name="raw_data.csv")
transformed_data = transform(data_frame=extracted_data, value="Apparel")
load(data_frame=transformed_data, file_name="cleaned_data.csv")
```

> [!success]- Answer This code demonstrates a complete ETL pipeline with three steps:
> 
> **Step 1: Extract**
> 
> ```python
> extracted_data = extract(file_name="raw_data.csv")
> ```
> 
> - Calls the `extract()` function
> - Reads data from "raw_data.csv"
> - Returns a DataFrame stored in `extracted_data`
> - Likely uses `pd.read_csv()` internally
> 
> **Step 2: Transform**
> 
> ```python
> transformed_data = transform(data_frame=extracted_data, value="Apparel")
> ```
> 
> - Calls the `transform()` function
> - Takes the extracted DataFrame
> - Filters rows where name == "Apparel"
> - Selects specific columns (["name", "num_firms"])
> - Returns filtered DataFrame stored in `transformed_data`
> - Uses `.loc[]` internally for filtering
> 
> **Step 3: Load**
> 
> ```python
> load(data_frame=transformed_data, file_name="cleaned_data.csv")
> ```
> 
> - Calls the `load()` function
> - Takes the transformed DataFrame
> - Writes it to "cleaned_data.csv"
> - Likely uses `.to_csv()` internally
> 
> **Result:** Data flows from raw_data.csv → filtered/transformed → cleaned_data.csv

**Q41.** Describe how SQL queries are used in ELT pipelines. Provide an example of a transformation query.

> [!success]- Answer **SQL in ELT pipelines:**
> 
> In ELT pipelines, SQL queries are used to transform data AFTER it has been loaded into the data warehouse. The raw data is stored in the warehouse, then SQL is used to create new tables with transformed data.
> 
> **How it works:**
> 
> 1. Raw data is loaded to the warehouse (using Python/pandas)
> 2. SQL queries run inside the warehouse to transform data
> 3. Transformed data is stored in new tables
> 4. Original raw data remains accessible
> 
> **Example transformation query:**
> 
> ```python
> def transform(source_table, target_table):
>     data_warehouse.execute("""
>         CREATE TABLE {target_table} AS
>         SELECT
>             ds AS date,
>             SUM(sales) AS total_sales,
>             COUNT(*) AS num_transactions
>         FROM {source_table}
>         WHERE sales > 0
>         GROUP BY ds
>         ORDER BY ds;
>     """)
> ```
> 
> **What this does:**
> 
> - Creates a new table `target_table`
> - Aggregates sales data by date
> - Filters out invalid sales (≤ 0)
> - Calculates totals and counts
> - Orders results by date
> 
> **Advantages:**
> 
> - Leverages data warehouse's processing power
> - Keeps raw data available
> - Easy to re-run transformations
> - SQL is optimized for data warehouse operations

**Q42.** What is the purpose of the `.head()` method, and how is it useful when working with data pipelines?

> [!success]- Answer **Purpose of `.head()`:**
> 
> The `.head()` method returns the first n rows of a DataFrame (default n=5), allowing you to quickly inspect data without displaying the entire dataset.
> 
> **Syntax:**
> 
> ```python
> data_frame.head()     # First 5 rows (default)
> data_frame.head(10)   # First 10 rows
> data_frame.head(3)    # First 3 rows
> ```
> 
> **Why it's useful in data pipelines:**
> 
> **1. Quick verification**
> 
> - Check if data extracted correctly
> - Verify column names and structure
> - Spot obvious data quality issues
> 
> **2. Debugging**
> 
> - See results after each pipeline step
> - Verify transformations worked as expected
> - Compare before/after transformation
> 
> **3. Performance**
> 
> - Doesn't load entire dataset into memory
> - Fast way to preview large files
> - Avoid overwhelming output in notebooks/logs
> 
> **4. Development workflow**
> 
> ```python
> # Extract
> extracted = extract("large_file.csv")
> print(extracted.head())  # Verify extraction
> 
> # Transform
> transformed = transform(extracted)
> print(transformed.head())  # Verify transformation
> 
> # Load
> load(transformed, "output.csv")
> ```
> 
> **Example output:**
> 
> ```
>    name           num_firms  total_income
> 0  Advertising    58         3892.41
> 1  Apparel        39         5422.69
> 2  Banking        42         8934.12
> ```
> 
> This gives you confidence that your pipeline is working correctly at each stage.

---

## Section D: Code Analysis & Scenarios (3 points each = 24 points)

**Q43.** Write a complete `extract()` function that reads a CSV file and returns a DataFrame. Include error handling.

> [!success]- Answer **Solution:**
> 
> ```python
> import pandas as pd
> 
> def extract(file_name):
>     """Extract data from a CSV file
>     
>     :param file_name: path to the CSV file
>     :return: DataFrame containing the data
>     """
>     try:
>         # Read CSV file into DataFrame
>         data_frame = pd.read_csv(file_name)
>         
>         # Log success
>         print(f"Successfully extracted data from {file_name}")
>         print(f"Loaded {len(data_frame)} rows and {len(data_frame.columns)} columns")
>         
>         # Show preview
>         print("\nFirst few rows:")
>         print(data_frame.head())
>         
>         return data_frame
>         
>     except FileNotFoundError:
>         print(f"Error: File '{file_name}' not found")
>         return None
>     except pd.errors.EmptyDataError:
>         print(f"Error: File '{file_name}' is empty")
>         return None
>     except Exception as e:
>         print(f"Error reading file: {e}")
>         return None
> ```
> 
> **Usage:**
> 
> ```python
> data = extract("raw_data.csv")
> if data is not None:
>     # Continue with pipeline
>     pass
> ```

**Q44.** Write a `transform()` function that filters a DataFrame for rows where a specific column equals a given value and selects only certain columns.

> [!success]- Answer **Solution:**
> 
> ```python
> def transform(data_frame, filter_column, filter_value, select_columns):
>     """Transform DataFrame by filtering rows and selecting columns
>     
>     :param data_frame: DataFrame to transform
>     :param filter_column: column name to filter on
>     :param filter_value: value to filter for
>     :param select_columns: list of columns to keep
>     :return: transformed DataFrame
>     """
>     try:
>         # Filter rows and select columns
>         transformed = data_frame.loc[
>             data_frame[filter_column] == filter_value,
>             select_columns
>         ]
>         
>         # Log transformation
>         print(f"Transforming data:")
>         print(f"  - Filtering {filter_column} == '{filter_value}'")
>         print(f"  - Selecting columns: {select_columns}")
>         print(f"  - Result: {len(transformed)} rows")
>         
>         return transformed
>         
>     except KeyError as e:
>         print(f"Error: Column not found - {e}")
>         return None
>     except Exception as e:
>         print(f"Error during transformation: {e}")
>         return None
> ```
> 
> **Usage:**
> 
> ```python
> transformed_data = transform(
>     data_frame=extracted_data,
>     filter_column="name",
>     filter_value="Apparel",
>     select_columns=["name", "num_firms", "total_income"]
> )
> ```

**Q45.** Write a `load()` function that saves a DataFrame to a CSV file with options to exclude the index and include a custom header.

> [!success]- Answer **Solution:**
> 
> ```python
> def load(data_frame, file_name, include_index=False, custom_header=True):
>     """Load DataFrame to a CSV file
>     
>     :param data_frame: DataFrame to save
>     :param file_name: output file path
>     :param include_index: whether to include row index
>     :param custom_header: whether to include column headers
>     :return: True if successful, False otherwise
>     """
>     try:
>         # Write DataFrame to CSV
>         data_frame.to_csv(
>             file_name,
>             index=include_index,
>             header=custom_header
>         )
>         
>         # Log success
>         print(f"Successfully loaded data to {file_name}")
>         print(f"  - Rows: {len(data_frame)}")
>         print(f"  - Columns: {len(data_frame.columns)}")
>         print(f"  - Index included: {include_index}")
>         print(f"  - Header included: {custom_header}")
>         
>         return True
>         
>     except PermissionError:
>         print(f"Error: Permission denied writing to '{file_name}'")
>         return False
>     except Exception as e:
>         print(f"Error loading data: {e}")
>         return False
> ```
> 
> **Usage:**
> 
> ```python
> # Save with default options (no index, with header)
> load(transformed_data, "output.csv")
> 
> # Save with custom options
> load(transformed_data, "output.csv", include_index=True, custom_header=False)
> ```

**Q46.** Create a complete ETL pipeline that extracts data from "sales.csv", filters for sales > 1000, selects "date" and "amount" columns, and loads to "filtered_sales.csv".

> [!success]- Answer **Complete ETL Pipeline:**
> 
> ```python
> import pandas as pd
> 
> def extract(file_name):
>     """Extract data from CSV file"""
>     print(f"[EXTRACT] Reading from {file_name}")
>     data = pd.read_csv(file_name)
>     print(f"[EXTRACT] Loaded {len(data)} rows")
>     return data
> 
> def transform(data_frame):
>     """Filter for sales > 1000 and select specific columns"""
>     print("[TRANSFORM] Applying transformations")
>     
>     # Filter for sales > 1000
>     filtered = data_frame.loc[
>         data_frame["amount"] > 1000,
>         ["date", "amount"]
>     ]
>     
>     print(f"[TRANSFORM] Filtered from {len(data_frame)} to {len(filtered)} rows")
>     print(f"[TRANSFORM] Selected columns: date, amount")
>     
>     return filtered
> 
> def load(data_frame, file_name):
>     """Load DataFrame to CSV file"""
>     print(f"[LOAD] Writing to {file_name}")
>     data_frame.to_csv(file_name, index=False)
>     print(f"[LOAD] Successfully saved {len(data_frame)} rows")
> 
> # Execute the ETL pipeline
> if __name__ == "__main__":
>     print("="*50)
>     print("Starting ETL Pipeline")
>     print("="*50)
>     
>     # Extract
>     raw_data = extract("sales.csv")
>     
>     # Transform
>     clean_data = transform(raw_data)
>     
>     # Load
>     load(clean_data, "filtered_sales.csv")
>     
>     print("="*50)
>     print("ETL Pipeline Complete!")
>     print("="*50)
> ```
> 
> **Expected Output:**
> 
> ```
> ==================================================
> Starting ETL Pipeline
> ==================================================
> [EXTRACT] Reading from sales.csv
> [EXTRACT] Loaded 150 rows
> [TRANSFORM] Applying transformations
> [TRANSFORM] Filtered from 150 to 87 rows
> [TRANSFORM] Selected columns: date, amount
> [LOAD] Writing to filtered_sales.csv
> [LOAD] Successfully saved 87 rows
> ==================================================
> ETL Pipeline Complete!
> ==================================================
> ```

**Q47.** Write code that demonstrates the difference between ETL and ELT pipelines for the same data transformation task.

> [!success]- Answer **Complete Comparison:**
> 
> ```python
> import pandas as pd
> 
> # ==================== ETL Pipeline ====================
> def etl_pipeline():
>     """ETL: Transform BEFORE loading"""
>     print("ETL PIPELINE")
>     print("-" * 40)
>     
>     # 1. Extract
>     print("1. Extracting from source...")
>     data = pd.read_csv("raw_sales.csv")
>     print(f"   Extracted {len(data)} rows")
>     
>     # 2. Transform (in Python/pandas)
>     print("2. Transforming in Python...")
>     transformed = data.loc[
>         data["amount"] > 100,
>         ["date", "product", "amount"]
>     ]
>     # Add calculated column
>     transformed["tax"] = transformed["amount"] * 0.1
>     print(f"   Transformed to {len(transformed)} rows")
>     
>     # 3. Load (only transformed data)
>     print("3. Loading transformed data...")
>     transformed.to_sql("sales_summary", con=db_connection)
>     print("   Loaded to database")
>     print("   ✓ Only clean data stored\n")
> 
> # ==================== ELT Pipeline ====================
> def elt_pipeline():
>     """ELT: Transform AFTER loading"""
>     print("ELT PIPELINE")
>     print("-" * 40)
>     
>     # 1. Extract
>     print("1. Extracting from source...")
>     data = pd.read_csv("raw_sales.csv")
>     print(f"   Extracted {len(data)} rows")
>     
>     # 2. Load (raw data first)
>     print("2. Loading raw data...")
>     data.to_sql("raw_sales", con=data_warehouse)
>     print("   Loaded raw data to warehouse")
>     
>     # 3. Transform (in SQL, in the warehouse)
>     print("3. Transforming in warehouse using SQL...")
>     data_warehouse.execute("""
>         CREATE TABLE sales_summary AS
>         SELECT
>             date,
>             product,
>             amount,
>             amount * 0.1 AS tax
>         FROM raw_sales
>         WHERE amount > 100;
>     """)
>     print("   Transformation complete")
>     print("   ✓ Both raw and clean data available\n")
> 
> # Run both pipelines
> etl_pipeline()
> elt_pipeline()
> 
> # ==================== Key Differences ====================
> print("KEY DIFFERENCES:")
> print("-" * 40)
> print("ETL:")
> print("  • Transforms in Python (pandas)")
> print("  • Only stores final result")
> print("  • Uses application resources")
> print("  • Good for complex Python logic")
> print()
> print("ELT:")
> print("  • Transforms in SQL (warehouse)")
> print("  • Keeps raw + transformed data")
> print("  • Uses warehouse resources")
> print("  • Good for SQL operations")
> ```
> 
> **Output Comparison:**
> 
> ```
> ETL PIPELINE
> ----------------------------------------
> 1. Extracting from source...
>    Extracted 1000 rows
> 2. Transforming in Python...
>    Transformed to 756 rows
> 3. Loading transformed data...
>    Loaded to database
>    ✓ Only clean data stored
> 
> ELT PIPELINE
> ----------------------------------------
> 4. Extracting from source...
>    Extracted 1000 rows
> 5. Loading raw data...
>    Loaded raw data to warehouse
> 6. Transforming in warehouse using SQL...
>    Transformation complete
>    ✓ Both raw and clean data available
> ```

**Q48.** Given this DataFrame, write `.loc[]` statements to accomplish three different filtering tasks:

```python
data = pd.DataFrame({
    'product': ['A', 'B', 'C', 'A', 'B'],
    'sales': [100, 200, 150, 300, 250],
    'region': ['North', 'South', 'North', 'South', 'North']
})
```

Tasks: (a) Get all North region rows, (b) Get only product and sales columns, (c) Get North region rows with only product column.

> [!success]- Answer **Solutions:**
> 
> ```python
> import pandas as pd
> 
> # Create the DataFrame
> data = pd.DataFrame({
>     'product': ['A', 'B', 'C', 'A', 'B'],
>     'sales': [100, 200, 150, 300, 250],
>     'region': ['North', 'South', 'North', 'South', 'North']
> })
> 
> print("Original DataFrame:")
> print(data)
> print()
> 
> # Task (a): Get all North region rows (all columns)
> task_a = data.loc[data['region'] == 'North', :]
> print("Task A - All North region rows:")
> print(task_a)
> print()
> # Output:
> #   product  sales region
> # 0       A    100  North
> # 2       C    150  North
> # 4       B    250  North
> 
> # Task (b): Get only product and sales columns (all rows)
> task_b = data.loc[:, ['product', 'sales']]
> print("Task B - Only product and sales columns:")
> print(task_b)
> print()
> # Output:
> #   product  sales
> # 0       A    100
> # 1       B    200
> # 2       C    150
> # 3       A    300
> # 4       B    250
> 
> # Task (c): Get North region rows with only product column
> task_c = data.loc[data['region'] == 'North', ['product']]
> print("Task C - North region, product column only:")
> print(task_c)
> print()
> # Output:
> #   product
> # 0       A
> # 2       C
> # 4       B
> 
> # BONUS: Combine multiple conditions
> bonus = data.loc[
>     (data['region'] == 'North') & (data['sales'] > 100),
>     ['product', 'sales']
> ]
> print("BONUS - North region with sales > 100:")
> print(bonus)
> # Output:
> #   product  sales
> # 2       C    150
> # 4       B    250
> ```
> 
> **Key syntax patterns:**
> 
> - `.loc[row_condition, :]` - Filter rows, all columns
> - `.loc[:, column_list]` - All rows, specific columns
> - `.loc[row_condition, column_list]` - Filter both
> - Use `&` for AND, `|` for OR in conditions

**Q49.** Identify and fix all issues in this ETL pipeline code:

```python
def extract(file):
    data = pd.read_csv(file)
    return data

def transform(df):
    result = df.loc[df["sales"] > 100]
    return result

def load(df, file):
    df.to_csv(file)

data = extract("sales.csv")
transformed = transform(data)
load(transformed, "output.csv")
```

> [!success]- Answer **Issues identified:**
> 
> 1. No import statement for pandas
> 2. No error handling
> 3. No logging/feedback to user
> 4. Transform function doesn't select specific columns (uses all)
> 5. Load function doesn't exclude index (will create unwanted column)
> 6. No docstrings
> 7. No validation that data exists before transforming
> 
> **Fixed code:**
> 
> ```python
> import pandas as pd
> 
> def extract(file_name):
>     """Extract data from a CSV file
>     
>     :param file_name: path to CSV file
>     :return: DataFrame or None if error
>     """
>     try:
>         print(f"Extracting data from {file_name}...")
>         data = pd.read_csv(file_name)
>         print(f"✓ Extracted {len(data)} rows, {len(data.columns)} columns")
>         return data
>     except FileNotFoundError:
>         print(f"✗ Error: File '{file_name}' not found")
>         return None
>     except Exception as e:
>         print(f"✗ Error extracting data: {e}")
>         return None
> 
> def transform(data_frame):
>     """Transform data by filtering sales > 100
>     
>     :param data_frame: DataFrame to transform
>     :return: transformed DataFrame or None if error
>     """
>     if data_frame is None or data_frame.empty:
>         print("✗ Error: No data to transform")
>         return None
>     
>     try:
>         print("Transforming data...")
>         # Filter rows and select specific columns
>         result = data_frame.loc[
>             data_frame["sales"] > 100,
>             ["product", "sales", "date"]
>         ]
>         print(f"✓ Transformed: {len(result)} rows remaining")
>         return result
>     except KeyError as e:
>         print(f"✗ Error: Column not found - {e}")
>         return None
>     except Exception as e:
>         print(f"✗ Error transforming data: {e}")
>         return None
> 
> def load(data_frame, file_name):
>     """Load DataFrame to CSV file
>     
>     :param data_frame: DataFrame to save
>     :param file_name: output file path
>     :return: True if successful, False otherwise
>     """
>     if data_frame is None or data_frame.empty:
>         print("✗ Error: No data to load")
>         return False
>     
>     try:
>         print(f"Loading data to {file_name}...")
>         # Exclude index when writing
>         data_frame.to_csv(file_name, index=False)
>         print(f"✓ Successfully loaded {len(data_frame)} rows")
>         return True
>     except Exception as e:
>         print(f"✗ Error loading data: {e}")
>         return False
> 
> # Execute pipeline with error handling
> if __name__ == "__main__":
>     print("="*50)
>     print("ETL Pipeline Starting")
>     print("="*50)
>     
>     # Extract
>     data = extract("sales.csv")
>     
>     # Only continue if extraction succeeded
>     if data is not None:
>         # Transform
>         transformed = transform(data)
>         
>         # Only load if transformation succeeded
>         if transformed is not None:
>             # Load
>             success = load(transformed, "output.csv")
>             
>             if success:
>                 print("="*50)
>                 print("✓ Pipeline completed successfully!")
>                 print("="*50)
>             else:
>                 print("✗ Pipeline failed at load stage")
>         else:
>             print("✗ Pipeline failed at transform stage")
>     else:
>         print("✗ Pipeline failed at extract stage")
> ```
> 
> **Improvements made:**
> 
> - ✓ Added pandas import
> - ✓ Added comprehensive error handling
> - ✓ Added logging for each step
> - ✓ Added docstrings
> - ✓ Added data validation
> - ✓ Excluded index in CSV output
> - ✓ Selected specific columns in transform
> - ✓ Return values indicate success/failure
> - ✓ Pipeline stops if any stage fails

**Q50.** Create an ELT pipeline that loads data to a warehouse and transforms it using SQL to calculate total sales by product.

> [!success]- Answer **Complete ELT Pipeline:**
> 
> ```python
> import pandas as pd
> from sqlalchemy import create_engine
> 
> # Database connection (example)
> warehouse_engine = create_engine('postgresql://user:pass@localhost/warehouse')
> 
> def extract(file_name):
>     """Extract data from CSV file
>     
>     :param file_name: path to CSV file
>     :return: DataFrame
>     """
>     print(f"[EXTRACT] Reading data from {file_name}")
>     data = pd.read_csv(file_name)
>     print(f"[EXTRACT] Loaded {len(data)} rows")
>     return data
> 
> def load(data_frame, table_name, engine):
>     """Load raw data to data warehouse
>     
>     :param data_frame: DataFrame to load
>     :param table_name: target table name
>     :param engine: database connection
>     """
>     print(f"[LOAD] Loading raw data to {table_name} table")
>     data_frame.to_sql(
>         name=table_name,
>         con=engine,
>         if_exists='replace',  # Replace if table exists
>         index=False
>     )
>     print(f"[LOAD] Successfully loaded {len(data_frame)} rows")
> 
> def transform(source_table, target_table, engine):
>     """Transform data in warehouse using SQL
>     
>     :param source_table: source table name
>     :param target_table: target table name
>     :param engine: database connection
>     """
>     print(f"[TRANSFORM] Transforming data in warehouse")
>     print(f"[TRANSFORM] Creating {target_table} from {source_table}")
>     
>     # SQL transformation query
>     transform_query = f"""
>         CREATE TABLE {target_table} AS
>         SELECT
>             product,
>             COUNT(*) AS num_transactions,
>             SUM(sales) AS total_sales,
>             AVG(sales) AS avg_sales,
>             MIN(sales) AS min_sales,
>             MAX(sales) AS max_sales
>         FROM {source_table}
>         WHERE sales > 0
>         GROUP BY product
>         ORDER BY total_sales DESC;
>     """
>     
>     # Execute transformation
>     with engine.connect() as connection:
>         # Drop target table if exists
>         connection.execute(f"DROP TABLE IF EXISTS {target_table}")
>         # Create new transformed table
>         connection.execute(transform_query)
>     
>     print(f"[TRANSFORM] Transformation complete")
>     print(f"[TRANSFORM] Created summary table: {target_table}")
> 
> def verify_results(table_name, engine):
>     """Verify the transformed results
>     
>     :param table_name: table to query
>     :param engine: database connection
>     """
>     print(f"\n[VERIFY] Checking results in {table_name}")
>     query = f"SELECT * FROM {table_name} LIMIT 5"
>     results = pd.read_sql(query, engine)
>     print(results)
> 
> # ==================== Execute ELT Pipeline ====================
> if __name__ == "__main__":
>     print("="*60)
>     print("ELT PIPELINE - Sales Summary")
>     print("="*60)
>     
>     # Step 1: Extract data from CSV
>     raw_data = extract("sales_data.csv")
>     
>     # Step 2: Load raw data to warehouse
>     load(
>         data_frame=raw_data,
>         table_name="raw_sales",
>         engine=warehouse_engine
>     )
>     
>     # Step 3: Transform in warehouse using SQL
>     transform(
>         source_table="raw_sales",
>         target_table="sales_by_product",
>         engine=warehouse_engine
>     )
>     
>     # Verify results
>     verify_results("sales_by_product", warehouse_engine)
>     
>     print("="*60)
>     print("ELT Pipeline Complete!")
>     print("="*60)
> ```
> 
> **Expected Output:**
> 
> ```
> ============================================================
> ELT PIPELINE - Sales Summary
> ============================================================
> [EXTRACT] Reading data from sales_data.csv
> [EXTRACT] Loaded 1000 rows
> [LOAD] Loading raw data to raw_sales table
> [LOAD] Successfully loaded 1000 rows
> [TRANSFORM] Transforming data in warehouse
> [TRANSFORM] Creating sales_by_product from raw_sales
> [TRANSFORM] Transformation complete
> [TRANSFORM] Created summary table: sales_by_product
> 
> [VERIFY] Checking results in sales_by_product
>   product  num_transactions  total_sales  avg_sales  min_sales  max_sales
> 0       A               234      45600.50     194.87      50.00     500.00
> 1       B               198      38900.25     196.47      75.00     450.00
> 2       C               156      32100.75     205.77     100.00     600.00
> 3       D               189      29800.00     157.67      25.00     400.00
> 4       E               223      28500.50     127.80      30.00     350.00
> ============================================================
> ELT Pipeline Complete!
> ============================================================
> ```
> 
> **Key ELT features demonstrated:**
> 
> - Raw data loaded first to warehouse
> - Transformation happens in SQL (inside warehouse)
> - Both raw and transformed tables available
> - Leverages database aggregation capabilities
> - Multiple statistics calculated efficiently

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (40 points)

Q1: B | Q2: B | Q3: B | Q4: B | Q5: B Q6: C | Q7: B | Q8: C | Q9: B | Q10: C Q11: C | Q12: C | Q13: B | Q14: B | Q15: B Q16: B | Q17: B | Q18: C | Q19: B | Q20: C

### Section B: True/False (15 points)

Q21: T | Q22: F | Q23: F | Q24: T | Q25: F Q26: T | Q27: T | Q28: T | Q29: T | Q30: T Q31: T | Q32: T | Q33: F | Q34: T | Q35: T

### Section C: Short Answer (21 points)

- Each question worth 3 points
- Full credit for complete explanations with examples
- Partial credit for correct but incomplete answers

### Section D: Code Analysis (24 points)

- Each question worth 3 points
- Award points for: correct working code, proper error handling, clear documentation
- Partial credit for partially correct solutions

---

## Quick Study Guide

### ETL vs ELT Comparison

|Feature|ETL|ELT|
|---|---|---|
|**Transform Location**|Outside destination (Python/pandas)|Inside destination (SQL)|
|**Transform Timing**|Before loading|After loading|
|**Pattern Age**|Traditional|More recent|
|**Tools**|Python, pandas|SQL, data warehouses|
|**Data Stored**|Only transformed|Raw + transformed|
|**Best For**|Complex Python logic, non-tabular|SQL operations, tabular data|

### Essential pandas Functions

```python
# Reading data
df = pd.read_csv("file.csv")              # Read CSV
df.head()                                  # View first 5 rows
df.head(10)                                # View first 10 rows

# Filtering data
df.loc[condition, :]                       # Filter rows
df.loc[:, ["col1", "col2"]]               # Select columns
df.loc[condition, ["col1"]]                # Filter rows AND columns

# Writing data
df.to_csv("output.csv")                    # Write to CSV
df.to_json("output.json")                  # Write to JSON
df.to_excel("output.xlsx")                 # Write to Excel
df.to_sql("table", con=connection)         # Write to SQL
```

### Pipeline Execution Order

**ETL Order:**

```
1. Extract  (from source)
2. Transform (using pandas)
3. Load (to destination)
```

**ELT Order:**

```
1. Extract (from source)
2. Load (to warehouse)
3. Transform (using SQL)
```

---

## Key Concepts Summary

|Concept|Description|Example|
|---|---|---|
|**Data Pipeline**|Moves data from source to destination|ETL or ELT workflow|
|**Extract**|Read data from source|`pd.read_csv()`|
|**Transform**|Process/clean data|Filter, aggregate, clean|
|**Load**|Write data to destination|`.to_sql()`, `.to_csv()`|
|**DataFrame**|pandas data structure|Tabular data in memory|
|**`.loc[]`**|Filter rows/columns|`.loc[rows, cols]`|
|**`:` (colon)**|Means "all" in `.loc[]`|`.loc[:, cols]` = all rows|
|**Data Warehouse**|Database for analytics|Where ELT transforms occur|

---

## Common Mistakes to Avoid

❌ **Don't:**

- Forget to import pandas (`import pandas as pd`)
- Transform data in ETL after loading
- Use ETL when ELT would be more efficient
- Forget the `:` in `.loc[]` (e.g., `.loc[condition]` instead of `.loc[condition, :]`)
- Include index when writing CSV (use `index=False`)
- Forget error handling in production code
- Mix up the order: ETL vs ELT

✅ **Do:**

- Always import pandas at the top
- Transform before loading in ETL
- Use ELT for data warehouse destinations
- Use `.loc[rows, columns]` syntax correctly
- Exclude index in CSV output
- Add try-except blocks for robustness
- Remember: ETL = Transform first, ELT = Load first
- Use `.head()` to verify data at each step
- Add logging to track pipeline progress

---

## Study Strategy (5-Day Plan)

### Day 1: Basics

- Understand data pipeline concept
- Learn ETL vs ELT differences
- Practice reading CSV files
- Use `.head()` to inspect data

### Day 2: Extraction & Filtering

- Master `pd.read_csv()`
- Practice `.loc[]` filtering
- Filter by rows, columns, and both
- Handle different data types

### Day 3: Transformation

- Practice filtering operations
- Learn column selection
- Combine multiple conditions
- Create transformation functions

### Day 4: Loading & Output

- Master `.to_csv()` and parameters
- Learn `.to_sql()` for databases
- Practice other output formats
- Add error handling

### Day 5: Complete Pipelines

- Build complete ETL pipelines
- Build complete ELT pipelines
- Compare the two approaches
- Take practice quiz

---

## Final Exam Checklist

### Before the Exam:

- [ ] Can explain what data pipelines do
- [ ] Know the difference between ETL and ELT
- [ ] Understand when to use each pattern
- [ ] Can use `pd.read_csv()`
- [ ] Can filter with `.loc[]`
- [ ] Know what `:` means in `.loc[]`
- [ ] Can write DataFrames to files
- [ ] Understand SQL in ELT pipelines
- [ ] Can build complete pipelines
- [ ] Know common pandas methods

### During the Exam:

- [ ] Read questions carefully for ETL vs ELT
- [ ] Remember: ETL transforms first, ELT loads first
- [ ] Check `.loc[]` syntax (rows, columns)
- [ ] Include error handling in code questions
- [ ] Add docstrings when creating functions
- [ ] Use `index=False` when writing CSV
- [ ] Remember `:` means "all"
- [ ] Verify pipeline order matches pattern

---

**Good luck with your studies! 📊**

Remember: ETL = Extract → Transform → Load | ELT = Extract → Load → Transform

---

#etl #elt #data-pipelines #pandas #python #data-engineering #csv #sql #dataframes #loc #to-csv #read-csv #data-warehouses #datacamp #certification #chapter1