## Source Systems

### Common Data Sources in This Course

| Source Type       | Description                 |
| ----------------- | --------------------------- |
| **CSV files**     | Comma-separated values      |
| **Parquet files** | Column-oriented file format |
| **JSON files**    | JavaScript Object Notation  |
| **SQL databases** | Relational databases        |

### Other Common Sources (Not Covered)
- APIs (Application Programming Interfaces)
- Data lakes
- Data warehouses
- Web scraping
- Many more!

---

## Reading Parquet Files

### What are Parquet Files?

**Definition:** Open source, column-oriented file format designed for efficient field storage and retrieval

### Characteristics
- **Column-oriented** - Stores data by column, not by row
- **Efficient** - Better compression and faster queries
- **Similar to CSV** - Works similarly in pandas

### Reading Parquet Files

```python
import pandas as pd

# Read the parquet file into memory
raw_stock_data = pd.read_parquet(
    "raw_stock_data.parquet",
    engine="fastparquet"
)
```

### Parameters
- **File path** - Path to the parquet file
- **`engine`** - Parser engine (e.g., "fastparquet", "pyarrow")

---

## Connecting to SQL Databases

### Requirements
- Connection URI (Uniform Resource Identifier)
- SQLAlchemy library for database engine

### Connection URI Format

```
schema_identifier://username:password@host:port/database
```

**Example:**
```
postgresql+psycopg2://repl:password@localhost:5432/market
```

### Components

| Component | Example | Description |
|-----------|---------|-------------|
| **Schema** | `postgresql+psycopg2` | Database type + driver |
| **Username** | `repl` | Database user |
| **Password** | `password` | User password |
| **Host** | `localhost` | Server address |
| **Port** | `5432` | Port number |
| **Database** | `market` | Database name |

### Connecting and Querying

```python
import sqlalchemy
import pandas as pd

# Connection URI
connection_uri = "postgresql+psycopg2://repl:password@localhost:5432/market"

# Create database engine
db_engine = sqlalchemy.create_engine(connection_uri)

# Query the SQL database
raw_stock_data = pd.read_sql(
    "SELECT * FROM raw_stock_data LIMIT 10",
    db_engine
)
```

---

## Modularity in Data Pipelines

### Benefits of Modular Functions

1. **Increases readability** - Clear, focused functions
2. **Don't Repeat Yourself (DRY)** - Write once, use many times
3. **Expedites troubleshooting** - Isolate issues easily
4. **Separates logic** - Each function has one responsibility

### Example: Modular Extract Function

```python
def extract_from_sql(connection_uri, query):
    """Extract data from SQL database
    
    :param connection_uri: database connection string
    :param query: SQL query to execute
    :return: DataFrame with query results
    """
    # Create an engine, query data and return DataFrame
    db_engine = sqlalchemy.create_engine(connection_uri)
    return pd.read_sql(query, db_engine)

# Use the function
data = extract_from_sql(
    "postgresql+psycopg2://repl:password@localhost:5432/market",
    "SELECT * FROM raw_stock_data LIMIT 10;"
)
```

---

## Transforming Data with pandas

### Why Transform Data?

**Purpose:** Data must be properly transformed to ensure value is provided to downstream users

### Powerful pandas Tools
- `.loc[]` - Label-based filtering
- `.iloc[]` - Integer-based filtering
- `.to_datetime()` - Convert to datetime type

---

## Filtering Records with `.loc[]`

### Filtering Rows Only

```python
# Keep only non-zero entries
cleaned = raw_stock_data.loc[raw_stock_data["open"] > 0, :]
```

### Filtering Columns Only

```python
# Remove excess columns
cleaned = raw_stock_data.loc[:, ["timestamps", "open", "close"]]
```

### Filtering Both Rows and Columns

```python
# Combine into one step
cleaned = raw_stock_data.loc[
    raw_stock_data["open"] > 0,
    ["timestamps", "open", "close"]
]
```

---

## Using `.iloc[]` for Integer Indexing

### Difference: `.loc[]` vs `.iloc[]`

| Method | Uses | Example |
|--------|------|---------|
| **`.loc[]`** | Labels/boolean conditions | `.loc[df["col"] > 5, :]` |
| **`.iloc[]`** | Integer positions | `.iloc[0:50, [0, 1, 2]]` |

### Example

```python
# Select first 50 rows, first 3 columns
cleaned = raw_stock_data.iloc[0:50, [0, 1, 2]]
```

**Syntax:** `.iloc[row_positions, column_positions]`

---

## Altering Data Types

### Why Convert Data Types?

Data types often need to be converted for downstream use cases (e.g., proper datetime operations, calculations)

### Using `.to_datetime()`

### Format 1: String Datetime

```python
# "timestamps" column currently looks like: "20230101085731"
# Convert "timestamps" column to type datetime
cleaned["timestamps"] = pd.to_datetime(
    cleaned["timestamps"],
    format="%Y%m%d%H%M%S"
)
```

**Result:** `Timestamp('2023-01-01 08:57:31')`

### Format 2: Unix Timestamp (milliseconds)

```python
# "timestamps" column currently looks like: 1681596000011
# Convert "timestamps" column to type datetime
cleaned["timestamps"] = pd.to_datetime(
    cleaned["timestamps"],
    unit="ms"
)
```

**Result:** `Timestamp('2023-04-15 22:00:00.011000')`

### Common Format Codes

| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | 4-digit year | 2023 |
| `%m` | Month (01-12) | 01 |
| `%d` | Day of month | 15 |
| `%H` | Hour (00-23) | 08 |
| `%M` | Minute (00-59) | 57 |
| `%S` | Second (00-59) | 31 |

---

## Validating Transformations

### Risks of Transformation
- **Losing information** - Accidentally filtering too much
- **Creating faulty data** - Incorrect calculations or conversions

### Validation Methods

```python
# Transform data
cleaned = raw_stock_data.loc[
    raw_stock_data["open"] > 0,
    ["timestamps", "open", "close"]
]

# Method 1: View first rows
print(cleaned.head())

# Method 2: View smallest records
print(cleaned.nsmallest(10, ["timestamps"]))

# Method 3: View largest records
print(cleaned.nlargest(10, ["timestamps"]))
```

### Validation Functions

| Method | Purpose | Usage |
|--------|---------|-------|
| `.head()` | View first n rows | `df.head()` |
| `.nsmallest(n, col)` | View n smallest values | `df.nsmallest(10, ["price"])` |
| `.nlargest(n, col)` | View n largest values | `df.nlargest(10, ["price"])` |

---

## Persisting Data with pandas

### Why Persist Data in ETL?

1. **Stable access** - Ensures data consumers have reliable access
2. **Final step** - Occurs at the end of ETL process
3. **Between steps** - Can save data between discrete pipeline steps
4. **Snapshot** - Captures a point-in-time view of data

---

## Loading Data to CSV Files

### Using `.to_csv()`

```python
import pandas as pd

# Data extraction and transformation
raw_data = pd.read_csv("raw_stock_data.csv")
stock_data = raw_data.loc[
    raw_data["open"] > 100,
    ["timestamps", "open"]
]

# Load data to a .csv file
stock_data.to_csv("stock_data.csv")
```

---

## Customizing CSV File Output

### Common Parameters

#### 1. `header` Parameter

```python
stock_data.to_csv("./stock_data.csv", header=True)
```

**Values:**
- `True` - Include column names (default)
- `False` - Exclude column names
- `list` - Use custom column names

#### 2. `index` Parameter

```python
stock_data.to_csv("./stock_data.csv", index=True)
```

**Values:**
- `True` - Include index column (default)
- `False` - Exclude index column

**Purpose:** Determines whether row index is written to file

#### 3. `sep` Parameter

```python
stock_data.to_csv("./stock_data.csv", sep="|")
```

**Purpose:** Specify column separator character

**Common values:**
- `","` - Comma (default)
- `"|"` - Pipe character
- `"\t"` - Tab character

### Other Output Methods

Similar methods exist for other formats:

| Method | Output Format |
|--------|---------------|
| `.to_parquet()` | Parquet files |
| `.to_json()` | JSON files |
| `.to_sql()` | SQL database |

---

## Ensuring Data Persistence

### Verifying File Creation

