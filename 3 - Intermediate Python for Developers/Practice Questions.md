
## 📋 Assessment Overview

This comprehensive questionnaire covers all three chapters:

- **Chapter 1:** Built-in Functions, Modules & Packages
- **Chapter 2:** Custom Functions, Arguments, Docstrings
- **Chapter 3:** Lambda Functions & Error Handling

**Total Questions:** 100 **Recommended Time:** 3-4 hours **Passing Score:** 70%

---

## Section A: Multiple Choice (30 Questions)

### Built-in Functions & Modules

**Q1.** Which function counts the number of elements in a data structure?

- [ ] A) count()
- [ ] B) size()
- [x] C) len()
- [ ] D) length()

**Q2.** What does `sorted()` return when given the list `[5, 2, 8, 1]`?

- [x] A) `[1, 2, 5, 8]`
- [ ] B) `[8, 5, 2, 1]`
- [ ] C) `None` (sorts in place)
- [ ] D) An error

**Q3.** Which module would you use for interacting with the operating system?

- [ ] A) sys
- [x] B) os
- [ ] C) platform
- [ ] D) system

**Q4.** What is the difference between a module attribute and a module function?

- [ ] A) Attributes use parentheses, functions don't
- [x] B) Attributes have values, functions perform tasks
- [ ] C) No difference
- [ ] D) Attributes are faster

**Q5.** How do you install a package from PyPI?

- [ ] A) `python install package_name`
- [ ] B) `pip get package_name`
- [x] C) `python3 -m pip install package_name`
- [ ] D) `import package_name`

**Q6.** What does the following import statement do: `from os import getcwd, chdir`?

- [ ] A) Imports the entire os module
- [x] B) Imports only getcwd and chdir functions
- [ ] C) Creates aliases for os functions
- [ ] D) Causes an error

**Q7.** Which is TRUE about `len()`?

- [ ] A) Works with floats
- [ ] B) Works with integers
- [x] C) Works with dictionaries
- [ ] D) Works with booleans

**Q8.** What type of object does `pd.read_csv()` return?

- [ ] A) List
- [ ] B) Dictionary
- [x] C) DataFrame
- [ ] D) Array

**Q9.** What is the difference between a package and a library?

- [x] A) They are completely different things
- [ ] B) They are the same thing
- [ ] C) Packages contain code, libraries don't
- [ ] D) Libraries are larger than packages

**Q10.** What does `round(3.14159, 2)` return?

- [ ] A) 3.1
- [x] B) 3.14
- [ ] C) 3.15
- [ ] D) 3

### Custom Functions & Arguments

**Q11.** What keyword starts a function definition?

- [ ] A) function
- [x] B) def
- [ ] C) define
- [ ] D) func

**Q12.** Which principle suggests avoiding code repetition?

- [ ] A) KISS
- [x] B) DRY
- [ ] C) SOLID
- [ ] D) YAGNI

**Q13.** In `round(number=3.14, ndigits=2)`, what type of arguments are used?

- [ ] A) Positional
- [x] B) Keyword
- [ ] C) Default
- [ ] D) Arbitrary

**Q14.** What is wrong with this function definition?

```python
def greet(name="User", age):
    return f"{name} is {age}"
```

- [ ] A) Nothing
- [x] B) Non-default arguments must come before default arguments
- [ ] C) Default arguments must come before non-default arguments
- [ ] D) Missing docstring

**Q15.** How do you access a function's docstring?

- [ ] A) `function.doc`
- [x] B) `function.__doc__`
- [ ] C) `function.docstring`
- [ ] D) `doc(function)`

**Q16.** What does `*args` create inside a function?

- [ ] A) List
- [x] B) Tuple
- [ ] C) Dictionary
- [ ] D) Set

**Q17.** What does `**kwargs` create inside a function?

- [ ] A) List
- [ ] B) Tuple
- [x] C) Dictionary
- [ ] D) Set

**Q18.** Which is a valid multi-line docstring section?

- [x] A) Parameters:
- [ ] B) Args:
- [ ] C) Inputs:
- [ ] D) Variables:

**Q19.** What happens if a function has no `return` statement?

- [ ] A) Returns 0
- [x] B) Returns None
- [ ] C) Causes an error
- [ ] D) Returns empty string

**Q20.** Can you have both `*args` and `**kwargs` in the same function?

- [x] A) Yes
- [ ] B) No
- [ ] C) Only in Python 3.9+
- [ ] D) Only for lambda functions

### Lambda Functions & Error Handling

**Q21.** What keyword creates a lambda function?

- [ ] A) def
- [x] B) lambda
- [ ] C) function
- [ ] D) anonymous

**Q22.** Do lambda functions require a return statement?

- [ ] A) Yes, always
- [x] B) No, return is implicit
- [ ] C) Only for complex operations
- [ ] D) Only when stored in a variable

**Q23.** What does `map()` return?

- [ ] A) List
- [x] B) Map object
- [ ] C) Tuple
- [ ] D) Dictionary

**Q24.** What error occurs when adding a string and integer: `"Hello" + 5`?

- [ ] A) ValueError
- [x] B) TypeError
- [ ] C) SyntaxError
- [ ] D) AttributeError

**Q25.** What shows the sequence of function calls leading to an error?

- [ ] A) Error log
- [x] B) Traceback
- [ ] C) Debug output
- [ ] D) Exception

**Q26.** Which error handling approach stops code execution?

- [ ] A) try-except
- [x] B) raise
- [ ] C) Both
- [ ] D) Neither

**Q27.** What's the conventional name for a single lambda argument?

- [ ] A) arg
- [ ] B) param
- [x] C) x
- [ ] D) value

**Q28.** When should you use a lambda function?

- [ ] A) Complex tasks
- [x] B) Simple, one-time operations
- [ ] C) Functions needing docstrings
- [ ] D) Multi-line operations

**Q29.** What error type indicates an invalid value?

- [ ] A) TypeError
- [x] B) ValueError
- [ ] C) InvalidError
- [ ] D) DataError

**Q30.** After a try-except block catches an error, what happens?

- [ ] A) Program stops
- [x] B) Program continues
- [ ] C) Function restarts
- [ ] D) Error is re-raised

---

## Section B: Code Output Prediction (20 Questions)

**Q31.** What is the output?

```python
numbers = [10, 20, 30, 40]
print(max(numbers) - min(numbers))
```

**Q32.** What is the output?

```python
print(len({"a": 1, "b": 2, "c": 3}))
```

**Q33.** What is the output?

```python
result = round(sum([1.5, 2.5, 3.5]) / 3, 2)
print(result)
```

**Q34.** What is the output?

```python
def multiply(x, y=2):
    return x * y

print(multiply(5))
```

**Q35.** What is the output?

```python
def greet(*args):
    return len(args)

print(greet("a", "b", "c", "d"))
```

**Q36.** What is the output?

```python
def info(**kwargs):
    return list(kwargs.keys())

print(info(name="John", age=30))
```

**Q37.** What is the output?

```python
result = (lambda x: x ** 2)(4)
print(result)
```

**Q38.** What is the output?

```python
numbers = [1, 2, 3]
doubled = map(lambda x: x * 2, numbers)
print(type(doubled))
```

**Q39.** What is the output?

```python
def test():
    try:
        return 10 / 2
    except:
        return 0
    
print(test())
```

**Q40.** What is the output?

```python
sorted("python")
```

**Q41.** What happens?

```python
import os
print(os.environ)
```

**Q42.** What is the output?

```python
def calculate(a, b, c=5):
    return a + b + c

print(calculate(1, 2, c=3))
```

**Q43.** What is the output?

```python
func = lambda x, y: x if x > y else y
print(func(10, 5))
```

**Q44.** What is the output?

```python
names = ["alice", "bob"]
result = list(map(lambda x: x.upper(), names))
print(result)
```

**Q45.** What is the output?

```python
def outer(x):
    def inner(y):
        return x + y
    return inner(5)

print(outer(10))
```

