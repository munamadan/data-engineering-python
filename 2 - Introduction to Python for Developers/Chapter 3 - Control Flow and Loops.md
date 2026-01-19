
## 📚 Chapter Topics

- Comparison operators
- Conditional statements (if/elif/else)
- For loops
- While loops
- Building workflows with keywords

---

## ✅ Booleans Review

**Boolean values** are used to make comparisons:

```python
the_truth = True
print(the_truth)
# Output: True
```

**Values:** `True` or `False` (capitalized!)

---

## ⚖️ Comparison Operators

### What are Operators?

**Operators** = Symbols or combinations of symbols used to compare things

Similar to math operators (`*`, `+`, `-`, `/`)

### Equality Operators

#### Equal to: `==`

```python
# Check if 2 equals 3
2 == 3  # False

# Check if 2 equals 2
2 == 2  # True
```

#### Not equal to: `!=`

```python
# Check if 2 is not equal to 3
2 != 3  # True
```

**Common use case:** Checking login details

---

### Numeric Comparison Operators

```python
# Less than
5 < 7   # True

# Less than or equal to
5 <= 7  # True

# Greater than
5 > 7   # False

# Greater than or equal to
5 >= 7  # False
```

### String Comparisons

**Strings are evaluated alphabetically:**

```python
# Is "James" greater than "Brian"?
"James" > "Brian"  # True
```

---

## 📊 Comparison Operators Cheat Sheet

|Operator|Function|Example|Result|
|---|---|---|---|
|`==`|Equal to|`2 == 2`|`True`|
|`!=`|Not equal to|`2 != 3`|`True`|
|`>`|Greater than|`5 > 7`|`False`|
|`>=`|Greater or equal|`5 >= 5`|`True`|
|`<`|Less than|`5 < 7`|`True`|
|`<=`|Less or equal|`5 <= 7`|`True`|

---

## 🔀 Conditional Statements

### If Statement

**Concept:** If condition is True, perform an action; otherwise, do nothing.

**Syntax:**

```python
if condition:
    action
```

**Example:**

```python
sales_target = 350
units_sold = 355

# Compare sales
if units_sold >= sales_target:
    print("Target achieved")
# Output: Target achieved
```

### Indentation is Critical!

**Indentation** shows what code belongs to the if statement.

```python
# Correct - indented
if units_sold >= sales_target:
    print("Target achieved")

# Error - not indented
if units_sold >= sales_target:
print("Target achieved")  # IndentationError!
```

---

### Elif Statement

**elif** = "else if" - Check additional conditions

```python
sales_target = 350
units_sold = 325

if units_sold >= sales_target:
    print("Target achieved")
# Check if close to target
elif units_sold >= 320:
    print("Target almost achieved")
```

**Note:** Can use **as many elif statements as needed!**

---

### Else Statement

**else** = Catch-all for when no conditions are met

```python
if units_sold >= sales_target:
    print("Target achieved")
elif units_sold >= 320:
    print("Target almost achieved")
else:
    print("Target not achieved")
```

---

## 📋 Conditional Keywords

|Keyword|Function|Use|
|---|---|---|
|`if`|If condition is met|First in workflow|
|`elif`|Else check if condition met|After if|
|`else`|Else perform this action|After elif|

**Order matters:** Must be `if` → `elif` → `else`

---

## 🔁 For Loops

### The Problem

**Checking individual list elements is tedious:**

```python
prices = [9.99, 8.99, 35.25, 1.50, 5.75]

prices[0] > 10   # False
prices[1] > 10   # False
prices[2] > 10   # True
# ... tedious!
```

### The Solution: For Loops

**For loop** = Perform action for each item in a sequence

**Syntax:**

```python
for value in sequence:
    action
```

**Components:**

- `value` = iterator (placeholder variable, can be any name)
- `sequence` = iterable (list, dictionary, string, etc.)
- `action` = indented code to execute

---

### Basic For Loop

