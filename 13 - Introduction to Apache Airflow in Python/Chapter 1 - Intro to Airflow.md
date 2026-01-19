## What is Data Engineering?

**Definition:** Taking any action involving data and turning it into a **reliable**, **repeatable**, and **maintainable** process.

Key characteristics:
- **Reliable** - Works consistently without failure
- **Repeatable** - Can be run multiple times with same results
- **Maintainable** - Easy to update and troubleshoot

---

## What is a Workflow?

**Definition:** A set of steps to accomplish a given data engineering task

### Examples of Workflow Steps:
- Downloading files
- Copying data
- Filtering information
- Writing to a database

### Characteristics:
- Varying levels of complexity
- Context-dependent meaning
- Sequential or parallel execution

---

## What is Apache Airflow?

**Apache Airflow** is a platform to program workflows, including:
- **Creation** - Build workflows programmatically
- **Scheduling** - Automate execution timing
- **Monitoring** - Track workflow status and health

### Key Features:

1. **Language Support**
   - Workflows written in **Python**
   - Can implement programs from any language

2. **DAG-Based**
   - Implements workflows as **DAGs** (Directed Acyclic Graphs)
   - Represents task dependencies visually

3. **Multiple Access Methods**
   - Code (Python)
   - Command-line interface
   - Web interface
   - REST API

4. **Official Documentation**
   - https://airflow.apache.org/docs/stable/

---

## Other Workflow Tools

Apache Airflow is not the only workflow orchestration tool:

| Tool | Description |
|------|-------------|
| **Luigi** | Python-based workflow tool by Spotify |
| **SSIS** | SQL Server Integration Services (Microsoft) |
| **Bash scripting** | Traditional shell-based automation |

---

## Introduction to DAGs

### What is a DAG?

**DAG** stands for **Directed Acyclic Graph**

**Breaking it down:**
- **Directed** - Has an inherent flow representing dependencies between components
- **Acyclic** - Does not loop, cycle, or repeat
- **Graph** - The actual set of components

### In Airflow Context:

A DAG represents:
- The set of **tasks** that make up your workflow
- The **dependencies** between those tasks

### DAG Components:

**Created with details about:**
- Name (dag_id)
- Start date
- Owner
- Schedule
- Other configuration parameters

### Where DAGs are Used:
- Apache Airflow
- Apache Spark
- dbt (data build tool)

---

## Creating a DAG in Airflow

### Modern Syntax (Airflow 2.x+)

```python
from airflow import DAG
from datetime import datetime

default_arguments = {
    'owner': 'jdoe',
    'email': 'jdoe@datacamp.com',
    'start_date': datetime(2020, 1, 20)
}

# Using context manager (recommended)
with DAG('etl_workflow', default_args=default_arguments) as etl_dag:
    # Tasks defined here
    pass
```

### Legacy Syntax (Before Airflow 2.x)

```python
from airflow import DAG
from datetime import datetime

default_arguments = {
    'owner': 'jdoe',
    'email': 'jdoe@datacamp.com',
    'start_date': datetime(2020, 1, 20)
}

# Direct assignment
etl_dag = DAG('etl_workflow', default_args=default_arguments)
```

### Simple DAG Definition

```python
etl_dag = DAG(
    dag_id='etl_pipeline',
    default_args={"start_date": "2024-01-08"}
)
```

---

## DAG Characteristics

### In Airflow, DAGs:

1. **Are written in Python**
   - But can use components written in other languages

2. **Are made up of components**
   - Typically **tasks** to be executed
   - Such as operators, sensors, etc.

3. **Contain dependencies**
   - Defined explicitly or implicitly
   - Example: "Copy the file to the server before trying to import it to the database service"

---

## Command Line Interface

### Running Airflow from Command Line

**Main command:** `airflow`

**Getting help:**
```bash
airflow -h
# Shows all available subcommands
```

### Common DAG Commands

**List all DAGs:**
```bash
airflow dags list
# Shows all recognized DAGs
```

**Run a specific task:**
```bash
airflow tasks test <dag_id> <task_id> [execution_date]
```

**Example:**
```bash
airflow tasks test example-etl download-file 2024-01-10
# Runs the 'download-file' task from 'example-etl' DAG for date 2024-01-10
```

---

## Command Line vs Python Usage

### Use Command Line For:

✅ Start Airflow processes
✅ Manually run DAGs/Tasks
✅ Get logging information from Airflow
✅ Administrative operations

### Use Python For:

✅ Create a DAG
✅ Edit individual properties of a DAG
✅ Define task logic
✅ Configure workflow behavior

---

## Airflow Web Interface

### Main Views

#### 1. **DAGs View**
The main dashboard showing all available DAGs

**Displays:**
- DAG names
- Owner information
- Run status
- Schedule information
- Last run time
- Next run time
- Recent task status

#### 2. **DAG Detail View**
Detailed information about a specific DAG

#### 3. **Graph View**
Visual representation of DAG structure showing:
- Tasks as nodes
- Dependencies as edges
- Task execution status

#### 4. **Code View**
Shows the actual Python code defining the DAG

#### 5. **Audit Logs**
Track changes and execution history

---

## Web UI vs Command Line

### Comparison

| Aspect | Web UI | Command Line |
|--------|--------|--------------|
| **Power** | Equally powerful | Equally powerful |
| **Ease of Use** | Easier for most tasks | May require more knowledge |
| **Accessibility** | Browser-based | Terminal/SSH access |
| **Visualization** | Excellent (graphs, charts) | Text-based output |
| **Monitoring** | Real-time dashboard | Manual commands |

### When to Use Each:

**Web UI:**
- Monitoring DAG runs
- Viewing task logs
- Triggering manual runs
- Exploring DAG structure visually

**Command Line:**
- Automated scripts
- Remote server access
- CI/CD pipelines
- Quick testing during development

---

## Key Concepts Summary

| Concept | Definition |
|---------|------------|
| **Data Engineering** | Making data processes reliable, repeatable, maintainable |
| **Workflow** | Set of steps to accomplish a data task |
| **Airflow** | Platform for programming, scheduling, and monitoring workflows |
| **DAG** | Directed Acyclic Graph - represents workflow structure |
| **Directed** | Has flow/direction (dependencies) |
| **Acyclic** | No loops or cycles |
| **Graph** | Set of connected components |
| **Task** | Individual unit of work in a DAG |
| **Operator** | Defines what a task does |
| **Sensor** | Waits for a condition before proceeding |

---

## Quick Reference

### Essential Commands

```bash
# List all DAGs
airflow dags list

# Run a task
airflow tasks test <dag_id> <task_id> <date>

# Get help
airflow -h
```

### Basic DAG Structure

```python
from airflow import DAG
from datetime import datetime

default_args = {
    'owner': 'your_name',
    'start_date': datetime(2024, 1, 1)
}

with DAG('my_dag', default_args=default_args) as dag:
    # Define tasks here
    pass
```

---

## Important Notes

> [!important]
> - DAGs must be **acyclic** - no circular dependencies allowed
> - All workflows in Airflow are defined as DAGs
> - DAGs are written in Python but can execute code in any language
> - Start date is required for every DAG

> [!tip]
> - Use the Web UI for monitoring and visualization
> - Use the command line for quick testing and automation
> - Always define default_args for consistent DAG behavior

---

#apache-airflow #workflow-orchestration #data-engineering #dags #python #etl #scheduling #monitoring #datacamp #chapter1

# Chapter 1: Introduction to Apache Airflow - Practice Quiz

**Total Points: 50 points**
**Passing Score: 35 points (70%)**

---

## Section A: Multiple Choice Questions (2 points each = 20 points)

### Data Engineering and Workflows

**Q1.** What is data engineering?

- [ ] A) Only analyzing data
- [ ] B) Taking any action involving data and turning it into a reliable, repeatable, and maintainable process
- [ ] C) Creating databases
- [ ] D) Writing Python code

> [!success]- Answer
> **Correct Answer: B) Taking any action involving data and turning it into a reliable, repeatable, and maintainable process**
> 
> Data engineering focuses on making data processes reliable (consistent), repeatable (can run multiple times), and maintainable (easy to update).

**Q2.** Which of the following is NOT mentioned as a workflow step?

- [ ] A) Downloading files
- [ ] B) Copying data
- [ ] C) Training machine learning models
- [ ] D) Writing to a database

> [!success]- Answer
> **Correct Answer: C) Training machine learning models**
> 
> The chapter mentions downloading files, copying data, filtering information, and writing to a database as workflow examples. Machine learning training was not mentioned.

**Q3.** What does Apache Airflow allow you to do?

