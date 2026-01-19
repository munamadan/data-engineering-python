# Cleaning Data in Python - Final Comprehensive Exam

**Course:** DataCamp - Cleaning Data in Python  
**Chapters Covered:** 1, 2, 3, 4 (Complete Course)  
**Total Points:** 200  
**Pass Score:** 140+ (70%)  
**Time Limit:** 180 minutes  

---

## Exam Structure

| Section | Points | Questions | Topics |
|---------|--------|-----------|--------|
| **A: Multiple Choice** | 90 | 45 | All chapters |
| **B: True/False** | 20 | 20 | All chapters |
| **C: Short Answer** | 45 | 15 | All chapters |
| **D: Code Analysis** | 45 | 18 | All chapters |
| **Total** | **200** | **98** | Chapters 1-4 |

---

## Section A: Multiple Choice Questions (90 points)
*2 points each | 45 questions total*

### Chapter 1: Common Data Problems (12 questions)

**Q1.** What does the principle "Garbage in, Garbage out" mean?

- [ ] A) Dirty data should be deleted
- [x] B) If you input dirty data, you'll get unreliable results
- [ ] C) Data cleaning removes all garbage
- [ ] D) Only clean data can be stored

> [!success]- Answer
> **Correct Answer: B) If you input dirty data, you'll get unreliable results**
> 
> The principle means that poor quality input leads to poor quality output. Data cleaning is essential for reliable analysis.

**Q2.** What pandas data type should be used for categories like marriage status or gender?

- [ ] A) str
- [ ] B) int
- [x] C) category
- [ ] D) object

> [!success]- Answer
> **Correct Answer: C) category**
> 
> Categorical data should be stored as `category` type for appropriate statistical summaries, not as strings or integers.

**Q3.** What does the `object` data type typically indicate in pandas?

- [ ] A) Numeric data
- [x] B) String data
- [ ] C) Boolean data
- [ ] D) Missing data

> [!success]- Answer
> **Correct Answer: B) String data**
> 
> In pandas, `object` dtype typically means the column contains string data.

**Q4.** What method converts a column to a different data type?

- [ ] A) `.convert()`
- [ ] B) `.change_type()`
- [x] C) `.astype()`
- [ ] D) `.to_type()`

> [!success]- Answer
> **Correct Answer: C) `.astype()`**
> 
> Use `.astype('int')`, `.astype('category')`, etc. to convert data types.

**Q5.** What happens when you sum a column stored as strings (e.g., "100$", "200$")?

- [ ] A) Returns the mathematical sum
- [ ] B) Returns an error
- [x] C) Concatenates the strings
- [ ] D) Converts automatically to numbers

> [!success]- Answer
> **Correct Answer: C) Concatenates the strings**
> 
> String addition concatenates: "100$" + "200$" = "100$200$" instead of 300.

**Q6.** What does an assert statement do if the condition is False?

- [ ] A) Prints a warning
- [x] B) Raises an AssertionError
- [ ] C) Returns False
- [ ] D) Continues execution

> [!success]- Answer
> **Correct Answer: B) Raises an AssertionError**
> 
> If the condition is False, assert raises an AssertionError and stops execution. If True, it passes silently.

**Q7.** Which is NOT a valid way to handle out of range data?

- [ ] A) Drop the data
- [ ] B) Cap at maximum value
- [x] C) Ignore it completely
- [ ] D) Treat as missing and impute

> [!success]- Answer
> **Correct Answer: C) Ignore it completely**
> 
> Out of range data must be handled - never ignored. Valid approaches include dropping, capping, or imputing.

**Q8.** What does `inplace=True` do?

- [ ] A) Creates a copy
- [x] B) Modifies the DataFrame directly
- [ ] C) Returns a new DataFrame
- [ ] D) Saves to disk

> [!success]- Answer
> **Correct Answer: B) Modifies the DataFrame directly**
> 
> `inplace=True` makes changes directly to the DataFrame without creating a new object.

**Q9.** What method identifies duplicate rows?

- [ ] A) `.find_duplicates()`
- [x] B) `.duplicated()`
- [ ] C) `.is_duplicate()`
- [ ] D) `.duplicates()`

> [!success]- Answer
> **Correct Answer: B) `.duplicated()`**
> 
> `.duplicated()` returns a boolean Series indicating which rows are duplicates.

**Q10.** What does `keep='first'` do in `.duplicated()`?

- [ ] A) Marks all duplicates as True
- [x] B) Marks first occurrence as False, rest as True
- [ ] C) Keeps only the first row
- [ ] D) Deletes the first occurrence

> [!success]- Answer
> **Correct Answer: B) Marks first occurrence as False, rest as True**
> 
> `keep='first'` marks the first occurrence as False (not duplicate) and subsequent ones as True.

**Q11.** What does `.groupby().agg()` do for duplicate handling?

- [ ] A) Deletes duplicates
- [ ] B) Finds duplicates
- [x] C) Aggregates duplicate rows into one with summary statistics
- [ ] D) Counts duplicates

> [!success]- Answer
> **Correct Answer: C) Aggregates duplicate rows into one with summary statistics**
> 
> Groups duplicate rows and applies aggregation functions (max, mean, sum) to combine them.

**Q12.** What does `pd.to_datetime()` do?

- [ ] A) Creates current datetime
- [x] B) Converts strings to datetime objects
- [ ] C) Formats dates
- [ ] D) Extracts date from datetime

> [!success]- Answer
> **Correct Answer: B) Converts strings to datetime objects**
> 
> `pd.to_datetime()` converts string dates to pandas datetime objects for proper date operations.

---

### Chapter 2: Text & Categorical Data (11 questions)

**Q13.** What are membership constraints?

- [ ] A) Data must be numbers
- [x] B) Data must come from a predefined set of categories
- [ ] C) Data must be unique
- [ ] D) Data must be complete

> [!success]- Answer
> **Correct Answer: B) Data must come from a predefined set of categories**
> 
> Membership constraints mean values must be from a specific valid set (e.g., blood type can only be O+, O-, A+, A-, etc.).

**Q14.** Which method finds values in your data that are NOT in valid categories?

- [ ] A) `.intersection()`
- [x] B) `.difference()`
- [ ] C) `.union()`
- [ ] D) `.compare()`

> [!success]- Answer
> **Correct Answer: B) `.difference()`**
> 
> `set(data['col']).difference(valid['col'])` returns values in data but not in valid categories.

**Q15.** What does the `~` operator do?

- [ ] A) Adds values
- [x] B) Inverts boolean values (NOT operator)
- [ ] C) Multiplies values
- [ ] D) Divides values

> [!success]- Answer
> **Correct Answer: B) Inverts boolean values (NOT operator)**
> 
> `~` flips True to False and False to True, used like: `df[~invalid_rows]`

**Q16.** What method converts strings to uppercase?

- [ ] A) `.upper()`
- [x] B) `.str.upper()`
- [ ] C) `.to_upper()`
- [ ] D) `.uppercase()`

> [!success]- Answer
> **Correct Answer: B) `.str.upper()`**
> 
> Pandas string methods require `.str` accessor: `df['col'].str.upper()`

**Q17.** What method removes leading and trailing whitespace?

- [ ] A) `.trim()`
- [x] B) `.str.strip()`
- [ ] C) `.clean()`
- [ ] D) `.remove_spaces()`

> [!success]- Answer
> **Correct Answer: B) `.str.strip()`**
> 
> `.str.strip()` removes spaces from both ends of strings.

**Q18.** What does `pd.qcut()` do?

- [ ] A) Creates custom value ranges
- [x] B) Creates equal-sized groups based on quantiles
- [ ] C) Cuts strings
- [ ] D) Removes outliers

> [!success]- Answer
> **Correct Answer: B) Creates equal-sized groups based on quantiles**
> 
> `pd.qcut()` divides data into quantile-based bins with approximately equal observations per bin.

**Q19.** What does `pd.cut()` do?