```python
prices = [9.99, 8.99, 35.25, 1.50, 5.75]

# Print each price
for price in prices:
    print(price)

# Output:
# 9.99
# 8.99
# 35.25
# 1.5
# 5.75
```

**Note:** `price` is just a placeholder - could be `p`, `item`, or any name

---

### Conditionals in For Loops

```python
for price in prices:
    # Check if price > 10
    if price > 10:
        print("More than $10")
    # Check if price < 5
    elif price < 5:
        print("Less than $5")
    # Otherwise print price
    else:
        print(price)

# Output:
# 9.99
# 8.99
# More than $10
# Less than $5
# 5.75
```

---

### Looping Through Strings

**Strings are iterable!**

```python
username = "george_dc"

# Loop through each character
for char in username:
    print(char)

# Output:
# g
# e
# o
# r
# g
# e
# _
# d
# c
```

---

### Looping Through Dictionaries

#### Loop Through Keys and Values

```python
products_dict = {"AG32": 87.99, "HT91": 21.50,
                 "PL65": 43.75, "OS31": 19.99}

# Loop through both keys and values
for key, val in products_dict.items():
    print(key, val)

# Output:
# AG32 87.99
# HT91 21.5
# PL65 43.75
# OS31 19.99
```

#### Loop Through Keys Only

```python
for key in products_dict.keys():
    print(key)
# Output: AG32, HT91, PL65, OS31
```

#### Loop Through Values Only

```python
for val in products_dict.values():
    print(val)
# Output: 87.99, 21.5, 43.75, 19.99
```

---

## 🔢 Range Function

**range()** = Generate sequence of numbers

**Syntax:** `range(start, end + 1)`

- `start` = inclusive
- `end` = NOT inclusive (add 1 to include it)

```python
for i in range(1, 6):
    print(i)

# Output:
# 1
# 2
# 3
# 4
# 5
```

### Building a Counter

```python
visits = 0

# Loop through numbers 1-10
for i in range(1, 11):
    visits += 1  # Same as visits = visits + 1

print(visits)
# Output: 10
```

---

## 🔄 While Loops

### If vs While

|If Statement|While Loop|
|---|---|
|Checks once|Checks repeatedly|
|Runs once if True|Runs continuously while True|
|Static|Dynamic|

### While Loop Syntax

```python
while condition:
    action
```

**Use cases:**

- Any continuous task
- Accelerate while button pressed
- Monitor while below/above threshold

---

### While Loop Example

```python
stock = 10
num_purchases = 0

# While purchases less than stock
while num_purchases < stock:
    num_purchases += 1
    print(stock - num_purchases)

# Output:
# 9
# 8
# 7
# ...
# 1
# 0
```

---

## ⚠️ While Loop Caution

### Infinite Loops

**Problem:** Loop runs forever if condition never becomes False

```python
stock = 10
num_purchases = 0

# INFINITE LOOP - num_purchases never changes!
while num_purchases < stock:
    print(stock - num_purchases)

# Output:
# 10
# 10
# 10
# ... (forever)
```

**Solution:** Always update the condition variable!

---

### Breaking a Loop

**break** = Terminate the loop immediately

```python
while num_purchases < stock:
    print(stock - num_purchases)
    break  # Exit loop

# Only prints once
```

**Works in:** Both for loops and while loops

**Stop running code:** Control + C (Windows/Linux) or Command + C (Mac)

---

### Conditionals in While Loops

```python
while num_purchases < stock:
    num_purchases += 1
    
    if stock - num_purchases > 7:
        print("Plenty of stock remaining")
    elif stock - num_purchases > 3:
        print("Some stock remaining")
    elif stock - num_purchases != 0:
        print("Low stock!")
    else:
        print("No stock!")
```

---

## 🔨 Building Complex Workflows

### Keywords for Complex Logic

#### The "in" Keyword

**in** = Check if value is in a data structure

```python
products_dict = {"AG32": 10, "HT91": 20, "PL65": 30}

# Check if "OS31" is a key
if "OS31" in products_dict.keys():
    print(True)
else:
    print(False)
# Output: False
```

