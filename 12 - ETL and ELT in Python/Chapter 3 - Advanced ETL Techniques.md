## Non-Tabular Data

### Definition

Most data produced and consumed is **unstructured data** (non-tabular format).

### Types of Non-Tabular Data

| Type        | Description                    | Examples                      |
| ----------- | ------------------------------ | ----------------------------- |
| **Text**    | Unstructured text documents    | Articles, logs, emails        |
| **Audio**   | Sound recordings               | Music, podcasts, calls        |
| **Image**   | Visual data                    | Photos, diagrams, scans       |
| **Video**   | Moving images                  | Movies, surveillance, streams |
| **Spatial** | Geographic/location data       | GPS, maps, coordinates        |
| **IoT**     | Internet of Things sensor data | Smart devices, sensors        |

---

## Working with APIs and JSON Data

### API (Application Programming Interface)

**Definition:** Software that sits on top of data sources

**Purpose:**
- Prevents direct interaction with database
- Provides controlled access to data
- Standardizes data retrieval

### JSON (JavaScript Object Notation)

**Characteristics:**
- **Key-value pairs** - Data stored as keys and values
- **No set schema** - Flexible structure
- **Similar to dictionaries** - Looks and works like Python dicts

**Example:**
```json
{
  "key": "value",
  "open": 0.121875,
  "close": 0.097917
}
```

---

## Reading JSON Files with pandas

### Simple JSON Structure

**Example JSON file:**
```json
{
  "timestamps": [863703000, 863789400, ...],
  "open": [0.121875, 0.098438, ...],
  "close": [...],
  "volume": [...]
}
```

### Using `pd.read_json()`

```python
# Read in a JSON file
raw_stock_data = pd.read_json(
    "raw_stock_data.json",
    orient="columns"
)
```

### `orient` Parameter

Specifies the expected JSON format:
- `"columns"` - JSON object with column names as keys
- `"records"` - List of dicts (one dict per row)
- `"index"` - Dict with index labels as keys
- `"values"` - Just the values array

---

## Nested or Unstructured JSON Data

### Challenge

**Data is not always DataFrame-ready:**
- Nested objects
- Varying "schema"
- Complex structures

### Example of Nested JSON

```json
{
  "863703000": {
    "volume": 1443120000,
    "price": {
      "close": 0.09791,
      "open": 0.12187
    }
  },
  "863789400": {
    ...
  }
}
```

This structure cannot be directly read by `pd.read_json()` with simple parameters.

---

## Reading JSON with the `json` Module

### For Complex/Nested JSON

```python
import json

# Open and read JSON file
with open("raw_stock_data.json", "r") as file:
    # Load the file into a dictionary
    raw_stock_data = json.load(file)

# Confirm the type
print(type(raw_stock_data))
```

**Output:** `<class 'dict'>`

### Why Use `json` Module?

- More control over parsing
- Works with any JSON structure
- Returns native Python dict
- Can handle nested structures

---

## Transforming Non-Tabular Data

### Goal

Convert nested dictionary into DataFrame-ready format:

**From:**
```json
{
  "863703000": {
    "price": {"open": 0.12187, "close": 0.09791},
    "volume": 1443120000
  }
}
```

**To:**
```python
[
  [863703000, 0.12187, 0.09791, 1443120000],
  [863789400, 0.09843, ...]
]
```

---

## Storing Data in Dictionaries

### Dictionary Methods for Iteration

#### 1. `.keys()` - Loop over keys

```python
for key in raw_data.keys():
    print(key)  # Prints each key
```

**Returns:** List of keys stored in dictionary

#### 2. `.values()` - Loop over values

```python
for value in raw_data.values():
    print(value)  # Prints each value
```

**Returns:** List of values stored in dictionary

#### 3. `.items()` - Loop over key-value pairs

```python
for key, value in raw_data.items():
    print(f"{key}: {value}")
```

**Returns:** List of tuples made up of key-value pairs

---

## Parsing Data from Dictionaries

### Using `.get()` Method

```python
entry = {
    "volume": 1443120000,
    "price": {
        "open": 0.12187,
        "close": 0.09791
    }
}

# Parse data using .get()
volume = entry.get("volume")

# .get() with default value
ticker = entry.get("ticker", "DCMP")  # Returns "DCMP" if "ticker" doesn't exist

# Chain .get() for nested values
open_price = entry.get("price").get("open", 0)
```

### Why Use `.get()`?

- **Safe access** - Doesn't raise KeyError if key missing
- **Default values** - Can specify fallback value
- **Chaining** - Can access nested dictionaries

---

## Creating a DataFrame from a List of Lists

### Step 1: Pass List of Lists

```python
# Pass a list of lists to pd.DataFrame
raw_data = pd.DataFrame(flattened_rows)
```

### Step 2: Set Column Headers

```python
# Create columns
raw_data.columns = ["timestamps", "open", "close", "volume"]
```

### Step 3: Set Index

```python
# Set the index column to be "timestamps"
raw_data = raw_data.set_index("timestamps")
```

---

## Complete Transformation Example

### Transforming Stock Data

```python
parsed_stock_data = []

# Loop through each key-value pair
for timestamp, ticker_info in raw_stock_data.items():
    parsed_stock_data.append([
        timestamp,
        # Parse the opening price
        ticker_info.get("price", {}).get("open", 0),
        # Parse the closing price
        ticker_info.get("price", {}).get("close", 0),
        # Parse the volume
        ticker_info.get("volume", 0)
    ])

# Create a DataFrame
transformed_stock_data = pd.DataFrame(parsed_stock_data)

# Assign column names
transformed_stock_data.columns = ["timestamps", "open", "close", "volume"]

# Set an index
transformed_stock_data = transformed_stock_data.set_index("timestamps")
```

---

## Advanced Data Transformation with pandas

### Filling Missing Values

#### Method 1: Fill All NaN with Same Value

**Before:**
```
timestamps              volume      open      close
1997-05-15 13:30:00  1443120000  0.121875  0.097917
1997-05-16 13:30:00   294000000       NaN  0.086458
1997-05-19 13:30:00   122136000  0.088021       NaN
```

```python
# Fill all NaN with value 0
clean_stock_data = raw_stock_data.fillna(value=0)
```

**After:**
```
timestamps              volume      open      close
1997-05-15 13:30:00  1443120000  0.121875  0.097917
1997-05-16 13:30:00   294000000  0.000000  0.086458
1997-05-19 13:30:00   122136000  0.088021  0.000000
```

#### Method 2: Fill with Column-Specific Values

```python
# Fill NaN values with specific value for each column
clean_stock_data = raw_stock_data.fillna(
    value={"open": 0, "close": 0.5},
    axis=1
)
```

**Result:**
```
timestamps              volume      open      close
1997-05-15 13:30:00  1443120000  0.121875  0.097917
1997-05-16 13:30:00   294000000  0.000000  0.086458
1997-05-19 13:30:00   122136000  0.088021  0.500000
```

#### Method 3: Fill Using Other Columns

```python
# Fill NaN value using other columns
raw_stock_data["open"].fillna(
    raw_stock_data["close"],
    inplace=True
)
```

**Result:**
```
timestamps              volume      open      close
1997-05-15 13:30:00  1443120000  0.121875  0.097917
1997-05-16 13:30:00   294000000  0.086458  0.086458  # open filled from close
1997-05-19 13:30:00   122136000  0.088021       NaN
```

---

## Grouping Data with pandas

### SQL GROUP BY Equivalent

**SQL:**
```sql
SELECT
    ticker,
    AVG(volume),
    AVG(open),
    AVG(close)
FROM raw_stock_data
GROUP BY ticker;
```

**pandas:**
```python
# Group data by ticker, find the mean
grouped_stock_data = raw_stock_data.groupby(
    by=["ticker"],
    axis=0
).mean()
```

### Example

**Before:**
```
ticker      volume        open      close
AAPL    1443120000    0.121875  0.097917
AAPL     297000000    0.098146  0.086458
AMZN     124186000    0.247511  0.251290
```

**After:**
```
               volume          open        close
ticker
AAPL     1.149287e+08    34.998377   34.986851
AMZN     1.434213e+08    30.844692   30.830233
```

### Other Aggregation Functions

```python
# Can use different aggregation functions
grouped_stock_data = raw_stock_data.groupby(["ticker"]).min()
grouped_stock_data = raw_stock_data.groupby(["ticker"]).max()
grouped_stock_data = raw_stock_data.groupby(["ticker"]).sum()
```

---

## Applying Advanced Transformations

### Using `.apply()` Method

**Purpose:** Handle more advanced transformations with custom functions

### Example: Classify Price Change

```python
def classify_change(row):
    """Classify if price increased or decreased"""
    change = row["close"] - row["open"]
    if change > 0:
        return "Increase"
    else:
        return "Decrease"

# Apply transformation to DataFrame
raw_stock_data["change"] = raw_stock_data.apply(
    classify_change,
    axis=1  # Apply to rows
)
```

### Before Transformation

```
ticker    open      close
AAPL    0.121875  0.097917
AAPL    0.098146  0.086458
AMZN    0.247511  0.251290
```