- [ ] A) Creates equal-sized groups
- [x] B) Creates categories based on custom value ranges
- [ ] C) Removes data
- [ ] D) Splits strings

> [!success]- Answer
> **Correct Answer: B) Creates categories based on custom value ranges**
> 
> `pd.cut()` creates bins based on specific value boundaries you define.

**Q20.** When should you use `pd.qcut()` vs `pd.cut()`?

- [x] A) qcut for equal groups, cut for custom ranges
- [ ] B) cut for equal groups, qcut for custom ranges
- [ ] C) They're the same
- [ ] D) Always use cut

> [!success]- Answer
> **Correct Answer: A) qcut for equal groups, cut for custom ranges**
> 
> qcut creates equal number of observations per bin; cut uses specific value ranges.

**Q21.** What does `.replace()` with a dictionary do?

- [ ] A) Removes values
- [x] B) Maps old values to new values
- [ ] C) Sorts values
- [ ] D) Finds values

> [!success]- Answer
> **Correct Answer: B) Maps old values to new values**
> 
> `.replace(mapping_dict)` substitutes values according to dictionary mappings.

**Q22.** What does `np.inf` represent?

- [ ] A) Zero
- [x] B) Infinity (no upper limit)
- [ ] C) Null
- [ ] D) -1

> [!success]- Answer
> **Correct Answer: B) Infinity (no upper limit)**
> 
> `np.inf` represents infinity, used for open-ended ranges like [200, np.inf] meaning "200+".

**Q23.** In regex, what does `r'\D+'` match?

- [ ] A) Any digit
- [x] B) One or more non-digit characters
- [ ] C) Exactly one digit
- [ ] D) Any letter

> [!success]- Answer
> **Correct Answer: B) One or more non-digit characters**
> 
> `\D` = non-digit, `+` = one or more. Matches sequences like "+", "-", "(", ")", spaces, etc.

---

### Chapter 3: Advanced Data Problems (11 questions)

**Q24.** What is uniformity in data cleaning?

- [ ] A) All data being the same
- [x] B) Ensuring consistent units and formats across data
- [ ] C) Having uniform column names
- [ ] D) Equal number of rows

> [!success]- Answer
> **Correct Answer: B) Ensuring consistent units and formats across data**
> 
> Uniformity means data represents values consistently (same units, same format) for proper comparison.

**Q25.** What is the formula to convert Fahrenheit to Celsius?

- [ ] A) C = F - 32
- [x] B) C = (F - 32) × 5/9
- [ ] C) C = F × 5/9
- [ ] D) C = (F + 32) × 5/9

> [!success]- Answer
> **Correct Answer: B) C = (F - 32) × 5/9**
> 
> The correct conversion formula: subtract 32, then multiply by 5/9.

**Q26.** What parameter in `pd.to_datetime()` converts invalid dates to NaT instead of raising errors?

- [ ] A) `invalid='coerce'`
- [x] B) `errors='coerce'`
- [ ] C) `missing='NaT'`
- [ ] D) `fail=False`

> [!success]- Answer
> **Correct Answer: B) `errors='coerce'`**
> 
> `errors='coerce'` converts unparseable dates to NaT (Not a Time) instead of raising errors.

**Q27.** What does `.dt.strftime()` do?

- [ ] A) Converts to datetime
- [x] B) Formats datetime output to a specific string format
- [ ] C) Extracts time only
- [ ] D) Calculates time differences

> [!success]- Answer
> **Correct Answer: B) Formats datetime output to a specific string format**
> 
> `.dt.strftime("%d-%m-%Y")` formats datetime objects into specified string format.

**Q28.** What is cross field validation?

- [ ] A) Comparing two DataFrames
- [x] B) Using multiple fields to check data integrity
- [ ] C) Validating field names
- [ ] D) Checking for missing fields

> [!success]- Answer
> **Correct Answer: B) Using multiple fields to check data integrity**
> 
> Cross field validation uses relationships between columns to verify data (e.g., age should match birthday).

**Q29.** What does `sum(axis=1)` do?

- [ ] A) Sums all values in the DataFrame
- [x] B) Sums values across columns for each row
- [ ] C) Sums values down each column
- [ ] D) Counts rows

> [!success]- Answer
> **Correct Answer: B) Sums values across columns for each row**
> 
> `axis=1` means sum across columns (row-wise). `axis=0` would sum down columns.

**Q30.** What does NaT stand for in pandas?

- [ ] A) Not a Type
- [x] B) Not a Time (datetime equivalent of NaN)
- [ ] C) Null at Time
- [ ] D) No Available Time

> [!success]- Answer
> **Correct Answer: B) Not a Time (datetime equivalent of NaN)**
> 
> NaT is pandas' null value for datetime data, similar to NaN for numeric data.

**Q31.** What does `.isna()` do?

- [ ] A) Counts missing values
- [x] B) Returns boolean DataFrame showing missing values
- [ ] C) Removes missing values
- [ ] D) Fills missing values

> [!success]- Answer
> **Correct Answer: B) Returns boolean DataFrame showing missing values**
> 
> `.isna()` returns True for missing values (NaN, NaT, None), False otherwise.

**Q32.** What does `.dropna()` do?

- [ ] A) Fills missing values
- [ ] B) Counts missing values
- [x] C) Removes rows/columns with missing values
- [ ] D) Marks missing values

> [!success]- Answer
> **Correct Answer: C) Removes rows/columns with missing values**
> 
> `.dropna()` drops rows (or columns with `axis=1`) containing missing values.

**Q33.** What does `.fillna()` do?

- [ ] A) Removes missing values
- [x] B) Fills missing values with specified value
- [ ] C) Counts missing values
- [ ] D) Finds missing values

> [!success]- Answer
> **Correct Answer: B) Fills missing values with specified value**
> 
> `.fillna(value)` replaces NaN/NaT with specified value (mean, median, forward fill, etc.).

**Q34.** Which type of missingness has NO pattern and is truly random?

- [ ] A) MAR (Missing at Random)
- [x] B) MCAR (Missing Completely at Random)
- [ ] C) MNAR (Missing Not at Random)
- [ ] D) None of the above

> [!success]- Answer
> **Correct Answer: B) MCAR (Missing Completely at Random)**
> 
> MCAR means missing values have no pattern - they're completely random. Safe to drop or use simple imputation.

---

### Chapter 4: Record Linkage (11 questions)

**Q35.** What is minimum edit distance?

- [ ] A) Maximum operations needed
- [x] B) Least possible steps to transition from one string to another
- [ ] C) Number of different characters
- [ ] D) String length difference

> [!success]- Answer
> **Correct Answer: B) Least possible steps to transition from one string to another**
> 
> Minimum edit distance is the optimal (minimum) number of operations to transform one string into another.

**Q36.** Which algorithm allows insertion, substitution, deletion, AND transposition?

- [ ] A) Levenshtein
- [ ] B) Hamming
- [x] C) Damerau-Levenshtein
- [ ] D) Jaro distance

> [!success]- Answer
> **Correct Answer: C) Damerau-Levenshtein**
> 
> Damerau-Levenshtein includes all four operations. Levenshtein lacks transposition; Hamming only allows substitution.

**Q37.** What does `fuzz.WRatio()` return?

- [ ] A) Boolean
- [ ] B) Number of differences
- [x] C) Similarity score between 0 and 100
- [ ] D) List of matches

> [!success]- Answer
> **Correct Answer: C) Similarity score between 0 and 100**
> 
> Returns similarity score from 0 (completely different) to 100 (identical).

**Q38.** What is blocking in record linkage?

- [ ] A) Preventing comparisons
- [x] B) Reducing comparisons by only comparing records with shared attributes
- [ ] C) Removing records
- [ ] D) Encrypting data

> [!success]- Answer
> **Correct Answer: B) Reducing comparisons by only comparing records with shared attributes**
> 
> Blocking reduces computational complexity by comparing only records within same block (e.g., same state).

**Q39.** What package is used for record linkage?

