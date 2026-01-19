
## 📚 Topics Covered

- Lambda Functions
- Introduction to Errors
- Error Handling (try-except and raise)
- Course Recap

---

## 🔹 Lambda Functions

### What are Lambda Functions?

**Lambda function** = Anonymous function (function without a name)

- Uses the `lambda` keyword
- Compact, one-line function
- No `return` statement needed (implicit return)
- Best for simple operations

### Syntax Structure

```python
lambda argument(s): expression
```

**Components:**

- `lambda` - keyword to define lambda function
- `argument(s)` - input parameters
- `:` - separates arguments from expression
- `expression` - the function body (what gets returned)

### Comparison: Regular vs Lambda Function

```python
# Regular custom function
def average(x):
    return sum(x) / len(x)

average
# Output: <function __main__.average(x)>

# Lambda function
lambda x: sum(x) / len(x)
# Output: <function __main__.<lambda>(x)>
```

### Convention

- Use `x` for a single argument
- The expression is equivalent to the function body
- No `return` statement required (automatically returns expression result)

---

## 🎯 Using Lambda Functions

### Method 1: Direct Call (Wrapped in Parentheses)

```python
# Call lambda function immediately
(lambda x: sum(x) / len(x))([3, 6, 9])
# Output: 6.0
```

### Method 2: Store in a Variable

```python
# Store lambda function as a variable
average = lambda x: sum(x) / len(x)

# Call the average function
average([3, 6, 9])
# Output: 6.0
```

### Multiple Parameters

```python
# Lambda function with two arguments
(lambda x, y: x**y)(2, 3)
# Output: 8
```

---

## 🔄 Lambda Functions with Iterables

### Using `map()` Function

**`map()`** applies a function to all elements in an iterable.

```python
names = ["john", "sally", "leah"]

# Apply a lambda function inside map()
capitalize = map(lambda x: x.capitalize(), names)
print(capitalize)
# Output: <map object at 0x7fb200529c10>

# Convert to a list to see results
list(capitalize)
# Output: ['John', 'Sally', 'Leah']
```

**Key Points:**

- `map()` returns a map object (iterator)
- Must convert to list to see actual values
- Lambda function is applied to each element

---

## 📊 When to Use: Custom vs Lambda Functions

|Scenario|Function Type|Reason|
|---|---|---|
|Complex task|**Custom Function**|Better readability and debugging|
|Same task several times|**Custom Function**|Reusability and documentation|
|Simple task|**Lambda Function**|Concise and efficient|
|Performed once|**Lambda Function**|No need for formal definition|

### Decision Guidelines

**Use Lambda when:**

- One-line operation
- Used once or within another function
- Simple transformation
- Working with map(), filter(), sorted(), etc.

**Use Custom Function when:**

- Multiple lines of logic
- Need documentation (docstrings)
- Used multiple times
- Complex operations
- Need debugging

---

## ⚠️ Introduction to Errors

### What are Errors?

**Error = Exception**

- Code that violates one or more rules
- Causes code to terminate
- Python has built-in exception hierarchy

📖 **Reference:** [Python Exception Hierarchy](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)

### Common Error Types

#### TypeError

**Cause:** Incorrect data type used

```python
"Hello" + 5
# TypeError: can only concatenate str (not "int") to str
```

#### ValueError

**Cause:** Value not in acceptable range

```python
float("Hello")
# ValueError: could not convert string to float: 'Hello'

# This works:
float("2")
# Output: 2.0
```

---

## 🔍 Understanding Tracebacks

### What is a Traceback?

**Traceback** = Error message showing:

1. Where the error occurred
2. What type of error
3. The sequence of function calls leading to the error

### Tracebacks in Packages

When using packages like pandas:

- Packages contain other people's code (source code)
- `pip install <package>` downloads source code locally
- Functions execute code written by package authors
- Tracebacks show path through package code

#### Example: Pandas KeyError

```python
import pandas as pd

# Create pandas DataFrame
products = pd.DataFrame({"ID": "ABC1", "price": 29.99})

# Try to access non-existent column
products["tag"]
# KeyError: 'tag'
```

**Traceback shows:**

- Line in your code that caused error
- Path through pandas source code
- Final error type and message

---

## 🛡️ Error Handling

### Design Thinking for Error Handling

**Questions to Ask:**

