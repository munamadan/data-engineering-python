
## 📚 What We'll Learn

- DataCamp editor
- Basic concepts
- Python syntax
- Variables and data types

---

## 🐍 Why Python?

### Key Advantages

1. **Free & Open Source**
    
    - No licensing costs
    - Community-driven development
    - Transparent codebase
2. **Large Package Library**
    
    - Extensive ecosystem
    - Solutions for almost any problem
    - Active community support
3. **Natural Syntax**
    
    - Easy to read and write
    - English-like commands
    - Beginner-friendly

---

## 💼 Multipurpose Python

### Common Use Cases

Python is used across many domains:

- **Automation** - Scripting repetitive tasks
- **Web Development** - Building websites and APIs
- **AI & Machine Learning** - Data science and AI models
- **Reporting** - Generating reports and dashboards
- **Web Scraping** - Extracting data from websites
- **Image Recognition** - Computer vision applications

---

## 🖥️ DataCamp Editor Components

### Interface Elements

1. **Exercise** - Task description and requirements
2. **Instructions** - Step-by-step guidance
3. **script.py** - Where you write your code
4. **IPython Shell** - Interactive Python console
5. **Submit Answer** - Check your solution
6. **Run Code** - Execute without submitting

---

## 🔢 Python Syntax Basics

### Running Code in Shell

**Simple Expression:**

```python
2 + 2
# Output: 4
```

**No need for print()** in IPython Shell - expressions automatically display their value.

### Using print() Function

**In script.py:**

```python
print("Hello Python")
# Output: Hello Python
```

**Note:** In scripts, use `print()` to display output.

---

## 🧮 Basic Operations

### Arithmetic Operators

```python
print(4 + 5)    # Addition: 9
print(10 - 4)   # Subtraction: 6
print(2 * 2)    # Multiplication: 4
print(20 / 4)   # Division: 5.0
```

**Operator Summary:**

| Operator | Operation      | Example  | Result |
| -------- | -------------- | -------- | ------ |
| `+`      | Addition       | `4 + 5`  | `9`    |
| `-`      | Subtraction    | `10 - 4` | `6`    |
| `*`      | Multiplication | `2 * 2`  | `4`    |
| `/`      | Division       | `20 / 4` | `5.0`  |

---

## 💬 Comments

### Purpose of Comments

Comments are lines that Python **does not execute**.

**Uses:**

- Explain code functionality
- Document decisions
- Temporarily disable code
- Add notes for yourself or others

### Comment Syntax

```python
# This is a comment
print(4 + 5)  # Comment after code

# Comments are not executed
```

**Good Practice:**

```python
# Calculate customer age
customer_age = 25

# Apply discount for senior citizens
if customer_age >= 65:
    discount = 0.15
```

---

## 📦 Variables

### What are Variables?

**Variables** store values for later use.

**Think of variables like:**

- Phone contacts (name stores phone number)
- Boxes with labels (label = variable name, contents = value)

### Creating Variables

**Syntax:** `variable_name = value`

```python
customer_age = 25
account_balance = 58.50
```

### Variable Assignment

Variables can be **reassigned**:

```python
customer_age = 25
customer_age = 26  # Updates the value

print(customer_age)  # Output: 26
```

**Latest assignment wins!**

### Variable Naming Rules

**Valid names:**

- Use letters, numbers, underscores
- Cannot start with a number
- Case-sensitive (`Age` ≠ `age`)
- Use snake_case for readability

**Examples:**

```python
customer_age = 25      # Good
account_balance = 100  # Good
is_active = True       # Good

# Invalid:
# 2nd_customer = "John"  # Can't start with number
# customer-age = 25      # Can't use hyphens
```

---

## 🏷️ Data Types

### What are Data Types?

**Data types** define what kind of value a variable holds.

### Three Basic Data Types

#### 1. Integer (int)

**Whole numbers** without decimals.

```python
customer_age = 25
print(type(customer_age))
# Output: <class 'int'>
```

**Examples:** `0`, `42`, `-17`, `1000`

#### 2. Float (float)

**Numbers with decimal points**.

```python
account_balance = 58.50
print(type(account_balance))
# Output: <class 'float'>
```

**Examples:** `3.14`, `0.5`, `-2.7`, `100.0`

#### 3. Boolean (bool)

**True or False values**.

```python
is_active_customer = True
print(type(is_active_customer))
# Output: <class 'bool'>
```

