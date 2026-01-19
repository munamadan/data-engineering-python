## Overview
Record linkage is the process of identifying and linking records that refer to the same entity across different datasets, even when joins won't work due to inconsistencies in the data.

---

## 1. String Comparison & Minimum Edit Distance

### Minimum Edit Distance
**Definition:** The least possible amount of steps needed to transition from one string to another.

### Common Algorithms

| Algorithm | Operations Allowed |
|-----------|-------------------|
| **Damerau-Levenshtein** | Insertion, substitution, deletion, transposition |
| **Levenshtein** | Insertion, substitution, deletion |
| **Hamming** | Substitution only |
| **Jaro distance** | Transposition only |

**Python Packages:** `nltk`, `thefuzz`, `textdistance`

---

## 2. The thefuzz Package

### Basic String Comparison

```python
from thefuzz import fuzz

# Simple comparison
fuzz.WRatio('Reeding', 'Reading')
# Returns: 86 (similarity score 0-100)
```

### Partial String Matching

```python
# Partial string comparison
fuzz.WRatio('Houston Rockets', 'Rockets')
# Returns: 90

# Different ordering
fuzz.WRatio('Houston Rockets vs Los Angeles Lakers', 'Lakers vs Rockets')
# Returns: 86
```

### Comparison with Arrays

```python
from thefuzz import process

# Define string and possible matches
string = "Houston Rockets vs Los Angeles Lakers"
choices = pd.Series(['Rockets vs Lakers', 'Lakers vs Rockets',
                     'Houson vs Los Angeles', 'Heat vs Bulls'])

# Extract top matches
process.extract(string, choices, limit=2)
# Returns: [('Rockets vs Lakers', 86, 0), ('Lakers vs Rockets', 86, 1)]
```

> [!info] Return Format
> `process.extract()` returns tuples: `(match_string, similarity_score, index)`

---

## 3. Collapsing Categories with String Similarity

### Problem
Too many variations of the same category:
- "EU", "eur", "Europ", "Europa", "Erope", "Evropa"
- "California", "Cali", "Calefornia", "Californie", etc.

### Solution: Automated Category Collapse

```python
# For each correct category
for state in categories['state']:
    # Find potential matches in states with typos
    matches = process.extract(state, survey['state'], 
                             limit=survey.shape[0])
    
    # For each potential match
    for potential_match in matches:
        # If high similarity score
        if potential_match[1] >= 80:
            # Replace typo with correct category
            survey.loc[survey['state'] == potential_match[0], 'state'] = state
```

**Key Parameters:**
- **Threshold:** Typically 80-85 for good balance
- **limit:** Number of matches to return

---

## 4. Record Linkage Basics

### When Joins Won't Work
- Different column names
- Different data formats
- Typos and inconsistencies
- Missing values
- Different record structures

### The recordlinkage Package
Specialized package for linking records across DataFrames when traditional joins fail.

---

## 5. Generating Pairs

### Basic Workflow

```python
import recordlinkage

# Create indexing object
indexer = recordlinkage.Index()

# Generate pairs blocked on state
indexer.block('state')
pairs = indexer.index(census_A, census_B)
```

### Blocking
**Purpose:** Reduce computational complexity by only comparing records that share certain attributes.

**Example:** Only compare records with the same state, reducing pairs from millions to thousands.

> [!warning] Performance Tip
> Always use blocking on a column with reasonable cardinality to avoid comparing every record with every other record.

---

## 6. Comparing DataFrames

### Setting Up Comparisons

```python
# Create a Compare object
compare_cl = recordlinkage.Compare()

# Find exact matches
compare_cl.exact('date_of_birth', 'date_of_birth', label='date_of_birth')
compare_cl.exact('state', 'state', label='state')

# Find similar matches using string similarity
compare_cl.string('surname', 'surname', threshold=0.85, label='surname')
compare_cl.string('address_1', 'address_1', threshold=0.85, label='address_1')

# Compute potential matches
potential_matches = compare_cl.compute(pairs, census_A, census_B)
```

### Comparison Methods

| Method | Use Case |
|--------|----------|
| `.exact()` | Exact matches (dates, IDs, states) |
| `.string()` | Fuzzy string matching with threshold |
| `.numeric()` | Numeric comparisons |
| `.date()` | Date comparisons |

---

## 7. Finding Matching Pairs

### Filtering Potential Matches

```python
# Sum across columns to get match score
# Filter for matches with 2+ matching columns
matches = potential_matches[potential_matches.sum(axis=1) >= 2]
```

**Example Output:**
```
                    date_of_birth  state  surname  address_1
rec_id_1     rec_id_2
rec-4878-org rec-4878-dup-0    1      1      1.0        0.0
rec-417-org  rec-2867-dup-0    0      1      0.0        1.0
rec-3964-org rec-394-dup-0     0      1      1.0        0.0
```

**Match Score Interpretation:**
- 1 = Exact match (for `.exact()`) or above threshold (for `.string()`)
- 0 = No match

---

## 8. Linking DataFrames

### Complete Workflow

```python
# Step 1: Generate pairs with blocking
indexer = recordlinkage.Index()
indexer.block('state')
full_pairs = indexer.index(census_A, census_B)

# Step 2: Compare columns
compare_cl = recordlinkage.Compare()
compare_cl.exact('date_of_birth', 'date_of_birth', label='date_of_birth')
compare_cl.exact('state', 'state', label='state')
compare_cl.string('surname', 'surname', threshold=0.85, label='surname')
compare_cl.string('address_1', 'address_1', threshold=0.85, label='address_1')

potential_matches = compare_cl.compute(full_pairs, census_A, census_B)

# Step 3: Filter for high-quality matches
matches = potential_matches[potential_matches.sum(axis=1) >= 3]

# Step 4: Get indices of duplicate rows from census_B
duplicate_rows = matches.index.get_level_values(1)

# Step 5: Separate duplicates from new rows
census_B_duplicates = census_B[census_B.index.isin(duplicate_rows)]
census_B_new = census_B[~census_B.index.isin(duplicate_rows)]

# Step 6: Link the DataFrames
full_census = pd.concat([census_A, census_B_new])
```

### Key Methods

**`.get_level_values(level)`**
- Extracts specific level from MultiIndex
- `level=0`: First DataFrame indices
- `level=1`: Second DataFrame indices

**`.isin(values)`**
- Returns boolean mask for rows in specified values
- `~` negates the mask (NOT operator)

---

## 9. Common Workflows

### Duplicate Detection
```python
# Find duplicates in census_B
census_B_duplicates = census_B[census_B.index.isin(duplicate_rows)]
```

### New Records Identification
```python
# Find new rows (non-duplicates)
census_B_new = census_B[~census_B.index.isin(duplicate_rows)]
```

### DataFrame Concatenation
```python
# Combine census_A with only new records from census_B
full_census = pd.concat([census_A, census_B_new])
```

---

## 10. Best Practices

### Choosing Thresholds
- **String similarity:** 0.80-0.85 for names/addresses
- **Match score:** 3+ matching columns for high confidence
- **Adjust based on:** Data quality, false positive tolerance

### Blocking Strategy
- Block on columns with:
  - High cardinality (not too many duplicates per value)
  - High data quality (few missing values)
  - Examples: state, zip code, year

### Performance Optimization
1. Always use blocking
2. Compare only necessary columns
3. Use exact matching when possible (faster than string matching)
4. Filter early to reduce downstream processing

---

## 11. Complete Example

```python
import recordlinkage
import pandas as pd
from thefuzz import process

# Example DataFrames
census_A = pd.DataFrame({
    'given_name': ['michaela', 'courtney'],
    'surname': ['neumann', 'painter'],
    'state': ['nsw', 'vic']
})

census_B = pd.DataFrame({
    'given_name': ['mitchell', 'elton'],
    'surname': ['maxon', 'NaN'],
    'state': ['nsw', 'vic']
})

# Generate and compare pairs
indexer = recordlinkage.Index()
indexer.block('state')
pairs = indexer.index(census_A, census_B)

compare_cl = recordlinkage.Compare()
compare_cl.exact('state', 'state', label='state')
compare_cl.string('surname', 'surname', threshold=0.85, label='surname')

potential_matches = compare_cl.compute(pairs, census_A, census_B)

# Find high-quality matches
matches = potential_matches[potential_matches.sum(axis=1) >= 2]

# Link DataFrames
duplicate_rows = matches.index.get_level_values(1)
census_B_new = census_B[~census_B.index.isin(duplicate_rows)]
full_census = pd.concat([census_A, census_B_new])
```

