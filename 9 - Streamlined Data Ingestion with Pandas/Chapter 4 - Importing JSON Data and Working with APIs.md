## Overview
This chapter covers working with JSON data, APIs, nested JSON structures, and combining multiple datasets using concatenation and merging.

## Key Concepts

### JavaScript Object Notation (JSON)

**Characteristics:**
- **Common web data format**
- **Not tabular** - unlike CSV/Excel
- Records don't have to all have the same set of attributes
- **Data organized into collections of objects**
- **Objects are collections of attribute-value pairs**
- **Nested JSON**: objects within objects

## Reading JSON Data

### pd.read_json() Function

```python
import pandas as pd
death_causes = pd.read_json("nyc_death_causes.json")
```

**Parameters:**
- Takes a **string path to JSON** OR **JSON data as a string**
- **`dtype`** keyword argument to specify data types
- **`orient`** keyword argument to flag uncommon JSON data layouts
  - Possible values in pandas documentation

### Data Orientation

**Key concept**: JSON data isn't tabular, so pandas guesses how to arrange it in a table. pandas can automatically handle common orientations.

#### Record Orientation
**Most common JSON arrangement**

```json
[
  {
    "age_adjusted_death_rate": "7.6",
    "death_rate": "6.2",
    "deaths": "32",
    "leading_cause": "Accidents Except Drug Posioning",
    "race_ethnicity": "Asian and Pacific Islander",
    "sex": "F",
    "year": "2007"
  },
  {
    "age_adjusted_death_rate": "8.1",
    "death_rate": "8.3",
    "deaths": "87",
    ...
  }
]
```

#### Column Orientation
**More space-efficient than record-oriented JSON**

```json
{
  "age_adjusted_death_rate": {
    "0": "7.6",
    "1": "8.1",
    "2": "7.1",
    ...
  }
}
```

#### Specifying Orientation

**Split oriented data** example:

```json
{
  "columns": ["age_adjusted_death_rate", "death_rate", "deaths", ...],
  "index": [...],
  "data": [["7.6", "6.2", "32", ...]]
}
```

**Usage:**
```python
death_causes = pd.read_json("nyc_death_causes.json", orient="split")
```

## Introduction to APIs

### Application Programming Interfaces (APIs)

**Definition:**
- Defines how an application communicates with other programs
- Way to get data from an application **without knowing database details**

### The requests Library

**Purpose**: Send and get data from websites

**Key features:**
- Not tied to a particular API
- **`requests.get()`** to get data from a URL

### requests.get() Function

**Syntax:**
```python
requests.get(url_string)
```

**Keyword arguments:**
- **`params`**: Takes a dictionary of parameters and values to customize API request
- **`headers`**: Takes a dictionary, can be used to provide user authentication to API

**Result:**
- Returns a **response object**, containing data and metadata
- **`response.json()`** will return just the JSON data

### response.json() and pandas

> [!warning] Important Distinction
> - `response.json()` returns a **dictionary**
> - `read_json()` expects **strings**, not dictionaries
> - **Load the response JSON to a dataframe with `pd.DataFrame()`**
> - `read_json()` will give an error!

### Making API Requests Example

```python
import requests
import pandas as pd

api_url = "https://api.yelp.com/v3/businesses/search"

# Set up parameter dictionary according to documentation
params = {"term": "bookstore",
          "location": "San Francisco"}

# Set up header dictionary w/ API key according to documentation
headers = {"Authorization": "Bearer {}".format(api_key)}

# Call the API
response = requests.get(api_url,
                       params=params,
                       headers=headers)
```

### Parsing API Responses

```python
# Isolate the JSON data from the response object
data = response.json()

# Load businesses data to a dataframe
bookstores = pd.DataFrame(data["businesses"])
print(bookstores.head(2))
```

## Working with Nested JSONs

### What is Nested JSON?

**Definition:**
- JSONs contain objects with attribute-value pairs
- **A JSON is nested when the value itself is an object**

### Example of Nested Data

```python
print(bookstores[["categories", "coordinates", "location"]].head(3))
```

Output shows nested dictionaries within columns:
- `categories`: `[{'alias': 'bookstores', 'title': 'Bookstores'}]`
- `coordinates`: `{'latitude': 37.7975997924805, 'longitude': -1...}`
- `location`: `{'address1': '261 Columbus Ave', 'address2': '...}`

### pandas.io.json Submodule

**Key points:**
- `pandas.io.json` submodule has tools for reading and writing JSON
- **Needs its own import statement**

### json_normalize() Function

**Purpose**: Flattens nested JSON data

**Characteristics:**
- Takes a dictionary/list of dictionaries (like `pd.DataFrame()` does)
- Returns a **flattened dataframe**
- **Default flattened column name pattern**: `attribute.nestedattribute`
- Choose a different separator with the **`sep`** argument

### Loading Nested JSON Data

```python
import pandas as pd
import requests
from pandas.io.json import json_normalize

# Set up headers, parameters, and API endpoint
api_url = "https://api.yelp.com/v3/businesses/search"
headers = {"Authorization": "Bearer {}".format(api_key)}
params = {"term": "bookstore", "location": "San Francisco"}

# Make the API call and extract the JSON data
response = requests.get(api_url, headers=headers, params=params)
data = response.json()

# Flatten data and load to dataframe, with _ separators
bookstores = json_normalize(data["businesses"], sep="_")
```