**Values:** Only `True` or `False` (capitalized!)

---

## 🔍 Checking Data Types

### Using type() Function

**Syntax:** `type(variable_or_value)`

```python
customer_age = 25
account_balance = 58.50
is_customer_active = True

print(type(customer_age))        # <class 'int'>
print(type(account_balance))     # <class 'float'>
print(type(is_customer_active))  # <class 'bool'>
```

**Why check types?**

- Debugging
- Understanding data
- Ensuring correct operations

---

## 📊 Data Type Comparison

| Type      | Description     | Examples         | Output Type       |
| --------- | --------------- | ---------------- | ----------------- |
| **int**   | Whole numbers   | `25`, `-10`, `0` | `<class 'int'>`   |
| **float** | Decimal numbers | `58.50`, `3.14`  | `<class 'float'>` |
| **bool**  | True/False      | `True`, `False`  | `<class 'bool'>`  |

---

## 💡 Key Concepts

### 1. Python is Case-Sensitive

```python
Age = 25
age = 30

print(Age)  # 25
print(age)  # 30
# These are different variables!
```

### 2. Variables Can Change Type

```python
value = 25        # int
print(type(value))  # <class 'int'>

value = 25.5      # now float
print(type(value))  # <class 'float'>
```

### 3. Boolean Capitalization Matters

```python
is_active = True   # Correct (capital T)
is_active = true   # Error! (lowercase t)
```

---

## 🎯 Best Practices

### Variable Naming

✅ **Good:**

```python
customer_age = 25
total_price = 99.99
is_logged_in = True
```

❌ **Avoid:**

```python
x = 25              # Not descriptive
customerAge = 25    # Use snake_case, not camelCase
CUSTOMER_AGE = 25   # ALL_CAPS typically for constants
```

### Comments

✅ **Good:**

```python
# Calculate total with tax
total = price * 1.08
```

❌ **Avoid:**

```python
# Set x to 5
x = 5  # Don't state the obvious
```

---

## 🔧 Common Operations

### Working with Numbers

```python
# Basic arithmetic
result = 10 + 5 * 2  # Order of operations applies: 20

# Combining variables
price = 50
quantity = 3
total = price * quantity
print(total)  # 150
```

### Working with Booleans

```python
is_member = True
has_coupon = False

print(is_member)   # True
print(has_coupon)  # False
```

---

## 📝 Quick Reference

### Basic Syntax

```python
# Comments
# Single line comment

# Variables
variable_name = value

# Print
print("text")
print(variable)

# Data types
integer_var = 42
float_var = 3.14
boolean_var = True

# Check type
type(variable)
```

### Operators

```python
+    # Addition
-    # Subtraction
*    # Multiplication
/    # Division
```

---

## 🎓 Summary

### What You've Learned

1. **Why Python:** Free, large library, natural syntax
2. **Python uses:** Automation, web dev, AI, scraping, etc.
3. **Basic operations:** Addition, subtraction, multiplication, division
4. **Comments:** Lines that aren't executed (start with `#`)
5. **Variables:** Store values with names
6. **Data types:** int (whole numbers), float (decimals), bool (True/False)
7. **type() function:** Check what type a variable is

### Key Takeaways

- Python syntax is **readable and intuitive**
- Variables can be **reassigned**
- Comments help **document code**
- Data types define **what kind of value** a variable holds
- Use `type()` to **check data types**

---

#python #basics #variables #data-types #introduction #datacamp

## Section A: Multiple Choice (15 Questions)

**Q1.** Which of these is NOT an advantage of Python?

- [ ] A) Free & Open Source
- [ ] B) Large Package Library
- [ ] C) Natural Syntax
- [x] D) Requires expensive license

**Q2.** What symbol starts a comment in Python?

