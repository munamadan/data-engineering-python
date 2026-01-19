## What are Templates?

**Templates** in Airflow provide dynamic content substitution during DAG execution.

### Key Characteristics:

- **Allow substituting information** during a DAG run
- **Provide added flexibility** when defining tasks
- **Created using Jinja templating language**

### Purpose:

- Avoid code duplication
- Make tasks reusable
- Add dynamic behavior
- Access runtime information

---

## Non-Templated vs Templated Example

### Without Templates (Repetitive)

```python
# Need separate task for each file
t1 = BashOperator(
    task_id='first_task',
    bash_command='echo "Reading file1.txt"',
    dag=dag
)

t2 = BashOperator(
    task_id='second_task',
    bash_command='echo "Reading file2.txt"',
    dag=dag
)
```

**Problems:**
- Code duplication
- Hard to maintain
- Not flexible

---

### With Templates (Reusable)

```python
templated_command = """
echo "Reading {{ params.filename }}"
"""

t1 = BashOperator(
    task_id='template_task',
    bash_command=templated_command,
    params={'filename': 'file1.txt'},
    dag=example_dag
)

t2 = BashOperator(
    task_id='template_task',
    bash_command=templated_command,
    params={'filename': 'file2.txt'},
    dag=example_dag
)
```

**Output:**
```
Reading file1.txt
Reading file2.txt
```

**Benefits:**
- Same command template
- Different parameters
- Easy to maintain
- Scalable

---

## Template Syntax

### Basic Jinja Syntax

**Variable substitution:**
```python
{{ variable_name }}
```

**Accessing parameters:**
```python
{{ params.filename }}
{{ params.date }}
```

**Dot notation for nested access:**
```python
{{ object.attribute }}
{{ dag.dag_id }}
```

---

## Advanced Templates with Loops

### For Loop Example

```python
templated_command = """
{% for filename in params.filenames %}
echo "Reading {{ filename }}"
{% endfor %}
"""

t1 = BashOperator(
    task_id='template_task',
    bash_command=templated_command,
    params={'filenames': ['file1.txt', 'file2.txt']},
    dag=example_dag
)
```

**Output:**
```
Reading file1.txt
Reading file2.txt
```

### Jinja Control Structures

**For loops:**
```jinja
{% for item in list %}
    {{ item }}
{% endfor %}
```

**If statements:**
```jinja
{% if condition %}
    do something
{% else %}
    do something else
{% endif %}
```

---

## Built-in Variables

Airflow provides **built-in runtime variables** for accessing DAG run information.

### Common Variables

| Variable | Format | Description | Example |
|----------|--------|-------------|---------|
| `{{ ds }}` | YYYY-MM-DD | Execution date | `2024-01-15` |
| `{{ ds_nodash }}` | YYYYMMDD | Execution date, no dashes | `20240115` |
| `{{ prev_ds }}` | YYYY-MM-DD | Previous execution date | `2024-01-14` |
| `{{ prev_ds_nodash }}` | YYYYMMDD | Previous execution, no dashes | `20240114` |
| `{{ dag }}` | Object | DAG object | Access DAG properties |
| `{{ conf }}` | Object | Airflow config object | System configuration |

### Usage Examples

```python
templated_command = """
echo "Processing data for {{ ds }}"
echo "Previous run was {{ prev_ds }}"
echo "DAG ID: {{ dag.dag_id }}"
"""
```

**Output (for 2024-01-15):**
```
Processing data for 2024-01-15
Previous run was 2024-01-14
DAG ID: my_etl_dag
```

### Reference Documentation

Full list of variables: https://airflow.apache.org/docs/stable/macros-ref.html

---

## Macros

**Macros** provide access to useful Python objects and methods in templates.

### The `{{ macros }}` Variable

**Purpose:** Reference to Airflow macros package

### Common Macros

| Macro | Purpose | Example |
|-------|---------|---------|
| `{{ macros.datetime }}` | datetime.datetime object | Date/time operations |
| `{{ macros.timedelta }}` | timedelta object | Time duration |
| `{{ macros.uuid }}` | Python's uuid object | Generate unique IDs |
| `{{ macros.ds_add() }}` | Modify days from date | Add/subtract days |

### `ds_add()` Example

```python
# Add 5 days to a date
{{ macros.ds_add('2020-04-15', 5) }}
# Returns: 2020-04-20

# Subtract 3 days
{{ macros.ds_add('2020-04-15', -3) }}
# Returns: 2020-04-12
```

### Usage in Templates

```python
templated_command = """
echo "Today: {{ ds }}"
echo "Next week: {{ macros.ds_add(ds, 7) }}"
echo "Last week: {{ macros.ds_add(ds, -7) }}"
"""
```

---

## Branching

**Branching** provides conditional logic in workflows.

### BranchPythonOperator

**Purpose:** Execute different tasks based on conditions

**Characteristics:**
- Uses `BranchPythonOperator`
- Takes a `python_callable`
- Callable returns next task ID (or list of IDs)
- Enables conditional workflow paths

### Import

```python
from airflow.operators.python import BranchPythonOperator
```

---

## Branching Example

### Branch Function

```python
def branch_test(**kwargs):
    if int(kwargs['ds_nodash']) % 2 == 0:
        return 'even_day_task'
    else:
        return 'odd_day_task'
```

**Logic:**
- Check if execution date is even/odd
- Return appropriate task ID

### Creating Branch Task

```python
branch_task = BranchPythonOperator(
    task_id='branch_task',
    dag=dag,
    provide_context=True,      # Required for accessing kwargs
    python_callable=branch_test
)

# Define workflow with branching
start_task >> branch_task >> even_day_task >> even_day_task2
branch_task >> odd_day_task >> odd_day_task2
```

### Workflow Structure

```
start_task
    ↓
branch_task
    ↓ (if even)    ↓ (if odd)
even_day_task    odd_day_task
    ↓                ↓
even_day_task2   odd_day_task2
```

### Important Notes