- [ ] A) pandas
- [x] B) recordlinkage
- [ ] C) numpy
- [ ] D) fuzzy

> [!success]- Answer
> **Correct Answer: B) recordlinkage**
> 
> The `recordlinkage` package is specialized for linking records across DataFrames.

**Q40.** What does `.exact()` method do in recordlinkage?

- [ ] A) Fuzzy matching
- [x] B) Checks if values are exactly identical
- [ ] C) Calculates similarity
- [ ] D) Removes duplicates

> [!success]- Answer
> **Correct Answer: B) Checks if values are exactly identical**
> 
> `.exact()` returns 1 for identical values, 0 otherwise. Used for dates, IDs, codes.

**Q41.** What parameter in `.string()` comparison sets the similarity threshold?

- [ ] A) `limit`
- [x] B) `threshold`
- [ ] C) `score`
- [ ] D) `minimum`

> [!success]- Answer
> **Correct Answer: B) `threshold`**
> 
> `threshold` parameter (0.80-0.85 typical) sets minimum similarity for a match.

**Q42.** What does `matches.index.get_level_values(1)` return?

- [ ] A) First DataFrame indices
- [x] B) Second DataFrame indices
- [ ] C) Match scores
- [ ] D) Column names

> [!success]- Answer
> **Correct Answer: B) Second DataFrame indices**
> 
> Level 0 = first DF indices, Level 1 = second DF indices. Used to extract duplicate IDs.

**Q43.** What does `indexer.block('state')` do?

- [ ] A) Removes records from state
- [x] B) Only generates pairs where both records have same state
- [ ] C) Sorts by state
- [ ] D) Creates state column

> [!success]- Answer
> **Correct Answer: B) Only generates pairs where both records have same state**
> 
> Blocking on 'state' means only compare records with matching state values.

**Q44.** What does `potential_matches.sum(axis=1)` calculate?

- [ ] A) Total matches
- [x] B) Sum of similarity scores across comparison columns for each pair
- [ ] C) Number of DataFrames
- [ ] D) Total records

> [!success]- Answer
> **Correct Answer: B) Sum of similarity scores across comparison columns for each pair**
> 
> Adds up match scores (0s and 1s) across columns to identify high-quality matches.

**Q45.** In record linkage, why use `~` with `.isin()`?

- [ ] A) To find exact matches
- [x] B) To select rows NOT in the list (new records, not duplicates)
- [ ] C) To sum values
- [ ] D) To sort data

> [!success]- Answer
> **Correct Answer: B) To select rows NOT in the list (new records, not duplicates)**
> 
> `~df.index.isin(duplicates)` selects rows NOT in duplicates - the new unique records.

---

## Section B: True/False Questions (20 points)
*1 point each*

### Chapter 1 (5 questions)

**Q46.** The `object` data type in pandas always means string data. **T/F**

> [!success]- Answer
> **True** - In pandas, `object` dtype typically indicates string/text data.

**Q47.** Summing a column stored as strings performs mathematical addition. **T/F**

> [!success]- Answer
> **False** - Summing strings concatenates them: "100$" + "200$" = "100$200$", not 300.

**Q48.** The assert statement raises an error if the condition is True. **T/F**

> [!success]- Answer
> **False** - Assert raises AssertionError if condition is **False**. If True, it passes silently.

**Q49.** `.duplicated()` returns a boolean Series. **T/F**

> [!success]- Answer
> **True** - Returns True/False for each row indicating if it's a duplicate.

**Q50.** `.groupby().agg()` can aggregate duplicate rows instead of deleting them. **T/F**

> [!success]- Answer
> **True** - Groups duplicates and applies aggregation functions to combine them while preserving information.

---

### Chapter 2 (5 questions)

**Q51.** The `~` operator inverts boolean values in pandas. **T/F**

> [!success]- Answer
> **True** - `~` is the NOT operator: `~True` = `False`, `~False` = `True`.

**Q52.** `.str.strip()` removes spaces from the middle of strings. **T/F**

> [!success]- Answer
> **False** - `.str.strip()` only removes spaces from beginning and end, not middle.

**Q53.** `pd.qcut()` creates bins based on specific value ranges. **T/F**

> [!success]- Answer
> **False** - `pd.qcut()` creates equal-sized groups based on quantiles. `pd.cut()` uses value ranges.

**Q54.** `np.inf` represents infinity. **T/F**

> [!success]- Answer
> **True** - `np.inf` represents infinity, used for open-ended upper bounds like "200+".

**Q55.** In `pd.cut(data, bins=[0, 100, 200])`, the number of categories created is 2. **T/F**

> [!success]- Answer
> **True** - 3 boundaries create 2 bins: [0-100) and [100-200). Number of bins = boundaries - 1.

---

### Chapter 3 (5 questions)

**Q56.** `pd.to_datetime()` with `errors='coerce'` converts invalid dates to NaT. **T/F**

> [!success]- Answer
> **True** - `errors='coerce'` converts unparseable dates to NaT instead of raising errors.

**Q57.** `.dt.strftime()` converts strings to datetime. **T/F**

> [!success]- Answer
> **False** - `.dt.strftime()` formats datetime TO strings. `pd.to_datetime()` converts strings TO datetime.

**Q58.** Cross field validation uses multiple columns to check data integrity. **T/F**

> [!success]- Answer
> **True** - Uses relationships between fields (e.g., age vs birthday) to verify data consistency.

**Q59.** `.isna()` removes missing values. **T/F**

> [!success]- Answer
> **False** - `.isna()` returns boolean mask showing where values are missing. `.dropna()` removes them.

**Q60.** MCAR (Missing Completely at Random) means missing values have no pattern. **T/F**

> [!success]- Answer
> **True** - MCAR is truly random missingness with no pattern or relationship to other variables.

---

### Chapter 4 (5 questions)

**Q61.** `fuzz.WRatio()` returns a value between 0 and 1. **T/F**

> [!success]- Answer
> **False** - Returns score between 0 and 100, not 0 and 1.

**Q62.** Blocking reduces the number of comparisons in record linkage. **T/F**

> [!success]- Answer
> **True** - Blocking dramatically reduces comparisons by only comparing records within same block.

**Q63.** `.exact()` in recordlinkage performs fuzzy matching. **T/F**

> [!success]- Answer
> **False** - `.exact()` checks for identical matches only. `.string()` performs fuzzy matching.

**Q64.** `matches.index.get_level_values(0)` returns the second DataFrame's indices. **T/F**

> [!success]- Answer
> **False** - Level 0 returns first DF indices. Level 1 returns second DF indices.

**Q65.** `pd.concat()` combines DataFrames by stacking them. **T/F**

> [!success]- Answer
> **True** - `pd.concat()` stacks DataFrames vertically (by default) or horizontally (with `axis=1`).

---

## Section C: Short Answer Questions (45 points)
*3 points each*

### Chapter 1 (4 questions)

**Q66.** Explain the difference between `.duplicated()` and `.drop_duplicates()`. What does each return?

> [!success]- Answer
> **`.duplicated()` - Identifies duplicates:**
> - Returns boolean Series (True/False for each row)
> - Purpose: Find which rows are duplicates
> - Example: `duplicates = df.duplicated(subset=['name'])`
> 
> **`.drop_duplicates()` - Removes duplicates:**
> - Returns DataFrame with duplicates removed
> - Purpose: Actually delete duplicate rows
> - Example: `df.drop_duplicates(subset=['name'], inplace=True)`
> 
> **Key difference:** `.duplicated()` identifies, `.drop_duplicates()` removes.

**Q67.** Describe the steps to convert a column with values like "100$", "200$" to integers and verify the conversion.

> [!success]- Answer
> **Complete workflow:**
> 
> ```python
> # Step 1: Check current type
> sales.dtypes  # Shows 'object'
> 
> # Step 2: Remove dollar sign
> sales['Revenue'] = sales['Revenue'].str.strip('$')
> 
> # Step 3: Convert to integer
> sales['Revenue'] = sales['Revenue'].astype('int')
> 
> # Step 4: Verify with assert
> assert sales['Revenue'].dtype == 'int'
> ```
> 
> Must strip the symbol first, then convert type, then verify.