```python
import pandas as pd
import os  # Import the os module

# Extract, transform and load data
raw_data = pd.read_csv("raw_stock_data.csv")
stock_data = raw_data.loc[
    raw_data["open"] > 100,
    ["timestamps", "open"]
]
stock_data.to_csv("stock_data.csv")

# Check that the path exists
file_exists = os.path.exists("stock_data.csv")
print(file_exists)
```

**Output:** `True` (if file was created successfully)

### `os.path.exists()` Function

**Purpose:** Check if a file or directory exists

**Returns:** `True` if exists, `False` otherwise

---

## Monitoring a Data Pipeline

### Why Monitor Pipelines?

Data pipelines should be monitored for:
- **Changes to data** - Unexpected data shifts
- **Failures in execution** - Runtime errors

### Common Issues to Monitor

1. **Missing data** - Incomplete datasets
2. **Shifting data types** - Type changes causing errors
3. **Package deprecation** - Library updates breaking code
4. **Functionality changes** - API changes

---

## Logging Data Pipeline Performance

### Purpose of Logging

- **Document performance** - Track execution at each step
- **Starting point for debugging** - Provides context when failures occur
- **Audit trail** - Know what happened and when

### Basic Logging Setup

```python
import logging

# Configure logging
logging.basicConfig(
    format='%(levelname)s: %(message)s',
    level=logging.DEBUG
)
```

### Log Levels

#### DEBUG Level

```python
logging.debug(f"Variable has value {path}")
```

**Output:** `DEBUG: Variable has value raw_file.csv`

**Use for:** Detailed diagnostic information

#### INFO Level

```python
logging.info("Data has been transformed and will now be loaded.")
```

**Output:** `INFO: Data has been transformed and will now be loaded.`

**Use for:** General informational messages

---

## Logging Warnings and Errors

### WARNING Level

```python
logging.warning("Unexpected number of rows detected.")
```

**Output:** `WARNING: Unexpected number of rows detected.`

**Use for:** Something unexpected but not breaking

### ERROR Level

```python
logging.error(f"{ke} arose in execution.")
```

**Output:** `ERROR: KeyError arose in execution.`

**Use for:** Error events that might still allow execution

### Log Levels Hierarchy

| Level | Severity | Purpose |
|-------|----------|---------|
| **DEBUG** | Lowest | Detailed diagnostic info |
| **INFO** | Low | General informational messages |
| **WARNING** | Medium | Unexpected but not breaking |
| **ERROR** | High | Error events |
| **CRITICAL** | Highest | Critical failures |

---

## Handling Exceptions with try-except

### Basic try-except Structure

```python
try:
    # Execute some code here
    ...
except:
    # Logging about failures that occurred
    # Logic to execute upon exception
    ...
```

**Purpose:** Provides a way to execute code if errors occur

---

## Handling Specific Exceptions

### Using Specific Exception Types

```python
try:
    # Try to filter by price_change
    clean_stock_data = transform(raw_stock_data)
    logging.info("Successfully filtered DataFrame by 'price_change'")
    
except KeyError as ke:
    # Handle the error, create new column, transform
    logging.warning(f"{ke}: Cannot filter DataFrame by 'price_change'")
    raw_stock_data["price_change"] = (
        raw_stock_data["close"] - raw_stock_data["open"]
    )
    clean_stock_data = transform(raw_stock_data)
```

### Benefits of Specific Exceptions

- **Targeted handling** - Different actions for different errors
- **Better logging** - Know exactly what went wrong
- **Graceful recovery** - Continue execution when possible

### Common Exception Types

| Exception | When It Occurs |
|-----------|----------------|
| `KeyError` | Dictionary key or DataFrame column not found |
| `FileNotFoundError` | File doesn't exist |
| `ValueError` | Invalid value for operation |
| `TypeError` | Wrong data type |
| `ConnectionError` | Database/network connection fails |

---

## Quick Reference

### Reading Data

```python
# CSV files
pd.read_csv("file.csv")

# Parquet files
pd.read_parquet("file.parquet", engine="fastparquet")

# SQL databases
pd.read_sql("SELECT * FROM table", engine)
```

### Writing Data

```python
# CSV files
df.to_csv("output.csv", index=False, sep=",")

# Parquet files
df.to_parquet("output.parquet")

# JSON files
df.to_json("output.json")

# SQL databases
df.to_sql("table_name", con=engine)
```

### Filtering Data

```python
# Label-based filtering
df.loc[condition, columns]

# Integer-based filtering
df.iloc[row_indices, column_indices]
```

### Datetime Conversion

```python
# From string with format
pd.to_datetime(df["col"], format="%Y%m%d")

# From Unix timestamp
pd.to_datetime(df["col"], unit="ms")
```

### Logging

```python
logging.debug("Debug message")
logging.info("Info message")
logging.warning("Warning message")
logging.error("Error message")
```

---

## Key Concepts Summary

| Concept | Purpose | Example |
|---------|---------|---------|
| **Parquet** | Column-oriented file format | `pd.read_parquet()` |
| **Connection URI** | Database connection string | `postgresql://user:pass@host/db` |
| **Modularity** | Separate logic into functions | DRY principle |
| **`.loc[]`** | Label-based filtering | `.loc[rows, cols]` |
| **`.iloc[]`** | Integer-based filtering | `.iloc[0:10, [0,1]]` |
| **`.to_datetime()`** | Convert to datetime | `format=` or `unit=` |
| **Validation** | Check transformations | `.nsmallest()`, `.nlargest()` |
| **Persistence** | Save data to files | `.to_csv()`, `.to_parquet()` |
| **Logging** | Track execution | `logging.info()` |
| **try-except** | Handle errors | Graceful error handling |

---

#etl #pandas #data-extraction #parquet #sql #sqlalchemy #transformations #logging #error-handling #modularity #data-persistence #datacamp #chapter2

