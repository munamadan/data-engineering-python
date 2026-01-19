
### The Problem

Data science projects fail when working with **dirty data**:

- Inaccurate analysis
- Wrong conclusions
- Poor model performance

### The Principle: "Garbage In, Garbage Out"

> [!warning] Critical Concept If you input dirty data into your analysis, you'll get garbage results out. Data cleaning is essential for reliable insights.

---

## Data Type Constraints

### Common Data Types and Their Python Equivalents

|Data Type|Example|Python Type|
|---|---|---|
|**Text data**|First name, last name, address|`str`|
|**Integers**|# Subscribers, # products sold|`int`|
|**Decimals**|Temperature, $ exchange rates|`float`|
|**Binary**|Is married, new customer, yes/no|`bool`|
|**Dates**|Order dates, ship dates|`datetime`|
|**Categories**|Marriage status, gender|`category`|

---

## Converting Strings to Integers

### The Problem: Numbers Stored as Strings

```python
# Import CSV file and output header
sales = pd.read_csv('sales.csv')
sales.head(2)
```

|SalesOrderID|Revenue|Quantity|
|---|---|---|
|43659|23153$|12|
|43660|1457$|2|

### Checking Data Types

**Method 1: Using `.dtypes`**

```python
# Get data types of columns
sales.dtypes
```

Output:

```
SalesOrderID    int64
Revenue         object
Quantity        int64
dtype: object
```

**Method 2: Using `.info()`**

```python
# Get DataFrame information
sales.info()
```

Output:

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 31465 entries, 0 to 31464
Data columns (total 3 columns):
SalesOrderID    31465 non-null int64
Revenue         31465 non-null object
Quantity        31465 non-null int64
dtypes: int64(2), object(1)
memory usage: 737.5+ KB
```

> [!info] Note `object` data type typically means string data in pandas.

### The Issue with String Numbers

```python
# Try to sum Revenue column (stored as strings)
sales['Revenue'].sum()
```

Output (concatenated strings!):

```
'23153$1457$36865$32474$472$27510$16158$5694$6876$40487$...'
```

### Solution: Convert to Integer

```python
# Remove $ from Revenue column
sales['Revenue'] = sales['Revenue'].str.strip('$')

# Convert to integer
sales['Revenue'] = sales['Revenue'].astype('int')

# Verify conversion worked
assert sales['Revenue'].dtype == 'int'
```

---

## The assert Statement

### Purpose

The `assert` statement tests if a condition is true. If false, it raises an `AssertionError`.

### Syntax

```python
# This will pass (no output)
assert 1 + 1 == 2

# This will fail
assert 1 + 1 == 3
```

Output:

```
AssertionError: 
```

### Usage in Data Cleaning

```python
# Verify data type conversion
assert sales['Revenue'].dtype == 'int'

# Verify data range
assert movies['avg_rating'].max() <= 5

# Verify no future dates
assert user_signups['subscription_date'].max() <= dt.date.today()
```

> [!tip] Best Practice Use `assert` statements after data cleaning operations to verify your changes worked correctly. No output means the assertion passed!

---

## Numeric vs Categorical Data

### The Problem: Numbers Representing Categories

Consider a marriage status column stored as integers:

|marriage_status|
|---|
|3|
|1|
|2|

**Mapping:** 0 = Never married, 1 = Married, 2 = Separated, 3 = Divorced

### Why This is Wrong

```python
df['marriage_status'].describe()
```

Output:

```
marriage_status
count    241.0
mean       1.4
std        0.20
min        0.00
50%        1.8
```

> [!warning] Problem Statistical summaries like mean and standard deviation don't make sense for categorical data!

### Solution: Convert to Category Type

```python
# Convert to categorical
df["marriage_status"] = df["marriage_status"].astype('category')

# Now describe() gives appropriate statistics
df.describe()
```

Output:

```
marriage_status
count       241
unique        4
top           1
freq        120
```

> [!success] Result Now we get appropriate statistics: count, unique values, most common value (top), and frequency.

---

## Data Range Constraints

### Types of Range Issues

1. **Out of range values** (e.g., ratings > 5 on a 1-5 scale)
2. **Impossible dates** (e.g., future subscription dates)
3. **Negative values where they shouldn't exist**

### Example: Movie Ratings

```python
movies.head()
```

|movie_name|avg_rating|
|---|---|
|The Godfather|5|
|Frozen 2|3|
|Shrek|4|

Problem: Some ratings are > 5 on a 1-5 scale!

```python
# Find out of range values
movies[movies['avg_rating'] > 5]
```

|movie_name|avg_rating|
|---|---|
|A Beautiful Mind|6|
|La Vita e Bella|6|
|Amelie|6|

---

## Dealing with Out of Range Data

### Option 1: Drop the Data

**Using Filtering:**

```python
# Keep only valid ratings
movies = movies[movies['avg_rating'] <= 5]
```

**Using `.drop()`:**

```python
# Drop rows with invalid ratings
movies.drop(movies[movies['avg_rating'] > 5].index, inplace=True)
```

**Verify:**

```python
# Assert no ratings exceed 5
assert movies['avg_rating'].max() <= 5
```

### Option 2: Set Custom Minimum/Maximum

```python
# Convert all ratings > 5 to exactly 5
movies.loc[movies['avg_rating'] > 5, 'avg_rating'] = 5

# Assert statement
assert movies['avg_rating'].max() <= 5
```

### Option 3: Treat as Missing and Impute

```python
# Set out of range values to NaN
movies.loc[movies['avg_rating'] > 5, 'avg_rating'] = np.nan

# Impute with mean, median, or other method
movies['avg_rating'].fillna(movies['avg_rating'].mean(), inplace=True)
```

### Option 4: Set Custom Value Based on Business Logic

Depends on domain knowledge and business requirements.

---

## Date Range Constraints

### The Problem: Future Dates

```python
import datetime as dt

# Get today's date
today_date = dt.date.today()

# Find future dates
user_signups[user_signups['subscription_date'] > today_date]
```

|subscription_date|user_name|Country|
|---|---|---|
|01/05/2021|Marah|Nauru|
|09/08/2020|Joshua|Austria|
|04/01/2020|Heidi|Guinea|

> [!warning] Problem Can't have future subscription dates!

### Solution: Convert and Clean Dates

**Step 1: Check Data Type**

```python
# Output data types
user_signups.dtypes
```

Output:

```
subscription_date    object
user_name           object
Country             object
dtype: object
```

**Step 2: Convert to Datetime**

```python
# Convert to date type
user_signups['subscription_date'] = pd.to_datetime(
    user_signups['subscription_date']
).dt.date
```

**Step 3: Clean Invalid Dates**

**Option A: Drop Future Dates (Filtering)**

```python
today_date = dt.date.today()

# Keep only past dates
user_signups = user_signups[user_signups['subscription_date'] < today_date]
```

**Option B: Drop Future Dates (Using .drop())**

```python
# Drop future dates
user_signups.drop(
    user_signups[user_signups['subscription_date'] > today_date].index,
    inplace=True
)
```

**Option C: Hardcode with Upper Limit**

```python
# Replace future dates with today
user_signups.loc[
    user_signups['subscription_date'] > today_date,
    'subscription_date'
] = today_date

