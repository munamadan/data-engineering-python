## Chapter Overview

This chapter covers three advanced data cleaning problems:
1. **Uniformity** - Inconsistent units and formats
2. **Cross field validation** - Checking consistency across multiple columns
3. **Completeness** - Handling missing data

---

## Uniformity

### The Problem

Data can represent the same value in different units or formats, making comparison and analysis difficult.

| Column | Issue | Examples |
|--------|-------|----------|
| **Temperature** | Different units | 32°C is also 89.6°F |
| **Weight** | Different units | 70 Kg is also 11 st. (stone) |
| **Date** | Different formats | 26-11-2019 is also 26, November, 2019 |
| **Money** | Different currencies | 100$ is also 10,763.90¥ |

---

## Example: Temperature Data

### The Problem

```python
temperatures = pd.read_csv('temperature.csv')
temperatures.head()
```

| Date | Temperature |
|------|-------------|
| 03.03.19 | 14.0 |
| 04.03.19 | 15.0 |
| 05.03.19 | 18.0 |
| 06.03.19 | 16.0 |
| 07.03.19 | **62.6** ← Outlier! |

> [!warning] Problem
> 62.6°C is unrealistic for NYC in March. Likely recorded in Fahrenheit!

### Visualizing the Problem

```python
import matplotlib.pyplot as plt

# Create scatter plot
plt.scatter(x='Date', y='Temperature', data=temperatures)
plt.title('Temperature in Celsius March 2019 - NYC')
plt.xlabel('Dates')
plt.ylabel('Temperature in Celsius')
plt.show()
```

The plot shows obvious outliers - values that are much higher than expected for Celsius.

---

## Converting Temperature Units

### Formula: Fahrenheit to Celsius

**Formula:** C = (F - 32) × 5/9

### Solution Code

```python
# Find temperatures that are likely in Fahrenheit (> 40°C is unrealistic)
temp_fah = temperatures.loc[temperatures['Temperature'] > 40, 'Temperature']

# Convert to Celsius
temp_cels = (temp_fah - 32) * (5/9)

# Replace the values
temperatures.loc[temperatures['Temperature'] > 40, 'Temperature'] = temp_cels

# Verify conversion worked
assert temperatures['Temperature'].max() < 40
```

**Step-by-step:**
1. **Identify** outliers using domain knowledge (> 40°C unrealistic)
2. **Extract** values that need conversion
3. **Apply** conversion formula
4. **Replace** original values with converted values
5. **Verify** with assertion

---

## Date Uniformity

### The Problem: Multiple Date Formats

```python
birthdays.head()
```

| Birthday | First name | Last name |
|----------|------------|-----------|
| **27/27/19** | Rowan | Nunez |
| 03-29-19 | Brynn | Yang |
| **March 3rd, 2019** | Sophia | Reilly |
| 24-03-19 | Deacon | Prince |
| 06-03-19 | Griffith | Neal |

> [!warning] Problems
> - Row 0: Invalid date (27/27/19 - no 27th month!)
> - Row 2: Text format with ordinal indicator
> - Various separators (-, /)
> - Ambiguous formats (MM-DD-YY vs DD-MM-YY)

---

## Datetime Formatting

### Understanding datetime Formats

| Date Example | datetime Format Code |
|--------------|---------------------|
| 25-12-2019 | `%d-%m-%Y` |
| December 25th 2019 | `%c` |
| 12-25-2019 | `%m-%d-%Y` |
| 2019/12/25 | `%Y/%m/%d` |

### Common Format Codes

| Code | Meaning | Example |
|------|---------|---------|
| `%d` | Day of month (01-31) | 25 |
| `%m` | Month (01-12) | 12 |
| `%Y` | 4-digit year | 2019 |
| `%y` | 2-digit year | 19 |
| `%B` | Full month name | December |
| `%b` | Abbreviated month | Dec |

---

## Using pd.to_datetime()

### Automatic Format Recognition

**`pd.to_datetime()`** can recognize most formats automatically, but sometimes fails with erroneous or unrecognizable formats.

### Handling Conversion Errors

```python
# This will fail with ValueError!
birthdays['Birthday'] = pd.to_datetime(birthdays['Birthday'])
# ValueError: month must be in 1..12
```