- [ ] A) Only create workflows
- [ ] B) Create, schedule, and monitor workflows
- [ ] C) Only schedule workflows
- [ ] D) Only monitor workflows

> [!success]- Answer
> **Correct Answer: B) Create, schedule, and monitor workflows**
> 
> Airflow is a comprehensive platform that handles all three aspects: creation, scheduling, and monitoring of workflows.

### DAGs (Directed Acyclic Graphs)

**Q4.** What does DAG stand for?

- [ ] A) Data Analysis Graph
- [ ] B) Directed Acyclic Graph
- [ ] C) Daily Automated Gateway
- [ ] D) Database Access Graph

> [!success]- Answer
> **Correct Answer: B) Directed Acyclic Graph**
> 
> DAG stands for Directed Acyclic Graph, which represents the workflow structure in Airflow.

**Q5.** What does "Directed" mean in Directed Acyclic Graph?

- [ ] A) The graph is sorted alphabetically
- [ ] B) There is an inherent flow representing dependencies between components
- [ ] C) The tasks run in random order
- [ ] D) The graph points north

> [!success]- Answer
> **Correct Answer: B) There is an inherent flow representing dependencies between components**
> 
> "Directed" means there's a specific flow or direction showing how tasks depend on each other.

**Q6.** What does "Acyclic" mean in DAG?

- [ ] A) The graph repeats continuously
- [ ] B) The graph has loops
- [ ] C) The graph does not loop, cycle, or repeat
- [ ] D) The graph runs in cycles

> [!success]- Answer
> **Correct Answer: C) The graph does not loop, cycle, or repeat**
> 
> "Acyclic" means no cycles or loops - the workflow has a clear beginning and end without circular dependencies.

**Q7.** In Airflow, what does a DAG represent?

- [ ] A) Only the schedule
- [ ] B) The set of tasks and dependencies that make up your workflow
- [ ] C) Just the Python code
- [ ] D) Only the database connections

> [!success]- Answer
> **Correct Answer: B) The set of tasks and dependencies that make up your workflow**
> 
> A DAG in Airflow represents the complete workflow including all tasks and how they depend on each other.

**Q8.** What language are Airflow workflows written in?

- [ ] A) JavaScript
- [ ] B) SQL only
- [ ] C) Python
- [ ] D) Java

> [!success]- Answer
> **Correct Answer: C) Python**
> 
> Airflow workflows are written in Python, though they can execute programs from any language.

### Accessing Airflow

**Q9.** Which of the following is NOT a way to access Airflow?

- [ ] A) Code (Python)
- [ ] B) Command-line
- [ ] C) Web interface
- [ ] D) Mobile app

> [!success]- Answer
> **Correct Answer: D) Mobile app**
> 
> Airflow can be accessed via code, command-line, web interface, and REST API. Mobile app is not mentioned.

**Q10.** What command lists all recognized DAGs in Airflow?

- [ ] A) `airflow show dags`
- [ ] B) `airflow dags list`
- [ ] C) `airflow list all`
- [ ] D) `airflow get dags`

> [!success]- Answer
> **Correct Answer: B) `airflow dags list`**
> 
> The command `airflow dags list` displays all DAGs that Airflow recognizes.

---

## Section B: True/False Questions (1 point each = 10 points)

**Q11.** Data engineering involves making data processes reliable, repeatable, and maintainable. **T/F**

> [!success]- Answer
> **True** - This is the core definition of data engineering provided in the chapter.

**Q12.** Airflow can only schedule workflows, not create or monitor them. **T/F**

> [!success]- Answer
> **False** - Airflow handles creation, scheduling, AND monitoring of workflows.

**Q13.** DAG stands for "Data Analysis Graph". **T/F**

> [!success]- Answer
> **False** - DAG stands for "Directed Acyclic Graph".

**Q14.** In a DAG, "Acyclic" means the workflow can loop back on itself. **T/F**

> [!success]- Answer
> **False** - "Acyclic" means it does NOT loop, cycle, or repeat.

**Q15.** Airflow workflows are written in Python but can implement programs from any language. **T/F**

> [!success]- Answer
> **True** - Workflows are defined in Python but can execute code in any language.

**Q16.** Luigi is mentioned as an alternative workflow tool to Airflow. **T/F**

> [!success]- Answer
> **True** - Luigi, SSIS, and Bash scripting are mentioned as other workflow tools.