logging.critical(f"Pipeline failed: {e}")
>         raise
> ```

---

## Section D: Code Analysis & Scenarios (3 points each = 24 points)

**Q43.** Write a modular function to extract data from a SQL database. Include the connection URI creation and proper error handling.

> [!success]- Answer
> **Solution:**
> 
> ```python
> import sqlalchemy
> import pandas as pd
> import logging
> 
> def extract_from_sql(host, port, database, username, password, query):
>     """Extract data from SQL database
>     
>     :param host: database host address
>     :param port: connection port
>     :param database: database name
>     :param username: database username
>     :param password: database password
>     :param query: SQL query to execute
>     :return: DataFrame with results or None if error
>     """
>     try:
>         # Build connection URI
>         connection_uri = (
>             f"postgresql+psycopg2://{username}:{password}"
>             f"@{host}:{port}/{database}"
>         )
>         
>         logging.info(f"Connecting to database: {database} at {host}:{port}")
>         
>         # Create database engine
>         db_engine = sqlalchemy.create_engine(connection_uri)
>         
>         # Execute query
>         logging.debug(f"Executing query: {query}")
>         data = pd.read_sql(query, db_engine)
>         
>         logging.info(f"Successfully extracted {len(data)} rows")
>         return data
>         
>     except sqlalchemy.exc.OperationalError as e:
>         logging.error(f"Database connection failed: {e}")
>         return None
>     except Exception as e:
>         logging.error(f"Error extracting data: {e}")
>         return None
> 
> # Usage
> data = extract_from_sql(
>     host="localhost",
>     port=5432,
>     database="market",
>     username="repl",
>     password="password",
>     query="SELECT * FROM raw_stock_data LIMIT 100"
> )
> ```

**Q44.** Write code that reads a Parquet file, filters rows where "sales" > 1000, selects specific columns, converts a timestamp column to datetime, and validates the transformation.

> [!success]- Answer
> **Complete Solution:**
> 
> ```python
> import pandas as pd
> import logging
> 
> logging.basicConfig(format='%(levelname)s: %(message)s', level=logging.INFO)
> 
> def extract_parquet(file_path):
>     """Extract data from Parquet file"""
>     try:
>         logging.info(f"Reading Parquet file: {file_path}")
>         data = pd.read_parquet(file_path, engine="fastparquet")
>         logging.info(f"Loaded {len(data)} rows, {len(data.columns)} columns")
>         return data
>     except Exception as e:
>         logging.error(f"Error reading Parquet: {e}")
>         return None
> 
> def transform_sales_data(df):
>     """Filter, select columns, and convert timestamps"""
>     if df is None:
>         return None
>     
>     try:
>         logging.info("Starting transformation")
>         original_rows = len(df)
>         
>         # Filter rows where sales > 1000
>         # Select specific columns
>         transformed = df.loc[
>             df["sales"] > 1000,
>             ["timestamp", "product", "sales", "region"]
>         ]
>         
>         logging.info(f"Filtered: {original_rows} → {len(transformed)} rows")
>         
>         # Convert timestamp to datetime
>         # Assuming format like "20230115143000"
>         transformed["timestamp"] = pd.to_datetime(
>             transformed["timestamp"],
>             format="%Y%m%d%H%M%S"
>         )
>         
>         logging.info("Converted timestamp to datetime")
>         
>         return transformed
>         
>     except KeyError as e:
>         logging.error(f"Column not found: {e}")
>         return None
>     except Exception as e:
>         logging.error(f"Transformation error: {e}")
>         return None
> 
> def validate_transformation(df):
>     """Validate the transformed data"""
>     if df is None or df.empty:
>         logging.warning("No data to validate")
>         return
>     
>     print("\n" + "="*50)
>     print("VALIDATION REPORT")
>     print("="*50)
>     
>     # Check row count
>     print(f"\nTotal rows: {len(df)}")
>     
>     # Check data types
>     print("\nData types:")
>     print(df.dtypes)
>     
>     # View first records
>     print("\nFirst 5 records:")
>     print(df.head())
>     
>     # Check for nulls
>     print("\nNull values:")
>     print(df.isnull().sum())
>     
>     # View smallest sales
>     print("\n5 Smallest sales (should all be > 1000):")
>     print(df.nsmallest(5, ["sales"])[["timestamp", "sales"]])
>     
>     # View largest sales
>     print("\n5 Largest sales:")
>     print(df.nlargest(5, ["sales"])[["timestamp", "sales"]])
>     
>     # Check timestamp range
>     print(f"\nTimestamp range:")
>     print(f"  Earliest: {df['timestamp'].min()}")
>     print(f"  Latest: {df['timestamp'].max()}")
>     
>     print("="*50)
> 
> # Execute pipeline
> if __name__ == "__main__":
>     # Extract
>     raw_data = extract_parquet("sales_data.parquet")
>     
>     # Transform
>     if raw_data is not None:
>         clean_data = transform_sales_data(raw_data)
>         
>         # Validate
>         if clean_data is not None:
>             validate_transformation(clean_data)
>         else:
>             logging.error("Transformation failed")
>     else:
>         logging.error("Extraction failed")
> ```

**Q45.** Write code that demonstrates the difference between `.loc[]` and `.iloc[]` using the same DataFrame with at least 3 different filtering examples for each.

> [!success]- Answer
> **Complete Comparison:**
> 
> ```python
> import pandas as pd
> 
> # Create sample DataFrame
> data = pd.DataFrame({
>     'date': ['2023-01-01', '2023-01-02', '2023-01-03', 
>              '2023-01-04', '2023-01-05'],
>     'product': ['A', 'B', 'A', 'C', 'B'],
>     'sales': [100, 250, 150, 300, 200],
>     'region': ['North', 'South', 'North', 'East', 'South']
> })
> 
> print("Original DataFrame:")
> print(data)
> print()
> 
> print("="*60)
> print("USING .loc[] - LABEL-BASED FILTERING")
> print("="*60)
> 
> # Example 1: Filter rows by condition
> print("\n1. Rows where sales > 150 (all columns):")
> result1 = data.loc[data['sales'] > 150, :]
> print(result1)
> 
> # Example 2: Select specific columns (all rows)
> print("\n2. All rows, only product and sales columns:")
> result2 = data.loc[:, ['product', 'sales']]
> print(result2)
> 
> # Example 3: Filter rows AND columns
> print("\n3. North region, product and sales columns:")
> result3 = data.loc[data['region'] == 'North', ['product', 'sales']]
> print(result3)
> 
> # Example 4: Multiple conditions
> print("\n4. Sales > 150 AND region is South:")
> result4 = data.loc[
>     (data['sales'] > 150) & (data['region'] == 'South'),
>     ['date', 'product', 'sales']
> ]
> print(result4)
> 
> print("\n" + "="*60)
> print("USING .iloc[] - INTEGER POSITION FILTERING")
> print("="*60)
> 
> # Example 1: Select first 3 rows (all columns)
> print("\n1. First 3 rows (by position), all columns:")
> result5 = data.iloc[0:3, :]
> print(result5)
> 
> # Example 2: Select specific columns by position
> print("\n2. All rows, columns at positions 1 and 2:")
> result6 = data.iloc[:, [1, 2]]
> print(result6)
> 
> # Example 3: Specific rows and columns by position
> print("\n3. Rows 1-3, columns 0 and 2:")
> result7 = data.iloc[1:4, [0, 2]]
> print(result7)
> 
> # Example 4: Last 2 rows, first 3 columns
> print("\n4. Last 2 rows, first 3 columns:")
> result8 = data.iloc[-2:, 0:3]
> print(result8)
> 
> print("\n" + "="*60)
> print("KEY DIFFERENCES SUMMARY")
> print("="*60)
> print("""
> .loc[]:
>   - Uses column NAMES and boolean CONDITIONS
>   - More readable and explicit
>   - Example: data.loc[data['sales'] > 150, ['product']]
> 
> .iloc[]:
>   - Uses INTEGER POSITIONS
>   - Useful for positional access
>   - Example: data.iloc[0:3, [1, 2]]
> 
> When to use:
>   - Use .loc[] for most data pipeline filtering (more readable)
>   - Use .iloc[] when you need specific positions or ranges
> """)
> ```

**Q46.** Create a complete ETL pipeline that: extracts from CSV, transforms by filtering and converting datetime, loads to CSV with custom parameters, and validates the output file exists.

> [!success]- Answer
> **Complete ETL Pipeline:**
> 
> ```python
> import pandas as pd
> import os
> import logging
> 
> # Setup logging
> logging.basicConfig(
>     format='%(asctime)s - %(levelname)s: %(message)s',
>     level=logging.INFO,
>     datefmt='%Y-%m-%d %H:%M:%S'
> )
> 
> def extract(file_path):
>     """Extract data from CSV file"""
>     try:
>         logging.info(f"[EXTRACT] Reading from {file_path}")
>         data = pd.read_csv(file_path)
>         logging.info(f"[EXTRACT] Loaded {len(data)} rows, {len(data.columns)} columns")
>         logging.debug(f"[EXTRACT] Columns: {list(data.columns)}")
>         return data
>     except FileNotFoundError:
>         logging.error(f"[EXTRACT] File not found: {file_path}")
>         return None
>     except Exception as e:
>         logging.error(f"[EXTRACT] Error: {e}")
>         return None
> 
> def transform(df):
>     """Transform: filter by date and price, convert timestamp"""
>     if df is None or df.empty:
>         logging.warning("[TRANSFORM] No data to transform")
>         return None
>     
>     try:
>         logging.info("[TRANSFORM] Starting transformation")
>         original_rows = len(df)
>         
>         # Filter: Keep only rows where price > 50
>         # Select specific columns
>         transformed = df.loc[
>             df['price'] > 50,
>             ['timestamp', 'product', 'price', 'quantity']
>         ]
>         
>         logging.info(f"[TRANSFORM] Filtered: {original_rows} → {len(transformed)} rows")
>         
>         # Convert timestamp to datetime
>         # Format: "20230115" → datetime
>         transformed['timestamp'] = pd.to_datetime(
>             transformed['timestamp'],
>             format='%Y%m%d'
>         )
>         logging.info("[TRANSFORM] Converted timestamp to datetime")
>         
>         # Add calculated column
>         transformed['total_value'] = (
>             transformed['price'] * transformed['quantity']
>         )
>         logging.info("[TRANSFORM] Added total_value column")
>         
>         # Validation during transform
>         if transformed['price'].min() <= 50:
>             logging.warning("[TRANSFORM] Found prices <= 50 after filter!")
>         
>         return transformed
>         
>     except KeyError as e:
>         logging.error(f"[TRANSFORM] Column not found: {e}")
>         return None
>     except Exception as e:
>         logging.error(f"[TRANSFORM] Error: {e}")
>         return None
> 
> def load(df, output_path, separator=',', include_index=False):
>     """Load DataFrame to CSV with custom parameters"""
>     if df is None or df.empty:
>         logging.warning("[LOAD] No data to load")
>         return False
>     
>     try:
>         logging.info(f"[LOAD] Writing to {output_path}")
>         
>         # Write with custom parameters
>         df.to_csv(
>             output_path,
>             sep=separator,
>             index=include_index,
>             header=True
>         )
>         
>         logging.info(f"[LOAD] Wrote {len(df)} rows to {output_path}")
>         logging.debug(f"[LOAD] Separator: '{separator}', Index: {include_index}")
>         
>         return True
>         
>     except PermissionError:
>         logging.error(f"[LOAD] Permission denied: {output_path}")
>         return False
>     except Exception as e:
>         logging.error(f"[LOAD] Error: {e}")
>         return False
> 
> def validate_output(file_path):
>     """Validate that output file was created and verify contents"""
>     logging.info("[VALIDATE] Checking output file")
>     
>     # Check file exists
>     if os.path.exists(file_path):
>         logging.info(f"[VALIDATE] ✓ File exists: {file_path}")
>         
>         # Get file size
>         file_size = os.path.getsize(file_path)
>         logging.info(f"[VALIDATE] File size: {file_size} bytes")
>         
>         # Read back and verify
>         try:
>             verify_df = pd.read_csv(file_path)
>             logging.info(f"[VALIDATE] ✓ File readable: {len(verify_df)} rows")
>             
>             # Show sample
>             print("\n[VALIDATE] Sample of output:")
>             print(verify_df.head(3))
>             
>             return True
>         except Exception as e:
>             logging.error(f"[VALIDATE] Cannot read file: {e}")
>             return False
>     else:
>         logging.error(f"[VALIDATE] ✗ File does not exist: {file_path}")
>         return False
> 
> # ==================== Execute Pipeline ====================
> if __name__ == "__main__":
>     print("="*60)
>     print("ETL PIPELINE STARTING")
>     print("="*60)
>     
>     # Configuration
>     input_file = "raw_sales.csv"
>     output_file = "transformed_sales.csv"
>     
>     try:
>         # Step 1: Extract
>         raw_data = extract(input_file)
>         
>         if raw_data is not None:
>             # Step 2: Transform
>             clean_data = transform(raw_data)
>             
>             if clean_data is not None:
>                 # Step 3: Load
>                 success = load(
>                     clean_data,
>                     output_file,
>                     separator=',',
>                     include_index=False
>                 )
>                 
>                 if success:
>                     # Step 4: Validate
>                     validate_output(output_file)
>                     
>                     print("="*60)
>                     print("ETL PIPELINE COMPLETED SUCCESSFULLY")
>                     print("="*60)
>                 else:
>                     logging.error("Pipeline failed at LOAD stage")
>             else:
>                 logging.error("Pipeline failed at TRANSFORM stage")
>         else:
>             logging.error("Pipeline failed at EXTRACT stage")
>             
>     except Exception as e:
>         logging.critical(f"Pipeline crashed: {e}")
>         raise
> ```

**Q47.** Write a try-except block that handles a KeyError when filtering a DataFrame. If the column doesn't exist, create it with a default calculation and retry the transformation.

> [!success]- Answer
> **Solution:**
> 
> ```python
> import pandas as pd
> import logging
> 
> logging.basicConfig(format='%(levelname)s: %(message)s', level=logging.INFO)
> 
> def transform_with_price_change(df):
>     """Transform data filtering by price_change column
>     
>     If price_change doesn't exist, create it from close - open
>     """
>     try:
>         # Try to filter by price_change
>         logging.info("Attempting to filter by 'price_change' column")
>         
>         clean_data = df.loc[
>             df['price_change'] > 0,
>             ['timestamp', 'open', 'close', 'price_change']
>         ]
>         
>         logging.info(f"Successfully filtered {len(clean_data)} rows by 'price_change'")
>         return clean_data
>         
>     except KeyError as ke:
>         # Handle missing column
>         logging.warning(f"{ke}: Column 'price_change' not found in DataFrame")
>         logging.info("Creating 'price_change' column from close - open")
>         
>         # Create the missing column
>         df['price_change'] = df['close'] - df['open']
>         logging.info(f"Created 'price_change' column: min={df['price_change'].min():.2f}, max={df['price_change'].max():.2f}")
>         
>         # Retry the transformation
>         logging.info("Retrying transformation with new column")
>         clean_data = df.loc[
>             df['price_change'] > 0,
>             ['timestamp', 'open', 'close', 'price_change']
>         ]
>         
>         logging.info(f"Successfully filtered {len(clean_data)} rows after creating column")
>         return clean_data
>         
>     except Exception as e:
>         logging.error(f"Unexpected error during transformation: {e}")
>         return None
> 
> # Example usage
> if __name__ == "__main__":
>     # Create sample data WITHOUT price_change column
>     stock_data = pd.DataFrame({
>         'timestamp': ['2023-01-01', '2023-01-02', '2023-01-03'],
>         'open': [100, 105, 103],
>         'close': [102, 104, 108]
>     })
>     
>     print("Original DataFrame:")
>     print(stock_data)
>     print(f"Columns: {list(stock_data.columns)}")
>     print()
>     
>     # This will trigger KeyError and handle it
>     result = transform_with_price_change(stock_data)
>     
>     if result is not None:
>         print("\nTransformed DataFrame:")
>         print(result)
> ```
> 
> **Output:**
> ```
> Original DataFrame:
>     timestamp  open  close
> 0  2023-01-01   100    102
> 1  2023-01-02   105    104
> 2  2023-01-03   103    108
> Columns: ['timestamp', 'open', 'close']
> 
> WARNING: 'price_change': Column 'price_change' not found in DataFrame
> INFO: Creating 'price_change' column from close - open
> INFO: Created 'price_change' column: min=-1.00, max=5.00
> INFO: Retrying transformation with new column
> INFO: Successfully filtered 2 rows after creating column
> 
> Transformed DataFrame:
>     timestamp  open  close  price_change
> 0  2023-01-01   100    102           2.0
> 2  2023-01-03   103    108           5.0
> ```
> 
> **Key features:**
> - Catches specific KeyError exception
> - Logs warning with context
> - Creates missing column with logical calculation
> - Retries transformation
> - Falls back to generic exception handling

**Q48.** Create a function that reads data from multiple sources (CSV, Parquet, and SQL) using a source_type parameter. Include appropriate error handling and logging for each source type.

> [!success]- Answer
> **Complete Multi-Source Extraction Function:**
> 
> ```python
> import pandas as pd
> import sqlalchemy
> import logging
> 
> logging.basicConfig(format='%(levelname)s: %(message)s', level=logging.INFO)
> 
> def extract_from_multiple_sources(
>     source_type,
>     file_path=None,
>     connection_uri=None,
>     query=None,
>     engine_type="fastparquet"
> ):
>     """
>     Extract data from multiple source types
>     
>     :param source_type: 'csv', 'parquet', or 'sql'
>     :param file_path: path to file (for csv/parquet)
>     :param connection_uri: database URI (for sql)
>     :param query: SQL query (for sql)
>     :param engine_type: parquet engine (default: fastparquet)
>     :return: DataFrame or None if error
>     """
>     
>     logging.info(f"Extracting from {source_type.upper()} source")
>     
>     # ========== CSV Source ==========
>     if source_type.lower() == 'csv':
>         try:
>             if file_path is None:
>                 logging.error("file_path is required for CSV extraction")
>                 return None
>             
>             logging.info(f"Reading CSV file: {file_path}")
>             data = pd.read_csv(file_path)
>             logging.info(f"✓ Loaded {len(data)} rows from CSV")
>             return data
>             
>         except FileNotFoundError:
>             logging.error(f"CSV file not found: {file_path}")
>             return None
>         except pd.errors.EmptyDataError:
>             logging.error(f"CSV file is empty: {file_path}")
>             return None
>         except Exception as e:
>             logging.error(f"Error reading CSV: {e}")
>             return None
>     
>     # ========== Parquet Source ==========
>     elif source_type.lower() == 'parquet':
>         try:
>             if file_path is None:
>                 logging.error("file_path is required for Parquet extraction")
>                 return None
>             
>             logging.info(f"Reading Parquet file: {file_path}")
>             logging.debug(f"Using engine: {engine_type}")
>             data = pd.read_parquet(file_path, engine=engine_type)
>             logging.info(f"✓ Loaded {len(data)} rows from Parquet")
>             return data
>             
>         except FileNotFoundError:
>             logging.error(f"Parquet file not found: {file_path}")
>             return None
>         except ImportError as e:
>             logging.error(f"Parquet engine not installed: {e}")
>             logging.info("Try: pip install fastparquet or pip install pyarrow")
>             return None
>         except Exception as e:
>             logging.error(f"Error reading Parquet: {e}")
>             return None
>     
>     # ========== SQL Source ==========
>     elif source_type.lower() == 'sql':
>         try:
>             if connection_uri is None:
>                 logging.error("connection_uri is required for SQL extraction")
>                 return None
>             if query is None:
>                 logging.error("query is required for SQL extraction")
>                 return None
>             
>             logging.info("Connecting to database")
>             logging.debug(f"Connection URI: {connection_uri.split('@')[1]}")  # Hide credentials
>             
>             db_engine = sqlalchemy.create_engine(connection_uri)
>             
>             logging.info("Executing query")
>             logging.debug(f"Query: {query[:100]}...")  # Show first 100 chars
>             
>             data = pd.read_sql(query, db_engine)
>             logging.info(f"✓ Loaded {len(data)} rows from SQL")
>             return data
>             
>         except sqlalchemy.exc.OperationalError as e:
>             logging.error(f"Database connection failed: {e}")
>             return None
>         except sqlalchemy.exc.ProgrammingError as e:
>             logging.error(f"SQL query error: {e}")
>             return None
>         except Exception as e:
>             logging.error(f"Error reading from SQL: {e}")
>             return None
>     
>     # ========== Invalid Source Type ==========
>     else:
>         logging.error(f"Invalid source_type: '{source_type}'")
>         logging.info("Valid options: 'csv', 'parquet', 'sql'")
>         return None
> 
> # ==================== Example Usage ====================
> if __name__ == "__main__":
>     print("="*60)
>     print("MULTI-SOURCE DATA EXTRACTION")
>     print("="*60)
>     
>     # Example 1: Extract from CSV
>     print("\n1. Extracting from CSV:")
>     csv_data = extract_from_multiple_sources(
>         source_type='csv',
>         file_path='sales_data.csv'
>     )
>     if csv_data is not None:
>         print(csv_data.head())
>     
>     # Example 2: Extract from Parquet
>     print("\n2. Extracting from Parquet:")
>     parquet_data = extract_from_multiple_sources(
>         source_type='parquet',
>         file_path='stock_data.parquet',
>         engine_type='fastparquet'
>     )
>     if parquet_data is not None:
>         print(parquet_data.head())
>     
>     # Example 3: Extract from SQL
>     print("\n3. Extracting from SQL:")
>     sql_data = extract_from_multiple_sources(
>         source_type='sql',
>         connection_uri='postgresql+psycopg2://user:pass@localhost:5432/db',
>         query='SELECT * FROM transactions LIMIT 10'
>     )
>     if sql_data is not None:
>         print(sql_data.head())
>     
>     # Example 4: Error handling - invalid source
>     print("\n4. Testing error handling:")
>     invalid_data = extract_from_multiple_sources(
>         source_type='excel'  # Not supported
>     )
> ```
> 
> **Key features:**
> - Handles 3 different source types
> - Specific error handling for each source
> - Appropriate logging at each step
> - Parameter validation
> - Detailed error messages
> - Hides sensitive connection info in logs
> - Returns None on error (consistent behavior)

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (40 points)
Q1: B | Q2: B | Q3: B | Q4: C | Q5: C
Q6: A | Q7: B | Q8: B | Q9: A | Q10: B
Q11: B | Q12: B | Q13: B | Q14: C | Q15: A
Q16: B | Q17: B | Q18: B | Q19: C | Q20: B

### Section B: True/False (15 points)
Q21: F | Q22: T | Q23: T | Q24: T | Q25: F
Q26: T | Q27: T | Q28: T | Q29: T | Q30: F
Q31: T | Q32: T | Q33: T | Q34: F | Q35: T

### Section C: Short Answer (21 points)
- Each question worth 3 points
- Full credit for complete explanations with examples
- Partial credit for correct but incomplete answers

### Section D: Code Analysis (24 points)
- Each question worth 3 points
- Award points for: working code, error handling, logging, proper structure
- Partial credit for partially correct solutions

---

## Quick Study Guide

### File Formats
```python
# CSV
df = pd.read_csv("file.csv")
df.to_csv("output.csv", index=False)