---

## Key Takeaways

✅ **String Similarity:** Use `thefuzz` for fuzzy matching with `fuzz.WRatio()` and `process.extract()`

✅ **Record Linkage:** Use `recordlinkage` package when traditional joins fail

✅ **Blocking:** Essential for performance—compare only relevant record pairs

✅ **Comparison Methods:** Mix `.exact()` for precise fields and `.string()` for fuzzy fields

✅ **Threshold Tuning:** Balance between false positives and false negatives

✅ **Workflow:** Generate pairs → Compare → Filter → Extract indices → Link DataFrames

---

#python #data-cleaning #record-linkage #string-matching #fuzzy-matching #datacamp


# Chapter 4: Record Linkage - Practice Quiz

**Total Points: 130**
**Pass Score: 91+ (70%)**
**Time Limit: 120 minutes**

---

## Section A: Multiple Choice Questions (60 points)
*2 points each*

### Topic 1: String Comparison & Minimum Edit Distance (10 questions)

**Q1.** What is the minimum edit distance between two strings?

- [ ] A) The maximum number of operations needed to transform one string to another
- [ ] B) The least possible amount of steps needed to transition from one string to another
- [ ] C) The number of characters that are different between two strings
- [ ] D) The length difference between two strings

> [!success]- Answer
> **Correct Answer: B) The least possible amount of steps needed to transition from one string to another**
> 
> Minimum edit distance represents the optimal (minimum) number of operations required to transform one string into another. It's not about the maximum operations, just character differences, or length differences—it's about the most efficient transformation path.

**Q2.** Which algorithm allows for insertion, substitution, deletion, AND transposition operations?

- [ ] A) Levenshtein
- [ ] B) Hamming
- [ ] C) Damerau-Levenshtein
- [ ] D) Jaro distance

> [!success]- Answer
> **Correct Answer: C) Damerau-Levenshtein**
> 
> Damerau-Levenshtein is the most comprehensive algorithm that includes all four operations: insertion, substitution, deletion, and transposition. Levenshtein doesn't include transposition, Hamming only allows substitution, and Jaro distance only uses transposition.

**Q3.** What does `fuzz.WRatio('Reading', 'Reeding')` return?

- [ ] A) A boolean indicating if strings match
- [ ] B) The number of character differences
- [ ] C) A similarity score between 0 and 100
- [ ] D) A tuple with match details

> [!success]- Answer
> **Correct Answer: C) A similarity score between 0 and 100**
> 
> `fuzz.WRatio()` returns a similarity score ranging from 0 (completely different) to 100 (identical match). In this case, it would return 86, indicating high similarity between "Reading" and "Reeding".

**Q4.** Which Python package is specifically recommended in the course for fuzzy string matching?

- [ ] A) nltk
- [ ] B) textdistance
- [ ] C) thefuzz
- [ ] D) stringdist

> [!success]- Answer
> **Correct Answer: C) thefuzz**
> 
> While nltk and textdistance can also perform string matching, the course specifically uses and recommends `thefuzz` for fuzzy string matching operations.

**Q5.** What is the purpose of the `limit` parameter in `process.extract()`?

- [ ] A) Sets the minimum similarity threshold
- [ ] B) Limits the number of characters compared
- [ ] C) Specifies the maximum number of matches to return
- [ ] D) Defines the maximum edit distance allowed

> [!success]- Answer
> **Correct Answer: C) Specifies the maximum number of matches to return**
> 
> The `limit` parameter controls how many of the best matches are returned from the choices array. For example, `limit=2` returns only the top 2 matches.
> 
> ```python
> process.extract(string, choices, limit=2)
> # Returns top 2 matches only
> ```

**Q6.** What does `process.extract()` return?

- [ ] A) A list of matching strings only
- [ ] B) A list of tuples containing (match_string, similarity_score, index)
- [ ] C) A dictionary with strings as keys and scores as values
- [ ] D) A pandas Series with match results

> [!success]- Answer
> **Correct Answer: B) A list of tuples containing (match_string, similarity_score, index)**
> 
> `process.extract()` returns a list of tuples where each tuple contains three elements: the matching string from the choices, the similarity score (0-100), and the index position in the original choices array.
> 
> Example: `[('Rockets vs Lakers', 86, 0), ('Lakers vs Rockets', 86, 1)]`

**Q7.** When collapsing categories with string similarity, what is a typical threshold value?

- [ ] A) 50
- [ ] B) 65
- [x] C) 80
- [ ] D) 95

> [!success]- Answer
> **Correct Answer: C) 80**
> 
> A threshold of 80 (or 80-85) is commonly used for collapsing categories as it provides a good balance between catching legitimate variations while avoiding false positives. Lower thresholds would create too many false matches, while higher thresholds might miss valid variations.

**Q8.** Which Hamming distance algorithm operation is allowed?

- [ ] A) Insertion only
- [ ] B) Deletion only
- [ ] C) Substitution only
- [ ] D) Transposition only

> [!success]- Answer
> **Correct Answer: C) Substitution only**
> 
> The Hamming distance algorithm is the most restrictive, allowing only substitution operations. This means strings must be the same length, and you can only replace characters, not insert, delete, or transpose them.

**Q9.** What would be the best use case for partial string matching with `fuzz.WRatio()`?

- [ ] A) Comparing exact product codes
- [ ] B) Matching team names where one might be abbreviated
- [ ] C) Validating email addresses
- [ ] D) Checking password strength

> [!success]- Answer
> **Correct Answer: B) Matching team names where one might be abbreviated**
> 
> Partial string matching is ideal when one string might be a subset or abbreviation of another, like 'Rockets' being part of 'Houston Rockets'. The example in the course shows `fuzz.WRatio('Houston Rockets', 'Rockets')` returning 90, demonstrating this use case.

**Q10.** In the category collapsing loop, why do we use `survey.shape[0]` as the limit?

- [ ] A) To compare only the first row
- [ ] B) To get all possible matches from the entire dataset
- [ ] C) To limit processing time
- [ ] D) To avoid memory errors

> [!success]- Answer
> **Correct Answer: B) To get all possible matches from the entire dataset**
> 
> `survey.shape[0]` returns the number of rows in the DataFrame. Using this as the limit ensures we evaluate ALL potential matches in the dataset, not just a subset. This is necessary to find and correct all variations of each category.
> 
> ```python
> matches = process.extract(state, survey['state'], 
>                          limit=survey.shape[0])
> ```

---

### Topic 2: Record Linkage Basics (10 questions)

**Q11.** What is the main purpose of record linkage?

- [ ] A) To merge DataFrames using common keys
- [ ] B) To identify and link records referring to the same entity across different datasets
- [ ] C) To remove duplicate rows from a single DataFrame
- [ ] D) To create foreign key relationships

> [!success]- Answer
> **Correct Answer: B) To identify and link records referring to the same entity across different datasets**
> 
> Record linkage is specifically designed for situations where traditional joins won't work due to inconsistencies, typos, different formats, or missing values. It identifies records that likely refer to the same real-world entity across different datasets.

**Q12.** Which package is used for record linkage in this course?

- [ ] A) pandas
- [ ] B) recordlinkage
- [ ] C) fuzzy_matching
- [ ] D) datalink

> [!success]- Answer
> **Correct Answer: B) recordlinkage**
> 
> The `recordlinkage` package is a specialized Python library designed specifically for linking records across DataFrames when traditional merge/join operations are insufficient.

**Q13.** What is "blocking" in record linkage?

- [ ] A) Preventing certain records from being compared
- [ ] B) A technique to reduce computational complexity by only comparing records that share certain attributes
- [ ] C) Removing blocked or invalid records
- [ ] D) Encrypting sensitive data fields

> [!success]- Answer
> **Correct Answer: B) A technique to reduce computational complexity by only comparing records that share certain attributes**
> 
> Blocking dramatically reduces the number of comparisons needed. For example, if you have 1000 records in each DataFrame, comparing all pairs would require 1,000,000 comparisons. Blocking on 'state' (with 50 states) reduces this to roughly 20,000 comparisons.