**Q68.** Explain what the `keep` parameter does in `.duplicated()`. Describe all three options: 'first', 'last', and False.

> [!success]- Answer
> **`keep='first'` (default):**
> - Marks first occurrence as False (not duplicate)
> - Marks subsequent occurrences as True
> 
> **`keep='last'`:**
> - Marks last occurrence as False
> - Marks earlier occurrences as True
> 
> **`keep=False`:**
> - Marks ALL occurrences as True
> - Useful for finding all duplicate rows including originals
> 
> Controls which duplicate occurrence is marked/kept.

**Q69.** Why is it wrong to calculate the mean of categorical variables stored as numbers? What's the solution?

> [!success]- Answer
> **The Problem:**
> Marriage status as numbers (0=Never married, 1=Married, 2=Separated, 3=Divorced):
> - Mean of 1.4 is meaningless
> - Standard deviation doesn't make sense
> - Mathematical operations assume numeric relationships that don't exist
> 
> **The Solution:**
> ```python
> df['marriage_status'] = df['marriage_status'].astype('category')
> df.describe()
> # Now shows: count, unique, top, freq (appropriate!)
> ```
> 
> Convert to category type for proper categorical statistics.

---

### Chapter 2 (4 questions)

**Q70.** Explain how to use set difference to find invalid categories. Include code example.

> [!success]- Answer
> **Method: Set Difference**
> 
> ```python
> # Step 1: Define valid categories
> valid = ['O-', 'O+', 'A-', 'A+', 'B+', 'B-', 'AB+', 'AB-']
> 
> # Step 2: Find invalid categories
> invalid = set(data['blood_type']).difference(valid)
> # Returns: {'Z+'} (in data but not valid)
> 
> # Step 3: Get rows with invalid values
> invalid_rows = data['blood_type'].isin(invalid)
> problem_data = data[invalid_rows]
> 
> # Step 4: Drop invalid rows
> clean_data = data[~invalid_rows]
> ```
> 
> `.difference()` finds values in first set but NOT in second.

**Q71.** Compare `pd.qcut()` and `pd.cut()`. When should you use each?

> [!success]- Answer
> **`pd.qcut()` - Quantile-based:**
> - Creates equal-sized groups
> - Divides by quantiles (percentiles)
> - ~Same number of observations per bin
> - Example: `pd.qcut(df['income'], q=3, labels=['Low', 'Med', 'High'])`
> 
> **`pd.cut()` - Value-based:**
> - Creates specific value ranges
> - Uses explicit boundaries
> - Can have uneven distribution
> - Example: `pd.cut(df['income'], bins=[0, 50000, 100000, np.inf])`
> 
> **When to use:**
> - **qcut:** Need balanced groups (A/B testing, equal samples)
> - **cut:** Have specific thresholds (tax brackets, age groups)

**Q72.** How do you standardize inconsistent capitalization and remove spaces in categorical data? Provide complete code.

> [!success]- Answer
> **Problem:** 'married', 'Married', 'MARRIED', ' married ', 'married '
> 
> **Solution:**
> ```python
> # Step 1: Remove spaces
> df['status'] = df['status'].str.strip()
> 
> # Step 2: Standardize case
> df['status'] = df['status'].str.lower()
> # Or: df['status'].str.upper()
> 
> # Combined approach:
> df['status'] = df['status'].str.strip().str.lower()
> 
> # Step 3: Verify
> print(df['status'].value_counts())
> # Output: married: 472, unmarried: 528
> ```
> 
> Always strip spaces AND standardize case for consistency.

**Q73.** Explain what regex pattern `r'\D+'` does and provide an example of its use in text cleaning.

> [!success]- Answer
> **Pattern breakdown:**
> - `r` = Raw string (treats backslash literally)
> - `\D` = Any NON-digit character (not 0-9)
> - `+` = One or more occurrences
> 
> **What it matches:** Any sequence of non-digits: +, -, (, ), spaces, letters
> 
> **Example use - cleaning phone numbers:**
> ```python
> # Before: '+(01706)-25891', '+0500-571437', '(555) 123-4567'
> 
> phones['number'] = phones['number'].str.replace(r'\D+', '')
> 
> # After: '0170625891', '0500571437', '5551234567'
> ```
> 
> Removes ALL non-digit characters in one operation.

---

### Chapter 3 (4 questions)

**Q74.** What is cross field validation? Provide an example with code showing how to validate that age matches birthday.

> [!success]- Answer
> **Definition:** Using multiple fields in a dataset to check data integrity and consistency.
> 
> **Example: Age vs Birthday validation**
> 
> ```python
> import datetime as dt
> import pandas as pd
> 
> # Step 1: Convert birthday to datetime
> users['Birthday'] = pd.to_datetime(users['Birthday'])
> 
> # Step 2: Get today's date
> today = dt.date.today()
> 
> # Step 3: Calculate age from birthday
> age_calculated = today.year - users['Birthday'].dt.year
> 
> # Step 4: Compare with reported age
> age_matches = age_calculated == users['Age']
> 
> # Step 5: Find inconsistencies
> inconsistent = users[~age_matches]
> consistent = users[age_matches]
> ```
> 
> Verifies that age field matches what birthday indicates.

**Q75.** Explain the three types of missingness (MCAR, MAR, MNAR) and how each affects handling strategy.

> [!success]- Answer
> **1. MCAR (Missing Completely at Random):**
> - No pattern in missingness
> - Missing values are truly random
> - Example: Random sensor failures
> - **Handling:** Safe to drop or use simple imputation
> 
> **2. MAR (Missing at Random):**
> - Missingness related to OTHER observed variables
> - Pattern explained by other columns
> - Example: Low-income respondents skip income questions
> - **Handling:** Can use other columns to predict/impute
> 
> **3. MNAR (Missing Not at Random):**
> - Missingness related to the VALUE itself
> - Pattern within the missing variable
> - Example: High earners don't report income
> - **Handling:** Most challenging - need domain expertise
> 
> **Why it matters:** Type determines best handling strategy.

**Q76.** Describe how to use `pd.to_datetime()` with error handling for dates in multiple formats. Include the `errors` parameter.

> [!success]- Answer
> **Problem:** Mixed date formats, invalid dates
> 
> **Solution with error handling:**
> ```python
> # Without error handling - will fail!
> df['date'] = pd.to_datetime(df['date'])
> # ValueError: month must be in 1..12
> 
> # With error handling
> df['date'] = pd.to_datetime(df['date'], errors='coerce')
> # Invalid dates become NaT (Not a Time)
> 
> # Format output consistently
> df['date'] = df['date'].dt.strftime("%d-%m-%Y")
> ```
> 
> **What happens:**
> - Valid dates: Converted successfully
> - Invalid dates (27/27/19): Become NaT
> - Text dates: Automatically parsed if recognizable
> 
> `errors='coerce'` prevents crashes and handles bad data gracefully.

**Q77.** What is temperature uniformity? Show how to convert Fahrenheit values to Celsius with code.

> [!success]- Answer
> **Temperature Uniformity Problem:**
> Data has mixed units - some Celsius, some Fahrenheit. Need consistent units for analysis.
> 
> **Example:** NYC temperature data shows 62.6°C (unrealistic - likely Fahrenheit)
> 
> **Solution - Convert F to C:**
> ```python
> # Formula: C = (F - 32) × 5/9
> 
> # Step 1: Identify Fahrenheit values (> 40°C unrealistic)
> temp_fah = temps.loc[temps['Temperature'] > 40, 'Temperature']
> 
> # Step 2: Convert to Celsius
> temp_cels = (temp_fah - 32) * (5/9)
> 
> # Step 3: Replace in DataFrame
> temps.loc[temps['Temperature'] > 40, 'Temperature'] = temp_cels
> 
> # Step 4: Verify
> assert temps['Temperature'].max() < 40
> ```
> 
> Use domain knowledge (40°C max realistic) to identify outliers.

