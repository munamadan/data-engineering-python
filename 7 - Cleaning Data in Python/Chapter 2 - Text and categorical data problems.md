
## Membership Constraints

### What Are Categories?

Categories are a **predefined finite set** of possible values that a variable can take.

| Type of Data | Example Values | Numeric Representation |
|--------------|----------------|------------------------|
| Marriage Status | unmarried, married | 0, 1 |
| Household Income Category | 0-20K, 20-40K, ... | 0, 1, ... |
| Loan Status | default, payed, no_loan | 0, 1, 2 |

> [!info] Key Principle
> Marriage status can only be **unmarried** OR **married** - nothing else is valid!

### Why Do We Have Category Problems?

1. **Data entry errors** - Typos or misspellings
2. **Multiple data sources** - Different encoding systems
3. **Free text fields** - Users entering invalid values
4. **System migrations** - Legacy data with different categories

---

## Finding Inconsistent Categories

### Example: Blood Type Data

**Valid blood types:** O-, O+, A-, A+, B+, B-, AB+, AB-

```python
# Read study data
study_data = pd.read_csv('study.csv')
```

| name | birthday | blood_type |
|------|----------|------------|
| Beth | 2019-10-20 | B- |
| Ignatius | 2020-07-08 | A- |
| Paul | 2019-08-12 | O+ |
| Helen | 2019-03-17 | O- |
| Jennifer | 2019-12-17 | **Z+** ← Invalid! |
| Kennedy | 2020-04-27 | A+ |
| Keith | 2019-04-19 | AB+ |

**Valid categories DataFrame:**

| blood_type |
|------------|
| O- |
| O+ |
| A- |
| A+ |
| B+ |
| B- |
| AB+ |
| AB- |

---

## Using Set Difference to Find Invalid Categories

### Method: Set Difference

```python
# Find inconsistent categories
inconsistent_categories = set(study_data['blood_type']).difference(categories['blood_type'])
print(inconsistent_categories)
```

Output:
```
{'Z+'}
```

**What this does:**
- `set(study_data['blood_type'])` - All unique values in data
- `.difference(categories['blood_type'])` - Values in data but NOT in valid categories
- Result: Invalid categories

### Finding Rows with Invalid Categories

```python
# Get rows with inconsistent categories
inconsistent_rows = study_data['blood_type'].isin(inconsistent_categories)
study_data[inconsistent_rows]
```

Output:

| name | birthday | blood_type |
|------|----------|------------|
| Jennifer | 2019-12-17 | Z+ |

---

## Understanding Joins (Visual Concept)

### Anti Join (Finding Mismatches)

An **anti join** returns rows from the left table that DON'T have matches in the right table.

```
study_data          categories
blood_type          blood_type
-----------         ----------
B-                  O-
A-           ←      O+
O+           Match  A-
O-           →      A+
Z+           ✗      B+
A+           No     B-
AB+          Match  AB+
                    AB-
```

Result: Only Z+ (no match in categories)

### Inner Join (Finding Matches)

An **inner join** returns only rows that exist in BOTH tables.

```
study_data    INNER JOIN    categories    =    Result
blood_type                  blood_type         blood_type
-----------                 ----------         ----------
B-                          O-                 B-
A-            ←→ Match →    O+                 A-
O+                          A-                 O+
O-                          A+                 O-
Z+            ✗ No Match    B+                 A+
A+                          B-                 AB+
AB+                         AB+
                            AB-
```

---

## Dropping Inconsistent Categories

```python
# Find inconsistent categories
inconsistent_categories = set(study_data['blood_type']).difference(categories['blood_type'])

# Get boolean mask of inconsistent rows
inconsistent_rows = study_data['blood_type'].isin(inconsistent_categories)

# Get inconsistent data
inconsistent_data = study_data[inconsistent_rows]

# Drop inconsistent categories using ~ (NOT operator)
consistent_data = study_data[~inconsistent_rows]
```

Result:

| name | birthday | blood_type |
|------|----------|------------|
| Beth | 2019-10-20 | B- |
| Ignatius | 2020-07-08 | A- |
| Paul | 2019-08-12 | O+ |
| Helen | 2019-03-17 | O- |
| Kennedy | 2020-04-27 | A+ |
| Keith | 2019-04-19 | AB+ |

> [!tip] The ~ Operator
> `~` is the NOT operator in pandas. It inverts boolean values: True → False, False → True

---

## Categorical Variable Problems

### Three Types of Errors

| Error Type | Description | Example |
|------------|-------------|---------|
| **I. Value Inconsistency** | Same value written differently | 'married', 'Maried', 'UNMARRIED' |
| **II. Collapsing Categories** | Creating or mapping groups | Grouping income into ranges |
| **III. Wrong Data Type** | Not stored as category | Numeric codes for categories |

---

## Problem I: Value Inconsistency

### Issue 1: Capitalization

```python
# Get marriage status column
marriage_status = demographics['marriage_status']
marriage_status.value_counts()
```

Output:
```
unmarried    352
married      268
MARRIED      204
UNMARRIED    176
dtype: int64
```

> [!warning] Problem
> 'married' and 'MARRIED' are treated as different categories!

### Solution: Standardize Capitalization

**Option A: Uppercase**
```python
# Capitalize all values
demographics['marriage_status'] = demographics['marriage_status'].str.upper()
demographics['marriage_status'].value_counts()
```

Output:
```
UNMARRIED    528
MARRIED      472
```

**Option B: Lowercase**
```python
# Lowercase all values
demographics['marriage_status'] = demographics['marriage_status'].str.lower()
demographics['marriage_status'].value_counts()
```

Output:
```
unmarried    528
married      472
```

### Alternative: Using .groupby()

```python
# Get value counts with groupby
demographics.groupby('marriage_status').count()
```

Output:

| marriage_status | household_income | gender |
|-----------------|------------------|--------|
| MARRIED | 204 | 204 |
| UNMARRIED | 176 | 176 |
| married | 268 | 268 |
| unmarried | 352 | 352 |

---

### Issue 2: Trailing Spaces

```python
# Get marriage status column
marriage_status = demographics['marriage_status']
marriage_status.value_counts()
```

Output:
```
unmarried     352  ← no space
unmarried     268  ← trailing space
married       204  ← trailing space
married       176  ← leading space
dtype: int64
```

> [!warning] Problem
> 'married' and 'married ' are different due to spaces!

### Solution: Strip Whitespace

```python
# Strip all spaces (leading and trailing)
demographics['marriage_status'] = demographics['marriage_status'].str.strip()
demographics['marriage_status'].value_counts()
```

Output:
```
unmarried    528
married      472
```

> [!tip] String Methods
> - `.str.strip()` - Remove spaces from both ends
> - `.str.lstrip()` - Remove spaces from left (start)
> - `.str.rstrip()` - Remove spaces from right (end)

---

## Problem II: Collapsing Data into Categories

### Method 1: Creating Categories from Continuous Data - qcut()

**Use `pd.qcut()`** when you want **equal-sized groups** (quantile-based).

```python
import pandas as pd

# Create 3 equal-sized groups
group_names = ['0-200K', '200K-500K', '500K+']
demographics['income_group'] = pd.qcut(
    demographics['household_income'],
    q=3,
    labels=group_names
)

# View results
demographics[['income_group', 'household_income']]
```

Output:

| income_group | household_income |
|--------------|------------------|
| 200K-500K | 189243 |
| 500K+ | 778533 |
| ... | ... |

**What qcut() does:**
- Divides data into quantiles (equal number of observations per bin)
- `q=3` creates 3 groups with approximately same number of rows
- Automatically calculates boundaries

---

### Method 2: Creating Categories with Custom Ranges - cut()

**Use `pd.cut()`** when you want **specific ranges** (value-based).

```python
import numpy as np

# Define custom ranges
ranges = [0, 200000, 500000, np.inf]
group_names = ['0-200K', '200K-500K', '500K+']

# Create income group column
demographics['income_group'] = pd.cut(
    demographics['household_income'],
    bins=ranges,
    labels=group_names
)

demographics[['income_group', 'household_income']]
```

