## Object-Oriented Programming (OOP)

Object-oriented programming is a programming paradigm that organizes code around objects and classes, enabling code reuse and modularity.

---

## Anatomy of a Class

### Basic Class Structure

**File: `work_dir/my_package/my_class.py`**
```python
# Define a minimal class with an attribute
class MyClass:
    """A minimal example class
    
    :param value: value to set as the ``attribute`` attribute
    :ivar attribute: contains the contents of ``value`` passed in init
    """
    # Method to create a new instance of MyClass
    def __init__(self, value):
        # Define attribute with the contents of the value param
        self.attribute = value
```

### Key Components

1. **Class definition**: `class MyClass:`
2. **Docstring**: Documentation describing the class
3. **`__init__` method**: Constructor that creates new instances
4. **`self` parameter**: Reference to the instance being created
5. **Attributes**: Data stored in the instance (`self.attribute`)

---

## Using a Class in a Package

### Making Classes Available

**File: `work_dir/my_package/__init__.py`**
```python
from .my_class import MyClass
```

### Using the Class

**File: `work_dir/my_script.py`**
```python
import my_package

# Create instance of MyClass
my_instance = my_package.MyClass(value='class attribute value')

# Print out class attribute value
print(my_instance.attribute)
```

**Output:**
```
'class attribute value'
```

---

## The `self` Convention

### What is `self`?