1. How might people use our custom function?
2. What different approaches might they try?
3. What errors could occur?

**Potential Issues:**

- Providing more than expected arguments
- Using wrong data type
- Missing required parameters
- Invalid values

### Example Problem

```python
def average(values):
    average_value = sum(values) / len(values)
    return average_value

# What if user provides a dictionary?
sales_dict = {"cust_id": ["JL93", "MT12", "IY64"],
              "order_value": [43.21, 68.70, 82.19]}

average(sales_dict)
# TypeError: unsupported operand type(s)
```

---

## 🔧 Error Handling Techniques

### 1. Control Flow (if/elif/else)

Simple conditional checks

### 2. Docstrings

Document expected inputs and usage

### 3. try-except

Catch and handle errors gracefully

### 4. raise

Explicitly raise custom errors

---

## 🎯 try-except Block

### Purpose

- Prevent errors from terminating program
- Allow code to continue executing
- Provide user-friendly error messages

### Syntax

```python
def average(values):
    try:
        # Code that might cause an error
        average_value = sum(values) / len(values)
        return average_value
    except:
        # Code to run if an error occurs
        print("average() accepts a list or set. Please provide a correct data type.")
```

### Usage

```python
average(sales_dict)
# Output: average() accepts a list or set. Please provide a correct data type.
# Program continues running
```

**Key Points:**

- Code in `try` block executes first
- If error occurs, `except` block runs
- Program does NOT terminate
- Subsequent code continues to execute

---

## 🚨 raise Statement

### Purpose

- Explicitly raise an error
- Stop code execution
- Provide specific error type and message

### Basic Structure

```python
def average(values):
    # Check data type
    if type(values) in ["list", "set"]:
        # Run if appropriate data type was used
        average_value = sum(values) / len(values)
        return average_value
    else:
        # Run if an Exception occurs
        raise
```

### Raising Specific Error Types

```python
def average(values):
    # Check data type
    if type(values) not in [list, set]:
        # Raise specific error with custom message
        raise TypeError("average() accepts a list or set, please provide a correct data type.")
    
    # Calculate average
    average_value = sum(values) / len(values)
    return average_value
```

### Usage

```python
average(sales_dict)
# TypeError: average() accepts a list or set, please provide a correct data type.
# Program STOPS executing
```

---

## ⚖️ try-except vs raise

|Feature|try-except|raise|
|---|---|---|
|**Error Handling**|Catches and handles errors|Creates and raises errors|
|**Program Flow**|Continues execution|Stops execution|
|**Use Case**|Graceful error recovery|Input validation|
|**Error Message**|Custom message in except|Custom error with message|
|**Best For**|Optional operations|Required validations|

### When to Use Each

**Use try-except when:**

- Error is expected and recoverable
- Want program to continue despite error
- Providing fallback behavior
- Logging errors but continuing

**Use raise when:**

- Invalid input must be rejected
- Error should stop execution
- Enforcing strict requirements
- Clear failure is better than wrong result

---

## 🎓 Course Recap

### Chapter 1: Built-in Functions, Modules & Packages

**Built-in Functions:**

- `print()`, `help()`, `type()`
- `max()`, `min()`, `sum()`
- `len()`, `round()`, `sorted()`

**Modules:**

- `collections`, `string`, `os`, `logging`, `subprocess`

**Packages:**

- `pandas` - DataFrames and data manipulation

### Chapter 2: Custom Functions

**Basic Function:**

```python
def average(values):
    average_value = sum(values) / len(values)
    rounded_average = round(average_value, 2)
    return rounded_average
```

**Default Arguments:**

```python
def average(values, rounded=False):
    if rounded == True:
        average_value = sum(values) / len(values)
        rounded_average = round(average_value, 2)
        return rounded_average
    else:
        average_value = sum(values) / len(values)
        return average_value
```

**Docstrings:**

```python
def average(values):
    """
    Find the mean in a sequence of values and round to two decimal places.
    
    Args:
        values (list): A list of numeric values.
    
    Returns:
        rounded_average (float): The mean of values, rounded to two decimal places.
    """
    average_value = sum(values) / len(values)
    rounded_average = round(average_value, 2)
    return rounded_average
```

**Arbitrary Arguments:**

