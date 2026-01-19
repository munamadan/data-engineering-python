## Overview
This chapter covers importing data from Excel files using pandas, including working with multiple sheets, handling Boolean data, and parsing dates.

## Key Concepts

### Spreadsheets vs Flat Files
- **Also known as**: Excel files
- **Storage**: Data stored in tabular form with cells arranged in rows and columns
- **Key difference**: Unlike flat files, can have formatting and formulas
- **Workbooks**: Multiple spreadsheets can exist in a single workbook

### Loading Function
- **Function**: `pd.read_excel()` - spreadsheets have their own loading function in pandas
- **Default behavior**: Loads the first sheet in an Excel file

## Basic Spreadsheet Import

```python
import pandas as pd

# Read the Excel file
survey_data = pd.read_excel("fcc_survey.xlsx")

# View the first 5 lines of data
print(survey_data.head())
```

## Loading Select Columns and Rows

### Common Parameters with read_csv()
`read_excel()` shares many keyword arguments with `read_csv()`:

| Parameter | Purpose |
|-----------|---------|
| `nrows` | Limit number of rows to load |
| `skiprows` | Specify number of rows or row numbers to skip |
| `usecols` | Choose columns by name, positional number, or **letter** (e.g., "A:P") |

> [!info] Excel-Specific Feature
> With `read_excel()`, `usecols` can also accept Excel column letters like "W:AB, AR"

### Example: Loading Specific Columns and Rows
```python
# Read columns W-AB and AR of file, skipping metadata header
survey_data = pd.read_excel("fcc_survey_with_headers.xlsx",
                            skiprows=2,
                            usecols="W:AB, AR")

print(survey_data.head())
```

## Getting Data from Multiple Worksheets

### Selecting Sheets to Load

#### Using sheet_name Parameter
- **Default**: `read_excel()` loads the first sheet by default
- **Parameter**: Use `sheet_name` keyword argument to load other sheets
- **Specification options**:
  - By name (string)
  - By position number (zero-indexed integer)
  - List of names/numbers to load multiple sheets
- **Note**: Any arguments passed to `read_excel()` apply to all sheets read

#### Loading by Position or Name
```python
# Get the second sheet by position index (zero-indexed)
survey_data_sheet2 = pd.read_excel('fcc_survey.xlsx', sheet_name=1)

# Get the second sheet by name
survey_data_2017 = pd.read_excel('fcc_survey.xlsx', sheet_name='2017')

# Both methods produce identical results
print(survey_data_sheet2.equals(survey_data_2017))  # True
```

#### Loading All Sheets
```python
# Pass sheet_name=None to read all sheets
survey_responses = pd.read_excel("fcc_survey.xlsx", sheet_name=None)

print(type(survey_responses))
# <class 'collections.OrderedDict'>

# Iterate through the OrderedDict
for key, value in survey_responses.items():
    print(key, type(value))
# 2016 <class 'pandas.core.frame.DataFrame'>
# 2017 <class 'pandas.core.frame.DataFrame'>
```

> [!important] Return Type
> When `sheet_name=None`, `read_excel()` returns an `OrderedDict`, not a DataFrame

### Combining Multiple Sheets

```python
# Create empty dataframe to hold all loaded sheets
all_responses = pd.DataFrame()

# Iterate through dataframes in dictionary
for sheet_name, frame in survey_responses.items():
    # Add a column so we know which year data is from
    frame["Year"] = sheet_name
    
    # Add the dataframe to all_responses
    all_responses = pd.concat([all_responses, frame])

# View years in data
print(all_responses.Year.unique())
# ['2016' '2017']
```

## Modifying Imports: True/False Data

### Boolean Data Basics
- **Definition**: True/False data
- **Default behavior**: pandas loads True/False columns as **float** data by default

### pandas and Booleans - Key Issues

#### Problem with Default Loading
```python
bootcamp_data = pd.read_excel("fcc_survey_booleans.xlsx")
print(bootcamp_data.dtypes)
# AttendedBootcamp    float64  # Should be bool!
# LoanYesNo           object
```

#### Issues with Float Boolean Columns:
1. **NA/missing values in Boolean columns are changed to True**
2. **Unrecognized values in a Boolean column are also changed to True**

### Specifying Boolean Data Types

```python
# Load data, casting True/False columns as Boolean
bool_data = pd.read_excel("fcc_survey_booleans.xlsx",
                          dtype={"AttendedBootcamp": bool,
                                 "AttendedBootCampYesNo": bool,
                                 "BootcampLoan": bool,
                                 "LoanYesNo": bool})

print(bool_data.dtypes)
# All specified columns now show: bool
```

> [!warning] Boolean Column Requirements
> - Boolean columns can **only** have True and False values
> - pandas automatically recognizes some values as True/False in Boolean columns
> - Use `dtype` argument (same as with `read_csv()`)

### Setting Custom True/False Values

#### Parameters
- **`true_values`**: Set custom values to be treated as True (accepts a list)
- **`false_values`**: Set custom values to be treated as False (accepts a list)
- **Important**: Custom True/False values are **only applied to columns set as Boolean**

