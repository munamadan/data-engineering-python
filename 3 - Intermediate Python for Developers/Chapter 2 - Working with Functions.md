
## 📚 Topics Covered

- Defining Custom Functions
- Default and Keyword Arguments
- Docstrings
- Arbitrary Arguments (*args and **kwargs)

---

## 🔨 Creating Custom Functions

### When to Create a Custom Function?

Consider these factors:

1. **Number of lines** - Code becomes repetitive
2. **Code complexity** - Task requires multiple steps
3. **Frequency of use** - Same task performed repeatedly
4. **DRY Principle** - Don't Repeat Yourself

### Example: Calculating Average Before Custom Function

```python
# Sales variable
sales = [125.97, 84.32, 99.78, 154.21, 78.50, 83.67, 111.13]

# Calculating average sales
average_sales = sum(sales) / len(sales)

# Rounding the results
rounded_average_sales = round(average_sales, 2)
print(rounded_average_sales)  # Output: 105.37
```

### Basic Function Structure

```python
def average(values):
    # Calculate the average
    average_value = sum(values) / len(values)
    # Round the results
    rounded_average = round(average_value, 2)
    # Return rounded_average as an output
    return rounded_average
```

**Function Components:**

- `def` - keyword to define function
- `average` - function name
- `(values)` - parameter(s)
- `:` - starts function body
- Indented code block - function logic
- `return` - output value

### Simplified Version

```python
def average(values):
    # Calculate the average
    average_value = sum(values) / len(values)
    # Return the rounded results
    return round(average_value, 2)
```

### Using the Custom Function

```python
sales = [125.97, 84.32, 99.78, 154.21, 78.50, 83.67, 111.13]

# Calling the function
average(sales)  # Returns: 105.37

# Storing the output
average_sales = average(sales)
print(average_sales)  # Output: 105.37
```

---

## 🎯 Arguments: Positional vs Keyword

### What are Arguments?

**Arguments** are values provided to a function or method.

Two types:

1. **Positional arguments**
2. **Keyword arguments**

### Positional Arguments

Arguments provided in order, separated by commas.

```python
# Round pi to 2 digits
round(3.1415926535, 2)  # Returns: 3.14
```

### Keyword Arguments

Arguments provided by assigning values to parameter names.

```python
# Round pi to 2 digits using keyword arguments
round(number=3.1415926535, ndigits=2)  # Returns: 3.14
```

**Benefits:**

- Clearer interpretation
- Easier to track arguments
- Order doesn't matter

### Identifying Keyword Arguments

Use `help()` to see available keyword arguments:

```python
help(round)
```

**Output:**

```
round(number, ndigits=None)
    Round a number to a given precision in decimal digits.
```

---

## 🔧 Default Arguments

### What are Default Arguments?

Default arguments set a default value for a parameter.

```python
round(number, ndigits=None)
```

- `ndigits=None` means if not provided, it defaults to `None`
- `None` = no value / empty
- If omitted, function returns an integer

### Why Use Default Arguments?

1. **Common use cases** - Set frequently used values as defaults
2. **Reduce code** - Users don't need to specify if using default
3. **Maintain flexibility** - Can still override when needed
4. **Better UX** - Makes function easier to use

### Adding a Default Argument

```python
def average(values, rounded=False):
    # Round average to two decimal places if rounded is True
    if rounded == True:
        average_value = sum(values) / len(values)
        rounded_average = round(average_value, 2)
        return rounded_average
    # Otherwise, don't round
    else:
        average_value = sum(values) / len(values)
        return average_value
```

### Using the Modified Function

```python
sales = [125.97, 84.32, 99.78, 154.21, 78.50, 83.67, 111.13]

# Get average without rounding (using default)
average(sales)  # Returns: 105.36857142857141

# Get average without rounding (explicitly)
average(sales, False)  # Returns: 105.36857142857141

# Get rounded average (using keyword argument)
average(values=sales, rounded=True)  # Returns: 105.37
```

---

## 📝 Docstrings

### What are Docstrings?

**Docstring** = String (block of text) describing a function

- Helps users understand how to use the function
- First string after function definition
- Enclosed in triple quotes `"""`

### Accessing Docstrings

#### Method 1: Using `help()`

```python
help(round)
```

#### Method 2: Using `__doc__` attribute

```python
round.__doc__
```

**Output:**

