
## 📚 Chapter Topics

- Strings and string methods
- Lists and indexing
- Dictionaries and key-value pairs
- Sets and tuples

---

## 📝 Strings

### What are Strings?

**Strings** are text data used everywhere:

- User profiles
- Search engines
- Large language models
- Text processing

### Creating Strings

**Python accepts both single and double quotes:**

```python
# Single quotes (apostrophe)
my_text = 'Hello, my name is George.'

# Double quotes
my_text = "Hello, my name is George."
```

**Both work equally!**

### Advantage of Double Quotes

**Problem with single quotes:**

```python
# Error - apostrophe conflicts with string delimiter
my_text = 'Hello, my name's George.'
# SyntaxError: invalid syntax
```

**Solution - use double quotes:**

```python
# Works! Apostrophe inside double-quoted string
my_text = "Hello, my name's George."
print(my_text)
# Output: Hello, my name's George.
```

---

## 🔧 String Methods

### What is a Method?

**Method** = A function that is only available to a specific data type

**Syntax:** `str_variable.method()`

### Common String Methods

#### 1. replace() - Replace Text

**Syntax:** `.replace(text_to_be_replaced, text_to_change_it_to)`

```python
my_text = "Hello, my name's George."

# Replace George with John
my_text = my_text.replace("George", "John")
print(my_text)
# Output: Hello, my name's John.
```

**Use Cases:**

- Reformatting (e.g., spaces to underscores)
- Fixing typos
- Removing unwanted text

#### 2. lower() - Convert to Lowercase

```python
current_top_album = "For All The Dogs"

# Convert to lowercase
current_top_album = current_top_album.lower()
print(current_top_album)
# Output: for all the dogs
```

#### 3. upper() - Convert to Uppercase

```python
current_top_album = "For All The Dogs"

# Convert to uppercase
current_top_album = current_top_album.upper()
print(current_top_album)
# Output: FOR ALL THE DOGS
```

### Multi-line Strings

**Use triple quotes for multi-line text:**

```python
# Create multi-line string
harry_potter = """Mr. and Mrs. Dursley,
of number four Privet Drive,
were proud to say that they were perfectly normal,
thank you very much.
"""
```

**Syntax:** `"""text"""`

**Benefits:**

- Enhance readability
- Avoid special characters
- Perfect for longer text (reviews, descriptions)
- Used for function documentation (docstrings)

---

## 📋 String Cheat Sheet

|Syntax|Purpose|Example|
|---|---|---|
|`' '`|String variable|`my_string = 'text'`|
|`" "`|String variable|`my_string = "text"`|
|`""" """`|Multi-line string|`my_string = """line1\nline2"""`|
|`.replace()`|Replace text|`str.replace("old", "new")`|
|`.lower()`|Lowercase|`str.lower()`|
|`.upper()`|Uppercase|`str.upper()`|

---

## 📚 Lists

### The Problem

**Too many individual variables:**

```python
price_one = 10
price_two = 20
price_three = 30
price_four = 15
price_five = 25
price_six = 35
```

### The Solution: Lists

**List** = Store multiple values in a single variable

```python
# List of prices
prices = [10, 20, 30, 15, 25, 35]
```

**Key Features:**

- Can contain **any combination of data types** (str, float, int, bool)
- Uses square brackets `[]`
- Values separated by commas

### Creating Lists

```python
# Direct values
prices = [10, 20, 30, 15, 25, 35]

# Using variables
prices = [price_one, price_two, price_three,
          price_four, price_five, price_six]
```

### Checking List Type

```python
type(prices)
# Output: <class 'list'>
```

---

## 🔢 Accessing List Elements

### Key Concepts

**Lists are ordered** - Each element has a position (index)

**Python uses zero-indexing** - First element is at index 0

### Indexing

**Syntax:** `list_name[index]`

```python
prices = [10, 20, 30, 15, 25, 35]

# First element (index 0)
prices[0]  # 10

# Fourth element (index 3)
prices[3]  # 15
```

### Index Visualization

```
prices = [10, 20, 30, 15, 25, 35]
Index:    0   1   2   3   4   5
```

### Negative Indexing

**Access from the end:**

```python
prices = [10, 20, 30, 15, 25, 35]

# Last element
prices[-1]  # 35
prices[5]   # 35 (same result)
```

---

## ✂️ List Slicing

### Accessing Multiple Elements