```python
# Positional
def average(*args):
    average_value = sum(args) / len(args)
    rounded_average = round(average_value, 2)
    return rounded_average

# Keyword
def average(**kwargs):
    average_value = sum(kwargs.values()) / len(kwargs.values())
    rounded_average = round(average_value, 2)
    return rounded_average
```

### Chapter 3: Lambda Functions & Error Handling

**Lambda Functions:**

```python
lambda argument(s): expression

# Example with map()
names = ["john", "sally", "leah"]
capitalize = map(lambda x: x.capitalize(), names)
list(capitalize)
# Output: ['John', 'Sally', 'Leah']
```

**Error Handling:**

- `try-except` - Graceful error handling
- `raise` - Explicit error raising

---

## 🚀 Next Steps

### Additional Built-in Functions to Explore

- `zip()` - Combine multiple iterables
- `input()` - Get user input
- `reduce()` - Apply function cumulatively
- `filter()` - Filter elements by condition

### More Packages and Modules

- `time` - Time-related functions
- `venv` - Virtual environments
- `requests` - HTTP requests
- `fastapi` - Web API framework

### Advanced Topics

- **Object-Oriented Programming (OOP)** - Classes and objects

---

## 🎯 Key Takeaways

1. **Lambda functions** are anonymous, one-line functions for simple operations
2. **`map()`** applies a function to all elements in an iterable
3. **Errors** (exceptions) cause code to terminate if not handled
4. **Tracebacks** show the path of error through code
5. **try-except** catches errors and allows code to continue
6. **raise** creates explicit errors to stop execution
7. **TypeError** occurs with incorrect data types
8. **ValueError** occurs with unacceptable values
9. Lambda functions are best for **simple, one-time operations**
10. Custom functions are best for **complex, reusable operations**

---

## 💡 Best Practices

### Lambda Functions

- Keep expressions simple and readable
- Use for one-time operations
- Prefer custom functions for complex logic
- Use with map(), filter(), sorted()

### Error Handling

- Anticipate how users might misuse functions
- Provide clear, helpful error messages
- Use specific error types (TypeError, ValueError, etc.)
- Document expected inputs in docstrings
- Use try-except for recoverable errors
- Use raise for validation errors

### Code Design

- Think about edge cases
- Test with different input types
- Document expected behavior
- Balance error handling with code readability

---

## 📝 Common Patterns

### Lambda with map()

```python
# Transform all elements
result = list(map(lambda x: x * 2, [1, 2, 3, 4]))
```

### Lambda with filter()

```python
# Filter elements
result = list(filter(lambda x: x > 5, [3, 7, 2, 9, 4]))
```

### Lambda with sorted()

```python
# Custom sorting
data = [(1, 'b'), (2, 'a'), (3, 'c')]
sorted(data, key=lambda x: x[1])
```

### Comprehensive Error Handling

```python
def process_data(data):
    """Process data with comprehensive error handling."""
    try:
        # Validate input type
        if not isinstance(data, (list, tuple)):
            raise TypeError("Data must be a list or tuple")
        
        # Validate content
        if not data:
            raise ValueError("Data cannot be empty")
        
        # Process data
        result = sum(data) / len(data)
        return result
        
    except (TypeError, ValueError) as e:
        print(f"Error: {e}")
        return None
```

---

#python #dataengineering #datacamp #lambda-functions #error-handling #intermediate-python

## Section A: Multiple Choice Questions

### 1. Lambda Functions Basics

**Q1.** What keyword is used to create a lambda function?

- [ ] A) def
- [ ] B) lambda
- [ ] C) function
- [ ] D) anonymous

**Q2.** Lambda functions are also known as:

- [ ] A) Named functions
- [ ] B) Anonymous functions
- [ ] C) Complex functions
- [ ] D) Class functions

**Q3.** What is the conventional name for a single argument in a lambda function?

- [ ] A) arg
- [ ] B) param
- [ ] C) x
- [ ] D) input

**Q4.** Do lambda functions require a return statement?

- [ ] A) Yes, always
- [ ] B) No, return is implicit
- [ ] C) Only for multiple lines
- [ ] D) Only for complex operations

**Q5.** What does `map()` return?

- [ ] A) A list
- [ ] B) A tuple
- [ ] C) A map object
- [ ] D) A dictionary

### 2. Error Types

**Q6.** What error occurs when you try to add a string and an integer?

- [ ] A) ValueError
- [ ] B) TypeError
- [ ] C) SyntaxError
- [ ] D) AttributeError