# Parquet
df = pd.read_parquet("file.parquet", engine="fastparquet")
df.to_parquet("output.parquet")

# SQL
df = pd.read_sql("SELECT * FROM table", engine)
df.to_sql("table_name", con=engine)
```

### Connection URI Format
```
schema://username:password@host:port/database
postgresql+psycopg2://repl:password@localhost:5432/market
```

### Filtering Data
```python
# Label-based (.loc[])
df.loc[df["col"] > 5, :]                    # Filter rows
df.loc[:, ["col1", "col2"]]                 # Select columns
df.loc[df["col"] > 5, ["col1", "col2"]]    # Both

# Position-based (.iloc[])
df.iloc[0:10, :]           # First 10 rows
df.iloc[:, [0, 1, 2]]      # First 3 columns
df.iloc[0:10, [0, 1]]      # Rows and columns
```

### Datetime Conversion
```python
# From string with format
pd.to_datetime(df["col"], format="%Y%m%d%H%M%S")

# From Unix timestamp
pd.to_datetime(df["col"], unit="ms")
```

### Validation Methods
```python
df.head()                      # View first rows
df.nsmallest(10, ["col"])     # Smallest values
df.nlargest(10, ["col"])      # Largest values
df.dtypes                      # Check data types
```

### Logging Levels
```python
logging.debug("Detailed info")      # Development
logging.info("General info")        # Normal operation
logging.warning("Unexpected")       # Potential issues
logging.error("Error occurred")     # Errors
logging.critical("Critical fail")   # System failures
```

### Error Handling
```python
try:
    # Code that might fail
    result = risky_operation()
