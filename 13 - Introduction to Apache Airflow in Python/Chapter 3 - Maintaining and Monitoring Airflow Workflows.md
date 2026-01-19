## Sensors

### What is a Sensor?

**Definition:** An operator that waits for a certain condition to be true

### Sensor Characteristics:

**Sensors wait for conditions like:**
- Creation of a file
- Upload of a database record
- Certain response from a web request

**Key Features:**
- Can define **how often** to check for the condition
- Are **assigned to tasks** (like operators)
- Derived from `airflow.sensors.base_sensor_operator`

---

## Sensor Arguments

### Core Parameters

| Argument | Description | Options |
|----------|-------------|---------|
| **`mode`** | How to check for condition | `'poke'` or `'reschedule'` |
| **`poke_interval`** | How often to wait between checks | Seconds (integer) |
| **`timeout`** | How long to wait before failing | Seconds (integer) |

### Mode Details

**`mode='poke'` (Default):**
- Run repeatedly
- Keeps checking continuously
- Holds the task slot

**`mode='reschedule'`:**
- Give up task slot and try again later
- Frees resources between checks
- More efficient for long waits

### Additional Features

- Also includes **normal operator attributes** (task_id, dag, etc.)

---

## FileSensor

**Purpose:** Checks for the existence of a file at a certain location

### Characteristics:

- Part of the `airflow.sensors.filesystem` library
- Checks for file existence
- Can check if any files exist within a directory

### Example

```python
from airflow.sensors.filesystem import FileSensor

file_sensor_task = FileSensor(
    task_id='file_sense',
    filepath='salesdata.csv',
    poke_interval=300,  # Check every 300 seconds (5 minutes)
    dag=sales_report_dag
)

# Usage in workflow
init_sales_cleanup >> file_sensor_task >> generate_report
```

**Flow:**
1. `init_sales_cleanup` - Prepare for file
2. `file_sensor_task` - Wait for file to appear
3. `generate_report` - Process the file

---

## Other Sensors

Airflow provides many sensor types:

| Sensor | Purpose | Library |
|--------|---------|---------|
| **FileSensor** | Wait for file existence | `airflow.sensors.filesystem` |
| **ExternalTaskSensor** | Wait for task in another DAG | `airflow.sensors` |
| **HttpSensor** | Request web URL, check content | `airflow.sensors` |
| **SqlSensor** | Run SQL query, check for content | `airflow.sensors` |
| **Others** | Various conditions | `airflow.sensors` and `airflow.providers.*.sensors` |

### ExternalTaskSensor

Waits for a task in another DAG to complete
- Useful for DAG dependencies
- Cross-workflow coordination

### HttpSensor

Requests a web URL and checks for content
- Monitor API availability
- Wait for external service

### SqlSensor

Runs a SQL query to check for content
- Wait for database records
- Monitor data availability

---

## Why Use Sensors?

### Use a sensor when:

1. **Uncertain timing**
   - Don't know when condition will be true
   - File upload time varies
   - External process duration unknown

2. **Failure not immediately desired**
   - Want to wait rather than fail immediately
   - Graceful handling of delays

3. **Task repetition without loops**
   - Add checking logic without while loops
   - Built-in retry mechanism

### Benefits:

- ✅ Declarative waiting logic
- ✅ Resource-efficient (especially with reschedule mode)
- ✅ Built-in timeout handling
- ✅ Logging and monitoring

---

## Executors

### What is an Executor?

**Executors run tasks**

Different executors handle running tasks differently based on:
- System resources
- Parallelism requirements
- Infrastructure setup

---

## Executor Types

### 1. SequentialExecutor

**Characteristics:**
- **Default** Airflow executor
- Runs **one task at a time**
- Useful for **debugging**
- **Not recommended for production**

**Use cases:**
- Development
- Testing
- Learning Airflow

### 2. LocalExecutor

**Characteristics:**
- Runs on a **single system**
- Treats tasks as **processes**
- **Parallelism defined by user**
- Can utilize **all resources** of host system

**Use cases:**
- Small to medium workloads
- Single-server deployments
- Development with parallelism

**Advantages:**
- Better performance than Sequential
- Easier setup than Kubernetes
- Good for moderate scale

### 3. KubernetesExecutor

**Characteristics:**
- Uses **Kubernetes** as task manager
- **Multiple worker systems** can be defined
- **Significantly more difficult** to setup & configure
- **Extremely powerful** for organizations with extensive workflows

**Use cases:**
- Large-scale production
- Dynamic resource allocation
- Enterprise environments

**Advantages:**
- Highly scalable
- Resource isolation
- Auto-scaling capabilities

---

## Determining Your Executor

### Method 1: Check airflow.cfg File

```bash
# Look for the executor= line in airflow.cfg
executor = SequentialExecutor
```

### Method 2: Use airflow Command

```bash
# Run airflow info command
airflow info

# Output shows:
executor | SequentialExecutor
```

**Location in output:**
- First few lines of `airflow info`
- Shows current executor configuration

---

## Debugging and Troubleshooting

### Common Issues

1. **DAG won't run on schedule**
2. **DAG won't load**
3. **Syntax errors**

---

## Issue 1: DAG Won't Run on Schedule

### Possible Causes and Solutions

#### Cause 1: Scheduler Not Running

**Problem:** Scheduler process stopped

**Solution:**
```bash
# Start the scheduler from command line
airflow scheduler
```

#### Cause 2: Schedule Interval Not Passed

**Problem:** At least one `schedule_interval` hasn't passed yet

**Solution:**
- Check `start_date` and `schedule_interval`
- Remember: First run is start_date + schedule_interval
- Modify attributes to meet requirements

#### Cause 3: Not Enough Free Tasks

**Problem:** Executor at capacity, no free task slots

**Solutions:**
- **Change executor type** (e.g., Sequential → Local)
- **Add system resources** (CPU, memory)
- **Add more systems** (distributed setup)
- **Change DAG scheduling** (reduce concurrent tasks)

---

## Issue 2: DAG Won't Load

### Symptoms:

- DAG not in Web UI
- DAG not in `airflow dags list`

### Possible Solutions:

#### Verify DAG File Location

**Check:**
1. Is DAG file in correct folder?
2. Find DAGs folder location

**Determine DAGs folder:**
```bash
# Check airflow.cfg file
# Look for: dags_folder = /path/to/dags
```

**Requirements:**
- Folder must be an **absolute path**
- File must have `.py` extension
- File must be readable by Airflow

---

## Issue 3: Syntax Errors

### Most Common Reason for DAG Not Appearing

**Problem:** Python syntax errors in DAG file

### Detection Methods

#### Method 1: Check Import Errors

```bash
airflow dags list-import-errors
```

**Shows:**
- Which DAG files have errors
- Error messages and tracebacks
- Line numbers of issues

#### Method 2: Run Python Interpreter

```bash
python3 <dagfile.py>
```

**Results:**

**With errors:**
- Shows Python traceback
- Indicates line with error
- Describes the problem

**Without errors:**
- No output (or expected output)
- File runs successfully
- DAG is syntactically correct

### Common Syntax Errors:

- Missing imports
- Indentation errors
- Unclosed parentheses/brackets
- Invalid Python syntax
- Undefined variables

---