```
'Round a number to a given precision in decimal digits.\n\nThe return value is an integer if ndigits is omitted or None. Otherwise\nthe return value has the same type as the number. ndigits may be negative.'
```

> **Note:** `__doc__` is called "dunder-doc" (double underscore doc)

### Creating a One-Line Docstring

```python
def average(values):
    """Find the mean in a sequence of values and round to two decimal places."""
    average_value = sum(values) / len(values)
    rounded_average = round(average_value, 2)
    return rounded_average
```

**Accessing it:**

```python
average.__doc__
# Returns: 'Find the mean in a sequence of values and round to two decimal places.'
```

### Updating a Docstring

```python
average.__doc__ = "Calculate the mean of values in a data structure, rounding the results to 2 digits."
```

### Creating a Multi-Line Docstring

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

**Multi-line docstring format:**

1. **Summary line** - Brief description
2. **Args section** - Parameter descriptions with types
3. **Returns section** - Return value description with type

**Accessing with help():**

```python
help(average)
```

**Output:**

```
Help on function average in module __main__:

average(values)
    Find the mean in a sequence of values and round to two decimal places.
    
    Args:
        values (list): A list of numeric values.
    
    Returns:
        rounded_average (float): The mean of values, rounded to two decimal places.
```

---

## 🔀 Arbitrary Arguments

### The Problem with Fixed Arguments

```python
def average(values):
    average_value = sum(values) / len(values)
    return round(average_value, 2)

# This will cause an error
average(15, 29, 4, 13, 11, 8)
# TypeError: average() takes 1 positional argument but 6 were given
```

### Solution: Arbitrary Positional Arguments (*args)

**Arbitrary arguments** allow functions to accept any number of arguments.

```python
def average(*args):
    average_value = sum(args) / len(args)
    return round(average_value, 2)
```

**Key Points:**

- `*args` - conventional naming (can use any name with `*`)
- Converts all positional arguments into a single tuple
- Allows variable number of arguments

### Using *args

```python
# Calling with six positional arguments
average(15, 29, 4, 13, 11, 8)  # Returns: 13.33
```

### Unpacking Multiple Iterables

The `*` operator converts arguments to a single iterable:

```python
# Calculating across multiple lists
average(*[15, 29], *[4, 13], *[11, 8])  # Returns: 13.33
```

### Arbitrary Keyword Arguments (**kwargs)

Accept any number of keyword arguments:

```python
def average(**kwargs):
    average_value = sum(kwargs.values()) / len(kwargs.values())
    rounded_average = round(average_value, 2)
    return rounded_average
```

**Key Points:**

- `**kwargs` - conventional naming (keyword arguments)
- Creates a dictionary of key-value pairs
- Format: `keyword=value`

### Using **kwargs

```python
# Calling with six keyword arguments
average(a=15, b=29, c=4, d=13, e=11, f=8)  # Returns: 13.33

# Calling with a dictionary (unpacking)
average(**{"a":15, "b":29, "c":4, "d":13, "e":11, "f":8})  # Returns: 13.33
```

> **Note:** Each key-value pair in the dictionary is mapped to a keyword argument!

### Unpacking Multiple Dictionaries

```python
# Calling with three dictionaries
average(**{"a":15, "b":29}, **{"c":4, "d":13}, **{"e":11, "f":8})
# Returns: 13.33
```

---

## 📊 Comparison: *args vs **kwargs

|Feature|*args|**kwargs|
|---|---|---|
|**Syntax**|`*args`|`**kwargs`|
|**Type**|Tuple|Dictionary|
|**Arguments**|Positional|Keyword (name=value)|
|**Access**|By index: `args[0]`|By key: `kwargs['key']`|
|**Example Call**|`func(1, 2, 3)`|`func(a=1, b=2, c=3)`|
|**Unpacking**|`*[1, 2, 3]`|`**{'a':1, 'b':2}`|

---

## 🎯 Key Takeaways

1. **Custom functions** follow DRY principle and reduce code repetition
2. **Positional arguments** are passed by order
3. **Keyword arguments** are passed by name for clarity
4. **Default arguments** provide common values, maintaining flexibility
5. **Docstrings** document function purpose, parameters, and return values
6. **`__doc__`** attribute accesses docstrings programmatically
7. **`*args`** accepts any number of positional arguments (tuple)
8. **`**kwargs`** accepts any number of keyword arguments (dictionary)
9. **Multi-line docstrings** include Args and Returns sections
10. **Arbitrary arguments** make functions more flexible and versatile