### After Transformation

```
ticker    open      close    change
AAPL    0.121875  0.097917  Decrease
AAPL    0.098146  0.086458  Decrease
AMZN    0.247511  0.251290  Increase
```

### How `.apply()` Works

- Takes a function as argument
- `axis=1` applies function to each row
- `axis=0` applies function to each column
- Function receives row/column as parameter

---

## Loading Data to SQL Database

### Using `.to_sql()` Method

pandas provides `.to_sql()` to persist data to SQL databases

### Key Parameters

| Parameter | Purpose | Example |
|-----------|---------|---------|
| `name` | Table name | `"filtered_stock_data"` |
| `con` | Database connection | `db_engine` |
| `if_exists` | What to do if table exists | `"append"`, `"replace"`, `"fail"` |
| `index` | Include index | `True` or `False` |
| `index_label` | Name for index column | `"timestamps"` |

---

## Persisting Data to Postgres

### Complete Example

```python
import sqlalchemy
import pandas as pd

# Create a connection object
connection_uri = "postgresql+psycopg2://repl:password@localhost:5432/market"
db_engine = sqlalchemy.create_engine(connection_uri)

# Use .to_sql() to persist data
clean_stock_data.to_sql(
    name="filtered_stock_data",
    con=db_engine,
    if_exists="append",
    index=True,
    index_label="timestamps"
)
```

### `if_exists` Options

| Value | Behavior |
|-------|----------|
| `"fail"` | Raise error if table exists (default) |
| `"replace"` | Drop table and create new one |
| `"append"` | Add data to existing table |

---

## Validating Data Persistence

### Why Validate?

Important to ensure data is persisted as expected:
- Ensure data can be queried
- Make sure counts match
- Validate that each row is present

### Validation Example

```python
# Pull data written to SQL table
to_validate = pd.read_sql(
    "SELECT * FROM cleaned_stock_data",
    db_engine
)

# Validate counts
assert len(to_validate) == len(clean_stock_data), "Row count mismatch!"

# Validate specific records
print("Sample of persisted data:")
print(to_validate.head())

# Check for nulls
print("\nNull values:")
print(to_validate.isnull().sum())
```

---

## Quick Reference

### Reading JSON

```python
# Simple JSON with pandas
df = pd.read_json("file.json", orient="columns")

# Complex/nested JSON with json module
import json
with open("file.json", "r") as f:
    data = json.load(f)
```

### Dictionary Iteration

```python
for key in dict.keys():           # Keys only
for value in dict.values():       # Values only
for key, value in dict.items():   # Key-value pairs
```

### Safe Dictionary Access

```python
value = dict.get("key", default)           # Single level
value = dict.get("key", {}).get("nested")  # Nested
```

### Creating DataFrames

```python
df = pd.DataFrame(list_of_lists)
df.columns = ["col1", "col2"]
df = df.set_index("col1")
```

### Filling Missing Values

```python
df.fillna(0)                           # All NaN → 0
df.fillna({"col1": 0, "col2": 1})     # Column-specific
df["col1"].fillna(df["col2"])          # From another column
```

### Grouping and Aggregating

```python
df.groupby(["col"]).mean()     # Average
df.groupby(["col"]).sum()      # Sum
df.groupby(["col"]).min()      # Minimum
df.groupby(["col"]).max()      # Maximum
```

### Applying Functions

```python
df["new_col"] = df.apply(function, axis=1)  # Apply to rows
df["new_col"] = df.apply(function, axis=0)  # Apply to columns
```

### Writing to SQL

```python
df.to_sql(
    name="table_name",
    con=engine,
    if_exists="append",
    index=False
)
```

---

## Key Concepts Summary

| Concept | Purpose | Example |
|---------|---------|---------|
| **Non-tabular data** | Unstructured data types | Text, audio, images, IoT |
| **API** | Interface to data sources | Controlled data access |
| **JSON** | Key-value data format | Similar to Python dict |
| `pd.read_json()` | Read simple JSON | `orient="columns"` |
| `json.load()` | Read complex JSON | Returns dict |
| `.get()` | Safe dict access | Default values |
| `.items()` | Iterate key-values | For loops |
| `.fillna()` | Fill missing values | Handle NaN |
| `.groupby()` | Aggregate data | Like SQL GROUP BY |
| `.apply()` | Custom transformations | Apply functions |
| `.to_sql()` | Write to database | Persist data |

---

#etl #json #non-tabular-data #apis #dictionaries #fillna #groupby #apply #to-sql #nested-data #data-transformation #pandas #datacamp #chapter3

**Example 3: Risk categorization**
> 
> ```python
> def categorize_risk(row):
>     """Categorize stock risk based on volume and volatility"""
>     volatility = abs(row["close"] - row["open"])
>     
>     if row["volume"] > 1000000000 and volatility > 0.05:
>         return "High Risk"
>     elif row["volume"] > 500000000:
>         return "Medium Risk"
>     else:
>         return "Low Risk"
> 
> stock_data["risk"] = stock_data.apply(categorize_risk, axis=1)
> ```
> 
> **How `.apply()` works:**
> 1. Function receives each row as a Series
> 2. Can access columns by name: `row["column_name"]`
> 3. Return value becomes the new column value
> 4. Applied to every row in DataFrame
> 
> **When to use `.apply()`:**
> - Complex logic that can't be done with simple operations
> - Conditional transformations
> - Calculations involving multiple columns
> - Creating categories or classifications
> 
> **Alternative for simple operations:**
> ```python
> # Simple calculation - don't need .apply()
> df["total"] = df["price"] * df["quantity"]
> 
> # Complex logic - use .apply()
> df["category"] = df.apply(complex_function, axis=1)
> ```

---

## Section D: Code Analysis & Scenarios (3 points each = 24 points)

**Q43.** Write code that reads a nested JSON file using the `json` module, transforms it into a list of lists, and creates a DataFrame with proper column names and index.

> [!success]- Answer
> **Complete Solution:**
> 
> ```python
> import json
> import pandas as pd
> 
> # Read nested JSON file
> with open("nested_stock_data.json", "r") as file:
>     raw_data = json.load(file)
> 
> print(f"Loaded data type: {type(raw_data)}")
> print(f"Number of records: {len(raw_data)}")
> 
> # Transform nested dictionary to list of lists
> parsed_data = []
> 
> # Iterate through key-value pairs
> for timestamp, ticker_info in raw_data.items():
>     # Parse nested values safely with .get()
>     parsed_data.append([
>         timestamp,
>         ticker_info.get("ticker", "UNKNOWN"),
>         ticker_info.get("price", {}).get("open", 0),
>         ticker_info.get("price", {}).get("close", 0),
>         ticker_info.get("volume", 0)
>     ])
> 
> print(f"\nParsed {len(parsed_data)} records")
> print("Sample parsed record:", parsed_data[0])
> 
> # Create DataFrame from list of lists
> stock_df = pd.DataFrame(parsed_data)
> 
> # Set column names
> stock_df.columns = ["timestamp", "ticker", "open", "close", "volume"]
> 
> # Set timestamp as index
> stock_df = stock_df.set_index("timestamp")
> 
> # Display results
> print("\nDataFrame created:")
> print(stock_df.head())
> print("\nData types:")
> print(stock_df.dtypes)
> ```
> 
> **Example nested JSON structure:**
> ```json
> {
>   "863703000": {
>     "ticker": "AAPL",
>     "price": {
>       "open": 0.12187,
>       "close": 0.09791
>     },
>     "volume": 1443120000
>   },
>   "863789400": {
>     "ticker": "AAPL",
>     "price": {
>       "open": 0.09843,
>       "close": 0.08645
>     },
>     "volume": 294000000
>   }
> }
> ```

**Q44.** Write code that fills missing values in a DataFrame using all three methods: same value for all, column-specific values, and values from another column.

> [!success]- Answer
> **Complete Solution:**
> 
> ```python
> import pandas as pd
> import numpy as np
> 
> # Create sample DataFrame with missing values
> data = {
>     'date': ['2023-01-01', '2023-01-02', '2023-01-03', '2023-01-04'],
>     'open': [100.0, np.nan, 105.0, np.nan],
>     'close': [102.0, 103.0, np.nan, 108.0],
>     'volume': [1000000, 950000, np.nan, 1200000]
> }
> 
> df = pd.DataFrame(data)
> 
> print("Original DataFrame with NaN values:")
> print(df)
> print("\nNull counts:")
> print(df.isnull().sum())
> 
> # ========== Method 1: Fill all NaN with same value ==========
> print("\n" + "="*50)
> print("METHOD 1: Fill all NaN with 0")
> print("="*50)
> 
> df_method1 = df.copy()
> df_method1 = df_method1.fillna(value=0)
> 
> print(df_method1)
> 
> # ========== Method 2: Column-specific fill values ==========
> print("\n" + "="*50)
> print("METHOD 2: Column-specific fill values")
> print("="*50)
> 
> df_method2 = df.copy()
> df_method2 = df_method2.fillna(
>     value={
>         'open': 0,           # Fill open with 0
>         'close': 999,        # Fill close with 999
>         'volume': -1         # Fill volume with -1 (indicates missing)
>     },
>     axis=1
> )
> 
> print(df_method2)
> 
> # ========== Method 3: Fill from another column ==========
> print("\n" + "="*50)
> print("METHOD 3: Fill open from close (if available)")
> print("="*50)
> 
> df_method3 = df.copy()
> # Fill missing open prices with close prices
> df_method3['open'].fillna(df_method3['close'], inplace=True)
> 
> print(df_method3)
> print("\nNote: If close is also NaN, open remains NaN")
> 
> # ========== Combined approach (realistic scenario) ==========
> print("\n" + "="*50)
> print("REALISTIC: Combined approach")
> print("="*50)
> 
> df_realistic = df.copy()
> 
> # Step 1: Fill open with close if missing
> df_realistic['open'].fillna(df_realistic['close'], inplace=True)
> 
> # Step 2: Fill remaining NaN with column-specific defaults
> df_realistic = df_realistic.fillna(
>     value={'open': 0, 'close': 0, 'volume': 0}
> )
> 
> print(df_realistic)
> print("\n✓ All NaN values handled")
> ```