except SpecificError as e:
    # Handle specific error
    logging.error(f"Error: {e}")
except Exception as e:
    # Handle any other error
    logging.error(f"Unexpected: {e}")
```# Chapter 2 Practice Quiz
## Extracting Data from Structured Sources

**Total Points: 100**
**Pass Threshold: 70/100 (70%)**

---

## Section A: Multiple Choice Questions (2 points each = 40 points)

### Source Systems and File Formats

**Q1.** What is a Parquet file?

- [ ] A) A row-oriented file format
- [ ] B) An open source, column-oriented file format designed for efficient storage and retrieval
- [ ] C) A type of database
- [ ] D) A compressed JSON file

> [!success]- Answer
> **Correct Answer: B) An open source, column-oriented file format designed for efficient storage and retrieval**
> 
> Parquet files are column-oriented, making them efficient for storing and querying large datasets. They offer better compression and faster queries than row-oriented formats.

**Q2.** Which function is used to read a Parquet file in pandas?

- [ ] A) `pd.read_csv()`
- [ ] B) `pd.read_parquet()`
- [ ] C) `pd.load_parquet()`
- [ ] D) `pd.from_parquet()`

> [!success]- Answer
> **Correct Answer: B) `pd.read_parquet()`**
> 
> Similar to `pd.read_csv()`, pandas uses `pd.read_parquet()` to read Parquet files into DataFrames.
> ```python
> df = pd.read_parquet("file.parquet", engine="fastparquet")
> ```

**Q3.** What is the format of a database connection URI?

- [ ] A) `database://host/username`
- [ ] B) `schema://username:password@host:port/database`
- [ ] C) `host:port/database/username`
- [ ] D) `username@database:password`

> [!success]- Answer
> **Correct Answer: B) `schema://username:password@host:port/database`**
> 
> The connection URI follows this pattern:
> ```
> postgresql+psycopg2://repl:password@localhost:5432/market
> ```
> Components: schema, username, password, host, port, database name.