**Q14.** Why might traditional DataFrame joins fail to work?

- [ ] A) DataFrames are too large
- [ ] B) Different column names, typos, missing values, and different formats
- [ ] C) Python version incompatibility
- [ ] D) Lack of memory

> [!success]- Answer
> **Correct Answer: B) Different column names, typos, missing values, and different formats**
> 
> Traditional joins require exact matches on key columns. They fail when data has inconsistencies like:
> - "California" vs "Cali" vs "CA"
> - "John Smith" vs "Jon Smith"
> - Missing values in join keys
> - Different date formats

**Q15.** What does `indexer.block('state')` do?

- [ ] A) Removes all records from the specified state
- [ ] B) Ensures pairs are only generated for records with matching state values
- [ ] C) Sorts the DataFrame by state
- [ ] D) Creates a new column called 'state'

> [!success]- Answer
> **Correct Answer: B) Ensures pairs are only generated for records with matching state values**
> 
> `indexer.block('state')` tells the indexer to only create pairs where both records have the same value in the 'state' column. This significantly reduces the number of comparisons needed.
> 
> ```python
> indexer = recordlinkage.Index()
> indexer.block('state')  # Only compare records with same state
> pairs = indexer.index(census_A, census_B)
> ```

**Q16.** What type of object does `indexer.index(census_A, census_B)` return?

- [ ] A) A regular pandas DataFrame
- [ ] B) A MultiIndex with pairs of record IDs
- [ ] C) A list of tuples
- [ ] D) A dictionary

> [!success]- Answer
> **Correct Answer: B) A MultiIndex with pairs of record IDs**
> 
> The `index()` method returns a pandas MultiIndex object where each entry represents a pair of records to compare. The two levels of the MultiIndex correspond to record IDs from census_A and census_B respectively.

**Q17.** When would you use record linkage instead of a standard merge?

- [ ] A) When you have perfect data with unique keys
- [ ] B) When the datasets have identical schemas
- [ ] C) When there are typos, inconsistencies, or missing values in join keys
- [ ] D) When both DataFrames are very small

> [!success]- Answer
> **Correct Answer: C) When there are typos, inconsistencies, or missing values in join keys**
> 
> Record linkage is specifically designed for messy, real-world data where standard merges would miss matches due to data quality issues. It uses fuzzy matching and similarity metrics to find likely matches despite imperfections.

**Q18.** What is the computational benefit of blocking?

- [ ] A) It increases matching accuracy
- [ ] B) It reduces the number of record pairs that need to be compared
- [ ] C) It improves data quality
- [ ] D) It creates better match scores

> [!success]- Answer
> **Correct Answer: B) It reduces the number of record pairs that need to be compared**
> 
> Without blocking, comparing n records from DataFrame A with m records from DataFrame B requires n×m comparisons. Blocking reduces this by only comparing records within the same block (e.g., same state), dramatically improving performance.

**Q19.** Which of the following is NOT a typical scenario where record linkage is needed?

- [ ] A) Merging customer databases from different systems with inconsistent IDs
- [ ] B) Joining two perfectly clean datasets with unique integer keys
- [ ] C) Combining census data from different years with name variations
- [ ] D) Linking patient records across hospitals with different formats

> [!success]- Answer
> **Correct Answer: B) Joining two perfectly clean datasets with unique integer keys**
> 
> Perfect data with unique integer keys can be joined using standard pandas merge operations. Record linkage is specifically for messy, real-world data where traditional joins fail.

**Q20.** What does the `recordlinkage.Index()` object do?

- [ ] A) Creates an index for faster searching
- [ ] B) Generates candidate pairs of records to compare
- [ ] C) Indexes both DataFrames
- [ ] D) Creates a primary key

> [!success]- Answer
> **Correct Answer: B) Generates candidate pairs of records to compare**
> 
> The `Index()` object is responsible for the pair generation step in record linkage. It creates candidate pairs based on blocking rules or other indexing strategies, determining which record combinations will be compared.

---

### Topic 3: Comparing & Linking DataFrames (10 questions)

**Q21.** What does the `.exact()` method in `recordlinkage.Compare()` do?

- [ ] A) Performs fuzzy matching
- [ ] B) Checks if two field values are exactly identical
- [ ] C) Calculates similarity scores
- [ ] D) Removes exact duplicates

> [!success]- Answer
> **Correct Answer: B) Checks if two field values are exactly identical**
> 
> The `.exact()` method performs exact matching, returning 1 if the values are identical and 0 if they differ. This is ideal for fields like dates, IDs, or state codes where exact matches are expected.
> 
> ```python
> compare_cl.exact('date_of_birth', 'date_of_birth', label='date_of_birth')
> # Returns 1 for exact match, 0 otherwise
> ```

**Q22.** What is the purpose of the `threshold` parameter in `.string()` comparison?

- [ ] A) Sets the minimum number of characters to compare
- [ ] B) Defines the minimum similarity score (0-1) for a match
- [ ] C) Limits the number of comparisons
- [ ] D) Sets the maximum string length

> [!success]- Answer
> **Correct Answer: B) Defines the minimum similarity score (0-1) for a match**
> 
> The threshold parameter (typically 0.80-0.85) determines how similar strings need to be to be considered a match. A threshold of 0.85 means strings must be at least 85% similar to score as a match (1), otherwise they score 0.

**Q23.** What does `potential_matches.sum(axis=1)` calculate?

- [ ] A) The total number of matches found
- [ ] B) The sum of similarity scores across all comparison columns for each pair
- [ ] C) The number of DataFrames compared
- [ ] D) The total number of records

> [!success]- Answer
> **Correct Answer: B) The sum of similarity scores across all comparison columns for each pair**
> 
> Summing across axis=1 adds up the match scores (0s and 1s) for each record pair. If you compared 4 columns, a sum of 3 means 3 columns matched. This helps identify high-quality matches.
> 
> ```python
> # If date_of_birth=1, state=1, surname=1, address=0
> # Sum = 3 (3 matching columns)
> matches = potential_matches[potential_matches.sum(axis=1) >= 3]
> ```

**Q24.** What does `matches.index.get_level_values(1)` return?

- [ ] A) The first column of matches
- [ ] B) The record IDs from the second DataFrame (census_B)
- [ ] C) The similarity scores
- [ ] D) The first level of the MultiIndex

> [!success]- Answer
> **Correct Answer: B) The record IDs from the second DataFrame (census_B)**
> 
> In a MultiIndex created by record linkage, level 0 contains IDs from the first DataFrame (census_A) and level 1 contains IDs from the second DataFrame (census_B). `get_level_values(1)` extracts the census_B IDs.

**Q25.** What does the `~` operator do in `census_B[~census_B.index.isin(duplicate_rows)]`?

- [ ] A) Bitwise NOT operation
- [ ] B) Negates the boolean mask to select rows NOT in duplicate_rows
- [ ] C) Approximate matching operator
- [ ] D) String concatenation

> [!success]- Answer
> **Correct Answer: B) Negates the boolean mask to select rows NOT in duplicate_rows**
> 
> The tilde (~) is the NOT operator for boolean arrays in pandas. It inverts the mask, so `~census_B.index.isin(duplicate_rows)` selects all rows whose index is NOT in the duplicate_rows list.
> 
> ```python
> # Get rows that are NOT duplicates (new records)
> census_B_new = census_B[~census_B.index.isin(duplicate_rows)]
> ```

**Q26.** In the record linkage workflow, what is computed by `compare_cl.compute(pairs, census_A, census_B)`?

- [ ] A) The merged DataFrame
- [ ] B) A DataFrame with similarity scores for each comparison column for each pair
- [ ] C) The number of matches found
- [ ] D) The average similarity score

> [!success]- Answer
> **Correct Answer: B) A DataFrame with similarity scores for each comparison column for each pair**
> 
> The `.compute()` method takes the pairs and both DataFrames, then calculates similarity scores for each comparison method defined (exact, string, etc.). It returns a DataFrame where rows are record pairs and columns are the comparison results.

**Q27.** Why do we filter matches with `sum(axis=1) >= 3`?

- [ ] A) To require at least 3 records
- [ ] B) To ensure at least 3 columns match, indicating a high-quality match
- [ ] C) To limit results to 3 matches
- [ ] D) To compare only 3 columns