**Q45.** Write code using `.groupby()` to calculate summary statistics (mean, min, max) for stock data grouped by ticker.

> [!success]- Answer
> **Complete Solution:**
> 
> ```python
> import pandas as pd
> 
> # Create sample stock data
> stock_data = pd.DataFrame({
>     'ticker': ['AAPL', 'AAPL', 'AAPL', 'GOOGL', 'GOOGL', 'GOOGL', 
>                'MSFT', 'MSFT', 'MSFT'],
>     'date': ['2023-01-01', '2023-01-02', '2023-01-03'] * 3,
>     'open': [150.0, 152.5, 151.0, 2800.0, 2850.0, 2825.0, 
>              300.0, 305.0, 302.5],
>     'close': [152.0, 151.0, 153.5, 2850.0, 2825.0, 2875.0, 
>               305.0, 302.0, 308.0],
>     'volume': [1000000, 950000, 1100000, 500000, 480000, 520000,
>                800000, 750000, 820000]
> })
> 
> print("Original Stock Data:")
> print(stock_data)
> print(f"\nTotal records: {len(stock_data)}")
> 
> # ========== Method 1: Single aggregation ==========
> print("\n" + "="*60)
> print("METHOD 1: Calculate mean by ticker")
> print("="*60)
> 
> grouped_mean = stock_data.groupby(['ticker']).mean()
> print(grouped_mean)
> 
> # ========== Method 2: Different aggregations ==========
> print("\n" + "="*60)
> print("METHOD 2: Min, Max, Sum")
> print("="*60)
> 
> print("\nMinimum values:")
> print(stock_data.groupby(['ticker']).min())
> 
> print("\nMaximum values:")
> print(stock_data.groupby(['ticker']).max())
> 
> print("\nSum:")
> print(stock_data.groupby(['ticker']).sum())
> 
> # ========== Method 3: Multiple aggregations at once ==========
> print("\n" + "="*60)
> print("METHOD 3: Multiple aggregations simultaneously")
> print("="*60)
> 
> multi_agg = stock_data.groupby(['ticker']).agg({
>     'open': ['mean', 'min', 'max'],
>     'close': ['mean', 'min', 'max'],
>     'volume': ['mean', 'sum']
> })
> 
> print(multi_agg)
> 
> # ========== Method 4: Named aggregations ==========
> print("\n" + "="*60)
> print("METHOD 4: Named aggregations (cleaner output)")
> print("="*60)
> 
> summary = stock_data.groupby(['ticker']).agg(
>     avg_open=('open', 'mean'),
>     min_open=('open', 'min'),
>     max_open=('open', 'max'),
>     avg_close=('close', 'mean'),
>     total_volume=('volume', 'sum'),
>     trade_count=('volume', 'count')
> ).round(2)
> 
> print(summary)
> 
> # ========== SQL equivalent ==========
> print("\n" + "="*60)
> print("SQL EQUIVALENT")
> print("="*60)
> print("""
> SELECT
>     ticker,
>     AVG(open) as avg_open,
>     MIN(open) as min_open,
>     MAX(open) as max_open,
>     AVG(close) as avg_close,
>     SUM(volume) as total_volume,
>     COUNT(*) as trade_count
> FROM stock_data
> GROUP BY ticker;
> """)
> ```

**Q46.** Create a custom function and use `.apply()` to add a new column that categorizes stocks as "High Volume", "Medium Volume", or "Low Volume" based on volume thresholds.

> [!success]- Answer
> **Complete Solution:**
> 
> ```python
> import pandas as pd
> 
> # Create sample stock data
> stock_data = pd.DataFrame({
>     'ticker': ['AAPL', 'GOOGL', 'MSFT', 'TSLA', 'AMZN', 
>                'META', 'NVDA', 'AMD'],
>     'volume': [50000000, 1200000, 30000000, 80000000, 2500000,
>                15000000, 60000000, 45000000]
> })
> 
> print("Original Data:")
> print(stock_data)
> 
> # ========== Define custom categorization function ==========
> def categorize_volume(row):
>     """
>     Categorize trading volume into High, Medium, or Low
>     
>     Thresholds:
>     - High: > 50,000,000
>     - Medium: 10,000,000 to 50,000,000
>     - Low: < 10,000,000
>     """
>     volume = row['volume']
>     
>     if volume > 50000000:
>         return "High Volume"
>     elif volume >= 10000000:
>         return "Medium Volume"
>     else:
>         return "Low Volume"
> 
> # ========== Apply function to DataFrame ==========
> print("\n" + "="*60)
> print("Applying volume categorization...")
> print("="*60)
> 
> # Apply to each row (axis=1)
> stock_data['volume_category'] = stock_data.apply(
>     categorize_volume,
>     axis=1
> )
> 
> print("\nData with volume categories:")
> print(stock_data)
> 
> # ========== Analyze results ==========
> print("\n" + "="*60)
> print("ANALYSIS")
> print("="*60)
> 
> # Count by category
> print("\nCount by category:")
> print(stock_data['volume_category'].value_counts())
> 
> # Average volume by category
> print("\nAverage volume by category:")
> avg_by_category = stock_data.groupby('volume_category')['volume'].mean()
> print(avg_by_category)
> 
> # ========== Advanced example: Multiple criteria ==========
> print("\n" + "="*60)
> print("ADVANCED: Multiple criteria classification")
> print("="*60)
> 
> # Add price data
> stock_data['price'] = [150, 2800, 300, 250, 3200, 350, 450, 120]
> 
> def classify_stock(row):
>     """Classify based on both volume and price"""
>     volume = row['volume']
>     price = row['price']
>     
>     # High volume AND high price
>     if volume > 50000000 and price > 400:
>         return "Blue Chip"
>     # High volume OR high price
>     elif volume > 50000000 or price > 1000:
>         return "Large Cap"
>     # Medium metrics
>     elif volume >= 10000000 or price >= 100:
>         return "Mid Cap"
>     # Everything else
>     else:
>         return "Small Cap"
> 
> stock_data['stock_class'] = stock_data.apply(classify_stock, axis=1)
> 
> print("\nMulti-criteria classification:")
> print(stock_data[['ticker', 'volume', 'price', 'stock_class']])
> 
> print("\n✓ Custom functions allow complex business logic in transformations")
> ```

**Q47.** Write complete code to persist a DataFrame to a PostgreSQL database using `.to_sql()`, then validate that the data was persisted correctly.