- **`self`** represents the instance of the class
- It's the first parameter in all instance methods
- Used to access attributes and methods of the instance
- Automatically passed by Python (you don't pass it when calling methods)

### How `self` Works

```python
class MyClass:
    def __init__(self, value):
        self.attribute = value  # self refers to the instance being created

# When you create an instance:
my_instance = my_package.MyClass(value='class attribute value')
# Python automatically passes my_instance as self to __init__
```

---

## Extending the Document Class

### Basic Document Class

```python
class Document:
    def __init__(self, text):
        self.text = text
```

### Adding Tokenization

#### Step 1: Revise `__init__`

```python
class Document:
    def __init__(self, text):
        self.text = text
        self.tokens = self._tokenize()
```

**Usage:**
```python
doc = Document('test doc')
print(doc.tokens)
```

**Output:**
```
['test', 'doc']
```

#### Step 2: Add `_tokenize()` Method

```python
# Import function to perform tokenization
from .token_utils import tokenize

class Document:
    def __init__(self, text, token_regex=r'[a-zA-Z]+'):
        self.text = text
        self.tokens = self._tokenize()
    
    def _tokenize(self):
        return tokenize(self.text)
```

---

## Non-Public Methods

### Naming Convention

- Methods starting with underscore (`_`) are **non-public** (also called "private")
- Example: `_tokenize()`, `_count_words()`
- Convention indicates: "internal use only, may change"

### Characteristics of Non-Public Methods

1. **Lack of documentation** - Often not documented in public API docs
2. **Unpredictability** - May change or be removed in future versions

### Risks of Using Non-Public Methods

- **Lack of documentation**: Harder to understand how to use them
- **Unpredictability**: No guarantee they'll work the same way in future versions

> [!warning] Best Practice
> Avoid using non-public methods from external code unless absolutely necessary. They're meant for internal implementation details.

---

## The DRY Principle

### What is DRY?

**DRY = Don't Repeat Yourself**

- Write code once, reuse it multiple times
- Reduces duplication
- Makes maintenance easier
- Prevents inconsistencies

### DRY and Classes

Classes help follow DRY by:
- Encapsulating related functionality
- Enabling code reuse through inheritance
- Providing a single source of truth for behavior

---

## Inheritance

### Introduction to Inheritance

**Inheritance** allows a class (child) to inherit attributes and methods from another class (parent).

### Benefits of Inheritance

- Code reuse
- Logical hierarchy
- Extensibility
- Follows DRY principle

---

## Inheritance in Python

### Basic Inheritance Syntax

```python
# Import ParentClass object
from .parent_class import ParentClass

# Create a child class with inheritance
class ChildClass(ParentClass):
    def __init__(self):
        # Call parent's __init__ method
        ParentClass.__init__(self)
        # Add attribute unique to child class
        self.child_attribute = "I'm a child class attribute!"
```

### Using Inherited Attributes

```python
# Create a ChildClass instance
child_class = ChildClass()
print(child_class.child_attribute)  # Child's own attribute
print(child_class.parent_attribute)  # Inherited from parent
```

### Key Points

- Child class inherits all parent attributes and methods
- Child can add new attributes/methods
- Child can override parent methods
- Must call parent's `__init__` if needed

---

## Multilevel Inheritance

### What is Multilevel Inheritance?

A class inherits from a child class, creating a hierarchy:
- **Parent** → **Child** → **Grandchild**

### Example Hierarchy

```
Document (parent)
    ↓
SocialMedia (child)
    ↓
Tweet (grandchild)
```

---

## Multiple Inheritance

**Multiple inheritance** occurs when a class inherits from more than one parent class.

```python
class Child(Parent1, Parent2):
    pass
```

> [!note]
> The chapter mentions multiple inheritance but focuses on multilevel inheritance examples.

---

## Using `super()`

### What is `super()`?

`super()` is a function that:
- Calls methods from parent class
- Cleaner than directly naming the parent class
- Essential for multilevel inheritance

### Basic `super()` Example

```python
class Parent:
    def __init__(self):
        print("I'm a parent!")

class Child(Parent):
    def __init__(self):
        Parent.__init__()  # Direct parent call
        print("I'm a child!")

class SuperChild(Child):
    def __init__(self):
        super().__init__()  # Using super()
        print("I'm a super child!")
```

### Multilevel Inheritance with `super()`

```python
class Parent:
    def __init__(self):
        print("I'm a parent!")

class SuperChild(Parent):
    def __init__(self):
        super().__init__()
        print("I'm a super child!")

class Grandchild(SuperChild):
    def __init__(self):
        super().__init__()
        print("I'm a grandchild!")

grandchild = Grandchild()
```

**Output:**
```
I'm a parent!
I'm a super child!
I'm a grandchild!
```

### How `super()` Works

- Calls the parent class's method in the inheritance chain
- Automatically handles method resolution order (MRO)
- Each `super().__init__()` calls the next parent up the chain

---

## Keeping Track of Inherited Attributes

### Using `dir()` Function

The `dir()` function shows all attributes and methods of an object:

```python
# Create an instance of SocialMedia
sm = SocialMedia('@DataCamp #DataScience #Python #sklearn')

# What methods does sm have?
dir(sm)
```

**Output (partial):**
```python
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__',
'__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__',
'__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__',
'__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__',
'__str__', '__subclasshook__', '__weakref__', '_count_hashtags',
'_count_mentions', '_count_words', '_tokenize', 'hashtag_counts',
'mention_counts', 'text', 'tokens', 'word_counts']
```

### What `dir()` Shows

- **Dunder methods** (double underscore): `__init__`, `__str__`, etc.
- **Non-public methods** (single underscore): `_tokenize`, `_count_words`
- **Public attributes**: `text`, `tokens`, `word_counts`
- **Public methods**: `hashtag_counts`, `mention_counts`

> [!tip] Useful for Debugging
> Use `dir()` to explore what's available on an object, especially with inherited classes.

---

## Quick Reference

### Class Definition Template
```python
class ClassName:
    """Class docstring
    
    :param param_name: parameter description
    :ivar attribute_name: attribute description
    """
    
    def __init__(self, param):
        self.attribute = param
    
    def public_method(self):
        return self.attribute
    
    def _private_method(self):
        # Internal use only
        pass
```

### Inheritance Template
```python
from .parent_module import ParentClass

class ChildClass(ParentClass):
    def __init__(self, param):
        # Call parent's __init__
        ParentClass.__init__(self)
        # Or use super()
        # super().__init__()
        
        # Add child-specific attributes
        self.child_attribute = param
```

### Key Concepts Summary

| Concept | Symbol/Syntax | Purpose |
|---------|---------------|---------|
| `self` | First parameter | Reference to instance |
| `__init__` | Method name | Constructor/initializer |
| `_method()` | Leading underscore | Non-public method |
| `super()` | Function | Call parent class method |
| `dir()` | Function | List object attributes |
| Inheritance | `class Child(Parent):` | Inherit from parent |

---

## Class vs Instance

| Type | Description | Example |
|------|-------------|---------|
| **Class** | Blueprint/template | `MyClass` |
| **Instance** | Specific object created from class | `my_instance = MyClass()` |
| **Class attribute** | Shared by all instances | Defined outside `__init__` |
| **Instance attribute** | Unique to each instance | Defined with `self` in `__init__` |

---

#python #oop #classes #inheritance #self #dry-principle #software-engineering #datacamp #chapter3

# Chapter 3 Practice Quiz
## Adding Classes to a Package

**Total Points: 100**
**Pass Threshold: 70/100 (70%)**

---

## Section A: Multiple Choice Questions (2 points each = 40 points)

### Class Basics

**Q1.** What is the primary method used to create a new instance of a class?

- [ ] A) `__new__()`
- [ ] B) `__init__()`
- [ ] C) `__create__()`
- [ ] D) `__instance__()`