> [!success]- Answer
> **Correct Answer: B) To ensure at least 3 columns match, indicating a high-quality match**
> 
> Requiring 3 or more matching columns increases confidence that the record pair truly refers to the same entity. Fewer matches might be coincidental, while more matches indicate stronger evidence of a true link.

**Q28.** What is the purpose of `pd.concat([census_A, census_B_new])`?

- [ ] A) To create duplicate entries
- [ ] B) To combine census_A with only the new (non-duplicate) records from census_B
- [ ] C) To merge on common columns
- [ ] D) To find the intersection

> [!success]- Answer
> **Correct Answer: B) To combine census_A with only the new (non-duplicate) records from census_B**
> 
> This final step creates a complete census by adding only the unique records from census_B to census_A. Records already in census_A (the duplicates) are excluded to avoid duplication.
> 
> ```python
> # Combine A with new records from B only
> full_census = pd.concat([census_A, census_B_new])
> ```

**Q29.** What type of comparison would be best for a 'date_of_birth' field?

- [ ] A) .string() with threshold
- [ ] B) .exact()
- [ ] C) .numeric()
- [ ] D) .fuzzy()

> [!success]- Answer
> **Correct Answer: B) .exact()**
> 
> Dates should match exactly for them to represent the same person. Using `.exact()` ensures that only identical dates are considered matches. String or fuzzy matching would be inappropriate for dates.

**Q30.** What does the label parameter do in comparison methods?

- [ ] A) Sets the column name in the input DataFrames
- [ ] B) Names the resulting comparison column in the output DataFrame
- [ ] C) Creates a new label column
- [ ] D) Filters by label

> [!success]- Answer
> **Correct Answer: B) Names the resulting comparison column in the output DataFrame**
> 
> The label parameter gives a name to the comparison result column. This makes the output more readable and helps identify which comparison produced which scores.
> 
> ```python
> compare_cl.exact('state', 'state', label='state')
> # Creates a column called 'state' in potential_matches
> ```

---

## Section B: True/False Questions (15 points)
*1 point each*

**Q31.** The Hamming distance algorithm can handle strings of different lengths. **T/F**

> [!success]- Answer
> **False** - Hamming distance only allows substitution operations, meaning both strings must be the same length. It cannot handle insertions or deletions that would be needed for different-length strings.

**Q32.** `fuzz.WRatio()` returns a value between 0 and 1. **T/F**

> [!success]- Answer
> **False** - `fuzz.WRatio()` returns a similarity score between 0 and 100, not 0 and 1. A score of 100 means identical strings, while 0 means completely different.

**Q33.** Blocking in record linkage reduces the number of comparisons needed. **T/F**

> [!success]- Answer
> **True** - Blocking is specifically designed to reduce computational complexity by only comparing records that share certain attributes (like the same state), dramatically reducing the number of pairs to evaluate.

**Q34.** The `process.extract()` function can only return exact matches. **T/F**

> [!success]- Answer
> **False** - `process.extract()` returns fuzzy matches based on similarity scores. It's designed to find approximate matches, not just exact ones.

**Q35.** A threshold of 0.85 in `.string()` comparison means strings must be exactly 85% identical. **T/F**

> [!success]- Answer
> **True** - A threshold of 0.85 (or 85%) means the similarity score must be at least 85% for the comparison to return a match (1). Below this threshold, it returns 0.

**Q36.** `matches.index.get_level_values(0)` returns the indices from the second DataFrame. **T/F**

> [!success]- Answer
> **False** - `get_level_values(0)` returns indices from the FIRST DataFrame (census_A). Level 1 would return indices from the second DataFrame (census_B).

**Q37.** Record linkage can only work with two DataFrames at a time. **T/F**

> [!success]- Answer
> **True** - The `recordlinkage` package's `index()` method takes exactly two DataFrames to compare. To link multiple DataFrames, you would need to perform multiple pairwise linkages.

**Q38.** The Levenshtein algorithm includes transposition as an allowed operation. **T/F**

> [!success]- Answer
> **False** - Levenshtein only allows insertion, substitution, and deletion. Transposition is included in Damerau-Levenshtein and Jaro distance, but not in standard Levenshtein.

**Q39.** The `.exact()` method returns a similarity score between 0 and 1. **T/F**

> [!success]- Answer
> **True** - `.exact()` returns 1 for an exact match and 0 for no match. This binary scoring (0 or 1) is technically a range between 0 and 1.

**Q40.** When collapsing categories, a higher similarity threshold (e.g., 95) will catch more variations. **T/F**

> [!success]- Answer
> **False** - A HIGHER threshold is more restrictive and will catch FEWER variations. A lower threshold (e.g., 70) would catch more variations but also more false positives. The recommended 80 provides a balance.

**Q41.** `pd.concat()` performs an inner join by default. **T/F**

> [!success]- Answer
> **False** - `pd.concat()` performs an outer join by default, keeping all rows from all DataFrames. Inner joins require the `join='inner'` parameter.

**Q42.** The `limit` parameter in `process.extract()` sets the minimum similarity score. **T/F**

> [!success]- Answer
> **False** - The `limit` parameter sets the NUMBER of matches to return, not the similarity threshold. It controls how many of the best matches you want back.

**Q43.** Blocking always improves the accuracy of record linkage. **T/F**

> [!success]- Answer
> **False** - Blocking improves PERFORMANCE but can reduce accuracy if the blocking key has errors. For example, blocking on 'state' would miss matches where the state is misspelled differently in each DataFrame.

**Q44.** The `census_B.index.isin(duplicate_rows)` method returns a boolean mask. **T/F**

> [!success]- Answer
> **True** - The `.isin()` method returns a boolean Series/mask with True for indices that are in duplicate_rows and False otherwise.

**Q45.** All comparison methods in recordlinkage return values between 0 and 1. **T/F**

> [!success]- Answer
> **True** - Both `.exact()` and `.string()` return scores of either 0 (no match) or 1 (match/above threshold). This 0-1 scoring is standard across comparison methods.

---

## Section C: Short Answer Questions (30 points)
*3 points each*

**Q46.** Explain the difference between the Levenshtein and Damerau-Levenshtein algorithms. When would you choose one over the other?

> [!success]- Answer
> **Levenshtein** allows three operations: insertion, substitution, and deletion.
> 
> **Damerau-Levenshtein** allows all Levenshtein operations PLUS transposition (swapping adjacent characters).
> 
> **When to choose:**
> - Use **Damerau-Levenshtein** when typos often involve transposed characters (e.g., "teh" vs "the", "Reeding" vs "Reading")
> - Use **Levenshtein** when transpositions are rare or you want a stricter distance metric
> - Damerau-Levenshtein is generally more appropriate for real-world text data as transposition is a common typo
> 
> **Example:** "house" → "hosue" has Damerau-Levenshtein distance of 1 (one transposition) but Levenshtein distance of 2 (one deletion + one insertion).

**Q47.** Describe the complete workflow for record linkage using the recordlinkage package. List all major steps in order.

> [!success]- Answer
> The complete record linkage workflow consists of 6 steps:
> 
> 1. **Create Indexer**: `indexer = recordlinkage.Index()`
> 2. **Set Blocking Rules**: `indexer.block('state')` to reduce comparisons
> 3. **Generate Pairs**: `pairs = indexer.index(census_A, census_B)`
> 4. **Create Comparison Object**: `compare_cl = recordlinkage.Compare()`
> 5. **Define Comparisons**: Add `.exact()` and `.string()` methods for relevant columns
> 6. **Compute Matches**: `potential_matches = compare_cl.compute(pairs, census_A, census_B)`
> 7. **Filter High-Quality Matches**: `matches = potential_matches[potential_matches.sum(axis=1) >= 3]`
> 8. **Extract Duplicate Indices**: `duplicate_rows = matches.index.get_level_values(1)`
> 9. **Separate New Records**: `census_B_new = census_B[~census_B.index.isin(duplicate_rows)]`
> 10. **Link DataFrames**: `full_census = pd.concat([census_A, census_B_new])`

**Q48.** What are the trade-offs when choosing a threshold value for string matching in record linkage? How does changing from 0.80 to 0.90 affect results?