Output:

| income_group | household_income |
|--------------|------------------|
| 0-200K | 189243 |
| 500K+ | 778533 |
| ... | ... |

**What cut() does:**
- Divides data into bins with specific boundaries
- `bins=[0, 200000, 500000, np.inf]` defines edges
- Groups can have different numbers of observations

### qcut() vs cut() Comparison

| Aspect | qcut() | cut() |
|--------|--------|-------|
| **Purpose** | Equal-sized groups | Custom ranges |
| **Bins** | Based on quantiles | Based on values |
| **Distribution** | Equal rows per bin | Can be uneven |
| **Use when** | Want balanced groups | Have specific ranges |

---

### Method 3: Mapping Categories to Fewer Categories

Reduce existing categories to broader groups.

```python
# Original operating system categories
# 'Microsoft', 'MacOS', 'IOS', 'Android', 'Linux'

# Create mapping dictionary
mapping = {
    'Microsoft': 'DesktopOS',
    'MacOS': 'DesktopOS',
    'Linux': 'DesktopOS',
    'IOS': 'MobileOS',
    'Android': 'MobileOS'
}

# Replace categories
devices['operating_system'] = devices['operating_system'].replace(mapping)

# Check unique values
devices['operating_system'].unique()
```

Output:
```
array(['DesktopOS', 'MobileOS'], dtype=object)
```

**What this does:**
- Maps 5 categories → 2 broader categories
- Desktop systems (Microsoft, MacOS, Linux) → 'DesktopOS'
- Mobile systems (IOS, Android) → 'MobileOS'

---

## Cleaning Text Data

### What is Text Data?

| Type of Data | Example Values |
|--------------|----------------|
| Names | Alex, Sara |
| Phone numbers | +96171679912 |
| Emails | adel@datacamp.com |
| Passwords | (sensitive) |

### Common Text Data Problems

1. **Data inconsistency** - Different formats: +96171679912 vs 0096171679912
2. **Fixed length violations** - Password must be ≥ 8 characters
3. **Typos** - +961.71.679912 (dots instead of dashes)

---

## Example: Cleaning Phone Numbers

### Original Data

```python
phones = pd.read_csv('phones.csv')
print(phones)
```

| Full name | Phone number |
|-----------|--------------|
| Noelani A. Gray | 001-702-397-5143 |
| Myles Z. Gomez | 001-329-485-0540 |
| Gil B. Silva | 001-195-492-2338 |
| Prescott D. Hardin | **+1-297-996-4904** ← Inconsistent format |
| Benedict G. Valdez | 001-969-820-3536 |
| Reece M. Andrews | **4138** ← Length violation |
| Hayfa E. Keith | 001-536-175-8444 |

---

### Step 1: Replace "+" with "00"

```python
# Replace "+" with "00"
phones["Phone number"] = phones["Phone number"].str.replace("+", "00")
```

Result:

| Full name | Phone number |
|-----------|--------------|
| Prescott D. Hardin | **001-297-996-4904** |

---

### Step 2: Remove Dashes

```python
# Replace "-" with nothing (empty string)
phones["Phone number"] = phones["Phone number"].str.replace("-", "")
```

Result:

| Full name | Phone number |
|-----------|--------------|
| Noelani A. Gray | 0017023975143 |
| Myles Z. Gomez | 0013294850540 |
| Prescott D. Hardin | 0012979964904 |
| Reece M. Andrews | 4138 |

---

### Step 3: Handle Length Violations

```python
# Get length of each phone number
digits = phones['Phone number'].str.len()

# Replace phone numbers with < 10 digits to NaN
phones.loc[digits < 10, "Phone number"] = np.nan
```

Result:

| Full name | Phone number |
|-----------|--------------|
| Noelani A. Gray | 0017023975143 |
| Reece M. Andrews | **NaN** ← Too short! |
| Hayfa E. Keith | 0015361758444 |

---

### Step 4: Verify with Assertions

```python
# Find length of each row in Phone number column
sanity_check = phones['Phone number'].str.len()

# Assert minimum phone number length is 10
assert sanity_check.min() >= 10

# Assert no numbers contain "+" or "-"
assert phones['Phone number'].str.contains("+|-").any() == False
```

> [!success] Remember
> assert returns nothing if the condition passes - no output means success!

---

## Regular Expressions (Regex)

### The Problem: Complex Patterns

What if phone numbers have many different formats?

```python
phones.head()
```

| Full name | Phone number |
|-----------|--------------|
| Olga Robinson | +(01706)-25891 |
| Justina Kim | +0500-571437 |
| Tamekah Henson | +0800-1111 |
| Miranda Solis | +07058-879063 |
| Caldwell Gilliam | +(016977)-8424 |

> [!info] Regular Expressions
> Regex is like **supercharged Control + F** - pattern matching on steroids!

---

### Regex Solution: Remove All Non-Digits

```python
# Replace all non-digit characters with nothing
phones['Phone number'] = phones['Phone number'].str.replace(r'\D+', '')

phones.head()
```

Result:

| Full name | Phone number |
|-----------|--------------|
| Olga Robinson | 0170625891 |
| Justina Kim | 0500571437 |
| Tamekah Henson | 08001111 |
| Miranda Solis | 07058879063 |
| Caldwell Gilliam | 0169778424 |

### Understanding the Regex Pattern

**Pattern:** `r'\D+'`

| Part | Meaning |
|------|---------|
| `r` | Raw string (treats backslashes literally) |
| `\D` | Any non-digit character (NOT 0-9) |
| `+` | One or more occurrences |

**What it matches:** Any sequence of non-digit characters: `+`, `-`, `(`, `)`, spaces, letters, etc.

**Result:** Replaces all non-digits with empty string, leaving only numbers!

---

## Summary of String Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `.str.upper()` | Convert to uppercase | 'married' → 'MARRIED' |
| `.str.lower()` | Convert to lowercase | 'MARRIED' → 'married' |
| `.str.strip()` | Remove leading/trailing spaces | ' married ' → 'married' |
| `.str.replace(old, new)` | Replace patterns | '+1' → '001' |
| `.str.len()` | Get string length | '001-702' → 7 |
| `.str.contains(pattern)` | Check if contains pattern | Contains '+' → True |

---

## Summary of Key Methods

### Finding Invalid Categories

```python
# Set difference
invalid = set(df['col']).difference(valid_categories['col'])

# Check membership
invalid_rows = df['col'].isin(invalid)

# Drop invalid
clean_df = df[~invalid_rows]
```

### Value Consistency

```python
# Standardize case
df['col'] = df['col'].str.upper()  # or .lower()

# Remove spaces
df['col'] = df['col'].str.strip()
```

### Creating Categories

```python
# Equal-sized groups (quantiles)
pd.qcut(df['col'], q=3, labels=names)

# Custom ranges
pd.cut(df['col'], bins=[0, 100, 200], labels=names)

# Map to fewer categories
df['col'].replace(mapping_dict)
```

### Text Cleaning

```python
# Replace patterns
df['col'].str.replace('old', 'new')

# Regex replace
df['col'].str.replace(r'\D+', '')  # Remove non-digits

# Length validation
lengths = df['col'].str.len()
df.loc[lengths < min_len, 'col'] = np.nan
```

---

## Key Takeaways

✅ **Membership constraints** - Categories must be from valid set  
✅ **Set difference** - Find invalid categories  
✅ **Value inconsistency** - Standardize case and trim spaces  
✅ **Collapsing categories** - Use qcut(), cut(), or .replace()  
✅ **Text cleaning** - Use .str methods for cleaning  
✅ **Regex** - Powerful pattern matching for complex text  
✅ **Assertions** - Verify cleaning with assert statements  

---

#datacamp #data-cleaning #python #pandas #categorical-data #text-data #regex #certification

## Section A: Multiple Choice Questions (60 points)
*2 points each | 30 questions total*

### Membership Constraints (Questions 1-10)