**Q17.** A DAG in Airflow must include both tasks and their dependencies. **T/F**

> [!success]- Answer
> **True** - DAGs consist of tasks and the dependencies between them.

**Q18.** The command line and Web UI have completely different capabilities in Airflow. **T/F**

> [!success]- Answer
> **False** - They are equally powerful depending on needs; the Web UI is just easier to use.

**Q19.** In Airflow 2.x+, you can use a context manager (`with DAG...`) to define a DAG. **T/F**

> [!success]- Answer
> **True** - The modern syntax uses `with DAG(...) as dag:` for DAG definition.

**Q20.** The Web UI can show a graph view of your DAG structure. **T/F**

> [!success]- Answer
> **True** - The Web UI includes a graph view that visually represents DAG structure.

---

## Section C: Short Answer Questions (3 points each = 12 points)

**Q21.** Explain the three characteristics that define data engineering.

> [!success]- Answer
> Data engineering is about making data processes have three key characteristics:
> 
> 1. **Reliable** - The process works consistently without failure. It produces expected results every time it runs.
> 
> 2. **Repeatable** - The process can be run multiple times and will produce the same results given the same inputs. It's not a one-off manual task.
> 
> 3. **Maintainable** - The process is easy to update, troubleshoot, and modify as requirements change. Code is organized and documented.
> 
> Together, these characteristics transform ad-hoc data tasks into professional, production-ready workflows.

**Q22.** Describe what each part of "Directed Acyclic Graph" means in the context of Airflow DAGs.

> [!success]- Answer
> **DAG = Directed Acyclic Graph**
> 
> **Directed:**
> - Has an inherent flow or direction
> - Represents dependencies between components
> - Shows which tasks must complete before others can start
> - Example: Task B depends on Task A completing first
> 
> **Acyclic:**
> - Does not loop, cycle, or repeat
> - No circular dependencies allowed
> - Has a clear beginning and end
> - Task A cannot depend on Task B if Task B depends on Task A
> 
> **Graph:**
> - The actual set of components (tasks)
> - Visual representation of the workflow
> - Nodes represent tasks
> - Edges represent dependencies
> 
> **In Airflow:** A DAG represents the complete workflow structure including all tasks and their relationships.

**Q23.** Compare when you should use the command line versus Python when working with Airflow.

> [!success]- Answer
> **Use Command Line For:**
> - Starting Airflow processes (webserver, scheduler)
> - Manually running DAGs or specific tasks
> - Getting logging information from Airflow
> - Testing tasks during development
> - Administrative operations
> - Quick checks of DAG status
> - Automated scripts and CI/CD pipelines
> 
> **Use Python For:**
> - Creating a new DAG
> - Editing individual properties of a DAG
> - Defining task logic and operators
> - Configuring workflow behavior
> - Setting up dependencies between tasks
> - Writing custom operators or sensors
> 
> **Summary:** Command line is for **running and managing** Airflow, Python is for **defining and creating** workflows.

**Q24.** List the different ways you can access Airflow and explain when the Web UI might be preferred over the command line.

> [!success]- Answer
> **Ways to Access Airflow:**
> 1. **Code (Python)** - For defining DAGs and workflows
> 2. **Command-line interface** - For testing and administration
> 3. **Web interface** - For monitoring and visualization
> 4. **REST API** - For programmatic access and integration
> 
> **When to Prefer Web UI:**
> 
> **Web UI is Better For:**
> - **Monitoring** - Real-time dashboard showing DAG status
> - **Visualization** - Graph view of task dependencies
> - **Exploration** - Browsing available DAGs and their details
> - **Viewing logs** - Easy access to task execution logs
> - **Manual triggers** - Click to run DAGs
> - **Team collaboration** - Multiple users can view simultaneously
> - **Non-technical users** - More intuitive interface
> 
> **Command Line is Better For:**
> - **Automation** - Scripting and CI/CD integration
> - **Remote access** - SSH terminal sessions
> - **Quick testing** - Fast task execution during development
> - **Server environments** - When GUI access is limited
> 
> **Conclusion:** Both are equally powerful, but Web UI is generally easier for monitoring and visualization, while command line excels in automation and remote management.

---

## Section D: Command and Code Analysis (2 points each = 8 points)

**Q25.** What does this command do?
```bash
airflow tasks test example-etl download-file 2024-01-10
```