**Result**: Nested attributes become separate columns:
- `coordinates_latitude`
- `coordinates_longitude`
- `location_address1`
- `location_city`
- etc.

### Deeply Nested Data

**Problem**: Sometimes data is nested multiple levels deep

**Solution**: `json_normalize()` advanced parameters:

- **`record_path`**: String/list of string attributes to nested data
- **`meta`**: List of other attributes to load to dataframe
- **`meta_prefix`**: String to prefix to meta column names

### Example: Deeply Nested Data

```python
# Flatten categories data, bring in business details
df = json_normalize(data["businesses"],
                   sep="_",
                   record_path="categories",
                   meta=["name",
                         "alias",
                         "rating",
                         ["coordinates", "latitude"],
                         ["coordinates", "longitude"]],
                   meta_prefix="biz_")
```

**Result**: Creates one row per category, with business info prefixed:
- `alias`, `title` (from categories)
- `biz_name`, `biz_alias`, `biz_rating` (from business)
- `biz_coordinates_latitude`, `biz_coordinates_longitude`

## Combining Multiple Datasets

### Concatenating

**Use case**: Adding rows from one dataframe to another

**Function**: `pd.concat()`
- pandas function
- **Syntax**: `pd.concat([df1, df2])`
- Set **`ignore_index=True`** to renumber rows

### Concatenating Example

```python
# Get first 20 bookstore results
first_20_bookstores = json_normalize(first_results["businesses"], sep="_")
print(first_20_bookstores.shape)  # (20, 24)

# Get the next 20 bookstores
params["offset"] = 20
next_20_bookstores = json_normalize(next_results["businesses"], sep="_")
print(next_20_bookstores.shape)  # (20, 24)

# Put bookstore datasets together, renumber rows
bookstores = pd.concat([first_20_bookstores, next_20_bookstores],
                       ignore_index=True)
print(bookstores.shape)  # (40, 24)
```

### Merging

**Use case**: Combining datasets to add related columns

**Requirements:**
- Datasets have key column(s) with common values

**Function**: `merge()` - pandas version of a SQL join

### merge() Details

**Usage**:
- Both a pandas function and a dataframe method
- `df.merge()` arguments:
  - Second dataframe to merge
  - Columns to merge on:
    - **`on`** if names are the same in both dataframes
    - **`left_on` and `right_on`** if key names differ
  - **Key columns should be the same data type**

### Merging Example

```python
# call_counts dataframe
#   created_date  call_counts
# 0  01/01/2018         4597
# 1  01/02/2018         4362

# weather dataframe
#         date  tmax  tmin
# 0 01/01/2018    19     7
# 1 01/02/2018    26    13

# Merge weather into call counts on date columns
merged = call_counts.merge(weather,
                          left_on="created_date",
                          right_on="date")

# Result:
#   created_date  call_counts       date  tmax  tmin
# 0   01/01/2018         4597 01/01/2018    19     7
# 1   01/02/2018         4362 01/02/2018    26    13
```

### Default merge() Behavior

- **Return only values that are in both datasets**
- One record for each value match between dataframes
- Multiple matches = multiple records

## Course Recap

### Chapters 1 and 2
- `read_csv()` and `read_excel()`
- Setting data types, choosing data to load
- Handling missing data and errors

### Chapter 3
- `read_sql()` and sqlalchemy
- SQL SELECT, WHERE, aggregate functions and joins

### Chapter 4
- `read_json()`, `json_normalize()`, and requests
- Working with APIs and nested JSONs
- Concatenating and merging datasets

## Key Functions Summary

| Function | Purpose | Key Parameters |
|----------|---------|----------------|
| `pd.read_json()` | Read JSON files | `orient`, `dtype` |
| `requests.get()` | Get data from URLs/APIs | `params`, `headers` |
| `response.json()` | Extract JSON from response | Returns dictionary |
| `pd.DataFrame()` | Create DataFrame from dict | Use for API responses |
| `json_normalize()` | Flatten nested JSON | `sep`, `record_path`, `meta`, `meta_prefix` |
| `pd.concat()` | Stack DataFrames vertically | `ignore_index` |
| `df.merge()` | Join DataFrames horizontally | `on`, `left_on`, `right_on` |

## Important Distinctions

| Concept | Key Point |
|---------|-----------|
| JSON vs CSV | JSON is not tabular; records can have different attributes |
| read_json() vs DataFrame() | Use `DataFrame()` for API response dicts, not `read_json()` |
| Nested JSON | Values can be objects themselves |
| json_normalize() | Flattens nested structures into columns |
| concat() vs merge() | concat() adds rows; merge() adds columns |
| merge() keys | Must be same data type |

## JSON Orientation Types

| Orientation | Description | Most Common? |
|-------------|-------------|--------------|
| Record | List of objects | Yes - most common |
| Column | Object of objects | More space-efficient |
| Split | Separate columns, index, data | Requires `orient="split"` |

---

#datacamp #pandas #json #apis #requests #data-ingestion #json-normalize #merge #concat

# Chapter 4: JSON & APIs - Practice Exam

**Course:** Streamlined Data Ingestion with pandas  
**Chapter:** Introduction to JSON  
**Total Points:** 100  
**Passing Score:** 70/100 (70%)  
**Time Limit:** 60 minutes