> [!success]- Answer
> **Complete Solution:**
> 
> ```python
> import pandas as pd
> import sqlalchemy
> import logging
> 
> # Setup logging
> logging.basicConfig(
>     format='%(levelname)s: %(message)s',
>     level=logging.INFO
> )
> 
> # ========== Prepare data ==========
> logging.info("Preparing data for persistence")
> 
> stock_data = pd.DataFrame({
>     'timestamp': pd.date_range('2023-01-01', periods=5, freq='D'),
>     'ticker': ['AAPL'] * 5,
>     'open': [150.0, 152.5, 151.0, 153.0, 154.5],
>     'close': [152.0, 151.0, 153.5, 154.0, 156.0],
>     'volume': [1000000, 950000, 1100000, 980000, 1050000]
> })
> 
> # Set timestamp as index
> stock_data = stock_data.set_index('timestamp')
> 
> print("Data to persist:")
> print(stock_data)
> print(f"\nRows: {len(stock_data)}, Columns: {len(stock_data.columns)}")
> 
> # ========== Create database connection ==========
> logging.info("Creating database connection")
> 
> connection_uri = "postgresql+psycopg2://repl:password@localhost:5432/market"
> db_engine = sqlalchemy.create_engine(connection_uri)
> 
> logging.info("✓ Connection established")
> 
> # ========== Persist data to SQL ==========
> logging.info("Persisting data to SQL database")
> 
> table_name = "stock_prices"
> 
> try:
>     stock_data.to_sql(
>         name=table_name,
>         con=db_engine,
>         if_exists="append",      # Add to existing table
>         index=True,               # Include timestamp index
>         index_label="timestamp"   # Name for index column
>     )
>     
>     logging.info(f"✓ Data persisted to '{table_name}' table")
>     
> except Exception as e:
>     logging.error(f"✗ Failed to persist data: {e}")
>     raise
> 
> # ========== Validate data persistence ==========
> logging.info("Validating data persistence")
> 
> try:
>     # Read data back from SQL
>     validation_query = f"SELECT * FROM {table_name}"
>     retrieved_data = pd.read_sql(validation_query, db_engine)
>     
>     print("\n" + "="*60)
>     print("VALIDATION RESULTS")
>     print("="*60)
>     
>     # Check 1: Row count
>     original_count = len(stock_data)
>     retrieved_count = len(retrieved_data)
>     
>     print(f"\n1. Row Count Validation:")
>     print(f"   Original:  {original_count} rows")
>     print(f"   Retrieved: {retrieved_count} rows")
>     
>     if retrieved_count >= original_count:
>         logging.info("   ✓ Row count matches (or table had existing data)")
>     else:
>         logging.error("   ✗ Row count mismatch!")
>     
>     # Check 2: Display sample
>     print(f"\n2. Sample of retrieved data:")
>     print(retrieved_data.head())
>     
>     # Check 3: Data types
>     print(f"\n3. Data types:")
>     print(retrieved_data.dtypes)
>     
>     # Check 4: Null values
>     print(f"\n4. Null value check:")
>     null_counts = retrieved_data.isnull().sum()
>     print(null_counts)
>     
>     if null_counts.sum() == 0:
>         logging.info("   ✓ No null values found")
>     else:
>         logging.warning(f"   ! Found {null_counts.sum()} null values")
>     
>     # Check 5: Specific record validation
>     print(f"\n5. Specific record validation:")
>     test_query = f"""
>         SELECT * FROM {table_name}
>         WHERE ticker = 'AAPL'
>         ORDER BY timestamp DESC
>         LIMIT 1
>     """
>     latest_record = pd.read_sql(test_query, db_engine)
>     print("Latest AAPL record:")
>     print(latest_record)
>     
>     print("\n" + "="*60)
>     print("✓ VALIDATION COMPLETE")
>     print("="*60)
>     
> except Exception as e:
>     logging.error(f"✗ Validation failed: {e}")
>     raise
> 
> # ========== Additional validation queries ==========
> print("\nAdditional Validation:")
> 
> # Count by ticker
> count_query = f"""
>     SELECT ticker, COUNT(*) as record_count
>     FROM {table_name}
>     GROUP BY ticker
> """
> counts = pd.read_sql(count_query, db_engine)
> print("\nRecords by ticker:")
> print(counts)
> 
> logging.info("✓ All validations passed successfully")
> ```

**Q48.** Write code that extracts nested JSON data, transforms it by filling missing values and grouping by ticker, then loads it to both CSV and SQL database.

> [!success]- Answer
> **Complete ETL Pipeline:**
> 
> ```python
> import json
> import pandas as pd
> import sqlalchemy
> import logging
> import os
> 
> # Setup logging
> logging.basicConfig(
>     format='%(asctime)s - %(levelname)s: %(message)s',
>     level=logging.INFO,
>     datefmt='%Y-%m-%d %H:%M:%S'
> )
> 
> # ==================== EXTRACT ====================
> def extract_from_json(file_path):
>     """Extract data from nested JSON file"""
>     logging.info(f"[EXTRACT] Reading from {file_path}")
>     
>     try:
>         with open(file_path, "r") as file:
>             raw_data = json.load(file)
>         
>         logging.info(f"[EXTRACT] Loaded {len(raw_data)} records")
>         return raw_data
>     except FileNotFoundError:
>         logging.error(f"[EXTRACT] File not found: {file_path}")
>         return None
>     except json.JSONDecodeError as e:
>         logging.error(f"[EXTRACT] Invalid JSON: {e}")
>         return None
> 
> # ==================== TRANSFORM ====================
> def transform_stock_data(raw_data):
>     """Transform nested JSON to DataFrame"""
>     logging.info("[TRANSFORM] Starting transformation")
>     
>     if raw_data is None:
>         return None
>     
>     # Parse nested JSON
>     parsed_data = []
>     for timestamp, info in raw_data.items():
>         parsed_data.append([
>             timestamp,
>             info.get("ticker", "UNKNOWN"),
>             info.get("price", {}).get("open", None),
>             info.get("price", {}).get("close", None),
>             info.get("volume", None)
>         ])
>     
>     # Create DataFrame
>     df = pd.DataFrame(parsed_data)
>     df.columns = ["timestamp", "ticker", "open", "close", "volume"]
>     
>     logging.info(f"[TRANSFORM] Created DataFrame: {len(df)} rows")
>     
>     # Fill missing values
>     logging.info("[TRANSFORM] Filling missing values")
>     df['open'].fillna(df['close'], inplace=True)
>     df = df.fillna({'open': 0, 'close': 0, 'volume': 0})
>     
>     # Group by ticker
>     logging.info("[TRANSFORM] Grouping by ticker")
>     grouped = df.groupby(['ticker']).agg({
>         'open': 'mean',
>         'close': 'mean',
>         'volume': ['mean', 'sum']
>     }).round(2)
>     
>     logging.info(f"[TRANSFORM] Grouped to {len(grouped)} ticker(s)")
>     
>     return grouped
> 
> # ==================== LOAD TO CSV ====================
> def load_to_csv(df, output_path):
>     """Load DataFrame to CSV file"""
>     logging.info(f"[LOAD CSV] Writing to {output_path}")
>     
>     try:
>         df.to_csv(output_path, index=True)
>         
>         # Validate
>         if os.path.exists(output_path):
>             file_size = os.path.getsize(output_path)
>             logging.info(f"[LOAD CSV] ✓ File created: {file_size} bytes")
>             return True
>         else:
>             logging.error("[LOAD CSV] ✗ File not created")
>             return False
>             
>     except Exception as e:
>         logging.error(f"[LOAD CSV] ✗ Error: {e}")
>         return False
> 
> # ==================== LOAD TO SQL ====================
> def load_to_sql(df, table_name, connection_uri):
>     """Load DataFrame to SQL database"""
>     logging.info(f"[LOAD SQL] Writing to {table_name}")
>     
>     try:
>         # Create engine
>         engine = sqlalchemy.create_engine(connection_uri)
>         
>         # Persist data
>         df.to_sql(
>             name=table_name,
>             con=engine,
>             if_exists="replace",
>             index=True
>         )
>         
>         # Validate
>         count_query = f"SELECT COUNT(*) as count FROM {table_name}"
>         result = pd.read_sql(count_query, engine)
>         row_count = result['count'][0]
>         
>         logging.info(f"[LOAD SQL] ✓ Loaded {row_count} rows to database")
>         return True
>         
>     except Exception as e:
>         logging.error(f"[LOAD SQL] ✗ Error: {e}")
>         return False
> 
> # ==================== EXECUTE PIPELINE ====================
> if __name__ == "__main__":
>     print("="*70)
>     print("ETL PIPELINE: JSON → Transform → CSV & SQL")
>     print("="*70)
>     
>     # Configuration
>     input_file = "nested_stock_data.json"
>     output_csv = "transformed_stock_data.csv"
>     output_table = "aggregated_stock_data"
>     db_uri = "postgresql+psycopg2://repl:password@localhost:5432/market"
>     
>     # Extract
>     raw_data = extract_from_json(input_file)
>     
>     if raw_data:
>         # Transform
>         transformed_data = transform_stock_data(raw_data)
>         
>         if transformed_data is not None:
>             print("\nTransformed Data Preview:")
>             print(transformed_data.head())
>             
>             # Load to both CSV and SQL
>             csv_success = load_to_csv(transformed_data, output_csv)
>             sql_success = load_to_sql(transformed_data, output_table, db_uri)
>             
>             # Summary
>             print("\n" + "="*70)
>             if csv_success and sql_success:
>                 print("✓ PIPELINE COMPLETED SUCCESSFULLY")
>                 print(f"  - CSV: {output_csv}")
>                 print(f"  - SQL: {output_table}")
>             else:
>                 print("✗ PIPELINE COMPLETED WITH ERRORS")
>             print("="*70)
>         else:
>             logging.error("Pipeline failed at TRANSFORM stage")
>     else:
>         logging.error("Pipeline failed at EXTRACT stage")
> ```

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (40 points)
Q1: B | Q2: A | Q3: B | Q4: C | Q5: C
Q6: B | Q7: C | Q8: C | Q9: B | Q10: B
Q11: B | Q12: B | Q13: B | Q14: B | Q15: B
Q16: A | Q17: C | Q18: B | Q19: B | Q20: B

### Section B: True/False (15 points)
Q21: F | Q22: T | Q23: F | Q24: F | Q25: T
Q26: T | Q27: F | Q28: T | Q29: F | Q30: T
Q31: T | Q32: F | Q33: T | Q34: T | Q35: T

### Section C: Short Answer (21 points)
- Each question worth 3 points
- Full credit for comprehensive explanations with examples
- Partial credit for correct but incomplete answers