**Q1.** What are membership constraints in categorical data?

- [ ] A) Data must be numbers
- [ ] B) Data must come from a predefined finite set of categories
- [ ] C) Data must be text
- [ ] D) Data must be unique

> [!success]- Answer
> **Correct Answer: B) Data must come from a predefined finite set of categories**
> 
> Membership constraints mean values must be from a specific set. For example, marriage status can only be 'unmarried' OR 'married' - nothing else is valid.

**Q2.** Which method finds values in your data that are NOT in the valid categories?

- [ ] A) `.intersection()`
- [ ] B) `.difference()`
- [ ] C) `.union()`
- [ ] D) `.subtract()`

> [!success]- Answer
> **Correct Answer: B) `.difference()`**
> 
> `set(data['col']).difference(valid['col'])` returns values in data but NOT in valid categories.

**Q3.** What does the `~` operator do in pandas?

- [ ] A) Adds values
- [ ] B) Inverts boolean values (NOT operator)
- [ ] C) Removes columns
- [ ] D) Sorts data

> [!success]- Answer
> **Correct Answer: B) Inverts boolean values (NOT operator)**
> 
> `~` flips True to False and False to True. Used to keep non-duplicate/non-invalid rows: `df[~invalid_rows]`

**Q4.** What does `.isin()` do?

- [ ] A) Checks if values are in a list
- [ ] B) Converts to integer
- [ ] C) Finds duplicates
- [ ] D) Sorts values

> [!success]- Answer
> **Correct Answer: A) Checks if values are in a list**
> 
> `.isin(list)` returns True for rows where the value is in the specified list: `df['col'].isin(['A', 'B'])`

**Q5.** Which type of join returns rows from the left table that DON'T have matches in the right table?

- [ ] A) Inner join
- [ ] B) Left join
- [ ] C) Anti join
- [ ] D) Outer join

> [!success]- Answer
> **Correct Answer: C) Anti join**
> 
> An anti join (or left anti join) finds rows in the left table with no match in the right table - useful for finding invalid categories.

**Q6.** Which join returns only rows that exist in BOTH tables?

- [ ] A) Inner join
- [ ] B) Left join
- [ ] C) Right join
- [ ] D) Anti join

> [!success]- Answer
> **Correct Answer: A) Inner join**
> 
> Inner join returns only matching rows from both tables - useful for keeping only valid categories.

**Q7.** What causes membership constraint violations?

- [ ] A) Data entry errors
- [ ] B) Multiple data sources with different encoding
- [ ] C) Free text fields allowing invalid input
- [ ] D) All of the above

> [!success]- Answer
> **Correct Answer: D) All of the above**
> 
> Membership violations occur due to data entry errors, inconsistent encoding across sources, free text fields, and system migrations.

**Q8.** How do you get rows with invalid categories after finding them?

- [ ] A) `df[invalid_categories]`
- [ ] B) `df[df['col'].isin(invalid_categories)]`
- [ ] C) `df.filter(invalid_categories)`
- [ ] D) `df.select(invalid_categories)`

> [!success]- Answer
> **Correct Answer: B) `df[df['col'].isin(invalid_categories)]`**
> 
> Use `.isin()` to create boolean mask, then filter: `invalid_rows = df['col'].isin(invalid_categories); df[invalid_rows]`

**Q9.** How do you DROP rows with invalid categories?

- [ ] A) `df[invalid_rows]`
- [ ] B) `df[~invalid_rows]`
- [ ] C) `df.drop(invalid_rows)`
- [ ] D) `df.remove(invalid_rows)`

> [!success]- Answer
> **Correct Answer: B) `df[~invalid_rows]`**
> 
> Use `~` (NOT) operator to keep rows that are NOT invalid: `consistent_data = df[~invalid_rows]`

**Q10.** What's the first step in finding inconsistent categories?

- [ ] A) Drop the data
- [ ] B) Compare actual values to valid categories
- [ ] C) Convert to uppercase
- [ ] D) Remove spaces

> [!success]- Answer
> **Correct Answer: B) Compare actual values to valid categories**
> 
> First identify what's invalid using set difference, then decide how to handle it.

### Value Consistency (Questions 11-20)

**Q11.** What are the three types of categorical variable problems mentioned?

- [ ] A) Spelling, grammar, punctuation
- [ ] B) Value inconsistency, collapsing categories, wrong data type
- [ ] C) Missing, duplicate, incorrect
- [ ] D) Text, numbers, dates

> [!success]- Answer
> **Correct Answer: B) Value inconsistency, collapsing categories, wrong data type**
> 
> The three problems are: I) Value inconsistency (case, spaces), II) Collapsing categories (grouping), III) Wrong data type (not category).

**Q12.** What method converts strings to uppercase?

- [ ] A) `.upper()`
- [ ] B) `.str.upper()`
- [ ] C) `.to_upper()`
- [ ] D) `.uppercase()`

> [!success]- Answer
> **Correct Answer: B) `.str.upper()`**
> 
> Use `.str.upper()` to convert all strings to uppercase: `df['col'] = df['col'].str.upper()`

**Q13.** What method converts strings to lowercase?

- [ ] A) `.lower()`
- [ ] B) `.str.lower()`
- [ ] C) `.to_lower()`
- [ ] D) `.lowercase()`

> [!success]- Answer
> **Correct Answer: B) `.str.lower()`**
> 
> Use `.str.lower()` to convert all strings to lowercase: `df['col'] = df['col'].str.lower()`

**Q14.** What method removes leading and trailing whitespace?

- [ ] A) `.trim()`
- [ ] B) `.str.strip()`
- [ ] C) `.remove_spaces()`
- [ ] D) `.clean()`

> [!success]- Answer
> **Correct Answer: B) `.str.strip()`**
> 
> `.str.strip()` removes spaces from both ends: `df['col'] = df['col'].str.strip()`

**Q15.** Why might 'married' and 'married ' be counted as different categories?

- [ ] A) Different data types
- [ ] B) Trailing whitespace
- [ ] C) Different encoding
- [ ] D) Case sensitivity

> [!success]- Answer
> **Correct Answer: B) Trailing whitespace**
> 
> 'married' (no space) and 'married ' (trailing space) are different strings. Use `.str.strip()` to remove spaces.

**Q16.** How can you standardize 'married', 'Married', 'MARRIED' to one format?

- [ ] A) Use `.replace()`
- [ ] B) Use `.str.upper()` or `.str.lower()`
- [ ] C) Use `.astype()`
- [ ] D) Use `.strip()`

> [!success]- Answer
> **Correct Answer: B) Use `.str.upper()` or `.str.lower()`**
> 
> Convert all to same case: `df['col'].str.upper()` makes all 'MARRIED', or `.str.lower()` makes all 'married'.

**Q17.** What does `.value_counts()` show?

- [ ] A) Sum of values
- [ ] B) Count of each unique value
- [ ] C) Mean of values
- [ ] D) Length of strings

> [!success]- Answer
> **Correct Answer: B) Count of each unique value**
> 
> `.value_counts()` shows frequency of each unique value in a column, useful for spotting inconsistencies.

**Q18.** What method can also show counts grouped by a column?

- [ ] A) `.count()`
- [ ] B) `.groupby().count()`
- [ ] C) `.sum()`
- [ ] D) `.aggregate()`

> [!success]- Answer
> **Correct Answer: B) `.groupby().count()`**
> 
> `df.groupby('col').count()` shows counts for each group, alternative to `.value_counts()`.

**Q19.** Which is the correct order to fix value inconsistency?

- [ ] A) Remove spaces, then standardize case
- [ ] B) Standardize case, then remove spaces
- [ ] C) Either order works
- [ ] D) Remove duplicates first

> [!success]- Answer
> **Correct Answer: C) Either order works**
> 
> Both `.str.strip()` and `.str.upper()`/`.str.lower()` can be done in any order - they're independent operations.

**Q20.** What's the best practice for standardizing text categories?

- [ ] A) Always use uppercase
- [ ] B) Always use lowercase
- [ ] C) Be consistent - choose one approach and stick with it
- [ ] D) Mix uppercase and lowercase