- **`provide_context=True`** - Required for accessing kwargs
- **`**kwargs`** - Function must accept keyword arguments
- **Returns task_id** - Must return string of next task ID
- **One path executes** - Only one branch runs per execution

---

## Running DAGs and Tasks

### Running Specific Tasks

**Test a single task:**
```bash
airflow tasks test <dag_id> <task_id> <date>
```

**Example:**
```bash
airflow tasks test my_etl_dag extract_data 2024-01-15
```

**Purpose:**
- Test individual tasks
- Debugging
- Development verification

### Running Full DAGs

**Trigger entire DAG:**
```bash
airflow dags trigger -e <date> <dag_id>
```

**Example:**
```bash
airflow dags trigger -e 2024-01-15 my_etl_dag
```

**Purpose:**
- Manual DAG execution
- Backfilling
- Testing complete workflow

---

## Operator Reminders

Quick reference for common operators:

### BashOperator

```python
from airflow.operators.bash import BashOperator

task = BashOperator(
    task_id='bash_task',
    bash_command='echo "Hello"'  # Required
)
```

**Expects:** `bash_command`

### PythonOperator

```python
from airflow.operators.python import PythonOperator

task = PythonOperator(
    task_id='python_task',
    python_callable=my_function  # Required
)
```

**Expects:** `python_callable`

### BranchPythonOperator

```python
from airflow.operators.python import BranchPythonOperator

task = BranchPythonOperator(
    task_id='branch',
    python_callable=branch_function,  # Required
    provide_context=True              # Required
)
```

**Requirements:**
- `python_callable` - Function to call
- `provide_context=True` - Access kwargs
- Function must accept `**kwargs`

### FileSensor

```python
from airflow.sensors.filesystem import FileSensor

sensor = FileSensor(
    task_id='wait_for_file',
    filepath='data.csv',      # Required
    poke_interval=60,         # Optional
    mode='poke'               # Optional
)
```

**Requires:**
- `filepath` argument
- Optional: `mode`, `poke_interval`

---

## Template Fields

### What are Template Fields?

**Definition:** Specific operator arguments that support templating

**Not all fields support templates!**

### How to Check Template Fields

#### Method 1: Python Help

```python
# 1. Open Python interpreter
python3

# 2. Import operator
from airflow.operators.bash import BashOperator

# 3. Get help
help(BashOperator)

# 4. Look for template_fields
# Output shows: template_fields = ('bash_command', 'env')
```

#### Method 2: Documentation

Check official Airflow documentation for each operator.

### Example: BashOperator Template Fields

```python
template_fields = ('bash_command', 'env')
```

**Meaning:**
- ✅ `bash_command` - Can use templates
- ✅ `env` - Can use templates
- ❌ `task_id` - Cannot use templates
- ❌ `dag` - Cannot use templates

### Using Template Fields

```python
# bash_command supports templates
task = BashOperator(
    task_id='templated_task',
    bash_command='echo "Date: {{ ds }}"'  # ✅ Works
)

# task_id does NOT support templates
task = BashOperator(
    task_id='task_{{ ds }}',  # ❌ Won't work
    bash_command='echo "Hello"'
)
```

---

## Complete Example: Production Pipeline

```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import BranchPythonOperator
from airflow.sensors.filesystem import FileSensor
from datetime import datetime, timedelta

# Default args
default_args = {
    'owner': 'data_team',
    'start_date': datetime(2024, 1, 1),
    'retries': 2,
    'retry_delay': timedelta(minutes=5)
}

# Branch function
def choose_branch(**kwargs):
    """Branch based on day of week"""
    execution_date = kwargs['ds']
    day = datetime.strptime(execution_date, '%Y-%m-%d').weekday()
    
    if day < 5:  # Monday-Friday
        return 'weekday_processing'
    else:  # Saturday-Sunday
        return 'weekend_processing'

with DAG(
    'production_etl',
    default_args=default_args,
    schedule_interval='@daily',
    catchup=False
) as dag:
    
    # Wait for file
    wait_for_data = FileSensor(
        task_id='wait_for_data',
        filepath='/data/input/data_{{ ds_nodash }}.csv',
        poke_interval=300,
        timeout=3600
    )
    
    # Extract with templates
    extract = BashOperator(
        task_id='extract',
        bash_command='''
        echo "Extracting data for {{ ds }}"
        cp /data/input/data_{{ ds_nodash }}.csv /tmp/staging/
        '''
    )
    
    # Branch based on day
    branch = BranchPythonOperator(
        task_id='branch',
        python_callable=choose_branch,
        provide_context=True
    )
    
    # Weekday processing
    weekday_task = BashOperator(
        task_id='weekday_processing',
        bash_command='echo "Running weekday process for {{ ds }}"'
    )
    
    # Weekend processing
    weekend_task = BashOperator(
        task_id='weekend_processing',
        bash_command='echo "Running weekend process for {{ ds }}"'
    )
    
    # Dependencies
    wait_for_data >> extract >> branch
    branch >> weekday_task
    branch >> weekend_task
```

---

## Course Summary

### What We've Learned

#### Core Concepts:
- ✅ **Workflows / DAGs** - Directed Acyclic Graphs
- ✅ **Operators** - BashOperator, PythonOperator, EmailOperator
- ✅ **Tasks** - Instances of operators
- ✅ **Dependencies** - Using bitshift operators (`>>`, `<<`)

#### Advanced Features:
- ✅ **Sensors** - Wait for conditions (FileSensor, etc.)
- ✅ **Scheduling** - Cron syntax, presets, intervals
- ✅ **SLAs / Alerting** - Service level agreements, notifications
- ✅ **Templates** - Jinja templating, variables, macros
- ✅ **Branching** - Conditional workflow logic

#### Operations:
- ✅ **Airflow Command Line** - Testing, triggering, debugging
- ✅ **Airflow UI** - Monitoring, visualization
- ✅ **Executors** - Sequential, Local, Kubernetes
- ✅ **Debugging / Troubleshooting** - Common issues, solutions

---

## Next Steps

### Practice and Exploration