**Syntax:** `list[start:stop]`

**Important:** Returns up to but **not including** stop index

```python
prices = [10, 20, 30, 15, 25, 35]

# Second and third elements (indices 1 and 2)
prices[1:3]  # [20, 30]
```

**Formula:** `[starting_element : last_element + 1]`

### Slicing Shortcuts

```python
prices = [10, 20, 30, 15, 25, 35]

# From fourth index onwards
prices[3:]  # [15, 25, 35]

# First three elements
prices[:3]  # [10, 20, 30]
```

### Step Slicing

**Syntax:** `list[start:stop:step]`

```python
prices = [10, 20, 30, 15, 25, 35]

# Every other element
prices[::2]  # [10, 30, 25]

# Every third element, starting at second
prices[1::3]  # [20, 25]
```

---

## 📊 Lists Cheat Sheet

|Syntax|Functionality|
|---|---|
|`a_list = [1, 2, 3, 4, 5, 6]`|Create list|
|`a_list[0]`|First element (index 0)|
|`a_list[-1]`|Last element|
|`a_list[0:3]`|First, second, third elements|
|`a_list[:3]`|First three elements|
|`a_list[2:]`|From third index onwards|
|`a_list[::2]`|Every other element|

---

## 🗂️ Dictionaries

### The Problem

**Two separate lists are hard to manage:**

```python
prices = [10, 20, 30, 15, 25, 35]
product_ids = ["AG32", "HT91", "PL65", "OS31", "KB07", "TR48"]
```

How to link product IDs to prices?

### The Solution: Dictionaries

**Dictionary** = Collection of key-value pairs

**Real-world analogy:** Like a real dictionary (word → definition)

**Use cases:**

- user_id → user data
- order_number → order details
- product_id → price

### Creating a Dictionary

**Syntax:** `{key: value, key: value, ...}`

```python
# Creating dictionary
products_dict = {"AG32": 10, "HT91": 20,
                 "PL65": 30, "OS31": 15,
                 "KB07": 25, "TR48": 35}
```

**Key Features:**

- Uses curly braces `{}`
- Key-value pairs separated by `:`
- Pairs separated by commas

---

## 🔍 Working with Dictionaries

### Accessing Values by Key

**Dictionaries are ordered** (Python 3.7+)

**Syntax:** `dict_name[key]`

```python
# Get price for product "AG32"
products_dict["AG32"]  # 10
```

### Dictionary Methods

#### Get All Values

```python
products_dict.values()
# Output: dict_values([10, 20, 30, 15, 25, 35])
```

#### Get All Keys

```python
products_dict.keys()
# Output: dict_keys(['AG32', 'HT91', 'PL65', 'OS31', 'KB07', 'TR48'])
```

#### Get All Items (Key-Value Pairs)

```python
products_dict.items()
# Output: dict_items([('AG32', 10), ('HT91', 20), ...])
```

**Note:** `.items()` is useful for iteration/looping

#### Print Entire Dictionary

```python
print(products_dict)
# Output: {'AG32': 10, 'HT91': 20, 'PL65': 30, ...}
```

---

## ✏️ Modifying Dictionaries

### Adding New Key-Value Pair

```python
# Add new product
products_dict["UI56"] = 40
```

### Updating Existing Value

```python
# Update price for existing product
products_dict["HT91"] = 12
```

### Duplicate Keys

**Last value wins!**

```python
# Dictionary with duplicate key
products_dict = {"AG32": 10, "AG32": 20,
                 "PL65": 30}

print(products_dict["AG32"])
# Output: 20  (latest value)
```

---

## 🎯 Sets

### What are Sets?

**Set** = Collection of unique values

**Key Characteristics:**

- Contain **unique data** (no duplicates)
- **Unchangeable*** - Can add/remove but not change values
- **Unordered** - No index
- Quick to search

**Use Cases:**

- Identify and remove duplicates
- Fast membership testing

### Creating a Set

**Syntax:** `{value1, value2, ...}` (no colons!)

```python
# Create set (notice duplicate "John Smith")
attendees_set = {"John Smith", "Alan Jones", "Roger Thompson",
                 "John Smith", "Brandon Sharp", "Sam Washington"}

print(attendees_set)
# Output: {'Alan Jones', 'Brandon Sharp', 'John Smith',
#          'Roger Thompson', 'Sam Washington'}
```

**Duplicate removed automatically!**

### Converting to Set