> [!success]- Answer
> **Correct Answer: B) `__init__()`**
> 
> The `__init__()` method is the constructor/initializer that's called when creating a new instance of a class. It sets up the initial state of the object.
> ```python
> def __init__(self, value):
>     self.attribute = value
> ```

**Q2.** What does `self` represent in a class method?

- [ ] A) The class itself
- [ ] B) The parent class
- [ ] C) The instance of the class
- [ ] D) A global variable

> [!success]- Answer
> **Correct Answer: C) The instance of the class**
> 
> `self` is a reference to the specific instance being created or manipulated. It's automatically passed by Python when you call a method on an instance.

**Q3.** In the code `my_instance = MyClass(value='test')`, what happens to the `value` parameter?

- [ ] A) It's stored as a class variable
- [ ] B) It's passed to `__init__` as an argument (along with self automatically)
- [ ] C) It's ignored
- [ ] D) It's stored globally

> [!success]- Answer
> **Correct Answer: B) It's passed to `__init__` as an argument (along with self automatically)**
> 
> When creating an instance, Python automatically calls `__init__`, passing the instance as `self` and any other arguments you provide. So `MyClass(value='test')` calls `__init__(self, value='test')`.

**Q4.** How do you make a class available at the package level?

- [ ] A) Define it in `setup.py`
- [ ] B) Import it in `__init__.py` using `from .module import ClassName`
- [ ] C) Create a special `classes.py` file
- [ ] D) Use the `@export` decorator

> [!success]- Answer
> **Correct Answer: B) Import it in `__init__.py` using `from .module import ClassName`**
> 
> To make a class available at package level:
> ```python
> # In __init__.py
> from .my_class import MyClass
> ```
> Then users can do: `import my_package` and use `my_package.MyClass()`

### Non-Public Methods

**Q5.** What naming convention indicates a method is non-public (private)?

- [ ] A) Starting with double underscore `__method`
- [ ] B) Starting with single underscore `_method`
- [ ] C) All capitals `METHOD`
- [ ] D) Ending with underscore `method_`

> [!success]- Answer
> **Correct Answer: B) Starting with single underscore `_method`**
> 
> A single leading underscore (e.g., `_tokenize()`) is the Python convention for non-public methods. It signals "internal use only."

**Q6.** What are the risks of using non-public methods mentioned in the chapter?

- [ ] A) They run slower
- [ ] B) Lack of documentation and unpredictability
- [ ] C) They don't work properly
- [ ] D) They consume more memory

> [!success]- Answer
> **Correct Answer: B) Lack of documentation and unpredictability**
> 
> The chapter specifically mentions two risks:
> 1. **Lack of documentation** - Non-public methods often aren't documented in public API
> 2. **Unpredictability** - They may change or be removed in future versions without notice

**Q7.** In the Document class, what does `self.tokens = self._tokenize()` do?

- [ ] A) Calls the tokenize function from another module
- [ ] B) Calls the _tokenize method of the instance and stores result in tokens attribute
- [ ] C) Creates a new class
- [ ] D) Imports the tokenization module

> [!success]- Answer
> **Correct Answer: B) Calls the _tokenize method of the instance and stores result in tokens attribute**
> 
> This line calls the instance's own `_tokenize()` method and assigns the returned value to the `tokens` attribute:
> ```python
> def __init__(self, text):
>     self.text = text
>     self.tokens = self._tokenize()  # Calls method, stores result
> ```

### DRY Principle and Inheritance

**Q8.** What does the DRY principle stand for?

- [ ] A) Do Remember Yourself
- [ ] B) Don't Repeat Yourself
- [ ] C) Define Reusable Yields
- [ ] D) Debug Repeatedly Yearly

> [!success]- Answer
> **Correct Answer: B) Don't Repeat Yourself**
> 
> DRY (Don't Repeat Yourself) is a software engineering principle that encourages writing code once and reusing it, rather than duplicating similar code throughout your project.

**Q9.** How does inheritance help follow the DRY principle?

- [ ] A) By making code run faster
- [ ] B) By allowing child classes to reuse parent class code
- [ ] C) By automatically documenting code
- [ ] D) By reducing memory usage