# Assert is true
assert user_signups['subscription_date'].max() <= today_date
```

---

## Uniqueness Constraints: Duplicate Values

### What Are Duplicates?

**Complete Duplicates:** All columns have the same values

|first_name|last_name|address|height|weight|
|---|---|---|---|---|
|Justin|Saddlemyer|Boulevard du Jardin Botanique 3|193 cm|87 kg|
|Justin|Saddlemyer|Boulevard du Jardin Botanique 3|193 cm|87 kg|

**Partial Duplicates:** Most columns have the same values

|first_name|last_name|address|height|weight|
|---|---|---|---|---|
|Justin|Saddlemyer|Boulevard du Jardin Botanique 3|193 cm|87 kg|
|Justin|Saddlemyer|Boulevard du Jardin Botanique 3|194 cm|87 kg|

### Why Do Duplicates Happen?

1. **Data entry errors** - Same data entered multiple times
2. **System errors** - Multiple form submissions
3. **Data merging issues** - Joining datasets incorrectly

---

## Finding Duplicate Values

### Using `.duplicated()`

```python
# Print the header
height_weight.head()
```

|first_name|last_name|address|height|weight|
|---|---|---|---|---|
|Lane|Reese|534-1559 Nam St.|181|64|
|Ivor|Pierce|102-3364 Non Road|168|66|
|Roary|Gibson|P.O. Box 344, 7785 Nisi Ave|191|99|

**Step 1: Find Duplicates**

```python
# Get duplicates across all columns
duplicates = height_weight.duplicated()
print(duplicates)
```

Output:

```
1     False
...   ...
22    True
23    False
...   ...
```

**Step 2: View Duplicate Rows**

```python
# Get duplicate rows
height_weight[duplicates]
```

### `.duplicated()` Parameters

```python
duplicates = height_weight.duplicated(
    subset=['first_name', 'last_name', 'address'],  # Columns to check
    keep='first'  # 'first', 'last', or False (mark all)
)
```

**Parameters:**

- **`subset`**: List of column names to check for duplication
- **`keep`**:
    - `'first'` - Mark duplicates as True except for first occurrence
    - `'last'` - Mark duplicates as True except for last occurrence
    - `False` - Mark all duplicates as True

### Finding Partial Duplicates

```python
# Check specific columns only
column_names = ['first_name', 'last_name', 'address']
duplicates = height_weight.duplicated(subset=column_names, keep=False)

# View duplicates sorted by name
height_weight[duplicates].sort_values(by='first_name')
```

Output:

|first_name|last_name|address|height|weight|
|---|---|---|---|---|
|Cole|Palmer|8366 At, Street|178|91|
|Cole|Palmer|8366 At, Street|178|91|
|Desirae|Shannon|P.O. Box 643, 5251...|195|83|
|Desirae|Shannon|P.O. Box 643, 5251...|196|83|

---

## Treating Duplicate Values

### Method 1: Drop Complete Duplicates

```python
# Drop duplicates
height_weight.drop_duplicates(inplace=True)
```

### `.drop_duplicates()` Parameters

```python
height_weight.drop_duplicates(
    subset=['first_name', 'last_name', 'address'],  # Columns to check
    keep='first',     # Which to keep: 'first', 'last', or False
    inplace=True      # Modify DataFrame directly
)
```

### Method 2: Aggregate Duplicates

When you want to keep information from all duplicates:

```python
# Group by identifying columns and aggregate
column_names = ['first_name', 'last_name', 'address']
summaries = {'height': 'max', 'weight': 'mean'}

height_weight = height_weight.groupby(by=column_names).agg(summaries).reset_index()
```

**What this does:**

- Groups rows with same name and address
- Takes maximum height
- Takes mean weight
- Results in one row per unique person

**Verify no duplicates remain:**

```python
duplicates = height_weight.duplicated(subset=column_names, keep=False)
height_weight[duplicates].sort_values(by='first_name')
```

Output: Empty DataFrame (no duplicates!)

---

## Summary of Methods

### Data Type Conversion

```python
# Check type
df.dtypes
df.info()

# Convert types
df['column'] = df['column'].astype('int')
df['column'] = df['column'].astype('category')
df['column'] = pd.to_datetime(df['column'])
```

### Range Validation

```python
# Drop out of range
df = df[df['column'] <= max_value]
df.drop(df[df['column'] > max_value].index, inplace=True)

# Cap at max value
df.loc[df['column'] > max_value, 'column'] = max_value
```

### Duplicate Handling

```python
# Find duplicates
duplicates = df.duplicated(subset=columns, keep=False)

# Drop duplicates
df.drop_duplicates(subset=columns, keep='first', inplace=True)

# Aggregate duplicates
df.groupby(by=columns).agg(summaries).reset_index()
```

### Assertions

```python
# Verify data type
assert df['column'].dtype == 'int'

# Verify range
assert df['column'].max() <= 5