**Solution: Use `errors='coerce'`**

```python
# Convert to datetime, return NA for failed conversions
birthdays['Birthday'] = pd.to_datetime(birthdays['Birthday'], 
                                       errors='coerce')
```

Result:

| Birthday | First name | Last name |
|----------|------------|-----------|
| **NaT** | Rowan | Nunez |
| 2019-03-29 | Brynn | Yang |
| 2019-03-03 | Sophia | Reilly |
| 2019-03-24 | Deacon | Prince |
| 2019-06-03 | Griffith | Neal |

> [!info] NaT
> **NaT** = "Not a Time" - pandas' null value for datetime data, similar to NaN for numeric data.

---

## Formatting Datetime Output

### Using .dt.strftime()

After converting to datetime, you can format the output in a consistent way:

```python
# Convert to desired format
birthdays['Birthday'] = birthdays['Birthday'].dt.strftime("%d-%m-%Y")
```

Result:

| Birthday | First name | Last name |
|----------|------------|-----------|
| NaT | Rowan | Nunez |
| **29-03-2019** | Brynn | Yang |
| **03-03-2019** | Sophia | Reilly |
| **24-03-2019** | Deacon | Prince |
| **03-06-2019** | Griffith | Neal |

All valid dates now in consistent DD-MM-YYYY format!

---

## Handling Ambiguous Dates

### The Problem: Is 2019-03-08 in August or March?

**Three approaches:**

1. **Convert to NA and treat accordingly**
   - Set ambiguous dates to NaT/NaN
   - Impute or drop later

2. **Infer format by understanding data source**
   - Check documentation
   - Ask data provider
   - Look at data collection context

3. **Infer format by understanding previous and subsequent data**
   - Look at patterns in surrounding rows
   - Use domain knowledge (e.g., seasonal patterns)

---

## Cross Field Validation

### Definition

**Cross field validation** = Using multiple fields in a dataset to sanity check data integrity.

### Example: Flight Passengers

```python
flights = pd.read_csv('flights.csv')
flights.head()
```

| flight_number | economy_class | business_class | first_class | total_passengers |
|---------------|---------------|----------------|-------------|------------------|
| DL140 | 100 | 60 | 40 | 200 |
| BA248 | 130 | 100 | 70 | 300 |
| MEA124 | 100 | 50 | 50 | 200 |
| AFR939 | 140 | 70 | 90 | 300 |
| TKA101 | 130 | 100 | 20 | 250 |

**Validation:** Does economy + business + first = total?

---

## Implementing Cross Field Validation

### Step 1: Calculate Expected Total

```python
# Sum across classes
sum_classes = flights[['economy_class', 'business_class', 'first_class']].sum(axis=1)
```

> [!info] axis=1
> `axis=1` means sum across columns (row-wise). `axis=0` would sum down columns.

### Step 2: Compare with Actual Total

```python
# Check if calculated sum equals reported total
passenger_equ = sum_classes == flights['total_passengers']
```

### Step 3: Filter Inconsistent Data

```python
# Find rows where totals DON'T match
inconsistent_pass = flights[~passenger_equ]

# Keep only rows where totals DO match
consistent_pass = flights[passenger_equ]
```

---

## Cross Field Validation: Age and Birthday

### Example Dataset

```python
users.head()
```

| user_id | Age | Birthday |
|---------|-----|----------|
| 32985 | 22 | 1998-03-02 |
| 94387 | 27 | 1993-12-04 |
| 34236 | 42 | 1978-11-24 |
| 12551 | 31 | 1989-01-03 |
| 55212 | 18 | 2002-07-02 |

**Validation:** Does Birthday match the Age?

---

## Calculating Age from Birthday

```python
import pandas as pd
import datetime as dt

# Convert to datetime
users['Birthday'] = pd.to_datetime(users['Birthday'])

# Get today's date
today = dt.date.today()

# Calculate age from birthday
age_manual = today.year - users['Birthday'].dt.year

# Compare calculated age with reported age
age_equ = age_manual == users['Age']

# Find inconsistent records
inconsistent_age = users[~age_equ]
consistent_age = users[age_equ]
```