---

## Section A: Multiple Choice Questions (30 questions × 2 points = 60 points)

### Topic 1: JSON Basics and Data Orientation (10 questions)

**Q1.** What does JSON stand for?
- [ ] A) Java Standard Object Notation
- [ ] B) JavaScript Object Notation
- [ ] C) Just Simple Object Notation
- [ ] D) Java Serialized Object Network

> [!success]- Answer
> **Correct Answer: B) JavaScript Object Notation**
> 
> JSON stands for JavaScript Object Notation, a common web data format.

**Q2.** Which of the following is NOT a characteristic of JSON mentioned in the chapter?
- [ ] A) Common web data format
- [ ] B) Not tabular
- [ ] C) Records must all have the same attributes
- [ ] D) Data organized into collections of objects

> [!success]- Answer
> **Correct Answer: C) Records must all have the same attributes**
> 
> The chapter explicitly states: "Records don't have to all have the same set of attributes" - this flexibility is a key feature of JSON.

**Q3.** What are objects in JSON?
- [ ] A) Collections of tables
- [ ] B) Collections of attribute-value pairs
- [ ] C) Arrays of numbers
- [ ] D) Database records

> [!success]- Answer
> **Correct Answer: B) Collections of attribute-value pairs**
> 
> Objects in JSON are collections of attribute-value pairs (like dictionaries in Python).

**Q4.** What is nested JSON?
- [ ] A) JSON stored in multiple files
- [ ] B) JSON with many rows
- [ ] C) Objects within objects
- [ ] D) Compressed JSON data

> [!success]- Answer
> **Correct Answer: C) Objects within objects**
> 
> The chapter defines nested JSON as "objects within objects" - when values themselves are objects.

**Q5.** Which function is used to read JSON data in pandas?
- [ ] A) pd.load_json()
- [ ] B) pd.json_read()
- [ ] C) pd.read_json()
- [ ] D) pd.import_json()

> [!success]- Answer
> **Correct Answer: C) pd.read_json()**
> 
> `pd.read_json()` is the pandas function for reading JSON data.

**Q6.** What can `pd.read_json()` accept as input?
- [ ] A) Only file paths
- [ ] B) Only JSON strings
- [ ] C) String path to JSON OR JSON data as a string
- [ ] D) Only dictionaries

> [!success]- Answer
> **Correct Answer: C) String path to JSON OR JSON data as a string**
> 
> The function can take either a file path or JSON data as a string.

**Q7.** What parameter is used to flag uncommon JSON data layouts?
- [ ] A) layout
- [ ] B) format
- [ ] C) orient
- [ ] D) structure

> [!success]- Answer
> **Correct Answer: C) orient**
> 
> The `orient` keyword argument specifies the JSON data layout/orientation.

**Q8.** Which JSON orientation is the most common?
- [ ] A) Column orientation
- [ ] B) Split orientation
- [ ] C) Record orientation
- [ ] D) Index orientation

> [!success]- Answer
> **Correct Answer: C) Record orientation**
> 
> The chapter states: "Record Orientation: Most common JSON arrangement"

**Q9.** Which JSON orientation is more space-efficient than record-oriented JSON?
- [ ] A) Split orientation
- [ ] B) Column orientation
- [ ] C) Index orientation
- [ ] D) Table orientation

> [!success]- Answer
> **Correct Answer: B) Column orientation**
> 
> The chapter explicitly states: "Column Orientation: More space-efficient than record-oriented JSON"

**Q10.** How does pandas handle JSON data orientation?
- [ ] A) Requires manual specification always
- [ ] B) Cannot handle different orientations
- [ ] C) Guesses how to arrange it in a table; can automatically handle common orientations
- [ ] D) Only works with record orientation

> [!success]- Answer
> **Correct Answer: C) Guesses how to arrange it in a table; can automatically handle common orientations**
> 
> pandas automatically detects common orientations, but you can specify with `orient` for uncommon layouts.

### Topic 2: APIs and requests Library (10 questions)

**Q11.** What does API stand for?
- [ ] A) Advanced Programming Interface
- [ ] B) Application Programming Interface
- [ ] C) Automated Process Integration
- [ ] D) Application Process Interface

> [!success]- Answer
> **Correct Answer: B) Application Programming Interface**
> 
> API stands for Application Programming Interface.

**Q12.** What is the purpose of an API?
- [ ] A) To compress data
- [ ] B) To define how an application communicates with other programs
- [ ] C) To store data locally
- [ ] D) To encrypt data

> [!success]- Answer
> **Correct Answer: B) To define how an application communicates with other programs**
> 
> APIs define communication protocols and allow getting data without knowing database details.

**Q13.** What library is used to send and get data from websites?
- [ ] A) urllib
- [ ] B) http
- [ ] C) requests
- [ ] D) api

> [!success]- Answer
> **Correct Answer: C) requests**
> 
> The `requests` library is used to send and get data from websites.

**Q14.** Is the requests library tied to a particular API?
- [ ] A) Yes, it only works with REST APIs
- [ ] B) Yes, it only works with JSON APIs
- [ ] C) No, it's not tied to a particular API
- [ ] D) Yes, it only works with Yelp API