### Section D: Code Analysis (24 points)
- Each question worth 3 points
- Award points for: correct code, proper error handling, good practices
- Partial credit for partially correct solutions

---

## Quick Study Guide

### Reading JSON
```python
# Simple JSON
df = pd.read_json("file.json", orient="columns")

# Complex/nested JSON
import json
with open("file.json", "r") as f:
    data = json.load(f)  # Returns dict
```

### Dictionary Methods
```python
dict.keys()          # List of keys
dict.values()        # List of values
dict.items()         # List of (key, value) tuples

# Safe access
dict.get("key", default)
dict.get("k1", {}).get("k2", 0)  # Nested
```

### Creating DataFrames
```python
df = pd.DataFrame(list_of_lists)
df.columns = ["col1", "col2", "col3"]
df = df.set_index("col1")
```

### Filling Missing Values
```python
df.fillna(0)                          # All → 0
df.fillna({"col1": 0, "col2": 1})    # Column-specific
df["col1"].fillna(df["col2"])         # From another column
```

### Grouping
```python
df.groupby(["col"]).mean()    # Average
df.groupby(["col"]).sum()     # Sum
df.groupby(["col"]).agg(['mean', 'min', 'max'])
```

### Applying Functions
```python
def custom_func(row):
    return row["col1"] + row["col2"]

df["new"] = df.apply(custom_func, axis=1)
```

### Writing to SQL
```python
df.to_sql(
    name="table",
    con=engine,
    if_exists="append",
    index=True,
    index_label="id"
)
```

---

## Key Concepts Summary

| Concept | Purpose | Example |
|---------|---------|---------|
| **Non-tabular** | Unstructured data | Text, audio, video, IoT |
| **JSON** | Key-value format | `{"key": "value"}` |
| **API** | Data interface | Controlled access |
| `pd.read_json()` | Read simple JSON | `orient="columns"` |
| `json.load()` | Read complex JSON | Returns dict |
| `.get()` | Safe dict access | No KeyError |
| `.items()` | Key-value iteration | For loops |
| `.fillna()` | Fill missing values | Handle NaN |
| `.groupby()` | Aggregate data | Like SQL GROUP BY |
| `.apply()` | Custom transforms | Apply functions |
| `.to_sql()` | Write to database | `if_exists="append"` |

---

**Good luck with your studies! 📊**

Remember: JSON → dict → DataFrame | .get() = safe | .groupby() = SQL GROUP BY | .apply() = custom functions!

---

#etl #json #non-tabular-data #apis #dictionaries #fillna #groupby #apply #to-sql #nested-data #data-transformation #pandas #datacamp #certification #chapter3

---

## Common Mistakes to Avoid

❌ **Don't:**
- Use `pd.read_json()` for complex nested structures
- Use bracket notation for uncertain dictionary keys
- Forget to handle missing/nested values with `.get()`
- Skip validation after transformation
- Forget to set column names after creating DataFrame
- Use `.fillna()` without understanding your data
- Apply `.groupby()` without specifying aggregation
- Forget `axis=1` in `.apply()` for row operations
- Use `if_exists="fail"` in production (use "append" or "replace")
- Skip validation after writing to SQL

✅ **Do:**
- Use `json` module for nested/complex JSON
- Use `.get()` with defaults for safe dictionary access
- Chain `.get()` for nested values: `.get("k1", {}).get("k2")`
- Always validate transformations
- Set meaningful column names and index
- Choose appropriate `.fillna()` strategy for your data
- Always specify aggregation function with `.groupby()`
- Use `axis=1` to apply functions to rows
- Use appropriate `if_exists` parameter for your use case
- Validate data persistence with queries
- Handle errors with try-except blocks
- Log important steps in pipeline

---

## Study Strategy (7-Day Plan)

### Day 1-2: JSON and Dictionary Basics
- Understand non-tabular data types
- Learn JSON structure and characteristics
- Practice reading with `pd.read_json()` and `json.load()`
- Master dictionary methods: `.keys()`, `.values()`, `.items()`
- Practice safe dictionary access with `.get()`

### Day 3: Data Transformation
- Learn to transform nested JSON to lists
- Practice creating DataFrames from lists
- Set column names and indexes
- Parse nested dictionaries with chained `.get()`

### Day 4: Handling Missing Data
- Master `.fillna()` three methods
- Practice column-specific fills
- Learn to fill from other columns
- Understand when to use each approach

### Day 5: Grouping and Aggregation
- Understand `.groupby()` mechanics
- Practice with different aggregation functions
- Learn SQL GROUP BY equivalents
- Combine multiple aggregations

### Day 6: Advanced Transformations
- Write custom functions for `.apply()`
- Practice row-wise operations
- Understand `axis=1` vs `axis=0`
- Create complex transformations

### Day 7: Loading and Integration
- Practice `.to_sql()` with all parameters
- Validate data persistence
- Build complete JSON → SQL pipelines
- Take practice quiz
- Review incorrect answers

---

## Final Exam Checklist

### Before the Exam:
- [ ] Know types of non-tabular data
- [ ] Understand what APIs and JSON are
- [ ] Can use `pd.read_json()` and `json.load()`
- [ ] Know when to use each JSON reading method
- [ ] Master `.keys()`, `.values()`, `.items()`
- [ ] Can use `.get()` safely with defaults
- [ ] Can chain `.get()` for nested values
- [ ] Can create DataFrames from lists
- [ ] Know how to set columns and index
- [ ] Understand all three `.fillna()` methods
- [ ] Can use `.groupby()` with aggregations
- [ ] Can write custom functions for `.apply()`
- [ ] Know `axis=1` applies to rows
- [ ] Understand `.to_sql()` parameters
- [ ] Know `if_exists` options
- [ ] Can validate data persistence

### During the Exam:
- [ ] Remember: Most data is unstructured
- [ ] JSON = key-value pairs, no fixed schema
- [ ] Use `json` module for nested JSON
- [ ] `.get()` doesn't raise KeyError
- [ ] Chain `.get()` with `{}` for nested: `.get("k", {}).get("nested")`
- [ ] `.items()` gives key-value pairs
- [ ] Set columns: `df.columns = [...]`
- [ ] `.fillna()` fills NaN, `.dropna()` removes rows
- [ ] `.groupby()` needs aggregation function
- [ ] `axis=1` in `.apply()` = rows
- [ ] `if_exists="append"` adds to table
- [ ] `if_exists="replace"` drops and recreates
- [ ] Always validate after SQL writes

---

## Integration with Previous Chapters

### Complete ETL Pipeline Pattern

**Chapter 1 + 2 + 3 Combined:**

```python
import pandas as pd
import json
import sqlalchemy
import logging

# Setup
logging.basicConfig(level=logging.INFO)

# 1. EXTRACT (Ch1 + Ch3)
# From CSV (Ch1)
csv_data = pd.read_csv("file.csv")

# From Parquet (Ch2)
parquet_data = pd.read_parquet("file.parquet", engine="fastparquet")

# From nested JSON (Ch3)
with open("file.json", "r") as f:
    json_data = json.load(f)

# 2. TRANSFORM (Ch1 + Ch2 + Ch3)
# Filter with .loc[] (Ch1)
filtered = csv_data.loc[csv_data["price"] > 100, :]

# Convert datetime (Ch2)
filtered["date"] = pd.to_datetime(filtered["date"], format="%Y%m%d")

# Fill missing values (Ch3)
filtered = filtered.fillna({"price": 0})

# Group and aggregate (Ch3)
aggregated = filtered.groupby(["ticker"]).mean()

# Apply custom function (Ch3)
def classify(row):
    return "High" if row["price"] > 200 else "Low"
aggregated["category"] = aggregated.apply(classify, axis=1)

# 3. LOAD (Ch1 + Ch2 + Ch3)
# To CSV (Ch1)
aggregated.to_csv("output.csv", index=False)

# To SQL (Ch3)
engine = sqlalchemy.create_engine(connection_uri)
aggregated.to_sql("results", con=engine, if_exists="append")

# 4. VALIDATE (Ch2 + Ch3)
# Check file exists (Ch2)
import os
assert os.path.exists("output.csv")

# Validate SQL (Ch3)
validation = pd.read_sql("SELECT COUNT(*) FROM results", engine)
logging.info(f"Loaded {validation.iloc[0, 0]} rows")
```

---

## Additional Practice Tips

1. **JSON practice:** Find real JSON files online and parse them
2. **Dictionary manipulation:** Practice nested dictionary access
3. **Missing data:** Create DataFrames with NaN and practice filling
4. **Grouping:** Practice SQL-like aggregations
5. **Custom functions:** Write various `.apply()` functions
6. **Complete pipelines:** Build JSON → Transform → SQL workflows
7. **Error handling:** Practice try-except with JSON/SQL operations
8. **Validation:** Always check results after transformations

---

## Real-World Applications

**When you'll use these skills:**

1. **API data ingestion** - Pulling JSON from REST APIs
2. **Log file analysis** - Parsing unstructured log data
3. **IoT pipelines** - Processing sensor data in JSON format
4. **Social media analytics** - Parsing nested Twitter/Facebook data
5. **Financial data** - Stock market data often comes as nested JSON
6. **E-commerce** - Product catalogs in JSON format
7. **Data cleaning** - Handling missing values in real datasets
8. **Reporting** - Aggregating data by categories
9. **Feature engineering** - Creating new features with `.apply()`
10. **Data warehouse loading** - Persisting to SQL databases