1. **Setup Your Own Environment**
   - Install Airflow locally
   - Practice creating DAGs
   - Experiment with different configurations

2. **Explore More Operators/Sensors**
   - HttpOperator
   - S3Sensor
   - EmailOperator variations
   - Custom operators

3. **Experiment with Dependencies**
   - Complex dependency patterns
   - Fan-in / fan-out structures
   - Parallel execution

4. **Advanced Topics Not Covered**
   - **XCom** - Task communication
   - **Connections** - External system credentials
   - **Pools** - Resource management
   - **Variables** - Dynamic configuration

### Resources

**Official Documentation:**
- https://airflow.apache.org/docs/

**Community:**
- Airflow Community Slack channel
- Stack Overflow
- GitHub issues

**Learning:**
- Countless blogs and newsletters
- Future DataCamp courses
- YouTube tutorials
- Conference talks

### Keep Building!

- Start with simple workflows
- Gradually add complexity
- Monitor and optimize
- Share knowledge with community

---

## Quick Reference

### Template Syntax

```python
# Variable substitution
{{ variable_name }}

# Parameters
{{ params.filename }}

# Built-in variables
{{ ds }}           # 2024-01-15
{{ ds_nodash }}    # 20240115
{{ prev_ds }}      # Previous date

# Macros
{{ macros.ds_add(ds, 5) }}  # Add 5 days

# Loops
{% for item in list %}
    {{ item }}
{% endfor %}
```

### Branching

```python
def branch_func(**kwargs):
    if condition:
        return 'task_a'
    else:
        return 'task_b'

branch = BranchPythonOperator(
    task_id='branch',
    python_callable=branch_func,
    provide_context=True
)
```

### Running Commands

```bash
# Test task
airflow tasks test <dag_id> <task_id> <date>

# Trigger DAG
airflow dags trigger -e <date> <dag_id>
```

### Check Template Fields

```python
from airflow.operators.bash import BashOperator
help(BashOperator)
# Look for: template_fields = (...)
```

---

## Key Concepts Summary

| Concept | Definition |
|---------|------------|
| **Templates** | Dynamic content substitution using Jinja |
| **`{{ params }}`** | Access task parameters |
| **`{{ ds }}`** | Execution date (YYYY-MM-DD) |
| **`{{ ds_nodash }}`** | Execution date (YYYYMMDD) |
| **Macros** | Useful objects/methods for templates |
| **`ds_add()`** | Add/subtract days from date |
| **Branching** | Conditional workflow logic |
| **BranchPythonOperator** | Operator for branching |
| **`provide_context`** | Give access to kwargs |
| **Template fields** | Operator arguments supporting templates |
| **XCom** | Task communication (advanced) |
| **Connections** | External system credentials (advanced) |

---

#apache-airflow #templates #jinja #branching #macros #production-pipelines #branchpythonoperator #variables #ds #params #workflow-automation #datacamp #chapter4


# Chapter 4: Templates and Production Pipelines - Practice Quiz

**Total Points: 60 points**
**Passing Score: 42 points (70%)**

---

## Section A: Multiple Choice Questions (2 points each = 26 points)

### Templates Basics

**Q1.** What are templates in Airflow?

- [ ] A) Pre-built DAG examples
- [ ] B) A way to substitute information during a DAG run
- [ ] C) Database connection configurations
- [ ] D) Scheduler settings

> [!success]- Answer
> **Correct Answer: B) A way to substitute information during a DAG run**
> 
> Templates allow substituting information during DAG execution, providing flexibility when defining tasks. They use the Jinja templating language.

**Q2.** What templating language does Airflow use?

- [ ] A) Mustache
- [ ] B) Handlebars
- [ ] C) Jinja
- [ ] D) Django Templates

> [!success]- Answer
> **Correct Answer: C) Jinja**
> 
> Airflow templates are created using the Jinja templating language.

**Q3.** What is the main benefit of using templates?

- [ ] A) Faster execution
- [ ] B) Better security
- [ ] C) Code reusability and flexibility
- [ ] D) Automatic error handling

> [!success]- Answer
> **Correct Answer: C) Code reusability and flexibility**
> 
> Templates allow you to reuse the same command with different parameters, avoiding code duplication and providing flexibility.

**Q4.** How do you access a parameter in a template?

- [ ] A) `$params.filename`
- [ ] B) `{{ params.filename }}`
- [ ] C) `%params.filename%`
- [ ] D) `@params.filename`

> [!success]- Answer
> **Correct Answer: B) `{{ params.filename }}`**
> 
> Jinja template syntax uses double curly braces: `{{ variable }}` to access variables and parameters.

### Built-in Variables

**Q5.** What does `{{ ds }}` represent?

- [ ] A) Data source
- [ ] B) Execution date in YYYY-MM-DD format
- [ ] C) DAG schedule
- [ ] D) Database string

> [!success]- Answer
> **Correct Answer: B) Execution date in YYYY-MM-DD format**
> 
> `{{ ds }}` is a built-in variable that provides the execution date in YYYY-MM-DD format (e.g., 2024-01-15).

**Q6.** What format does `{{ ds_nodash }}` use?

- [ ] A) YYYY-MM-DD
- [ ] B) YYYYMMDD
- [ ] C) MM-DD-YYYY
- [ ] D) DD/MM/YYYY

> [!success]- Answer
> **Correct Answer: B) YYYYMMDD**
> 
> `{{ ds_nodash }}` provides the execution date without dashes in YYYYMMDD format (e.g., 20240115).

**Q7.** What does `{{ prev_ds }}` provide?

- [ ] A) Next execution date
- [ ] B) Previous execution date
- [ ] C) Start date
- [ ] D) Current date

> [!success]- Answer
> **Correct Answer: B) Previous execution date**
> 
> `{{ prev_ds }}` provides the previous execution date in YYYY-MM-DD format.

### Macros

**Q8.** What does the `{{ macros }}` variable provide?

- [ ] A) Database macros
- [ ] B) Reference to Airflow macros package with useful objects/methods
- [ ] C) SQL macros
- [ ] D) System macros