> [!success]- Answer
> **Correct Answer: C) Be consistent - choose one approach and stick with it**
> 
> Choose either `.upper()` or `.lower()` and use consistently throughout your project.

### Collapsing Categories (Questions 21-30)

**Q21.** What does `pd.qcut()` do?

- [ ] A) Creates categories with custom ranges
- [ ] B) Creates equal-sized groups based on quantiles
- [ ] C) Removes categories
- [ ] D) Counts categories

> [!success]- Answer
> **Correct Answer: B) Creates equal-sized groups based on quantiles**
> 
> `pd.qcut()` divides data into quantile-based bins with approximately equal number of observations per bin.

**Q22.** What does `pd.cut()` do?

- [ ] A) Creates equal-sized groups
- [ ] B) Creates categories based on custom value ranges
- [ ] C) Removes data
- [ ] D) Sorts data

> [!success]- Answer
> **Correct Answer: B) Creates categories based on custom value ranges**
> 
> `pd.cut()` divides data into bins based on specific value boundaries you define.

**Q23.** When should you use `pd.qcut()` vs `pd.cut()`?

- [ ] A) qcut for equal groups, cut for custom ranges
- [ ] B) cut for equal groups, qcut for custom ranges
- [ ] C) They're the same
- [ ] D) Always use cut

> [!success]- Answer
> **Correct Answer: A) qcut for equal groups, cut for custom ranges**
> 
> - `qcut()`: Equal number of observations per bin (quantile-based)
> - `cut()`: Specific value ranges (value-based)

**Q24.** In `pd.qcut(data, q=3, labels=['Low', 'Med', 'High'])`, what does `q=3` mean?

- [ ] A) 3 quantiles
- [ ] B) 3 equal-sized groups
- [ ] C) 3 value ranges
- [ ] D) 3 minimum values

> [!success]- Answer
> **Correct Answer: B) 3 equal-sized groups**
> 
> `q=3` divides data into 3 bins with approximately equal number of observations in each.

**Q25.** In `pd.cut(data, bins=[0, 100, 200, np.inf])`, how many categories are created?

- [ ] A) 2
- [ ] B) 3
- [ ] C) 4
- [ ] D) 5

> [!success]- Answer
> **Correct Answer: B) 3**
> 
> 4 boundaries create 3 bins: [0-100), [100-200), [200-inf). Number of bins = number of boundaries - 1.

**Q26.** What does `.replace()` with a dictionary do?

- [ ] A) Removes values
- [ ] B) Maps old values to new values
- [ ] C) Sorts values
- [ ] D) Counts values

> [!success]- Answer
> **Correct Answer: B) Maps old values to new values**
> 
> `.replace(mapping_dict)` substitutes values according to dictionary: `{'old1': 'new1', 'old2': 'new2'}`

**Q27.** What does `np.inf` represent in bin ranges?

- [ ] A) Zero
- [ ] B) Infinity (no upper limit)
- [ ] C) Null
- [ ] D) Maximum value

> [!success]- Answer
> **Correct Answer: B) Infinity (no upper limit)**
> 
> `np.inf` represents infinity, used for open-ended upper bound: `[200, np.inf]` means "200 and above".

**Q28.** What parameter in `pd.cut()` specifies the category names?

- [ ] A) `names`
- [ ] B) `categories`
- [ ] C) `labels`
- [ ] D) `groups`

> [!success]- Answer
> **Correct Answer: C) `labels`**
> 
> Use `labels` parameter: `pd.cut(data, bins=[...], labels=['Low', 'Medium', 'High'])`

**Q29.** If you want to group 5 operating systems into 2 categories (Desktop/Mobile), which method is best?

- [ ] A) `pd.qcut()`
- [ ] B) `pd.cut()`
- [ ] C) `.replace()` with mapping dictionary
- [ ] D) `.astype()`

> [!success]- Answer
> **Correct Answer: C) `.replace()` with mapping dictionary**
> 
> Use `.replace()` to map existing categories to new ones: `{'Windows': 'Desktop', 'Mac': 'Desktop', 'iOS': 'Mobile', ...}`

**Q30.** After using `.replace()` with a mapping, what method shows the new unique categories?

- [ ] A) `.categories()`
- [ ] B) `.unique()`
- [ ] C) `.distinct()`
- [ ] D) `.values()`

> [!success]- Answer
> **Correct Answer: B) `.unique()`**
> 
> `.unique()` returns array of unique values: `df['col'].unique()` shows all distinct categories.

---

## Section B: True/False Questions (15 points)
*1 point each | 15 questions total*

**Q31.** Membership constraints mean data must come from a predefined set of categories. **T/F**

> [!success]- Answer
> **True** - Categories are a finite set of valid values. For example, blood type must be one of: O-, O+, A-, A+, B+, B-, AB+, AB-.

**Q32.** The `.difference()` method finds values that exist in both sets. **T/F**

> [!success]- Answer
> **False** - `.difference()` finds values in the first set but NOT in the second. `.intersection()` finds values in both.

**Q33.** The `~` operator in pandas inverts boolean values. **T/F**

> [!success]- Answer
> **True** - `~` is the NOT operator: `~True` becomes `False`, `~False` becomes `True`.

**Q34.** An anti join returns only rows that match in both tables. **T/F**

> [!success]- Answer
> **False** - Anti join returns rows from left table with NO match in right table. Inner join returns matching rows.

**Q35.** `.str.upper()` converts strings to uppercase. **T/F**

> [!success]- Answer
> **True** - `.str.upper()` converts all characters to uppercase: 'married' → 'MARRIED'.

**Q36.** `.str.strip()` removes spaces from the middle of strings. **T/F**

> [!success]- Answer
> **False** - `.str.strip()` removes spaces only from the beginning and end, not the middle.

**Q37.** 'married' and 'married ' (with trailing space) are counted as the same value. **T/F**

> [!success]- Answer
> **False** - They're different strings. Trailing space makes them distinct. Use `.str.strip()` to fix this.

**Q38.** `.value_counts()` shows the frequency of each unique value. **T/F**

> [!success]- Answer
> **True** - `.value_counts()` returns count of each unique value in descending order.

**Q39.** `pd.qcut()` creates bins based on specific value ranges you define. **T/F**

> [!success]- Answer
> **False** - `pd.qcut()` creates equal-sized bins based on quantiles. `pd.cut()` uses specific value ranges.

**Q40.** `pd.cut()` creates equal-sized groups. **T/F**

> [!success]- Answer
> **False** - `pd.cut()` creates bins based on value ranges (can be uneven). `pd.qcut()` creates equal-sized groups.

**Q41.** In `pd.cut(data, bins=[0, 100, 200])`, the number of categories created is 2. **T/F**

> [!success]- Answer
> **True** - 3 boundaries create 2 bins: [0-100) and [100-200). Number of bins = boundaries - 1.

**Q42.** `.replace()` with a dictionary can map multiple old values to new values. **T/F**

> [!success]- Answer
> **True** - `.replace(mapping_dict)` maps according to dictionary: `{'old1': 'new1', 'old2': 'new2', ...}`

**Q43.** `np.inf` represents zero in pandas. **T/F**

> [!success]- Answer
> **False** - `np.inf` represents infinity (unlimited upper bound). Used for open-ended ranges like "200+".

**Q44.** The `labels` parameter in `pd.cut()` specifies the names for categories. **T/F**

> [!success]- Answer
> **True** - `labels` parameter assigns names to bins: `pd.cut(data, bins=[...], labels=['Low', 'High'])`

**Q45.** `.unique()` returns an array of all distinct values in a column. **T/F**

> [!success]- Answer
> **True** - `.unique()` returns unique values: `df['col'].unique()` shows all distinct categories.

---

## Section C: Short Answer Questions (30 points)
*3 points each | 10 questions total*

**Q46.** Explain what membership constraints are and why they're important in categorical data.