---

### Chapter 4 (3 questions)

**Q78.** Describe the complete record linkage workflow using recordlinkage package. List all major steps.

> [!success]- Answer
> **Complete 10-Step Workflow:**
> 
> ```python
> import recordlinkage
> 
> # 1. Create indexer
> indexer = recordlinkage.Index()
> 
> # 2. Set blocking rule
> indexer.block('state')
> 
> # 3. Generate pairs
> pairs = indexer.index(df_A, df_B)
> 
> # 4. Create comparison object
> compare_cl = recordlinkage.Compare()
> 
> # 5. Define comparisons
> compare_cl.exact('date_of_birth', 'date_of_birth', label='dob')
> compare_cl.exact('state', 'state', label='state')
> compare_cl.string('surname', 'surname', threshold=0.85, label='surname')
> compare_cl.string('address', 'address', threshold=0.85, label='address')
> 
> # 6. Compute matches
> potential_matches = compare_cl.compute(pairs, df_A, df_B)
> 
> # 7. Filter high-quality matches
> matches = potential_matches[potential_matches.sum(axis=1) >= 3]
> 
> # 8. Extract duplicate indices
> duplicate_rows = matches.index.get_level_values(1)
> 
> # 9. Separate new records
> df_B_new = df_B[~df_B.index.isin(duplicate_rows)]
> 
> # 10. Link DataFrames
> full_df = pd.concat([df_A, df_B_new])
> ```

**Q79.** Why is blocking important in record linkage? Give a specific numerical example showing computational savings.

> [!success]- Answer
> **Why Blocking Matters:**
> Prevents comparing every record with every other record (Cartesian product explosion).
> 
> **Numerical Example:**
> 
> **Scenario:** 
> - df_A: 1,000 records
> - df_B: 1,000 records
> 
> **WITHOUT Blocking:**
> - Comparisons: 1,000 × 1,000 = **1,000,000 comparisons**
> 
> **WITH Blocking on 'state' (50 states, ~20 records per state):**
> - Per state: 20 × 20 = 400
> - Total: 50 × 400 = **20,000 comparisons**
> 
> **Result:** 98% reduction (from 1M → 20K)
> 
> **Trade-off:** Assumes blocking key (state) is accurate. Typos in state would miss matches.

**Q80.** Explain when to use `.exact()` vs `.string()` comparison in record linkage. Give 3 examples for each.

> [!success]- Answer
> **Use `.exact()` for structured/consistent fields:**
> 
> 1. **Dates** - Date of birth, transaction dates
>    - Must match exactly
>    - `compare_cl.exact('date_of_birth', 'date_of_birth')`
> 
> 2. **Codes** - State abbreviations, zip codes
>    - Standardized formats
>    - `compare_cl.exact('state', 'state')`
> 
> 3. **IDs** - Social Security Numbers (when available)
>    - Should be identical
>    - `compare_cl.exact('ssn', 'ssn')`
> 
> **Use `.string()` for text prone to variations:**
> 
> 4. **Names** - First/last names
>    - Typos, nicknames
>    - `compare_cl.string('surname', 'surname', threshold=0.85)`
> 
> 5. **Addresses** - Street addresses
>    - Abbreviations, formatting
>    - `compare_cl.string('address', 'address', threshold=0.85)`
> 
> 6. **Company names** - Organization names
>    - Different orderings, abbreviations
>    - `compare_cl.string('company', 'company', threshold=0.80)`

---

## Section D: Code Analysis & Scenarios (45 points)
*2.5 points each*

### Chapter 1 (5 questions)

**Q81.** What does this code do?

```python
sales['Revenue'] = sales['Revenue'].str.strip(')
sales['Revenue'] = sales['Revenue'].astype('int')
assert sales['Revenue'].dtype == 'int'
```

> [!success]- Answer
> **This code converts currency strings to integers and verifies the conversion.**
> 
> **Step-by-step:**
> 1. `.str.strip(')` removes dollar signs: "100$" → "100"
> 2. `.astype('int')` converts strings to integers: "100" → 100
> 3. `assert` verifies the type is now int (passes silently if True)
> 
> **Result:** Revenue column changes from object (string) to int type, enabling mathematical operations.

**Q82.** Complete this code to drop rows where rating > 5:

```python
movies = movies[________]
assert movies['avg_rating'].____() <= 5
```

> [!success]- Answer
> **Completed code:**
> ```python
> movies = movies[movies['avg_rating'] <= 5]
> assert movies['avg_rating'].max() <= 5
> ```
> 
> **Explanation:**
> - Filter keeps only rows where rating ≤ 5
> - `.max()` gets maximum value
> - Assert verifies no ratings exceed 5

**Q83.** What does this code do?

```python
duplicates = height_weight.duplicated(subset=['first_name', 'last_name', 'address'], keep=False)
height_weight[duplicates].sort_values(by='first_name')
```

> [!success]- Answer
> **This code finds and displays ALL duplicate rows (including originals).**
> 
> **Breakdown:**
> 1. `.duplicated(subset=[...])` - Check only name and address
> 2. `keep=False` - Mark ALL occurrences as duplicates (not just subsequent)
> 3. `height_weight[duplicates]` - Filter to show only duplicate rows
> 4. `.sort_values(by='first_name')` - Sort for easier viewing
> 
> **Use case:** Finding partial duplicates where some columns differ (like height/weight vary but name/address match).

**Q84.** Complete this code to aggregate duplicates:

```python
cols = ['first_name', 'last_name', 'address']
summaries = {'height': '____', 'weight': '____'}
df = df.______(by=cols).______(summaries).reset_index()
```

> [!success]- Answer
> **Completed code:**
> ```python
> cols = ['first_name', 'last_name', 'address']
> summaries = {'height': 'max', 'weight': 'mean'}
> df = df.groupby(by=cols).agg(summaries).reset_index()
> ```
> 
> **What it does:**
> - Groups by identifying columns (name, address)
> - Takes maximum height among duplicates
> - Takes mean weight among duplicates
> - Results in one row per unique person
> - Preserves information from all duplicate rows

**Q85.** Debug this code that's trying to convert dates:

```python
import datetime as dt
today = dt.date.today()
future = users[users['subscription_date'] > today]
```

Assume subscription_date is currently object (string) type.

> [!success]- Answer
> **Issue:** Can't compare strings directly with date objects.
> 
> **Fixed code:**
> ```python
> import datetime as dt
> import pandas as pd
> 
> # Step 1: Convert to datetime first
> users['subscription_date'] = pd.to_datetime(
>     users['subscription_date']
> ).dt.date
> 
> # Step 2: Now comparison works
> today = dt.date.today()
> future = users[users['subscription_date'] > today]
> 
> print(f"Found {len(future)} future dates")
> ```
> 
> **Why:** Must convert string dates to datetime before comparing with `dt.date.today()`.

---

### Chapter 2 (4 questions)

**Q86.** What does this code do?

```python
invalid = set(data['blood_type']).difference(valid_categories['blood_type'])
invalid_rows = data['blood_type'].isin(invalid)
clean_data = data[~invalid_rows]
```

> [!success]- Answer
> **This code finds and removes invalid blood type categories.**
> 
> **Step-by-step:**
> 1. `.difference()` finds blood types in data but NOT in valid list (e.g., 'Z+')
> 2. `.isin(invalid)` creates boolean mask: True for invalid rows
> 3. `~invalid_rows` inverts mask: True for VALID rows
> 4. `data[~invalid_rows]` keeps only valid rows
> 
> **Result:** clean_data contains only records with valid blood types.

**Q87.** Complete this code to create 3 income groups using custom ranges:

```python
ranges = [0, 50000, 100000, ______]
labels = ['Low', 'Medium', 'High']
df['income_group'] = pd.______(df['income'], ______=ranges, ______=labels)
```