---

#### The "not" Keyword

**not** = Check if condition is NOT met

```python
# Check if "OS31" is NOT a key
if "OS31" not in products_dict.keys():
    print("Key not found")
else:
    print("Key found")
# Output: Key not found
```

---

#### The "and" Keyword

**and** = Check if ALL conditions are True

```python
# Check if "HT91" exists AND min price > 5
if "HT91" in products_dict.keys() and min(products_dict.values()) > 5:
    print(True)
else:
    print(False)
# Output: True (both conditions met)
```

---

#### The "or" Keyword

**or** = Check if AT LEAST ONE condition is True

```python
# Check if "HT91" exists OR min price < 5
if "HT91" in products_dict.keys() or min(products_dict.values()) < 5:
    print(True)
else:
    print(False)
# Output: True (first condition met)
```

---

## 📊 Logical Keywords Summary

|Keyword|Function|Example|
|---|---|---|
|`in`|Value is in structure|`"key" in dict.keys()`|
|`not`|Condition is not met|`"key" not in dict`|
|`and`|ALL conditions true|`x > 5 and y < 10`|
|`or`|AT LEAST ONE true|`x > 5 or y < 10`|

---

## 🔢 Updating Variables

### Addition Assignment

```python
sales_count = 0

for sale in range(1, 10):
    sales_count += 1  # Same as sales_count = sales_count + 1

print(sales_count)  # 9
```

### Subtraction Assignment

```python
stock = 10

for sale in range(1, 10):
    stock -= 1  # Same as stock = stock - 1

print(stock)  # 1
```

---

## 📝 Appending to Lists

**Append** = Add items to a list dynamically

```python
# Create empty list
expensive_products = []

products_dict = {"AG32": 10, "HT91": 20, "PL65": 30, "KB07": 25}

# Loop through dictionary
for key, val in products_dict.items():
    # Check if price >= 20
    if val >= 20:
        # Append product ID to list
        expensive_products.append(key)

print(expensive_products)
# Output: ['HT91', 'PL65', 'KB07']
```

**Use case:** Store information that meets specific criteria

---

## 🎯 Course Recap

### Chapter 1: Basics

- Variables and data types (int, float, bool, str)
- Arithmetic operators (+, -, *, /, **, %)
- Comments (#)

### Chapter 2: Data Structures

- Strings and methods (.lower(), .upper(), .replace())
- Lists and indexing
- Dictionaries (key-value pairs)
- Sets (unique values)
- Tuples (immutable)

### Chapter 3: Control Flow

- Comparison operators (==, !=, >, <, >=, <=)
- Conditional statements (if/elif/else)
- For loops (iterate through sequences)
- While loops (continuous tasks)
- Logical keywords (and, or, in, not)
- List appending

---

## 🚀 Next Steps

**Additional Topics to Explore:**

- Built-in functions: `zip()`, `enumerate()`
- Packages: `os`, `time`, `pandas`, `numpy`, `requests`
- Building custom functions
- Virtual environments (`venv`)

---

## 📖 Quick Reference

### Comparison Operators

```python
==  # Equal
!=  # Not equal
>   # Greater than
<   # Less than
>=  # Greater or equal
<=  # Less or equal
```

### Conditionals

```python
if condition:
    action
elif condition:
    action
else:
    action
```

### For Loop

```python
for item in sequence:
    action
```

### While Loop

```python
while condition:
    action
```

### Logical Keywords

```python
and  # All true
or   # At least one true
in   # Value in structure
not  # Condition not met
```

### Updating Variables

```python
x += 1   # Add
x -= 1   # Subtract
list.append(item)  # Add to list
```

---

#python #conditionals #loops #control-flow #for-loops #while-loops #datacamp
## Section A: Multiple Choice (20 Questions)

**Q1.** What operator checks if two values are equal?

- [ ] A) =
- [x] B) ==
- [ ] C) ===
- [ ] D) !=

**Q2.** What does `!=` mean?