## SLAs (Service Level Agreements)

### What is an SLA?

**SLA** = Service Level Agreement

**In Airflow:**
- The **amount of time** a task or DAG should require to run
- Sets expected completion time

### SLA Miss

**Definition:** Any time the task/DAG does not meet the expected timing

**When SLA is Missed:**
- Email is sent out
- Log is stored
- Can view in Web UI under "Browse: SLA Misses"

---

## Defining SLAs

### Method 1: On Individual Task

```python
from datetime import timedelta

task1 = BashOperator(
    task_id='sla_task',
    bash_command='runcode.sh',
    sla=timedelta(seconds=30),  # Must complete within 30 seconds
    dag=dag
)
```

### Method 2: In default_args (Applies to All Tasks)

```python
from datetime import datetime, timedelta

default_args = {
    'sla': timedelta(minutes=20),  # All tasks must complete within 20 minutes
    'start_date': datetime(2023, 2, 20)
}

dag = DAG('sla_dag', default_args=default_args)
```

---

## timedelta Object

**Purpose:** Represents a duration of time

### Import

```python
from datetime import timedelta
```

### Arguments

Available parameters:
- `days`
- `seconds`
- `minutes`
- `hours`
- `weeks`

### Examples

```python
# 30 seconds
timedelta(seconds=30)

# 2 weeks
timedelta(weeks=2)

# Complex duration
timedelta(days=4, hours=10, minutes=20, seconds=30)

# 20 minutes
timedelta(minutes=20)

# 1 hour
timedelta(hours=1)
```

---

## General Reporting

### Email Notifications

Configure in `default_args` dictionary:

```python
default_args = {
    'email': ['airflowalerts@datacamp.com'],
    'email_on_failure': True,   # Send email when task fails
    'email_on_retry': False,    # Don't send on retry
    'email_on_success': True,   # Send email on success
    # ... other args
}
```

### Reporting Options

| Key | Purpose | Values |
|-----|---------|--------|
| `email` | Recipient email(s) | List of strings |
| `email_on_failure` | Email when task fails | Boolean |
| `email_on_retry` | Email on retry attempt | Boolean |
| `email_on_success` | Email on successful completion | Boolean |

### Using EmailOperator

Alternative to default_args:
- Use `EmailOperator` within DAGs
- Send custom emails at specific points
- More control over email content

```python
from airflow.operators.email import EmailOperator

notify = EmailOperator(
    task_id='send_notification',
    to='team@example.com',
    subject='DAG Completed',
    html_content='The workflow finished successfully'
)
```

---

## Quick Reference

### Sensors

```python
from airflow.sensors.filesystem import FileSensor

sensor = FileSensor(
    task_id='wait_for_file',
    filepath='data.csv',
    poke_interval=60,        # Check every 60 seconds
    timeout=3600,            # Fail after 1 hour
    mode='poke'              # or 'reschedule'
)
```

### Executors

| Executor | Parallelism | Complexity | Production |
|----------|-------------|------------|------------|
| Sequential | None (1 task) | Simple | ❌ No |
| Local | User-defined | Medium | ✅ Small/Medium |
| Kubernetes | High | Complex | ✅ Large |

### Debugging Commands

```bash
# Check for import errors
airflow dags list-import-errors

# Test Python syntax
python3 dagfile.py

# Start scheduler
airflow scheduler

# Get Airflow info
airflow info
```

### SLA Definition

```python
from datetime import timedelta

# Individual task
task = BashOperator(
    task_id='task',
    bash_command='cmd',
    sla=timedelta(minutes=10)
)

# All tasks
default_args = {
    'sla': timedelta(minutes=20)
}
```

---

## Key Concepts Summary

| Concept | Definition |
|---------|------------|
| **Sensor** | Operator that waits for condition |
| **FileSensor** | Waits for file existence |
| **poke_interval** | Time between sensor checks |
| **mode** | How sensor checks (poke/reschedule) |
| **Executor** | Runs tasks |
| **SequentialExecutor** | One task at a time (default) |
| **LocalExecutor** | Parallel on single system |
| **KubernetesExecutor** | Distributed on Kubernetes |
| **SLA** | Service Level Agreement - expected duration |
| **SLA Miss** | Task exceeds expected time |
| **timedelta** | Time duration object |

---

## Troubleshooting Checklist

### DAG Won't Run
- [ ] Is scheduler running? (`airflow scheduler`)
- [ ] Has schedule_interval passed?
- [ ] Are there free task slots?

### DAG Won't Load
- [ ] Is file in correct DAGs folder?
- [ ] Check `airflow.cfg` for dags_folder path
- [ ] Is path absolute?

### Syntax Errors
- [ ] Run `airflow dags list-import-errors`
- [ ] Run `python3 dagfile.py`
- [ ] Check Python syntax
- [ ] Verify all imports

### SLA Issues
- [ ] Is SLA defined correctly?
- [ ] Check Web UI: Browse > SLA Misses
- [ ] Verify email configuration
- [ ] Check task execution time

---

#apache-airflow #sensors #executors #debugging #troubleshooting #sla #monitoring #filesensor #timedelta #reporting #datacamp #chapter3


# Chapter 3: Maintaining and Monitoring Airflow - Practice Quiz

**Total Points: 70 points**
**Passing Score: 49 points (70%)**

---

## Section A: Multiple Choice Questions (2 points each = 30 points)

### Sensors

**Q1.** What is a sensor in Airflow?

- [ ] A) A monitoring tool for system resources
- [ ] B) An operator that waits for a certain condition to be true
- [ ] C) A type of executor
- [ ] D) A scheduling configuration

> [!success]- Answer
> **Correct Answer: B) An operator that waits for a certain condition to be true**
> 
> Sensors are special operators that wait for conditions like file creation, database records, or web responses before proceeding.

**Q2.** What are the two `mode` options for sensors?

- [ ] A) `check` and `wait`
- [ ] B) `poke` and `reschedule`
- [ ] C) `poll` and `defer`
- [ ] D) `active` and `passive`

> [!success]- Answer
> **Correct Answer: B) `poke` and `reschedule`**
> 
> - `mode='poke'`: Run repeatedly (default)
> - `mode='reschedule'`: Give up task slot and try again later

**Q3.** What does `poke_interval` control in a sensor?

- [ ] A) How long before timeout
- [ ] B) How often to wait between checks
- [ ] C) The mode of operation
- [ ] D) The file path to check

> [!success]- Answer
> **Correct Answer: B) How often to wait between checks**
> 
> `poke_interval` specifies the time in seconds between sensor checks. For example, `poke_interval=300` checks every 5 minutes.

**Q4.** What does FileSensor check for?

- [ ] A) File size
- [ ] B) File permissions
- [ ] C) File existence at a location
- [ ] D) File content

> [!success]- Answer
> **Correct Answer: C) File existence at a location**
> 
> FileSensor waits for a file to exist at a specified location. It can also check if any files exist within a directory.

**Q5.** Which sensor would you use to wait for a task in another DAG to complete?

- [ ] A) FileSensor
- [ ] B) HttpSensor
- [ ] C) ExternalTaskSensor
- [ ] D) SqlSensor

> [!success]- Answer
> **Correct Answer: C) ExternalTaskSensor**
> 
> ExternalTaskSensor waits for a task in another DAG to complete, enabling cross-DAG dependencies.