> [!success]- Answer
> **Membership Constraints:**
> Data values must come from a **predefined finite set** of valid categories.
> 
> **Example:**
> Marriage status can ONLY be:
> - 'unmarried' (value 0)
> - 'married' (value 1)
> 
> Any other value (like 'divorced', 'separated', or typos like 'maried') violates the constraint.
> 
> **Why Important:**
> 1. **Data integrity** - Ensures data quality
> 2. **Analysis accuracy** - Prevents invalid values from affecting results
> 3. **Consistency** - Maintains standardized categories
> 4. **Downstream processing** - Machine learning models expect specific categories
> 
> **Real-world impact:** A blood type of 'Z+' could lead to medical errors; an invalid country code could break geographic analysis.

**Q47.** Describe how to use set difference to find inconsistent categories. Include code example.

> [!success]- Answer
> **Method: Set Difference**
> 
> Set difference finds values in your data that are NOT in valid categories.
> 
> **Complete workflow:**
> ```python
> # Step 1: Define valid categories
> valid_categories = ['O-', 'O+', 'A-', 'A+', 'B+', 'B-', 'AB+', 'AB-']
> 
> # Step 2: Find inconsistent categories
> inconsistent = set(study_data['blood_type']).difference(valid_categories)
> print(inconsistent)  # Output: {'Z+'}
> 
> # Step 3: Get rows with inconsistent values
> inconsistent_rows = study_data['blood_type'].isin(inconsistent)
> problem_data = study_data[inconsistent_rows]
> 
> # Step 4: Drop inconsistent rows
> clean_data = study_data[~inconsistent_rows]
> ```
> 
> **What each step does:**
> - `set()` converts to set for set operations
> - `.difference()` finds values in first set but not in second
> - `.isin()` creates boolean mask
> - `~` inverts mask to keep valid rows only

**Q48.** Explain the difference between an anti join and an inner join. When would you use each for category validation?

> [!success]- Answer
> **Anti Join (Left Anti Join):**
> Returns rows from left table that DON'T have matches in right table.
> 
> ```
> Data: ['A', 'B', 'Z']    Valid: ['A', 'B', 'C']
> Anti Join Result: ['Z']  (in data, NOT in valid)
> ```
> 
> **Use for:** Finding INVALID categories
> 
> **Inner Join:**
> Returns only rows that exist in BOTH tables.
> 
> ```
> Data: ['A', 'B', 'Z']    Valid: ['A', 'B', 'C']
> Inner Join Result: ['A', 'B']  (in BOTH)
> ```
> 
> **Use for:** Keeping only VALID categories
> 
> **Category validation workflow:**
> 1. **Use anti join** to identify what's wrong:
>    ```python
>    set(data['col']).difference(valid['col'])  # Find invalid
>    ```
> 
> 2. **Use inner join** to keep only valid:
>    ```python
>    data[data['col'].isin(valid['col'])]  # Keep only valid
>    ```
> 
> **Example:**
> - Anti join: "Show me blood types that aren't valid" → Z+
> - Inner join: "Keep only valid blood types" → O+, A-, B+, etc.

**Q49.** Describe the three types of categorical variable problems and provide examples of each.

> [!success]- Answer
> **I. Value Inconsistency**
> Same category written in different ways.
> 
> **Examples:**
> - **Capitalization:** 'married', 'Married', 'MARRIED', 'unmarried', 'UNMARRIED'
> - **Trailing spaces:** 'married', 'married ', ' married '
> 
> **Solution:**
> ```python
> df['col'] = df['col'].str.strip().str.lower()
> ```
> 
> **II. Collapsing Too Many/Few Categories**
> Creating or reducing category groups.
> 
> **Examples:**
> - Creating ranges from continuous data: Income $45,000 → '20K-50K'
> - Mapping to broader groups: 'Windows', 'Mac', 'Linux' → 'Desktop'
> 
> **Solution:**
> ```python
> # Create groups
> pd.qcut(df['income'], q=3, labels=['Low', 'Med', 'High'])
> 
> # Map to fewer
> df['os'].replace({'Windows': 'Desktop', 'iOS': 'Mobile'})
> ```
> 
> **III. Wrong Data Type**
> Categories stored as wrong type (covered in Chapter 1).
> 
> **Example:**
> Marriage status as integers (0, 1, 2) instead of category type.
> 
> **Solution:**
> ```python
> df['col'] = df['col'].astype('category')
> ```

**Q50.** Compare and contrast `pd.qcut()` and `pd.cut()`. When should you use each?

> [!success]- Answer
> **`pd.qcut()` - Quantile-based binning**
> 
> **Purpose:** Create equal-sized groups
> **How:** Divides by quantiles (percentiles)
> **Result:** Approximately same number of observations per bin
> 
> ```python
> pd.qcut(df['income'], q=3, labels=['Low', 'Medium', 'High'])
> # Creates 3 groups with ~33% of data each
> ```
> 
> **`pd.cut()` - Value-based binning**
> 
> **Purpose:** Create specific value ranges
> **How:** Divides by explicit boundaries
> **Result:** Can have uneven distribution
> 
> ```python
> pd.cut(df['income'], bins=[0, 50000, 100000, np.inf],
>        labels=['Low', 'Medium', 'High'])
> # Creates groups: 0-50K, 50K-100K, 100K+
> ```
> 
> **Comparison:**
> 
> | Aspect | qcut() | cut() |
> |--------|--------|-------|
> | **Bin size** | Equal observations | Can be uneven |
> | **Boundaries** | Automatic (quantiles) | Manual (you specify) |
> | **Use when** | Want balanced groups | Have specific ranges |
> | **Example** | Divide customers into thirds | Income brackets: 0-20K, 20-50K, 50K+ |
> 
> **When to use:**
> - **qcut():** Analysis requiring equal sample sizes (A/B testing groups)
> - **cut():** Business rules with specific thresholds (tax brackets, age groups)

**Q51.** Explain how to standardize inconsistent capitalization and remove trailing spaces in categorical data.

> [!success]- Answer
> **Problem:** Same category written differently
> ```
> 'married', 'Married', 'MARRIED', ' married ', 'married '
> ```
> 
> **Solution: Two-step process**
> 
> **Step 1: Standardize capitalization**
> ```python
> # Option A: Uppercase
> df['marriage_status'] = df['marriage_status'].str.upper()
> # Result: All become 'MARRIED'
> 
> # Option B: Lowercase (recommended)
> df['marriage_status'] = df['marriage_status'].str.lower()
> # Result: All become 'married'
> ```
> 
> **Step 2: Remove spaces**
> ```python
> df['marriage_status'] = df['marriage_status'].str.strip()
> # Removes leading and trailing spaces
> ```
> 
> **Combined approach:**
> ```python
> df['marriage_status'] = df['marriage_status'].str.strip().str.lower()
> ```
> 
> **Verification:**
> ```python
> # Check unique values
> print(df['marriage_status'].value_counts())
> # Output:
> # married      472
> # unmarried    528
> ```
> 
> **Best practices:**
> 1. Choose one case standard (upper or lower) and use consistently
> 2. Always strip spaces first or along with case conversion
> 3. Use `.value_counts()` before and after to verify consolidation
> 4. Document which standard you chose for your project

**Q52.** Describe how to use `.replace()` with a mapping dictionary to collapse categories. Provide a complete example.