```python
# Existing list with duplicates
attendees_list = ["John Smith", "Alan Jones", "Roger Thompson",
                  "John Smith", "Brandon Sharp", "Sam Washington"]

# Convert to set (removes duplicates)
attendees_set = set(attendees_list)

print(attendees_set)
# Output: {'Sam Washington', 'Roger Thompson', 'Alan Jones',
#          'John Smith', 'Brandon Sharp'}
```

### Limitations of Sets

❌ **No indexing** - Can't use `[]` to access elements

```python
# This causes an error
attendees_set[0]
# TypeError: 'set' object is not subscriptable
```

❌ **Can't have duplicates** ❌ **Can't subset with brackets**

### Sorting a Set

```python
# Sort a set (returns a list!)
sorted(attendees_set)
# Output: ['Alan Jones', 'Brandon Sharp', 'John Smith',
#          'Roger Thompson', 'Sam Washington']
```

**Note:** `sorted()` returns a **list**, not a set

---

## 📦 Tuples

### What are Tuples?

**Tuple** = Immutable ordered collection

**Key Characteristics:**

- **Immutable** - Cannot be changed
    - ❌ No adding values
    - ❌ No removing values
    - ❌ No changing values
- **Ordered** - Has index positions
- ✅ Can subset by index `[0]`

**Use Cases:**

- Data that shouldn't change
- Coordinates (x, y)
- RGB colors (r, g, b)
- Database records

### Creating a Tuple

**Syntax:** `(value1, value2, ...)`

```python
# Creating tuple
office_locations = ("New York City", "London", "Leuven")

# Convert from another structure
attendees = tuple(attendees_list)
```

### Accessing Tuples

```python
# Access second element
office_locations[1]
# Output: "London"
```

---

## 📊 Data Structures Summary

|Data Structure|Syntax|Immutable|Allow Duplicates|Ordered|Subset with []|
|---|---|---|---|---|---|
|**List**|`[1, 2, 3]`|No|Yes|Yes|Yes (index)|
|**Dictionary**|`{key:value}`|No|Yes|Yes|Yes (key)|
|**Set**|`{1, 2, 3}`|No|No|No|No|
|**Tuple**|`(1, 2, 3)`|Yes|Yes|Yes|Yes (index)|

---

## 🎯 When to Use Each

### Use Lists When:

- Order matters
- Need to change values
- Allow duplicates
- Need indexing

### Use Dictionaries When:

- Need key-value pairs
- Quick lookups by key
- Storing related data

### Use Sets When:

- Need unique values only
- Remove duplicates
- Fast membership testing
- Order doesn't matter

### Use Tuples When:

- Data shouldn't change
- Returning multiple values from function
- Dictionary keys (must be immutable)
- Coordinates or fixed data

---

## 💡 Key Concepts

### Dictionary vs Set Syntax

```python
# Dictionary - has key:value pairs
my_dict = {"key": "value"}

# Set - just values, no colons
my_set = {"value1", "value2"}
```

### Mutability

**Mutable (can change):**

- Lists
- Dictionaries
- Sets

**Immutable (cannot change):**

- Tuples
- Strings
- Numbers

### Zero-Indexing

```python
my_list = ["a", "b", "c"]
#         0    1    2     ← indices
```

---

## 📝 Quick Reference

### Strings

```python
"text"              # String
"""multi-line"""    # Multi-line
str.replace()       # Replace
str.lower()         # Lowercase
str.upper()         # Uppercase
```

### Lists

```python
[1, 2, 3]          # Create
list[0]            # First
list[-1]           # Last
list[1:3]          # Slice
```

### Dictionaries

```python
{"key": value}     # Create
dict[key]          # Access
dict.keys()        # All keys
dict.values()      # All values
dict.items()       # All pairs
```

### Sets

```python
{1, 2, 3}          # Create
set(list)          # Convert
sorted(set)        # Sort (→ list)
```

### Tuples

```python
(1, 2, 3)          # Create
tuple[0]           # Access
tuple(list)        # Convert
```

---

#python #strings #lists #dictionaries #sets #tuples #data-structures #datacamp

## Section A: Multiple Choice (20 Questions)

**Q1.** What is a method in Python?

- [ ] A) Any function
- [x] B) A function only available to a specific data type
- [ ] C) A variable
- [ ] D) A comment

**Q2.** Which quote type allows apostrophes inside strings?