> [!success]- Answer
> **Correct Answer: C) No, it's not tied to a particular API**
> 
> The chapter states: "Not tied to a particular API" - it's a general-purpose library.

**Q15.** Which function gets data from a URL?
- [ ] A) requests.fetch()
- [ ] B) requests.download()
- [ ] C) requests.get()
- [ ] D) requests.retrieve()

> [!success]- Answer
> **Correct Answer: C) requests.get()**
> 
> `requests.get()` is used to get data from a URL.

**Q16.** What parameter in `requests.get()` takes a dictionary of parameters to customize API requests?
- [ ] A) parameters
- [ ] B) params
- [ ] C) args
- [ ] D) options

> [!success]- Answer
> **Correct Answer: B) params**
> 
> The `params` keyword argument takes a dictionary of parameters and values.

**Q17.** What parameter in `requests.get()` can be used to provide user authentication to an API?
- [ ] A) auth
- [ ] B) credentials
- [ ] C) headers
- [ ] D) token

> [!success]- Answer
> **Correct Answer: C) headers**
> 
> The `headers` keyword argument takes a dictionary and can be used for authentication.

**Q18.** What does `requests.get()` return?
- [ ] A) A DataFrame
- [ ] B) A dictionary
- [ ] C) A response object containing data and metadata
- [ ] D) JSON data

> [!success]- Answer
> **Correct Answer: C) A response object containing data and metadata**
> 
> The result is a response object that contains both data and metadata.

**Q19.** What method extracts just the JSON data from a response object?
- [ ] A) response.data()
- [ ] B) response.json()
- [ ] C) response.get_json()
- [ ] D) response.to_json()

> [!success]- Answer
> **Correct Answer: B) response.json()**
> 
> `response.json()` returns just the JSON data from the response object.

**Q20.** What data type does `response.json()` return?
- [ ] A) A string
- [ ] B) A DataFrame
- [ ] C) A dictionary
- [ ] D) A list

> [!success]- Answer
> **Correct Answer: C) A dictionary**
> 
> The chapter states: "response.json() returns a dictionary"

### Topic 3: Nested JSON, json_normalize(), and Combining Data (10 questions)

**Q21.** Why can't you use `pd.read_json()` with the output of `response.json()`?
- [ ] A) It's too slow
- [ ] B) read_json() expects strings, not dictionaries
- [ ] C) It requires a file path
- [ ] D) APIs use different JSON format

> [!success]- Answer
> **Correct Answer: B) read_json() expects strings, not dictionaries**
> 
> The chapter emphasizes: "read_json() expects strings, not dictionaries" and "read_json() will give an error!"

**Q22.** What function should you use to load API response JSON to a DataFrame?
- [ ] A) pd.read_json()
- [ ] B) pd.from_json()
- [ ] C) pd.DataFrame()
- [ ] D) pd.load_dict()

> [!success]- Answer
> **Correct Answer: C) pd.DataFrame()**
> 
> Use `pd.DataFrame()` to load response JSON (dictionaries) to a DataFrame.

**Q23.** When is JSON considered "nested"?
- [ ] A) When it has many attributes
- [ ] B) When the value itself is an object
- [ ] C) When it's in a file
- [ ] D) When it uses arrays

> [!success]- Answer
> **Correct Answer: B) When the value itself is an object**
> 
> "A JSON is nested when the value itself is an object"

**Q24.** Where is the `json_normalize()` function located?
- [ ] A) In the main pandas module
- [ ] B) In pandas.io.json submodule
- [ ] C) In the requests library
- [ ] D) In the json module

> [!success]- Answer
> **Correct Answer: B) In pandas.io.json submodule**
> 
> `json_normalize()` is in the `pandas.io.json` submodule and needs its own import.

**Q25.** Does `pandas.io.json` need its own import statement?
- [ ] A) No, it's imported with pandas
- [ ] B) Yes, it needs its own import statement
- [ ] C) Only for some functions
- [ ] D) Only in Python 2

> [!success]- Answer
> **Correct Answer: B) Yes, it needs its own import statement**
> 
> The chapter explicitly states: "Needs its own import statement"

**Q26.** What does `json_normalize()` do?
- [ ] A) Validates JSON syntax
- [ ] B) Compresses JSON data
- [ ] C) Takes a dictionary/list of dictionaries and returns a flattened dataframe
- [ ] D) Converts JSON to CSV

> [!success]- Answer
> **Correct Answer: C) Takes a dictionary/list of dictionaries and returns a flattened dataframe**
> 
> `json_normalize()` flattens nested JSON structures into a tabular DataFrame.

**Q27.** What is the default column name pattern when `json_normalize()` flattens nested data?
- [ ] A) attribute_nestedattribute
- [ ] B) attribute-nestedattribute
- [ ] C) attribute.nestedattribute
- [ ] D) attribute::nestedattribute

> [!success]- Answer
> **Correct Answer: C) attribute.nestedattribute**
> 
> Default pattern uses a period (dot): "attribute.nestedattribute"

**Q28.** What parameter in `json_normalize()` changes the separator in flattened column names?
- [ ] A) separator
- [ ] B) sep
- [ ] C) delimiter
- [ ] D) join

> [!success]- Answer
> **Correct Answer: B) sep**
> 
> Use the `sep` argument to choose a different separator (e.g., `sep="_"`).