# Verify no future dates
assert df['date_column'].max() <= dt.date.today()
```

---

## Key Takeaways

✅ **Data type matters** - Ensure columns have correct types (int, float, category, datetime)  
✅ **Validate ranges** - Check for impossible values (ratings > 5, future dates)  
✅ **Handle out of range** - Drop, cap, or impute based on business logic  
✅ **Find duplicates** - Use `.duplicated()` with appropriate parameters  
✅ **Remove duplicates** - Drop or aggregate based on requirements  
✅ **Use assertions** - Verify your cleaning worked correctly

---

#datacamp #data-cleaning #python #pandas #data-quality #certification

## Section A: Multiple Choice Questions (60 points)

_2 points each | 30 questions total_

### Data Type Constraints (Questions 1-10)

**Q1.** What principle describes the problem of using dirty data?

- [ ] A) Clean in, Clean out
- [ ] B) Garbage in, Garbage out
- [ ] C) Data in, Results out
- [ ] D) Quality in, Quality out

> [!success]- Answer **Correct Answer: B) Garbage in, Garbage out**
> 
> If you input dirty data into your analysis, you'll get garbage results. Data cleaning is essential for reliable insights.

**Q2.** What Python data type corresponds to text data like names and addresses?

- [ ] A) int
- [ ] B) float
- [ ] C) str
- [ ] D) bool

> [!success]- Answer **Correct Answer: C) str**
> 
> Text data (first name, last name, address) is stored as string (`str`) type in Python.

**Q3.** What Python data type should be used for the number of subscribers or products sold?

- [ ] A) str
- [ ] B) int
- [ ] C) float
- [ ] D) category

> [!success]- Answer **Correct Answer: B) int**
> 
> Integers (`int`) are used for whole numbers like # of subscribers or # of products sold.

**Q4.** What pandas data type is marriage status or gender best stored as?

- [ ] A) int
- [ ] B) str
- [ ] C) float
- [ ] D) category

> [!success]- Answer **Correct Answer: D) category**
> 
> Categorical data like marriage status or gender should be stored as `category` type for appropriate statistical summaries.

**Q5.** What pandas method shows data types of all columns?

- [ ] A) `.types()`
- [ ] B) `.dtypes`
- [ ] C) `.info()`
- [ ] D) Both B and C

> [!success]- Answer **Correct Answer: D) Both B and C**
> 
> Both `.dtypes` and `.info()` show data types of columns. `.dtypes` shows only types, while `.info()` shows types plus additional information.

**Q6.** In pandas, what does the `object` data type typically indicate?

- [ ] A) Integer data
- [ ] B) String data
- [ ] C) Boolean data
- [ ] D) Float data

> [!success]- Answer **Correct Answer: B) String data**
> 
> The `object` data type in pandas typically means the column contains string data.

**Q7.** What method converts a column to integer type?

- [ ] A) `.convert('int')`
- [ ] B) `.to_int()`
- [ ] C) `.astype('int')`
- [ ] D) `.change_type('int')`

> [!success]- Answer **Correct Answer: C) `.astype('int')`**
> 
> Use `.astype('int')` to convert a column to integer type: `df['column'] = df['column'].astype('int')`

**Q8.** What happens if you sum a column stored as strings (e.g., "100$", "200$")?

- [ ] A) Returns mathematical sum
- [ ] B) Returns error
- [ ] C) Concatenates strings
- [ ] D) Returns NaN

> [!success]- Answer **Correct Answer: C) Concatenates strings**
> 
> Summing strings concatenates them: "100$" + "200$" + "300$" becomes "100$200$300$" instead of 600.

**Q9.** What string method removes specified characters from the beginning and end of strings?

- [ ] A) `.remove()`
- [ ] B) `.strip()`
- [ ] C) `.delete()`
- [ ] D) `.clean()`

> [!success]- Answer **Correct Answer: B) `.strip()`**
> 
> `.str.strip('$')` removes the dollar sign: `df['Revenue'].str.strip('$')` converts "100$" to "100".

**Q10.** Why is it wrong to calculate the mean of a categorical variable stored as numbers (0, 1, 2, 3)?

- [ ] A) It causes an error
- [ ] B) The mean doesn't have meaningful interpretation for categories
- [ ] C) Categories can't be numbers
- [ ] D) It's too slow

> [!success]- Answer **Correct Answer: B) The mean doesn't have meaningful interpretation for categories**
> 
> For categorical data like marriage status (0=Never married, 1=Married, 2=Separated, 3=Divorced), calculating a mean of 1.4 is meaningless. Use `category` type to get appropriate statistics.

### Data Range Constraints (Questions 11-20)

**Q11.** What is an example of an out of range value?

- [ ] A) A rating of 6 on a 1-5 scale
- [ ] B) A future subscription date
- [ ] C) A negative age
- [ ] D) All of the above

> [!success]- Answer **Correct Answer: D) All of the above**
> 
> All represent impossible or invalid values: ratings can't exceed 5, subscription dates can't be in the future, and ages can't be negative.

**Q12.** Which is NOT a valid way to handle out of range data?

- [ ] A) Dropping the data
- [ ] B) Setting custom minimums and maximums
- [ ] C) Ignoring it
- [ ] D) Treating as missing and imputing

> [!success]- Answer **Correct Answer: C) Ignoring it**
> 
> Out of range data should be handled, not ignored. Valid approaches: drop, cap at min/max, treat as missing, or apply business logic.

**Q13.** What does this code do: `movies = movies[movies['avg_rating'] <= 5]`?

- [ ] A) Selects movies with rating > 5
- [ ] B) Drops movies with rating > 5
- [ ] C) Changes ratings > 5 to 5
- [ ] D) Counts movies with rating <= 5

> [!success]- Answer **Correct Answer: B) Drops movies with rating > 5**
> 
> This filtering keeps only rows where `avg_rating <= 5`, effectively dropping any movies with ratings greater than 5.

**Q14.** What does this code do: `movies.loc[movies['avg_rating'] > 5, 'avg_rating'] = 5`?

- [ ] A) Drops movies with rating > 5
- [ ] B) Converts all ratings > 5 to exactly 5
- [ ] C) Finds movies with rating > 5
- [ ] D) Deletes the avg_rating column

> [!success]- Answer **Correct Answer: B) Converts all ratings > 5 to exactly 5**
> 
> `.loc[]` with a condition in the first position and column name in the second position allows you to set values. This caps all ratings at 5.

**Q15.** What function converts a column to datetime type?

- [ ] A) `.astype('datetime')`
- [ ] B) `pd.to_datetime()`
- [ ] C) `.convert_datetime()`
- [ ] D) `.datetime()`

> [!success]- Answer **Correct Answer: B) `pd.to_datetime()`**
> 
> Use `pd.to_datetime(df['date_column'])` to convert strings to datetime objects.

**Q16.** What does `.dt.date` do after converting to datetime?

- [ ] A) Converts to string
- [ ] B) Extracts just the date part (no time)
- [ ] C) Extracts the time part
- [ ] D) Formats the date

> [!success]- Answer **Correct Answer: B) Extracts just the date part (no time)**
> 
> `.dt.date` extracts only the date from a datetime object: `pd.to_datetime(df['column']).dt.date`

**Q17.** How do you get today's date in Python?

- [ ] A) `datetime.now()`
- [ ] B) `dt.date.today()`
- [ ] C) `pd.today()`
- [ ] D) `datetime.current_date()`

> [!success]- Answer **Correct Answer: B) `dt.date.today()`**
> 
> After importing `import datetime as dt`, use `dt.date.today()` to get today's date.

**Q18.** What parameter in `.drop()` removes rows directly in the DataFrame without creating a new object?

- [ ] A) `direct=True`
- [ ] B) `inplace=True`
- [ ] C) `modify=True`
- [ ] D) `permanent=True`

> [!success]- Answer **Correct Answer: B) `inplace=True`**
> 
> `inplace=True` modifies the DataFrame directly: `df.drop(index, inplace=True)`

**Q19.** What does this code do: `df.drop(df[df['rating'] > 5].index, inplace=True)`?

- [ ] A) Finds ratings > 5
- [ ] B) Drops rows where rating > 5
- [ ] C) Changes ratings > 5 to 5
- [ ] D) Counts ratings > 5

> [!success]- Answer **Correct Answer: B) Drops rows where rating > 5**
> 
> `df[df['rating'] > 5].index` gets the index of rows to drop, then `.drop()` removes those rows.

**Q20.** When should you drop out of range data vs. capping it at a maximum?

- [ ] A) Always drop
- [ ] B) Always cap
- [ ] C) Depends on business requirements and domain knowledge
- [ ] D) It doesn't matter

> [!success]- Answer **Correct Answer: C) Depends on business requirements and domain knowledge**
> 
> The decision to drop vs. cap depends on your specific situation, data quality issues, and business needs.

### Uniqueness Constraints (Questions 21-30)

**Q21.** What are complete duplicates?

- [ ] A) Rows where some columns match
- [ ] B) Rows where all columns have the same values
- [ ] C) Rows with similar names
- [ ] D) Rows in the same category

> [!success]- Answer **Correct Answer: B) Rows where all columns have the same values**
> 
> Complete duplicates have identical values in every column. Partial duplicates have matching values in most (but not all) columns.

**Q22.** What method identifies duplicate rows?

- [ ] A) `.find_duplicates()`
- [ ] B) `.duplicated()`
- [ ] C) `.is_duplicate()`
- [ ] D) `.check_duplicates()`

> [!success]- Answer **Correct Answer: B) `.duplicated()`**
> 
> Use `.duplicated()` to get a boolean Series indicating which rows are duplicates.

**Q23.** What does `.duplicated()` return?

- [ ] A) The duplicate rows themselves
- [ ] B) Count of duplicates
- [ ] C) Boolean Series (True/False for each row)
- [ ] D) Index of duplicates

> [!success]- Answer **Correct Answer: C) Boolean Series (True/False for each row)**
> 
> `.duplicated()` returns a Series of True/False values where True indicates a duplicate row.

**Q24.** What parameter in `.duplicated()` specifies which columns to check?

- [ ] A) `columns`
- [ ] B) `subset`
- [ ] C) `check`
- [ ] D) `on`

> [!success]- Answer **Correct Answer: B) `subset`**
> 
> Use `subset` parameter: `.duplicated(subset=['col1', 'col2'])` to check specific columns only.

**Q25.** What does `keep='first'` do in `.duplicated()`?

- [ ] A) Marks all duplicates as True
- [ ] B) Marks first occurrence as False, rest as True
- [ ] C) Keeps only first occurrence
- [ ] D) Deletes first occurrence

> [!success]- Answer **Correct Answer: B) Marks first occurrence as False, rest as True**
> 
> `keep='first'` marks the first occurrence as False (not a duplicate) and subsequent occurrences as True (duplicates).

**Q26.** What does `keep=False` do in `.duplicated()`?

- [ ] A) Marks all duplicates as True (including first occurrence)
- [ ] B) Marks no duplicates
- [ ] C) Keeps no duplicates
- [ ] D) Deletes all duplicates

> [!success]- Answer **Correct Answer: A) Marks all duplicates as True (including first occurrence)**
> 
> `keep=False` marks ALL occurrences of duplicated rows as True, including the first one.

**Q27.** What method removes duplicate rows?

- [ ] A) `.remove_duplicates()`
- [ ] B) `.drop_duplicates()`
- [ ] C) `.delete_duplicates()`
- [ ] D) `.unique()`

> [!success]- Answer **Correct Answer: B) `.drop_duplicates()`**
> 
> Use `.drop_duplicates()` to remove duplicate rows: `df.drop_duplicates(inplace=True)`

**Q28.** What does `.groupby()` combined with `.agg()` do for duplicate handling?

- [ ] A) Deletes duplicates
- [ ] B) Finds duplicates
- [ ] C) Aggregates duplicate rows into one row with summary statistics
- [ ] D) Counts duplicates

> [!success]- Answer **Correct Answer: C) Aggregates duplicate rows into one row with summary statistics**
> 
> `.groupby().agg()` groups duplicate rows and applies aggregation functions (max, mean, sum, etc.) to combine them.

**Q29.** In this code: `summaries = {'height': 'max', 'weight': 'mean'}`, what aggregation is applied to weight?

- [ ] A) Maximum
- [ ] B) Mean (average)
- [ ] C) Minimum
- [ ] D) Sum

> [!success]- Answer **Correct Answer: B) Mean (average)**
> 
> The dictionary specifies `'weight': 'mean'`, so the mean (average) of weight values is calculated for each group.

**Q30.** Why might you use `.groupby().agg()` instead of `.drop_duplicates()`?

- [ ] A) It's faster
- [ ] B) To keep information from all duplicate rows
- [ ] C) It's required
- [ ] D) To delete duplicates

> [!success]- Answer **Correct Answer: B) To keep information from all duplicate rows**
> 
> `.drop_duplicates()` removes rows, losing information. `.groupby().agg()` combines duplicates using aggregation functions (max, mean), preserving information from all rows.

---

## Section B: True/False Questions (15 points)

_1 point each | 15 questions total_

**Q31.** "Garbage in, Garbage out" means dirty data leads to unreliable analysis results. **T/F**

> [!success]- Answer **True** - If you input dirty data into your analysis, you'll get garbage results. Data cleaning is essential.

**Q32.** The `object` data type in pandas always means string data. **T/F**

> [!success]- Answer **True** - In pandas, `object` data type typically indicates string data.

**Q33.** Summing a column stored as strings will perform mathematical addition. **T/F**

> [!success]- Answer **False** - Summing strings concatenates them: "100$" + "200$" becomes "100$200$" instead of calculating 300.

**Q34.** The `.astype()` method converts column data types. **T/F**

> [!success]- Answer **True** - Use `.astype('int')`, `.astype('float')`, `.astype('category')`, etc. to convert data types.

**Q35.** Calculating the mean of categorical variables stored as numbers (0, 1, 2, 3) provides meaningful statistics. **T/F**

> [!success]- Answer **False** - The mean of categorical codes (like marriage status: 0=Never married, 1=Married, etc.) has no meaningful interpretation. Convert to `category` type instead.

**Q36.** The assert statement raises an error if the condition is True. **T/F**

> [!success]- Answer **False** - The assert statement raises an `AssertionError` if the condition is **False**. If True, it passes silently with no output.

**Q37.** You can drop out of range data using either filtering or the `.drop()` method. **T/F**

> [!success]- Answer **True** - Both methods work: `df = df[df['column'] <= max]` (filtering) or `df.drop(df[df['column'] > max].index, inplace=True)` (.drop method).

**Q38.** `pd.to_datetime()` converts strings to datetime objects. **T/F**

> [!success]- Answer **True** - Use `pd.to_datetime(df['date_column'])` to convert string dates to datetime objects.

**Q39.** The `.dt.date` accessor extracts the time portion from a datetime object. **T/F**

> [!success]- Answer **False** - `.dt.date` extracts the **date** portion (not time) from a datetime object.

**Q40.** `inplace=True` modifies the DataFrame directly without creating a new object. **T/F**

> [!success]- Answer **True** - `inplace=True` makes changes directly to the DataFrame, avoiding the need to reassign the result.

**Q41.** Complete duplicates have identical values in all columns. **T/F**

> [!success]- Answer **True** - Complete duplicates have the same values in every single column.

**Q42.** `.duplicated()` returns the actual duplicate rows. **T/F**

> [!success]- Answer **False** - `.duplicated()` returns a boolean Series (True/False). To get actual rows, use: `df[df.duplicated()]`

**Q43.** The `subset` parameter in `.duplicated()` specifies which columns to check for duplication. **T/F**

> [!success]- Answer **True** - Use `subset=['col1', 'col2']` to check only specific columns for duplication.

**Q44.** `keep='first'` in `.duplicated()` marks the first occurrence as a duplicate. **T/F**

> [!success]- Answer **False** - `keep='first'` marks the first occurrence as **False** (not a duplicate) and marks subsequent occurrences as True.

**Q45.** `.groupby().agg()` can be used to aggregate duplicate rows instead of deleting them. **T/F**

> [!success]- Answer **True** - `.groupby().agg()` groups duplicate rows and applies aggregation functions (max, mean, sum) to combine them while preserving information.

---

## Section C: Short Answer Questions (30 points)

_3 points each | 10 questions total_

**Q46.** Explain the "Garbage in, Garbage out" principle and why data cleaning is important.

> [!success]- Answer **"Garbage in, Garbage out" principle:** If you input dirty, incorrect, or poor-quality data into your analysis, you will get unreliable, inaccurate, or meaningless results out.
> 
> **Why data cleaning is important:**
> 
> 1. **Accurate analysis** - Clean data leads to correct insights
> 2. **Better decisions** - Reliable data supports sound business decisions
> 3. **Model performance** - Machine learning models require clean data
> 4. **Trust** - Stakeholders can trust analysis results
> 5. **Efficiency** - Prevents time wasted on incorrect conclusions
> 
> Data cleaning is the foundation of any successful data science project.

**Q47.** List the six common data types with their Python equivalents and provide an example for each.

> [!success]- Answer
> 
> |Data Type|Python Type|Example|
> |---|---|---|
> |**Text data**|`str`|First name, last name, address|
> |**Integers**|`int`|# of subscribers, # of products sold|
> |**Decimals**|`float`|Temperature, exchange rates|
> |**Binary**|`bool`|Is married (True/False), new customer|
> |**Dates**|`datetime`|Order dates, ship dates|
> |**Categories**|`category`|Marriage status, gender|

**Q48.** Describe the steps to convert a column with values like "100$", "200$" to integers.

> [!success]- Answer **Step-by-step process:**
> 
> 1. **Check current data type:**
>     
>     ```python
>     sales.dtypes  # Shows Revenue is 'object'
>     ```
>     
> 2. **Remove the dollar sign:**
>     
>     ```python
>     sales['Revenue'] = sales['Revenue'].str.strip('$')
>     ```
>     
> 3. **Convert to integer:**
>     
>     ```python
>     sales['Revenue'] = sales['Revenue'].astype('int')
>     ```
>     
> 4. **Verify conversion:**
>     
>     ```python
>     assert sales['Revenue'].dtype == 'int'
>     ```
>     
> 
> The `.str.strip('$')` method removes the dollar sign, then `.astype('int')` converts the string numbers to integers.

**Q49.** Explain why storing marriage status as numbers (0, 1, 2, 3) and treating it as numeric data is problematic. What's the solution?

> [!success]- Answer **The problem:** When marriage status is stored as numbers (0=Never married, 1=Married, 2=Separated, 3=Divorced) and treated as numeric:
> 
> ```python
> df['marriage_status'].describe()
> # Output: mean=1.4, std=0.20
> ```
> 
> **Issues:**
> 
> - Mean of 1.4 is meaningless for categories
> - Standard deviation doesn't make sense
> - Statistical operations assume numeric relationships that don't exist
> - Can't tell ordering has no meaning
> 
> **Solution:**
> 
> ```python
> df['marriage_status'] = df['marriage_status'].astype('category')
> df.describe()
> # Output: count=241, unique=4, top=1, freq=120
> ```
> 
> Now you get appropriate statistics: count of values, number of unique categories, most common value, and its frequency.

**Q50.** List and describe four strategies for dealing with out of range data.

> [!success]- Answer **Four strategies for handling out of range data:**
> 
> 1. **Dropping the data**
>     
>     - Remove rows with invalid values
>     - Best when: Few outliers, data quality is poor
>     
>     ```python
>     df = df[df['rating'] <= 5]
>     ```
>     
> 2. **Setting custom minimums/maximums (capping)**
>     
>     - Replace out of range values with the boundary value
>     - Best when: Want to preserve row count
>     
>     ```python
>     df.loc[df['rating'] > 5, 'rating'] = 5
>     ```
>     
> 3. **Treat as missing and impute**
>     
>     - Set to NaN and fill with mean/median/mode
>     - Best when: Have imputation strategy
>     
>     ```python
>     df.loc[df['rating'] > 5, 'rating'] = np.nan
>     df['rating'].fillna(df['rating'].mean(), inplace=True)
>     ```
>     
> 4. **Set custom value based on business assumptions**
>     
>     - Apply domain knowledge/business rules
>     - Best when: Have specific business logic
> 
> Choice depends on business requirements and data quality.

**Q51.** Explain the difference between `.duplicated()` and `.drop_duplicates()`. Include parameters for each.

> [!success]- Answer **`.duplicated()` - Identifies duplicates:**
> 
> - **Returns:** Boolean Series (True/False for each row)
> - **Purpose:** Find which rows are duplicates
> - **Parameters:**
>     - `subset`: List of columns to check
>     - `keep`: 'first', 'last', or False
> 
> ```python
> duplicates = df.duplicated(subset=['name', 'address'], keep='first')
> df[duplicates]  # View duplicate rows
> ```
> 
> **`.drop_duplicates()` - Removes duplicates:**
> 
> - **Returns:** DataFrame with duplicates removed
> - **Purpose:** Actually delete duplicate rows
> - **Parameters:**
>     - `subset`: List of columns to check
>     - `keep`: 'first', 'last', or False
>     - `inplace`: True to modify DataFrame directly
> 
> ```python
> df.drop_duplicates(subset=['name', 'address'], keep='first', inplace=True)
> ```
> 
> **Key difference:** `.duplicated()` finds, `.drop_duplicates()` removes.

**Q52.** Describe what the `keep` parameter does in `.duplicated()` and `.drop_duplicates()`. Explain all three options.

> [!success]- Answer The `keep` parameter controls which duplicate occurrences are marked/kept:
> 
> **1. `keep='first'` (default):**
> 
> - Marks/keeps the **first** occurrence
> - Marks subsequent occurrences as duplicates
> 
> ```python
> # Row 1: False (first, not marked as duplicate)
> # Row 2: True (duplicate of row 1)
> # Row 3: True (duplicate of row 1)
> ```
> 
> **2. `keep='last'`:**
> 
> - Marks/keeps the **last** occurrence
> - Marks earlier occurrences as duplicates
> 
> ```python
> # Row 1: True (duplicate of row 3)
> # Row 2: True (duplicate of row 3)
> # Row 3: False (last, not marked as duplicate)
> ```
> 
> **3. `keep=False`:**
> 
> - Marks **all** occurrences as duplicates
> - Doesn't keep any - marks everything
> 
> ```python
> # Row 1: True (duplicate)
> # Row 2: True (duplicate)
> # Row 3: True (duplicate)
> ```
> 
> Useful for finding ALL duplicate rows including the originals.

**Q53.** Explain how to use `.groupby()` and `.agg()` to handle duplicates. Why would you choose this over `.drop_duplicates()`?

> [!success]- Answer **Using `.groupby()` and `.agg()` for duplicates:**
> 
> ```python
> # Specify grouping columns (what makes a duplicate)
> column_names = ['first_name', 'last_name', 'address']
> 
> # Specify aggregation functions for other columns
> summaries = {'height': 'max', 'weight': 'mean'}
> 
> # Group and aggregate
> df = df.groupby(by=column_names).agg(summaries).reset_index()
> ```
> 
> **What this does:**
> 
> - Groups rows with same name and address
> - Takes maximum height among duplicates
> - Takes mean weight among duplicates
> - Results in one row per unique person
> 
> **Why choose this over `.drop_duplicates()`:**
> 
> `.drop_duplicates()`:
> 
> - Deletes entire rows
> - Loses information from deleted rows
> - Simple but discards data
> 
> `.groupby().agg()`:
> 
> - Preserves information from ALL duplicates
> - Combines data intelligently (max, mean, sum, etc.)
> - More sophisticated handling
> - Better when duplicates contain valuable but different information
> 
> **Example:** If person has heights [195, 196], `.drop_duplicates()` keeps one arbitrarily. `.groupby().agg()` can take the max (196) or mean (195.5), using all data.

**Q54.** Write code to check if a date column contains any future dates and fix the problem by capping at today's date.

> [!success]- Answer **Complete solution:**
> 
> ```python
> import datetime as dt
> import pandas as pd
> 
> # Step 1: Convert to datetime if needed
> user_signups['subscription_date'] = pd.to_datetime(
>     user_signups['subscription_date']
> ).dt.date
> 
> # Step 2: Get today's date
> today_date = dt.date.today()
> 
> # Step 3: Check for future dates
> future_dates = user_signups[user_signups['subscription_date'] > today_date]
> print(f"Found {len(future_dates)} future dates")
> 
> # Step 4: Cap at today's date
> user_signups.loc[
>     user_signups['subscription_date'] > today_date,
>     'subscription_date'
> ] = today_date
> 
> # Step 5: Verify fix worked
> assert user_signups['subscription_date'].max() <= today_date
> print("All dates are valid!")
> ```
> 
> **Explanation:**
> 
> 1. Convert column to date type
> 2. Get today's date for comparison
> 3. Find and count future dates
> 4. Use `.loc[]` to replace future dates with today
> 5. Assert verification ensures no future dates remain

**Q55.** Explain what the assert statement does and provide three examples of how it's used in data cleaning.

> [!success]- Answer **What assert does:** The `assert` statement tests if a condition is True. If False, it raises an `AssertionError` and stops execution. If True, it passes silently (no output).
> 
> **Syntax:**
> 
> ```python
> assert condition, "optional error message"
> ```
> 
> **Three data cleaning examples:**
> 
> **1. Verify data type conversion:**
> 
> ```python
> # After converting Revenue to integer
> df['Revenue'] = df['Revenue'].astype('int')
> assert df['Revenue'].dtype == 'int'
> # No output = passed, conversion worked
> ```
> 
> **2. Verify data range constraints:**
> 
> ```python
> # After capping ratings at 5
> movies.loc[movies['avg_rating'] > 5, 'avg_rating'] = 5
> assert movies['avg_rating'].max() <= 5
> # No output = all ratings now <= 5
> ```
> 
> **3. Verify no future dates:**
> 
> ```python
> # After cleaning dates
> import datetime as dt
> today = dt.date.today()
> assert df['subscription_date'].max() <= today
> # No output = no future dates remain
> ```
> 
> **Why use assert in data cleaning:**
> 
> - Validates cleaning operations worked correctly
> - Catches errors early in pipeline
> - Documents expected data quality
> - No output = success (Pythonic)

---

## Section D: Code Analysis & Scenarios (25 points)

_2.5 points each | 10 questions total_

**Q56.** What does this code do?

```python
sales['Revenue'] = sales['Revenue'].str.strip(')
sales['Revenue'] = sales['Revenue'].astype('int')
```

> [!success]- Answer **This code converts a currency column from string to integer.**
> 
> **Step-by-step:**
> 
> 1. **First line:** `.str.strip(')` removes dollar signs
>     - "100$" → "100"
>     - "200$" → "200"
> 2. **Second line:** `.astype('int')` converts strings to integers
>     - "100" → 100
>     - "200" → 200
> 
> **Result:** Revenue column changes from object (string) type to int, allowing mathematical operations like sum() to work correctly.

**Q57.** Fix the issues in this code:

```python
sales.info()
sales['Revenue'].sum()
```

The Revenue column is stored as strings like "100$", "200$" and sum() is concatenating instead of adding.

> [!success]- Answer **Fixed code:**
> 
> ```python
> # Check data type
> sales.info()
> 
> # Remove dollar signs
> sales['Revenue'] = sales['Revenue'].str.strip(')
> 
> # Convert to integer
> sales['Revenue'] = sales['Revenue'].astype('int')
> 
> # Now sum() works correctly
> total = sales['Revenue'].sum()
> 
> # Verify conversion
> assert sales['Revenue'].dtype == 'int'
> ```
> 
> **Explanation:** The issue was Revenue stored as strings. Need to:
> 
> 1. Remove the $ symbol with `.str.strip()`
> 2. Convert to numeric type with `.astype('int')`
> 3. Verify with assert statement

**Q58.** Complete this code to drop movies with ratings > 5:

```python
# Drop using filtering
movies = ________[________]

# Verify
assert ________ <= 5
```

> [!success]- Answer **Completed code:**
> 
> ```python
> # Drop using filtering
> movies = movies[movies['avg_rating'] <= 5]
> 
> # Verify
> assert movies['avg_rating'].max() <= 5
> ```
> 
> **Explanation:**
> 
> - `movies[movies['avg_rating'] <= 5]` keeps only rows where rating is 5 or less
> - `.max()` gets maximum value in the column
> - Assert verifies no ratings exceed 5

**Q59.** Complete this code to cap ratings at 5 instead of dropping them:

```python
# Cap ratings at 5
movies.______[movies['avg_rating'] > 5, ________] = ____

# Verify
assert movies['avg_rating'].____() <= 5
```

> [!success]- Answer **Completed code:**
> 
> ```python
> # Cap ratings at 5
> movies.loc[movies['avg_rating'] > 5, 'avg_rating'] = 5
> 
> # Verify
> assert movies['avg_rating'].max() <= 5
> ```
> 
> **Explanation:**
> 
> - `.loc[condition, column]` allows conditional value assignment
> - Sets all ratings > 5 to exactly 5
> - Preserves rows rather than dropping them

**Q60.** What does this code do?

```python
duplicates = height_weight.duplicated(subset=['first_name', 'last_name', 'address'], keep=False)
height_weight[duplicates].sort_values(by='first_name')
```

> [!success]- Answer **This code finds and displays ALL duplicate rows (including originals).**
> 
> **Step-by-step:**
> 
> 1. `.duplicated(subset=[...])` - Check only name and address columns
> 2. `keep=False` - Mark ALL occurrences as duplicates (not just subsequent ones)
> 3. `height_weight[duplicates]` - Filter to show only duplicate rows
> 4. `.sort_values(by='first_name')` - Sort by first name for easier viewing
> 
> **Result:** Shows all rows where people have the same name and address, making it easy to spot partial duplicates (where other columns differ).

**Q61.** Complete this code to remove all duplicate rows:

```python
# Remove duplicates
height_weight.________(inplace=______)

# Verify no duplicates remain
duplicates = height_weight.duplicated()
assert duplicates.__() == 0
```

> [!success]- Answer **Completed code:**
> 
> ```python
> # Remove duplicates
> height_weight.drop_duplicates(inplace=True)
> 
> # Verify no duplicates remain
> duplicates = height_weight.duplicated()
> assert duplicates.sum() == 0
> ```
> 
> **Explanation:**
> 
> - `.drop_duplicates(inplace=True)` removes duplicate rows directly
> - `.sum()` counts True values (duplicates remaining)
> - Assert verifies sum is 0 (no duplicates left)

**Q62.** What does this aggregation code do?

```python
column_names = ['first_name', 'last_name', 'address']
summaries = {'height': 'max', 'weight': 'mean'}
height_weight = height_weight.groupby(by=column_names).agg(summaries).reset_index()
```

> [!success]- Answer **This code aggregates duplicate rows intelligently instead of dropping them.**
> 
> **What it does:**
> 
> 1. **Groups by** name and address (identifying columns)
> 2. **For duplicates found:**
>     - Takes the **maximum** height value
>     - Takes the **mean** (average) weight value
> 3. **Results in** one row per unique person
> 4. **`reset_index()`** converts grouped result back to normal DataFrame
> 
> **Example:**
> 
> ```
> Before:
> Name: John, Address: 123 St, Height: 195, Weight: 85
> Name: John, Address: 123 St, Height: 196, Weight: 87
> 
> After:
> Name: John, Address: 123 St, Height: 196 (max), Weight: 86 (mean)
> ```
> 
> **Benefit:** Preserves information from all duplicate rows rather than discarding.

**Q63.** Debug this code that's trying to find future subscription dates:

```python
import datetime as dt
today_date = dt.date.today()
future = user_signups[user_signups['subscription_date'] > today_date]
```

Assume subscription_date is currently stored as object (string) type.

> [!success]- Answer **Issue:** Can't compare strings with date objects directly.
> 
> **Fixed code:**
> 
> ```python
> import datetime as dt
> import pandas as pd
> 
> # Convert to datetime first
> user_signups['subscription_date'] = pd.to_datetime(
>     user_signups['subscription_date']
> ).dt.date
> 
> # Now comparison works
> today_date = dt.date.today()
> future = user_signups[user_signups['subscription_date'] > today_date]
> 
> print(f"Found {len(future)} future dates")
> ```
> 
> **Explanation:** Must convert string dates to datetime objects using `pd.to_datetime()` before comparing with `dt.date.today()`. The `.dt.date` accessor extracts just the date part.

**Q64.** Write complete code to:

1. Find duplicates based on first_name, last_name, and address
2. Display them sorted by first_name
3. Drop the duplicates keeping the first occurrence

> [!success]- Answer **Complete solution:**
> 
> ```python
> # Step 1: Find duplicates based on specific columns
> column_names = ['first_name', 'last_name', 'address']
> duplicates = height_weight.duplicated(
>     subset=column_names,
>     keep=False  # Mark all occurrences
> )
> 
> # Step 2: Display duplicates sorted
> print("Duplicate rows found:")
> print(height_weight[duplicates].sort_values(by='first_name'))
> 
> # Step 3: Drop duplicates keeping first
> height_weight.drop_duplicates(
>     subset=column_names,
>     keep='first',
>     inplace=True
> )
> 
> # Step 4: Verify no duplicates remain
> duplicates_check = height_weight.duplicated(subset=column_names)
> assert duplicates_check.sum() == 0
> print("All duplicates removed successfully!")
> ```
> 
> **Key points:**
> 
> - Use `keep=False` to see all duplicates initially
> - Use `keep='first'` to keep only first occurrence when dropping
> - Always verify with assert after cleaning

**Q65.** A DataFrame has a marriage_status column with values 0, 1, 2, 3 stored as integers. Write code to convert it to categorical type and verify the conversion.

> [!success]- Answer **Complete solution:**
> 
> ```python
> # Step 1: Check current type and statistics
> print("Before conversion:")
> print(df['marriage_status'].dtype)
> print(df['marriage_status'].describe())
> # Shows: mean, std, min, max (numeric stats)
> 
> # Step 2: Convert to category
> df['marriage_status'] = df['marriage_status'].astype('category')
> 
> # Step 3: Verify conversion
> assert df['marriage_status'].dtype.name == 'category'
> print("\nAfter conversion:")
> print(df['marriage_status'].dtype)
> print(df['marriage_status'].describe())
> # Shows: count, unique, top, freq (categorical stats)
> ```
> 
> **Result:**
> 
> ```
> Before:
> dtype: int64
> mean: 1.4, std: 0.20 (meaningless for categories!)
> 
> After:
> dtype: category
> count: 241, unique: 4, top: 1, freq: 120 (appropriate!)
> ```
> 
> **Why this matters:** Categorical data should have categorical statistics (counts, frequencies) not numeric statistics (mean, standard deviation).

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)

1. B | 2. C | 3. B | 4. D | 5. D | 6. B | 7. C | 8. C | 9. B | 10. B
2. D | 12. C | 13. B | 14. B | 15. B | 16. B | 17. B | 18. B | 19. B | 20. C
3. B | 22. B | 23. C | 24. B | 25. B | 26. A | 27. B | 28. C | 29. B | 30. B

### Section B: True/False (15 points)

31. T | 32. T | 33. F | 34. T | 35. F | 36. F | 37. T | 38. T | 39. F | 40. T
32. T | 42. F | 43. T | 44. F | 45. T

### Section C: Short Answer (30 points)

Questions 46-55 require detailed written answers. Award full points for complete, accurate responses with code examples where applicable.

### Section D: Code Analysis (25 points)

Questions 56-65 require code analysis, debugging, or completion. Award full points for correct, working solutions with explanations.

---

## Quick Study Guide

### Data Type Conversion

**Check data types:**

```python
df.dtypes          # Show all column types
df.info()          # Detailed information
df['col'].dtype    # Single column type
```

**Convert types:**

```python
# String to integer
df['col'] = df['col'].str.strip(')
df['col'] = df['col'].astype('int')

# To category
df['col'] = df['col'].astype('category')

# To datetime
df['col'] = pd.to_datetime(df['col'])
df['col'] = pd.to_datetime(df['col']).dt.date
```

### Range Constraints

**Drop out of range:**

```python
# Method 1: Filtering
df = df[df['rating'] <= 5]

# Method 2: .drop()
df.drop(df[df['rating'] > 5].index, inplace=True)
```

**Cap at maximum:**

```python
# Set all values > 5 to 5
df.loc[df['rating'] > 5, 'rating'] = 5
```

**Date validation:**

```python
import datetime as dt

# Convert and validate
df['date'] = pd.to_datetime(df['date']).dt.date
today = dt.date.today()

# Drop future dates
df = df[df['date'] <= today]

# Or cap at today
df.loc[df['date'] > today, 'date'] = today
```

### Duplicate Handling

**Find duplicates:**

```python
# All columns
duplicates = df.duplicated()

# Specific columns
duplicates = df.duplicated(
    subset=['col1', 'col2'],
    keep='first'  # or 'last' or False
)

# View duplicates
df[duplicates]
df[duplicates].sort_values(by='col')
```

**Remove duplicates:**

```python
# Drop duplicates
df.drop_duplicates(inplace=True)

# Drop specific columns
df.drop_duplicates(
    subset=['col1', 'col2'],
    keep='first',
    inplace=True
)
```

**Aggregate duplicates:**

```python
# Group and aggregate
cols = ['name', 'address']
aggs = {'height': 'max', 'weight': 'mean'}
df = df.groupby(by=cols).agg(aggs).reset_index()
```

### Assertions

```python
# Verify data type
assert df['col'].dtype == 'int'

# Verify range
assert df['col'].max() <= 5
assert df['col'].min() >= 0

# Verify dates
assert df['date'].max() <= dt.date.today()

# Verify no duplicates
assert df.duplicated().sum() == 0
```

---

## Key Concepts Summary

### Data Types

|Type|Python|Use For|Example|
|---|---|---|---|
|Text|`str`|Names, addresses|"John", "123 Main St"|
|Integer|`int`|Counts, IDs|5, 100, 1234|
|Decimal|`float`|Measurements|98.6, 3.14|
|Boolean|`bool`|Yes/no|True, False|
|Date|`datetime`|Dates, times|2024-01-15|
|Category|`category`|Fixed categories|"Married", "Single"|

### Methods Summary

|Method|Purpose|Returns|
|---|---|---|
|`.dtypes`|Check data types|Series of types|
|`.info()`|Detailed info|None (prints)|
|`.astype()`|Convert type|Converted Series|
|`.str.strip()`|Remove characters|Cleaned strings|
|`.duplicated()`|Find duplicates|Boolean Series|
|`.drop_duplicates()`|Remove duplicates|Cleaned DataFrame|
|`.groupby().agg()`|Aggregate data|Grouped DataFrame|
|`pd.to_datetime()`|Convert to datetime|DateTime Series|

### Parameters Reference

**`.duplicated()` and `.drop_duplicates()`:**

- `subset`: List of columns to check
- `keep`: 'first', 'last', or False
- `inplace`: True/False (drop_duplicates only)

**`.groupby().agg()`:**

- `by`: Columns to group by
- Aggregation dict: {'column': 'function'}
- Common functions: 'max', 'min', 'mean', 'sum', 'count'

---

## Common Mistakes to Avoid

❌ **Not converting types before operations**

```python
# WRONG - strings concatenate
sales['Revenue'].sum()  # "100$200$300$"

# CORRECT - convert first
sales['Revenue'] = sales['Revenue'].str.strip(').astype('int')
sales['Revenue'].sum()  # 600
```

❌ **Using numeric operations on categorical data**

```python
# WRONG
df['marriage_status'].mean()  # 1.4 (meaningless!)

# CORRECT
df['marriage_status'] = df['marriage_status'].astype('category')
df['marriage_status'].describe()  # count, unique, top, freq
```

❌ **Comparing strings with dates**

```python
# WRONG
df[df['date'] > dt.date.today()]  # Error if date is string

# CORRECT
df['date'] = pd.to_datetime(df['date']).dt.date
df[df['date'] > dt.date.today()]
```

❌ **Not using inplace or reassigning**

```python
# WRONG - changes not saved
df.drop_duplicates()
df.astype('int')

# CORRECT - reassign or use inplace
df = df.drop_duplicates()
df.drop_duplicates(inplace=True)
```

❌ **Forgetting to verify with assert**

```python
# WRONG - no verification
df['col'] = df['col'].astype('int')

# CORRECT - verify conversion
df['col'] = df['col'].astype('int')
assert df['col'].dtype == 'int'
```

❌ **Using keep='first' when you want to see all duplicates**

```python
# WRONG - only shows subsequent duplicates
duplicates = df.duplicated(keep='first')

# CORRECT - shows all duplicates including first
duplicates = df.duplicated(keep=False)
```

---

## Study Strategy (5-Day Plan)

### Day 1: Data Types

- [ ] Understand 6 common data types
- [ ] Learn `.dtypes` and `.info()`
- [ ] Practice `.astype()` conversions
- [ ] Master `.str.strip()` for cleaning
- [ ] Understand numeric vs categorical
- **Practice:** Convert strings to int, create categories

### Day 2: Assert & Range Constraints

- [ ] Learn assert statement syntax
- [ ] Practice verifying conversions
- [ ] Understand range validation
- [ ] Learn to drop out of range data
- [ ] Learn to cap values
- [ ] Master date conversions
- **Practice:** Validate ratings, fix future dates

### Day 3: Finding Duplicates

- [ ] Understand complete vs partial duplicates
- [ ] Master `.duplicated()` method
- [ ] Learn all `keep` parameter options
- [ ] Practice with `subset` parameter
- [ ] Learn to view and sort duplicates
- **Practice:** Find duplicates in various scenarios

### Day 4: Removing Duplicates

- [ ] Master `.drop_duplicates()` method
- [ ] Understand `inplace` parameter
- [ ] Learn `.groupby().agg()` approach
- [ ] Compare drop vs aggregate strategies
- [ ] Practice verification with assert
- **Practice:** Clean duplicate datasets

### Day 5: Integration & Review

- [ ] Complete workflow examples
- [ ] Combine all techniques
- [ ] Debug common errors
- [ ] Practice code completion
- **Practice:** Take this practice quiz

---

## Final Exam Checklist

### Before the Exam ✓

**Data Types:**

- [ ] Know 6 data types and Python equivalents
- [ ] Can identify object = string
- [ ] Know `.astype()` syntax
- [ ] Understand when to use category type

**Range Constraints:**

- [ ] Know how to drop out of range
- [ ] Know how to cap at max/min
- [ ] Can convert dates with pd.to_datetime()
- [ ] Know how to use .dt.date accessor

**Duplicates:**

- [ ] Understand `.duplicated()` vs `.drop_duplicates()`
- [ ] Know all `keep` parameter options
- [ ] Can use `subset` parameter
- [ ] Understand `.groupby().agg()` approach

**Assertions:**

- [ ] Know assert syntax
- [ ] Can verify data types
- [ ] Can verify ranges
- [ ] Understand "no output = passed"

### During the Exam ✓

**General:**

- [ ] Read questions carefully
- [ ] Check if inplace=True is needed
- [ ] Remember to verify with assert

**Common Traps:**

- [ ] object = string type
- [ ] Sum on strings concatenates
- [ ] Categorical stats vs numeric stats
- [ ] keep='first' vs keep=False
- [ ] .duplicated() returns bool, not rows
- [ ] Must convert dates before comparing

**Quick Checks:**

- [ ] Data type: `.dtypes` or `.info()`
- [ ] Convert: `.astype()`, `pd.to_datetime()`
- [ ] Clean strings: `.str.strip()`
- [ ] Drop range: filtering or `.drop()`
- [ ] Cap: `.loc[condition, col] = value`
- [ ] Find dups: `.duplicated()`
- [ ] Remove dups: `.drop_duplicates()`
- [ ] Aggregate: `.groupby().agg()`
- [ ] Verify: `assert`

---

## Congratulations! 🎉

You've mastered:

- ✅ Data type constraints and conversions
- ✅ Range validation and constraints
- ✅ Finding and handling duplicates
- ✅ Assert statements for verification
- ✅ Complete data cleaning workflows

**You're ready to clean messy data like a pro!** 🧹🐍

---

#datacamp #data-cleaning #python #pandas #data-types #duplicates #data-quality #certification