### Executors

**Q6.** What is an executor in Airflow?

- [ ] A) A type of operator
- [ ] B) Something that runs tasks
- [ ] C) A scheduling algorithm
- [ ] D) A monitoring tool

> [!success]- Answer
> **Correct Answer: B) Something that runs tasks**
> 
> Executors are responsible for running tasks. Different executors handle task execution differently.

**Q7.** What is the default Airflow executor?

- [ ] A) LocalExecutor
- [ ] B) KubernetesExecutor
- [ ] C) SequentialExecutor
- [ ] D) CeleryExecutor

> [!success]- Answer
> **Correct Answer: C) SequentialExecutor**
> 
> SequentialExecutor is the default. It runs one task at a time and is useful for debugging but not recommended for production.

**Q8.** Which executor runs one task at a time?

- [ ] A) LocalExecutor
- [ ] B) SequentialExecutor
- [ ] C) KubernetesExecutor
- [ ] D) All executors

> [!success]- Answer
> **Correct Answer: B) SequentialExecutor**
> 
> SequentialExecutor runs tasks sequentially (one at a time) with no parallelism.

**Q9.** Which executor can utilize all resources of a single host system?

- [ ] A) SequentialExecutor
- [ ] B) LocalExecutor
- [ ] C) KubernetesExecutor
- [ ] D) None of these

> [!success]- Answer
> **Correct Answer: B) LocalExecutor**
> 
> LocalExecutor runs on a single system, treats tasks as processes, and can utilize all resources of the host with user-defined parallelism.

**Q10.** Which executor uses Kubernetes as a task manager?

- [ ] A) LocalExecutor
- [ ] B) SequentialExecutor
- [ ] C) KubernetesExecutor
- [ ] D) ClusterExecutor

> [!success]- Answer
> **Correct Answer: C) KubernetesExecutor**
> 
> KubernetesExecutor uses Kubernetes for task management, supports multiple worker systems, and is extremely powerful but complex to setup.

### Debugging and SLAs

**Q11.** What command checks for DAG import errors?

- [ ] A) `airflow dags check`
- [ ] B) `airflow dags list-import-errors`
- [ ] C) `airflow errors list`
- [ ] D) `airflow debug`

> [!success]- Answer
> **Correct Answer: B) `airflow dags list-import-errors`**
> 
> This command shows which DAG files have errors, with error messages and tracebacks.

**Q12.** What does SLA stand for?

- [ ] A) System Level Agreement
- [ ] B) Service Level Agreement
- [ ] C) Standard Log Analysis
- [ ] D) Scheduled Load Allocation

> [!success]- Answer
> **Correct Answer: B) Service Level Agreement**
> 
> SLA is the amount of time a task or DAG should require to run. An SLA Miss occurs when this time is exceeded.

**Q13.** What happens when an SLA is missed?

- [ ] A) The task is cancelled
- [ ] B) An email is sent and a log is stored
- [ ] C) The DAG stops running
- [ ] D) Nothing happens

> [!success]- Answer
> **Correct Answer: B) An email is sent and a log is stored**
> 
> When an SLA is missed, Airflow sends an email notification and stores a log. You can view SLA misses in the Web UI.

**Q14.** Which Python object is used to define time durations for SLAs?

- [ ] A) `datetime`
- [ ] B) `time`
- [ ] C) `timedelta`
- [ ] D) `duration`

> [!success]- Answer
> **Correct Answer: C) `timedelta`**
> 
> `timedelta` from the datetime library is used to specify durations like `timedelta(minutes=20)` or `timedelta(seconds=30)`.

**Q15.** If a DAG won't run on schedule, what should you check first?

- [ ] A) File permissions
- [ ] B) If the scheduler is running
- [ ] C) Database connection
- [ ] D) Python version

> [!success]- Answer
> **Correct Answer: B) If the scheduler is running**
> 
> The most common reason a DAG won't run on schedule is that the scheduler process is not running. Fix by running `airflow scheduler`.

---

## Section B: True/False Questions (1 point each = 15 points)

**Q16.** Sensors are operators that wait for a certain condition to be true. **T/F**

> [!success]- Answer
> **True** - Sensors are special operators designed to wait for specific conditions before proceeding.

**Q17.** The default mode for sensors is `reschedule`. **T/F**

> [!success]- Answer
> **False** - The default mode is `poke`, not `reschedule`.

**Q18.** FileSensor checks for the existence of a file at a certain location. **T/F**

> [!success]- Answer
> **True** - FileSensor waits for a file to exist at a specified filepath.

**Q19.** SqlSensor runs a SQL query to check for content. **T/F**

> [!success]- Answer
> **True** - SqlSensor executes SQL queries to check if certain data exists or conditions are met.

**Q20.** SequentialExecutor is recommended for production use. **T/F**

> [!success]- Answer
> **False** - SequentialExecutor runs one task at a time and is NOT recommended for production, only for debugging.

**Q21.** LocalExecutor can run tasks in parallel on a single system. **T/F**

> [!success]- Answer
> **True** - LocalExecutor treats tasks as processes and supports parallelism defined by the user.

**Q22.** KubernetesExecutor is the easiest executor to setup and configure. **T/F**

> [!success]- Answer
> **False** - KubernetesExecutor is significantly more difficult to setup and configure, though it's extremely powerful.

**Q23.** You can determine your current executor by checking the `airflow.cfg` file. **T/F**

> [!success]- Answer
> **True** - The executor is specified in the `airflow.cfg` file with the `executor=` line.

**Q24.** The most common reason a DAG won't appear is syntax errors in the DAG file. **T/F**

> [!success]- Answer
> **True** - Syntax errors are the most common reason for DAGs not appearing in the UI or DAG list.

**Q25.** Running `python3 <dagfile.py>` can help identify syntax errors. **T/F**

> [!success]- Answer
> **True** - Running the DAG file directly with Python will show syntax errors and tracebacks.

**Q26.** An SLA defines the maximum time a task should take to complete. **T/F**

> [!success]- Answer
> **True** - SLA (Service Level Agreement) specifies the expected completion time for tasks or DAGs.

**Q27.** SLA misses can be viewed in the Airflow Web UI under "Browse: SLA Misses". **T/F**

> [!success]- Answer
> **True** - The Web UI has a dedicated section for viewing SLA misses.

**Q28.** `timedelta` is part of the datetime library. **T/F**

> [!success]- Answer
> **True** - `timedelta` is imported from the datetime library: `from datetime import timedelta`.

**Q29.** The `email_on_failure` option sends an email when a task fails. **T/F**

> [!success]- Answer
> **True** - Setting `email_on_failure: True` in default_args enables email notifications on task failures.

**Q30.** Sensors should be used when you want to add task repetition without using loops. **T/F**

> [!success]- Answer
> **True** - Sensors provide a way to add repetitive checking logic without explicit while loops.

---

## Section C: Short Answer Questions (3 points each = 15 points)

**Q31.** Explain the difference between `mode='poke'` and `mode='reschedule'` in sensors. When would you use each?