**Q4.** Which library is used to create a database engine for connecting to SQL databases?

- [ ] A) pandas
- [ ] B) numpy
- [ ] C) sqlalchemy
- [ ] D) psycopg2

> [!success]- Answer
> **Correct Answer: C) sqlalchemy**
> 
> SQLAlchemy is used to create database engines:
> ```python
> import sqlalchemy
> db_engine = sqlalchemy.create_engine(connection_uri)
> ```

### Modularity and Best Practices

**Q5.** Which of the following is NOT a benefit of modularity mentioned in the chapter?

- [ ] A) Increases readability
- [ ] B) Adheres to "don't repeat yourself"
- [ ] C) Makes code run faster
- [ ] D) Expedites troubleshooting

> [!success]- Answer
> **Correct Answer: C) Makes code run faster**
> 
> The benefits of modularity are:
> - Increases readability
> - Adheres to DRY (Don't Repeat Yourself)
> - Expedites troubleshooting
> - Separates logic into functions
> 
> Modularity is about code organization, not execution speed.

**Q6.** What principle does modular code adhere to?

- [ ] A) Don't Repeat Yourself (DRY)
- [ ] B) Keep It Simple, Stupid (KISS)
- [ ] C) You Aren't Gonna Need It (YAGNI)
- [ ] D) Single Responsibility Principle only

> [!success]- Answer
> **Correct Answer: A) Don't Repeat Yourself (DRY)**
> 
> The chapter specifically mentions that modular code adheres to the "don't repeat yourself" principle, allowing you to write code once and reuse it.

### Filtering with .loc[] and .iloc[]

**Q7.** What is the main difference between `.loc[]` and `.iloc[]`?

- [ ] A) `.loc[]` is faster than `.iloc[]`
- [ ] B) `.loc[]` uses labels/conditions; `.iloc[]` uses integer positions
- [ ] C) `.iloc[]` can filter columns but `.loc[]` cannot
- [ ] D) They are the same

> [!success]- Answer
> **Correct Answer: B) `.loc[]` uses labels/conditions; `.iloc[]` uses integer positions**
> 
> - `.loc[]`: Label-based filtering (e.g., `df.loc[df["col"] > 5, :]`)
> - `.iloc[]`: Integer position filtering (e.g., `df.iloc[0:50, [0, 1, 2]]`)

**Q8.** What does this code do: `df.loc[df["open"] > 0, :]`?

- [ ] A) Selects all columns where values are greater than 0
- [ ] B) Selects all rows where the "open" column is greater than 0, all columns
- [ ] C) Deletes rows where "open" is 0
- [ ] D) Changes all "open" values to 0

> [!success]- Answer
> **Correct Answer: B) Selects all rows where the "open" column is greater than 0, all columns**
> 
> This filters rows based on a condition (`df["open"] > 0`) and selects all columns (`:` means "all").

**Q9.** What does `.iloc[0:50, [0, 1, 2]]` do?

- [ ] A) Selects rows 0-50, columns 0-2 by integer position
- [ ] B) Selects the first column only
- [ ] C) Selects rows with index 0, 1, 2
- [ ] D) Deletes rows 0-50

> [!success]- Answer
> **Correct Answer: A) Selects rows 0-50, columns 0-2 by integer position**
> 
> `.iloc[]` uses integer indexing:
> - `0:50` = first 50 rows (0-49)
> - `[0, 1, 2]` = first 3 columns by position

**Q10.** How do you filter DataFrame rows AND columns in one step?

- [ ] A) Use two separate `.loc[]` calls
- [ ] B) `df.loc[row_condition, column_list]`
- [ ] C) `df.filter(rows, columns)`
- [ ] D) Not possible in pandas

> [!success]- Answer
> **Correct Answer: B) `df.loc[row_condition, column_list]`**
> 
> You can filter both dimensions in one step:
> ```python
> df.loc[df["open"] > 0, ["timestamps", "open", "close"]]
> ```

### Data Type Conversion

**Q11.** What function is used to convert a column to datetime type?

- [ ] A) `pd.convert_datetime()`
- [ ] B) `pd.to_datetime()`
- [ ] C) `.astype(datetime)`
- [ ] D) `pd.datetime()`

> [!success]- Answer
> **Correct Answer: B) `pd.to_datetime()`**
> 
> pandas uses `pd.to_datetime()` to convert columns to datetime:
> ```python
> df["timestamps"] = pd.to_datetime(df["timestamps"])
> ```

**Q12.** In `pd.to_datetime(df["col"], format="%Y%m%d")`, what does `%Y` represent?

- [ ] A) 2-digit year
- [ ] B) 4-digit year
- [ ] C) Month
- [ ] D) Day

> [!success]- Answer
> **Correct Answer: B) 4-digit year**
> 
> Format codes:
> - `%Y` = 4-digit year (2023)
> - `%y` = 2-digit year (23)
> - `%m` = month
> - `%d` = day

**Q13.** What parameter is used when converting Unix timestamps (in milliseconds) to datetime?

- [ ] A) `format="ms"`
- [ ] B) `unit="ms"`
- [ ] C) `type="milliseconds"`
- [ ] D) `convert="ms"`

> [!success]- Answer
> **Correct Answer: B) `unit="ms"`**
> 
> For Unix timestamps, use the `unit` parameter:
> ```python
> pd.to_datetime(df["timestamps"], unit="ms")
> ```
> Common units: "s" (seconds), "ms" (milliseconds), "us" (microseconds)

### Validation and Persistence

**Q14.** Which method returns the n smallest records from a DataFrame?

- [ ] A) `.smallest(n)`
- [ ] B) `.min(n)`
- [ ] C) `.nsmallest(n, columns)`
- [ ] D) `.bottom(n)`

> [!success]- Answer
> **Correct Answer: C) `.nsmallest(n, columns)`**
> 
> Usage:
> ```python
> df.nsmallest(10, ["timestamps"])  # 10 smallest timestamp values
> ```

**Q15.** What are two risks mentioned when transforming data?

- [ ] A) Losing information and creating faulty data
- [ ] B) Running out of memory and slow performance
- [ ] C) Security issues and data breaches
- [ ] D) Network failures and timeouts

> [!success]- Answer
> **Correct Answer: A) Losing information and creating faulty data**
> 
> The chapter specifically mentions:
> - **Losing information** - Accidentally filtering too much
> - **Creating faulty data** - Incorrect calculations or conversions

**Q16.** What parameter in `.to_csv()` determines whether the index column is written to the file?

- [ ] A) `include_index`
- [ ] B) `index`
- [ ] C) `write_index`
- [ ] D) `show_index`

> [!success]- Answer
> **Correct Answer: B) `index`**
> 
> Usage:
> ```python
> df.to_csv("file.csv", index=False)  # Don't include index
> df.to_csv("file.csv", index=True)   # Include index (default)
> ```

**Q17.** What does the `sep` parameter in `.to_csv()` specify?

- [ ] A) The file extension
- [ ] B) The column separator character
- [ ] C) The number of columns
- [ ] D) The encoding

> [!success]- Answer
> **Correct Answer: B) The column separator character**
> 
> Common separators:
> ```python
> df.to_csv("file.csv", sep=",")   # Comma (default)
> df.to_csv("file.csv", sep="|")   # Pipe
> df.to_csv("file.csv", sep="\t")  # Tab
> ```

**Q18.** Which module is used to check if a file exists?

- [ ] A) `sys`
- [ ] B) `os`
- [ ] C) `file`
- [ ] D) `path`

> [!success]- Answer
> **Correct Answer: B) `os`**
> 
> Usage:
> ```python
> import os
> file_exists = os.path.exists("file.csv")
> print(file_exists)  # True or False
> ```

### Logging and Error Handling

**Q19.** Which logging level is used for detailed diagnostic information?

- [ ] A) INFO
- [ ] B) WARNING
- [ ] C) DEBUG
- [ ] D) ERROR

> [!success]- Answer
> **Correct Answer: C) DEBUG**
> 
> Log levels from lowest to highest severity:
> - DEBUG: Detailed diagnostic info
> - INFO: General informational messages
> - WARNING: Unexpected but not breaking
> - ERROR: Error events
> - CRITICAL: Critical failures

**Q20.** What is the purpose of a try-except block?