> [!success]- Answer
> **Correct Answer: B) By allowing child classes to reuse parent class code**
> 
> Inheritance enables code reuse - child classes inherit attributes and methods from parent classes, so you don't have to rewrite the same functionality multiple times.

**Q10.** What is the syntax to create a class that inherits from a parent class?

- [ ] A) `class Child <- Parent:`
- [ ] B) `class Child(Parent):`
- [ ] C) `class Child extends Parent:`
- [ ] D) `class Child inherits Parent:`

> [!success]- Answer
> **Correct Answer: B) `class Child(Parent):`**
> 
> Python uses parentheses to specify inheritance:
> ```python
> class ChildClass(ParentClass):
>     def __init__(self):
>         ParentClass.__init__(self)
> ```

**Q11.** In a child class, how do you call the parent class's `__init__` method?

- [ ] A) `parent.__init__(self)`
- [ ] B) `ParentClass.__init__(self)`
- [ ] C) `super().__init__()`
- [ ] D) Both B and C are correct

> [!success]- Answer
> **Correct Answer: D) Both B and C are correct**
> 
> You can call the parent's `__init__` either way:
> ```python
> # Method 1: Direct parent call
> ParentClass.__init__(self)
> 
> # Method 2: Using super()
> super().__init__()
> ```
> Both are valid, though `super()` is generally preferred for multilevel inheritance.

**Q12.** What can a child class access from its parent class?

- [ ] A) Only public methods
- [ ] B) Only attributes defined in __init__
- [ ] C) All attributes and methods (public and non-public)
- [ ] D) Nothing unless explicitly specified

> [!success]- Answer
> **Correct Answer: C) All attributes and methods (public and non-public)**
> 
> A child class inherits everything from the parent - all methods (public and non-public) and all attributes. As shown in the example:
> ```python
> child_class = ChildClass()
> print(child_class.child_attribute)   # Child's own
> print(child_class.parent_attribute)  # Inherited from parent
> ```

### Multilevel Inheritance and super()

**Q13.** What is multilevel inheritance?

- [ ] A) A class inheriting from multiple parents simultaneously
- [ ] B) A chain of inheritance: Parent → Child → Grandchild
- [ ] C) Inheriting only some methods from a parent
- [ ] D) A class with multiple __init__ methods

> [!success]- Answer
> **Correct Answer: B) A chain of inheritance: Parent → Child → Grandchild**
> 
> Multilevel inheritance is a hierarchy where a class inherits from another class, which itself inherits from another class, creating a chain:
> ```
> Document (parent)
>     ↓
> SocialMedia (child)
>     ↓
> Tweet (grandchild)
> ```

**Q14.** What is the purpose of the `super()` function?

- [ ] A) To create a superior version of the class
- [ ] B) To call methods from the parent class
- [ ] C) To delete inherited attributes
- [ ] D) To make a class abstract

> [!success]- Answer
> **Correct Answer: B) To call methods from the parent class**
> 
> `super()` provides access to parent class methods, especially useful for calling the parent's `__init__`:
> ```python
> def __init__(self):
>     super().__init__()  # Calls parent's __init__
> ```

**Q15.** In the multilevel inheritance example with Parent → SuperChild → Grandchild, what is the output order when creating a Grandchild instance?

- [ ] A) "I'm a grandchild!" → "I'm a super child!" → "I'm a parent!"
- [ ] B) "I'm a parent!" → "I'm a super child!" → "I'm a grandchild!"
- [ ] C) Only "I'm a grandchild!" prints
- [ ] D) Random order

> [!success]- Answer
> **Correct Answer: B) "I'm a parent!" → "I'm a super child!" → "I'm a grandchild!"**
> 
> When using `super()`, the calls execute from the top of the hierarchy down:
> ```python
> grandchild = Grandchild()
> # Output:
> # I'm a parent!
> # I'm a super child!
> # I'm a grandchild!
> ```
> Each `super().__init__()` calls the next parent up the chain first.

**Q16.** What is the difference between `ParentClass.__init__(self)` and `super().__init__()`?

- [ ] A) No difference, they do exactly the same thing
- [ ] B) super() is cleaner and better for multilevel inheritance
- [ ] C) ParentClass.__init__ is deprecated
- [ ] D) super() only works with multiple inheritance