**Q7.** What error occurs when converting "Hello" to a float?

- [ ] A) TypeError
- [ ] B) ValueError
- [ ] C) ConversionError
- [ ] D) CastError

**Q8.** What is another name for an error in Python?

- [ ] A) Bug
- [ ] B) Exception
- [ ] C) Fault
- [ ] D) Issue

**Q9.** What shows the sequence of function calls leading to an error?

- [ ] A) Error log
- [ ] B) Debug output
- [ ] C) Traceback
- [ ] D) Stack print

### 3. Error Handling

**Q10.** Which statement catches and handles errors?

- [ ] A) catch-except
- [ ] B) try-except
- [ ] C) handle-error
- [ ] D) catch-error

**Q11.** Which statement explicitly raises an error?

- [ ] A) throw
- [ ] B) error
- [ ] C) raise
- [ ] D) except

**Q12.** What happens to code execution after a `raise` statement?

- [ ] A) Continues normally
- [ ] B) Stops executing
- [ ] C) Skips to next function
- [ ] D) Restarts the program

**Q13.** What happens to code execution after a `try-except` block catches an error?

- [ ] A) Stops executing
- [ ] B) Continues executing
- [ ] C) Restarts the function
- [ ] D) Raises a new error

---

## Section B: Code Analysis Questions

**Q14.** What will be the output?

```python
result = (lambda x: x * 2)(5)
print(result)
```

- [ ] A) 5
- [ ] B) 10
- [ ] C) 25
- [ ] D) Error

**Q15.** What is the output of this code?

```python
numbers = [1, 2, 3, 4]
squared = map(lambda x: x**2, numbers)
print(squared)
```

- [ ] A) [1, 4, 9, 16]
- [ ] B) <map object at ...>
- [ ] C) None
- [ ] D) Error

**Q16.** What will happen?

```python
try:
    result = 10 / 0
    print(result)
except:
    print("Error occurred")
print("Program continues")
```

- [ ] A) Prints result only
- [ ] B) Prints "Error occurred" and "Program continues"
- [ ] C) Program crashes
- [ ] D) Prints nothing

**Q17.** What will this code do?

```python
def validate(value):
    if value < 0:
        raise ValueError("Value must be positive")
    return value * 2

validate(-5)
```

- [ ] A) Returns -10
- [ ] B) Returns 0
- [ ] C) Raises ValueError and stops
- [ ] D) Prints error message and continues

**Q18.** What is wrong with this lambda function?

```python
calculate = lambda x:
    result = x * 2
    return result
```

- [ ] A) Missing parentheses
- [ ] B) Lambda functions cannot have multiple lines
- [ ] C) Missing semicolon
- [ ] D) Nothing is wrong

**Q19.** What will be the output?

```python
func = lambda x, y: x + y
result = func(3, 4)
print(result)
```

- [ ] A) 34
- [ ] B) 7
- [ ] C) Error
- [ ] D) None

**Q20.** Identify the issue:

```python
numbers = [1, 2, 3]
result = map(lambda x: x * 2, numbers)
print(result[0])
```

- [ ] A) Lambda syntax error
- [ ] B) Map objects don't support indexing
- [ ] C) Missing import
- [ ] D) No issue

---

## Section C: Short Answer Questions

**Q21.** Explain the key differences between a custom function and a lambda function.

**Q22.** When should you use a lambda function versus a custom function? Provide specific scenarios.

**Q23.** What is a traceback and what information does it provide?

**Q24.** Explain the difference between `try-except` and `raise` in terms of program flow.

**Q25.** Why must we convert a map object to a list to see the results?

---

## Section D: Code Writing Questions

**Q26.** Write a lambda function that:

- Takes a single number as input
- Returns the square of that number
- Call it with the value 7

**Q27.** Use `map()` and a lambda function to convert this list of temperatures from Celsius to Fahrenheit:

```python
celsius = [0, 10, 20, 30, 40]
# Formula: F = (C * 9/5) + 32
```

**Q28.** Write a function with try-except that:

- Attempts to convert a string to an integer
- Returns the integer if successful
- Returns 0 if conversion fails
- Prints an error message in the except block

**Q29.** Write a function that uses `raise` to validate:

- Input must be a positive number
- Raises ValueError if negative
- Raises TypeError if not a number
- Returns the square root if valid