**Q46.** What happens?

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    result = None
print(result)
```

**Q47.** What is the output?

```python
values = [1, 2, 3, 4, 5]
print(sum(values) / len(values))
```

**Q48.** What is the output?

```python
def test(*args, **kwargs):
    return len(args) + len(kwargs)

print(test(1, 2, 3, a=4, b=5))
```

**Q49.** What is the output?

```python
data = [("a", 1), ("b", 2), ("c", 3)]
result = sorted(data, key=lambda x: x[1])
print(result[0])
```

**Q50.** What is the output?

```python
from os import getcwd
print(type(getcwd))
```

---

## Section C: Error Identification & Debugging (15 Questions)

**Q51.** Identify and fix the error:

```python
def calculate(x)
    return x * 2
```

**Q52.** Identify and fix the error:

```python
double = lambda x return x * 2
```

**Q53.** Identify and fix the error:

```python
def greet(age, name="User"):
    return f"Hello {name}, age {age}"
```

**Q54.** Identify and fix the error:

```python
try
    result = 10 / 2
except:
    print("Error")
```

**Q55.** Identify and fix the error:

```python
def add(*args)
    return sum(args)
```

**Q56.** Identify and fix the error:

```python
numbers = [1, 2, 3]
result = map(lambda x: x * 2)
print(list(result))
```

**Q57.** Identify and fix the error:

```python
if value < 0:
    raise "Value must be positive"
```

**Q58.** Identify and fix the error:

```python
from pandas import *
df = DataFrame({"a": [1, 2, 3]})
```

**Q59.** Identify and fix the error:

```python
def process(data):
    if type(data) != list:
        raise TypeError
    return sum(data)
```

**Q60.** Identify and fix the error:

```python
result = len(45)
```

**Q61.** Identify and fix the error:

```python
average = lambda x, y, z:
    total = x + y + z
    return total / 3
```

**Q62.** Identify and fix the error:

```python
os.getcwd()
```

**Q63.** Identify and fix the error:

```python
def calculate(default=10, required):
    return default + required
```

**Q64.** Identify and fix the error:

```python
def test(**kwargs):
    return kwargs[0]
```

**Q65.** Identify and fix the error:

```python
sorted([3, 1, 4, 1, 5], reverse=True, key=lambda x: x)
```

---

## Section D: Code Writing (20 Questions)

**Q66.** Write a function that uses `max()` and `min()` to find the range (difference) of values in a list.

**Q67.** Import only the `chdir` function from the `os` module and use it to change to `/home/user`.

**Q68.** Create a pandas DataFrame from this dictionary:

```python
data = {"product": ["A", "B", "C"], "price": [10, 20, 30]}
```

**Q69.** Write a function with a default argument:

```python
def calculate_tax(amount, tax_rate=0.08):
    # Your code
```

**Q70.** Write a function that accepts any number of numbers and returns their average:

```python
def average(*args):
    # Your code
```

**Q71.** Write a multi-line docstring for this function:

```python
def calculate_discount(price, discount_percent):
    """
    # Your docstring here
    """
    return price - (price * discount_percent / 100)
```

**Q72.** Write a lambda function that takes a string and returns its length.

**Q73.** Use `map()` and a lambda to square all numbers in `[1, 2, 3, 4, 5]`.

**Q74.** Write a try-except block that attempts to open a file:

```python
def read_file(filename):
    # Your code
```

**Q75.** Write a function that raises a ValueError if the input is negative:

```python
def validate_positive(number):
    # Your code
```

**Q76.** Write a function using `**kwargs` to create a formatted string:

```python
def format_info(**kwargs):
    # Example: format_info(name="John", age=30) 
    # Returns: "name: John, age: 30"
```

**Q77.** Convert this function to a lambda:

```python
def add_ten(x):
    return x + 10
```

**Q78.** Write a function that safely converts a string to int:

```python
def safe_int_conversion(value):
    # Use try-except
    # Return the integer or 0 if conversion fails