**Q29.** What is the use case for `pd.concat()`?
- [ ] A) Adding columns from one dataframe to another
- [ ] B) Adding rows from one dataframe to another
- [ ] C) Joining tables like SQL
- [ ] D) Sorting dataframes

> [!success]- Answer
> **Correct Answer: B) Adding rows from one dataframe to another**
> 
> The chapter states: "Use case: adding rows from one dataframe to another"

**Q30.** What is the use case for `merge()`?
- [ ] A) Adding rows from one dataframe to another
- [ ] B) Combining datasets to add related columns
- [ ] C) Removing duplicates
- [ ] D) Sorting data

> [!success]- Answer
> **Correct Answer: B) Combining datasets to add related columns**
> 
> The chapter states: "Use case: combining datasets to add related columns"

---

## Section B: True/False Questions (15 questions × 1 point = 15 points)

**Q31.** JSON is a tabular data format like CSV. **T/F**

> [!success]- Answer
> **False** - JSON is explicitly "Not tabular" according to the chapter.

**Q32.** In JSON, all records must have the same set of attributes. **T/F**

> [!success]- Answer
> **False** - "Records don't have to all have the same set of attributes" is a key feature of JSON.

**Q33.** Record orientation is the most common JSON arrangement. **T/F**

> [!success]- Answer
> **True** - The chapter explicitly states this.

**Q34.** Column-oriented JSON is more space-efficient than record-oriented JSON. **T/F**

> [!success]- Answer
> **True** - This is stated directly in the chapter.

**Q35.** An API requires knowledge of database details to get data. **T/F**

> [!success]- Answer
> **False** - APIs provide a "way to get data from an application without knowing database details"

**Q36.** The requests library is tied to a specific API. **T/F**

> [!success]- Answer
> **False** - The requests library is "Not tied to a particular API"

**Q37.** The `params` parameter in `requests.get()` takes a dictionary. **T/F**

> [!success]- Answer
> **True** - `params` takes a dictionary of parameters and values.

**Q38.** `response.json()` returns a string. **T/F**

> [!success]- Answer
> **False** - `response.json()` returns a dictionary, not a string.

**Q39.** You can use `pd.read_json()` directly with `response.json()` output. **T/F**

> [!success]- Answer
> **False** - `read_json()` expects strings, not dictionaries. This will give an error!

**Q40.** `pd.DataFrame()` should be used to load API response JSON. **T/F**

> [!success]- Answer
> **True** - Use `pd.DataFrame()` for dictionaries from API responses.

**Q41.** `json_normalize()` is part of the main pandas import. **T/F**

> [!success]- Answer
> **False** - It's in `pandas.io.json` submodule and needs its own import statement.

**Q42.** The default separator in `json_normalize()` flattened columns is an underscore. **T/F**

> [!success]- Answer
> **False** - The default separator is a period (dot): "attribute.nestedattribute"

**Q43.** `pd.concat()` is used to add rows from one dataframe to another. **T/F**

> [!success]- Answer
> **True** - This is the use case for concatenation.

**Q44.** `merge()` is the pandas version of a SQL join. **T/F**

> [!success]- Answer
> **True** - The chapter explicitly states this.

**Q45.** In a merge, key columns should be the same data type. **T/F**

> [!success]- Answer
> **True** - Just like with SQL joins, "Key columns should be the same data type"

---

## Section C: Short Answer Questions (8 questions × 3 points = 24 points)

**Q46.** List four characteristics of JSON mentioned in the chapter.

> [!success]- Answer
> The four characteristics of JSON are:
> 1. **Common web data format** - widely used on the web
> 2. **Not tabular** - unlike CSV or Excel files
> 3. **Records don't have to all have the same set of attributes** - flexible structure
> 4. **Data organized into collections of objects** - objects are collections of attribute-value pairs
> 
> Additionally, JSON supports nested structures (objects within objects).

**Q47.** Explain the three JSON orientation types mentioned in the chapter.

> [!success]- Answer
> The three JSON orientation types are:
> 
> 1. **Record Orientation (most common)**
>    - List of objects with attribute-value pairs
>    - Each object is one record
>    - Example: `[{"name": "John", "age": 30}, {"name": "Jane", "age": 25}]`
> 
> 2. **Column Orientation (more space-efficient)**
>    - Object of objects, organized by column
>    - Each attribute maps to an object of row indices and values
>    - Example: `{"name": {"0": "John", "1": "Jane"}, "age": {"0": 30, "1": 25}}`
> 
> 3. **Split Orientation**
>    - Separates columns, index, and data
>    - Requires `orient="split"` parameter
>    - Example: `{"columns": ["name", "age"], "index": [0, 1], "data": [["John", 30], ["Jane", 25]]}`

**Q48.** What are the two keyword arguments for `requests.get()` and what do they do?

> [!success]- Answer
> The two keyword arguments for `requests.get()` are:
> 
> 1. **`params`**
>    - Takes a dictionary of parameters and values
>    - Used to customize the API request
>    - Adds query parameters to the URL
>    - Example: `params={"term": "bookstore", "location": "San Francisco"}`
> 
> 2. **`headers`**
>    - Takes a dictionary
>    - Can be used to provide user authentication to the API
>    - Often contains API keys or tokens
>    - Example: `headers={"Authorization": "Bearer YOUR_API_KEY"}`