> [!success]- Answer
> **Two Sensor Modes:**
> 
> **`mode='poke'` (Default):**
> - **Behavior:** Run repeatedly without releasing task slot
> - **How it works:** Sensor keeps checking at intervals while holding resources
> - **Task slot:** Remains occupied during entire wait period
> - **Resource usage:** Higher - continuously uses a worker slot
> 
> **When to use poke:**
> - Short wait times (minutes)
> - Condition expected soon
> - Have available task slots
> - Don't mind resource consumption
> 
> **`mode='reschedule'`:**
> - **Behavior:** Give up task slot and try again later
> - **How it works:** Sensor checks, releases slot, gets rescheduled later
> - **Task slot:** Released between checks, frees up resources
> - **Resource usage:** Lower - only uses slot when actively checking
> 
> **When to use reschedule:**
> - Long wait times (hours/days)
> - Condition uncertain
> - Limited task slots available
> - Need to conserve resources
> 
> **Example Comparison:**
> ```python
> # Short wait - use poke
> quick_sensor = FileSensor(
>     task_id='wait_for_file',
>     filepath='data.csv',
>     poke_interval=30,      # Check every 30 seconds
>     mode='poke',           # Keep checking
>     timeout=300            # 5 minute timeout
> )
> 
> # Long wait - use reschedule
> long_sensor = FileSensor(
>     task_id='wait_for_daily_file',
>     filepath='daily_report.csv',
>     poke_interval=3600,    # Check every hour
>     mode='reschedule',     # Free up slot between checks
>     timeout=86400          # 24 hour timeout
> )
> ```
> 
> **Best Practice:** Use `reschedule` for long waits to avoid blocking precious task slots.

**Q32.** Describe the three executor types covered and explain which would be appropriate for different production scenarios.

> [!success]- Answer
> **Three Executor Types:**
> 
> **1. SequentialExecutor**
> 
> **Characteristics:**
> - Runs one task at a time
> - No parallelism
> - Default Airflow executor
> - Simple configuration
> 
> **Appropriate for:**
> - ❌ **NOT for production**
> - ✅ Development and testing
> - ✅ Debugging workflows
> - ✅ Learning Airflow
> - ✅ Single-task workflows
> 
> **Scenario:** Developer testing new DAG logic on laptop
> 
> ---
> 
> **2. LocalExecutor**
> 
> **Characteristics:**
> - Runs on single system
> - Tasks as processes
> - User-defined parallelism
> - Utilizes all host resources
> 
> **Appropriate for:**
> - ✅ **Small to medium production workloads**
> - Single-server deployments
> - Moderate task concurrency
> - Limited infrastructure
> 
> **Scenario:** Startup company with 20-30 daily DAGs running on one powerful server
> 
> **Configuration Example:**
> ```python
> # Can run multiple tasks simultaneously
> # Limited by server CPU/memory
> parallelism = 16  # Run up to 16 tasks at once
> ```
> 
> ---
> 
> **3. KubernetesExecutor**
> 
> **Characteristics:**
> - Uses Kubernetes cluster
> - Multiple worker systems
> - Highly scalable
> - Complex setup
> - Dynamic resource allocation
> 
> **Appropriate for:**
> - ✅ **Large-scale production**
> - Enterprise environments
> - Hundreds/thousands of DAGs
> - Variable workload demands
> - Need resource isolation
> 
> **Scenario:** Large enterprise with 500+ DAGs, peak loads requiring 100+ concurrent tasks, needing auto-scaling
> 
> ---
> 
> **Decision Matrix:**
> 
> | Scale | DAG Count | Recommendation |
> |-------|-----------|----------------|
> | Learning | 1-5 | Sequential |
> | Small Prod | 10-50 | Local |
> | Medium Prod | 50-200 | Local (powerful) |
> | Large Prod | 200+ | Kubernetes |
> 
> **Summary:** Start with SequentialExecutor for learning, use LocalExecutor for small/medium production, scale to KubernetesExecutor for enterprise needs.

**Q33.** List three common issues when a DAG won't run on schedule and provide solutions for each.

> [!success]- Answer
> **Three Common Issues and Solutions:**
> 
> **Issue 1: Scheduler Not Running**
> 
> **Problem:**
> - Scheduler process has stopped
> - No process monitoring scheduled DAGs
> - DAGs sit idle waiting to run
> 
> **Solution:**
> ```bash
> # Start the scheduler from command line
> airflow scheduler
> ```
> 
> **Prevention:**
> - Use process manager (systemd, supervisor)
> - Set up monitoring/alerts
> - Auto-restart on failure
> 
> ---
> 
> **Issue 2: Schedule Interval Not Passed**
> 
> **Problem:**
> - At least one schedule_interval hasn't passed yet
> - Airflow runs at start_date + schedule_interval
> - First run is delayed
> 
> **Example:**
> ```python
> 'start_date': datetime(2024, 1, 10, 12, 0),
> 'schedule_interval': '@daily'
> # First run: January 11, 2024 at 12:00 (not January 10!)
> ```
> 
> **Solution:**
> - Adjust start_date to earlier time
> - Wait for interval to pass
> - Manually trigger if needed
> - Verify schedule_interval matches requirements
> 
> **Check:**
> ```python
> # Make sure start_date is in the past
> # And enough time has passed for interval
> ```
> 
> ---
> 
> **Issue 3: Not Enough Free Task Slots**
> 
> **Problem:**
> - Executor at capacity
> - All task slots occupied
> - New tasks queued, waiting for slots
> - DAG can't start because no resources
> 
> **Solutions:**
> 
> **Option A: Change Executor Type**
> ```bash
> # Switch from Sequential to Local for parallelism
> # Edit airflow.cfg:
> executor = LocalExecutor
> ```
> 
> **Option B: Add System Resources**
> - Increase CPU count
> - Add more RAM
> - Upgrade server capacity
> 
> **Option C: Add More Systems**
> - Distribute workload
> - Use KubernetesExecutor
> - Scale horizontally
> 
> **Option D: Optimize DAG Scheduling**
> ```python
> # Reduce max_active_runs
> dag = DAG(
>     'my_dag',
>     max_active_runs=2,  # Limit concurrent DAG runs
>     concurrency=5       # Limit concurrent tasks per DAG
> )
> ```
> 
> **Monitoring:**
> - Check Web UI for queue sizes
> - Monitor task slot usage
> - Review executor configuration
> 
> ---
> 
> **Troubleshooting Workflow:**
> 1. Check scheduler status first
> 2. Verify timing and schedule
> 3. Assess resource availability
> 4. Scale appropriately

**Q34.** Explain what an SLA is in Airflow, how to define one, and what happens when an SLA is missed.