```python
# Load data with Boolean dtypes and custom T/F values
bool_data = pd.read_excel("fcc_survey_booleans.xlsx",
                          dtype={"AttendedBootCampYesNo": bool,
                                 "LoanYesNo": bool},
                          true_values=["Yes"],
                          false_values=["No"])

print(bool_data.sum())  # Count True values
```

### Boolean Considerations
Questions to ask before using Boolean types:
1. Are there missing values, or could there be in the future?
2. How will this column be used in analysis?
3. What would happen if a value were incorrectly coded as True?
4. Could the data be modeled another way (e.g., as floats or integers)?

## Modifying Imports: Parsing Dates

### Date and Time Data Basics
- **Data type**: Dates and times have their own data type and internal representation
- **String conversion**: Datetime values can be translated into string representations
- **Formatting codes**: Common set of codes to describe datetime string formatting

### pandas and Datetimes

#### Default Behavior
- **Datetime columns are loaded as objects (strings) by default**

#### Specifying Datetime Columns
- **Parameter**: Use `parse_dates` argument (**not** `dtype`!)
- **Accepts**:
  1. A list of column names or numbers to parse
  2. A list containing lists of columns to combine and parse
  3. A dictionary where keys are new column names and values are lists of columns to parse together

### Parsing Dates - Method 1: Simple List

```python
# List columns of dates to parse
date_cols = ["Part1StartTime", "Part1EndTime"]

# Load file, parsing standard datetime columns
survey_df = pd.read_excel("fcc_survey.xlsx", parse_dates=date_cols)

# Check data types
print(survey_df[["Part1StartTime", "Part1EndTime"]].dtypes)
# Part1StartTime    datetime64[ns]
# Part1EndTime      datetime64[ns]
```

### Parsing Dates - Method 2: Combining Columns

```python
# List includes nested list for columns to combine
date_cols = ["Part1StartTime",
             "Part1EndTime",
             [["Part2StartDate", "Part2StartTime"]]]

# Load file, parsing standard and split datetime columns
survey_df = pd.read_excel("fcc_survey.xlsx", parse_dates=date_cols)

# Combined column name: Part2StartDate_Part2StartTime
print(survey_df.head(3))
```

> [!info] Combined Column Naming
> When combining columns, pandas creates a new column with names joined by underscores

### Parsing Dates - Method 3: Dictionary with Custom Names

```python
# Dictionary: keys are new column names, values are columns to parse
date_cols = {"Part1Start": "Part1StartTime",
             "Part1End": "Part1EndTime",
             "Part2Start": ["Part2StartDate", "Part2StartTime"]}

# Load file with custom column names
survey_df = pd.read_excel("fcc_survey.xlsx", parse_dates=date_cols)

print(survey_df.Part2Start.head(3))
# 0   2016-03-29 21:24:57
# 1   2016-03-29 21:27:14
# 2   2016-03-29 21:27:13
# Name: Part2Start, dtype: datetime64[ns]
```

### Non-Standard Dates

#### When parse_dates Doesn't Work
- **Limitation**: `parse_dates` doesn't work with non-standard datetime formats
- **Solution**: Use `pd.to_datetime()` after loading data

#### pd.to_datetime() Arguments
- Dataframe and column to convert
- **`format`**: String representation of datetime format

### Datetime Formatting Codes

Describe datetime string formatting with codes and characters. Refer to **strftime.org** for the full list.

| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | Year (4-digit) | 1999 |
| `%m` | Month (zero-padded) | 03 |
| `%d` | Day (zero-padded) | 01 |
| `%H` | Hour (24-hour clock) | 21 |
| `%M` | Minute (zero-padded) | 09 |
| `%S` | Second (zero-padded) | 05 |

### Parsing Non-Standard Dates Example

```python
# Define the format string
format_string = "%m%d%Y %H:%M:%S"

# Parse the non-standard datetime column
survey_df["Part2EndTime"] = pd.to_datetime(survey_df["Part2EndTime"],
                                           format=format_string)

print(survey_df.Part2EndTime.head())
# 0   2016-03-29 21:27:25
# 1   2016-03-29 21:29:10
# Name: Part2EndTime, dtype: datetime64[ns]
```

## Key Parameters Summary

| Parameter | Purpose | Specific to read_excel() |
|-----------|---------|--------------------------|
| `sheet_name` | Select which sheet(s) to load | ✓ Yes |
| `usecols` | Select columns (can use Excel letters) | Partial - letters are Excel-specific |
| `dtype` | Specify data types (including bool) | No - shared with read_csv() |
| `true_values` | Set custom True values for Boolean columns | ✓ Yes |
| `false_values` | Set custom False values for Boolean columns | ✓ Yes |
| `parse_dates` | Parse datetime columns | No - shared with read_csv() |

## Important Distinctions

| Feature | read_csv() | read_excel() |
|---------|-----------|--------------|
| File type | Flat files | Spreadsheets/Excel |
| Can have formatting | No | Yes |
| Multiple sheets | No | Yes |
| `usecols` letters | No | Yes (e.g., "A:P") |
| Boolean handling | Same | Same |
| Date parsing | Same | Same |

---

#datacamp #pandas #data-ingestion #excel #spreadsheets #boolean #datetime

## Section A: Multiple Choice Questions (30 questions × 2 points = 60 points)

### Topic 1: Spreadsheets and Basic Loading (10 questions)