> [!success]- Answer
> **Lower Threshold (0.80):**
> - **Pros**: Catches more variations and misspellings; fewer false negatives
> - **Cons**: More false positives (incorrect matches); less precision
> 
> **Higher Threshold (0.90):**
> - **Pros**: Higher precision; fewer false positives; more confident matches
> - **Cons**: Misses legitimate variations; more false negatives
> 
> **Changing from 0.80 to 0.90:**
> - Reduces the number of matches found
> - Increases confidence in remaining matches
> - May miss valid matches with minor typos
> - Example: "Smith" vs "Smyth" might match at 0.80 but not 0.90
> 
> **Best Practice**: Start with 0.80-0.85, then adjust based on your data quality and tolerance for false positives vs. false negatives.

**Q49.** Explain what the MultiIndex returned by `indexer.index()` represents and how to extract information from different levels.

> [!success]- Answer
> The MultiIndex contains pairs of record IDs to compare:
> - **Level 0**: Record IDs from the first DataFrame (census_A)
> - **Level 1**: Record IDs from the second DataFrame (census_B)
> 
> **Structure Example:**
> ```
> rec_id_1 (Level 0)    rec_id_2 (Level 1)
> rec-1070-org          rec-561-dup-0
> rec-1070-org          rec-2642-dup-0
> ```
> 
> **Extracting Information:**
> ```python
> # Get all census_A IDs
> census_A_ids = matches.index.get_level_values(0)
> 
> # Get all census_B IDs
> census_B_ids = matches.index.get_level_values(1)
> 
> # Get both as a list of tuples
> pair_tuples = list(matches.index)
> ```
> 
> This structure allows efficient representation of which records from each DataFrame should be compared or are potential matches.

**Q50.** Why is blocking important in record linkage? Give a specific example with numbers showing the computational difference.

> [!success]- Answer
> **Why Blocking Matters:**
> Blocking prevents the computational explosion of comparing every record with every other record (Cartesian product).
> 
> **Example with Numbers:**
> - **Scenario**: census_A has 1,000 records, census_B has 1,000 records
> 
> **Without Blocking:**
> - Comparisons needed: 1,000 × 1,000 = 1,000,000 comparisons
> 
> **With Blocking on 'state' (assume 50 states, ~20 records per state per DataFrame):**
> - Comparisons per state: 20 × 20 = 400
> - Total comparisons: 50 states × 400 = 20,000 comparisons
> 
> **Result**: 98% reduction in comparisons (from 1M to 20K)
> 
> **Trade-off**: Blocking assumes the blocking key (state) is accurate. If state values have typos, some true matches might be missed.

**Q51.** What is the purpose of the `label` parameter in comparison methods? How does it affect the output?

> [!success]- Answer
> **Purpose**: The `label` parameter names the output column in the comparison results DataFrame.
> 
> **Without labels**: Columns would have generic names or be difficult to interpret
> 
> **With labels**: Each comparison gets a meaningful column name
> 
> **Example:**
> ```python
> compare_cl.exact('date_of_birth', 'date_of_birth', label='dob_match')
> compare_cl.string('surname', 'surname', threshold=0.85, label='surname_match')
> ```
> 
> **Output DataFrame:**
> ```
>                     dob_match  surname_match
> rec_id_1    rec_id_2
> rec-100     rec-200      1          0.9
> ```
> 
> This makes results interpretable and helps track which comparison produced which score.

**Q52.** Explain how `potential_matches.sum(axis=1) >= 3` filters results. What does a sum of 3 indicate?

> [!success]- Answer
> **How it works:**
> `sum(axis=1)` adds up match scores across all comparison columns for each record pair.
> 
> **Scoring:**
> - Each comparison returns 0 (no match) or 1 (match)
> - Sum = total number of matching columns
> 
> **Example:**
> ```python
> # If potential_matches has 4 comparison columns:
>                 date_of_birth  state  surname  address_1  sum
> pair1                1          1       1        0      = 3
> pair2                0          1       0.9      0.8    = 2.7
> pair3                1          1       1        1      = 4
> ```
> 
> **A sum >= 3 indicates:**
> - At least 3 columns matched between the record pairs
> - High confidence this is a true match
> - Strong evidence that records refer to the same entity
> 
> **Why use 3?** It balances precision (avoiding false positives) with recall (catching true matches). Adjust based on number of columns compared and data quality.

**Q53.** Compare and contrast `census_B[census_B.index.isin(duplicate_rows)]` and `census_B[~census_B.index.isin(duplicate_rows)]`. What does each return?

> [!success]- Answer
> **First Expression (without ~):**
> ```python
> census_B_duplicates = census_B[census_B.index.isin(duplicate_rows)]
> ```
> - Returns records from census_B that ARE duplicates (already in census_A)
> - These are records we want to EXCLUDE from the final linked DataFrame
> - Useful for audit/verification purposes
> 
> **Second Expression (with ~):**
> ```python
> census_B_new = census_B[~census_B.index.isin(duplicate_rows)]
> ```
> - The `~` negates the boolean mask
> - Returns records that are NOT duplicates (new unique records)
> - These are records we want to ADD to census_A
> 
> **Example:**
> ```python
> # census_B has indices: ['rec-1', 'rec-2', 'rec-3', 'rec-4']
> # duplicate_rows = ['rec-2', 'rec-4']
> 
> # With .isin(): returns rec-2 and rec-4 (duplicates)
> # With ~.isin(): returns rec-1 and rec-3 (new records)
> ```
> 
> Together, they partition census_B into duplicates and new records.

**Q54.** When would you use `.exact()` versus `.string()` comparison in record linkage? Give 3 examples of fields for each.

> [!success]- Answer
> **Use `.exact()` for:**
> 
> 1. **Date fields** - Date of birth, transaction dates
>    - Dates should match exactly
>    - Example: `compare_cl.exact('date_of_birth', 'date_of_birth')`
> 
> 2. **Standardized codes** - State abbreviations, country codes, zip codes
>    - These have consistent formats
>    - Example: `compare_cl.exact('state', 'state')`
> 
> 3. **Numeric IDs** - Social Security Numbers, employee IDs (when available)
>    - Should be identical if they exist
>    - Example: `compare_cl.exact('ssn', 'ssn')`
> 
> **Use `.string()` for:**
> 
> 4. **Names** - First names, last names, full names
>    - Subject to typos and variations
>    - Example: `compare_cl.string('surname', 'surname', threshold=0.85)`
> 
> 5. **Addresses** - Street addresses, city names
>    - Many abbreviations and spelling variations
>    - Example: `compare_cl.string('address_1', 'address_1', threshold=0.85)`
> 
> 6. **Company names** - Organization names, business names
>    - Can have different orderings or abbreviations
>    - Example: `compare_cl.string('company', 'company', threshold=0.80)`
> 
> **Key Principle**: Use `.exact()` for structured data with expected consistency; use `.string()` for free-text fields prone to variations.

**Q55.** Describe a real-world scenario where record linkage would be necessary. Explain why a standard merge wouldn't work.

> [!success]- Answer
> **Scenario: Hospital Patient Records Integration**
> 
> Two hospitals merge and need to combine patient databases:
> 
> **Hospital A Database:**
> ```
> PatientID: A-1234
> Name: Robert J. Smith
> DOB: 03/15/1985
> Address: 123 Main St, Apt 4B
> ```
> 
> **Hospital B Database:**
> ```
> PatientID: B-5678
> Name: Bob Smith
> DOB: 1985-03-15
> Address: 123 Main Street #4B
> ```
> 
> **Why Standard Merge Fails:**
> 
> 1. **Different ID systems**: A-1234 vs B-5678 (no common key)
> 2. **Name variations**: "Robert J. Smith" vs "Bob Smith"
> 3. **Date format differences**: 03/15/1985 vs 1985-03-15
> 4. **Address inconsistencies**: "St" vs "Street", "Apt 4B" vs "#4B"
> 5. **No unique identifier** exists across both systems
> 
> **Record Linkage Solution:**
> ```python
> # Block on last 4 digits of DOB year
> indexer.block('dob_year')
> 
> # Use string matching for names and addresses
> compare_cl.string('name', 'name', threshold=0.85)
> compare_cl.string('address', 'address', threshold=0.80)
> 
> # Use exact match for normalized dates
> compare_cl.exact('dob_normalized', 'dob_normalized')
> ```
> 
> This identifies that both records refer to the same patient despite the inconsistencies.