> [!success]- Answer
> **Completed code:**
> ```python
> ranges = [0, 50000, 100000, np.inf]
> labels = ['Low', 'Medium', 'High']
> df['income_group'] = pd.cut(df['income'], bins=ranges, labels=labels)
> ```
> 
> **Explanation:**
> - `np.inf` = infinity (no upper limit)
> - `pd.cut()` = value-based binning
> - `bins` = boundary values
> - `labels` = category names
> 
> **Bins:** Low: $0-50K, Medium: $50K-100K, High: $100K+

**Q88.** What does this code do?

```python
df['status'] = df['status'].str.strip().str.lower()
print(df['status'].value_counts())
```

> [!success]- Answer
> **This code standardizes categorical data by removing spaces and converting to lowercase.**
> 
> **Transformation:**
> Before: 'married', 'Married', 'MARRIED', ' married ', 'married '
> 
> After `.str.strip()`: 'married', 'Married', 'MARRIED', 'married', 'married'
> 
> After `.str.lower()`: 'married', 'married', 'married', 'married', 'married'
> 
> **Result:** All variations become 'married' - ready for counting/analysis.
> 
> `.value_counts()` then shows consolidated counts.

**Q89.** Complete this code to remove all non-digit characters from phone numbers:

```python
phones['number'] = phones['number'].str.replace(______, '')
```

> [!success]- Answer
> **Completed code:**
> ```python
> phones['number'] = phones['number'].str.replace(r'\D+', '')
> ```
> 
> **Explanation:**
> - `r'\D+'` is regex pattern
> - `\D` = any non-digit
> - `+` = one or more
> - Replaces with empty string (removes)
> 
> **Example:**
> - '+(01706)-25891' → '0170625891'
> - '(555) 123-4567' → '5551234567'
> 
> Removes +, -, (, ), spaces, letters in one operation.

---

### Chapter 3 (5 questions)

**Q90.** What does this code do?

```python
temp_fah = temps.loc[temps['Temperature'] > 40, 'Temperature']
temp_cels = (temp_fah - 32) * (5/9)
temps.loc[temps['Temperature'] > 40, 'Temperature'] = temp_cels
```

> [!success]- Answer
> **This code converts Fahrenheit temperatures to Celsius for outlier values.**
> 
> **Breakdown:**
> 1. Identifies temperatures > 40°C (unrealistic, likely in Fahrenheit)
> 2. Applies conversion formula: C = (F - 32) × 5/9
> 3. Replaces outlier values with converted Celsius values
> 
> **Example:**
> - 62.6°F → (62.6 - 32) × 5/9 = 17.0°C
> 
> **Why:** Ensures temperature uniformity - all values in Celsius.

**Q91.** Complete this code to validate that passenger totals match class sums:

```python
sum_classes = flights[['economy_class', 'business_class', 'first_class']].sum(______)
matches = sum_classes ______ flights['total_passengers']
inconsistent = flights[______]
```

> [!success]- Answer
> **Completed code:**
> ```python
> sum_classes = flights[['economy_class', 'business_class', 'first_class']].sum(axis=1)
> matches = sum_classes == flights['total_passengers']
> inconsistent = flights[~matches]
> ```
> 
> **Explanation:**
> - `axis=1` sums across columns (row-wise)
> - `==` compares calculated sum with reported total
> - `~matches` gets rows where totals DON'T match
> 
> **Cross field validation:** economy + business + first should equal total.

**Q92.** What does this code do?

```python
df['date'] = pd.to_datetime(df['date'], errors='coerce')
df['date'] = df['date'].dt.strftime("%d-%m-%Y")
```

> [!success]- Answer
> **This code converts dates to datetime, handles errors, then formats consistently.**
> 
> **Step 1:** `pd.to_datetime(..., errors='coerce')`
> - Converts valid dates to datetime
> - Invalid dates (27/27/19) become NaT
> - Doesn't crash on errors
> 
> **Step 2:** `.dt.strftime("%d-%m-%Y")`
> - Formats all valid dates as DD-MM-YYYY
> - Example: 2019-03-29 → 29-03-2019
> 
> **Result:** Consistent date format, with invalid dates as NaT.

**Q93.** Complete this code to check for missing values and count them:

```python
missing_mask = df.______()
missing_count = missing_mask.______()
print(missing_count)
```

> [!success]- Answer
> **Completed code:**
> ```python
> missing_mask = df.isna()
> missing_count = missing_mask.sum()
> print(missing_count)
> ```
> 
> **Explanation:**
> - `.isna()` returns boolean DataFrame (True = missing)
> - `.sum()` counts True values per column
> 
> **Output example:**
> ```
> Date            0
> Temperature     0
> CO2           366
> dtype: int64
> ```
> 
> Shows 366 missing CO2 values.

**Q94.** What does this code do?

```python
missing = df[df['CO2'].isna()]
complete = df[~df['CO2'].isna()]
print(missing.describe())
print(complete.describe())
```

> [!success]- Answer
> **This code separates and analyzes missing vs complete data subsets.**
> 
> **Purpose:** Compare characteristics of rows with missing CO2 vs rows with complete CO2.
> 
> **What it reveals:**
> - Are missing values random or systematic?
> - Do rows with missing CO2 have unusual values in other columns?
> 
> **Example finding:**
> ```
> # Missing subset has Temperature mean = -39.7°C
> # Complete subset has Temperature mean = 18.3°C
> ```
> 
> Conclusion: Missing CO2 occurs with placeholder temperatures (-40°C) - systematic missingness!

---

### Chapter 4 (4 questions)

**Q95.** What does this code do?

```python
from thefuzz import fuzz
result1 = fuzz.WRatio('Los Angeles Lakers', 'Lakers')
result2 = fuzz.WRatio('Lakers vs Rockets', 'Rockets vs Lakers')
print(result1, result2)
```

> [!success]- Answer
> **This code performs partial string matching and compares similarity scores.**
> 
> **Expected output:** `90 86`
> 
> **Explanation:**
> - `result1 = 90`: High score - "Lakers" is subset of "Los Angeles Lakers" (partial match)
> - `result2 = 86`: Slightly lower - same words, different order
> 
> **Key insight:** `WRatio` handles partial strings and word reordering well.

**Q96.** Find and fix ALL issues:

```python
indexer = recordlinkage.Index()
indexer.block('city')
pairs = indexer.index(df_A, df_B)

compare = recordlinkage.Compare()
compare.exact('name', 'name', label='name')
compare.string('address', 'address', label='address')

matches = compare.compute(pairs, df_A, df_B)
duplicates = matches.index.get_level_values(0)
new_records = df_B[~df_B.index.isin(duplicates)]
```

> [!success]- Answer
> **Issues:**
> 1. ❌ Missing `threshold` in `.string()`
> 2. ❌ No filtering before extracting indices
> 3. ❌ Wrong level (0 instead of 1)
> 
> **Fixed code:**
> ```python
> import recordlinkage
> import pandas as pd
> 
> indexer = recordlinkage.Index()
> indexer.block('city')
> pairs = indexer.index(df_A, df_B)
> 
> compare = recordlinkage.Compare()
> compare.exact('name', 'name', label='name')
> compare.string('address', 'address', threshold=0.85, label='address')  # FIX 1
> 
> potential_matches = compare.compute(pairs, df_A, df_B)
> matches = potential_matches[potential_matches.sum(axis=1) >= 2]  # FIX 2
> 
> duplicates = matches.index.get_level_values(1)  # FIX 3: Use level 1
> new_records = df_B[~df_B.index.isin(duplicates)]
> ```

**Q97.** Complete this code to collapse state variations:

```python
from thefuzz import process

correct_states = ['California', 'Texas', 'New York']

for state in correct_states:
    matches = process.extract(state, survey['state'], limit=______)
    for potential_match in matches:
        if potential_match[______] >= 80:
            survey.loc[survey['state'] == potential_match[______], 'state'] = state
```