```

**Q79.** Use lambda with `sorted()` to sort this list by the second element:

```python
data = [(1, 5), (3, 2), (2, 8)]
```

**Q80.** Write a comprehensive function with all concepts:

```python
def process_data(*values, operation="sum", **options):
    """
    Process values with specified operation.
    Include error handling and docstring.
    """
    # Your code
```

---

## Section E: Scenario-Based Questions (15 Questions)

**Q81.** You need to calculate the average of test scores, but some scores might be invalid strings. Write a function that:

- Accepts a list of scores
- Skips invalid values
- Returns the average of valid scores
- Uses try-except for error handling

**Q82.** You're building a user registration system. Write a validation function that:

- Accepts username, email, and age
- Raises TypeError if types are incorrect
- Raises ValueError if values are invalid
- Username must be 3-20 characters
- Email must contain '@'
- Age must be 13-120

**Q83.** You need to quickly transform prices by applying a discount. Should you use lambda or custom function? Write the code for a 20% discount on `[100, 200, 300]`.

**Q84.** You're reading multiple CSV files. Some might not exist. Write code using try-except to:

- Attempt to read each file
- Print error message if file not found
- Continue processing other files
- Return list of successfully loaded DataFrames

**Q85.** Create a flexible calculator function that:

- Accepts any number of values (*args)
- Has operation parameter with default "sum"
- Supports operations: sum, average, max, min
- Includes proper docstring
- Handles empty input with raise

**Q86.** You need to process employee data stored in dictionaries. Write a function that:

- Accepts **kwargs for employee info
- Validates required fields: name, id, salary
- Raises KeyError if required fields missing
- Returns formatted string with all info

**Q87.** Write a data cleaning function that uses map() and lambda to:

- Remove whitespace from list of strings
- Convert to lowercase
- Filter out empty strings after cleaning

**Q88.** You're building an API client. Design a function signature that:

- Requires URL
- Has default method='GET'
- Accepts any headers as **kwargs
- Include comprehensive docstring

**Q89.** Create a logging wrapper function that:

- Takes any function as parameter
- Accepts *args and **kwargs
- Uses try-except to catch errors
- Prints function name and error message if error occurs
- Re-raises the error

**Q90.** Write a function to validate and process configuration:

```python
def validate_config(**config):
    # Required keys: host, port, username
    # Optional keys: password, timeout
    # Port must be integer between 1-65535
    # Use raise for validation errors
```

**Q91.** You need to process transaction data. Write a function that:

- Accepts list of transaction dictionaries
- Each has 'amount' and 'fee' keys
- Use lambda with map() to calculate net (amount - fee)
- Apply 8% tax to net
- Return list of final amounts rounded to 2 decimals

**Q92.** Create a grade calculator that:

- Accepts *args for test scores
- Has optional extra_credit=0 parameter
- Validates all scores are 0-100
- Raises ValueError for invalid scores
- Returns average including extra credit

**Q93.** Write a file processor that:

- Attempts to read file content
- If file not found, try alternate filename
- If both fail, raise custom error message
- Return file content if successful

**Q94.** Design a function for data type validation:

```python
def validate_types(data, expected_type, strict=True):
    # If strict=True, raise TypeError for wrong type
    # If strict=False, try to convert and return converted value
    # Include try-except for conversion attempts