> [!success]- Answer
> **Correct Answer: B) Reference to Airflow macros package with useful objects/methods**
> 
> `{{ macros }}` provides access to useful Python objects and methods for templates, like datetime, timedelta, and uuid.

**Q9.** What does `{{ macros.ds_add('2020-04-15', 5) }}` return?

- [ ] A) 2020-04-10
- [ ] B) 2020-04-20
- [ ] C) 2020-05-15
- [ ] D) 2020-04-15

> [!success]- Answer
> **Correct Answer: B) 2020-04-20**
> 
> `ds_add()` adds (or subtracts if negative) days from a date. Adding 5 days to April 15 gives April 20.

### Branching

**Q10.** What operator is used for branching in Airflow?

- [ ] A) BranchOperator
- [ ] B) BranchPythonOperator
- [ ] C) ConditionalOperator
- [ ] D) IfOperator

> [!success]- Answer
> **Correct Answer: B) BranchPythonOperator**
> 
> BranchPythonOperator provides conditional logic and branching capabilities in workflows.

**Q11.** What must a BranchPythonOperator's python_callable return?

- [ ] A) Boolean (True/False)
- [ ] B) The next task ID (or list of IDs) to follow
- [ ] C) A dictionary
- [ ] D) Nothing (None)

> [!success]- Answer
> **Correct Answer: B) The next task ID (or list of IDs) to follow**
> 
> The branching function must return a string containing the task_id of the next task to execute.

**Q12.** What parameter is required for BranchPythonOperator to access kwargs?

- [ ] A) `enable_context=True`
- [ ] B) `provide_context=True`
- [ ] C) `use_context=True`
- [ ] D) `context=True`

> [!success]- Answer
> **Correct Answer: B) `provide_context=True`**
> 
> `provide_context=True` is required to give the callable function access to kwargs like `ds`, `ds_nodash`, etc.

**Q13.** What must the branching function accept as parameters?

- [ ] A) No parameters
- [ ] B) Only task_id
- [ ] C) `**kwargs`
- [ ] D) Only dag parameter

> [!success]- Answer
> **Correct Answer: C) `**kwargs`**
> 
> The branching function must accept `**kwargs` to receive context variables from Airflow.

---

## Section B: True/False Questions (1 point each = 12 points)

**Q14.** Templates allow substituting information during a DAG run. **T/F**

> [!success]- Answer
> **True** - This is the primary purpose of templates in Airflow.

**Q15.** Templates are created using the Django templating language. **T/F**

> [!success]- Answer
> **False** - Templates use the Jinja templating language, not Django.

**Q16.** `{{ ds }}` provides the execution date in YYYYMMDD format. **T/F**

> [!success]- Answer
> **False** - `{{ ds }}` uses YYYY-MM-DD format. `{{ ds_nodash }}` uses YYYYMMDD format.

**Q17.** `{{ prev_ds_nodash }}` provides the previous execution date without dashes. **T/F**

> [!success]- Answer
> **True** - This variable provides the previous execution date in YYYYMMDD format.

**Q18.** The `{{ macros.timedelta }}` object is available in templates. **T/F**

> [!success]- Answer
> **True** - macros.timedelta provides access to Python's timedelta object in templates.

**Q19.** `ds_add()` can only add days, not subtract them. **T/F**

> [!success]- Answer
> **False** - `ds_add()` can both add (positive numbers) and subtract (negative numbers) days.

**Q20.** BranchPythonOperator enables conditional logic in workflows. **T/F**

> [!success]- Answer
> **True** - BranchPythonOperator provides conditional branching based on logic in the callable function.

**Q21.** A branching function can return multiple task IDs as a list. **T/F**

> [!success]- Answer
> **True** - The function can return a single task_id string or a list of task_ids to execute.

**Q22.** All operator fields support templating. **T/F**

> [!success]- Answer
> **False** - Only specific fields (template_fields) support templating. Not all operator arguments can use templates.

**Q23.** You can check template_fields using `help(OperatorName)` in Python. **T/F**

> [!success]- Answer
> **True** - Running `help(BashOperator)` in Python shows which fields support templates.

**Q24.** The command `airflow tasks test` runs a specific task without affecting the scheduler. **T/F**

> [!success]- Answer
> **True** - The test command runs a task in isolation for testing/debugging purposes.

**Q25.** XCom is a feature for task communication covered in this chapter. **T/F**

> [!success]- Answer
> **False** - XCom was mentioned as an advanced topic NOT covered in this introductory course.

---

## Section C: Short Answer Questions (4 points each = 12 points)

**Q26.** Explain the difference between `{{ ds }}` and `{{ ds_nodash }}`. Provide examples of when you would use each.

> [!success]- Answer
> **Difference:**
> 
> **`{{ ds }}`:**
> - Format: YYYY-MM-DD (with dashes)
> - Example: `2024-01-15`
> - Human-readable format
> - Standard date format
> 
> **`{{ ds_nodash }}`:**
> - Format: YYYYMMDD (no dashes)
> - Example: `20240115`
> - Compact format
> - Good for filenames and IDs
> 
> **When to Use Each:**
> 
> **Use `{{ ds }}` when:**
> - Logging and display purposes
> - SQL queries with date filters
> - Human-readable output
> - Standard date comparisons
> 
> **Example:**
> ```python
> bash_command = '''
>     echo "Processing data for {{ ds }}"
>     SELECT * FROM sales WHERE date = '{{ ds }}'
> '''
> ```
> 
> **Use `{{ ds_nodash }}` when:**
> - Creating filenames (no special characters needed)
> - Generating unique identifiers
> - Compact date representation
> - Systems that don't handle dashes well
> 
> **Example:**
> ```python
> bash_command = '''
>     # Reading file with date in filename
>     cat /data/sales_{{ ds_nodash }}.csv
>     
>     # Creating output file
>     python process.py --output=/output/report_{{ ds_nodash }}.txt
> '''
> ```
> 
> **Real-World Scenario:**
> ```python
> # Good filename practice
> filepath = '/data/daily_sales_{{ ds_nodash }}.csv'  # sales_20240115.csv
> 
> # Good for logging
> bash_command = 'echo "Report generated for {{ ds }}"'  # 2024-01-15
> 
> # SQL query
> sql_query = "SELECT * FROM orders WHERE order_date = '{{ ds }}'"
> ```
> 
> **Summary:** Use `ds` for readability and standard formats, use `ds_nodash` for filenames and compact representations.