---

## Section D: Code Analysis & Scenarios (25 points)
*2.5 points each*

**Q56.** What does this code do? Explain the output.

```python
from thefuzz import fuzz

result1 = fuzz.WRatio('Los Angeles Lakers', 'Lakers')
result2 = fuzz.WRatio('Lakers vs Rockets', 'Rockets vs Lakers')
print(result1, result2)
```

> [!success]- Answer
> **What it does:**
> This code performs partial string matching using the `thefuzz` library's `WRatio` function.
> 
> **Line by line:**
> 1. `result1`: Compares full team name with abbreviated version
> 2. `result2`: Compares same teams but in different order
> 3. Prints both similarity scores
> 
> **Expected Output:**
> ```
> 90 86
> ```
> 
> **Explanation:**
> - `result1 = 90`: High score because "Lakers" is a subset of "Los Angeles Lakers" (partial match)
> - `result2 = 86`: Slightly lower score because the order is different, but both teams are mentioned
> 
> **Key Insight**: `WRatio` handles partial strings and different word orderings well, making it ideal for matching variations of the same entity.

**Q57.** Find and fix ALL issues in this record linkage code:

```python
import recordlinkage

indexer = recordlinkage.Index()
indexer.block('city')
pairs = indexer.index(df_A, df_B)

compare = recordlinkage.Compare()
compare.exact('name', 'name', label='name')
compare.string('address', 'address', label='address')

matches = compare.compute(pairs, df_A, df_B)
duplicates = matches.index.get_level_values(0)
new_records = df_B[~df_B.index.isin(duplicates)]
final_df = pd.concat([df_A, new_records])
```

> [!success]- Answer
> **Issues Found:**
> 
> 1. ❌ Missing threshold in `.string()` method
> 2. ❌ No filtering of matches before extracting indices
> 3. ❌ Using wrong level (0 instead of 1) to get census_B indices
> 
> **Fixed Code:**
> ```python
> import recordlinkage
> import pandas as pd  # Also need to import pandas
> 
> indexer = recordlinkage.Index()
> indexer.block('city')
> pairs = indexer.index(df_A, df_B)
> 
> compare = recordlinkage.Compare()
> compare.exact('name', 'name', label='name')
> compare.string('address', 'address', threshold=0.85, label='address')  # FIX 1: Add threshold
> 
> potential_matches = compare.compute(pairs, df_A, df_B)
> 
> # FIX 2: Filter for high-quality matches
> matches = potential_matches[potential_matches.sum(axis=1) >= 2]
> 
> # FIX 3: Use level 1 to get df_B indices, not level 0
> duplicates = matches.index.get_level_values(1)
> 
> new_records = df_B[~df_B.index.isin(duplicates)]
> final_df = pd.concat([df_A, new_records])
> ```
> 
> **Why These Fixes:**
> - `.string()` requires a threshold parameter to determine match/no-match
> - Must filter potential_matches to get high-confidence matches
> - Level 0 has df_A indices; we need level 1 for df_B indices to identify duplicates

**Q58.** Complete this code to collapse state name variations in the survey DataFrame:

```python
from thefuzz import process

correct_states = ['California', 'Texas', 'New York']

for state in correct_states:
    matches = process.extract(state, survey['state'], limit=___)
    
    for potential_match in matches:
        if ___:
            survey.loc[___, 'state'] = state
```

> [!success]- Answer
> **Completed Code:**
> ```python
> from thefuzz import process
> 
> correct_states = ['California', 'Texas', 'New York']
> 
> for state in correct_states:
>     # Get all possible matches
>     matches = process.extract(state, survey['state'], limit=survey.shape[0])
>     
>     for potential_match in matches:
>         # Check if similarity score is >= 80
>         if potential_match[1] >= 80:
>             # Replace the typo with correct state name
>             survey.loc[survey['state'] == potential_match[0], 'state'] = state
> ```
> 
> **Explanation:**
> - `limit=survey.shape[0]`: Get all rows to check every possible match
> - `potential_match[1] >= 80`: Check if similarity score (second element of tuple) is at least 80
> - `survey['state'] == potential_match[0]`: Find rows where state equals the matched variation
> - Replace those rows with the correct state name
> 
> **Example**: If survey has "Cali", "Calefornia", "Californie", they all get replaced with "California"