**Q1.** What is another name for spreadsheets in the context of this chapter?
- [ ] A) CSV files
- [ ] B) Excel files
- [ ] C) Flat files
- [ ] D) Text files

> [!success]- Answer
> **Correct Answer: B) Excel files**
> 
> The chapter explicitly states "Spreadsheets: Also known as Excel files"

**Q2.** What is a key difference between spreadsheets and flat files?
- [ ] A) Spreadsheets can't store data in rows and columns
- [ ] B) Spreadsheets can have formatting and formulas, flat files cannot
- [ ] C) Spreadsheets are always larger than flat files
- [ ] D) Spreadsheets can only store text data

> [!success]- Answer
> **Correct Answer: B) Spreadsheets can have formatting and formulas, flat files cannot**
> 
> The chapter states: "Unlike flat files, can have formatting and formulas"

**Q3.** Can multiple spreadsheets exist in a single workbook?
- [ ] A) No, only one spreadsheet per workbook
- [ ] B) Yes, multiple spreadsheets can exist in a workbook
- [ ] C) Only in special Excel formats
- [ ] D) Yes, but only up to 2 spreadsheets

> [!success]- Answer
> **Correct Answer: B) Yes, multiple spreadsheets can exist in a workbook**
> 
> The chapter states: "Multiple spreadsheets can exist in a workbook"

**Q4.** Which pandas function is used to load spreadsheets?
- [ ] A) pd.read_csv()
- [ ] B) pd.load_excel()
- [ ] C) pd.read_excel()
- [ ] D) pd.import_excel()

> [!success]- Answer
> **Correct Answer: C) pd.read_excel()**
> 
> The chapter states: "Spreadsheets have their own loading function in pandas: read_excel()"

**Q5.** What does `read_excel()` load by default?
- [ ] A) All sheets in the workbook
- [ ] B) The last sheet
- [ ] C) The first sheet in an Excel file
- [ ] D) No sheets until specified

> [!success]- Answer
> **Correct Answer: C) The first sheet in an Excel file**
> 
> The chapter explicitly states: "read_excel() loads the first sheet in an Excel file by default"

**Q6.** How is data stored in spreadsheets?
- [ ] A) In linear format
- [ ] B) In tabular form, with cells arranged in rows and columns
- [ ] C) In JSON format
- [ ] D) In binary format only

> [!success]- Answer
> **Correct Answer: B) In tabular form, with cells arranged in rows and columns**
> 
> This is the definition provided in the chapter for how spreadsheet data is organized.

**Q7.** Which parameters does `read_excel()` have in common with `read_csv()`?
- [ ] A) Only nrows
- [ ] B) Only usecols
- [ ] C) nrows, skiprows, and usecols
- [ ] D) None, they're completely different

> [!success]- Answer
> **Correct Answer: C) nrows, skiprows, and usecols**
> 
> The chapter states: "read_excel() has many keyword arguments in common with read_csv()" and lists these three specifically.

**Q8.** What is unique about the `usecols` parameter in `read_excel()` compared to `read_csv()`?
- [ ] A) It doesn't work in read_excel()
- [ ] B) It can accept Excel column letters like "A:P"
- [ ] C) It only accepts column numbers
- [ ] D) It's faster in read_excel()

> [!success]- Answer
> **Correct Answer: B) It can accept Excel column letters like "A:P"**
> 
> The chapter shows `usecols` can choose columns "by name, positional number, or letter (e.g. 'A:P')" - the letter option is Excel-specific.

**Q9.** What does this code do: `pd.read_excel("file.xlsx", skiprows=2, usecols="W:AB, AR")`?
- [ ] A) Loads first 2 rows from columns W to AB
- [ ] B) Skips first 2 rows and loads columns W through AB plus column AR
- [ ] C) Loads row 2 from specified columns
- [ ] D) Skips columns W through AB

> [!success]- Answer
> **Correct Answer: B) Skips first 2 rows and loads columns W through AB plus column AR**
> 
> The `skiprows=2` skips the first 2 rows (often metadata/headers), and `usecols="W:AB, AR"` specifies which Excel columns to load.

**Q10.** What is the basic syntax for loading an Excel file named "data.xlsx"?
- [ ] A) pd.load_excel("data.xlsx")
- [ ] B) pd.read_excel("data.xlsx")
- [ ] C) pd.import_excel("data.xlsx")
- [ ] D) pd.excel_read("data.xlsx")

> [!success]- Answer
> **Correct Answer: B) pd.read_excel("data.xlsx")**
> 
> This is the standard syntax shown throughout the chapter.

### Topic 2: Multiple Worksheets (10 questions)

**Q11.** What parameter is used to select which sheet to load from an Excel file?
- [ ] A) sheet
- [ ] B) worksheet
- [ ] C) sheet_name
- [ ] D) select_sheet

> [!success]- Answer
> **Correct Answer: C) sheet_name**
> 
> The `sheet_name` keyword argument is used to load specific sheets.

**Q12.** How can you specify spreadsheets to load with `sheet_name`?
- [ ] A) By name only
- [ ] B) By position number only
- [ ] C) By name and/or zero-indexed position number
- [ ] D) By column letter

> [!success]- Answer
> **Correct Answer: C) By name and/or zero-indexed position number**
> 
> The chapter states: "Specify spreadsheets by name and/or (zero-indexed) position number"