> [!success]- Answer
> **SLA (Service Level Agreement) in Airflow:**
> 
> **Definition:**
> - The **expected time** a task or DAG should take to complete
> - Sets performance expectations
> - Helps monitor workflow health
> - Ensures timely data delivery
> 
> **Purpose:**
> - Track task performance
> - Identify slow tasks
> - Alert on delays
> - Maintain service quality
> 
> ---
> 
> **How to Define SLAs:**
> 
> **Method 1: On Individual Task**
> ```python
> from datetime import timedelta
> from airflow.operators.bash import BashOperator
> 
> task1 = BashOperator(
>     task_id='sla_task',
>     bash_command='runcode.sh',
>     sla=timedelta(seconds=30),  # Must complete within 30 seconds
>     dag=dag
> )
> ```
> 
> **Method 2: In default_args (All Tasks)**
> ```python
> from datetime import datetime, timedelta
> 
> default_args = {
>     'sla': timedelta(minutes=20),      # All tasks: 20 min limit
>     'start_date': datetime(2023, 2, 20),
>     'email': ['alerts@company.com']
> }
> 
> dag = DAG('sla_dag', default_args=default_args)
> ```
> 
> **Using timedelta:**
> ```python
> # Various duration formats
> timedelta(seconds=30)                    # 30 seconds
> timedelta(minutes=20)                    # 20 minutes
> timedelta(hours=2)                       # 2 hours
> timedelta(days=1, hours=6, minutes=30)  # Complex duration
> ```
> 
> ---
> 
> **What Happens When SLA is Missed:**
> 
> **Automatic Actions:**
> 1. **Email Notification**
>    - Sent to configured email addresses
>    - Includes task details
>    - Shows expected vs actual time
> 
> 2. **Log Storage**
>    - Miss recorded in database
>    - Available for reporting
>    - Historical tracking
> 
> 3. **Web UI Display**
>    - Visible in "Browse > SLA Misses"
>    - Shows all recent misses
>    - Filterable and searchable
> 
> **What Does NOT Happen:**
> - ❌ Task is NOT stopped
> - ❌ DAG is NOT cancelled
> - ❌ Execution is NOT interrupted
> - Task continues running to completion
> 
> **Viewing SLA Misses:**
> ```
> Web UI Navigation:
> Browse → SLA Misses
> 
> Shows:
> - DAG ID
> - Task ID
> - Expected time
> - Actual time
> - Timestamp
> ```
> 
> ---
> 
> **Complete Example:**
> ```python
> from airflow import DAG
> from airflow.operators.bash import BashOperator
> from datetime import datetime, timedelta
> 
> default_args = {
>     'owner': 'data_team',
>     'start_date': datetime(2024, 1, 1),
>     'email': ['team@company.com'],
>     'email_on_failure': True,
>     'sla': timedelta(minutes=10)  # Default: 10 min for all tasks
> }
> 
> with DAG('etl_with_sla', default_args=default_args) as dag:
>     
>     # This task uses default SLA (10 min)
>     extract = BashOperator(
>         task_id='extract',
>         bash_command='extract.sh'
>     )
>     
>     # This task has custom SLA (30 min)
>     transform = BashOperator(
>         task_id='transform',
>         bash_command='transform.sh',
>         sla=timedelta(minutes=30)  # Override default
>     )
> ```
> 
> **Best Practices:**
> - Set realistic SLAs based on historical data
> - Monitor SLA misses regularly
> - Investigate patterns of misses
> - Adjust SLAs as workflows evolve
> - Use for critical production workflows

**Q35.** Describe three ways to debug and troubleshoot why a DAG won't load in Airflow.

> [!success]- Answer
> **Three Debugging Methods:**
> 
> **Method 1: Check DAG Import Errors**
> 
> **Command:**
> ```bash
> airflow dags list-import-errors
> ```
> 
> **What it shows:**
> - List of DAG files with errors
> - Python tracebacks
> - Error messages
> - Line numbers where errors occur
> 
> **Example Output:**
> ```
> filepath                    | error
> ---------------------------|------------------
> /dags/my_dag.py            | ImportError: No module named 'pandas'
> /dags/broken_dag.py        | SyntaxError: invalid syntax (line 15)
> ```
> 
> **How to use:**
> 1. Run command
> 2. Identify which DAG has errors
> 3. Read error message
> 4. Fix the specific issue
> 5. Verify with `airflow dags list`
> 
> **Common errors found:**
> - Missing imports
> - Module not found
> - Syntax errors
> - Invalid DAG definitions
> 
> ---
> 
> **Method 2: Run Python Interpreter Directly**
> 
> **Command:**
> ```bash
> python3 <dagfile.py>
> ```
> 
> **What it does:**
> - Executes the DAG file as regular Python
> - Shows syntax errors immediately
> - Displays full tracebacks
> - Tests imports and logic
> 
> **Results:**
> 
> **With Errors:**
> ```bash
> $ python3 my_dag.py
> Traceback (most recent call last):
>   File "my_dag.py", line 5, in <module>
>     from airflow import DAG
> ImportError: No module named 'airflow'
> ```
> 
> **Without Errors:**
> ```bash
> $ python3 my_dag.py
> # No output or expected prints
> # File is syntactically correct
> ```
> 
> **Advantages:**
> - Quick feedback
> - Standard Python errors
> - No Airflow needed running
> - Easy to iterate and fix
> 
> **How to use:**
> 1. Navigate to DAG file location
> 2. Run `python3 filename.py`
> 3. Fix any errors shown
> 4. Re-run until no errors
> 5. Check Airflow UI
> 
> ---
> 
> **Method 3: Verify DAG File Location**
> 
> **Problem:** DAG file not in correct folder
> 
> **Steps to verify:**
> 
> **Step 1: Find DAGs folder**
> ```bash
> # Check airflow.cfg file
> cat airflow.cfg | grep dags_folder
> 
> # Or use airflow command
> airflow info
> ```
> 
> **Step 2: Check requirements**
> - ✅ Path must be **absolute** (not relative)
> - ✅ File must have **.py** extension
> - ✅ File must be **readable** by Airflow user
> - ✅ Folder must **exist** and be accessible
> 
> **Example from airflow.cfg:**
> ```ini
> [core]
> dags_folder = /opt/airflow/dags
> ```
> 
> **Step 3: Verify file location**
> ```bash
> # List files in DAGs folder
> ls -la /opt/airflow/dags/
> 
> # Check if your DAG is there
> ls /opt/airflow/dags/my_dag.py
> ```
> 
> **Step 4: Check permissions**
> ```bash
> # Verify file is readable
> chmod 644 /opt/airflow/dags/my_dag.py
> 
> # Verify ownership
> chown airflow:airflow /opt/airflow/dags/my_dag.py
> ```
> 
> **Common location issues:**
> - File in wrong directory
> - Relative path used instead of absolute
> - Permissions too restrictive
> - Symbolic links broken
> - Network mount not accessible
> 
> ---
> 
> **Complete Troubleshooting Workflow:**
> 
> ```bash
> # Step 1: Check if DAG appears
> airflow dags list | grep my_dag
> 
> # If not found:
> 
> # Step 2: Check for import errors
> airflow dags list-import-errors
> 
> # Step 3: Test Python syntax
> cd /path/to/dags
> python3 my_dag.py
> 
> # Step 4: Verify location
> cat $AIRFLOW_HOME/airflow.cfg | grep dags_folder
> ls -la <dags_folder>/my_dag.py
> 
> # Step 5: Check Airflow logs
> tail -f $AIRFLOW_HOME/logs/scheduler/*.log
> ```
> 
> **Prevention Tips:**
> - Use version control (Git)
> - Test locally before deploying
> - Use CI/CD for validation
> - Set up linting (flake8, pylint)
> - Create DAG template with tests

---

## Section D: Code Analysis (2 points each = 10 points)