**Q27.** Describe how BranchPythonOperator works and provide an example of a branching function that chooses different tasks based on a condition.

> [!success]- Answer
> **How BranchPythonOperator Works:**
> 
> **Purpose:**
> - Enables conditional logic in workflows
> - Chooses which task(s) to execute next
> - Creates different execution paths
> 
> **Requirements:**
> 1. **python_callable** - Function that contains branching logic
> 2. **provide_context=True** - Gives access to execution context
> 3. **Function must accept `**kwargs`** - Receives Airflow variables
> 4. **Function must return task_id(s)** - String or list of strings
> 
> **How It Works:**
> 1. BranchPythonOperator executes the callable function
> 2. Function evaluates conditions using available context
> 3. Function returns task_id of next task to run
> 4. Airflow follows that path, skipping others
> 
> ---
> 
> **Example 1: Weekend vs Weekday Processing**
> 
> ```python
> from airflow import DAG
> from airflow.operators.python import BranchPythonOperator
> from airflow.operators.bash import BashOperator
> from datetime import datetime
> 
> def choose_processing_type(**kwargs):
>     """Branch based on day of week"""
>     execution_date = kwargs['ds']  # Get execution date
>     
>     # Convert to datetime and get day of week (0=Monday, 6=Sunday)
>     day_of_week = datetime.strptime(execution_date, '%Y-%m-%d').weekday()
>     
>     if day_of_week < 5:  # Monday-Friday (0-4)
>         return 'weekday_processing'
>     else:  # Saturday-Sunday (5-6)
>         return 'weekend_processing'
> 
> with DAG('branch_example', start_date=datetime(2024, 1, 1)) as dag:
>     
>     start = BashOperator(
>         task_id='start',
>         bash_command='echo "Starting process"'
>     )
>     
>     branch = BranchPythonOperator(
>         task_id='branch',
>         python_callable=choose_processing_type,
>         provide_context=True  # Required!
>     )
>     
>     weekday = BashOperator(
>         task_id='weekday_processing',
>         bash_command='echo "Running weekday logic"'
>     )
>     
>     weekend = BashOperator(
>         task_id='weekend_processing',
>         bash_command='echo "Running weekend logic"'
>     )
>     
>     # Dependencies
>     start >> branch >> [weekday, weekend]
> ```
> 
> ---
> 
> **Example 2: Data Volume Based Branching**
> 
> ```python
> def choose_processing_method(**kwargs):
>     """Branch based on data volume"""
>     # Simulate checking data size
>     # In reality, you might read from a file or database
>     
>     # Using ds_nodash to check even/odd day as proxy for size
>     execution_date_num = int(kwargs['ds_nodash'])
>     
>     if execution_date_num % 3 == 0:
>         return 'large_batch_processing'  # Divisible by 3
>     elif execution_date_num % 2 == 0:
>         return 'medium_batch_processing'  # Even but not divisible by 3
>     else:
>         return 'small_batch_processing'  # Odd
> 
> branch_task = BranchPythonOperator(
>     task_id='choose_method',
>     python_callable=choose_processing_method,
>     provide_context=True
> )
> ```
> 
> ---
> 
> **Key Points:**
> - Function receives `**kwargs` with context variables (ds, ds_nodash, dag, etc.)
> - Returns string of task_id to execute
> - Only returned path executes; others are skipped
> - Must have `provide_context=True`
> 
> **Flow Diagram:**
> ```
> start_task
>     ↓
> branch_task (evaluates condition)
>     ↓
>     ├─ (if condition A) → task_a → task_a2
>     └─ (if condition B) → task_b → task_b2
> 
> Only one path executes per run!
> ```

**Q28.** Explain what template_fields are and how to find which fields support templating for a given operator. Why is this important?