---

## 💡 Best Practices

### Function Design

- Use descriptive function names
- Keep functions focused on one task
- Add docstrings to all custom functions
- Use default arguments for common values

### Arguments

- Use positional arguments for required parameters
- Use keyword arguments for optional/clarity
- Use *args for variable positional arguments
- Use **kwargs for variable keyword arguments

### Documentation

- Write clear, concise docstrings
- Document all parameters and return values
- Include type hints in docstrings
- Update docstrings when modifying functions

---

## 📝 Quick Reference

### Function Template

```python
def function_name(required_param, optional_param=default_value, *args, **kwargs):
    """
    Brief description of function.
    
    Args:
        required_param (type): Description
        optional_param (type): Description. Defaults to default_value.
        *args: Variable length argument list
        **kwargs: Arbitrary keyword arguments
    
    Returns:
        type: Description of return value
    """
    # Function logic
    return result
```

---

#python #dataengineering #datacamp #custom-functions #intermediate-python

## Section A: Multiple Choice Questions

### 1. Function Basics

**Q1.** Which keyword is used to define a function in Python?

- [ ] A) function
- [ ] B) def
- [ ] C) define
- [ ] D) func

**Q2.** What does the DRY principle stand for?

- [ ] A) Do Repeat Yourself
- [ ] B) Don't Repeat Yourself
- [ ] C) Define Reusable Yields
- [ ] D) Data Redundancy Yield

**Q3.** Which of the following is NOT a valid consideration for creating a custom function?

- [ ] A) Number of lines
- [ ] B) Code complexity
- [ ] C) Variable name length
- [ ] D) Frequency of use

**Q4.** What keyword is used to return a value from a function?

- [ ] A) output
- [ ] B) give
- [ ] C) return
- [ ] D) yield

### 2. Arguments

**Q5.** What is the difference between positional and keyword arguments?

- [ ] A) Positional are faster than keyword
- [ ] B) Positional use order, keyword use names
- [ ] C) Keyword arguments are required
- [ ] D) No difference

**Q6.** In the function call `round(3.14159, 2)`, what type of arguments are being used?

- [ ] A) Keyword arguments
- [ ] B) Positional arguments
- [ ] C) Default arguments
- [ ] D) Arbitrary arguments

**Q7.** What does `ndigits=None` indicate in a function signature?

- [ ] A) The parameter is required
- [ ] B) The parameter has a default value of None
- [ ] C) The parameter is invalid
- [ ] D) The function will error

**Q8.** Which statement about default arguments is TRUE?

- [ ] A) They must always be provided when calling the function
- [ ] B) They reduce flexibility
- [ ] C) They can be overridden when calling the function
- [ ] D) They can only be integers

### 3. Docstrings

**Q9.** What are docstrings enclosed in?

- [ ] A) Single quotes
- [ ] B) Double quotes
- [ ] C) Triple quotes
- [ ] D) Backticks

**Q10.** How do you access a function's docstring using the dunder method?

- [ ] A) `function.doc()`
- [ ] B) `function.__doc__`
- [ ] C) `function._doc_`
- [ ] D) `function.docstring`

**Q11.** What does "dunder-doc" refer to?

- [ ] A) Double underscore documentation
- [ ] B) Dynamic under documentation
- [ ] C) Default under documentation
- [ ] D) Duplicate underscore doc

**Q12.** In a multi-line docstring, which section describes the parameters?

- [ ] A) Parameters:
- [ ] B) Args:
- [ ] C) Inputs:
- [ ] D) Params:

### 4. Arbitrary Arguments

**Q13.** What does `*args` create inside a function?

- [ ] A) A list
- [ ] B) A tuple
- [ ] C) A dictionary
- [ ] D) A set

**Q14.** What does `**kwargs` create inside a function?

- [ ] A) A list
- [ ] B) A tuple
- [ ] C) A dictionary
- [ ] D) A set

**Q15.** What does the `*` operator do when used with a list in a function call?

- [ ] A) Multiplies the list
- [ ] B) Unpacks the list into separate arguments
- [ ] C) Creates a copy of the list
- [ ] D) Deletes the list

---

## Section B: Code Analysis Questions

**Q16.** What will be the output of this code?

```python
def calculate(a, b=10):
    return a + b

result = calculate(5)
print(result)
```