**Q59.** Analyze this code output. What does it tell us about the matches found?

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
rec-104-org rec-204-dup    0        0      0.7       0.0
```

Which pairs would be selected with `sum(axis=1) >= 3`?

> [!success]- Answer
> **Analysis of Output:**
> 
> **Row-by-row sum calculation:**
> - `rec-100-org / rec-200-dup`: 1 + 1 + 0.9 + 0 = **2.9** ❌ (below 3)
> - `rec-101-org / rec-201-dup`: 1 + 1 + 1 + 1 = **4.0** ✅ (4 matches!)
> - `rec-102-org / rec-202-dup`: 0 + 1 + 0.85 + 0 = **1.85** ❌ (below 3)
> - `rec-103-org / rec-203-dup`: 1 + 0 + 1 + 0.9 = **2.9** ❌ (below 3)
> - `rec-104-org / rec-204-dup`: 0 + 0 + 0.7 + 0 = **0.7** ❌ (below 3)
> 
> **Selected Pairs:**
> Only `rec-101-org / rec-201-dup` would be selected (sum = 4.0)
> 
> **Interpretation:**
> - This pair has **perfect or near-perfect matches** across all 4 fields
> - Very high confidence that these records refer to the same person
> - Other pairs lack sufficient matching columns
> 
> **Key Insight:**
> - `rec-100-org` and `rec-103-org` came close (2.9) but didn't make the cutoff
> - If we wanted to catch these, we could lower threshold to `>= 2.5`
> - However, this increases risk of false positives

**Q60.** What's wrong with this blocking strategy? Suggest a better approach.

```python
indexer = recordlinkage.Index()
indexer.block('full_name')
pairs = indexer.index(customer_A, customer_B)
```

> [!success]- Answer
> **Problems with Blocking on 'full_name':**
> 
> 1. **Defeats the purpose**: If names have typos/variations, blocking on exact name match will MISS the very records we want to link
> 2. **Example failure**:
>    - "John Smith" in customer_A
>    - "Jon Smith" in customer_B
>    - These WON'T be compared because block values don't match exactly
> 3. **Too restrictive**: Only compares records with identical names, missing the messy data record linkage is designed to handle
> 
> **Better Approaches:**
> 
> **Option 1: Block on more stable field**
> ```python
> indexer.block('zip_code')
> # Zip codes less prone to typos, still reduces comparisons
> ```
> 
> **Option 2: Block on multiple fields**
> ```python
> indexer.block(['state', 'birth_year'])
> # Combines stable fields for better blocking
> ```
> 
> **Option 3: Block on first letter + year**
> ```python
> # Create blocking key in advance
> customer_A['block_key'] = customer_A['last_name'].str[0] + customer_A['birth_year'].astype(str)
> customer_B['block_key'] = customer_B['last_name'].str[0] + customer_B['birth_year'].astype(str)
> indexer.block('block_key')
> ```
> 
> **Best Practice**: Block on fields that are:
> - Standardized (less prone to variation)
> - High quality (few missing values)
> - Sufficient cardinality (not too many records per block)
> - NOT the primary matching fields

**Q61.** Debug this code. Why doesn't it produce the expected full census?

```python
matches = potential_matches[potential_matches.sum(axis=1) >= 2]
duplicate_rows = matches.index.get_level_values(1)
census_B_new = census_B[census_B.index.isin(duplicate_rows)]
full_census = pd.concat([census_A, census_B_new])
```

> [!success]- Answer
> **The Bug:**
> Line 3 has the logic reversed! It's selecting duplicates instead of new records.
> 
> **Current (Wrong) Logic:**
> ```python
> census_B_new = census_B[census_B.index.isin(duplicate_rows)]
> # This gets records that ARE in duplicate_rows (the duplicates!)
> ```
> 
> **Fixed Code:**
> ```python
> matches = potential_matches[potential_matches.sum(axis=1) >= 2]
> duplicate_rows = matches.index.get_level_values(1)
> census_B_new = census_B[~census_B.index.isin(duplicate_rows)]  # Add ~ to negate!
> full_census = pd.concat([census_A, census_B_new])
> ```
> 
> **Why This Matters:**
> - **Without `~`**: Adds duplicates to census_A → creates duplicate records in full_census
> - **With `~`**: Adds only new unique records → proper deduplicated census
> 
> **Result of Bug:**
> Instead of:
> ```
> full_census = census_A rows + new census_B rows
> ```
> 
> You get:
> ```
> full_census = census_A rows + duplicate census_B rows (already in A!)
> ```
> 
> This creates a census with duplicate people!

**Q62.** Scenario: You're linking product databases from two e-commerce sites. Products have fields: product_name, brand, price, category. Design the complete record linkage approach.

> [!success]- Answer
> **Complete Record Linkage Design:**
> 
> ```python
> import recordlinkage
> import pandas as pd
> 
> # STEP 1: Blocking Strategy
> indexer = recordlinkage.Index()
> indexer.block('category')  # Block on category - stable and reduces comparisons
> pairs = indexer.index(products_A, products_B)
> 
> # STEP 2: Comparison Strategy
> compare_cl = recordlinkage.Compare()
> 
> # Exact match for structured fields
> compare_cl.exact('category', 'category', label='category')
> compare_cl.exact('brand', 'brand', label='brand')
> 
> # String matching for product names (prone to variations)
> compare_cl.string('product_name', 'product_name', 
>                   threshold=0.85, label='product_name')
> 
> # Numeric comparison for prices (within 5% tolerance)
> compare_cl.numeric('price', 'price', method='step', 
>                    offset=0.05, label='price')
> 
> potential_matches = compare_cl.compute(pairs, products_A, products_B)
> 
> # STEP 3: Filter for high-quality matches
> # Require at least 3 out of 4 fields to match
> matches = potential_matches[potential_matches.sum(axis=1) >= 3]
> 
> # STEP 4: Extract and link
> duplicate_rows = matches.index.get_level_values(1)
> products_B_new = products_B[~products_B.index.isin(duplicate_rows)]
> full_products = pd.concat([products_A, products_B_new])
> ```
> 
> **Design Rationale:**
> - **Block on category**: Reduces comparisons, categories are standardized
> - **Exact for brand/category**: These should match exactly
> - **String for product_name**: Handles "iPhone 13 Pro" vs "Apple iPhone 13 Pro"
> - **Numeric for price**: Allows small differences in pricing
> - **Threshold of 3**: With 4 comparisons, requiring 3 matches ensures high confidence
> 
> **Alternative Considerations:**
> - If brands have variations (e.g., "Nike" vs "NIKE Inc"), use `.string()` for brand too
> - Could add `.string()` for product descriptions if available
> - Consider blocking on brand+category for even faster processing

**Q63.** Write code to find all records in census_B that matched with multiple records in census_A (indicating potential data quality issues).

> [!success]- Answer
> **Code to Find Multiple Matches:**
> 
> ```python
> # After computing matches
> matches = potential_matches[potential_matches.sum(axis=1) >= 3]
> 
> # Get census_B indices (level 1)
> census_B_indices = matches.index.get_level_values(1)
> 
> # Count how many times each census_B record appears
> match_counts = census_B_indices.value_counts()
> 
> # Find records that matched multiple times
> multiple_matches = match_counts[match_counts > 1]
> 
> print("Records with multiple matches:")
> print(multiple_matches)
> 
> # Get the actual records from census_B
> problematic_records = census_B[census_B.index.isin(multiple_matches.index)]
> 
> # For detailed analysis, get all their match pairs
> for rec_id in multiple_matches.index:
>     print(f"\n{rec_id} matched with:")
>     # Get all census_A records this matched with
>     matched_with = matches[matches.index.get_level_values(1) == rec_id]
>     census_A_matches = matched_with.index.get_level_values(0)
>     print(census_A[census_A.index.isin(census_A_matches)])
> ```
> 
> **What This Reveals:**
> - **Multiple matches** suggest:
>   - Duplicate records in census_A
>   - Ambiguous/generic data in census_B record
>   - Threshold too low (catching false positives)
>   - Data quality issues
> 
> **Example Output:**
> ```
> Records with multiple matches:
> rec-500-dup    3
> rec-601-dup    2
> ```
> 
> This shows rec-500-dup matched with 3 different census_A records - a red flag for investigation!

**Q64.** Complete this code to create a summary report of the record linkage results:

```python
# After linking
full_census = pd.concat([census_A, census_B_new])

print("Record Linkage Summary Report")
print("="*50)
print(f"Original census_A records: {___}")
print(f"Original census_B records: {___}")
print(f"Duplicate records found: {___}")
print(f"New unique records added: {___}")
print(f"Final combined census: {___}")
print(f"Duplicate rate: {___:.2%}")
```

> [!success]- Answer
> **Completed Summary Report Code:**
> 
> ```python
> # After linking
> full_census = pd.concat([census_A, census_B_new])
> 
> # Calculate metrics
> census_A_count = len(census_A)
> census_B_count = len(census_B)
> duplicates_count = len(duplicate_rows)
> new_records_count = len(census_B_new)
> final_count = len(full_census)
> duplicate_rate = duplicates_count / census_B_count
> 
> print("Record Linkage Summary Report")
> print("="*50)
> print(f"Original census_A records: {census_A_count}")
> print(f"Original census_B records: {census_B_count}")
> print(f"Duplicate records found: {duplicates_count}")
> print(f"New unique records added: {new_records_count}")
> print(f"Final combined census: {final_count}")
> print(f"Duplicate rate: {duplicate_rate:.2%}")
> print(f"\nVerification: {census_A_count} + {new_records_count} = {final_count}")
> ```
> 
> **Example Output:**
> ```
> Record Linkage Summary Report
> ==================================================
> Original census_A records: 5000
> Original census_B records: 3000
> Duplicate records found: 800
> New unique records added: 2200
> Final combined census: 7200
> Duplicate rate: 26.67%
> 
> Verification: 5000 + 2200 = 7200
> ```
> 
> **Additional Useful Metrics:**
> ```python
> # Quality metrics
> print(f"\nMatch Quality:")
> print(f"Average match score: {matches.sum(axis=1).mean():.2f}")
> print(f"Match score distribution:")
> print(matches.sum(axis=1).value_counts().sort_index())
> ```

**Q65.** Scenario: After running record linkage, you notice the duplicate_rate is 95%. What could be wrong, and how would you investigate?

> [!success]- Answer
> **Possible Issues with 95% Duplicate Rate:**
> 
> 1. **Threshold too low**
>    - Catching false positives
>    - Almost everything matches
> 
> 2. **census_B is actually a copy** of census_A with minor changes
>    - Expected behavior if datasets overlap heavily
> 
> 3. **Blocking key too broad**
>    - Comparing too many records
>    - E.g., blocking on only one value
> 
> 4. **Comparison criteria too lenient**
>    - String thresholds too low (e.g., 0.5)
>    - Match threshold too low (sum >= 1)
> 
> **Investigation Code:**
> 
> ```python
> # 1. Check match score distribution
> print("Match Scores Distribution:")
> print(matches.sum(axis=1).value_counts().sort_index())
> 
> # 2. Sample some "matches" to verify
> sample_matches = matches.sample(10)
> for idx in sample_matches.index:
>     rec_A_id = idx[0]
>     rec_B_id = idx[1]
>     print(f"\nMatch Pair:")
>     print("Census A:")
>     print(census_A.loc[rec_A_id])
>     print("Census B:")
>     print(census_B.loc[rec_B_id])
>     print(f"Match scores: {matches.loc[idx].values}")
> 
> # 3. Check if datasets are identical
> # Try direct comparison on a sample
> common_cols = list(set(census_A.columns) & set(census_B.columns))
> print(f"\nDirect comparison sample:")
> print(census_A[common_cols].head())
> print(census_B[common_cols].head())
> 
> # 4. Review thresholds
> print(f"\nCurrent string threshold: {0.85}")  # Your threshold
> print(f"Current match threshold: sum >= {3}")  # Your threshold
> ```
> 
> **Solutions:**
> 
> ```python
> # Solution 1: Increase string similarity threshold
> compare_cl.string('surname', 'surname', threshold=0.90, label='surname')
> 
> # Solution 2: Require more matching columns
> matches = potential_matches[potential_matches.sum(axis=1) >= 4]
> 
> # Solution 3: Add more discriminating comparisons
> compare_cl.string('middle_name', 'middle_name', threshold=0.85, label='middle')
> compare_cl.numeric('age', 'age', method='step', offset=0, label='age')
> 
> # Solution 4: Check if census_B is actually a subset/update
> # If so, 95% duplicates might be correct!
> ```
> 
> **Key Takeaway**: High duplicate rates aren't always errors—verify with sample inspection before adjusting thresholds.

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)
1. B (2pts)
2. C (2pts)
3. C (2pts)
4. C (2pts)
5. C (2pts)
6. B (2pts)
7. C (2pts)
8. C (2pts)
9. B (2pts)
10. B (2pts)
11. B (2pts)
12. B (2pts)
13. B (2pts)
14. B (2pts)
15. B (2pts)
16. B (2pts)
17. C (2pts)
18. B (2pts)
19. B (2pts)
20. B (2pts)
21. B (2pts)
22. B (2pts)
23. B (2pts)
24. B (2pts)
25. B (2pts)
26. B (2pts)
27. B (2pts)
28. B (2pts)
29. B (2pts)
30. B (2pts)

### Section B: True/False (15 points)
31. False (1pt)
32. False (1pt)
33. True (1pt)
34. False (1pt)
35. True (1pt)
36. False (1pt)
37. True (1pt)
38. False (1pt)
39. True (1pt)
40. False (1pt)
41. False (1pt)
42. False (1pt)
43. False (1pt)
44. True (1pt)
45. True (1pt)

### Section C: Short Answer (30 points)
46-55: 3 points each - See detailed answers above

### Section D: Code Analysis (25 points)
56-65: 2.5 points each - See detailed answers above

**Total: 130 points**
**Passing Score: 91 points (70%)**

---

## Quick Study Guide

### Essential Code Patterns

**String Similarity:**
```python
from thefuzz import fuzz, process