**Q49.** Explain why you can't use `pd.read_json()` with API response data and what you should use instead.

> [!success]- Answer
> **Why you can't use `pd.read_json()`:**
> - `response.json()` returns a **dictionary**
> - `pd.read_json()` expects **strings** (file paths or JSON strings), not dictionaries
> - Using `pd.read_json()` with a dictionary will give an error
> 
> **What to use instead:**
> - Use **`pd.DataFrame()`** to load API response dictionaries
> - Example:
> ```python
> data = response.json()
> df = pd.DataFrame(data["businesses"])  # Correct approach
> ```
> 
> **Note:** You can use `pd.read_json()` with JSON files or JSON strings, just not with the dictionary output from `response.json()`.

**Q50.** What is `json_normalize()` and where is it located? Why does it need special handling?

> [!success]- Answer
> **What it is:**
> - A function that takes a dictionary/list of dictionaries and returns a flattened DataFrame
> - Specifically designed to handle nested JSON data
> - Converts nested structures (objects within objects) into flat columns
> 
> **Where it's located:**
> - In the `pandas.io.json` submodule
> - NOT in the main pandas import
> 
> **Why special handling:**
> - **Needs its own import statement**: `from pandas.io.json import json_normalize`
> - Cannot be accessed with just `import pandas as pd`
> - This is different from most pandas functions which are available directly after importing pandas

**Q51.** Explain the three advanced parameters of `json_normalize()` for deeply nested data.

> [!success]- Answer
> The three advanced parameters for deeply nested data are:
> 
> 1. **`record_path`**
>    - String or list of string attributes pointing to the nested data you want to flatten
>    - Specifies which nested element to use as the main records
>    - Example: `record_path="categories"` flattens the categories array
> 
> 2. **`meta`**
>    - List of other attributes to bring into the DataFrame from the parent level
>    - Preserves non-nested data alongside the flattened nested data
>    - Can use nested lists for nested attributes: `["name", ["coordinates", "latitude"]]`
> 
> 3. **`meta_prefix`**
>    - String to prefix to meta column names
>    - Helps distinguish meta columns from record_path columns
>    - Example: `meta_prefix="biz_"` creates columns like `biz_name`, `biz_rating`

**Q52.** Explain the difference between `pd.concat()` and `merge()`, including their use cases.

> [!success]- Answer
> **`pd.concat()`:**
> - **Use case:** Adding rows from one dataframe to another (vertical stacking)
> - **Purpose:** Combine DataFrames that have the same columns
> - **Syntax:** `pd.concat([df1, df2])`
> - **Key parameter:** `ignore_index=True` to renumber rows
> - **Result:** More rows, same columns
> - **Example:** Combining multiple pages of API results
> 
> **`merge()`:**
> - **Use case:** Combining datasets to add related columns (horizontal joining)
> - **Purpose:** Join DataFrames that have related data via key columns
> - **Syntax:** `df1.merge(df2, on="key")` or `df1.merge(df2, left_on="key1", right_on="key2")`
> - **Requirement:** Datasets must have key column(s) with common values
> - **Result:** Same rows (matched), more columns
> - **Note:** Pandas version of a SQL join

**Q53.** What is the default behavior of `merge()` and what requirement must be met for keys?

> [!success]- Answer
> **Default `merge()` behavior:**
> 1. **Returns only values that are in both datasets** (inner join)
> 2. **One record for each value match** between dataframes
> 3. **Multiple matches = multiple records** in the result
> 
> **Key requirement:**
> - **Key columns must be the same data type**
> - If one key is a string and the other is an integer, they won't match even if they look similar
> - This is the same requirement as SQL joins
> 
> **Example issue:** If `created_date` is a string "01/01/2018" in one DataFrame and a datetime object in another, the merge will fail to find matches.

---

## Section D: Code Analysis & Scenarios (5 questions × 2 points = 10 points)

**Q54.** What does this code do?
```python
from pandas.io.json import json_normalize

bookstores = json_normalize(data["businesses"], sep="_")
```

> [!success]- Answer
> This code:
> 1. **Imports** the `json_normalize` function from the `pandas.io.json` submodule
> 2. **Flattens** the nested JSON data in `data["businesses"]`
> 3. **Creates a DataFrame** with nested attributes as separate columns
> 4. **Uses underscore as separator** for nested column names (e.g., `coordinates_latitude` instead of `coordinates.latitude`)
> 
> **Result:** Nested structures like `{"coordinates": {"latitude": 37.79, "longitude": -122.40}}` become separate columns `coordinates_latitude` and `coordinates_longitude`.

**Q55.** Identify the issue in this code:
```python
response = requests.get(api_url, params=params, headers=headers)
data = response.json()
df = pd.read_json(data)
```

> [!success]- Answer
> **Issue:** Using `pd.read_json()` with a dictionary from `response.json()`
> 
> **Problem:**
> - `response.json()` returns a **dictionary**
> - `pd.read_json()` expects a **string** (file path or JSON string)
> - This code will raise an error
> 
> **Fix:**
> ```python
> response = requests.get(api_url, params=params, headers=headers)
> data = response.json()
> df = pd.DataFrame(data["businesses"])  # Use DataFrame(), not read_json()
> ```