> [!success]- Answer
> **What are template_fields:**
> 
> **Definition:**
> `template_fields` are specific operator arguments that support Jinja templating. Not all operator parameters can use template variables.
> 
> **Purpose:**
> - Define which arguments accept dynamic values
> - Enable runtime substitution for specific fields
> - Prevent template syntax in non-template fields
> 
> **Example:**
> ```python
> # BashOperator template_fields = ('bash_command', 'env')
> 
> # ✅ Works - bash_command supports templates
> task = BashOperator(
>     task_id='templated',
>     bash_command='echo "Date: {{ ds }}"'  # Template works!
> )
> 
> # ❌ Doesn't work - task_id is not a template field
> task = BashOperator(
>     task_id='task_{{ ds }}',  # Template won't substitute
>     bash_command='echo "Hello"'
> )
> ```
> 
> ---
> 
> **How to Find Template Fields:**
> 
> **Method 1: Python help() Function**
> 
> ```python
> # Step 1: Open Python interpreter
> python3
> 
> # Step 2: Import the operator
> from airflow.operators.bash import BashOperator
> 
> # Step 3: Get help documentation
> help(BashOperator)
> 
> # Step 4: Look for template_fields line
> # Output shows:
> # template_fields = ('bash_command', 'env')
> ```
> 
> **Method 2: Check Source Code**
> ```python
> # In Python interpreter
> from airflow.operators.bash import BashOperator
> print(BashOperator.template_fields)
> # Output: ('bash_command', 'env')
> ```
> 
> **Method 3: Official Documentation**
> - Visit Airflow documentation
> - Find operator reference
> - Look for "Template Fields" section
> 
> ---
> 
> **Examples by Operator:**
> 
> **BashOperator:**
> ```python
> template_fields = ('bash_command', 'env')
> 
> # ✅ bash_command - supports templates
> bash_command='echo {{ ds }}'
> 
> # ✅ env - supports templates
> env={'DATE': '{{ ds }}'}
> ```
> 
> **PythonOperator:**
> ```python
> template_fields = ('op_args', 'op_kwargs')
> 
> # ✅ op_kwargs - supports templates
> op_kwargs={'date': '{{ ds }}'}
> ```
> 
> **FileSensor:**
> ```python
> template_fields = ('filepath',)
> 
> # ✅ filepath - supports templates
> filepath='/data/sales_{{ ds_nodash }}.csv'
> ```
> 
> ---
> 
> **Why This is Important:**
> 
> **1. Prevents Errors**
> - Trying to use templates in non-template fields won't work
> - Helps understand operator limitations
> - Saves debugging time
> 
> **2. Enables Dynamic Workflows**
> - Know where you can use runtime variables
> - Create flexible, reusable tasks
> - Avoid hardcoding values
> 
> **3. Best Practices**
> - Use templates where supported
> - Don't waste time trying templates on non-supported fields
> - Understand operator capabilities
> 
> **4. Documentation and Maintenance**
> - Others know which fields are dynamic
> - Clear understanding of DAG behavior
> - Easier to modify and extend
> 
> ---
> 
> **Complete Example:**
> 
> ```python
> from airflow.operators.bash import BashOperator
> 
> # Check template fields first
> print(BashOperator.template_fields)
> # Output: ('bash_command', 'env')
> 
> # Now use templates appropriately
> task = BashOperator(
>     task_id='daily_report',  # ❌ Not a template field
>     bash_command='''
>         # ✅ bash_command IS a template field
>         echo "Processing {{ ds }}"
>         python generate_report.py --date {{ ds }}
>         cp report.pdf /reports/report_{{ ds_nodash }}.pdf
>     ''',
>     env={
>         # ✅ env IS a template field
>         'PROCESS_DATE': '{{ ds }}',
>         'OUTPUT_DIR': '/output/{{ ds_nodash }}'
>     }
> )
> ```
> 
> **Summary:** Always check template_fields to know which operator arguments support templating. This prevents errors and enables proper use of Airflow's dynamic capabilities.

---

## Section D: Code Analysis (2 points each = 10 points)

**Q29.** What will this template output?
```python
templated_command = """
{% for filename in params.filenames %}
echo "Reading {{ filename }}"
{% endfor %}
"""

t1 = BashOperator(
    task_id='template_task',
    bash_command=templated_command,
    params={'filenames': ['data1.csv', 'data2.csv', 'data3.csv']},
    dag=example_dag
)
```

> [!success]- Answer
> **Output:**
> ```
> Reading data1.csv
> Reading data2.csv
> Reading data3.csv
> ```
> 
> **How it works:**
> 
> **Template breakdown:**
> ```jinja
> {% for filename in params.filenames %}  # Loop through list
>     echo "Reading {{ filename }}"        # Print each filename
> {% endfor %}                             # End loop
> ```
> 
> **Execution steps:**
> 1. Template receives `params.filenames` = `['data1.csv', 'data2.csv', 'data3.csv']`
> 2. For loop iterates through each filename
> 3. Each iteration echoes "Reading [filename]"
> 4. Three echo commands generated
> 
> **Equivalent to:**
> ```bash
> echo "Reading data1.csv"
> echo "Reading data2.csv"
> echo "Reading data3.csv"
> ```
> 
> **Use case:** Processing multiple files with same command template, avoiding repetitive task definitions.

**Q30.** Analyze this branching function. What tasks will it return for these dates: 2024-01-14 and 2024-01-15?
```python
def branch_test(**kwargs):
    if int(kwargs['ds_nodash']) % 2 == 0:
        return 'even_day_task'
    else:
        return 'odd_day_task'
```

> [!success]- Answer
> **Analysis:**
> 
> **Function logic:**
> - Gets execution date without dashes (ds_nodash)
> - Converts to integer
> - Checks if divisible by 2 (even number)
> - Returns 'even_day_task' if even, 'odd_day_task' if odd
> 
> ---
> 
> **For 2024-01-14:**
> 
> ```
> ds_nodash = '20240114'
> int('20240114') = 20240114
> 20240114 % 2 = 0  (even number!)
> Returns: 'even_day_task'
> ```
> 
> **Result:** Executes `even_day_task`
> 
> ---
> 
> **For 2024-01-15:**
> 
> ```
> ds_nodash = '20240115'
> int('20240115') = 20240115
> 20240115 % 2 = 1  (odd number!)
> Returns: 'odd_day_task'
> ```
> 
> **Result:** Executes `odd_day_task`
> 
> ---
> 
> **Summary Table:**
> 
> | Date | ds_nodash | Integer | Even/Odd | Task Executed |
> |------|-----------|---------|----------|---------------|
> | 2024-01-14 | 20240114 | 20240114 | Even | even_day_task |
> | 2024-01-15 | 20240115 | 20240115 | Odd | odd_day_task |
> 
> **Note:** The function checks if the numeric representation of the date is even/odd, not the day of the month. So January 14 (20240114) is even, January 15 (20240115) is odd.
> 
> **Workflow Result:**
> ```
> start_task
>     ↓
> branch_task
>     ↓
> (Jan 14) even_day_task  OR  (Jan 15) odd_day_task
> ```

**Q31.** What does this template produce if executed on January 15, 2024?
```python
templated_command = """
echo "Today: {{ ds }}"
echo "Yesterday: {{ prev_ds }}"
echo "Next week: {{ macros.ds_add(ds, 7) }}"
echo "File: data_{{ ds_nodash }}.csv"
"""
```