> [!success]- Answer
> **Completed code:**
> ```python
> from thefuzz import process
> 
> correct_states = ['California', 'Texas', 'New York']
> 
> for state in correct_states:
>     matches = process.extract(state, survey['state'], limit=survey.shape[0])
>     for potential_match in matches:
>         if potential_match[1] >= 80:
>             survey.loc[survey['state'] == potential_match[0], 'state'] = state
> ```
> 
> **Explanation:**
> - `limit=survey.shape[0]`: Get all rows
> - `potential_match[1]`: Similarity score (second element)
> - `potential_match[0]`: Matched string (first element)
> 
> Replaces variations like "Cali", "Californie" with "California".

**Q98.** Analyze this output and determine which pairs pass the threshold:

```python
potential_matches = compare_cl.compute(pairs, census_A, census_B)
print(potential_matches.head())
```

Output:
```
                    date_of_birth  state  surname  address_1
rec_id_1    rec_id_2
rec-100-org rec-200-dup    1        1      0.9       0.0
rec-101-org rec-201-dup    1        1      1.0       1.0
rec-102-org rec-202-dup    0        1      0.85      0.0
rec-103-org rec-203-dup    1        0      1.0       0.9
```

Which pairs would be selected with `sum(axis=1) >= 3`?

> [!success]- Answer
> **Row-by-row sum calculation:**
> - rec-100-org / rec-200-dup: 1 + 1 + 0.9 + 0 = **2.9** ❌
> - rec-101-org / rec-201-dup: 1 + 1 + 1 + 1 = **4.0** ✅
> - rec-102-org / rec-202-dup: 0 + 1 + 0.85 + 0 = **1.85** ❌
> - rec-103-org / rec-203-dup: 1 + 0 + 1 + 0.9 = **2.9** ❌
> 
> **Selected:** Only rec-101-org / rec-201-dup (sum = 4.0)
> 
> **Interpretation:** This pair has matches across all 4 fields - very high confidence. Others just miss the threshold.

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (90 points)
**Q1-12 (Chapter 1):** B, C, B, C, C, B, C, B, B, B, C, B  
**Q13-23 (Chapter 2):** B, B, B, B, B, B, B, A, B, B, B  
**Q24-34 (Chapter 3):** B, B, B, B, B, B, B, B, C, B, B  
**Q35-45 (Chapter 4):** B, C, C, B, B, B, B, B, B, B, B

### Section B: True/False (20 points)
**Q46-50 (Ch 1):** T, F, F, T, T  
**Q51-55 (Ch 2):** T, F, F, T, T  
**Q56-60 (Ch 3):** T, F, T, F, T  
**Q61-65 (Ch 4):** F, T, F, F, T

### Section C: Short Answer (45 points)
Q66-80: See detailed answers above. Award 3 points each for complete, accurate responses with code examples where appropriate.

### Section D: Code Analysis (45 points)
Q81-98: See detailed answers above. Award 2.5 points each for correct solutions with explanations.

**Total: 200 points**
**Passing Score: 140 points (70%)**

---

## Quick Reference Guide

### Chapter 1: Common Data Problems

**Data Types:**
```python
df.dtypes                           # Check types
df['col'].astype('int')            # Convert type
df['col'].astype('category')       # To category
pd.to_datetime(df['col'])          # To datetime
```

**Handling Ranges:**
```python
df = df[df['rating'] <= 5]         # Drop out of range
df.loc[df['rating'] > 5, 'rating'] = 5  # Cap at max
```

**Duplicates:**
```python
df.duplicated()                     # Find duplicates
df.drop_duplicates(inplace=True)   # Remove duplicates
df.groupby(cols).agg(funcs)        # Aggregate duplicates
```

**Assertions:**
```python
assert df['col'].dtype == 'int'    # Verify type
assert df['col'].max() <= 5        # Verify range
```

---

### Chapter 2: Text & Categorical Data

**Find Invalid Categories:**
```python
invalid = set(df['col']).difference(valid)
invalid_rows = df['col'].isin(invalid)
clean = df[~invalid_rows]
```

**Value Consistency:**
```python
df['col'] = df['col'].str.strip()  # Remove spaces
df['col'] = df['col'].str.lower()  # Lowercase
```

**Create Categories:**
```python
pd.qcut(df['col'], q=3, labels=names)  # Equal groups
pd.cut(df['col'], bins=[0,100,200], labels=names)  # Custom ranges
df['col'].replace(mapping_dict)     # Map to fewer
```

**Text Cleaning:**
```python
df['col'].str.replace('+', '00')   # Replace chars
df['col'].str.replace(r'\D+', '')  # Regex: remove non-digits
df['col'].str.len()                # Check length
```

---

### Chapter 3: Advanced Problems

**Uniformity:**
```python
# Temperature conversion
temp_c = (temp_f - 32) * (5/9)

# Date conversion
df['date'] = pd.to_datetime(df['date'], errors='coerce')
df['date'] = df['date'].dt.strftime("%d-%m-%Y")
```

**Cross Field Validation:**
```python
# Sum across columns
sum_cols = df[cols].sum(axis=1)
matches = sum_cols == df['total']
inconsistent = df[~matches]
```

**Missing Data:**
```python
df.isna()                          # Find missing
df.isna().sum()                    # Count missing
df.dropna(subset=['col'])          # Drop missing
df.fillna({'col': value})          # Fill missing
```

---

### Chapter 4: Record Linkage

**String Matching:**
```python
from thefuzz import fuzz, process

fuzz.WRatio('string1', 'string2')  # Similarity score 0-100
process.extract(target, choices, limit=n)  # Top n matches
```

**Record Linkage:**
```python
import recordlinkage

# 1. Generate pairs
indexer = recordlinkage.Index()
indexer.block('state')
pairs = indexer.index(df_A, df_B)

# 2. Compare
compare = recordlinkage.Compare()
compare.exact('col1', 'col1', label='col1')
compare.string('col2', 'col2', threshold=0.85, label='col2')
potential = compare.compute(pairs, df_A, df_B)

# 3. Filter and link
matches = potential[potential.sum(axis=1) >= 3]
duplicates = matches.index.get_level_values(1)
df_B_new = df_B[~df_B.index.isin(duplicates)]
result = pd.concat([df_A, df_B_new])
```

---

## Study Strategy (10-Day Plan)

### Days 1-2: Chapter 1 - Common Data Problems
- [ ] Master data type conversions (`.astype()`, `pd.to_datetime()`)
- [ ] Learn `.str.strip()` for cleaning strings
- [ ] Practice assert statements for verification
- [ ] Understand range handling (drop, cap, impute)
- [ ] Master `.duplicated()` vs `.drop_duplicates()`
- [ ] Learn `.groupby().agg()` for aggregation

### Days 3-4: Chapter 2 - Text & Categorical Data
- [ ] Understand membership constraints
- [ ] Master set `.difference()` for finding invalids
- [ ] Learn `~` operator for inverting masks
- [ ] Practice `.str.upper()`, `.str.lower()`, `.str.strip()`
- [ ] Master `pd.qcut()` vs `pd.cut()`
- [ ] Learn `.replace()` with dictionaries
- [ ] Understand regex basics (`r'\D+'`)

### Days 5-6: Chapter 3 - Advanced Problems
- [ ] Learn uniformity (temperature, date conversions)
- [ ] Master `errors='coerce'` in `pd.to_datetime()`
- [ ] Understand cross field validation
- [ ] Practice `sum(axis=1)` for row-wise operations
- [ ] Learn missing data detection (`.isna()`)
- [ ] Master `.dropna()` vs `.fillna()`
- [ ] Understand MCAR, MAR, MNAR

### Days 7-9: Chapter 4 - Record Linkage
- [ ] Master `thefuzz` library (`fuzz.WRatio()`, `process.extract()`)
- [ ] Understand minimum edit distance algorithms
- [ ] Learn blocking concept and implementation
- [ ] Master `recordlinkage` package workflow
- [ ] Practice `.exact()` vs `.string()` comparisons
- [ ] Understand MultiIndex navigation
- [ ] Master `.get_level_values()` for extracting indices
- [ ] Learn complete linking workflow