**Q56.** Complete this code to flatten deeply nested categories data while bringing in business name and rating with a "biz_" prefix:
```python
df = json_normalize(data["businesses"],
                   sep="_",
                   record_path=_______,
                   meta=_______,
                   meta_prefix=_______)
```

> [!success]- Answer
> ```python
> df = json_normalize(data["businesses"],
>                    sep="_",
>                    record_path="categories",
>                    meta=["name", "rating"],
>                    meta_prefix="biz_")
> ```
> 
> **Explanation:**
> - `record_path="categories"` - flattens the nested categories array
> - `meta=["name", "rating"]` - brings in these attributes from the parent business level
> - `meta_prefix="biz_"` - prefixes meta columns as `biz_name` and `biz_rating`

**Q57.** What will be the shape of the resulting DataFrame?
```python
first_df = pd.DataFrame({"A": [1, 2, 3], "B": [4, 5, 6]})  # Shape: (3, 2)
second_df = pd.DataFrame({"A": [7, 8], "B": [9, 10]})      # Shape: (2, 2)

combined = pd.concat([first_df, second_df], ignore_index=True)
print(combined.shape)
```

> [!success]- Answer
> **Output: (5, 2)**
> 
> **Explanation:**
> - `pd.concat()` stacks DataFrames vertically (adds rows)
> - `first_df` has 3 rows, `second_df` has 2 rows
> - Combined: 3 + 2 = **5 rows**
> - Both have 2 columns (A and B), so result has **2 columns**
> - `ignore_index=True` renumbers rows from 0-4
> 
> Result:
> ```
>    A   B
> 0  1   4
> 1  2   5
> 2  3   6
> 3  7   9
> 4  8  10
> ```

**Q58.** Complete this merge to combine call_counts and weather DataFrames where the date columns have different names:
```python
# call_counts has "created_date" column
# weather has "date" column

merged = call_counts.merge(weather,
                          _______="created_date",
                          _______="date")
```

> [!success]- Answer
> ```python
> merged = call_counts.merge(weather,
>                           left_on="created_date",
>                           right_on="date")
> ```
> 
> **Explanation:**
> - Use `left_on` for the key column name in the left DataFrame (call_counts)
> - Use `right_on` for the key column name in the right DataFrame (weather)
> - This joins records where `call_counts.created_date` equals `weather.date`
> - If column names were the same, you could just use `on="column_name"`

---

## Answer Key & Scoring

### Section A: Multiple Choice (60 points)
1. B  |  2. C  |  3. B  |  4. C  |  5. C  
2. C  |  7. C  |  8. C  |  9. B  |  10. C  
3. B  |  12. B  |  13. C  |  14. C  |  15. C  
4. B  |  17. C  |  18. C  |  19. B  |  20. C  
5. B  |  22. C  |  23. B  |  24. B  |  25. B  
6. C  |  27. C  |  28. B  |  29. B  |  30. B  

### Section B: True/False (15 points)
31. F  |  32. F  |  33. T  |  34. T  |  35. F  
32. F  |  37. T  |  38. F  |  39. F  |  40. T  
33. F  |  42. F  |  43. T  |  44. T  |  45. T  

### Section C: Short Answer (24 points)
- Q46-Q53: See detailed answers in collapsible sections (3 points each)

### Section D: Code Analysis (10 points)
- Q54-Q58: See detailed answers in collapsible sections (2 points each)

---

## Quick Reference Guide

### JSON Reading Patterns
```python
import pandas as pd

# Read JSON file
df = pd.read_json("file.json")

# Read with specific orientation
df = pd.read_json("file.json", orient="split")

# Specify data types
df = pd.read_json("file.json", dtype={"column": str})
```

### API Request Pattern
```python
import requests
import pandas as pd

# Make API request
response = requests.get(api_url,
                       params={"key": "value"},
                       headers={"Authorization": "Bearer TOKEN"})

# Extract JSON and create DataFrame
data = response.json()
df = pd.DataFrame(data["key"])  # Use DataFrame(), not read_json()!
```

### Nested JSON Flattening
```python
from pandas.io.json import json_normalize

# Basic flattening
df = json_normalize(data, sep="_")

# Deeply nested with meta
df = json_normalize(data["items"],
                   sep="_",
                   record_path="nested_list",
                   meta=["parent_attr1", "parent_attr2"],
                   meta_prefix="parent_")
```

### Combining DataFrames
```python
# Concatenate (add rows)
combined = pd.concat([df1, df2], ignore_index=True)

# Merge (add columns) - same key name
merged = df1.merge(df2, on="key_column")

# Merge - different key names
merged = df1.merge(df2, left_on="key1", right_on="key2")
```

---

## Key Concepts Summary

| Concept | Key Points |
|---------|-----------|
| **JSON** | Not tabular; objects with attribute-value pairs; records can differ |
| **JSON Orientations** | Record (most common), Column (space-efficient), Split |
| **pd.read_json()** | For JSON files/strings; has `orient` parameter |
| **API** | Way to get data without knowing database details |
| **requests.get()** | Gets data from URLs; uses `params` and `headers` |
| **response.json()** | Returns dictionary (not string!) |
| **pd.DataFrame()** | Use for API response dicts, NOT read_json() |
| **Nested JSON** | Value itself is an object |
| **json_normalize()** | In pandas.io.json; flattens nested data; needs own import |
| **Separators** | Default is `.` (dot); change with `sep` parameter |
| **record_path** | Points to nested data to flatten |
| **meta** | Parent-level attributes to preserve |
| **meta_prefix** | Prefix for meta columns |
| **concat()** | Adds rows (vertical); use `ignore_index=True` |
| **merge()** | Adds columns (horizontal); pandas SQL join |
| **Merge keys** | Must be same data type |