> [!success]- Answer
> **Output for January 15, 2024:**
> 
> ```
> Today: 2024-01-15
> Yesterday: 2024-01-14
> Next week: 2024-01-22
> File: data_20240115.csv
> ```
> 
> **Line-by-line breakdown:**
> 
> **Line 1:** `echo "Today: {{ ds }}"`
> - `ds` = execution date in YYYY-MM-DD format
> - Output: `Today: 2024-01-15`
> 
> **Line 2:** `echo "Yesterday: {{ prev_ds }}"`
> - `prev_ds` = previous execution date
> - Assuming daily schedule, previous run was Jan 14
> - Output: `Yesterday: 2024-01-14`
> 
> **Line 3:** `echo "Next week: {{ macros.ds_add(ds, 7) }}"`
> - `ds_add(ds, 7)` = add 7 days to current date
> - 2024-01-15 + 7 days = 2024-01-22
> - Output: `Next week: 2024-01-22`
> 
> **Line 4:** `echo "File: data_{{ ds_nodash }}.csv"`
> - `ds_nodash` = execution date without dashes (YYYYMMDD)
> - Output: `File: data_20240115.csv`
> 
> ---
> 
> **Variables Used:**
> 
> | Variable | Value | Format |
> |----------|-------|--------|
> | `ds` | 2024-01-15 | YYYY-MM-DD |
> | `prev_ds` | 2024-01-14 | YYYY-MM-DD |
> | `ds_nodash` | 20240115 | YYYYMMDD |
> | `macros.ds_add(ds, 7)` | 2024-01-22 | YYYY-MM-DD |
> 
> **Use case:** This pattern is common for ETL tasks that need to reference current date, previous date for incremental processing, future dates for forecasting, and date-based filenames.

**Q32.** Explain what this command does:
```bash
airflow dags trigger -e 2024-01-15 my_etl_dag
```