**Q36.** Analyze this FileSensor configuration:
```python
file_sensor_task = FileSensor(
    task_id='file_sense',
    filepath='salesdata.csv',
    poke_interval=300,
    dag=sales_report_dag
)
```

> [!success]- Answer
> **Analysis:**
> 
> **What it does:**
> Creates a sensor task that waits for a file to exist before proceeding.
> 
> **Breaking down parameters:**
> 
> **1. `task_id='file_sense'`**
> - Unique identifier for this task
> - Used in logs and UI
> - Used in dependencies
> 
> **2. `filepath='salesdata.csv'`**
> - File to wait for
> - Relative or absolute path
> - Sensor checks if this file exists
> 
> **3. `poke_interval=300`**
> - Check every 300 seconds (5 minutes)
> - Wait time between checks
> - Balance between responsiveness and resource usage
> 
> **4. `dag=sales_report_dag`**
> - Associates with specific DAG
> - Legacy syntax (Airflow <2.0)
> - Not needed with `with DAG(...)` context manager
> 
> **What's missing (uses defaults):**
> - `mode` - Defaults to `'poke'`
> - `timeout` - No timeout set (waits indefinitely)
> - `fs_conn_id` - Uses default filesystem connection
> 
> **Execution behavior:**
> - Checks for `salesdata.csv` every 5 minutes
> - Continues checking until file appears
> - Holds task slot during checks (poke mode)
> - No timeout - waits forever if needed
> 
> **Use case:**
> Waiting for daily sales data file to arrive before generating report. Checks every 5 minutes until file appears.
> 
> **Improvements:**
> ```python
> file_sensor_task = FileSensor(
>     task_id='file_sense',
>     filepath='/absolute/path/salesdata.csv',  # Use absolute path
>     poke_interval=300,
>     timeout=3600,              # Add 1-hour timeout
>     mode='reschedule'          # Free up slot between checks
> )
> ```

**Q37.** What does this SLA configuration mean?
```python
default_args = {
    'sla': timedelta(minutes=20),
    'start_date': datetime(2023, 2, 20)
}
```

> [!success]- Answer
> **SLA Configuration Analysis:**
> 
> **What it means:**
> All tasks in DAGs using these default_args must complete within 20 minutes from their start time.
> 
> **Breaking down the configuration:**
> 
> **1. `'sla': timedelta(minutes=20)`**
> - Sets Service Level Agreement
> - Each task has 20-minute completion limit
> - Applies to ALL tasks in the DAG
> - Clock starts when task begins execution
> 
> **2. `'start_date': datetime(2023, 2, 20)`**
> - DAG starts scheduling from February 20, 2023
> - Unrelated to SLA (separate configuration)
> 
> **Behavior:**
> 
> **Timeline Example:**
> ```
> Task starts: 10:00:00
> SLA deadline: 10:20:00  (start + 20 minutes)
> 
> If task finishes at 10:15:00 ✅ Within SLA
> If task finishes at 10:25:00 ❌ SLA MISS
> ```
> 
> **When SLA is missed:**
> 1. Email notification sent (if email configured)
> 2. Log entry created
> 3. Visible in Web UI "Browse > SLA Misses"
> 4. Task continues running (not stopped)
> 
> **Usage in DAG:**
> ```python
> from airflow import DAG
> from airflow.operators.bash import BashOperator
> from datetime import datetime, timedelta
> 
> default_args = {
>     'sla': timedelta(minutes=20),
>     'start_date': datetime(2023, 2, 20),
>     'email': ['team@company.com'],
>     'email_on_failure': True
> }
> 
> with DAG('my_dag', default_args=default_args) as dag:
>     # All tasks inherit 20-minute SLA
>     task1 = BashOperator(task_id='task1', bash_command='cmd1')
>     task2 = BashOperator(task_id='task2', bash_command='cmd2')
> ```
> 
> **Impact:**
> - Helps monitor task performance
> - Alerts when tasks take too long
> - Identifies bottlenecks
> - Maintains service quality
> 
> **Best Practice:**
> Set SLA based on historical task execution times plus buffer (e.g., if task typically takes 10 minutes, set SLA to 15-20 minutes).

**Q38.** Compare these two debugging approaches:
```bash
# Approach 1
airflow dags list-import-errors

# Approach 2
python3 my_dag.py
```

> [!success]- Answer
> **Comparison of Debugging Approaches:**
> 
> **Approach 1: `airflow dags list-import-errors`**
> 
> **What it does:**
> - Airflow-specific command
> - Checks all DAG files in dags_folder
> - Shows import/loading errors
> - Displays Airflow-specific issues
> 
> **Output format:**
> ```
> filepath              | error
> ---------------------|------------------------
> /dags/broken_dag.py  | ImportError: No module
> /dags/bad_dag.py     | SyntaxError: line 23
> ```
> 
> **Advantages:**
> - ✅ Shows ALL DAGs with errors at once
> - ✅ Airflow context (knows about DAG structure)
> - ✅ Finds DAG-specific issues
> - ✅ Professional output format
> - ✅ Production-ready diagnostic
> 
> **Finds:**
> - Import errors
> - Missing dependencies
> - DAG loading issues
> - Airflow configuration problems
> 
> **When to use:**
> - Multiple DAGs to check
> - Need overview of all issues
> - Production environment
> - Don't know which DAG has problem
> 
> ---
> 
> **Approach 2: `python3 my_dag.py`**
> 
> **What it does:**
> - Runs DAG file as regular Python script
> - Tests basic Python syntax
> - Executes imports
> - Shows Python-level errors
> 
> **Output format:**
> ```
> Traceback (most recent call last):
>   File "my_dag.py", line 15
>     def my_function(
>                    ^
> SyntaxError: invalid syntax
> ```
> 
> **Advantages:**
> - ✅ Quick and simple
> - ✅ Standard Python error messages
> - ✅ Detailed traceback
> - ✅ No Airflow needed running
> - ✅ Fast iteration cycle
> 
> **Finds:**
> - Python syntax errors
> - Import problems
> - Indentation issues
> - Basic logic errors
> 
> **When to use:**
> - Single specific DAG
> - Quick syntax check
> - Development/testing
> - Know which file to check
> - Airflow not running
> 
> ---
> 
> **Comparison Table:**
> 
> | Aspect | `list-import-errors` | `python3 dag.py` |
> |--------|---------------------|------------------|
> | **Scope** | All DAGs | Single file |
> | **Context** | Airflow-aware | Plain Python |
> | **Speed** | Slower | Faster |
> | **Detail** | Airflow-specific | Python-standard |
> | **Requirements** | Airflow running | Python only |
> | **Best for** | Production | Development |
> 
> **Best Practice Workflow:**
> ```bash
> # Step 1: Quick check during development
> python3 my_dag.py
> 
> # Step 2: Verify in Airflow context
> airflow dags list-import-errors
> 
> # Step 3: Confirm DAG loaded
> airflow dags list | grep my_dag
> ```
> 
> **Example Scenario:**
> ```bash
> # Developer writes new DAG
> vim sales_dag.py
> 
> # Quick syntax check
> python3 sales_dag.py  # Fast feedback
> 
> # Deploy to Airflow
> cp sales_dag.py /opt/airflow/dags/
> 
> # Check Airflow can load it
> airflow dags list-import-errors  # Production verification
> ```
> 
> **Recommendation:** Use `python3` for rapid development testing, use `airflow dags list-import-errors` for production verification and comprehensive checks.