> [!success]- Answer
> **Correct Answer: B) super() is cleaner and better for multilevel inheritance**
> 
> While both work for simple inheritance, `super()` is preferred because:
> - Cleaner syntax (no need to name the parent class)
> - Better for multilevel inheritance (automatically handles the chain)
> - Follows proper Method Resolution Order (MRO)

### Using dir()

**Q17.** What does the `dir()` function do when called on an object?

- [ ] A) Deletes the object
- [ ] B) Returns a list of all attributes and methods of the object
- [ ] C) Changes the object's directory
- [ ] D) Creates documentation for the object

> [!success]- Answer
> **Correct Answer: B) Returns a list of all attributes and methods of the object**
> 
> `dir()` returns a list of all attributes and methods available on an object, including inherited ones:
> ```python
> sm = SocialMedia('text')
> dir(sm)  # Returns list of all methods and attributes
> ```

**Q18.** In the `dir()` output, what are "dunder methods" (e.g., `__init__`, `__str__`)?

- [ ] A) Methods that don't work
- [ ] B) Special methods with double underscores that Python uses internally
- [ ] C) Deprecated methods
- [ ] D) Methods from third-party packages

> [!success]- Answer
> **Correct Answer: B) Special methods with double underscores that Python uses internally**
> 
> "Dunder methods" (double underscore) are special methods Python uses for built-in functionality:
> - `__init__`: Constructor
> - `__str__`: String representation
> - `__eq__`: Equality comparison
> These are also called "magic methods."

**Q19.** When viewing `dir()` output for a SocialMedia instance, methods like `_tokenize` and `_count_words` indicate what?

- [ ] A) These are broken methods
- [ ] B) These are non-public (private) methods
- [ ] C) These are class variables
- [ ] D) These are from external libraries

> [!success]- Answer
> **Correct Answer: B) These are non-public (private) methods**
> 
> The leading underscore indicates these are non-public methods meant for internal use:
> ```python
> '_count_hashtags', '_count_mentions', '_count_words', '_tokenize'
> ```

**Q20.** What type of information does `dir()` show about an object?

- [ ] A) Only methods
- [ ] B) Only attributes
- [ ] C) Both methods and attributes, including inherited ones
- [ ] D) Only the source code

> [!success]- Answer
> **Correct Answer: C) Both methods and attributes, including inherited ones**
> 
> `dir()` shows everything:
> - Dunder methods (`__init__`, `__str__`)
> - Non-public methods (`_tokenize`)
> - Public methods (`hashtag_counts`)
> - Attributes (`text`, `tokens`)
> - Everything inherited from parent classes

---

## Section B: True/False Questions (1 point each = 15 points)

**Q21.** The `self` parameter is automatically passed by Python when you call a method. **T/F**

> [!success]- Answer
> **True** - When you call `my_instance.method()`, Python automatically passes `my_instance` as the `self` parameter to the method.

**Q22.** A class must have an `__init__` method to be valid. **T/F**

> [!success]- Answer
> **False** - While `__init__` is very common, a class can be defined without it. Python will provide a default constructor if none is specified.

**Q23.** Methods starting with a single underscore (_) are completely private and cannot be accessed from outside the class. **T/F**

> [!success]- Answer
> **False** - The single underscore is a *convention* indicating non-public methods, but they can still be accessed. It's not enforced by Python, just a signal to other developers that these methods are internal.

**Q24.** The DRY principle encourages code reuse and reduces duplication. **T/F**