> [!success]- Answer
> **What it does:**
> This command runs a specific task in test mode without affecting the scheduler or database state.
> 
> **Breaking down the command:**
> - `airflow tasks test` - Test command for running a single task
> - `example-etl` - DAG ID (the workflow name)
> - `download-file` - Task ID (specific task to run)
> - `2024-01-10` - Execution date (the logical date for this run)
> 
> **Purpose:**
> - Tests a single task without running the entire DAG
> - Useful during development and debugging
> - Doesn't record the run in Airflow's database
> - Allows quick verification that a task works correctly
> 
> **Use case:** Testing the "download-file" task from the "example-etl" DAG to ensure it works before deploying to production.

**Q26.** Analyze this DAG definition. What are the key components?
```python
from airflow import DAG
from datetime import datetime

default_arguments = {
    'owner': 'jdoe',
    'email': 'jdoe@datacamp.com',
    'start_date': datetime(2020, 1, 20)
}

with DAG('etl_workflow', default_args=default_arguments) as etl_dag:
    pass
```

> [!success]- Answer
> **Key Components:**
> 
> **1. Imports:**
> - `from airflow import DAG` - Imports the DAG class
> - `from datetime import datetime` - For date handling
> 
> **2. Default Arguments Dictionary:**
> ```python
> default_arguments = {
>     'owner': 'jdoe',              # Who owns this DAG
>     'email': 'jdoe@datacamp.com',  # Contact email
>     'start_date': datetime(2020, 1, 20)  # When DAG becomes active
> }
> ```
> - Sets defaults that apply to all tasks in the DAG
> - `owner`: Identifies who's responsible for the DAG
> - `email`: Used for alerting and notifications
> - `start_date`: Required - when the DAG should start running
> 
> **3. DAG Definition:**
> ```python
> with DAG('etl_workflow', default_args=default_arguments) as etl_dag:
> ```
> - Uses context manager syntax (Airflow 2.x+)
> - `'etl_workflow'`: DAG ID (unique identifier)
> - `default_args=default_arguments`: Applies the defaults
> - `as etl_dag`: Variable name to reference this DAG
> 
> **4. Task Area:**
> - `pass`: Placeholder - tasks would be defined here
> - This is where operators and sensors would be added
> 
> **What's Missing:**
> This DAG has no tasks yet. In practice, you'd add operators inside the `with` block to define what the workflow actually does.

**Q27.** What is the difference between these two DAG definition styles?
```python
# Style 1
with DAG('etl_workflow', default_args=default_arguments) as etl_dag:
    pass

# Style 2
etl_dag = DAG('etl_workflow', default_args=default_arguments)
```

> [!success]- Answer
> **Style 1: Context Manager (Modern - Airflow 2.x+)**
> ```python
> with DAG('etl_workflow', default_args=default_arguments) as etl_dag:
>     # Tasks defined here automatically belong to etl_dag
>     task1 = SomeOperator(task_id='task1')
> ```
> 
> **Advantages:**
> - **Recommended** for Airflow 2.x and later
> - Tasks defined inside automatically belong to the DAG
> - Cleaner syntax, more Pythonic
> - Prevents errors from forgetting to specify dag parameter
> - Uses Python's context manager pattern
> 
> **Style 2: Direct Assignment (Legacy - Before Airflow 2.x)**
> ```python
> etl_dag = DAG('etl_workflow', default_args=default_arguments)
> task1 = SomeOperator(task_id='task1', dag=etl_dag)  # Must specify dag!
> ```
> 
> **Characteristics:**
> - Older style, still works but not recommended
> - Must explicitly pass `dag=etl_dag` to each operator
> - More verbose
> - More prone to errors (forgetting dag parameter)
> 
> **Recommendation:** Use Style 1 (context manager) for new DAGs in Airflow 2.x+. Style 2 is mainly for legacy code or older Airflow versions.
> 
> **Both are functionally equivalent** but Style 1 is cleaner and less error-prone.

**Q28.** If you run `airflow dags list`, what information would you expect to see?