- [ ] A) Single quotes only
- [x] B) Double quotes
- [ ] C) Triple quotes only
- [ ] D) Backticks

**Q3.** What does `.lower()` do?

- [ ] A) Deletes lowercase letters
- [x] B) Converts string to lowercase
- [ ] C) Counts lowercase letters
- [ ] D) Removes spaces

**Q4.** What data structure stores multiple values in a single variable?

- [ ] A) Integer
- [ ] B) String
- [x] C) List
- [ ] D) Boolean

**Q5.** What is Python's first index number?

- [ ] A) 1
- [x] B) 0
- [ ] C) -1
- [ ] D) null

**Q6.** How do you access the last element of a list?

- [ ] A) `list[last]`
- [x] B) `list[-1]`
- [ ] C) `list[end]`
- [ ] D) `list.last()`

**Q7.** What does `my_list[1:3]` return?

- [ ] A) Elements at indices 1, 2, and 3
- [x] B) Elements at indices 1 and 2
- [ ] C) Elements at indices 1 and 3
- [ ] D) Element at index 1

**Q8.** What is a dictionary?

- [ ] A) A list of words
- [x] B) Collection of key-value pairs
- [ ] C) Ordered list
- [ ] D) Set of unique values

**Q9.** What syntax creates a dictionary?

- [ ] A) `[key: value]`
- [x] B) `{key: value}`
- [ ] C) `(key: value)`
- [ ] D) `key = value`

**Q10.** How do you access a dictionary value?

- [ ] A) `dict(key)`
- [ ] B) `dict.key`
- [x] C) `dict[key]`
- [ ] D) `dict->key`

**Q11.** What makes sets unique?

- [ ] A) They're sorted
- [x] B) They contain only unique values
- [ ] C) They're immutable
- [ ] D) They're indexed

**Q12.** What syntax differentiates a set from a dictionary?

- [ ] A) Sets use ()
- [ ] B) Sets use []
- [x] C) Sets use {} without colons
- [ ] D) No difference

**Q13.** What does `set()` do to a list with duplicates?

- [ ] A) Returns error
- [x] B) Removes duplicates
- [ ] C) Sorts the list
- [ ] D) Counts duplicates

**Q14.** What is immutable about tuples?

- [ ] A) Can't access elements
- [x] B) Can't change, add, or remove values
- [ ] C) Can't create them
- [ ] D) Can't print them

**Q15.** What syntax creates a tuple?

- [ ] A) `[1, 2, 3]`
- [ ] B) `{1, 2, 3}`
- [x] C) `(1, 2, 3)`
- [ ] D) `<1, 2, 3>`

**Q16.** What happens with duplicate keys in a dictionary?

- [ ] A) Error occurs
- [ ] B) Both values are stored
- [x] C) Last value wins
- [ ] D) First value wins

**Q17.** What does `sorted()` return when applied to a set?

- [ ] A) Sorted set
- [x] B) Sorted list
- [ ] C) Sorted tuple
- [ ] D) Error

**Q18.** Can you subset a set with `[]`?

- [ ] A) Yes
- [x] B) No
- [ ] C) Only if sorted
- [ ] D) Only with numbers

**Q19.** What are triple quotes used for?

- [ ] A) Comments
- [ ] B) Variables
- [x] C) Multi-line strings
- [ ] D) Lists

**Q20.** Which data structure is ordered and allows duplicates but is immutable?

- [ ] A) List
- [ ] B) Set
- [ ] C) Dictionary
- [x] D) Tuple

---

## Section B: True/False (10 Questions)

**Q21.** Single and double quotes both create strings in Python. **T/F**

**Q22.** Lists are zero-indexed in Python. **T/F**

**Q23.** Sets allow duplicate values. **T/F**

**Q24.** Dictionaries use curly braces `{}`. **T/F**

**Q25.** Tuples can be modified after creation. **T/F**

**Q26.** `list[-1]` accesses the last element. **T/F**

**Q27.** Sets have index positions. **T/F**

**Q28.** `.upper()` converts a string to uppercase. **T/F**

**Q29.** Lists can contain mixed data types. **T/F**

**Q30.** Dictionaries are unordered in Python 3.7+. **T/F**

---

## Section C: Code Analysis (10 Questions)

**Q31.** What is the output?

```python
text = "Hello"
print(text.upper())
```

- [ ] A) hello
- [x] B) HELLO
- [ ] C) Hello
- [ ] D) Error