- [ ] A) 5
- [ ] B) 10
- [ ] C) 15
- [ ] D) Error

**Q17.** What is wrong with this function definition?

```python
def greet(name="User", age):
    return f"Hello {name}, you are {age}"
```

- [ ] A) Nothing is wrong
- [ ] B) Default arguments must come after non-default arguments
- [ ] C) Non-default arguments must come after default arguments
- [ ] D) Function names cannot be 'greet'

**Q18.** What will this return?

```python
def multiply(*args):
    result = 1
    for num in args:
        result *= num
    return result

multiply(2, 3, 4)
```

- [ ] A) 9
- [ ] B) 24
- [ ] C) 234
- [ ] D) Error

**Q19.** What will be the output?

```python
def display(**kwargs):
    return len(kwargs)

display(a=1, b=2, c=3, d=4)
```

- [ ] A) 10
- [ ] B) 4
- [ ] C) {'a':1, 'b':2, 'c':3, 'd':4}
- [ ] D) Error

**Q20.** What is the issue with this code?

```python
def average(values):
    """Calculate average"""
    return sum(values) / len(values)

average(10, 20, 30)
```

- [ ] A) Missing return statement
- [ ] B) Function expects 1 argument (list), but got 3 separate arguments
- [ ] C) Docstring is incorrect
- [ ] D) No issues

---

## Section C: Short Answer Questions

**Q21.** Explain the four considerations for deciding whether to create a custom function.

**Q22.** What is the difference between accessing a docstring with `help()` versus `.__doc__`?

**Q23.** Why would you use keyword arguments instead of positional arguments?

**Q24.** Describe the structure of a proper multi-line docstring with an example.

**Q25.** Explain how `*args` and `**kwargs` differ in terms of:

- How they store data
- How you call functions with them
- How you access values inside the function

---

## Section D: Code Writing Questions

**Q26.** Write a custom function called `calculate_total()` that:

- Accepts a list of prices
- Applies a 10% tax
- Returns the total rounded to 2 decimal places
- Include a proper docstring

**Q27.** Modify the function from Q26 to include a default argument `tax_rate=0.10` that can be customized.

**Q28.** Write a function called `concat_strings()` using `*args` that:

- Accepts any number of string arguments
- Concatenates them with a space between each
- Returns the final string
- Example: `concat_strings("Hello", "World", "Python")` returns `"Hello World Python"`

**Q29.** Create a function `person_info()` using `**kwargs` that:

- Accepts any number of keyword arguments
- Returns a formatted string with all key-value pairs
- Example: `person_info(name="John", age=30, city="NYC")` returns `"name: John, age: 30, city: NYC"`

**Q30.** Write a function with both regular arguments and arbitrary arguments:

```python
def order_summary(order_id, *items, **details):
    # Your code here
```

The function should return a formatted summary of an order.

---

## Section E: Debugging Questions

**Q31.** Fix this function:

```python
def add_numbers(*args)
    total = sum(args)
    return total
```

**Q32.** Fix this function call:

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet(greeting="Hi", "Alice")
```

**Q33.** Fix this docstring:

```python
def divide(a, b):
    'Divides two numbers'
    return a / b
```

**Q34.** What's wrong with this function?

```python
def process_data(default_value=10, required_data):
    return required_data + default_value
```

**Q35.** Fix this *args usage:

```python
def sum_all(args):
    return sum(args)

sum_all(1, 2, 3, 4, 5)
```

---

## Section F: Scenario-Based Questions

**Q36.** You need to create a function that calculates student grades. The function should:

- Accept a list of scores
- Have an optional parameter to apply extra credit (default 0)
- Return the average
- Include proper documentation

Write this function with appropriate arguments and docstring.

**Q37.** You're building a logging function that needs to accept varying amounts of information. Sometimes it gets just a message, sometimes a message with a timestamp, sometimes additional metadata. What type of arguments would be most appropriate and why?

**Q38.** You have three functions that all calculate averages but in slightly different ways. How would you refactor them into a single flexible function using default arguments?

**Q39.** You need to create a function that formats database queries. It always needs a table name, but might need various WHERE clauses. What combination of regular and arbitrary arguments would work best?

**Q40.** Explain when you would use:

- Regular positional arguments
- Default arguments
- `*args`
- `**kwargs`

Give a specific example for each.

---

## Section G: True/False Questions

**Q41.** The `return` statement is optional in Python functions.

- [ ] True
- [ ] False

**Q42.** Keyword arguments must always come before positional arguments in a function call.

- [ ] True
- [ ] False

**Q43.** You can have both `*args` and `**kwargs` in the same function.

- [ ] True
- [ ] False

**Q44.** The names `args` and `kwargs` are required syntax in Python.

- [ ] True
- [ ] False

**Q45.** A function can have multiple return statements.

- [ ] True
- [ ] False

**Q46.** Default arguments are evaluated once at function definition time.

- [ ] True
- [ ] False

**Q47.** Docstrings must be multi-line to be valid.

- [ ] True
- [ ] False

**Q48.** `*args` converts arguments to a list.

- [ ] True
- [ ] False

---

## Section H: Advanced Application

**Q49.** Write a flexible calculator function that:

```python
def calculator(operation, *numbers, precision=2, **options):
    """
    Your implementation here
    """