- [ ] A) To make code run faster
- [ ] B) To execute code if errors occur and handle exceptions gracefully
- [ ] C) To log all operations
- [ ] D) To validate data types

> [!success]- Answer
> **Correct Answer: B) To execute code if errors occur and handle exceptions gracefully**
> 
> try-except allows you to:
> - Catch errors without crashing
> - Execute alternative logic
> - Log failures
> - Continue pipeline execution when possible

---

## Section B: True/False Questions (1 point each = 15 points)

**Q21.** Parquet files are row-oriented file formats. **T/F**

> [!success]- Answer
> **False** - Parquet files are column-oriented, which makes them more efficient for analytics and queries.

**Q22.** `pd.read_parquet()` requires an `engine` parameter to specify the parser. **T/F**

> [!success]- Answer
> **True** - You need to specify an engine like "fastparquet" or "pyarrow":
> ```python
> pd.read_parquet("file.parquet", engine="fastparquet")
> ```

**Q23.** A connection URI includes the username, password, host, port, and database name. **T/F**

> [!success]- Answer
> **True** - Format: `schema://username:password@host:port/database`

**Q24.** Modular code adheres to the "Don't Repeat Yourself" (DRY) principle. **T/F**

> [!success]- Answer
> **True** - Modularity allows you to write code once and reuse it, following the DRY principle.

**Q25.** `.loc[]` uses integer positions to filter DataFrames. **T/F**

> [!success]- Answer
> **False** - `.loc[]` uses labels and boolean conditions. `.iloc[]` uses integer positions.

**Q26.** `.iloc[0:50, [0, 1, 2]]` selects the first 50 rows and first 3 columns by integer position. **T/F**

> [!success]- Answer
> **True** - `.iloc[]` uses integer indexing for both rows and columns.

**Q27.** `pd.to_datetime()` can convert both string dates and Unix timestamps to datetime type. **T/F**

> [!success]- Answer
> **True** - It can handle both:
> - String dates: `pd.to_datetime(df["col"], format="%Y%m%d")`
> - Unix timestamps: `pd.to_datetime(df["col"], unit="ms")`

**Q28.** The format code `%m` in `pd.to_datetime()` represents the month. **T/F**

> [!success]- Answer
> **True** - `%m` is the format code for month (01-12).

**Q29.** `.nsmallest()` and `.nlargest()` are used to validate transformations by viewing extreme values. **T/F**

> [!success]- Answer
> **True** - These methods help validate that transformations worked correctly by showing the smallest and largest values.

**Q30.** Transforming data has no risks if done carefully. **T/F**

> [!success]- Answer
> **False** - There are always risks: losing information and creating faulty data are mentioned in the chapter.

**Q31.** The `.to_csv()` method has a `header` parameter that can be True, False, or a list of strings. **T/F**

> [!success]- Answer
> **True** - `header` can be:
> - `True`: Include column names
> - `False`: Exclude column names  
> - list: Use custom column names

**Q32.** The `sep` parameter in `.to_csv()` is commonly set to `"|"` (pipe character). **T/F**

> [!success]- Answer
> **True** - While comma is default, pipe (`"|"`) is a common alternative separator.

**Q33.** `os.path.exists()` returns True if a file or directory exists. **T/F**

> [!success]- Answer
> **True** - This function checks if the specified path exists on the filesystem.

**Q34.** logging.INFO is more severe than logging.WARNING. **T/F**

> [!success]- Answer
> **False** - WARNING is more severe than INFO. Order: DEBUG < INFO < WARNING < ERROR < CRITICAL

**Q35.** Specific exception types (like KeyError) allow for more targeted error handling than generic exceptions. **T/F**

> [!success]- Answer
> **True** - Catching specific exceptions allows you to handle different errors differently and take appropriate actions.

---

## Section C: Short Answer Questions (3 points each = 21 points)

**Q36.** Explain what Parquet files are and why they might be preferred over CSV files for large datasets.

> [!success]- Answer
> **What are Parquet files:**
> Parquet files are open source, column-oriented file formats designed for efficient field storage and retrieval.
> 
> **Key characteristics:**
> - **Column-oriented** - Data stored by column, not by row
> - **Compressed** - Better compression than CSV
> - **Efficient** - Faster queries and reads
> 
> **Why prefer over CSV for large datasets:**
> 
> 1. **Better compression** - Smaller file sizes save storage space
> 2. **Faster queries** - Column orientation allows reading only needed columns
> 3. **Type preservation** - Maintains data types (CSV stores everything as strings)
> 4. **Optimized for analytics** - Designed for analytical workloads
> 5. **Better performance** - Especially when selecting specific columns from wide tables
> 
> **Example:**
> If you have a table with 100 columns but only need 3, Parquet only reads those 3 columns, while CSV must read all 100.
> 
> **Usage in pandas:**
> ```python
> # Read Parquet
> df = pd.read_parquet("data.parquet", engine="fastparquet")
> 
> # Write Parquet
> df.to_parquet("output.parquet")
> ```

**Q37.** Describe the components of a database connection URI and provide an example with explanation of each part.

> [!success]- Answer
> **Connection URI Format:**
> ```
> schema_identifier://username:password@host:port/database
> ```
> 
> **Components:**
> 
> 1. **Schema identifier** - Database type and driver
>    - Example: `postgresql+psycopg2`
>    - Format: `database_type+driver`
> 
> 2. **Username** - Database user account
>    - Example: `repl`
>    - Used for authentication
> 
> 3. **Password** - User's password
>    - Example: `password`
>    - Sensitive credential
> 
> 4. **Host** - Server address
>    - Example: `localhost` or `192.168.1.100`
>    - Where database is hosted
> 
> 5. **Port** - Connection port number
>    - Example: `5432` (PostgreSQL default)
>    - Different databases use different default ports
> 
> 6. **Database** - Specific database name
>    - Example: `market`
>    - Which database to connect to
> 
> **Complete example:**
> ```python
> connection_uri = "postgresql+psycopg2://repl:password@localhost:5432/market"
> ```
> 
> Breakdown:
> - `postgresql+psycopg2`: PostgreSQL with psycopg2 driver
> - `repl`: username
> - `password`: password
> - `localhost`: server on same machine
> - `5432`: PostgreSQL default port
> - `market`: database name

**Q38.** Explain the benefits of modularity in data pipelines. How does it improve code quality and maintainability?

> [!success]- Answer
> **Benefits of modularity:**
> 
> **1. Increases readability**
> - Clear, focused functions
> - Easy to understand what each part does
> - Self-documenting code structure
> 
> **2. Don't Repeat Yourself (DRY)**
> - Write code once, use many times
> - Reduces duplication
> - Single source of truth
> 
> **3. Expedites troubleshooting**
> - Isolate issues to specific functions
> - Test components independently
> - Debug one piece at a time
> 
> **4. Separates logic**
> - Each function has one responsibility
> - Easier to modify individual steps
> - Changes don't affect other parts
> 
> **Example of modular vs non-modular:**
> 
> **Non-modular (bad):**
> ```python
> # Everything in one place
> data = pd.read_sql("SELECT * FROM table", engine)
> data = data.loc[data["price"] > 0, ["date", "price"]]
> data.to_csv("output.csv")
> ```
> 
> **Modular (good):**
> ```python
> def extract_from_sql(uri, query):
>     engine = sqlalchemy.create_engine(uri)
>     return pd.read_sql(query, engine)
> 
> def transform(df):
>     return df.loc[df["price"] > 0, ["date", "price"]]
> 
> def load(df, path):
>     df.to_csv(path, index=False)
> 
> # Clear pipeline
> data = extract_from_sql(uri, query)
> clean = transform(data)
> load(clean, "output.csv")
> ```
> 
> **Maintainability improvements:**
> - Easy to add logging to each function
> - Can reuse `extract_from_sql` for different queries
> - Can test `transform` with sample data
> - Changes to extraction don't affect transformation

**Q39.** Compare `.loc[]` and `.iloc[]`. When would you use each, and what are the syntax differences?