### Day 10: Integration & Final Review
- [ ] Complete end-to-end data cleaning workflows
- [ ] Practice mixed scenarios combining all chapters
- [ ] Review all common mistakes
- [ ] Take this comprehensive practice exam
- [ ] Review incorrect answers thoroughly

---

## Common Mistakes to Avoid

### Chapter 1
❌ Not using `.str` accessor: `df['col'].upper()` → Use `df['col'].str.upper()`  
❌ Forgetting `inplace=True` or reassignment: `df.drop_duplicates()` → Use `df = df.drop_duplicates()` or `inplace=True`  
❌ Using wrong `keep` parameter: Remember `keep='first'` marks first as False, rest as True  

### Chapter 2
❌ Forgetting `~` to invert: `df[invalid_rows]` keeps invalids → Use `df[~invalid_rows]`  
❌ Confusing `qcut` and `cut`: qcut = equal groups, cut = value ranges  
❌ Wrong number of labels: 4 boundaries need 3 labels (boundaries - 1)  
❌ Forgetting `r` prefix in regex: `'\D+'` → Use `r'\D+'`  

### Chapter 3
❌ Not using `errors='coerce'`: `pd.to_datetime()` crashes on bad dates → Add `errors='coerce'`  
❌ Wrong axis parameter: `sum(axis=0)` sums columns → Use `sum(axis=1)` for rows  
❌ Confusing `.isna()` with `.dropna()`: `.isna()` finds, `.dropna()` removes  

### Chapter 4
❌ Missing `threshold` in `.string()`: Always required for string comparisons  
❌ Wrong MultiIndex level: Level 0 = first DF, Level 1 = second DF  
❌ Forgetting `~` before `.isin()`: Gets duplicates instead of new records  
❌ Not filtering before extracting indices: Filter `potential_matches` first  

---

## Final Exam Checklist

### Before the Exam ✓

**Chapter 1:**
- [ ] Know 6 data types and Python equivalents
- [ ] Can convert types with `.astype()`
- [ ] Understand assert for verification
- [ ] Know how to handle out of range data
- [ ] Master `.duplicated()` and `.drop_duplicates()`

**Chapter 2:**
- [ ] Understand membership constraints
- [ ] Can use set `.difference()`
- [ ] Know `~` operator
- [ ] Can standardize with `.str` methods
- [ ] Understand `qcut()` vs `cut()`
- [ ] Can use `.replace()` with dict
- [ ] Know basic regex patterns

**Chapter 3:**
- [ ] Can convert temperature units
- [ ] Master `pd.to_datetime()` with `errors='coerce'`
- [ ] Understand cross field validation
- [ ] Know `sum(axis=1)` for row operations
- [ ] Can detect and handle missing data
- [ ] Understand MCAR, MAR, MNAR

**Chapter 4:**
- [ ] Know 4 edit distance algorithms
- [ ] Can use `fuzz.WRatio()` and `process.extract()`
- [ ] Understand blocking concept
- [ ] Master complete record linkage workflow
- [ ] Know `.exact()` vs `.string()`
- [ ] Can extract from MultiIndex
- [ ] Remember the `~` for new records

### During the Exam ✓

**General Tips:**
- [ ] Read each question carefully
- [ ] Watch for "NOT" questions
- [ ] Check all code line by line
- [ ] Look for common mistakes
- [ ] Use process of elimination

**Quick Checks:**
- [ ] Data type: `.dtypes`, `.astype()`
- [ ] Clean strings: `.str.strip()`, `.str.lower()`
- [ ] Drop range: filtering or `.drop()`
- [ ] Cap: `.loc[condition, col] = value`
- [ ] Find duplicates: `.duplicated()`
- [ ] Invalid categories: `set().difference()`
- [ ] Equal groups: `pd.qcut(q=n)`
- [ ] Custom ranges: `pd.cut(bins=[...])`
- [ ] Convert dates: `pd.to_datetime(errors='coerce')`
- [ ] Missing data: `.isna()`, `.dropna()`, `.fillna()`
- [ ] String similarity: `fuzz.WRatio()`
- [ ] Record linkage: block → compare → filter → link

---

## Key Formulas & Patterns

### Data Type Conversion
```python
df['col'].astype('int')                    # To integer
df['col'].astype('category')               # To category
pd.to_datetime(df['col'], errors='coerce') # To datetime
```

### String Cleaning
```python
df['col'].str.strip()                      # Remove spaces
df['col'].str.upper()                      # Uppercase
df['col'].str.lower()                      # Lowercase
df['col'].str.replace('old', 'new')        # Replace
df['col'].str.replace(r'\D+', '')          # Regex remove non-digits
```

### Handling Out of Range
```python
df = df[df['col'] <= max_value]            # Drop
df.loc[df['col'] > max, 'col'] = max       # Cap
df.loc[df['col'] > max, 'col'] = np.nan    # Set to missing
```

### Duplicates
```python
df.duplicated(subset=['col'], keep='first') # Find
df.drop_duplicates(subset=['col'])          # Remove
df.groupby(['col']).agg({'val': 'mean'})   # Aggregate
```

### Categories
```python
set(df['col']).difference(valid)           # Find invalid
df[~df['col'].isin(invalid)]               # Drop invalid
pd.qcut(df['col'], q=3, labels=names)      # Equal groups
pd.cut(df['col'], bins=[0,100,200])        # Custom ranges
df['col'].replace(mapping_dict)            # Map categories
```

### Missing Data
```python
df.isna()                                  # Find missing
df.isna().sum()                            # Count missing
df.dropna(subset=['col'])                  # Drop
df.fillna({'col': value})                  # Fill
df['col'].fillna(df['col'].mean())         # Fill with mean
```

### Cross Field Validation
```python
sum_cols = df[cols].sum(axis=1)            # Sum across columns
matches = sum_cols == df['total']          # Compare
inconsistent = df[~matches]                # Get mismatches
```

### Record Linkage
```python
# String matching
fuzz.WRatio('str1', 'str2')                # Similarity 0-100
process.extract(target, choices, limit=2)  # Top 2 matches

# Record linkage workflow
indexer.block('col')                       # Block
pairs = indexer.index(df_A, df_B)         # Generate pairs
compare.exact('col', 'col', label='col')  # Exact match
compare.string('col', 'col', threshold=0.85) # Fuzzy match
potential = compare.compute(pairs, df_A, df_B)
matches = potential[potential.sum(axis=1) >= 3] # Filter
dups = matches.index.get_level_values(1)  # Get indices
new = df_B[~df_B.index.isin(dups)]        # New records
result = pd.concat([df_A, new])           # Link
```

---

## Chapter Coverage Summary

| Chapter | Topics | Weight |
|---------|--------|--------|
| **Chapter 1** | Data types, ranges, duplicates | 25% |
| **Chapter 2** | Categories, text cleaning, regex | 25% |
| **Chapter 3** | Uniformity, validation, missing data | 25% |
| **Chapter 4** | Record linkage, string matching | 25% |

**Each chapter equally important for certification!**

---

## Congratulations! 🎉

You've completed the comprehensive review of:

✅ **Chapter 1:** Common data problems - types, ranges, duplicates  
✅ **Chapter 2:** Text & categorical data - membership, consistency, collapsing  
✅ **Chapter 3:** Advanced problems - uniformity, validation, missingness  
✅ **Chapter 4:** Record linkage - string matching, blocking, linking  

**You're ready to clean any messy dataset!** 🧹📊🐍

### Next Steps:
1. Take this comprehensive practice exam
2. Review any weak areas
3. Practice with real datasets
4. Take the DataCamp certification exam
5. Become a data cleaning expert!

---

#datacamp #data-cleaning #python #pandas #certification #exam-prep #comprehensive-review #data-quality #data-engineering