> [!success]- Answer
> **What it does:**
> 
> Manually triggers the entire `my_etl_dag` DAG to run with execution date of January 15, 2024.
> 
> **Command breakdown:**
> 
> **`airflow`**
> - Base Airflow command-line tool
> 
> **`dags trigger`**
> - Subcommand to manually trigger a DAG run
> - Creates new DAG run instance
> - Does not wait for schedule
> 
> **`-e 2024-01-15`**
> - `-e` flag: execution date
> - Sets logical date for this run
> - Used in templates (ds, ds_nodash, etc.)
> - Format: YYYY-MM-DD
> 
> **`my_etl_dag`**
> - The DAG ID to trigger
> - Must match dag_id in DAG definition
> 
> ---
> 
> **Effect:**
> 
> 1. **Creates DAG run** for my_etl_dag
> 2. **Sets execution date** to 2024-01-15
> 3. **All template variables** use this date:
>    - `{{ ds }}` = `2024-01-15`
>    - `{{ ds_nodash }}` = `20240115`
> 4. **Starts execution** immediately (doesn't wait for schedule)
> 5. **Runs all tasks** in the DAG following dependencies
> 
> ---
> 
> **When to use:**
> 
> **Manual execution:**
> - Testing after DAG changes
> - Backfilling specific dates
> - Reprocessing data for a particular date
> - Running outside normal schedule
> 
> **Example scenarios:**
> ```bash
> # Test new DAG
> airflow dags trigger -e 2024-01-15 my_etl_dag
> 
> # Backfill missing date
> airflow dags trigger -e 2024-01-10 my_etl_dag
> 
> # Reprocess due to error
> airflow dags trigger -e 2024-01-12 my_etl_dag
> ```
> 
> ---
> 
> **Compare with:**
> 
> **`airflow tasks test`** - Tests single task:
> ```bash
> airflow tasks test my_etl_dag extract_task 2024-01-15
> # Runs only extract_task, not full DAG
> ```
> 
> **`airflow dags trigger`** - Triggers entire DAG:
> ```bash
> airflow dags trigger -e 2024-01-15 my_etl_dag
> # Runs ALL tasks in DAG
> ```
> 
> ---
> 
> **Output:**
> ```bash
> $ airflow dags trigger -e 2024-01-15 my_etl_dag
> Created <DagRun my_etl_dag @ 2024-01-15 00:00:00+00:00: manual__2024-01-15T00:00:00+00:00, externally triggered: True>
> ```
> 
> **Summary:** This command manually starts a complete DAG run for a specific execution date, useful for testing, backfilling, or reprocessing data.

**Q33.** What would you need to check if this template doesn't work as expected?
```python
task = BashOperator(
    task_id='process_{{ ds }}',
    bash_command='echo "Date: {{ ds }}"'
)
```

> [!success]- Answer
> **Problem Identification:**
> 
> This code has an issue: `task_id` is using a template, but `task_id` is NOT a template field for BashOperator.
> 
> **What to check:**
> 
> **1. Check template_fields**
> 
> ```python
> from airflow.operators.bash import BashOperator
> help(BashOperator)
> 
> # or
> print(BashOperator.template_fields)
> # Output: ('bash_command', 'env')
> ```
> 
> **Finding:** `task_id` is NOT in template_fields!
> 
> ---
> 
> **2. Identify the issues:**
> 
> **Issue #1: task_id with template**
> ```python
> task_id='process_{{ ds }}'  # ❌ Won't work!
> ```
> - `task_id` is not a template field
> - `{{ ds }}` won't be substituted
> - Task will literally be named "process_{{ ds }}"
> - Causes confusion and errors
> 
> **Issue #2: Expected behavior**
> ```python
> bash_command='echo "Date: {{ ds }}"'  # ✅ Works!
> ```
> - `bash_command` IS a template field
> - `{{ ds }}` will be substituted correctly
> - Will echo the actual date
> 
> ---
> 
> **3. Correct the code:**
> 
> **Fixed version:**
> ```python
> task = BashOperator(
>     task_id='process_date',  # ✅ Static task_id
>     bash_command='echo "Date: {{ ds }}"'  # ✅ Template in bash_command
> )
> ```
> 
> **Alternative if you need date in task:**
> ```python
> # Option 1: Use parameter in command
> task = BashOperator(
>     task_id='process_date',
>     bash_command='echo "Processing {{ ds }}"'
> )
> 
> # Option 2: Include date in output/logs
> task = BashOperator(
>     task_id='daily_process',
>     bash_command='''
>         echo "Task ID: daily_process"
>         echo "Processing date: {{ ds }}"
>         python process.py --date {{ ds }}
>     '''
> )
> ```
> 
> ---
> 
> **4. How to debug:**
> 
> **Check operator template_fields:**
> ```python
> # In Python
> from airflow.operators.bash import BashOperator
> print(BashOperator.template_fields)
> # Shows: ('bash_command', 'env')
> 
> # Or use help
> help(BashOperator)
> # Look for: template_fields = ...
> ```
> 
> **Test the task:**
> ```bash
> # Run the task
> airflow tasks test my_dag process_{{ ds }} 2024-01-15
> 
> # If task_id has literal {{ ds }}, you'll see error:
> # "Task 'process_{{ ds }}' not found"
> ```
> 
> ---
> 
> **Summary of what to check:**
> 
> 1. ✅ **Verify template_fields** - Is the field you're templating actually supported?
> 2. ✅ **Check operator documentation** - Use `help()` or docs to confirm
> 3. ✅ **Test the task** - Run it and check if templates substitute
> 4. ✅ **Review task_id naming** - Keep task_ids static and descriptive
> 5. ✅ **Use templates in supported fields** - bash_command, env, filepath, etc.
> 
> **Key takeaway:** Not all operator fields support templates. Always verify template_fields before using Jinja syntax in an argument.

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (26 points)
1. B | 2. C | 3. C | 4. B | 5. B | 6. B | 7. B | 8. B | 9. B | 10. B | 11. B | 12. B | 13. C

### Section B: True/False (12 points)
14. T | 15. F | 16. F | 17. T | 18. T | 19. F | 20. T | 21. T | 22. F | 23. T | 24. T | 25. F

### Section C: Short Answer (12 points)
See detailed answers above - award full points for comprehensive responses

### Section D: Code Analysis (10 points)
See detailed answers above - award points for correct interpretation

---

## Quick Study Guide

### Template Syntax

```python
# Variable substitution
{{ variable }}

# Parameters
{{ params.filename }}

# Built-in variables
{{ ds }}           # 2024-01-15
{{ ds_nodash }}    # 20240115
{{ prev_ds }}      # 2024-01-14
{{ prev_ds_nodash }}  # 20240114

# Macros
{{ macros.ds_add(ds, 5) }}  # Add 5 days
{{ macros.timedelta }}      # timedelta object
{{ macros.datetime }}       # datetime object

# Loops
{% for item in params.list %}
    {{ item }}
{% endfor %}
```

### Branching Pattern

```python
def branch_function(**kwargs):
    if condition:
        return 'task_a'
    else:
        return 'task_b'

branch = BranchPythonOperator(
    task_id='branch',
    python_callable=branch_function,
    provide_context=True  # Required!
)
```

### Running Commands

```bash
# Test single task
airflow tasks test <dag_id> <task_id> <date>

# Trigger full DAG
airflow dags trigger -e <date> <dag_id>
```

### Check Template Fields

```python
from airflow.operators.bash import BashOperator
help(BashOperator)
# Look for: template_fields = ('bash_command', 'env')
```

---

## Key Concepts Summary

| Concept | Definition |
|---------|------------|
| **Templates** | Dynamic substitution using Jinja |
| **Jinja** | Templating language used by Airflow |
| **`{{ params }}`** | Access task parameters |
| **`{{ ds }}`** | Execution date (YYYY-MM-DD) |
| **`{{ ds_nodash }}`** | Execution date (YYYYMMDD) |
| **`{{ prev_ds }}`** | Previous execution date |
| **Macros** | Useful Python objects/methods |
| **`ds_add()`** | Add/subtract days from date |
| **Branching** | Conditional workflow logic |
| **BranchPythonOperator** | Operator for conditional paths |
| **`provide_context`** | Give access to kwargs |
| **template_fields** | Operator fields supporting templates |

---

## Common Mistakes to Avoid

❌ **Don't:**
- Use templates in non-template fields (e.g., task_id)
- Forget `provide_context=True` in BranchPythonOperator
- Forget `**kwargs` in branching functions
- Confuse `ds` (YYYY-MM-DD) with `ds_nodash` (YYYYMMDD)
- Use templates without checking template_fields

✅ **Do:**
- Check template_fields before using templates
- Use `provide_context=True` for branching
- Accept `**kwargs` in branching functions
- Use `ds` for display, `ds_nodash` for filenames
- Test templates with `airflow tasks test`
- Use `macros.ds_add()` for date arithmetic

---

## Study Strategy (3-Day Plan)

### Day 1: Templates Basics
- Learn Jinja syntax
- Understand template variables
- Practice with params
- Study built-in variables (ds, ds_nodash)

### Day 2: Advanced Templates and Branching
- Learn macros and ds_add()
- Understand template_fields
- Study BranchPythonOperator
- Practice branching functions

### Day 3: Production Skills
- Practice running commands
- Review all operators
- Build complete examples
- Take practice quiz

---

## Before the Exam - Final Checklist

- [ ] Know what templates are (Jinja-based substitution)
- [ ] Understand basic template syntax `{{ }}`
- [ ] Know template loops `{% for %}`
- [ ] Remember `{{ ds }}` format (YYYY-MM-DD)
- [ ] Remember `{{ ds_nodash }}` format (YYYYMMDD)
- [ ] Know `{{ prev_ds }}` gives previous date
- [ ] Understand `{{ macros.ds_add() }}` usage
- [ ] Know BranchPythonOperator for branching
- [ ] Remember `provide_context=True` required
- [ ] Know branching function needs `**kwargs`
- [ ] Understand template_fields concept
- [ ] Can check template_fields with help()
- [ ] Know `airflow tasks test` command
- [ ] Know `airflow dags trigger` command

---

#apache-airflow #templates #jinja #branching #macros #production #branchpythonoperator #ds #ds_nodash #template_fields #params #workflow-automation #datacamp #chapter4 #practice-quiz