```

- Accepts an operation ('sum', 'multiply', 'average')
- Works with any number of values
- Has default precision for rounding
- Can accept additional options via kwargs
- Includes comprehensive docstring

**Q50.** Create a function decorator concept (advanced): Write a function that takes another function as an argument and adds timing functionality:

```python
def time_function(func, *args, **kwargs):
    """
    Times the execution of any function
    """
    # Your implementation
```

**Q51.** Analyze this code and explain what happens:

```python
def mystery_func(*args, separator="-", **kwargs):
    result = separator.join([str(x) for x in args])
    for key, value in kwargs.items():
        result += f"\n{key}: {value}"
    return result

output = mystery_func(1, 2, 3, separator="->", name="Test", value=100)
```

What is the output and why?

**Q52.** Write a function that validates input types using docstring conventions:

```python
def process_payment(amount, currency="USD", **transaction_details):
    """
    Process a payment transaction
    
    Args:
        amount (float): Payment amount, must be positive
        currency (str): Currency code (default: USD)
        **transaction_details: Additional transaction metadata
            card_number (str): Credit card number
            cvv (str): Security code
    
    Returns:
        dict: Transaction confirmation with status and receipt_id
        
    Raises:
        ValueError: If amount is negative or zero
        KeyError: If required transaction details are missing
    """
    # Implement with proper validation
```

---

## Section I: Comparison Questions

**Q53.** Compare these two function designs:

```python
# Design A
def calculate_price(base_price, tax, discount, shipping):
    return base_price + tax - discount + shipping

# Design B
def calculate_price(base_price, **charges):
    total = base_price
    for charge_type, amount in charges.items():
        if charge_type == 'discount':
            total -= amount
        else:
            total += amount
    return total
```

Which design is better and why? What are the trade-offs?

**Q54.** Compare using `*args` vs accepting a list parameter:

```python
# Option 1
def sum_values(*args):
    return sum(args)

# Option 2
def sum_values(values):
    return sum(values)
```

When would you use each approach?

---

## Section J: Real-World Application

**Q55.** You're building an API client that makes HTTP requests. Design a function signature that:

- Requires a URL
- Has a default HTTP method of 'GET'
- Can accept any number of headers
- Can accept any number of query parameters
- Include proper docstring

**Q56.** Create a data validation function for a data engineering pipeline:

```python
def validate_data(dataframe, *required_columns, allow_nulls=False, **column_types):
    """
    Your implementation and docstring
    """
```

---

## Answer Key Location

> Answers are provided in a separate document: `Chapter_2_Answer_Key.md`

---

## Study Tips

1. **Practice defining functions** - Start simple, add complexity gradually
2. **Experiment with arguments** - Try different combinations of positional, keyword, and arbitrary arguments
3. **Write docstrings immediately** - Make it a habit when creating functions
4. **Test your functions** - Call them with different argument types
5. **Use `help()`** - Check docstrings of both built-in and custom functions
6. **Refactor existing code** - Look for opportunities to extract repeated logic into functions
7. **Read other people's functions** - Study well-documented open-source code

---

## Common Mistakes to Avoid

❌ Forgetting the colon after function definition ❌ Not indenting function body ❌ Putting default arguments before non-default ones ❌ Forgetting to return a value ❌ Using mutable default arguments (like lists or dictionaries) ❌ Not documenting functions ❌ Making functions too complex (do one thing well)

---

#python #quiz #dataengineering #custom-functions #certification-prep