**Q13.** What does `sheet_name=1` load?
- [ ] A) The first sheet
- [ ] B) The second sheet (zero-indexed)
- [ ] C) Sheet named "1"
- [ ] D) An error occurs

> [!success]- Answer
> **Correct Answer: B) The second sheet (zero-indexed)**
> 
> Position numbers are zero-indexed, so 0 is the first sheet, 1 is the second sheet, etc.

**Q14.** If you load a sheet both by position (`sheet_name=1`) and by its name (`sheet_name='2017'`), assuming they refer to the same sheet, will the DataFrames be equal?
- [ ] A) No, they'll be different
- [ ] B) Yes, they'll be identical
- [ ] C) Only the data will match, not the structure
- [ ] D) It will cause an error

> [!success]- Answer
> **Correct Answer: B) Yes, they'll be identical**
> 
> The chapter demonstrates:
> ```python
> print(survey_data_sheet2.equals(survey_data_2017))
> True
> ```

**Q15.** How do you load all sheets from an Excel workbook at once?
- [ ] A) sheet_name="all"
- [ ] B) sheet_name=None
- [ ] C) sheet_name=True
- [ ] D) sheet_name=*

> [!success]- Answer
> **Correct Answer: B) sheet_name=None**
> 
> The chapter states: "Passing sheet_name=None to read_excel() reads all sheets in a workbook"

**Q16.** What data type does `read_excel()` return when `sheet_name=None`?
- [ ] A) DataFrame
- [ ] B) List
- [ ] C) OrderedDict
- [ ] D) Dictionary

> [!success]- Answer
> **Correct Answer: C) OrderedDict**
> 
> When loading all sheets, the return type is `<class 'collections.OrderedDict'>`

**Q17.** In the OrderedDict returned by `sheet_name=None`, what are the keys and values?
- [ ] A) Keys are column names, values are data
- [ ] B) Keys are row numbers, values are DataFrames
- [ ] C) Keys are sheet names, values are DataFrames
- [ ] D) Keys are file names, values are sheets

> [!success]- Answer
> **Correct Answer: C) Keys are sheet names, values are DataFrames**
> 
> The chapter shows:
> ```python
> for key, value in survey_responses.items():
>     print(key, type(value))
> # 2016 <class 'pandas.core.frame.DataFrame'>
> # 2017 <class 'pandas.core.frame.DataFrame'>
> ```

**Q18.** Can you pass a list to `sheet_name` to load multiple specific sheets?
- [ ] A) No, only one sheet at a time
- [ ] B) Yes, you can pass a list of names/numbers
- [ ] C) Only for sheets with numeric names
- [ ] D) Only in newer pandas versions

> [!success]- Answer
> **Correct Answer: B) Yes, you can pass a list of names/numbers**
> 
> The chapter states: "Pass a list of names/numbers to load more than one sheet at a time"

**Q19.** If you pass arguments like `nrows=100` when loading multiple sheets, what happens?
- [ ] A) Only the first sheet uses the argument
- [ ] B) It causes an error
- [ ] C) The arguments apply to all sheets read
- [ ] D) You must specify separately for each sheet

> [!success]- Answer
> **Correct Answer: C) The arguments apply to all sheets read**
> 
> The chapter states: "Any arguments passed to read_excel() apply to all sheets read"

**Q20.** What pandas function is used to combine multiple DataFrames vertically (stack them)?
- [ ] A) pd.merge()
- [ ] B) pd.concat()
- [ ] C) pd.join()
- [ ] D) pd.append()

> [!success]- Answer
> **Correct Answer: B) pd.concat()**
> 
> The chapter demonstrates using `pd.concat([all_responses, frame])` to combine DataFrames from multiple sheets.

### Topic 3: Boolean and Datetime Data (10 questions)

**Q21.** How does pandas load True/False columns by default?
- [ ] A) As boolean type
- [ ] B) As float data
- [ ] C) As integer data
- [ ] D) As string/object data

> [!success]- Answer
> **Correct Answer: B) As float data**
> 
> The chapter explicitly states: "pandas loads True/False columns as float data by default"

**Q22.** What parameter is used to specify that a column should be Boolean?
- [ ] A) bool_cols
- [ ] B) dtype
- [ ] C) boolean
- [ ] D) type

> [!success]- Answer
> **Correct Answer: B) dtype**
> 
> Use `dtype={"ColumnName": bool}` to specify Boolean columns, just like with `read_csv()`

**Q23.** What can Boolean columns contain?
- [ ] A) True, False, and NA values
- [ ] B) Only True and False values
- [ ] C) Any numeric values
- [ ] D) True, False, 0, and 1

> [!success]- Answer
> **Correct Answer: B) Only True and False values**
> 
> The chapter states: "Boolean columns can only have True and False values"

**Q24.** What happens to NA/missing values when a column is set to Boolean type?
- [ ] A) They remain as NA
- [ ] B) They are changed to True
- [ ] C) They are changed to False
- [ ] D) They cause an error

> [!success]- Answer
> **Correct Answer: B) They are changed to True**
> 
> This is one of the key issues mentioned: "NA/missing values in Boolean columns are changed to True"