- [ ] A) Equal to
- [x] B) Not equal to
- [ ] C) Greater than
- [ ] D) Assignment

**Q3.** What must follow an if statement?

- [ ] A) Semicolon
- [x] B) Colon and indentation
- [ ] C) Parentheses
- [ ] D) Nothing

**Q4.** What keyword checks additional conditions after if?

- [ ] A) else
- [ ] B) elif
- [x] C) elseif
- [ ] D) then

**Q5.** What is the purpose of a for loop?

- [ ] A) Create variables
- [x] B) Perform action for each item in sequence
- [ ] C) Delete items
- [ ] D) Compare values

**Q6.** What does `range(1, 5)` generate?

- [ ] A) 1, 2, 3, 4, 5
- [x] B) 1, 2, 3, 4
- [ ] C) 0, 1, 2, 3, 4
- [ ] D) 2, 3, 4, 5

**Q7.** What does a while loop do?

- [ ] A) Runs once
- [x] B) Runs continuously while condition is True
- [ ] C) Never runs
- [ ] D) Runs backwards

**Q8.** What keyword stops a loop immediately?

- [ ] A) stop
- [ ] B) end
- [x] C) break
- [ ] D) exit

**Q9.** What does the `in` keyword check?

- [x] A) If value is in a data structure
- [ ] B) If value is greater than
- [ ] C) If value is equal
- [ ] D) If value is a number

**Q10.** What does the `and` keyword require?

- [ ] A) One condition to be True
- [x] B) All conditions to be True
- [ ] C) No conditions
- [ ] D) Conditions to be False

**Q11.** What does the `or` keyword require?

- [ ] A) All conditions True
- [ ] B) All conditions False
- [x] C) At least one condition True
- [ ] D) No conditions

**Q12.** What does `+=` do?

- [ ] A) Multiplies
- [x] B) Adds to variable
- [ ] C) Divides
- [ ] D) Compares

**Q13.** What method adds items to a list?

- [ ] A) add()
- [x] B) append()
- [ ] C) insert()
- [ ] D) push()

**Q14.** How do you loop through dictionary keys and values?

- [ ] A) `for k, v in dict:`
- [x] B) `for k, v in dict.items():`
- [ ] C) `for key, value in dict:`
- [ ] D) `for dict in items:`

**Q15.** What happens without proper indentation after if?

- [ ] A) Code runs normally
- [x] B) IndentationError
- [ ] C) SyntaxWarning
- [ ] D) Nothing

**Q16.** What's the risk with while loops?

- [ ] A) They're too slow
- [x] B) They can run infinitely
- [ ] C) They use too much memory
- [ ] D) They only run once

**Q17.** What's the first keyword in a conditional workflow?

- [ ] A) elif
- [ ] B) else
- [x] C) if
- [ ] D) while

**Q18.** How are strings compared?

- [ ] A) By length
- [x] B) Alphabetically
- [ ] C) Numerically
- [ ] D) Randomly

**Q19.** What does `-=` do?

- [ ] A) Adds
- [ ] B) Multiplies
- [x] C) Subtracts from variable
- [ ] D) Divides

**Q20.** Can you use break in for loops?

- [x] A) Yes
- [ ] B) No
- [ ] C) Only in Python 3
- [ ] D) Only with while

---

## Section B: True/False (10 Questions)

**Q21.** `==` is used for assignment. **T/F**

**Q22.** elif must come after if. **T/F**

**Q23.** Indentation is required after if statements. **T/F**

**Q24.** range(1, 5) includes 5. **T/F**

**Q25.** While loops run continuously while condition is True. **T/F**

**Q26.** The `and` keyword requires all conditions to be True. **T/F**

**Q27.** You can loop through strings character by character. **T/F**

**Q28.** break stops only for loops, not while loops. **T/F**

**Q29.** `in` checks if a value exists in a data structure. **T/F**

**Q30.** `+=` and `= +` are exactly the same. **T/F**

---

## Section C: Code Analysis (10 Questions)

**Q31.** What is the output?