**What this does:**
1. Converts Birthday to datetime
2. Calculates current year - birth year
3. Compares calculated age with reported Age column
4. Identifies mismatches

---

## What to Do with Inconsistencies?

When cross field validation catches problems:

1. **Drop the inconsistent rows**
   - If data quality is poor
   - If can't determine correct value

2. **Keep one field, recalculate the other**
   - Trust birthday, recalculate age
   - Trust total passengers, ignore class breakdown

3. **Investigate further**
   - Check original source
   - Contact data provider
   - Look for patterns in errors

4. **Flag for review**
   - Mark as uncertain
   - Require manual verification

---

## Completeness: Missing Data

### What is Missing Data?

Missing data can be represented as:
- `NA` or `NaN` - pandas standard
- `0` - Sometimes used incorrectly
- `.` or empty string
- Placeholder values (like -999)

### Causes of Missing Data

1. **Technical error**
   - Sensor malfunction
   - Data transmission failure
   - Database corruption

2. **Human error**
   - Forgot to fill field
   - Skipped question in survey
   - Data entry mistakes

---

## Example: Air Quality Data

```python
airquality = pd.read_csv('airquality.csv')
print(airquality)
```

| Date | Temperature | CO2 |
|------|-------------|-----|
| 20/04/2004 | 16.8 | 0.0 |
| 07/06/2004 | 18.7 | 0.8 |
| 20/06/2004 | **-40.0** | **NaN** |
| 01/06/2004 | 19.6 | 1.8 |
| 19/02/2005 | 11.2 | 1.2 |

> [!warning] Problems
> - Row 2: CO2 is NaN (missing)
> - Row 2: Temperature is -40.0 (likely missing, encoded as placeholder)

---

## Detecting Missing Values

### Using .isna()

```python
# Return boolean DataFrame showing missing values
airquality.isna()
```

| Date | Temperature | CO2 |
|------|-------------|-----|
| False | False | False |
| False | False | False |
| False | False | **True** |
| False | False | False |
| False | False | False |

### Counting Missing Values

```python
# Get summary of missingness
airquality.isna().sum()
```

Output:
```
Date            0
Temperature     0
CO2           366
dtype: int64
```

366 missing CO2 values!

---

## Visualizing Missing Data with missingno

### The missingno Package

**missingno** is a useful package for visualizing and understanding missing data patterns.

```python
import missingno as msno
import matplotlib.pyplot as plt

# Visualize missingness
msno.matrix(airquality)
plt.show()
```

**What the plot shows:**
- White lines = missing data
- Black/colored = present data
- Patterns in missingness become visible
- Sparkline on right shows completeness per row

---

## Analyzing Missing vs Complete Data

### Separate Missing and Complete Subsets

```python
# Isolate missing and complete values
missing = airquality[airquality['CO2'].isna()]
complete = airquality[~airquality['CO2'].isna()]
```

### Compare Statistics

```python
# Describe complete DataFrame
complete.describe()
```

|  | Temperature | CO2 |
|--|-------------|-----|
| count | 8991.0 | 8991.0 |
| mean | 18.3 | 1.74 |
| std | 8.83 | 1.54 |
| min | -1.9 | 0.0 |
| max | 44.6 | 11.9 |

```python
# Describe missing DataFrame
missing.describe()
```

|  | Temperature | CO2 |
|--|-------------|-----|
| count | 366.0 | 0.0 |
| mean | **-39.7** | NaN |
| std | 5.99 | NaN |
| min | **-49.0** | NaN |
| max | **-30.0** | NaN |

> [!info] Key Finding
> Missing CO2 values occur when Temperature is around -40°C!
> This is unrealistic - likely a placeholder value for missing temperature data too.

---

## Visualizing Patterns in Missingness

### Sorted Visualization

```python
# Sort by Temperature to see pattern
sorted_airquality = airquality.sort_values(by='Temperature')

# Visualize
msno.matrix(sorted_airquality)
plt.show()
```

When sorted, the pattern becomes clear: all missing CO2 values cluster at extreme temperature values, indicating **systematic missingness**.

---

## Types of Missingness