# Simple comparison
score = fuzz.WRatio('string1', 'string2')

# Array comparison
matches = process.extract(target, choices, limit=n)
# Returns: [(match, score, index), ...]
```

**Record Linkage Workflow:**
```python
import recordlinkage

# 1. Create indexer and block
indexer = recordlinkage.Index()
indexer.block('column_name')
pairs = indexer.index(df1, df2)

# 2. Compare columns
compare = recordlinkage.Compare()
compare.exact('col1', 'col1', label='label1')
compare.string('col2', 'col2', threshold=0.85, label='label2')
potential_matches = compare.compute(pairs, df1, df2)

# 3. Filter and link
matches = potential_matches[potential_matches.sum(axis=1) >= 3]
duplicates = matches.index.get_level_values(1)
df2_new = df2[~df2.index.isin(duplicates)]
result = pd.concat([df1, df2_new])
```

---

## Key Concepts Summary

### String Matching Algorithms

| Algorithm | Operations | Use Case |
|-----------|------------|----------|
| Damerau-Levenshtein | Insert, substitute, delete, transpose | General text with typos |
| Levenshtein | Insert, substitute, delete | Standard text matching |
| Hamming | Substitute only | Same-length strings |
| Jaro distance | Transpose only | Short strings |

### recordlinkage Methods

| Method | Purpose | Returns |
|--------|---------|---------|
| `.exact()` | Exact field matching | 1 or 0 |
| `.string()` | Fuzzy string matching | 1 or 0 (based on threshold) |
| `.numeric()` | Numeric comparison | Similarity score |
| `.date()` | Date comparison | Similarity score |

### Key Python Operations

| Operation | Purpose |
|-----------|---------|
| `matches.index.get_level_values(0)` | Get DataFrame A indices |
| `matches.index.get_level_values(1)` | Get DataFrame B indices |
| `df.index.isin(values)` | Boolean mask for matching indices |
| `~df.index.isin(values)` | Boolean mask for NON-matching indices |
| `potential_matches.sum(axis=1)` | Sum match scores across columns |

---

## Common Mistakes to Avoid

❌ **Forgetting threshold in `.string()`** - Will cause an error
```python
compare.string('name', 'name', label='name')  # Missing threshold!
```

❌ **Using wrong MultiIndex level** - Gets wrong DataFrame's indices
```python
duplicates = matches.index.get_level_values(0)  # Wrong! Use level 1
```

❌ **Forgetting the `~` negation** - Adds duplicates instead of new records
```python
df_new = df_B[df_B.index.isin(duplicates)]  # Wrong! Add ~ before df_B
```

❌ **Not filtering potential_matches** - Includes low-quality matches
```python
duplicates = potential_matches.index.get_level_values(1)  # Should filter first!
```

❌ **Blocking on fuzzy fields** - Defeats purpose of fuzzy matching
```python
indexer.block('full_name')  # Bad! Names have variations
```

❌ **Setting threshold too low or too high**
- Too low (0.50): Many false positives
- Too high (0.95): Misses valid matches

❌ **Not using blocking at all** - Computational explosion
```python
pairs = indexer.index(df_A, df_B)  # Without block() = very slow!
```

❌ **Forgetting to import pandas for concat**
```python
pd.concat([df_A, df_B_new])  # Need: import pandas as pd
```

---

## Study Strategy

### Day 1-2: String Matching Fundamentals
- ✅ Understand minimum edit distance concept
- ✅ Learn the four main algorithms and their differences
- ✅ Practice `fuzz.WRatio()` and `process.extract()`
- ✅ Code category collapsing examples

### Day 3-4: Record Linkage Basics
- ✅ Understand when joins fail and why record linkage is needed
- ✅ Learn the `recordlinkage` package structure
- ✅ Master blocking concepts and implementation
- ✅ Practice generating pairs with different blocking strategies

### Day 5-6: Comparing and Linking
- ✅ Learn `.exact()` vs `.string()` comparison methods
- ✅ Understand MultiIndex structure and navigation
- ✅ Practice filtering matches with `sum(axis=1)`
- ✅ Master the complete linking workflow
- ✅ Understand `.get_level_values()` and `.isin()`

### Day 7: Integration and Practice
- ✅ Complete full record linkage examples end-to-end
- ✅ Practice debugging common errors
- ✅ Work through scenario-based problems
- ✅ Review all code patterns and key concepts
- ✅ Take practice quiz

---

## Final Exam Checklist

### Before the Exam:
- [ ] Review all four string matching algorithms
- [ ] Memorize typical threshold values (0.80-0.85 for strings, >= 3 for matches)
- [ ] Practice the 10-step record linkage workflow
- [ ] Understand MultiIndex levels (0 = first DF, 1 = second DF)
- [ ] Know when to use `.exact()` vs `.string()`
- [ ] Remember the `~` negation operator for selecting new records

### During the Exam:
- [ ] Read each question carefully - watch for "NOT" questions
- [ ] For code analysis, trace through line by line
- [ ] Check for common mistakes (missing threshold, wrong level, missing ~)
- [ ] For scenarios, think about the complete workflow
- [ ] In debugging questions, check EVERY line
- [ ] Use process of elimination on multiple choice

### Key Formulas to Remember:
```python
# Category collapsing threshold
if similarity_score >= 80: replace

# Match filtering
matches = potential_matches[potential_matches.sum(axis=1) >= 3]

# Get duplicates from second DF
duplicates = matches.index.get_level_values(1)

# Get new records (note the ~!)
new_records = df_B[~df_B.index.isin(duplicates)]

# Final link
result = pd.concat([df_A, new_records])
```

---

## Quick Reference: thefuzz vs recordlinkage

### Use `thefuzz` when:
- ✅ Collapsing categories with variations
- ✅ Simple string matching tasks
- ✅ Finding best match from a list
- ✅ You don't need full record linkage workflow

### Use `recordlinkage` when:
- ✅ Linking entire DataFrames
- ✅ Multiple columns need comparison
- ✅ Need systematic blocking strategy
- ✅ Want structured match scores
- ✅ Handling complex deduplication

---

#python #data-cleaning #record-linkage #string-matching #thefuzz #recordlinkage #fuzzy-matching #datacamp #certification #exam-prep