- [x] A) //
- [ ] B) #
- [ ] C) /*
- [ ] D) --

**Q3.** What is the correct syntax for creating a variable?

- [x] A) variable_name = value
- [ ] B) var variable_name = value
- [ ] C) variable_name := value
- [ ] D) set variable_name value

**Q4.** Which data type represents True or False?

- [ ] A) int
- [ ] B) float
- [x] C) bool
- [ ] D) binary

**Q5.** What function checks a variable's data type?

- [ ] A) check()
- [ ] B) typeof()
- [x] C) type()
- [ ] D) datatype()

**Q6.** Which is a valid Python variable name?

- [ ] A) 2nd_customer
- [ ] B) customer-age
- [x] C) customer_age
- [ ] D) customer age

**Q7.** What data type is `25`?

- [ ] A) float
- [x] B) int
- [ ] C) string
- [ ] D) bool

**Q8.** What data type is `25.5`?

- [x] A) float
- [ ] B) int
- [ ] C) double
- [ ] D) decimal

**Q9.** Which operator performs division?

- [ ] A) %
- [ ] B) //
- [x] C) /
- [ ] D) \

**Q10.** What does Python use for multiplication?

- [ ] A) x
- [x] B) *
- [ ] C) ×
- [ ] D) mult

**Q11.** Which Python use case is mentioned in the course?

- [ ] A) Game development
- [ ] B) Mobile apps
- [x] C) Web scraping
- [ ] D) Operating systems

**Q12.** What is the output of `print(2 + 3)`?

- [ ] A) 2 + 3
- [x] B) 5
- [ ] C) 23
- [ ] D) Error

**Q13.** Which is the correct way to write a boolean True value?

- [ ] A) true
- [ ] B) TRUE
- [x] C) True
- [ ] D) T

**Q14.** What happens when you reassign a variable?

- [ ] A) Creates a new variable
- [ ] B) Error occurs
- [x] C) Updates the value
- [ ] D) Deletes the old variable

**Q15.** Which component of DataCamp editor is where you write code?

- [ ] A) IPython Shell
- [ ] B) Instructions
- [ ] C) script.py
- [x] D) Console

---

## Section B: True/False (10 Questions)

**Q16.** Python is free and open source. **T/F**

**Q17.** Comments are executed by Python. **T/F**

**Q18.** Variables can be reassigned to new values. **T/F**

**Q19.** `age` and `Age` are the same variable in Python. **T/F**

**Q20.** An int is a number with decimals. **T/F**

**Q21.** Python can be used for web development. **T/F**

**Q22.** Variable names can start with numbers. **T/F**

**Q23.** `print()` is used to display output. **T/F**

**Q24.** Python uses natural, readable syntax. **T/F**

**Q25.** Boolean values must be capitalized (True/False). **T/F**

---

## Section C: Code Analysis (5 Questions)

**Q26.** What is the output?

```python
print(10 - 4)
```

- [ ] A) 10 - 4
- [x] B) 6
- [ ] C) 14
- [ ] D) Error

**Q27.** What is the output?

```python
age = 25
age = 30
print(age)
```

- [ ] A) 25
- [x] B) 30
- [ ] C) 25 30
- [ ] D) Error

**Q28.** What is the output?

```python
value = 58.50
print(type(value))
```

- [ ] A) `<class 'int'>`
- [x] B) `<class 'float'>`
- [ ] C) `<class 'decimal'>`
- [ ] D) `58.50`

**Q29.** What happens when this runs?

```python
# Calculate total
total = 100
```

- [ ] A) Calculates total
- [x] B) Creates variable total with value 100
- [ ] C) Error - comments can't be above code
- [ ] D) Nothing executes

**Q30.** What is the output?

```python
is_active = True
print(type(is_active))
```

- [ ] A) `True`
- [ ] B) `<class 'bool'>`
- [x] C) `<class 'boolean'>`
- [ ] D) `1`

---

## Answer Key

### Section A

1. D 2. B 3. A 4. C 5. C 6. C 7. B 8. A 9. C 10. B
2. C 12. B 13. C 14. C 15. C

### Section B

16. T 17. F 18. T 19. F 20. F 21. T 22. F 23. T 24. T 25. T

### Section C

26. B 27. B 28. B 29. B 30. B

---

## Quick Reference

### Basic Concepts

```python
# Comments - not executed
# Use # symbol

# Variables
variable_name = value

# Print output
print("Hello")
print(variable)

# Check data type
type(variable)
```

### Data Types

- **int:** Whole numbers (25, -10, 0)
- **float:** Decimals (3.14, 58.50)
- **bool:** True or False (capitalized!)

### Operators

```python
+    # Addition
-    # Subtraction
*    # Multiplication
/    # Division
```

### Variable Rules

✅ Use letters, numbers, underscores ✅ Use snake_case (customer_age) ✅ Descriptive names ❌ Can't start with number ❌ No hyphens or spaces ❌ Case-sensitive

### Python Advantages

- Free & Open Source
- Large Package Library
- Natural Syntax
- Multipurpose (AI, Web, Automation, etc.)

---

#python #quiz #basics #variables #introduction