> [!success]- Answer
> **Purpose:** Map multiple existing categories to fewer new categories
> 
> **Complete example: Operating Systems**
> 
> **Before:** 5 categories
> ```
> 'Microsoft', 'MacOS', 'Linux', 'IOS', 'Android'
> ```
> 
> **After:** 2 categories
> ```
> 'DesktopOS', 'MobileOS'
> ```
> 
> **Implementation:**
> ```python
> # Step 1: Create mapping dictionary
> mapping = {
>     'Microsoft': 'DesktopOS',
>     'MacOS': 'DesktopOS',
>     'Linux': 'DesktopOS',
>     'IOS': 'MobileOS',
>     'Android': 'MobileOS'
> }
> 
> # Step 2: Apply mapping
> devices['operating_system'] = devices['operating_system'].replace(mapping)
> 
> # Step 3: Verify results
> print(devices['operating_system'].unique())
> # Output: array(['DesktopOS', 'MobileOS'], dtype=object)
> 
> # Step 4: Check counts
> print(devices['operating_system'].value_counts())
> # Output:
> # DesktopOS    450
> # MobileOS     350
> ```
> 
> **How it works:**
> - Dictionary key = old value
> - Dictionary value = new value
> - `.replace()` substitutes all occurrences
> 
> **Another example: Income levels**
> ```python
> mapping = {
>     '0-20K': 'Low Income',
>     '20-50K': 'Low Income',
>     '50-100K': 'Middle Income',
>     '100K+': 'High Income'
> }
> df['income_level'] = df['income_bracket'].replace(mapping)
> ```

**Q53.** What are common text data problems and how do you fix them? Use the phone number example.

> [!success]- Answer
> **Three common text data problems:**
> 
> 1. **Data inconsistency** - Different formats
> 2. **Fixed length violations** - Too short/long
> 3. **Typos** - Invalid characters
> 
> **Example: Phone Numbers**
> 
> **Original problems:**
> ```
> '001-702-397-5143'  ← Dashes
> '+1-297-996-4904'   ← Plus sign, dashes
> '4138'              ← Too short
> ```
> 
> **Solution workflow:**
> 
> ```python
> # Step 1: Fix inconsistent formats
> # Replace "+" with "00"
> phones['Phone number'] = phones['Phone number'].str.replace('+', '00')
> 
> # Remove dashes
> phones['Phone number'] = phones['Phone number'].str.replace('-', '')
> 
> # Step 2: Handle length violations
> # Get length of each phone number
> digits = phones['Phone number'].str.len()
> 
> # Set too-short numbers to NaN
> phones.loc[digits < 10, 'Phone number'] = np.nan
> 
> # Step 3: Verify with assertions
> sanity_check = phones['Phone number'].str.len()
> 
> # Assert minimum length
> assert sanity_check.min() >= 10
> 
> # Assert no special characters remain
> assert phones['Phone number'].str.contains('+|-').any() == False
> ```
> 
> **Result:**
> ```
> '0017023975143'  ← Clean!
> '0012979964904'  ← Clean!
> NaN              ← Too short, set to missing
> ```
> 
> **Key methods:**
> - `.str.replace()` - Fix inconsistencies
> - `.str.len()` - Check length
> - `.loc[]` with condition - Handle violations
> - `assert` - Verify cleaning worked

**Q54.** Explain what regular expressions (regex) are and how they're used for text cleaning. Include the `\D+` example.

> [!success]- Answer
> **Regular Expressions (Regex):**
> Pattern matching tool for finding and manipulating text - like "supercharged Control + F"
> 
> **Why use regex:**
> When data has many different formats that can't be fixed with simple `.replace()`
> 
> **Problem example:**
> ```
> '+(01706)-25891'
> '+0500-571437'
> '+0800-1111'
> '+(016977)-8424'
> ```
> Many different patterns: +, (, ), -, varying lengths
> 
> **Regex solution:**
> ```python
> # Remove ALL non-digit characters
> phones['Phone number'] = phones['Phone number'].str.replace(r'\D+', '')
> ```
> 
> **Result:**
> ```
> '0170625891'
> '0500571437'
> '08001111'
> '0169778424'
> ```
> 
> **Understanding `r'\D+'`:**
> 
> | Part | Meaning |
> |------|---------|
> | `r` | Raw string (treats backslash literally) |
> | `\D` | Any NON-digit character (not 0-9) |
> | `+` | One or more occurrences |
> | `''` | Replace with nothing (empty string) |
> 
> **What it matches:** Any character that's NOT a digit: +, -, (, ), spaces, letters, etc.
> 
> **Common regex patterns:**
> - `\d` - Any digit (0-9)
> - `\D` - Any NON-digit
> - `\s` - Whitespace (space, tab, newline)
> - `\w` - Word character (letter, digit, underscore)
> - `+` - One or more
> - `*` - Zero or more
> 
> **Why powerful:** One line replaces multiple `.replace()` calls!

**Q55.** Write a complete workflow for cleaning a categorical column with inconsistent values and invalid categories.

> [!success]- Answer
> **Complete cleaning workflow:**
> 
> ```python
> import pandas as pd
> import numpy as np
> 
> # Step 1: Load data and inspect
> df = pd.read_csv('data.csv')
> print(df['blood_type'].value_counts())
> # Output shows: 'O+', 'o+', ' A- ', 'Z+', etc.
> 
> # Step 2: Standardize format (case and spaces)
> df['blood_type'] = df['blood_type'].str.strip().str.upper()
> print(df['blood_type'].value_counts())
> # Now: 'O+', 'A-', 'Z+', etc.
> 
> # Step 3: Define valid categories
> valid_categories = ['O-', 'O+', 'A-', 'A+', 'B+', 'B-', 'AB+', 'AB-']
> 
> # Step 4: Find inconsistent categories
> inconsistent = set(df['blood_type']).difference(valid_categories)
> print(f"Invalid categories: {inconsistent}")
> # Output: {'Z+'}
> 
> # Step 5: Get rows with invalid categories
> inconsistent_rows = df['blood_type'].isin(inconsistent)
> print(f"Found {inconsistent_rows.sum()} invalid rows")
> 
> # Step 6: Decide how to handle invalids
> # Option A: Drop invalid rows
> df_clean = df[~inconsistent_rows]
> 
> # Option B: Set to NaN for later imputation
> df.loc[inconsistent_rows, 'blood_type'] = np.nan
> 
> # Step 7: Convert to category type
> df_clean['blood_type'] = df_clean['blood_type'].astype('category')
> 
> # Step 8: Verify cleaning
> print(df_clean['blood_type'].value_counts())
> assert df_clean['blood_type'].isin(valid_categories).all()
> print("✓ All categories valid!")
> ```
> 
> **Summary checklist:**
> - ✅ Inspect data with `.value_counts()`
> - ✅ Standardize format (`.strip()`, `.upper()`/`.lower()`)
> - ✅ Define valid categories
> - ✅ Find invalids with set `.difference()`
> - ✅ Handle invalids (drop or set to NaN)
> - ✅ Convert to category type
> - ✅ Verify with assert

---

## Section D: Code Analysis & Scenarios (25 points)
*2.5 points each | 10 questions total*

**Q56.** What does this code do?

```python
inconsistent = set(study_data['blood_type']).difference(categories['blood_type'])
print(inconsistent)
```

> [!success]- Answer
> **This code finds invalid blood types in the data.**
> 
> **Step-by-step:**
> 1. `set(study_data['blood_type'])` - Get all unique blood types in actual data
> 2. `categories['blood_type']` - List of valid blood types
> 3. `.difference()` - Find values in data but NOT in valid categories
> 4. `print(inconsistent)` - Display the invalid values
> 
> **Example output:** `{'Z+'}` - Z+ is in data but not valid
> 
> **Use case:** Identify membership constraint violations before deciding how to handle them.

**Q57.** Complete this code to drop rows with invalid categories:

```python
inconsistent_rows = study_data['blood_type'].isin(inconsistent)
clean_data = study_data[________]
```

> [!success]- Answer
> **Completed code:**
> 
> ```python
> inconsistent_rows = study_data['blood_type'].isin(inconsistent)
> clean_data = study_data[~inconsistent_rows]
> ```
> 
> **Explanation:**
> - `inconsistent_rows` is a boolean Series: True for invalid rows
> - `~inconsistent_rows` inverts it: True for VALID rows
> - `study_data[~inconsistent_rows]` keeps only valid rows
> 
> **Alternative approach:**
> ```python
> consistent_rows = ~study_data['blood_type'].isin(inconsistent)
> clean_data = study_data[consistent_rows]
> ```

**Q58.** What does this code do?

```python
demographics['marriage_status'] = demographics['marriage_status'].str.strip().str.lower()
```