```

**Q95.** Create a batch processor that:

- Accepts list of items to process
- Accepts processing function as parameter
- Uses map() to apply function to all items
- Handles errors per item with try-except
- Returns tuple: (successful_results, failed_items)

---

## Section F: Conceptual Questions (5 Questions)

**Q96.** Explain the complete workflow of installing and using a package from PyPI. Include:

- Installation command
- Import statement
- How to find package functions
- Difference between module and package

**Q97.** Compare and contrast:

- Positional arguments
- Keyword arguments
- Default arguments
- *args
- **kwargs

When would you use each? Provide examples.

**Q98.** Explain the decision tree for choosing between:

- Custom function vs lambda function
- try-except vs raise
- Regular arguments vs arbitrary arguments

**Q99.** Describe the anatomy of a professional Python function including:

- Function signature with various argument types
- Multi-line docstring with Args and Returns
- Error handling
- Return statement

**Q100.** Explain how Python's error handling works:

- What is a traceback?
- How does try-except work?
- When to use raise?
- Different error types and when they occur
- Best practices for error messages

---

## Answer Key Organization

### Scoring Guide

- **Section A (Q1-Q30):** 1 point each = 30 points
- **Section B (Q31-Q50):** 1 point each = 20 points
- **Section C (Q51-Q65):** 2 points each = 30 points
- **Section D (Q66-Q80):** 3 points each = 45 points
- **Section E (Q81-Q95):** 4 points each = 60 points
- **Section F (Q96-Q100):** 6 points each = 30 points

**Total Points:** 215 **Passing Score:** 150+ points (70%)

---

## Study Strategy

### Week 1: Foundation

- Review Chapter 1 notes
- Complete Section A questions (Q1-Q10)
- Complete Section B questions (Q31-Q41)
- Practice with built-in functions daily

### Week 2: Custom Functions

- Review Chapter 2 notes
- Complete Section A questions (Q11-Q20)
- Complete Section C questions (Q51-Q65)
- Complete Section D questions (Q66-Q76)
- Write 5 custom functions per day

### Week 3: Lambda & Error Handling

- Review Chapter 3 notes
- Complete Section A questions (Q21-Q30)
- Complete Section B questions (Q42-Q50)
- Complete Section D questions (Q77-Q80)
- Practice error handling scenarios

### Week 4: Integration & Practice

- Complete all Section E questions (Q81-Q95)
- Complete Section F questions (Q96-Q100)
- Retake questions you missed
- Build a small project using all concepts

---

## Quick Reference Cheat Sheet

### Built-in Functions

```python
max(list)          # Largest value
min(list)          # Smallest value
sum(list)          # Sum all elements
len(list)          # Count elements
round(num, n)      # Round to n decimals
sorted(list)       # Return sorted list
help(function)     # Get documentation
```

### Module Operations

```python
import module                    # Import entire module
from module import function      # Import specific function
import module as alias          # Import with alias
module.function()               # Call module function
module.attribute                # Access module attribute
```

### Custom Function Template

```python
def function_name(required, optional=default, *args, **kwargs):
    """
    Brief description.
    
    Args:
        required (type): Description
        optional (type): Description. Defaults to default.
        *args: Variable positional arguments
        **kwargs: Variable keyword arguments
    
    Returns:
        type: Description
    """
    # Function body
    return result
```

### Lambda Functions

```python
lambda x: expression                    # Single argument
lambda x, y: expression                 # Multiple arguments
map(lambda x: x*2, list)               # With map()
sorted(list, key=lambda x: x[1])       # With sorted()
```

### Error Handling

```python
# try-except (catch errors, continue)
try:
    # risky code
except ErrorType:
    # handle error

# raise (create errors, stop)
if not valid:
    raise ErrorType("Message")
```

---

## Practical Tips

### Before the Exam

- [ ] Review all three chapter notes
- [ ] Complete all 100 questions
- [ ] Understand WHY answers are correct, not just WHAT they are
- [ ] Practice writing code by hand
- [ ] Create flashcards for key concepts
- [ ] Build a small project using all concepts

### During Practice

- [ ] Time yourself on each section
- [ ] Don't look at notes initially
- [ ] Mark questions you're unsure about
- [ ] Review incorrect answers thoroughly
- [ ] Understand the reasoning behind each answer

### Common Pitfalls

- ⚠️ Confusing `*args` (tuple) with `**kwargs` (dictionary)
- ⚠️ Forgetting to convert map objects to lists
- ⚠️ Missing colons after function definitions and try statements
- ⚠️ Putting default arguments before required ones
- ⚠️ Using lambda for complex operations
- ⚠️ Not providing error messages with raise
- ⚠️ Calling attributes with parentheses

---

#python #comprehensive-quiz #dataengineering #certification-exam #intermediate-python