**Q25.** What happens to unrecognized values in a Boolean column?
- [ ] A) They cause an error
- [ ] B) They remain as strings
- [ ] C) They are changed to True
- [ ] D) They are changed to False

> [!success]- Answer
> **Correct Answer: C) They are changed to True**
> 
> The chapter warns: "Unrecognized values in a Boolean column are also changed to True"

**Q26.** What parameter sets custom values to be treated as True?
- [ ] A) true_vals
- [ ] B) custom_true
- [ ] C) true_values
- [ ] D) set_true

> [!success]- Answer
> **Correct Answer: C) true_values**
> 
> Use `true_values` parameter, which accepts a list of values

**Q27.** What parameter sets custom values to be treated as False?
- [ ] A) false_vals
- [ ] B) custom_false
- [ ] C) false_values
- [ ] D) set_false

> [!success]- Answer
> **Correct Answer: C) false_values**
> 
> Use `false_values` parameter, which accepts a list of values

**Q28.** When are custom True/False values applied?
- [ ] A) To all columns
- [ ] B) Only to columns set as Boolean
- [ ] C) Only to numeric columns
- [ ] D) To object columns only

> [!success]- Answer
> **Correct Answer: B) Only to columns set as Boolean**
> 
> The chapter emphasizes: "Custom True/False values are only applied to columns set as Boolean"

**Q29.** How are datetime columns loaded by default in pandas?
- [ ] A) As datetime64[ns] type
- [ ] B) As objects (strings)
- [ ] C) As float
- [ ] D) As integer timestamps

> [!success]- Answer
> **Correct Answer: B) As objects (strings)**
> 
> The chapter states: "Datetime columns are loaded as objects (strings) by default"

**Q30.** What parameter is used to parse datetime columns during import?
- [ ] A) dtype
- [ ] B) datetime_cols
- [ ] C) parse_dates
- [ ] D) date_parser

> [!success]- Answer
> **Correct Answer: C) parse_dates**
> 
> The chapter emphasizes: "Specify that columns have datetimes with the parse_dates argument (not dtype!)"

---

## Section B: True/False Questions (15 questions × 1 point = 15 points)

**Q31.** Spreadsheets and flat files are essentially the same thing. **T/F**

> [!success]- Answer
> **False** - Spreadsheets can have formatting and formulas, while flat files cannot. They are stored differently.

**Q32.** The `read_excel()` function is the same as `read_csv()` with different syntax. **T/F**

> [!success]- Answer
> **False** - While they share many parameters, `read_excel()` is specifically for spreadsheets and has unique features like `sheet_name`, `true_values`, and letter-based `usecols`.

**Q33.** By default, `read_excel()` loads all sheets in a workbook. **T/F**

> [!success]- Answer
> **False** - By default, it loads only the first sheet. You must use `sheet_name=None` to load all sheets.

**Q34.** Sheet names in `sheet_name` parameter use zero-indexed position numbers. **T/F**

> [!success]- Answer
> **True** - Position 0 is the first sheet, position 1 is the second sheet, etc.

**Q35.** When `sheet_name=None`, the function returns a DataFrame. **T/F**

> [!success]- Answer
> **False** - It returns an OrderedDict, not a DataFrame. The OrderedDict contains sheet names as keys and DataFrames as values.

**Q36.** Arguments like `nrows` apply to all sheets when loading multiple sheets. **T/F**

> [!success]- Answer
> **True** - The chapter states: "Any arguments passed to read_excel() apply to all sheets read"

**Q37.** pandas automatically loads Boolean columns with the correct bool data type. **T/F**

> [!success]- Answer
> **False** - pandas loads True/False columns as float data by default. You must use `dtype` to specify bool type.

**Q38.** Boolean columns can contain True, False, and NA values. **T/F**

> [!success]- Answer
> **False** - Boolean columns can only have True and False values. NA values are converted to True.

**Q39.** NA values in Boolean columns remain as NA after import. **T/F**

> [!success]- Answer
> **False** - NA/missing values in Boolean columns are changed to True.

**Q40.** The `true_values` parameter accepts a list of values to treat as True. **T/F**

> [!success]- Answer
> **True** - Both `true_values` and `false_values` accept lists.

**Q41.** Custom True/False values apply to all columns in the DataFrame. **T/F**

> [!success]- Answer
> **False** - They only apply to columns that are set as Boolean type using `dtype`.

**Q42.** Use the `dtype` parameter to specify datetime columns. **T/F**

> [!success]- Answer
> **False** - Use `parse_dates` for datetime columns, not `dtype`.

**Q43.** The `parse_dates` parameter can accept a list of column names. **T/F**

> [!success]- Answer
> **True** - It can accept lists of names/numbers, nested lists, or dictionaries.

**Q44.** When combining date columns, pandas automatically creates a new column name by joining the original names with underscores. **T/F**

> [!success]- Answer
> **True** - For example, combining "Part2StartDate" and "Part2StartTime" creates "Part2StartDate_Part2StartTime"

**Q45.** The `parse_dates` parameter works with all datetime formats, including non-standard ones. **T/F**

> [!success]- Answer
> **False** - `parse_dates` doesn't work with non-standard datetime formats. You must use `pd.to_datetime()` after loading for those.

---

## Section C: Short Answer Questions (8 questions × 3 points = 24 points)