> [!success]- Answer
> **True** - DRY (Don't Repeat Yourself) specifically promotes writing code once and reusing it rather than duplicating similar code.

**Q25.** A child class inherits all attributes and methods from its parent class. **T/F**

> [!success]- Answer
> **True** - Inheritance means the child class gets all attributes and methods from the parent, which it can then use, override, or extend.

**Q26.** In a child class, you must always call the parent's `__init__` method. **T/F**

> [!success]- Answer
> **False** - While it's often necessary and good practice, it's not automatically required. It depends on whether you need the parent's initialization logic. However, if the parent has important setup code, you should call it.

**Q27.** `super()` can only be used to call the parent's `__init__` method. **T/F**

> [!success]- Answer
> **False** - `super()` can be used to call any parent class method, not just `__init__`. Example: `super().some_method()`

**Q28.** Multilevel inheritance creates a chain of parent-child relationships. **T/F**

> [!success]- Answer
> **True** - Multilevel inheritance is exactly this: a hierarchy where classes inherit from each other in a chain (e.g., Parent → Child → Grandchild).

**Q29.** Multiple inheritance means a class inherits from more than one parent class simultaneously. **T/F**

> [!success]- Answer
> **True** - Multiple inheritance is when a class has multiple parents: `class Child(Parent1, Parent2):`. This is different from multilevel inheritance.

**Q30.** The `dir()` function only shows methods you explicitly defined in your class. **T/F**

> [!success]- Answer
> **False** - `dir()` shows everything: explicitly defined methods, inherited methods, dunder methods, and attributes.

**Q31.** In the Document class, `self.tokens = self._tokenize()` calls the `_tokenize()` method immediately during initialization. **T/F**

> [!success]- Answer
> **True** - This line in `__init__` calls `_tokenize()` right when the instance is created, storing the result in the `tokens` attribute.

**Q32.** Non-public methods (starting with _) are guaranteed to remain unchanged in future versions of a package. **T/F**

> [!success]- Answer
> **False** - This is actually the opposite. Non-public methods are unpredictable and may change or be removed in future versions without warning, which is one of their main risks.

**Q33.** When using `super().__init__()` in multilevel inheritance, parent initializers are called from top to bottom in the hierarchy. **T/F**

> [!success]- Answer
> **True** - Each `super().__init__()` call propagates up the chain, so the top-most parent initializes first, then each child down the chain, as shown in the "I'm a parent!" → "I'm a super child!" → "I'm a grandchild!" example.

**Q34.** Attributes defined with `self.attribute = value` are instance attributes. **T/F**

> [!success]- Answer
> **True** - Attributes assigned to `self` in methods (especially `__init__`) are instance attributes, unique to each instance of the class.

**Q35.** The docstring format `:param` and `:ivar` is used to document class parameters and instance variables. **T/F**

> [!success]- Answer
> **True** - This is the Sphinx documentation format used in the chapter:
> - `:param value:` documents a parameter
> - `:ivar attribute:` documents an instance variable

---

## Section C: Short Answer Questions (3 points each = 15 points)

**Q36.** Explain the purpose of `self` in a class and how it's used. Provide an example.

> [!success]- Answer
> **Purpose of `self`:**
> `self` is a reference to the instance of the class. It allows methods to access and modify the instance's attributes and call other instance methods.
> 
> **How it's used:**
> - First parameter in all instance methods
> - Automatically passed by Python (you don't pass it manually)
> - Used with dot notation to access attributes: `self.attribute`
> 
> **Example:**
> ```python
> class MyClass:
>     def __init__(self, value):
>         self.attribute = value  # self stores the value in the instance
>     
>     def get_value(self):
>         return self.attribute  # self accesses the instance's attribute
> 
> # When you call:
> instance = MyClass(10)
> # Python automatically does: MyClass.__init__(instance, 10)
> # so 'self' refers to 'instance'
> ```

**Q37.** Describe the two risks of using non-public methods mentioned in the chapter.

> [!success]- Answer
> The two risks of using non-public methods are:
> 
> **1. Lack of documentation**
> - Non-public methods are often not included in public API documentation
> - Makes them harder to understand and use correctly
> - You may not know what parameters they expect or what they return
> 
> **2. Unpredictability**
> - These methods may change in future versions without warning
> - They might be renamed, removed, or have their behavior modified
> - No guarantee of backward compatibility
> - Your code might break when you update the package
> 
> These risks exist because non-public methods are intended for internal implementation details, not for external use.

**Q38.** Explain how inheritance helps follow the DRY principle. Use the class hierarchy concept in your answer.

> [!success]- Answer
> **How inheritance supports DRY:**
> 
> Inheritance follows the DRY (Don't Repeat Yourself) principle by allowing code reuse through a hierarchical structure:
> 
> **1. Write once, use many times:**
> - Common functionality is defined in a parent class once
> - All child classes automatically inherit this functionality
> - No need to duplicate the same code across multiple classes
> 
> **2. Example hierarchy:**
> ```
> Document (parent)
>   - text attribute
>   - _tokenize() method
>       ↓
> SocialMedia (child)
>   - Inherits text and _tokenize()
>   - Adds hashtag_counts()
>       ↓
> Tweet (grandchild)
>   - Inherits everything from above
>   - Adds tweet-specific features
> ```
> 
> **3. Benefits:**
> - Single source of truth for shared behavior
> - Changes to parent automatically propagate to children
> - Reduces code duplication and maintenance burden
> - Makes the codebase more maintainable and consistent

**Q39.** Compare and contrast `ParentClass.__init__(self)` and `super().__init__()` for calling parent initializers.

> [!success]- Answer
> **`ParentClass.__init__(self)` - Direct parent call:**
> - Explicitly names the parent class
> - Must pass `self` manually
> - Works fine for simple, single inheritance
> - Less flexible if parent class name changes
> 
> ```python
> class Child(Parent):
>     def __init__(self):
>         ParentClass.__init__(self)
> ```
> 
> **`super().__init__()` - Using super():**
> - Doesn't name the parent class explicitly
> - Automatically handles `self`
> - Better for multilevel inheritance
> - Follows proper Method Resolution Order (MRO)
> - Cleaner, more Pythonic syntax
> - Preferred modern approach
> 
> ```python
> class Child(Parent):
>     def __init__(self):
>         super().__init__()
> ```
> 
> **Key difference:**
> In multilevel inheritance, `super()` correctly chains through all parent classes, while direct parent calls might skip levels in complex hierarchies.
> 
> **Recommendation:** Use `super()` as it's more maintainable and handles complex inheritance better.

**Q40.** What information does `dir()` provide, and why is it useful for working with inherited classes?

> [!success]- Answer
> **What `dir()` provides:**
> `dir()` returns a list of all attributes and methods available on an object, including:
> - Dunder (magic) methods: `__init__`, `__str__`, `__eq__`
> - Non-public methods: `_tokenize`, `_count_words`
> - Public methods: `hashtag_counts`, `mention_counts`
> - Attributes: `text`, `tokens`, `word_counts`
> - Everything inherited from parent classes
> 
> **Why it's useful for inherited classes:**
> 
> 1. **Discovery:** Quickly see what's available on an object without reading documentation
> 2. **Inherited features:** Shows all methods and attributes from parent classes
> 3. **Debugging:** Verify that inheritance worked correctly
> 4. **Exploration:** Understand what a class can do, especially with complex inheritance
> 5. **Documentation aid:** Helps when lack of documentation makes understanding difficult
> 
> **Example use:**
> ```python
> sm = SocialMedia('@DataCamp #Python')
> dir(sm)  # Shows all available methods/attributes
> # Helps answer: "What can I do with this object?"
> ```
> 
> This is particularly valuable when working with classes that have multilevel inheritance, as the chapter's comment suggests: "What methods does sm have? ¯\\_(ツ)_/¯"

---

## Section D: Code Analysis & Scenarios (3 points each = 30 points)

**Q41.** Create a basic class called `Book` with `__init__` that takes `title` and `author` parameters and stores them as attributes.

> [!success]- Answer
> **Solution:**
> ```python
> class Book:
>     """A class representing a book
>     
>     :param title: the title of the book
>     :param author: the author of the book
>     :ivar title: stores the book's title
>     :ivar author: stores the book's author
>     """
>     
>     def __init__(self, title, author):
>         self.title = title
>         self.author = author
> ```
> 
> **Usage:**
> ```python
> my_book = Book("Python Basics", "DataCamp")
> print(my_book.title)   # "Python Basics"
> print(my_book.author)  # "DataCamp"
> ```

**Q42.** Given the Document class that has a `_tokenize()` method, add this class to a package's `__init__.py` so it can be imported as `my_package.Document`.

> [!success]- Answer
> **In `work_dir/my_package/__init__.py`:**
> ```python
> from .document import Document
> ```
> 
> **Assuming the Document class is in:**
> `work_dir/my_package/document.py`
> 
> **Then users can:**
> ```python
> import my_package
> 
> doc = my_package.Document('test doc')
> print(doc.tokens)
> ```

**Q43.** Write a `Calculator` parent class with an `__init__` that sets `self.result = 0`. Then create a `ScientificCalculator` child class that inherits from it and adds a `self.mode = 'scientific'` attribute.

> [!success]- Answer
> **Solution:**
> ```python
> # Parent class
> class Calculator:
>     """Basic calculator class"""
>     
>     def __init__(self):
>         self.result = 0
> 
> # Child class
> class ScientificCalculator(Calculator):
>     """Scientific calculator that extends Calculator"""
>     
>     def __init__(self):
>         # Call parent's __init__
>         Calculator.__init__(self)
>         # Or: super().__init__()
>         
>         # Add child-specific attribute
>         self.mode = 'scientific'
> ```
> 
> **Usage:**
> ```python
> calc = ScientificCalculator()
> print(calc.result)  # 0 (inherited)
> print(calc.mode)    # 'scientific' (child's own)
> ```

**Q44.** Rewrite the ScientificCalculator from Q43 to use `super().__init__()` instead of directly calling the parent class.

> [!success]- Answer
> **Solution:**
> ```python
> class Calculator:
>     """Basic calculator class"""
>     
>     def __init__(self):
>         self.result = 0
> 
> class ScientificCalculator(Calculator):
>     """Scientific calculator that extends Calculator"""
>     
>     def __init__(self):
>         # Use super() instead of Calculator.__init__(self)
>         super().__init__()
>         
>         # Add child-specific attribute
>         self.mode = 'scientific'
> ```
> 
> **Why this is better:**
> - Cleaner syntax
> - More maintainable (no need to change if parent class name changes)
> - Better for multilevel inheritance

**Q45.** Given this class hierarchy, what will be printed when you create a `Grandchild` instance?

```python
class Parent:
    def __init__(self):
        print("Parent initialized")

class Child(Parent):
    def __init__(self):
        super().__init__()
        print("Child initialized")

class Grandchild(Child):
    def __init__(self):
        super().__init__()
        print("Grandchild initialized")

obj = Grandchild()
```

> [!success]- Answer
> **Output:**
> ```
> Parent initialized
> Child initialized
> Grandchild initialized
> ```
> 
> **Explanation:**
> 1. `Grandchild.__init__()` is called first
> 2. It calls `super().__init__()`, which calls `Child.__init__()`
> 3. `Child.__init__()` calls `super().__init__()`, which calls `Parent.__init__()`
> 4. `Parent.__init__()` prints "Parent initialized"
> 5. Control returns to `Child.__init__()`, which prints "Child initialized"
> 6. Control returns to `Grandchild.__init__()`, which prints "Grandchild initialized"
> 
> The `super()` chain ensures parent classes initialize from top to bottom.

**Q46.** Identify what's wrong with this class definition and fix it:

```python
class MyClass:
    def __init__(value):
        attribute = value
```

> [!success]- Answer
> **Problems identified:**
> 1. Missing `self` parameter in `__init__`
> 2. `attribute` is not assigned to instance (missing `self.`)
> 
> **Corrected code:**
> ```python
> class MyClass:
>     def __init__(self, value):
>         self.attribute = value
> ```
> 
> **Explanation:**
> - `self` must be the first parameter in `__init__` (and all instance methods)
> - Instance attributes must be assigned using `self.attribute_name`
> - Without `self.`, `attribute` would just be a local variable that disappears after `__init__` finishes

**Q47.** Write code to create a Document instance with the text "hello world" and then use `dir()` to explore its attributes. What would you look for in the output?

> [!success]- Answer
> **Code:**
> ```python
> doc = Document('hello world')
> 
> # Explore the instance
> attributes = dir(doc)
> print(attributes)
> ```
> 
> **What to look for in the output:**
> 
> 1. **Instance attributes:**
>    - `text` - should contain 'hello world'
>    - `tokens` - should contain ['hello', 'world']
> 
> 2. **Methods:**
>    - `_tokenize` - non-public method for tokenization
>    - Any other custom methods
> 
> 3. **Dunder methods:**
>    - `__init__`, `__str__`, `__dict__`, etc.
> 
> 4. **Inherited members:**
>    - If Document inherits from another class, those members too
> 
> **Filter to see just custom attributes:**
> ```python
> # Filter out dunder methods
> custom = [attr for attr in dir(doc) if not attr.startswith('__')]
> print(custom)
> # Might show: ['_tokenize', 'text', 'tokens']
> ```

**Q48.** Create a multilevel inheritance hierarchy: `Vehicle` → `Car` → `ElectricCar`. Each level should add one attribute and use `super().__init__()`.

> [!success]- Answer
> **Solution:**
> ```python
> class Vehicle:
>     """Base vehicle class"""
>     
>     def __init__(self):
>         self.wheels = 4
>         print("Vehicle initialized")
> 
> class Car(Vehicle):
>     """Car class inheriting from Vehicle"""
>     
>     def __init__(self):
>         super().__init__()
>         self.fuel_type = 'gasoline'
>         print("Car initialized")
> 
> class ElectricCar(Car):
>     """Electric car inheriting from Car"""
>     
>     def __init__(self):
>         super().__init__()
>         self.battery_capacity = 75  # kWh
>         print("ElectricCar initialized")
> ```
> 
> **Usage:**
> ```python
> tesla = ElectricCar()
> # Output:
> # Vehicle initialized
> # Car initialized
> # ElectricCar initialized