### 1. Missing Completely at Random (MCAR)

- No pattern in missingness
- Missing values are random
- Example: Random sensor failures

**Visualization:** Scattered white lines throughout

### 2. Missing at Random (MAR)

- Missingness related to OTHER variables
- Pattern can be explained by observed data
- Example: Low-income respondents skip income questions

**Visualization:** Missing values correlated with other columns

### 3. Missing Not at Random (MNAR)

- Missingness related to the VALUE itself
- Pattern in the missing variable
- Example: High earners don't report income

**Visualization:** Systematic pattern within the column itself

> [!important] Why This Matters
> The type of missingness determines the best handling strategy:
> - **MCAR**: Safe to drop or use simple imputation
> - **MAR**: Can use other columns to predict missing values
> - **MNAR**: Most challenging - need domain expertise

---

## Handling Missing Data

### Simple Approaches

#### 1. Drop Missing Data

```python
# Drop rows where CO2 is missing
airquality_dropped = airquality.dropna(subset=['CO2'])
```

Result:

| Date | Temperature | CO2 |
|------|-------------|-----|
| 05/03/2005 | 8.5 | 2.5 |
| 23/08/2004 | 21.8 | 0.0 |
| 18/02/2005 | 6.3 | 1.0 |
| 13/03/2005 | 19.9 | 0.1 |
| 02/04/2005 | 17.0 | 0.8 |

**When to use:** MCAR, small percentage missing, plenty of data

#### 2. Impute with Statistical Measures

```python
# Calculate mean of CO2
co2_mean = airquality['CO2'].mean()

# Fill missing values with mean
airquality_imputed = airquality.fillna({'CO2': co2_mean})
```

Result:

| Date | Temperature | CO2 |
|------|-------------|-----|
| 05/03/2005 | 8.5 | 2.500000 |
| 23/08/2004 | 21.8 | 0.000000 |
| 18/02/2005 | 6.3 | 1.000000 |
| 08/02/2005 | -31.0 | **1.739584** ← Imputed |
| 13/03/2005 | 19.9 | 0.100000 |

**Other options:**
- `.fillna()` with median: Less affected by outliers
- `.fillna()` with mode: For categorical data
- Forward fill: Use previous value
- Backward fill: Use next value

---

## More Complex Approaches

### 1. Algorithmic Imputation

- Interpolation (linear, polynomial)
- Time series methods (for temporal data)
- KNN imputation (use similar rows)

### 2. Machine Learning Models

- Train model on complete data
- Predict missing values
- More sophisticated but requires:
  - Enough complete data
  - Appropriate features
  - Computational resources

---

## Summary of Methods

### Uniformity

```python
# Temperature conversion
temp_cels = (temp_fah - 32) * (5/9)
df.loc[condition, 'col'] = temp_cels

# Date conversion
df['date'] = pd.to_datetime(df['date'], errors='coerce')
df['date'] = df['date'].dt.strftime("%d-%m-%Y")
```

### Cross Field Validation

```python
# Calculate expected value
expected = df[cols].sum(axis=1)

# Compare with actual
matches = expected == df['actual']

# Filter
inconsistent = df[~matches]
consistent = df[matches]
```

### Completeness

```python
# Detect missing
df.isna()
df.isna().sum()

# Separate
missing = df[df['col'].isna()]
complete = df[~df['col'].isna()]

# Handle
df.dropna(subset=['col'])  # Drop
df.fillna({'col': value})  # Impute
```

---

## Key Takeaways

✅ **Uniformity** - Ensure consistent units and formats across data  
✅ **Temperature** - Convert between units using formulas  
✅ **Dates** - Use pd.to_datetime() with errors='coerce'  
✅ **Cross field validation** - Check consistency across related columns  
✅ **Sum validation** - Verify totals match component sums  
✅ **Age validation** - Calculate from birthday and compare  
✅ **Missing data** - Identify with .isna(), visualize with missingno  
✅ **Missingness types** - MCAR, MAR, MNAR affect handling strategy  
✅ **Handling missing** - Drop or impute based on situation  

---

#datacamp #data-cleaning #python #pandas #uniformity #cross-validation #missing-data #datetime #certification