```python
if 5 > 3:
    print("Yes")
```

- [ ] A) Nothing
- [x] B) Yes
- [ ] C) True
- [ ] D) Error

**Q32.** What is the output?

```python
x = 10
if x < 5:
    print("Small")
elif x < 15:
    print("Medium")
else:
    print("Large")
```

- [ ] A) Small
- [x] B) Medium
- [ ] C) Large
- [ ] D) Nothing

**Q33.** What is the output?

```python
for i in range(1, 4):
    print(i)
```

- [ ] A) 1 2 3 4
- [x] B) 1 2 3
- [ ] C) 0 1 2 3
- [ ] D) 2 3 4

**Q34.** What is the output?

```python
count = 0
for i in range(5):
    count += 1
print(count)
```

- [x] A) 4
- [ ] B) 5
- [ ] C) 0
- [ ] D) Error

**Q35.** What is the output?

```python
x = 5
while x > 3:
    print(x)
    break
```

- [x] A) Infinite loop
- [ ] B) 5
- [ ] C) 5 4 3
- [ ] D) Nothing

**Q36.** What is the output?

```python
if True and False:
    print("Yes")
else:
    print("No")
```

- [ ] A) Yes
- [x] B) No
- [ ] C) True
- [ ] D) Error

**Q37.** What is the output?

```python
if True or False:
    print("Yes")
else:
    print("No")
```

- [x] A) Yes
- [ ] B) No
- [ ] C) True
- [ ] D) Error

**Q38.** What is the output?

```python
my_list = []
for i in range(3):
    my_list.append(i)
print(my_list)
```

- [ ] A) [1, 2, 3]
- [x] B) [0, 1, 2]
- [ ] C) [3]
- [ ] D) []

**Q39.** What is the output?

```python
x = 10
x -= 5
print(x)
```

- [ ] A) 10
- [x] B) 5
- [ ] C) 15
- [ ] D) -5

**Q40.** What is the output?

```python
for char in "Hi":
    print(char)
```

- [ ] A) Hi
- [x] B) H i (on separate lines)
- [ ] C) H i (on same line)
- [ ] D) Error

---

## Answer Key

### Section A

1. B 2. B 3. B 4. B 5. B 6. B 7. B 8. C 9. A 10. B
2. C 12. B 13. B 14. B 15. B 16. B 17. C 18. B 19. C 20. A

### Section B

21. F 22. T 23. T 24. F 25. T 26. T 27. T 28. F 29. T 30. F

### Section C

31. B 32. B 33. B 34. B 35. B 36. B 37. A 38. B 39. B 40. B

---

## Quick Reference

### Comparison Operators

```python
==  # Equal to
!=  # Not equal to
>   # Greater than
<   # Less than
>=  # Greater or equal
<=  # Less or equal
```

### Conditional Statements

```python
if condition:
    action
elif condition:
    action
else:
    action
```

### For Loop

```python
for item in sequence:
    action

for i in range(1, 6):  # 1, 2, 3, 4, 5
    print(i)
```

### While Loop

```python
while condition:
    action
    # Must update condition!
```

### Logical Keywords

```python
and  # All conditions must be True
or   # At least one condition True
in   # Check if value in structure
not  # Negate condition
```

### Loop Control

```python
break  # Exit loop immediately
```

### Variable Updates

```python
x += 1   # Add 1 to x
x -= 1   # Subtract 1 from x
list.append(item)  # Add to list
```

### Dictionary Looping

```python
# Keys and values
for key, val in dict.items():
    print(key, val)

# Keys only
for key in dict.keys():
    print(key)

# Values only
for val in dict.values():
    print(val)
```

### Key Concepts

- **Indentation matters** after colons
- **range(start, stop)** - stop is NOT included
- **While loops** can be infinite if condition never changes
- **break** works in both for and while loops
- **and** requires ALL conditions True
- **or** requires AT LEAST ONE condition True
- **Strings are iterable** - can loop through characters

---

#python #quiz #conditionals #loops #control-flow #for-loops #while-loops