> [!success]- Answer
> **This code standardizes marriage status by removing spaces and converting to lowercase.**
> 
> **What happens:**
> 
> **Before:**
> ```
> 'married'
> 'Married'
> 'MARRIED'
> ' married '
> 'married '
> ```
> 
> **After `.str.strip()`:**
> ```
> 'married'
> 'Married'
> 'MARRIED'
> 'married'
> 'married'
> ```
> 
> **After `.str.lower()`:**
> ```
> 'married'  ← All standardized!
> 'married'
> 'married'
> 'married'
> 'married'
> ```
> 
> **Result:** All variations become 'married' - ready for proper counting/analysis.
> 
> **Method chaining:** `.str.strip().str.lower()` applies operations sequentially.

**Q59.** Complete this code to create 3 income groups using quantiles:

```python
group_names = ['Low', 'Medium', 'High']
df['income_group'] = pd.______(df['household_income'], q=____, labels=______)
```

> [!success]- Answer
> **Completed code:**
> 
> ```python
> group_names = ['Low', 'Medium', 'High']
> df['income_group'] = pd.qcut(df['household_income'], q=3, labels=group_names)
> ```
> 
> **Explanation:**
> - `pd.qcut()` - Quantile-based binning
> - `q=3` - Create 3 equal-sized groups
> - `labels=group_names` - Name the groups 'Low', 'Medium', 'High'
> 
> **Result:** Each group contains ~33% of observations, divided by income percentiles.

**Q60.** Complete this code to create custom income ranges:

```python
ranges = [0, 50000, 100000, ______]
labels = ['Low', 'Medium', 'High']
df['income_group'] = pd.______(df['household_income'], bins=______, labels=______)
```

> [!success]- Answer
> **Completed code:**
> 
> ```python
> ranges = [0, 50000, 100000, np.inf]
> labels = ['Low', 'Medium', 'High']
> df['income_group'] = pd.cut(df['household_income'], bins=ranges, labels=labels)
> ```
> 
> **Explanation:**
> - First blank: `np.inf` - Infinity (no upper limit)
> - Second blank: `cut` - Value-based binning
> - Third blank: `ranges` - Bin boundaries
> - Fourth blank: `labels` - Category names
> 
> **Bins created:**
> - Low: $0 - $50,000
> - Medium: $50,000 - $100,000
> - High: $100,000+

**Q61.** What does this mapping code do?

```python
mapping = {'Microsoft': 'DesktopOS', 'MacOS': 'DesktopOS', 'IOS': 'MobileOS'}
devices['os'] = devices['os'].replace(mapping)
```

> [!success]- Answer
> **This code collapses 3 operating system categories into 2 broader categories.**
> 
> **Transformation:**
> ```
> Before:               After:
> 'Microsoft'    →     'DesktopOS'
> 'MacOS'        →     'DesktopOS'
> 'IOS'          →     'MobileOS'
> ```
> 
> **How it works:**
> 1. `mapping` dictionary defines old → new value pairs
> 2. `.replace(mapping)` substitutes all occurrences
> 3. Multiple values can map to same new value
> 
> **Result:** 3 specific OS types reduced to 2 platform types for broader analysis.
> 
> **Verification:**
> ```python
> print(devices['os'].unique())
> # Output: array(['DesktopOS', 'MobileOS'], dtype=object)
> ```

**Q62.** Fix all issues in this code:

```python
df['status'] = df['status'].upper()
df['status'] = df['status'].strip()
```

> [!success]- Answer
> **Issues identified:**
> 1. Missing `.str` accessor before string methods
> 2. Won't work on pandas Series
> 
> **Fixed code:**
> 
> ```python
> df['status'] = df['status'].str.upper()
> df['status'] = df['status'].str.strip()
> ```
> 
> **Or combined:**
> ```python
> df['status'] = df['status'].str.strip().str.upper()
> ```
> 
> **Why the fix:**
> - Pandas string methods require `.str` accessor
> - `.upper()` alone is for Python strings, not Series
> - `.str.upper()` is the pandas method for Series
> 
> **Remember:** Always use `.str` before string methods on pandas columns!

**Q63.** What does this phone cleaning code do?

```python
phones['Phone number'] = phones['Phone number'].str.replace('+', '00')
phones['Phone number'] = phones['Phone number'].str.replace('-', '')
```

> [!success]- Answer
> **This code standardizes phone number format by replacing characters.**
> 
> **Step-by-step transformation:**
> 
> **Original:**
> ```
> '+1-297-996-4904'
> ```
> 
> **After first replace (+ → 00):**
> ```
> '001-297-996-4904'
> ```
> 
> **After second replace (- → empty):**
> ```
> '0012979964904'
> ```
> 
> **What each line does:**
> 1. `.str.replace('+', '00')` - Replaces plus sign with "00"
> 2. `.str.replace('-', '')` - Removes all dashes
> 
> **Result:** Consistent format with only digits, ready for length validation.

**Q64.** Complete this code to set short phone numbers to NaN:

```python
digits = phones['Phone number'].______.____()
phones.____[digits < 10, ______] = np.nan
```

> [!success]- Answer
> **Completed code:**
> 
> ```python
> digits = phones['Phone number'].str.len()
> phones.loc[digits < 10, 'Phone number'] = np.nan
> ```
> 
> **Explanation:**
> - First blank: `str` - String accessor
> - Second blank: `len()` - Get length of each string
> - Third blank: `loc` - Label-based indexer
> - Fourth blank: `'Phone number'` - Column to modify
> 
> **What it does:**
> 1. Get length of each phone number
> 2. Find rows where length < 10
> 3. Set those values to NaN (missing)
> 
> **Example:**
> ```
> '0017023975143'  → 13 digits → Keep
> '4138'           → 4 digits  → Set to NaN
> ```

**Q65.** What does this regex code do?

```python
phones['Phone number'] = phones['Phone number'].str.replace(r'\D+', '')
```

> [!success]- Answer
> **This code removes ALL non-digit characters from phone numbers.**
> 
> **Regex breakdown:**
> - `r` - Raw string (treats \ literally)
> - `\D` - Matches any NON-digit character
> - `+` - One or more occurrences
> - `''` - Replace with empty string (remove)
> 
> **What gets removed:** Everything except 0-9
> - Plus signs (+)
> - Dashes (-)
> - Parentheses (, )
> - Spaces
> - Letters
> - Any other non-digit character
> 
> **Example transformations:**
> ```
> Before:                After:
> '+(01706)-25891'   →   '0170625891'
> '+0500-571437'     →   '0500571437'
> '(555) 123-4567'   →   '5551234567'
> 'ABC-123'          →   '123'
> ```
> 
> **Why powerful:** One regex line replaces many `.replace()` calls!
> 
> **Equivalent without regex:**
> ```python
> # Would need many lines:
> phones['Phone number'].str.replace('+', '')
> phones['Phone number'].str.replace('-', '')
> phones['Phone number'].str.replace('(', '')
> phones['Phone number'].str.replace(')', '')
> # ... and so on
> ```

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)
1. B | 2. B | 3. B | 4. A | 5. C | 6. A | 7. D | 8. B | 9. B | 10. B
2. B | 12. B | 13. B | 14. B | 15. B | 16. B | 17. B | 18. B | 19. C | 20. C
3. B | 22. B | 23. A | 24. B | 25. B | 26. B | 27. B | 28. C | 29. C | 30. B

### Section B: True/False (15 points)
31. T | 32. F | 33. T | 34. F | 35. T | 36. F | 37. F | 38. T | 39. F | 40. F
32. T | 42. T | 43. F | 44. T | 45. T

### Section C: Short Answer (30 points)
Questions 46-55 require detailed written answers. Award full points for complete, accurate responses with code examples.

### Section D: Code Analysis (25 points)
Questions 56-65 require code completion or analysis. Award full points for correct solutions with explanations.

---

## Quick Study Guide

### Finding Invalid Categories