**Q46.** List three ways you can specify which sheet(s) to load using the `sheet_name` parameter.

> [!success]- Answer
> The `sheet_name` parameter can accept:
> 1. **By name (string)** - e.g., `sheet_name='2017'` loads the sheet named "2017"
> 2. **By position number (zero-indexed integer)** - e.g., `sheet_name=1` loads the second sheet
> 3. **A list of names/numbers** - e.g., `sheet_name=[0, '2017', 2]` loads multiple sheets
> 4. **`None` to load all sheets** - e.g., `sheet_name=None` loads every sheet in the workbook

**Q47.** Explain what happens when you set `sheet_name=None` and what data type is returned.

> [!success]- Answer
> When you set `sheet_name=None`:
> - **All sheets in the workbook are loaded** at once
> - **Returns an `OrderedDict`**, not a DataFrame
> - The OrderedDict contains:
>   - **Keys**: Sheet names (as strings)
>   - **Values**: DataFrames containing each sheet's data
> - You can iterate through it with `.items()` to access each sheet's name and DataFrame

**Q48.** Why does pandas load Boolean columns as float by default, and what problems does this cause?

> [!success]- Answer
> pandas loads Boolean columns as float by default (not explicitly explained why in the chapter, but it's for technical compatibility reasons).
> 
> **Problems this causes:**
> 1. **NA/missing values are changed to True** when the column is set to Boolean type
> 2. **Unrecognized values are also changed to True** instead of raising errors
> 3. Makes it difficult to distinguish between actual True values and converted NA/unrecognized values
> 4. Can lead to incorrect analysis if not handled properly
> 
> **Solution**: Explicitly set columns to bool type with `dtype` and use `true_values`/`false_values` to control conversion.

**Q49.** What are the three types of input that the `parse_dates` parameter can accept?

> [!success]- Answer
> The `parse_dates` parameter can accept:
> 
> 1. **A list of column names or numbers to parse**
>    - Example: `parse_dates=["Part1StartTime", "Part1EndTime"]`
> 
> 2. **A list containing lists of columns to combine and parse**
>    - Example: `parse_dates=[["Part2StartDate", "Part2StartTime"]]`
>    - Combines multiple columns into one datetime column
> 
> 3. **A dictionary where keys are new column names and values are lists of columns to parse together**
>    - Example: `parse_dates={"Part2Start": ["Part2StartDate", "Part2StartTime"]}`
>    - Allows custom naming of combined datetime columns

**Q50.** List the four Boolean considerations mentioned in the chapter that you should think about before using Boolean data types.

> [!success]- Answer
> The four Boolean considerations are:
> 
> 1. **Are there missing values, or could there be in the future?**
>    - Important because NAs convert to True in Boolean columns
> 
> 2. **How will this column be used in analysis?**
>    - Determines if Boolean is the right choice
> 
> 3. **What would happen if a value were incorrectly coded as True?**
>    - Assess the risk of incorrect conversions
> 
> 4. **Could the data be modeled another way (e.g., as floats or integers)?**
>    - Consider alternatives that might preserve more information

**Q51.** Explain when you should use `pd.to_datetime()` instead of `parse_dates`, and what parameters it uses.

> [!success]- Answer
> **When to use `pd.to_datetime()`:**
> - When `parse_dates` doesn't work with **non-standard datetime formats**
> - Use it **after loading** the data (post-processing)
> 
> **Parameters:**
> 1. **DataFrame and column to convert** - the data to parse
> 2. **`format`** - string representation of the datetime format using codes like `%Y`, `%m`, `%d`, etc.
> 
> **Example:**
> ```python
> format_string = "%m%d%Y %H:%M:%S"
> survey_df["Part2EndTime"] = pd.to_datetime(
>     survey_df["Part2EndTime"], 
>     format=format_string
> )
> ```

**Q52.** Explain the relationship between `true_values`/`false_values` and the `dtype` parameter.

> [!success]- Answer
> **Relationship:**
> - `true_values` and `false_values` parameters **only work on columns that have been set as Boolean** using the `dtype` parameter
> - They do not apply to all columns automatically
> 
> **Usage pattern:**
> ```python
> pd.read_excel("file.xlsx",
>               dtype={"AttendedBootcamp": bool},  # Set column as Boolean FIRST
>               true_values=["Yes"],                # Then these apply to it
>               false_values=["No"])
> ```
> 
> **Why this matters:**
> If you use `true_values=["Yes"]` without setting the column to `bool` in `dtype`, the custom values won't be applied properly.

**Q53.** What is the purpose of the `pd.concat()` function as shown in the chapter, and how is it used?

> [!success]- Answer
> **Purpose:** 
> `pd.concat()` combines multiple DataFrames vertically (stacks them on top of each other), which is useful for combining data from multiple sheets into one DataFrame.
> 
> **Usage in the chapter:**
> ```python
> all_responses = pd.DataFrame()  # Start with empty DataFrame
> 
> for sheet_name, frame in survey_responses.items():
>     frame["Year"] = sheet_name  # Add identifying column
>     all_responses = pd.concat([all_responses, frame])  # Stack them
> ```
> 
> **Result:** Creates a single DataFrame containing all rows from all sheets, with a new "Year" column identifying which sheet each row came from.

---

## Section D: Code Analysis & Scenarios (5 questions × 2 points = 10 points)

**Q54.** What will this code return?
```python
survey_data = pd.read_excel("fcc_survey.xlsx", sheet_name=None)
print(type(survey_data))
```

> [!success]- Answer
> **Output:** `<class 'collections.OrderedDict'>`
> 
> When `sheet_name=None`, `read_excel()` loads all sheets and returns an OrderedDict instead of a DataFrame. The keys are sheet names and the values are DataFrames.

**Q55.** Identify the issue in this code and explain how to fix it:
```python
bool_data = pd.read_excel("survey.xlsx",
                          true_values=["Yes"],
                          false_values=["No"])
# Result: "Yes" and "No" are not converted to True/False
```

> [!success]- Answer
> **Issue:** The columns haven't been set as Boolean type using `dtype`. Custom True/False values only apply to columns set as Boolean.
> 
> **Fix:** Add the `dtype` parameter to specify which columns should be Boolean:
> ```python
> bool_data = pd.read_excel("survey.xlsx",
>                           dtype={"ColumnName": bool},  # Add this!
>                           true_values=["Yes"],
>                           false_values=["No"])
> ```
> 
> Without `dtype`, pandas treats the column as object/string type and doesn't apply the custom True/False values.

**Q56.** What does this code accomplish?
```python
date_cols = {"StartTime": ["StartDate", "StartHour"],
             "EndTime": ["EndDate", "EndHour"]}

df = pd.read_excel("data.xlsx", parse_dates=date_cols)
```

> [!success]- Answer
> This code:
> 1. **Combines multiple columns** into datetime columns
> 2. **Creates custom column names** using the dictionary keys
> 
> **Result:**
> - Combines "StartDate" and "StartHour" columns → creates new column named "StartTime"
> - Combines "EndDate" and "EndHour" columns → creates new column named "EndTime"
> - Both new columns have `datetime64[ns]` data type
> 
> The dictionary format allows you to control the names of the combined datetime columns instead of using the default underscore-joined names.

**Q57.** Complete this code to parse the "SubmissionDate" column which has the format "03292016 21:24:57":
```python
format_string = _______
survey_df["SubmissionDate"] = pd.to_datetime(survey_df["SubmissionDate"],
                                              format=_______)
```

> [!success]- Answer
> ```python
> format_string = "%m%d%Y %H:%M:%S"
> survey_df["SubmissionDate"] = pd.to_datetime(survey_df["SubmissionDate"],
>                                               format=format_string)
> ```
> 
> **Format breakdown:**
> - `%m` = month (03)
> - `%d` = day (29)
> - `%Y` = 4-digit year (2016)
> - ` ` = space
> - `%H` = hour 24-hour clock (21)
> - `%M` = minute (24)
> - `%S` = second (57)

**Q58.** What will happen when this code runs?
```python
survey_responses = pd.read_excel("survey.xlsx", 
                                 sheet_name=None,
                                 nrows=100)

for sheet_name, frame in survey_responses.items():
    print(sheet_name, frame.shape)
```

> [!success]- Answer
> **Result:** Each sheet in the workbook will be loaded with only its first 100 rows.
> 
> **Output example:**
> ```
> 2016 (100, 98)
> 2017 (100, 98)
> ```
> 
> **Explanation:** 
> - `sheet_name=None` loads all sheets
> - `nrows=100` applies to all sheets (as stated in the chapter: "Any arguments passed to read_excel() apply to all sheets read")
> - Each DataFrame in the OrderedDict will have at most 100 rows
> - The code then prints each sheet's name and shape

---

## Answer Key & Scoring

### Section A: Multiple Choice (60 points)
1. B  |  2. B  |  3. B  |  4. C  |  5. C  
2. B  |  7. C  |  8. B  |  9. B  |  10. B  
3. C  |  12. C  |  13. B  |  14. B  |  15. B  
4. C  |  17. C  |  18. B  |  19. C  |  20. B  
5. B  |  22. B  |  23. B  |  24. B  |  25. C  
6. C  |  27. C  |  28. B  |  29. B  |  30. C  

### Section B: True/False (15 points)
31. F  |  32. F  |  33. F  |  34. T  |  35. F  
32. T  |  37. F  |  38. F  |  39. F  |  40. T  
33. F  |  42. F  |  43. T  |  44. T  |  45. F  

### Section C: Short Answer (24 points)
- Q46-Q53: See detailed answers in collapsible sections (3 points each)

### Section D: Code Analysis (10 points)
- Q54-Q58: See detailed answers in collapsible sections (2 points each)

---

## Quick Reference Guide

### Essential read_excel() Patterns
```python
# Basic import
pd.read_excel("file.xlsx")

# Select specific sheet
pd.read_excel("file.xlsx", sheet_name="2017")
pd.read_excel("file.xlsx", sheet_name=1)  # Second sheet (zero-indexed)

# Load all sheets
pd.read_excel("file.xlsx", sheet_name=None)  # Returns OrderedDict

# Limit columns and rows
pd.read_excel("file.xlsx", usecols="A:F, Z", skiprows=2, nrows=1000)

# Boolean columns
pd.read_excel("file.xlsx",
              dtype={"Col1": bool, "Col2": bool},
              true_values=["Yes", "Y"],
              false_values=["No", "N"])

# Parse dates
pd.read_excel("file.xlsx", 
              parse_dates=["DateCol1", "DateCol2"])

# Parse and combine date columns
pd.read_excel("file.xlsx",
              parse_dates={"Start": ["StartDate", "StartTime"]})

# Non-standard dates (use after loading)
df["DateCol"] = pd.to_datetime(df["DateCol"], format="%m%d%Y %H:%M:%S")
```

---

## Key Concepts Summary

| Concept | Description | Key Point |
|---------|-------------|-----------|
| **Spreadsheets** | Excel files with formatting/formulas | Different from flat files |
| **read_excel()** | Function to load Excel files | Loads first sheet by default |
| **sheet_name** | Parameter to select sheets | Can use name, number, list, or None |
| **OrderedDict** | Return type when sheet_name=None | Keys=sheet names, Values=DataFrames |
| **Boolean Default** | Loaded as float by default | Must use dtype to set as bool |
| **NA in Boolean** | Converted to True | Major pitfall to watch for |
| **true_values/false_values** | Set custom Boolean values | Only work with dtype=bool |
| **parse_dates** | Parse datetime columns | Use this, not dtype! |
| **pd.to_datetime()** | For non-standard dates | Use after loading data |
| **Datetime Codes** | %Y, %m, %d, %H, %M, %S | Describe format patterns |

### Datetime Format Codes Reference

| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | Year (4-digit) | 1999, 2016 |
| `%m` | Month (zero-padded) | 01, 03, 12 |
| `%d` | Day (zero-padded) | 01, 15, 31 |
| `%H` | Hour (24-hour) | 00, 13, 21 |
| `%M` | Minute (zero-padded) | 00, 09, 45 |
| `%S` | Second (zero-padded) | 00, 05, 59 |

**Reference:** strftime.org for complete list

---

## Common Mistakes to Avoid

❌ Assuming `read_excel()` loads all sheets by default (it only loads the first)  
❌ Forgetting that sheet positions are zero-indexed (0 = first sheet)  
❌ Expecting a DataFrame when using `sheet_name=None` (returns OrderedDict)  
❌ Not setting columns to `dtype=bool` before using `true_values`/`false_values`  
❌ Thinking NA values stay as NA in Boolean columns (they become True!)  
❌ Using `dtype` for datetime columns instead of `parse_dates`  
❌ Forgetting that `parse_dates` doesn't work for non-standard formats  
❌ Not using `format` parameter with `pd.to_datetime()` for custom date formats  
❌ Assuming arguments only apply to one sheet when loading multiple sheets  
❌ Confusing spreadsheet column letters (Excel-specific) with position numbers  

---

## Study Strategy (3-Day Plan)

### Day 1: Basic Excel Loading
- Review differences between spreadsheets and flat files
- Practice basic Excel import with `read_excel()`
- Work with `usecols` using Excel letter notation ("A:F")
- Load specific sheets by name and position
- Practice with `nrows` and `skiprows`

### Day 2: Multiple Sheets
- Practice loading sheets with `sheet_name` parameter
- Load all sheets with `sheet_name=None`
- Work with OrderedDict structure
- Combine multiple sheets using `pd.concat()`
- Understand how arguments apply to all sheets

### Day 3: Boolean and Datetime Data
- Review Boolean data default behavior (float)
- Practice setting columns to bool with `dtype`
- Work with `true_values` and `false_values`
- Parse datetime columns with `parse_dates`
- Use `pd.to_datetime()` for non-standard formats
- Learn datetime format codes (%Y, %m, %d, etc.)
- Review Boolean considerations
- Complete practice quiz

---

## Pre-Exam Checklist

✅ Know `read_excel()` loads first sheet by default  
✅ Understand `sheet_name` accepts name, number, list, or None  
✅ Remember `sheet_name=None` returns OrderedDict, not DataFrame  
✅ Know that sheet positions are zero-indexed  
✅ Understand Boolean columns load as float by default  
✅ Remember NA values become True in Boolean columns  
✅ Know `true_values`/`false_values` only work with `dtype=bool`  
✅ Use `parse_dates` (not `dtype`) for datetime columns  
✅ Know three input types for `parse_dates` (list, nested list, dict)  
✅ Remember `pd.to_datetime()` for non-standard date formats  
✅ Know key datetime codes: %Y, %m, %d, %H, %M, %S  
✅ Understand arguments apply to ALL sheets when loading multiple  
✅ Can use Excel column letters in `usecols` (e.g., "A:P")  

---

## During the Exam

💡 **Sheet loading** - Remember default is first sheet only, not all sheets  
💡 **Data types** - Check return type (DataFrame vs OrderedDict)  
💡 **Boolean pitfalls** - Watch for NA → True conversions  
💡 **Parameter pairing** - `true_values` needs `dtype=bool` to work  
💡 **Date parsing** - Use `parse_dates`, not `dtype`!  
💡 **Format codes** - Know common ones: %Y, %m, %d, %H, %M, %S  
💡 **Zero-indexing** - Position 0 = first sheet, 1 = second sheet  
💡 **OrderedDict** - Remember it has `.items()` method for iteration  

---

#datacamp #pandas #data-ingestion #excel #spreadsheets #boolean #datetime #certification #exam-prep