**Q32.** What is the output?

```python
my_list = [10, 20, 30, 40]
print(my_list[1])
```

- [ ] A) 10
- [x] B) 20
- [ ] C) 30
- [ ] D) 40

**Q33.** What is the output?

```python
my_list = [10, 20, 30, 40]
print(my_list[1:3])
```

- [ ] A) `[10, 20]`
- [x] B) `[20, 30]`
- [ ] C) `[20, 30, 40]`
- [ ] D) `[10, 20, 30]`

**Q34.** What is the output?

```python
prices = {"A": 10, "B": 20}
print(prices["A"])
```

- [ ] A) A
- [x] B) 10
- [ ] C) "A": 10
- [ ] D) Error

**Q35.** What is the output?

```python
my_set = {1, 2, 2, 3}
print(my_set)
```

- [ ] A) `{1, 2, 2, 3}`
- [x] B) `{1, 2, 3}`
- [ ] C) `[1, 2, 3]`
- [ ] D) Error

**Q36.** What is the output?

```python
text = "Python"
print(text.replace("P", "J"))
```

- [ ] A) Python
- [x] B) Jython
- [ ] C) python
- [ ] D) Error

**Q37.** What is the output?

```python
my_tuple = (10, 20, 30)
print(my_tuple[0])
```

- [x] A) 10
- [ ] B) 20
- [ ] C) 30
- [ ] D) Error

**Q38.** What happens?

```python
my_set = {1, 2, 3}
print(my_set[0])
```

- [ ] A) Prints 1
- [ ] B) Prints 0
- [x] C) TypeError
- [ ] D) Prints {1}

**Q39.** What is the output?

```python
my_list = [1, 2, 3, 4, 5]
print(my_list[::2])
```

- [ ] A) `[1, 2]`
- [x] B) `[1, 3, 5]`
- [ ] C) `[2, 4]`
- [ ] D) `[1, 2, 3, 4, 5]`

**Q40.** What is the output?

```python
my_dict = {"a": 1, "a": 2}
print(my_dict["a"])
```

- [ ] A) 1
- [x] B) 2
- [ ] C) [1, 2]
- [ ] D) Error

---

## Answer Key

### Section A

1. B 2. B 3. B 4. C 5. B 6. B 7. B 8. B 9. B 10. C
2. B 12. C 13. B 14. B 15. C 16. C 17. B 18. B 19. C 20. D

### Section B

21. T 22. T 23. F 24. T 25. F 26. T 27. F 28. T 29. T 30. F

### Section C

31. B 32. B 33. B 34. B 35. B 36. B 37. A 38. C 39. B 40. B

---

## Quick Reference

### Strings

```python
"text" or 'text'        # Create
"""multi-line"""        # Multi-line
str.replace("old", "new") # Replace
str.lower()             # Lowercase
str.upper()             # Uppercase
```

### Lists

```python
[1, 2, 3]              # Create
list[0]                # Index 0 (first)
list[-1]               # Last element
list[1:3]              # Slice (indices 1-2)
list[::2]              # Every other
```

### Dictionaries

```python
{"key": value}         # Create
dict[key]              # Access value
dict["key"] = value    # Add/update
dict.keys()            # All keys
dict.values()          # All values
dict.items()           # All pairs
```

### Sets

```python
{1, 2, 3}              # Create (no duplicates)
set(list)              # Convert & remove duplicates
sorted(set)            # Sort (returns list)
```

### Tuples

```python
(1, 2, 3)              # Create (immutable)
tuple[0]               # Access by index
tuple(list)            # Convert
```

### Data Structure Comparison

|Structure|Syntax|Mutable|Unique|Ordered|Indexed|
|---|---|---|---|---|---|
|List|`[]`|Yes|No|Yes|Yes|
|Dict|`{k:v}`|Yes|Keys|Yes|By key|
|Set|`{}`|Yes|Yes|No|No|
|Tuple|`()`|No|No|Yes|Yes|

### Key Concepts

- **Zero-indexed:** First element is index 0
- **Slicing:** `[start:stop]` - excludes stop
- **Negative index:** `-1` is last element
- **Immutable:** Can't be changed (tuples, strings)
- **Unique:** Sets automatically remove duplicates
- **Key-value:** Dictionaries link keys to values

---

#python #quiz #strings #lists #dictionaries #sets #tuples #data-structures