> [!success]- Answer
> **Expected Output:**
> The `airflow dags list` command displays all DAGs that Airflow recognizes.
> 
> **Information Shown:**
> - **DAG ID** - Unique identifier for each DAG
> - **Filepath** - Where the DAG file is located
> - **Owner** - Who owns/maintains the DAG
> - **Paused** - Whether the DAG is active or paused
> 
> **Example Output:**
> ```
> dag_id           | filepath                    | owner | paused
> -----------------|----------------------------|-------|--------
> etl_workflow     | /dags/etl_workflow.py      | jdoe  | False
> example-etl      | /dags/example_dag.py       | admin | True
> data_pipeline    | /dags/data_pipeline.py     | team  | False
> ```
> 
> **Use Cases:**
> - **Verify DAG recognition** - Check if Airflow found your DAG file
> - **Debugging** - See if your new DAG is loaded
> - **Overview** - Get quick list of all available workflows
> - **Status check** - See which DAGs are paused
> 
> **Common Issue:** If your DAG doesn't appear, check:
> - Is the file in the correct DAGs folder?
> - Does the Python file have syntax errors?
> - Is the DAG properly defined?
> 
> **Related Commands:**
> - `airflow dags list-runs` - Show DAG execution history
> - `airflow dags show <dag_id>` - Display specific DAG structure

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (20 points)
1. B | 2. C | 3. B | 4. B | 5. B | 6. C | 7. B | 8. C | 9. D | 10. B

### Section B: True/False (10 points)
11. T | 12. F | 13. F | 14. F | 15. T | 16. T | 17. T | 18. F | 19. T | 20. T

### Section C: Short Answer (12 points)
See detailed answers above - award full points for comprehensive responses

### Section D: Command Analysis (8 points)
See detailed answers above - award points for correct interpretation and explanation

---

## Quick Study Guide

### Essential Concepts

**Data Engineering:**
- Reliable, Repeatable, Maintainable

**DAG:**
- **D**irected (has flow/dependencies)
- **A**cyclic (no loops)
- **G**raph (set of components)

**Airflow Capabilities:**
- Create workflows
- Schedule execution
- Monitor status

### Key Commands

```bash
# List DAGs
airflow dags list

# Test a task
airflow tasks test <dag_id> <task_id> <date>

# Get help
airflow -h
```

### DAG Definition Template

```python
from airflow import DAG
from datetime import datetime

default_args = {
    'owner': 'name',
    'start_date': datetime(2024, 1, 1)
}

with DAG('dag_name', default_args=default_args) as dag:
    # Define tasks here
    pass
```

---

## Key Concepts Summary

| Concept | Definition |
|---------|------------|
| **Data Engineering** | Reliable, repeatable, maintainable data processes |
| **Workflow** | Set of steps for a data task |
| **Airflow** | Platform for workflow creation, scheduling, monitoring |
| **DAG** | Directed Acyclic Graph - workflow structure |
| **Directed** | Has flow/dependencies |
| **Acyclic** | No loops |
| **Task** | Individual work unit |
| **Operator** | Defines task action |

---

## Common Mistakes to Avoid

❌ **Don't:**
- Create circular dependencies in DAGs
- Forget to set start_date
- Use old DAG definition style in Airflow 2.x+
- Confuse DAG ID with task ID
- Forget to import required modules

✅ **Do:**
- Use context manager (`with DAG...`) in Airflow 2.x+
- Always set default_args including start_date
- Give DAGs descriptive, unique IDs
- Test tasks before deploying
- Use Web UI for monitoring

---

## Study Strategy (3-Day Plan)

### Day 1: Concepts
- Review data engineering definition
- Understand workflow concept
- Learn DAG terminology (Directed, Acyclic, Graph)
- Study Airflow capabilities

### Day 2: Implementation
- Practice DAG definition syntax
- Learn command line basics
- Understand default_args
- Explore Web UI features

### Day 3: Practice
- Write sample DAG definitions
- Practice commands
- Compare command line vs Web UI
- Take practice quiz

---

## Before the Exam - Final Checklist

- [ ] Know data engineering definition (reliable, repeatable, maintainable)
- [ ] Understand what a workflow is
- [ ] Know what Airflow does (create, schedule, monitor)
- [ ] Understand DAG acronym and meaning
- [ ] Know DAG must be acyclic (no loops)
- [ ] Remember workflows written in Python
- [ ] Know command: `airflow dags list`
- [ ] Know command: `airflow tasks test`
- [ ] Understand context manager syntax for DAGs
- [ ] Know difference between command line and Python use
- [ ] Understand Web UI capabilities

---

#apache-airflow #data-engineering #dags #workflows #python #orchestration #etl #datacamp #chapter1 #practice-quiz