---

## Summary: Chapter 3 vs Previous Chapters

| Aspect | Chapter 1 | Chapter 2 | Chapter 3 |
|--------|-----------|-----------|-----------|
| **Data Type** | Tabular (CSV) | Tabular (CSV, Parquet, SQL) | Non-tabular (JSON) |
| **Extraction** | `pd.read_csv()` | `pd.read_parquet()`, SQL | `json.load()` |
| **Main Skills** | Basic filtering | Datetime, `.loc[]`, logging | Nested data, grouping |
| **Transformation** | Simple filters | Type conversion, validation | `.fillna()`, `.groupby()`, `.apply()` |
| **Loading** | `.to_csv()` | `.to_csv()`, `.to_sql()` | `.to_sql()` with validation |
| **Complexity** | Basic | Intermediate | Advanced |

---

**You've now mastered extracting from non-tabular sources! 🎉**

**Next steps:**
- Practice with real JSON APIs
- Build end-to-end pipelines
- Combine techniques from all chapters
- Prepare for comprehensive exam

---

#etl #json #non-tabular-data #apis #nested-json #dictionaries #fillna #groupby #apply #to-sql #data-transformation #pandas #sqlalchemy #validation #datacamp #certification #chapter3# Chapter 3 Practice Quiz
## Extracting Non-Tabular Data

**Total Points: 100**
**Pass Threshold: 70/100 (70%)**

---

## Section A: Multiple Choice Questions (2 points each = 40 points)

### Non-Tabular Data and JSON Basics

**Q1.** Which of the following is NOT listed as a type of non-tabular data?

- [ ] A) Text
- [ ] B) CSV files
- [ ] C) Audio
- [ ] D) Video

> [!success]- Answer
> **Correct Answer: B) CSV files**
> 
> CSV files are tabular data. Non-tabular data types include: text, audio, image, video, spatial, and IoT data.

**Q2.** What does API stand for?

- [ ] A) Application Programming Interface
- [ ] B) Advanced Python Integration
- [ ] C) Automated Process Integration
- [ ] D) Application Process Interface

> [!success]- Answer
> **Correct Answer: A) Application Programming Interface**
> 
> An API is software that sits on top of data sources and prevents direct interaction with the database, providing controlled access.

**Q3.** What does JSON stand for?

- [ ] A) Java Standard Object Notation
- [ ] B) JavaScript Object Notation
- [ ] C) Joint System Object Network
- [ ] D) Java Serialized Object Notation

> [!success]- Answer
> **Correct Answer: B) JavaScript Object Notation**
> 
> JSON is a key-value pair format with no set schema that looks and feels similar to Python dictionaries.

**Q4.** Which pandas function is used to read simple JSON files?

- [ ] A) `pd.load_json()`
- [ ] B) `pd.from_json()`
- [ ] C) `pd.read_json()`
- [ ] D) `pd.import_json()`

> [!success]- Answer
> **Correct Answer: C) `pd.read_json()`**
> 
> Similar to `pd.read_csv()`, pandas uses `pd.read_json()` to read JSON files into DataFrames.
> ```python
> df = pd.read_json("file.json", orient="columns")
> ```

**Q5.** What parameter in `pd.read_json()` specifies the expected JSON format?

- [ ] A) `format`
- [ ] B) `type`
- [ ] C) `orient`
- [ ] D) `structure`

> [!success]- Answer
> **Correct Answer: C) `orient`**
> 
> The `orient` parameter specifies JSON structure:
> - `"columns"` - Column names as keys
> - `"records"` - List of dicts
> - `"index"` - Index labels as keys
> - `"values"` - Just values array

### Dictionary Methods and JSON Module

**Q6.** When should you use the `json` module instead of `pd.read_json()`?

- [ ] A) For all JSON files
- [ ] B) For complex or nested JSON structures
- [ ] C) Only for very large files
- [ ] D) Never, always use pandas

> [!success]- Answer
> **Correct Answer: B) For complex or nested JSON structures**
> 
> The `json` module provides more control and can handle nested/complex JSON structures that `pd.read_json()` cannot easily parse.

**Q7.** What does `json.load()` return?

- [ ] A) A DataFrame
- [ ] B) A list
- [ ] C) A Python dictionary
- [ ] D) A string

> [!success]- Answer
> **Correct Answer: C) A Python dictionary**
> 
> ```python
> import json
> with open("file.json", "r") as f:
>     data = json.load(f)  # Returns dict
> print(type(data))  # <class 'dict'>
> ```

**Q8.** Which dictionary method returns a list of tuples containing key-value pairs?

- [ ] A) `.keys()`
- [ ] B) `.values()`
- [ ] C) `.items()`
- [ ] D) `.pairs()`

> [!success]- Answer
> **Correct Answer: C) `.items()`**
> 
> `.items()` generates a list of tuples made up of key-value pairs:
> ```python
> for key, value in raw_data.items():
>     print(f"{key}: {value}")
> ```

**Q9.** What is the advantage of using `.get()` instead of bracket notation for dictionary access?

- [ ] A) It's faster
- [ ] B) It doesn't raise KeyError if key is missing and can provide default values
- [ ] C) It returns multiple values
- [ ] D) It's required for nested dictionaries

> [!success]- Answer
> **Correct Answer: B) It doesn't raise KeyError if key is missing and can provide default values**
> 
> ```python
> # Safe access with default
> value = dict.get("key", "default_value")
> 
> # vs bracket notation (raises KeyError if missing)
> value = dict["key"]  # KeyError if "key" doesn't exist
> ```

**Q10.** How do you access a nested value in a dictionary using `.get()`?

- [ ] A) `dict.get("key.nested")`
- [ ] B) `dict.get("key").get("nested")`
- [ ] C) `dict.get(["key", "nested"])`
- [ ] D) `dict.nested.get("key")`

> [!success]- Answer
> **Correct Answer: B) `dict.get("key").get("nested")`**
> 
> Chain `.get()` calls to access nested values:
> ```python
> # For nested structure: {"price": {"open": 0.12}}
> open_price = entry.get("price", {}).get("open", 0)
> ```
> The `{}` provides an empty dict if "price" doesn't exist.

### DataFrame Creation and Transformation

**Q11.** How do you create a DataFrame from a list of lists?

- [ ] A) `pd.from_list(list_of_lists)`
- [ ] B) `pd.DataFrame(list_of_lists)`
- [ ] C) `pd.create_dataframe(list_of_lists)`
- [ ] D) `pd.to_dataframe(list_of_lists)`

> [!success]- Answer
> **Correct Answer: B) `pd.DataFrame(list_of_lists)`**
> 
> ```python
> data = [[1, 2, 3], [4, 5, 6]]
> df = pd.DataFrame(data)
> df.columns = ["col1", "col2", "col3"]
> ```

**Q12.** How do you set column names for a DataFrame?

- [ ] A) `df.set_columns(["col1", "col2"])`
- [ ] B) `df.columns = ["col1", "col2"]`
- [ ] C) `df.rename_columns(["col1", "col2"])`
- [ ] D) `df.column_names = ["col1", "col2"]`

> [!success]- Answer
> **Correct Answer: B) `df.columns = ["col1", "col2"]`**
> 
> Assign column names directly to the `.columns` attribute:
> ```python
> df.columns = ["timestamps", "open", "close", "volume"]
> ```

**Q13.** What method is used to set a column as the DataFrame index?

- [ ] A) `.index()`
- [ ] B) `.set_index()`
- [ ] C) `.make_index()`
- [ ] D) `.create_index()`

> [!success]- Answer
> **Correct Answer: B) `.set_index()`**
> 
> ```python
> df = df.set_index("timestamps")
> ```
> This sets the "timestamps" column as the index.

**Q14.** What does `.fillna(value=0)` do?

- [ ] A) Removes all rows with NaN
- [ ] B) Fills all NaN values with 0
- [ ] C) Counts NaN values
- [ ] D) Creates a new column with 0s

> [!success]- Answer
> **Correct Answer: B) Fills all NaN values with 0**
> 
> ```python
> clean_data = raw_data.fillna(value=0)
> ```
> This replaces all NaN (missing) values in the DataFrame with 0.

**Q15.** How do you fill NaN values with different values for different columns?

- [ ] A) Call `.fillna()` multiple times
- [ ] B) `df.fillna(value={"col1": 0, "col2": 1})`
- [ ] C) Use a loop
- [ ] D) Not possible in pandas

> [!success]- Answer
> **Correct Answer: B) `df.fillna(value={"col1": 0, "col2": 1})`**
> 
> Pass a dictionary to specify different fill values per column:
> ```python
> clean_data = raw_data.fillna(
>     value={"open": 0, "close": 0.5},
>     axis=1
> )
> ```

### Grouping and Advanced Transformations

**Q16.** What does the `.groupby()` method do?