### JSON vs Other Formats

| Feature | JSON | CSV | Excel | SQL |
|---------|------|-----|-------|-----|
| Tabular | No | Yes | Yes | Yes |
| Nested data | Yes | No | No | No |
| Uniform attributes | No | Yes | Yes | Yes |
| pandas function | read_json() | read_csv() | read_excel() | read_sql() |

---

## Common Mistakes to Avoid

❌ Thinking JSON is tabular like CSV  
❌ Assuming all JSON records have the same attributes  
❌ Using `pd.read_json()` with `response.json()` output (it's a dict!)  
❌ Forgetting to import `json_normalize` from `pandas.io.json`  
❌ Not specifying `sep` parameter in `json_normalize()` (default is `.` not `_`)  
❌ Using `concat()` when you need `merge()`, or vice versa  
❌ Forgetting `ignore_index=True` when concatenating  
❌ Not ensuring merge keys are the same data type  
❌ Using `on` when key names differ (use `left_on`/`right_on`)  
❌ Forgetting that `params` and `headers` in `requests.get()` take dictionaries  
❌ Not extracting data from response object with `response.json()`  
❌ Confusing record vs column vs split orientations  

---

## Study Strategy (3-Day Plan)

### Day 1: JSON Basics and APIs
- Review JSON characteristics (not tabular, flexible structure)
- Learn three orientation types (record, column, split)
- Practice `pd.read_json()` with different orientations
- Understand APIs and their purpose
- Work with `requests.get()` and its parameters
- Practice extracting data with `response.json()`
- Remember: Use `pd.DataFrame()` for API responses, not `read_json()`!

### Day 2: Nested JSON and json_normalize()
- Understand what makes JSON "nested"
- Import `json_normalize` from `pandas.io.json`
- Practice basic flattening with `sep` parameter
- Work with deeply nested data using `record_path`, `meta`, and `meta_prefix`
- Understand default separator is `.` (dot)
- Create column name patterns with different separators

### Day 3: Combining DataFrames
- Master `pd.concat()` for adding rows
- Understand `ignore_index=True` parameter
- Practice `merge()` for adding columns
- Use `on` for same-named keys
- Use `left_on`/`right_on` for different key names
- Ensure merge keys are same data type
- Complete practice quiz and review

---

## Pre-Exam Checklist

✅ Know JSON is not tabular  
✅ Remember records can have different attributes  
✅ Identify record (most common) vs column (space-efficient) orientations  
✅ Know `orient` parameter flags uncommon layouts  
✅ Understand APIs allow data access without database details  
✅ Know `requests.get()` uses `params` and `headers` dictionaries  
✅ Remember `response.json()` returns a dictionary  
✅ Never use `pd.read_json()` with `response.json()` output  
✅ Use `pd.DataFrame()` for API response dictionaries  
✅ Import `json_normalize` from `pandas.io.json` (needs own import!)  
✅ Default separator is `.` (dot), change with `sep="_"`  
✅ Know three advanced parameters: `record_path`, `meta`, `meta_prefix`  
✅ `concat()` adds rows (vertical), `merge()` adds columns (horizontal)  
✅ Use `ignore_index=True` with concat  
✅ Merge keys must be same data type  
✅ Use `on` for same key names, `left_on`/`right_on` for different names  

---

## During the Exam

💡 **JSON structure** - Not tabular, flexible attributes  
💡 **Orientations** - Record is most common  
💡 **API responses** - Use `pd.DataFrame()`, not `read_json()`!  
💡 **response.json()** - Returns dictionary, not string  
💡 **json_normalize import** - From `pandas.io.json`, needs separate import  
💡 **Default separator** - Dot (`.`), not underscore  
💡 **Nested JSON** - Value itself is an object  
💡 **concat vs merge** - concat=rows, merge=columns  
💡 **ignore_index** - Renumbers rows in concat  
💡 **Merge keys** - Must be same data type  
💡 **Key names** - Same: use `on`; Different: use `left_on`/`right_on`  

---

## Course Complete! 🎉

You've now covered all four chapters:
1. **Flat Files** - CSV/TSV with `read_csv()`
2. **Spreadsheets** - Excel with `read_excel()`, Booleans, datetimes
3. **Databases** - SQL with `read_sql()`, queries, joins
4. **JSON & APIs** - `read_json()`, APIs with `requests`, nested data with `json_normalize()`, combining data

**Next Steps:**
- Complete this practice quiz
- Review any weak areas
- Request comprehensive quiz combining all chapters (if desired)
- Continue learning with:
  - Data Manipulation with pandas
  - Exploratory Data Analysis
  - Data Visualization
  - Statistical analysis courses

---

#datacamp #pandas #json #apis #requests #data-ingestion #json-normalize #merge #concat #certification #exam-prep