**Q39.** Explain what this sensor dependency pattern accomplishes:
```python
init_sales_cleanup >> file_sensor_task >> generate_report
```

> [!success]- Answer
> **Dependency Pattern Analysis:**
> 
> **What it accomplishes:**
> Creates a three-stage workflow with a waiting period in the middle.
> 
> **Flow Breakdown:**
> 
> **Stage 1: `init_sales_cleanup`**
> - **First task** to execute
> - Likely prepares environment
> - Examples:
>   - Clear old files
>   - Create directories
>   - Reset temporary tables
>   - Prepare for new data
> 
> **Stage 2: `file_sensor_task`** ⏰
> - **Waits** for condition (file existence)
> - Blocks until file appears
> - Acts as synchronization point
> - Bridge between preparation and processing
> 
> **Stage 3: `generate_report`**
> - **Final task** to execute
> - Processes the file
> - Examples:
>   - Read sales data
>   - Calculate metrics
>   - Create report
>   - Send to stakeholders
> 
> **Execution Timeline:**
> ```
> 1. init_sales_cleanup runs (completes in seconds/minutes)
>    ↓
> 2. file_sensor_task starts checking
>    - Check every poke_interval
>    - Wait for salesdata.csv
>    - Could wait hours
>    ↓
> 3. File appears!
>    ↓
> 4. generate_report runs (uses the file)
> ```
> 
> **Why This Pattern:**
> 
> **Problem it solves:**
> - External systems provide data at uncertain times
> - Can't predict when file will arrive
> - Need to process as soon as available
> - Want to prepare environment beforehand
> 
> **Real-World Scenario:**
> ```
> Daily Sales ETL Pipeline:
> 
> 1. init_sales_cleanup
>    - Delete yesterday's temp files
>    - Create fresh staging area
>    - Log start of process
> 
> 2. file_sensor_task
>    - Wait for: /data/sales/daily_sales_2024-01-15.csv
>    - External system uploads between 3-6 AM
>    - Don't know exact time
>    - Check every 5 minutes
> 
> 3. generate_report
>    - Load CSV into database
>    - Calculate daily totals
>    - Generate executive dashboard
>    - Email to management
> ```
> 
> **Complete Example:**
> ```python
> from airflow import DAG
> from airflow.operators.bash import BashOperator
> from airflow.sensors.filesystem import FileSensor
> from datetime import datetime, timedelta
> 
> default_args = {
>     'start_date': datetime(2024, 1, 1),
>     'schedule_interval': '@daily'
> }
> 
> with DAG('sales_etl', default_args=default_args) as dag:
>     
>     # Stage 1: Preparation
>     init_sales_cleanup = BashOperator(
>         task_id='init_sales_cleanup',
>         bash_command='''
>             rm -f /tmp/staging/*
>             mkdir -p /tmp/staging
>             echo "Environment ready"
>         '''
>     )
>     
>     # Stage 2: Wait for file
>     file_sensor_task = FileSensor(
>         task_id='file_sense',
>         filepath='/data/sales/salesdata.csv',
>         poke_interval=300,      # Check every 5 minutes
>         timeout=14400,          # Give up after 4 hours
>         mode='reschedule'       # Free up slot while waiting
>     )
>     
>     # Stage 3: Process file
>     generate_report = BashOperator(
>         task_id='generate_report',
>         bash_command='''
>             python /scripts/process_sales.py
>             python /scripts/create_dashboard.py
>             python /scripts/send_email.py
>         '''
>     )
>     
>     # Define flow
>     init_sales_cleanup >> file_sensor_task >> generate_report
> ```
> 
> **Key Benefits:**
> 4. **Guaranteed order** - Cleanup happens first
> 5. **Automatic waiting** - No manual intervention
> 6. **Immediate processing** - Starts as soon as file arrives
> 7. **Resilient** - Handles variable timing
> 8. **Observable** - Can monitor each stage
> 
> **Alternative Without Sensor:**
> ```python
> # BAD: Without sensor
> cleanup >> process_file
> 
> # Problem: If file doesn't exist, process_file fails immediately
> # No waiting, no retry logic
> ```
> 
> **Summary:** This pattern creates a robust, self-managing workflow that prepares, waits intelligently, and processes automatically - perfect for external data dependencies.

**Q40.** What configuration would you check if a DAG appears in `airflow dags list` but won't run?