- [ ] A) Groups rows with similar values for aggregation
- [ ] B) Sorts the DataFrame
- [ ] C) Filters the DataFrame
- [ ] D) Renames columns

> [!success]- Answer
> **Correct Answer: A) Groups rows with similar values for aggregation**
> 
> `.groupby()` is the pandas equivalent of SQL's GROUP BY:
> ```python
> grouped = df.groupby(["ticker"]).mean()
> ```

**Q17.** Which of the following is NOT a valid aggregation function for `.groupby()`?

- [ ] A) `.mean()`
- [ ] B) `.sum()`
- [ ] C) `.sort()`
- [ ] D) `.max()`

> [!success]- Answer
> **Correct Answer: C) `.sort()`**
> 
> Valid aggregation functions: `.mean()`, `.sum()`, `.min()`, `.max()`, `.count()`, etc.
> `.sort()` is not an aggregation function.

**Q18.** What does the `.apply()` method do?

- [ ] A) Applies formatting to text
- [ ] B) Applies a custom function to DataFrame rows or columns
- [ ] C) Applies filters
- [ ] D) Applies indexes

> [!success]- Answer
> **Correct Answer: B) Applies a custom function to DataFrame rows or columns**
> 
> `.apply()` allows you to apply custom transformation functions:
> ```python
> df["new_col"] = df.apply(custom_function, axis=1)
> ```

**Q19.** In `.apply(function, axis=1)`, what does `axis=1` mean?

- [ ] A) Apply to first column
- [ ] B) Apply to each row
- [ ] C) Apply to each column
- [ ] D) Apply to index

> [!success]- Answer
> **Correct Answer: B) Apply to each row**
> 
> - `axis=1` applies function to each row
> - `axis=0` applies function to each column

### Loading to SQL

**Q20.** What parameter in `.to_sql()` determines what happens if the table already exists?

- [ ] A) `exists`
- [ ] B) `if_exists`
- [ ] C) `on_exists`
- [ ] D) `when_exists`

> [!success]- Answer
> **Correct Answer: B) `if_exists`**
> 
> Options for `if_exists`:
> - `"fail"` - Raise error (default)
> - `"replace"` - Drop and recreate table
> - `"append"` - Add to existing table
> ```python
> df.to_sql(name="table", con=engine, if_exists="append")
> ```

---

## Section B: True/False Questions (1 point each = 15 points)

**Q21.** Most data produced and consumed is structured tabular data. **T/F**

> [!success]- Answer
> **False** - Most data is actually unstructured (non-tabular) data like text, audio, images, video, etc.

**Q22.** APIs prevent direct interaction with databases. **T/F**

> [!success]- Answer
> **True** - APIs sit on top of data sources and provide controlled access, preventing direct database interaction.

**Q23.** JSON has a strict, fixed schema like CSV files. **T/F**

> [!success]- Answer
> **False** - JSON has no set schema and is flexible in structure, unlike CSV which has fixed columns.

**Q24.** `pd.read_json()` can handle all types of JSON structures. **T/F**

> [!success]- Answer
> **False** - `pd.read_json()` works for simple structures. Complex or nested JSON requires the `json` module for more control.

**Q25.** The `json.load()` function returns a Python dictionary. **T/F**

> [!success]- Answer
> **True** - `json.load()` parses JSON and returns a Python dict object.

**Q26.** The `.items()` dictionary method returns key-value pairs. **T/F**

> [!success]- Answer
> **True** - `.items()` generates a list of tuples containing key-value pairs.

**Q27.** Using `.get()` on a dictionary will raise a KeyError if the key doesn't exist. **T/F**

> [!success]- Answer
> **False** - `.get()` returns `None` (or a specified default) if the key doesn't exist, avoiding KeyError.

**Q28.** You can chain `.get()` calls to access nested dictionary values. **T/F**

> [!success]- Answer
> **True** - Example: `dict.get("key", {}).get("nested", 0)`

**Q29.** `.fillna(value=0)` removes all rows containing NaN values. **T/F**

> [!success]- Answer
> **False** - `.fillna()` replaces NaN values with the specified value. To remove rows, use `.dropna()`.

**Q30.** You can fill NaN values in one column using values from another column. **T/F**

> [!success]- Answer
> **True** - Example: `df["open"].fillna(df["close"], inplace=True)`

**Q31.** `.groupby()` in pandas is equivalent to SQL's GROUP BY. **T/F**

> [!success]- Answer
> **True** - Both group rows by specified columns for aggregation operations.

**Q32.** `.apply()` can only be used with built-in pandas functions. **T/F**

> [!success]- Answer
> **False** - `.apply()` can use custom functions you define, not just built-in ones.

**Q33.** In `.apply(function, axis=1)`, the function receives each row as a parameter. **T/F**

> [!success]- Answer
> **True** - When `axis=1`, the function is applied to each row.

**Q34.** The `if_exists="append"` parameter in `.to_sql()` adds data to an existing table. **T/F**

> [!success]- Answer
> **True** - `"append"` adds new rows to the existing table without dropping it.

**Q35.** It's important to validate data after persisting it to a SQL database. **T/F**

> [!success]- Answer
> **True** - Validation ensures data was persisted correctly (check counts, query data back, etc.).

---

## Section C: Short Answer Questions (3 points each = 21 points)

**Q36.** Explain what non-tabular data is and provide at least 4 types with examples of each.

> [!success]- Answer
> **Non-tabular data** is unstructured data that doesn't fit into rows and columns format like traditional databases or spreadsheets.
> 
> **Types and examples:**
> 
> 1. **Text**
>    - Articles and documents
>    - Emails and messages
>    - Log files
>    - Social media posts
> 
> 2. **Audio**
>    - Music files
>    - Podcasts
>    - Phone call recordings
>    - Voice assistants
> 
> 3. **Image**
>    - Photographs
>    - Scanned documents
>    - Medical imaging (X-rays, MRIs)
>    - Satellite imagery
> 
> 4. **Video**
>    - Movies and TV shows
>    - Surveillance footage
>    - Livestreams
>    - Video conferences
> 
> 5. **Spatial**
>    - GPS coordinates
>    - Maps
>    - Geographic data
>    - Location tracking
> 
> 6. **IoT (Internet of Things)**
>    - Smart device sensors
>    - Temperature monitors
>    - Fitness trackers
>    - Industrial sensors
> 
> **Why it matters:** Most data produced today is non-tabular, requiring special extraction and transformation techniques to make it usable in data pipelines.

**Q37.** Compare using `pd.read_json()` versus the `json` module for reading JSON files. When would you use each?

> [!success]- Answer
> **`pd.read_json()` - pandas approach:**
> 
> **When to use:**
> - Simple, DataFrame-ready JSON structures
> - JSON with consistent schema
> - When you want immediate DataFrame output
> 
> **Advantages:**
> - Direct to DataFrame conversion
> - One-line solution
> - Built-in data type handling
> 
> **Example:**
> ```python
> df = pd.read_json("file.json", orient="columns")
> # Immediately usable DataFrame
> ```
> 
> **Limitations:**
> - Struggles with nested structures
> - Limited control over parsing
> - Requires specific JSON format
> 
> **`json` module approach:**
> 
> **When to use:**
> - Complex or nested JSON
> - Varying schema
> - Need more control over parsing
> - Need to transform before creating DataFrame
> 
> **Advantages:**
> - Handles any JSON structure
> - Full control over parsing
> - Can transform as dict first
> - Better for nested data
> 
> **Example:**
> ```python
> import json
> with open("file.json", "r") as f:
>     data = json.load(f)  # Returns dict
> # Then transform and create DataFrame
> ```
> 
> **Best practice:**
> - Try `pd.read_json()` first for simple cases
> - Use `json` module for nested/complex structures
> - The example in the chapter uses `json` module because the JSON has nested price objects

**Q38.** Describe the three dictionary methods (`.keys()`, `.values()`, `.items()`) and provide examples of when to use each.

> [!success]- Answer
> **Three dictionary iteration methods:**
> 
> **1. `.keys()` - Returns list of keys**
> 
> **Use when:** You only need the keys, not the values
> 
> **Example:**
> ```python
> stock_data = {"AAPL": 150, "GOOGL": 2800, "MSFT": 300}
> 
> # Get all ticker symbols
> for ticker in stock_data.keys():
>     print(ticker)
> # Output: AAPL, GOOGL, MSFT
> ```
> 
> **2. `.values()` - Returns list of values**
> 
> **Use when:** You only need the values, not the keys
> 
> **Example:**
> ```python
> stock_data = {"AAPL": 150, "GOOGL": 2800, "MSFT": 300}
> 
> # Calculate average price
> prices = list(stock_data.values())
> average = sum(prices) / len(prices)
> print(f"Average: {average}")
> # Output: Average: 1083.33
> ```
> 
> **3. `.items()` - Returns list of (key, value) tuples**
> 
> **Use when:** You need both keys and values together (most common)
> 
> **Example:**
> ```python
> stock_data = {"AAPL": 150, "GOOGL": 2800, "MSFT": 300}
> 
> # Print ticker and price together
> for ticker, price in stock_data.items():
>     print(f"{ticker}: ${price}")
> # Output:
> # AAPL: $150
> # GOOGL: $2800
> # MSFT: $300
> ```
> 
> **Common usage in ETL:**
> ```python
> # Transform nested JSON
> parsed_data = []
> for timestamp, info in raw_data.items():  # Use .items()
>     parsed_data.append([
>         timestamp,
>         info.get("open"),
>         info.get("close")
>     ])
> ```
> 
> **Summary:**
> - `.keys()`: Need only keys
> - `.values()`: Need only values
> - `.items()`: Need both (most common in ETL)