> [!success]- Answer
> **`.loc[]` - Label-based filtering:**
> 
> **Characteristics:**
> - Uses column names and boolean conditions
> - Label-based indexing
> - More readable and explicit
> 
> **Syntax:**
> ```python
> df.loc[row_condition, column_labels]
> df.loc[df["price"] > 100, ["date", "price"]]
> df.loc[:, ["col1", "col2"]]  # All rows, specific columns
> ```
> 
> **When to use:**
> - Filtering based on conditions
> - Selecting columns by name
> - Most common in data pipelines
> - When readability matters
> 
> **`.iloc[]` - Integer position filtering:**
> 
> **Characteristics:**
> - Uses integer positions
> - Position-based indexing
> - Independent of column names
> 
> **Syntax:**
> ```python
> df.iloc[row_positions, column_positions]
> df.iloc[0:50, [0, 1, 2]]  # First 50 rows, first 3 columns
> df.iloc[:10, :]           # First 10 rows, all columns
> ```
> 
> **When to use:**
> - Need first/last N rows
> - Column positions known
> - Column names might change
> - Working with positional data
> 
> **Comparison table:**
> 
> | Feature | `.loc[]` | `.iloc[]` |
> |---------|----------|-----------|
> | Uses | Labels/conditions | Integer positions |
> | Row filter | `df["col"] > 5` | `0:10` |
> | Column filter | `["col1", "col2"]` | `[0, 1, 2]` |
> | Readability | High | Lower |
> | Common use | Most pipelines | Positional access |

**Q40.** Describe the process of converting different datetime formats using `pd.to_datetime()`. Provide examples for both string formats and Unix timestamps.

> [!success]- Answer
> **`pd.to_datetime()` converts various formats to datetime type**
> 
> **Method 1: String dates with format codes**
> 
> When dates are stored as strings in a specific format:
> 
> ```python
> # Example: "20230101085731" → 2023-01-01 08:57:31
> df["timestamps"] = pd.to_datetime(
>     df["timestamps"],
>     format="%Y%m%d%H%M%S"
> )
> ```
> 
> **Common format codes:**
> - `%Y` = 4-digit year (2023)
> - `%m` = month (01-12)
> - `%d` = day (01-31)
> - `%H` = hour (00-23)
> - `%M` = minute (00-59)
> - `%S` = second (00-59)
> 
> **More examples:**
> ```python
> # "2023-01-15"
> pd.to_datetime(df["date"], format="%Y-%m-%d")
> 
> # "01/15/2023 14:30"
> pd.to_datetime(df["datetime"], format="%m/%d/%Y %H:%M")
> ```
> 
> **Method 2: Unix timestamps**
> 
> When dates are stored as numbers (seconds/milliseconds since epoch):
> 
> ```python
> # Example: 1681596000011 → 2023-04-15 22:00:00.011
> df["timestamps"] = pd.to_datetime(
>     df["timestamps"],
>     unit="ms"  # milliseconds
> )
> ```
> 
> **Common units:**
> - `unit="s"` - seconds since epoch
> - `unit="ms"` - milliseconds since epoch
> - `unit="us"` - microseconds since epoch
> - `unit="ns"` - nanoseconds since epoch
> 
> **Why convert:**
> 1. Enable date operations (sorting, filtering by date)
> 2. Extract components (year, month, day)
> 3. Calculate time differences
> 4. Proper formatting for output
> 
> **Validation:**
> ```python
> # Check conversion worked
> print(df["timestamps"].dtype)  # Should show datetime64
> print(df["timestamps"].head())  # Verify format
> ```

**Q41.** What are the risks of data transformation, and how can you validate that transformations were performed correctly?

> [!success]- Answer
> **Risks of transformation:**
> 
> **1. Losing information**
> - Filtering too aggressively
> - Dropping important records
> - Losing precision in calculations
> - Example: `df[df["price"] > 100]` might exclude valid low-price items
> 
> **2. Creating faulty data**
> - Incorrect calculations
> - Wrong data type conversions
> - Misaligned data after joins
> - Example: Converting "20230132" (invalid date) without validation
> 
> **Validation methods:**
> 
> **1. Use `.head()` to inspect results**
> ```python
> transformed = df.loc[df["open"] > 0, ["timestamps", "open", "close"]]
> print(transformed.head())
> # Check: Do the values look correct?
> ```
> 
> **2. Check extreme values with `.nsmallest()` and `.nlargest()`**
> ```python
> # Check smallest timestamps
> print(transformed.nsmallest(10, ["timestamps"]))
> # Check largest timestamps
> print(transformed.nlargest(10, ["timestamps"]))
> # Verify: Are date ranges correct?
> ```
> 
> **3. Compare before and after counts**
> ```python
> print(f"Before: {len(raw_data)} rows")
> print(f"After: {len(transformed)} rows")
> print(f"Removed: {len(raw_data) - len(transformed)} rows")
> ```
> 
> **4. Check data types**
> ```python
> print(transformed.dtypes)  # Verify datetime conversions
> ```
> 
> **5. Statistical validation**
> ```python
> print(transformed.describe())  # Check min, max, mean
> ```
> 
> **6. Check for nulls**
> ```python
> print(transformed.isnull().sum())  # Any unexpected nulls?
> ```
> 
> **Best practice workflow:**
> ```python
> # Transform
> transformed = transform(raw_data)
> 
> # Validate
> print("="*50)
> print("VALIDATION")
> print("="*50)
> print(f"Rows: {len(raw_data)} → {len(transformed)}")
> print("\nFirst records:")
> print(transformed.head())
> print("\nSmallest values:")
> print(transformed.nsmallest(5, ["price"]))
> print("\nData types:")
> print(transformed.dtypes)
> ```

**Q42.** Explain the purpose of logging in data pipelines and describe the different log levels with examples of when to use each.

> [!success]- Answer
> **Purpose of logging:**
> 
> **1. Document performance at execution**
> - Track what happens at each step
> - Record timing and data volumes
> - Create audit trail
> 
> **2. Provides starting point when solution fails**
> - See exactly where failure occurred
> - Understand state before failure
> - Debug faster
> 
> **3. Monitor pipeline health**
> - Detect unusual patterns
> - Track data quality issues
> - Alert on problems
> 
> **Log levels (lowest to highest severity):**
> 
> **1. DEBUG** - Detailed diagnostic information
> ```python
> logging.debug(f"Variable has value: {path}")
> logging.debug(f"Processing {len(df)} rows")
> ```
> **When to use:**
> - Development and debugging
> - Detailed troubleshooting
> - Variable values and states
> 
> **2. INFO** - General informational messages
> ```python
> logging.info("Data has been transformed and will now be loaded")
> logging.info(f"Successfully extracted {len(df)} rows from database")
> ```
> **When to use:**
> - Normal operation milestones
> - Pipeline progress updates
> - Successful completions
> 
> **3. WARNING** - Unexpected but not breaking
> ```python
> logging.warning("Unexpected number of rows detected")
> logging.warning(f"Column 'price_change' not found, will create it")
> ```
> **When to use:**
> - Unusual situations
> - Potential issues
> - Degraded functionality
> - Still works but might need attention
> 
> **4. ERROR** - Error events
> ```python
> logging.error(f"{ke} arose in execution")
> logging.error("Failed to connect to database")
> ```
> **When to use:**
> - Errors that occurred
> - Failed operations
> - Issues that need fixing
> - Might still continue execution
> 
> **5. CRITICAL** - Critical failures
> ```python
> logging.critical("Pipeline completely failed")
> logging.critical("Database is unreachable")
> ```
> **When to use:**
> - System failures
> - Cannot continue execution
> - Immediate attention required
> 
> **Complete example:**
> ```python
> import logging
> 
> logging.basicConfig(
>     format='%(levelname)s: %(message)s',
>     level=logging.DEBUG
> )
> 
> def etl_pipeline():
>     logging.info("Starting ETL pipeline")
>     
>     try:
>         logging.debug("Connecting to database")
>         data = extract()
>         logging.info(f"Extracted {len(data)} rows")
>         
>         logging.debug("Beginning transformation")
>         transformed = transform(data)
>         logging.info("Transformation complete")
>         
>         if len(transformed) < 100:
>             logging.warning(f"Only {len(transformed)} rows after transformation")
>         
>         load(transformed)
>         logging.info("Pipeline completed successfully")
>         
>     except KeyError as e:
>         logging.error(f"Column not found: {e}")
>     except Exception as e:
>         logging.critical(f"Pipeline failed: {e}")