```python
# Set difference method
invalid = set(df['col']).difference(valid_categories)

# Get rows with invalid values
invalid_rows = df['col'].isin(invalid)

# View invalid data
df[invalid_rows]

# Drop invalid rows
clean_df = df[~invalid_rows]
```

### Value Consistency

```python
# Standardize case
df['col'] = df['col'].str.upper()  # or .lower()

# Remove spaces
df['col'] = df['col'].str.strip()

# Combined
df['col'] = df['col'].str.strip().str.lower()

# Check results
df['col'].value_counts()
```

### Creating Categories

```python
# Equal-sized groups (quantiles)
pd.qcut(df['income'], q=3, labels=['Low', 'Med', 'High'])

# Custom ranges
pd.cut(df['income'], 
       bins=[0, 50000, 100000, np.inf],
       labels=['Low', 'Med', 'High'])

# Map to fewer categories
mapping = {'A': 'Group1', 'B': 'Group1', 'C': 'Group2'}
df['col'].replace(mapping)
```

### Text Cleaning

```python
# Replace characters
df['phone'].str.replace('+', '00')
df['phone'].str.replace('-', '')

# Regex - remove non-digits
df['phone'].str.replace(r'\D+', '')

# Check length
lengths = df['phone'].str.len()
df.loc[lengths < 10, 'phone'] = np.nan

# Verify with assert
assert df['phone'].str.len().min() >= 10
```

---

## Key Concepts Summary

### Membership Constraints

| Concept | Method | Purpose |
|---------|--------|---------|
| Find invalid | `set().difference()` | Values in data not in valid |
| Check membership | `.isin()` | Boolean mask for matching |
| Drop invalid | `df[~mask]` | Keep only valid rows |
| Joins | Anti, Inner | Find mismatches or matches |

### String Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `.str.upper()` | Uppercase | 'text' → 'TEXT' |
| `.str.lower()` | Lowercase | 'TEXT' → 'text' |
| `.str.strip()` | Remove spaces | ' text ' → 'text' |
| `.str.replace()` | Replace pattern | '+1' → '00' |
| `.str.len()` | String length | 'hello' → 5 |
| `.str.contains()` | Check pattern | Contains 'a' → True |

### Category Operations

| Operation | Method | Use Case |
|-----------|--------|----------|
| Equal groups | `pd.qcut()` | Balanced samples |
| Custom ranges | `pd.cut()` | Specific thresholds |
| Map categories | `.replace()` | Collapse groups |
| Check unique | `.unique()` | See distinct values |
| Count values | `.value_counts()` | Frequency table |

### Regex Patterns

| Pattern | Matches | Example |
|---------|---------|---------|
| `\d` | Any digit | 0-9 |
| `\D` | Non-digit | +, -, space |
| `\s` | Whitespace | space, tab |
| `\w` | Word character | a-z, A-Z, 0-9, _ |
| `+` | One or more | `\d+` = 1 or more digits |
| `*` | Zero or more | `\s*` = any spaces |

---

## Common Mistakes to Avoid

❌ **Forgetting .str accessor**
```python
# WRONG
df['col'].upper()

# CORRECT
df['col'].str.upper()
```

❌ **Not using ~ to invert boolean mask**
```python
# WRONG - keeps invalid rows
clean = df[invalid_rows]

# CORRECT - drops invalid rows
clean = df[~invalid_rows]
```

❌ **Confusing qcut and cut**
```python
# WRONG - qcut doesn't take bins
pd.qcut(df['col'], bins=[0, 100, 200])

# CORRECT
pd.cut(df['col'], bins=[0, 100, 200])
pd.qcut(df['col'], q=3)  # Use q for quantiles
```

❌ **Wrong number of labels for bins**
```python
# WRONG - 4 boundaries, 2 labels (need 3!)
pd.cut(df['col'], bins=[0, 50, 100, 150], labels=['Low', 'High'])

# CORRECT - 4 boundaries create 3 bins
pd.cut(df['col'], bins=[0, 50, 100, 150], labels=['Low', 'Med', 'High'])
```

❌ **Not standardizing before finding invalids**
```python
# WRONG - 'A+' and 'a+' treated as different
invalid = set(df['col']).difference(valid)

# CORRECT - standardize first
df['col'] = df['col'].str.strip().str.upper()
invalid = set(df['col']).difference(valid)
```

❌ **Forgetting raw string for regex**
```python
# WRONG - backslash interpreted
df['col'].str.replace('\D+', '')

# CORRECT - r prefix for raw string
df['col'].str.replace(r'\D+', '')
```

---

## Study Strategy (5-Day Plan)

### Day 1: Membership Constraints
- [ ] Understand predefined categories
- [ ] Learn set `.difference()` method
- [ ] Practice `.isin()` for filtering
- [ ] Master `~` (NOT) operator
- [ ] Understand anti join vs inner join
- **Practice:** Find and drop invalid categories

### Day 2: Value Consistency
- [ ] Learn `.str.upper()` and `.str.lower()`
- [ ] Master `.str.strip()` for spaces
- [ ] Practice `.value_counts()` for inspection
- [ ] Understand method chaining
- **Practice:** Clean inconsistent categories

### Day 3: Collapsing Categories
- [ ] Understand `pd.qcut()` for equal groups
- [ ] Learn `pd.cut()` for custom ranges
- [ ] Know when to use each
- [ ] Master `.replace()` with dictionaries
- [ ] Use `.unique()` to verify
- **Practice:** Create income brackets, map OS types

### Day 4: Text Cleaning
- [ ] Learn string `.replace()` method
- [ ] Practice `.str.len()` for validation
- [ ] Use `.loc[]` for conditional assignment
- [ ] Master assert for verification
- **Practice:** Clean phone numbers

### Day 5: Regex & Review
- [ ] Understand regex basics
- [ ] Learn `\D`, `\d`, `+`, `*` patterns
- [ ] Practice `r'\D+'` for removing non-digits
- [ ] Combine all techniques
- **Practice:** Complete workflows, take quiz

---

## Final Exam Checklist

### Before the Exam ✓

**Membership:**
- [ ] Know what membership constraints are
- [ ] Can use set `.difference()`
- [ ] Understand `.isin()` method
- [ ] Know `~` inverts boolean
- [ ] Understand anti join concept

**Consistency:**
- [ ] Know `.str.upper()` and `.str.lower()`
- [ ] Can use `.str.strip()` for spaces
- [ ] Understand `.value_counts()`
- [ ] Can chain methods

**Categories:**
- [ ] Know `pd.qcut()` = equal groups
- [ ] Know `pd.cut()` = custom ranges
- [ ] Can use `.replace()` with dictionary
- [ ] Understand bins vs labels count

**Text:**
- [ ] Know `.str.replace()` syntax
- [ ] Can use `.str.len()` for validation
- [ ] Understand regex basics
- [ ] Know `r'\D+'` removes non-digits

### During the Exam ✓

**Common Traps:**
- [ ] Don't forget `.str` accessor
- [ ] Use `~` to drop invalid (not keep)
- [ ] qcut uses `q=`, cut uses `bins=`
- [ ] Number of bins = boundaries - 1
- [ ] Regex needs `r` prefix
- [ ] `.isin()` checks membership

**Quick Checks:**
- [ ] Invalid categories: `set().difference()`
- [ ] Keep valid: `df[~invalid_rows]`
- [ ] Standardize: `.str.strip().str.lower()`
- [ ] Equal groups: `pd.qcut(q=n)`
- [ ] Custom ranges: `pd.cut(bins=[...])`
- [ ] Map categories: `.replace(dict)`
- [ ] Remove non-digits: `r'\D+'`

---

## Congratulations! 🎉

You've mastered:
- ✅ Finding and handling invalid categories
- ✅ Standardizing inconsistent values
- ✅ Creating and collapsing categories
- ✅ Cleaning text data
- ✅ Using regex for pattern matching

**You're ready to tackle categorical and text data problems!** 📊🐍

---

#datacamp #data-cleaning #python #pandas #categorical-data #text-cleaning #regex #membership-constraints #certification