**Q39.** Explain how to safely access nested dictionary values using `.get()`. Why is this better than bracket notation?

> [!success]- Answer
> **Safe dictionary access with `.get()`:**
> 
> **Basic `.get()` syntax:**
> ```python
> value = dictionary.get("key", default_value)
> ```
> 
> **For nested dictionaries - chain `.get()` calls:**
> ```python
> entry = {
>     "volume": 1443120000,
>     "price": {
>         "open": 0.12187,
>         "close": 0.09791
>     }
> }
> 
> # Safe nested access
> open_price = entry.get("price", {}).get("open", 0)
> ```
> 
> **How it works:**
> 1. `entry.get("price", {})` - Returns price dict or empty dict `{}`
> 2. `.get("open", 0)` - Returns open value or 0 if not found
> 
> **Why better than bracket notation:**
> 
> **Bracket notation (risky):**
> ```python
> # This raises KeyError if keys don't exist
> open_price = entry["price"]["open"]  # KeyError if missing!
> ```
> 
> **`.get()` advantages:**
> 
> 1. **No KeyError**
>    - Returns default instead of crashing
>    - Pipeline continues running
> 
> 2. **Default values**
>    - Specify fallback values
>    - Handle missing data gracefully
> 
> 3. **Chainable**
>    - Works for nested structures
>    - Each level provides safety
> 
> 4. **Robust ETL**
>    - Handles varying schemas
>    - Won't crash on missing fields
> 
> **Real-world example:**
> ```python
> # Some records might not have price data
> parsed_data = []
> for timestamp, info in raw_data.items():
>     parsed_data.append([
>         timestamp,
>         # Safe - won't crash if price or open missing
>         info.get("price", {}).get("open", 0),
>         info.get("price", {}).get("close", 0),
>         # Safe - won't crash if volume missing
>         info.get("volume", 0)
>     ])
> ```
> 
> **Best practice in ETL:** Always use `.get()` when parsing external data sources to handle incomplete or varying data structures.

**Q40.** Describe the three methods for filling missing values (NaN) in pandas DataFrames with examples of each.

> [!success]- Answer
> **Three methods for filling missing values:**
> 
> **Method 1: Fill all NaN with same value**
> 
> **Use when:** All missing values should have the same replacement
> 
> ```python
> # Fill all NaN with 0
> clean_data = raw_data.fillna(value=0)
> 
> # Common use cases:
> # - Fill missing prices with 0
> # - Fill missing counts with 0
> # - Fill missing flags with False
> ```
> 
> **Before:**
> ```
> open    close   volume
> 0.12    0.09    1000
> NaN     0.08    500
> 0.10    NaN     750
> ```
> 
> **After:**
> ```
> open    close   volume
> 0.12    0.09    1000
> 0.00    0.08    500
> 0.10    0.00    750
> ```
> 
> **Method 2: Fill with column-specific values**
> 
> **Use when:** Different columns need different fill values
> 
> ```python
> # Different fill value per column
> clean_data = raw_data.fillna(
>     value={"open": 0, "close": 0.5, "volume": -1},
>     axis=1
> )
> 
> # Common use cases:
> # - Fill prices with 0, volumes with -1 (to indicate missing)
> # - Fill different defaults based on column meaning
> ```
> 
> **After:**
> ```
> open    close   volume
> 0.12    0.09    1000
> 0.00    0.08    500
> 0.10    0.50    750
> ```
> 
> **Method 3: Fill using other columns**
> 
> **Use when:** Missing values can be inferred from related columns
> 
> ```python
> # Fill open price with close price if missing
> raw_data["open"].fillna(raw_data["close"], inplace=True)
> 
> # Common use cases:
> # - Fill missing open with close (assume no change)
> # - Fill missing values with averages
> # - Forward/backward fill
> ```
> 
> **Before:**
> ```
> open    close
> 0.12    0.09
> NaN     0.08
> 0.10    NaN
> ```
> 
> **After:**
> ```
> open    close
> 0.12    0.09
> 0.08    0.08   # open filled from close
> 0.10    NaN    # close still NaN (no fill source)
> ```
> 
> **Additional techniques:**
> ```python
> # Forward fill - use previous value
> df.fillna(method='ffill')
> 
> # Backward fill - use next value
> df.fillna(method='bfill')
> 
> # Fill with mean
> df.fillna(df.mean())
> ```
> 
> **Best practice:** Choose method based on data context:
> - Use 0 for counts/quantities
> - Use column values for related fields
> - Use statistical methods (mean/median) for continuous variables

**Q41.** Explain what `.groupby()` does in pandas and how it's equivalent to SQL GROUP BY. Provide an example.

> [!success]- Answer
> **`.groupby()` in pandas:**
> 
> **Purpose:** Groups rows with the same values in specified columns, then applies aggregation functions
> 
> **SQL equivalent:**
> ```sql
> SELECT
>     ticker,
>     AVG(volume) as avg_volume,
>     AVG(open) as avg_open,
>     AVG(close) as avg_close
> FROM raw_stock_data
> GROUP BY ticker;
> ```
> 
> **pandas equivalent:**
> ```python
> grouped_data = raw_stock_data.groupby(
>     by=["ticker"],
>     axis=0
> ).mean()
> ```
> 
> **Complete example:**
> 
> **Before grouping:**
> ```
> ticker    volume        open      close
> AAPL      1443120000    0.121875  0.097917
> AAPL      297000000     0.098146  0.086458
> AAPL      150000000     0.095000  0.092000
> AMZN      124186000     0.247511  0.251290
> AMZN      98000000      0.240000  0.245000
> ```
> 
> **After grouping and aggregation:**
> ```
>                 volume          open        close
> ticker
> AAPL      630040000.0      0.105007    0.092125
> AMZN      111093000.0      0.243756    0.248145
> ```
> 
> **How it works:**
> 1. **Group:** Separates rows by ticker value
> 2. **Aggregate:** Applies `.mean()` to each group
> 3. **Result:** One row per unique ticker with averages
> 
> **Other aggregation functions:**
> ```python
> # Minimum values per group
> raw_stock_data.groupby(["ticker"]).min()
> 
> # Maximum values per group
> raw_stock_data.groupby(["ticker"]).max()
> 
> # Sum per group
> raw_stock_data.groupby(["ticker"]).sum()
> 
> # Count per group
> raw_stock_data.groupby(["ticker"]).count()
> 
> # Multiple aggregations
> raw_stock_data.groupby(["ticker"]).agg(['mean', 'min', 'max'])
> ```
> 
> **Group by multiple columns:**
> ```python
> # Group by ticker AND date
> grouped = raw_stock_data.groupby(
>     ["ticker", "date"]
> ).mean()
> ```
> 
> **When to use:**
> - Calculate summary statistics per category
> - Aggregate data by time periods
> - Find patterns across groups
> - Reduce data dimensionality

**Q42.** Describe how to use `.apply()` to create custom transformations. Include an example with a custom function.

> [!success]- Answer
> **`.apply()` for custom transformations:**
> 
> **Purpose:** Apply a custom function to DataFrame rows or columns
> 
> **Basic syntax:**
> ```python
> df["new_column"] = df.apply(function_name, axis=1)
> ```
> 
> **Parameters:**
> - `function_name`: Your custom function
> - `axis=1`: Apply to rows (most common)
> - `axis=0`: Apply to columns
> 
> **Example 1: Classify price changes**
> 
> ```python
> def classify_change(row):
>     """Classify if price increased or decreased"""
>     change = row["close"] - row["open"]
>     if change > 0:
>         return "Increase"
>     elif change < 0:
>         return "Decrease"
>     else:
>         return "No Change"
> 
> # Apply to DataFrame
> stock_data["change_type"] = stock_data.apply(
>     classify_change,
>     axis=1
> )
> ```
> 
> **Before:**
> ```
> ticker    open      close
> AAPL      0.121875  0.097917
> AAPL      0.098146  0.086458
> AMZN      0.247511  0.251290
> ```
> 
> **After:**
> ```
> ticker    open      close     change_type
> AAPL      0.121875  0.097917  Decrease
> AAPL      0.098146  0.086458  Decrease
> AMZN      0.247511  0.251290  Increase
> ```
> 
> **Example 2: Calculate percentage change**
> 
> ```python
> def calc_pct_change(row):
>     """Calculate percentage change"""
>     if row["open"] == 0:
>         return 0
>     return ((row["close"] - row["open"]) / row["open"]) * 100
> 
> stock_data["pct_change"] = stock_data.apply(
>     calc_pct_change,
>     axis=1
> )
> ```
> 
> **Example 3: Risk categorization**
>