> [!success]- Answer
> **Troubleshooting DAG That Won't Run:**
> 
> If DAG appears in list but won't run, check these configurations:
> 
> **1. Scheduler Status**
> 
> **Check if scheduler is running:**
> ```bash
> # Check process
> ps aux | grep "airflow scheduler"
> 
> # Or check Airflow health
> airflow info
> ```
> 
> **Fix:**
> ```bash
> # Start scheduler
> airflow scheduler
> 
> # Or use process manager
> systemctl start airflow-scheduler  # systemd
> supervisorctl start airflow-scheduler  # supervisor
> ```
> 
> **Why:** Scheduler is responsible for triggering DAG runs. Without it, no DAGs run regardless of schedule.
> 
> ---
> 
> **2. Start Date and Schedule Configuration**
> 
> **Check in DAG definition:**
> ```python
> # Verify these settings
> default_args = {
>     'start_date': datetime(2024, 1, 1),  # Must be in past
>     'schedule_interval': '@daily'         # Or other interval
> }
> ```
> 
> **Common issues:**
> - `start_date` in future
> - Not enough time passed (remember: start_date + schedule_interval)
> - No `schedule_interval` defined
> 
> **Check:**
> ```bash
> # View DAG details in UI
> # Or check from command line
> airflow dags show <dag_id>
> ```
> 
> **Fix:**
> ```python
> # Ensure start_date is in past
> 'start_date': datetime(2024, 1, 1),  # Not tomorrow!
> 
> # Ensure schedule_interval is set
> 'schedule_interval': '@daily',  # Or None for manual only
> ```
> 
> ---
> 
> **3. DAG Pause State**
> 
> **Check if DAG is paused:**
> - Look in Web UI - toggle switch next to DAG name
> - Paused DAGs don't run automatically
> 
> **Fix in Web UI:**
> - Click toggle to unpause
> - Should turn from red/gray to green
> 
> **Fix from command line:**
> ```bash
> # Unpause DAG
> airflow dags unpause <dag_id>
> ```
> 
> **Why:** New DAGs are often paused by default to prevent accidental execution.
> 
> ---
> 
> **4. Executor Capacity**
> 
> **Check for available task slots:**
> 
> **In Web UI:**
> - Look at running tasks count
> - Check if at maximum capacity
> 
> **Check configuration:**
> ```bash
> # Check airflow.cfg
> cat airflow.cfg | grep -E "(parallelism|executor)"
> 
> # Output shows:
> executor = SequentialExecutor  # Only 1 task at a time!
> parallelism = 32
> ```
> 
> **Issue:** SequentialExecutor or all slots occupied
> 
> **Fix:**
> ```ini
> # In airflow.cfg, change executor
> executor = LocalExecutor
> 
> # Increase parallelism if needed
> parallelism = 64
> ```
> 
> ---
> 
> **5. Max Active Runs**
> 
> **Check DAG configuration:**
> ```python
> dag = DAG(
>     'my_dag',
>     max_active_runs=1,  # Only 1 concurrent run allowed
>     schedule_interval='@daily'
> )
> ```
> 
> **Issue:** Previous run still in progress, blocking new runs
> 
> **Check:**
> - View "DAG Runs" in Web UI
> - Look for stuck/running instances
> 
> **Fix:**
> - Wait for previous run to complete
> - Kill stuck runs if necessary
> - Increase `max_active_runs` if appropriate
> 
> ---
> 
> **6. Catchup Setting**
> 
> **Check catchup configuration:**
> ```python
> dag = DAG(
>     'my_dag',
>     catchup=False,  # Don't backfill
>     start_date=datetime(2020, 1, 1)  # Old date
> )
> ```
> 
> **Without `catchup=False`:**
> - Airflow tries to run all missed intervals since start_date
> - Can overwhelm system
> - Might appear "stuck"
> 
> **Fix:**
> ```python
> # Add catchup=False for new DAGs
> dag = DAG(
>     'my_dag',
>     catchup=False
> )
> ```
> 
> ---
> 
> **7. Dependencies Blocking Execution**
> 
> **Check:**
> - Are upstream tasks failing?
> - Are sensors waiting forever?
> - Check task dependencies in Graph view
> 
> **Fix:**
> - Review task states
> - Fix failing upstream tasks
> - Check sensor timeouts
> 
> ---
> 
> **Complete Troubleshooting Checklist:**
> 
> ```bash
> # 1. Check scheduler
> ps aux | grep scheduler
> airflow info | grep executor
> 
> # 2. Check DAG is unpaused
> airflow dags list | grep my_dag
> # Look for "True" in paused column
> 
> # 3. Unpause if needed
> airflow dags unpause my_dag
> 
> # 4. Check last run
> airflow dags list-runs -d my_dag
> 
> # 5. Manually trigger to test
> airflow dags trigger my_dag
> 
> # 6. Check logs
> tail -f ~/airflow/logs/scheduler/*.log
> ```
> 
> **Web UI Checks:**
> 1. Browse → DAG Runs → Check for queued/running
> 2. Graph View → Check task states
> 3. Admin → Configuration → Verify executor
> 4. Toggle DAG pause state
> 
> **Most Common Causes (in order):**
> 1. ⚠️ Scheduler not running (40%)
> 2. ⚠️ DAG is paused (30%)
> 3. ⚠️ Schedule interval not passed (15%)
> 4. ⚠️ Executor at capacity (10%)
> 5. ⚠️ Other configuration issues (5%)

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (30 points)
1. B | 2. B | 3. B | 4. C | 5. C | 6. B | 7. C | 8. B | 9. B | 10. C | 11. B | 12. B | 13. B | 14. C | 15. B

### Section B: True/False (15 points)
16. T | 17. F | 18. T | 19. T | 20. F | 21. T | 22. F | 23. T | 24. T | 25. T | 26. T | 27. T | 28. T | 29. T | 30. T

### Section C: Short Answer (15 points)
See detailed answers above - award full points for comprehensive responses

### Section D: Code Analysis (10 points)
See detailed answers above - award points for correct interpretation

---

## Quick Study Guide

### Sensors

```python
from airflow.sensors.filesystem import FileSensor

sensor = FileSensor(
    task_id='wait',
    filepath='file.csv',
    poke_interval=300,     # Check every 5 min
    timeout=3600,          # Timeout after 1 hour
    mode='poke'            # or 'reschedule'
)
```

**Modes:**
- `poke`: Keep checking (holds slot)
- `reschedule`: Release slot between checks

### Executors

| Executor | Tasks/Time | Use Case |
|----------|------------|----------|
| Sequential | 1 | Debug only |
| Local | Many (1 system) | Small/Medium prod |
| Kubernetes | Many (cluster) | Large prod |

### Debugging Commands

```bash
# Find import errors
airflow dags list-import-errors

# Test Python syntax
python3 my_dag.py

# Start scheduler
airflow scheduler

# Check executor
airflow info | grep executor
```

### SLA Definition

```python
from datetime import timedelta

# Individual task
task = BashOperator(
    task_id='task',
    bash_command='cmd',
    sla=timedelta(minutes=10)
)

# All tasks
default_args = {
    'sla': timedelta(minutes=20)
}
```

### Reporting

```python
default_args = {
    'email': ['alerts@company.com'],
    'email_on_failure': True,
    'email_on_retry': False,
    'email_on_success': True
}
```

---

## Key Concepts Summary

| Concept | Definition |
|---------|------------|
| **Sensor** | Operator that waits for condition |
| **FileSensor** | Waits for file existence |
| **poke** | Check repeatedly (holds slot) |
| **reschedule** | Release slot between checks |
| **poke_interval** | Time between checks (seconds) |
| **SequentialExecutor** | 1 task at a time (default) |
| **LocalExecutor** | Parallel on single system |
| **KubernetesExecutor** | Distributed on Kubernetes |
| **SLA** | Expected task duration |
| **SLA Miss** | Task exceeds SLA time |
| **timedelta** | Time duration object |

---

## Common Mistakes to Avoid

❌ **Don't:**
- Use SequentialExecutor in production
- Set timeout=None for sensors (can wait forever)
- Forget to start scheduler
- Ignore SLA misses
- Use `mode='poke'` for long waits
- Deploy without testing syntax first

✅ **Do:**
- Use `mode='reschedule'` for long waits
- Set reasonable timeouts on sensors
- Monitor scheduler status
- Set realistic SLAs
- Test DAGs with `python3 dagfile.py`
- Use LocalExecutor or better for production
- Check `airflow dags list-import-errors`

---

## Study Strategy (3-Day Plan)

### Day 1: Sensors and Executors
- Learn sensor types and modes
- Understand poke vs reschedule
- Study executor differences
- Practice sensor syntax

### Day 2: Debugging
- Learn troubleshooting methods
- Practice debugging commands
- Understand common issues
- Test DAG loading

### Day 3: SLAs and Monitoring
- Understand SLA concept
- Practice timedelta usage
- Learn reporting configuration
- Take practice quiz

---

## Before the Exam - Final Checklist

- [ ] Know what sensors do (wait for conditions)
- [ ] Understand poke vs reschedule modes
- [ ] Know poke_interval and timeout
- [ ] Understand FileSensor checks file existence
- [ ] Know ExternalTaskSensor for cross-DAG
- [ ] Know three executor types
- [ ] Remember SequentialExecutor not for production
- [ ] Know LocalExecutor uses single system
- [ ] Know KubernetesExecutor uses cluster
- [ ] Can check executor in airflow.cfg
- [ ] Know `airflow dags list-import-errors` command
- [ ] Know `python3 dagfile.py` tests syntax
- [ ] Understand SLA = expected time
- [ ] Know SLA miss sends email and logs
- [ ] Can use timedelta for durations
- [ ] Know email reporting options

---

#apache-airflow #sensors #executors #debugging #sla #monitoring #filesensor #troubleshooting #timedelta #sequential #local #kubernetes #datacamp #chapter3 #practice-quiz
>