**Q30.** Create a lambda function that takes two arguments and returns the larger of the two.

---

## Section E: Debugging Questions

**Q31.** Fix this lambda function:

```python
double = lambda x return x * 2
```

**Q32.** Fix this try-except block:

```python
def divide(a, b):
    try
        result = a / b
        return result
    except:
        print("Cannot divide by zero")
```

**Q33.** Fix this raise statement:

```python
def check_age(age):
    if age < 0:
        raise "Age cannot be negative"
    return age
```

**Q34.** Fix this map() usage:

```python
numbers = [1, 2, 3, 4]
doubled = map(lambda x: x * 2)
print(list(doubled))
```

**Q35.** What's wrong with this error handling?

```python
def process(data):
    if type(data) != list:
        raise TypeError
    return sum(data)
```

---

## Section F: Scenario-Based Questions

**Q36.** You need to quickly transform a list of prices by applying a 15% discount. Should you use a lambda function or a custom function? Write the code.

**Q37.** You're validating user input for a registration form. Age must be between 18 and 100. Write a function using `raise` to validate this.

**Q38.** You're reading data from multiple files, and some might be corrupted. Use try-except to handle potential errors while continuing to process other files.

**Q39.** You have a list of strings and need to filter out any that are shorter than 5 characters. Use a lambda function with `filter()`.

**Q40.** You're building a calculator function. What error handling approach (try-except or raise) would you use for:

- Division by zero
- Invalid operator
- Non-numeric inputs

---

## Section G: True/False Questions

**Q41.** Lambda functions can have multiple lines of code.

- [ ] True
- [ ] False

**Q42.** A map object must be converted to a list or tuple to see its values.

- [ ] True
- [ ] False

**Q43.** The `raise` statement allows code to continue executing after an error.

- [ ] True
- [ ] False

**Q44.** Lambda functions can have multiple arguments.

- [ ] True
- [ ] False

**Q45.** TypeError occurs when a value is outside an acceptable range.

- [ ] True
- [ ] False

**Q46.** Try-except blocks prevent errors from terminating the program.

- [ ] True
- [ ] False

**Q47.** Lambda functions require a name.

- [ ] True
- [ ] False

**Q48.** You can store a lambda function in a variable.

- [ ] True
- [ ] False

---

## Section H: Comparison Questions

**Q49.** Compare these two approaches:

```python
# Approach A
def square(x):
    return x ** 2
result = list(map(square, [1, 2, 3, 4]))

# Approach B
result = list(map(lambda x: x ** 2, [1, 2, 3, 4]))
```

When would you use each approach?

**Q50.** Compare these error handling methods:

```python
# Method A
def divide_a(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return None

# Method B
def divide_b(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b
```

What are the trade-offs?

---

## Section I: Advanced Application

**Q51.** Write a function that uses both try-except and raise:

```python
def safe_divide(a, b, return_none_on_error=True):
    """
    Safely divide two numbers with flexible error handling.
    
    Args:
        a: Numerator
        b: Denominator
        return_none_on_error: If True, return None on error; if False, raise error
    
    Returns:
        Result of division or None
    """
    # Your implementation
```

**Q52.** Create a data processing pipeline using lambda functions:

```python
# Given data
transactions = [
    {"amount": 100, "fee": 5},
    {"amount": 250, "fee": 10},
    {"amount": 75, "fee": 3}
]

# Use map() with lambda to:
# 1. Calculate net amount (amount - fee) for each transaction
# 2. Apply 8% tax to net amount
# 3. Round to 2 decimal places
```

**Q53.** Implement comprehensive error handling:

```python
def calculate_average(values, exclude_negatives=False):
    """
    Calculate average with robust error handling.
    
    Should handle:
    - Non-iterable inputs (raise TypeError)
    - Empty lists (raise ValueError)
    - Non-numeric values (try-except with skip)
    - Negative values (optional exclusion)
    """
    # Your implementation
```

**Q54.** Chain multiple lambda operations:

```python
names = ["  john doe  ", "  JANE SMITH  ", "  bob jones  "]

# Use map() with nested lambdas or multiple map() calls to:
# 1. Strip whitespace
# 2. Convert to title case
# 3. Replace spaces with underscores
```

---

## Section J: Real-World Application

**Q55.** You're processing customer orders. Write error handling for:

```python
def process_order(order_id, items, payment_method):
    """
    Process a customer order with validation.
    
    Validations needed:
    - order_id must be a string
    - items must be a non-empty list
    - payment_method must be in ['credit', 'debit', 'cash']
    
    Use raise for validation errors
    Use try-except for processing errors
    """
    # Your implementation
```

**Q56.** Create a data cleaning function using lambda:

```python
# Given messy data
prices = ["$10.50", "$25.99", "$5.00", "$invalid", "$100.00"]

# Use map() and lambda to:
# 1. Remove '$' symbol
# 2. Convert to float
# 3. Handle invalid values (hint: use try-except in lambda or filter first)
```

**Q57.** Build an error logging system:

```python
def log_and_handle(func, *args, **kwargs):
    """
    Wrapper function that:
    - Executes the given function
    - Catches any errors
    - Logs error details
    - Re-raises the error
    """
    # Your implementation
```

---

## Section K: Error Type Identification

**Q58.** Identify the error type for each scenario:

a) `"Hello" + 5` b) `float("abc")` c) `my_list[10]` when list has only 5 elements d) `my_dict["nonexistent_key"]` e) `10 / 0`

**Q59.** What error type should you raise for these validations?

a) Age must be positive b) Input must be a list c) File must exist d) Username must be unique

---

## Section L: Practical Exercises

**Q60.** Convert this custom function to a lambda:

```python
def add_tax(price):
    return price * 1.08
```

**Q61.** Convert this lambda to a custom function with proper docstring:

```python
calculate = lambda x, y: (x + y) / 2
```

**Q62.** Add error handling to this function:

```python
def get_first_element(collection):
    return collection[0]
```

**Q63.** Use lambda with sorted() to sort this list of dictionaries by age:

```python
people = [
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 25},
    {"name": "Charlie", "age": 35}
]
```

**Q64.** Create a validation function that uses multiple raise statements:

```python
def validate_user_data(username, email, age):
    """
    Validate user registration data.
    Raise appropriate errors for:
    - Username too short (< 3 chars)
    - Email without '@'
    - Age not between 13 and 120
    """
    # Your implementation
```

---

## Section M: Mixed Concepts

**Q65.** Combine lambda functions and error handling:

```python
# Process a list of strings that might contain invalid numbers
data = ["10", "20", "invalid", "30", "40"]

# Use map() with a lambda that safely converts to int
# Invalid values should become 0
```

**Q66.** Create a function that uses all concepts from Chapter 3:

```python
def process_data(*args, strict_mode=False, **kwargs):
    """
    Process data using lambda functions and error handling.
    
    - Accept arbitrary arguments
    - Use lambda for transformations
    - Handle errors differently based on strict_mode
    - If strict_mode=True, raise errors
    - If strict_mode=False, use try-except and continue
    """
    # Your implementation
```

---

## Answer Key Location

> Answers are provided in a separate document: `Chapter_3_Answer_Key.md`

---

## Study Tips

1. **Practice lambda syntax** - Write simple lambdas before complex ones
2. **Experiment with map()** - Try different transformations
3. **Intentionally cause errors** - Learn from tracebacks
4. **Try both error handling approaches** - Understand when to use each
5. **Convert between lambda and custom** - Understand the trade-offs
6. **Read error messages carefully** - They tell you what went wrong
7. **Use try-except for learning** - Catch errors to understand them

---

## Common Mistakes to Avoid

❌ Trying to use multiple lines in lambda functions ❌ Forgetting to convert map objects to lists ❌ Using bare `except:` without specifying error type ❌ Using `raise` without an error type or message ❌ Forgetting the colon in try-except blocks ❌ Using lambda when a custom function would be clearer ❌ Not providing helpful error messages ❌ Raising exceptions for recoverable errors

---

## Key Patterns to Remember

### Lambda with map()

```python
result = list(map(lambda x: transformation(x), iterable))
```

### Try-Except Pattern

```python
try:
    # risky operation
except SpecificError:
    # handle error
```

### Raise Pattern

```python
if not valid_condition:
    raise ErrorType("Clear error message")
```

### Combined Pattern

```python
try:
    if not valid:
        raise ValueError("Invalid input")
    # process
except ValueError as e:
    # handle validation error
```

---

#python #quiz #dataengineering #lambda-functions #error-handling #certification-prep