## Overview

This chapter introduces version control concepts, Git as a version control system, basic terminal commands, and how to create and initialize Git repositories.

## What is Version Control?

**Definition**: Processes and systems to manage changes to files, programs, and directories

### What Should Be Version Controlled?

Version control is useful for anything that:

- **Changes over time**, or
- **Needs to be shared**

### What Can Version Control Do?

- Track files in different states
- Combine different versions of files
- Identify a particular version
- Revert changes

### Why is Version Control Important?

Version control helps manage projects collaboratively and maintains history of changes, preventing loss of work and enabling team collaboration.

## Introducing Git

### What is Git?

- **Popular version control system** for software development and data projects
- **Open source**
- **Scalable**

### Benefits of Git

- **Git stores everything**, so nothing is lost
- We can **compare files at different times**
- See what **changes were made, by who, and when**
- **Revert to previous versions** of files!

## Using Git

### The Shell (Terminal)

**Git commands are run on the shell**, also known as the **terminal**

**The shell:**

- Is a program for executing commands
- Can be used to easily preview or inspect files and directories

> [!info] Directory = Folder In the command line, "directory" and "folder" mean the same thing

## Useful Terminal Commands

### pwd (Print Working Directory)

Shows your current location in the file system:

```bash
pwd
# Output: /home/repl/Documents
```

### ls (List)

Lists files and directories in the current directory:

```bash
ls
# Output: archive finance.csv finance_data_clean.csv finance_data_modified.csv
```

### cd (Change Directory)

Changes to a different directory:

```bash
cd archive
pwd
# Output: /home/repl/Documents/archive
```

### Checking Git Version

```bash
git --version
# Output: git version 2.46.0
```

## Creating Repos

### What is a Git Repo?

**Git repo** = directory containing files and sub-directories

**More specifically:**

- Git repo = directory containing files and sub-directories, **and Git storage**
- Git stores version information in a hidden `.git` directory

> [!warning] Do Not Edit .git! Never manually edit the `.git` directory - it contains Git's internal data

### Why Make a Repo?

**Benefits of repos:**

- Systematically track versions
- Revert to previous versions
- Compare versions at different points in time
- Collaborate with colleagues

### Creating a New Repo

**Method 1: Create new directory and initialize**

```bash
git init mental-health-workspace
cd mental-health-workspace
git status
# Output: On branch main
#         No commits yet
#         nothing to commit (create/copy files and use "git add" to track)
```

**Method 2: Convert existing directory into a repo**

```bash
# From inside an existing directory
git init
# Output: Initialized empty Git repository in /home/repl/mental-health-workspace/.git/
```

### Checking What is Being Tracked

```bash
git status
# Output:
# On branch main
# No commits yet
# Untracked files:
#   (use "git add <file>..." to include what will be committed)
#         data/
#         report.md
# nothing added to commit but untracked files present (use "git add" to track)
```

**Git recognizes there are modified files not being tracked!**

### Nested Repositories

> [!warning] Don't Create Nested Repos **Don't create a Git repo inside another Git repo**
> 
> - Known as **nested repos**
> - There will be two `.git` directories
> - Problem: Which `.git` directory should be updated?

## Staging and Committing Files

### The Git Workflow

**Three-step process:**

1. **Edit and save files** on our computer
2. **Add the file(s) to the Git staging area**
    - Tracks what has been modified
3. **Commit the files**
    - Git takes a snapshot of the files at the point in time
    - Allows us to compare and revert files

### Staging versus Committing

**Staging area:**

- Temporary holding area for changes
- You choose which changes to include in the next commit

**Making a commit:**

- Creates a permanent snapshot
- Records changes to the repository
- Includes a message describing what changed

### Adding to the Staging Area

**Adding a single file:**

```bash
git add README.md
```

**Adding all modified files:**

```bash
git add .
```

> [!info] The Dot (.) `.` = all files in the current directory and sub-directories

### Making a Commit

```bash
git commit -m "Adding a README."
# Output:
# [main cb33c18] Adding a README.
# 1 file changed, 1 insertion(+)
# create mode 100644 README.md
```

**Key points:**

- **`-m`** allows a log message without opening a text editor
- **Log message is useful for reference**
- **Best practice** = short and concise

## Key Commands Summary

|Command|Purpose|Example|
|---|---|---|
|`pwd`|Print working directory|Shows current location|
|`ls`|List files/directories|Shows contents of directory|
|`cd <directory>`|Change directory|Navigate to a folder|
|`git --version`|Check Git version|Verify Git installation|
|`git init`|Initialize repository|Create new repo in current dir|
|`git init <name>`|Create & initialize repo|Create new directory with Git|
|`git status`|Check repository status|See tracked/untracked files|
|`git add <file>`|Add file to staging|Stage a specific file|
|`git add .`|Add all files|Stage all modified files|
|`git commit -m "message"`|Commit changes|Create snapshot with message|

## Important Concepts

### Git Workflow Stages

1. **Working Directory** - Where you edit files
2. **Staging Area** - Where you prepare files for commit
3. **Repository** - Where Git stores committed snapshots

### Best Practices

- Use short, concise commit messages
- Don't create nested repositories
- Don't edit the `.git` directory
- Use `git status` frequently to check what's tracked
- Stage related changes together

---

#git #version-control #git-basics #repository #staging #commit #shell #terminal

# Chapter 1: Introduction to Version Control - Practice Exam

**Course:** Introduction to Git  
**Chapter:** Introduction to Version Control  
**Total Points:** 100  
**Passing Score:** 70/100 (70%)  
**Time Limit:** 60 minutes

---

## Section A: Multiple Choice Questions (30 questions × 2 points = 60 points)

### Topic 1: Version Control Basics (10 questions)

**Q1.** What is version control?
- [ ] A) A system for backing up files
- [ ] B) Processes and systems to manage changes to files, programs, and directories
- [ ] C) A file compression tool
- [ ] D) A security system for protecting files

> [!success]- Answer
> **Correct Answer: B) Processes and systems to manage changes to files, programs, and directories**
> 
> This is the definition provided in the chapter for version control.

**Q2.** Version control is useful for anything that does which of the following?
- [ ] A) Only changes over time
- [ ] B) Only needs to be shared
- [ ] C) Changes over time OR needs to be shared
- [ ] D) Is stored in the cloud

> [!success]- Answer
> **Correct Answer: C) Changes over time OR needs to be shared**
> 
> The chapter states version control is useful for anything that "changes over time, or needs to be shared"

**Q3.** Which of the following can version control do?
- [ ] A) Track files in different states
- [ ] B) Combine different versions of files
- [ ] C) Identify a particular version
- [ ] D) All of the above

> [!success]- Answer
> **Correct Answer: D) All of the above**
> 
> Version control can track files, combine versions, identify versions, and revert changes.

**Q4.** What is Git?
- [ ] A) A cloud storage service
- [ ] B) A popular version control system
- [ ] C) A programming language
- [ ] D) A text editor

> [!success]- Answer
> **Correct Answer: B) A popular version control system**
> 
> Git is described as a "Popular version control system for software development and data projects"

**Q5.** Which of the following is NOT mentioned as a characteristic of Git?
- [ ] A) Open source
- [ ] B) Scalable
- [ ] C) Proprietary
- [ ] D) Popular for software development

> [!success]- Answer
> **Correct Answer: C) Proprietary**
> 
> Git is open source, not proprietary. The chapter mentions it's open source, scalable, and popular.

**Q6.** What is a key benefit of Git mentioned in the chapter?
- [ ] A) Git deletes old versions to save space
- [ ] B) Git stores everything, so nothing is lost
- [ ] C) Git only works with text files
- [ ] D) Git requires internet connection

> [!success]- Answer
> **Correct Answer: B) Git stores everything, so nothing is lost**
> 
> This is explicitly stated as one of the benefits of Git.

**Q7.** Can Git revert to previous versions of files?
- [ ] A) No, once changed files cannot be reverted
- [ ] B) Yes, Git can revert to previous versions
- [ ] C) Only within the last 24 hours
- [ ] D) Only if you have backups

> [!success]- Answer
> **Correct Answer: B) Yes, Git can revert to previous versions**
> 
> Reverting to previous versions is one of Git's key benefits.

**Q8.** What can Git show you about changes?
- [ ] A) Only what changed
- [ ] B) Only when changes were made
- [ ] C) What changes were made, by who, and when
- [ ] D) Only who made changes

> [!success]- Answer
> **Correct Answer: C) What changes were made, by who, and when**
> 
> Git tracks all three aspects: what, who, and when.

**Q9.** Why is version control important for collaboration?
- [ ] A) It allows multiple people to work on files
- [ ] B) It tracks who made what changes
- [ ] C) It helps prevent loss of work
- [ ] D) All of the above

> [!success]- Answer
> **Correct Answer: D) All of the above**
> 
> Version control enables collaboration, tracking, and prevents work loss.

**Q10.** What is one thing version control can help you do?
- [ ] A) Compile code faster
- [ ] B) Revert changes
- [ ] C) Encrypt files
- [ ] D) Compress files

> [!success]- Answer
> **Correct Answer: B) Revert changes**
> 
> Reverting changes is explicitly listed as something version control can do.

### Topic 2: Shell/Terminal Commands (10 questions)

**Q11.** Where are Git commands run?
- [ ] A) In a graphical interface only
- [ ] B) On the shell, also known as the terminal
- [ ] C) In a web browser
- [ ] D) In a mobile app

> [!success]- Answer
> **Correct Answer: B) On the shell, also known as the terminal**
> 
> The chapter states: "Git commands are run on the shell, also known as the terminal"

**Q12.** What is the shell?
- [ ] A) A file storage system
- [ ] B) A program for executing commands
- [ ] C) A type of database
- [ ] D) A cloud service

> [!success]- Answer
> **Correct Answer: B) A program for executing commands**
> 
> The shell is defined as "a program for executing commands"

**Q13.** In the context of the shell, what does "directory" mean?
- [ ] A) A file
- [ ] B) A command
- [ ] C) A folder
- [ ] D) A program

> [!success]- Answer
> **Correct Answer: C) A folder**
> 
> The chapter explicitly states: "Directory = folder"

**Q14.** What does the `pwd` command do?
- [ ] A) Print working directory (shows current location)
- [ ] B) Print working document
- [ ] C) Password
- [ ] D) Power down

> [!success]- Answer
> **Correct Answer: A) Print working directory (shows current location)**
> 
> `pwd` prints the current working directory path.

**Q15.** What does the `ls` command do?
- [ ] A) Lists Git commands
- [ ] B) Lists files and directories in the current directory
- [ ] C) Logs into system
- [ ] D) Loads saved files

> [!success]- Answer
> **Correct Answer: B) Lists files and directories in the current directory**
> 
> `ls` lists the contents of the current directory.

**Q16.** What does the `cd` command do?
- [ ] A) Creates a directory
- [ ] B) Changes directory
- [ ] C) Copies data
- [ ] D) Clears display

> [!success]- Answer
> **Correct Answer: B) Changes directory**
> 
> `cd` stands for "change directory" and navigates to a different folder.

**Q17.** What command checks your Git version?
- [ ] A) git version
- [ ] B) git check
- [ ] C) git --version
- [ ] D) version git

> [!success]- Answer
> **Correct Answer: C) git --version**
> 
> The command `git --version` displays the Git version installed.

**Q18.** After running `cd archive`, where are you located?
- [ ] A) In the parent directory
- [ ] B) In the archive directory
- [ ] C) In the root directory
- [ ] D) In the home directory

> [!success]- Answer
> **Correct Answer: B) In the archive directory**
> 
> `cd archive` moves you into the archive directory.

**Q19.** What can the shell be used for besides running Git commands?
- [ ] A) Previewing or inspecting files and directories
- [ ] B) Creating graphics
- [ ] C) Playing videos
- [ ] D) Sending emails

> [!success]- Answer
> **Correct Answer: A) Previewing or inspecting files and directories**
> 
> The shell "can be used to easily preview or inspect files and directories"

**Q20.** Which command would you use to see your current location in the file system?
- [ ] A) ls
- [ ] B) cd
- [ ] C) pwd
- [ ] D) git status

> [!success]- Answer
> **Correct Answer: C) pwd**
> 
> `pwd` (print working directory) shows your current location.

### Topic 3: Git Repositories and Workflow (10 questions)

**Q21.** What is a Git repo?
- [ ] A) A text file
- [ ] B) A directory containing files and sub-directories, and Git storage
- [ ] C) A website
- [ ] D) A database

> [!success]- Answer
> **Correct Answer: B) A directory containing files and sub-directories, and Git storage**
> 
> A Git repo includes both your files and the Git storage (`.git` directory).

**Q22.** Where does Git store its version information?
- [ ] A) In a cloud server
- [ ] B) In a .git directory
- [ ] C) In individual files
- [ ] D) In a database

> [!success]- Answer
> **Correct Answer: B) In a .git directory**
> 
> Git stores information in a hidden `.git` directory.

**Q23.** Should you manually edit the .git directory?
- [ ] A) Yes, it's recommended
- [ ] B) Yes, but only for advanced users
- [ ] C) No, do not edit .git
- [ ] D) Only on Mondays

> [!success]- Answer
> **Correct Answer: C) No, do not edit .git**
> 
> The chapter explicitly warns: "Do not edit .git!"

**Q24.** What command initializes a new Git repository in an existing directory?
- [ ] A) git start
- [ ] B) git create
- [ ] C) git init
- [ ] D) git new

> [!success]- Answer
> **Correct Answer: C) git init**
> 
> `git init` initializes a Git repository in the current directory.

**Q25.** What does `git init mental-health-workspace` do?
- [ ] A) Only creates a directory
- [ ] B) Creates a new directory and initializes it as a Git repo
- [ ] C) Deletes a directory
- [ ] D) Renames a directory

> [!success]- Answer
> **Correct Answer: B) Creates a new directory and initializes it as a Git repo**
> 
> This command both creates the directory and initializes Git in it.

**Q26.** What command shows the status of your Git repository?
- [ ] A) git check
- [ ] B) git show
- [ ] C) git status
- [ ] D) git info

> [!success]- Answer
> **Correct Answer: C) git status**
> 
> `git status` displays the current state of the repository.

**Q27.** What is a "nested repo"?
- [ ] A) A repo with many files
- [ ] B) A Git repo inside another Git repo
- [ ] C) A compressed repo
- [ ] D) A remote repo

> [!success]- Answer
> **Correct Answer: B) A Git repo inside another Git repo**
> 
> Nested repos are when you create a Git repo inside another Git repo.

**Q28.** Should you create nested repositories?
- [ ] A) Yes, it's best practice
- [ ] B) Yes, for organization
- [ ] C) No, don't create nested repos
- [ ] D) Only for large projects

> [!success]- Answer
> **Correct Answer: C) No, don't create nested repos**
> 
> The chapter warns: "Don't create a Git repo inside another Git repo"

**Q29.** What problem do nested repositories cause?
- [ ] A) Files get deleted
- [ ] B) Unclear which .git directory should be updated
- [ ] C) Git stops working
- [ ] D) Files get compressed

> [!success]- Answer
> **Correct Answer: B) Unclear which .git directory should be updated**
> 
> With two `.git` directories, it's unclear which one should track changes.

**Q30.** What does `git status` show you about files?
- [ ] A) Only file sizes
- [ ] B) Only file names
- [ ] C) Tracked and untracked files
- [ ] D) Only deleted files

> [!success]- Answer
> **Correct Answer: C) Tracked and untracked files**
> 
> `git status` shows which files are tracked, untracked, or modified.

---

## Section B: True/False Questions (15 questions × 1 point = 15 points)

**Q31.** Version control can only track files, not directories. **T/F**

> [!success]- Answer
> **False** - Version control manages changes to files, programs, AND directories.

**Q32.** Git is an open source version control system. **T/F**

> [!success]- Answer
> **True** - Git is explicitly described as open source.

**Q33.** Git stores everything, so nothing is lost. **T/F**

> [!success]- Answer
> **True** - This is stated as one of Git's key benefits.

**Q34.** Git commands are run in a web browser. **T/F**

> [!success]- Answer
> **False** - Git commands are run on the shell/terminal.

**Q35.** In command line terminology, "directory" and "folder" mean the same thing. **T/F**

> [!success]- Answer
> **True** - The chapter states: "Directory = folder"

**Q36.** The `pwd` command prints your password. **T/F**

> [!success]- Answer
> **False** - `pwd` stands for "print working directory" and shows your current location.

**Q37.** The `ls` command lists files and directories in the current directory. **T/F**

> [!success]- Answer
> **True** - This is the purpose of the `ls` command.

**Q38.** It's safe to manually edit the `.git` directory. **T/F**

> [!success]- Answer
> **False** - The chapter explicitly warns: "Do not edit .git!"

**Q39.** `git init` creates a new Git repository in the current directory. **T/F**

> [!success]- Answer
> **True** - This command initializes a Git repository.

**Q40.** You should create Git repositories inside other Git repositories for better organization. **T/F**

> [!success]- Answer
> **False** - The chapter warns against creating nested repos.

**Q41.** `git status` can show you which files are untracked. **T/F**

> [!success]- Answer
> **True** - `git status` displays tracked and untracked files.

**Q42.** The staging area is where Git permanently stores file snapshots. **T/F**

> [!success]- Answer
> **False** - The staging area is temporary; commits create permanent snapshots.

**Q43.** `git add .` adds all files in the current directory and sub-directories to the staging area. **T/F**

> [!success]- Answer
> **True** - The `.` represents all files in current directory and subdirectories.

**Q44.** The `-m` flag in `git commit -m "message"` allows you to add a log message without opening a text editor. **T/F**

> [!success]- Answer
> **True** - This is explicitly stated in the chapter.

**Q45.** Commit messages should be long and detailed. **T/F**

> [!success]- Answer
> **False** - Best practice is short and concise commit messages.

---

## Section C: Short Answer Questions (8 questions × 3 points = 24 points)

**Q46.** List the four things that version control can do according to the chapter.

> [!success]- Answer
> Version control can:
> 1. **Track files in different states** - Monitor the status of files over time
> 2. **Combine different versions of files** - Merge changes from multiple versions
> 3. **Identify a particular version** - Pinpoint specific points in file history
> 4. **Revert changes** - Go back to previous versions of files
> 
> These capabilities make version control essential for managing projects with changing files.

**Q47.** Describe the three-step Git workflow process.

> [!success]- Answer
> The three-step Git workflow is:
> 
> **Step 1: Edit and save files on our computer**
> - Make changes to your files in your working directory
> - Save them normally as you would any file
> 
> **Step 2: Add the file(s) to the Git staging area**
> - Use `git add` to stage changes
> - The staging area tracks what has been modified
> - You choose which changes to include in the next commit
> 
> **Step 3: Commit the files**
> - Use `git commit -m "message"` to create a snapshot
> - Git takes a snapshot of the files at that point in time
> - This allows us to compare and revert files later
> 
> This workflow gives you control over what gets permanently recorded in your repository history.

**Q48.** Explain the difference between the staging area and making a commit.

> [!success]- Answer
> **Staging Area:**
> - A temporary holding area for changes
> - You add files here with `git add`
> - Tracks what has been modified
> - You can add and remove files from staging before committing
> - Allows you to choose which changes to include in the next commit
> - Not permanent - can be changed before committing
> 
> **Making a Commit:**
> - Creates a permanent snapshot of staged changes
> - Records changes to the repository with `git commit`
> - Git saves the exact state of files at that point in time
> - Allows you to compare and revert files in the future
> - Includes a message describing what changed
> - Permanent record in the repository history
> 
> **Analogy:** Staging is like preparing items for a photo, while committing is like actually taking the photo.

**Q49.** What are the benefits of creating a Git repository?

> [!success]- Answer
> The benefits of repos are:
> 
> 1. **Systematically track versions** - Keep organized history of all changes
> 2. **Revert to previous versions** - Undo mistakes by going back in time
> 3. **Compare versions at different points in time** - See what changed between versions
> 4. **Collaborate with colleagues** - Multiple people can work on the same project
> 
> These benefits make Git repositories essential for any project involving:
> - Files that change over time
> - Files that need to be shared
> - Collaborative work
> - Need to maintain history of changes

**Q50.** What is a nested repository and why should you avoid creating them?

> [!success]- Answer
> **What is a nested repository:**
> - A nested repo is a Git repository created inside another Git repository
> - This means there would be two `.git` directories:
>   - One in the parent directory
>   - One in a subdirectory
> 
> **Why you should avoid them:**
> - **Unclear which `.git` directory should be updated** when you make changes
> - Git doesn't know which repository to track changes in
> - Can cause confusion and conflicts
> - Makes version control unpredictable
> - Violates the principle of having one repository per project
> 
> **Best practice:** Keep repositories separate and don't initialize Git inside an existing Git repository.

**Q51.** Explain what `git add .` does and what the dot (.) represents.

> [!success]- Answer
> **What it does:**
> `git add .` adds all modified files to the Git staging area, preparing them for the next commit.
> 
> **What the dot (.) represents:**
> - The `.` means "all files"
> - Specifically: all files in the current directory and sub-directories
> - It's a shorthand for selecting everything
> 
> **Comparison:**
> - `git add README.md` - adds only one specific file
> - `git add .` - adds all modified files in current directory and subdirectories
> 
> **When to use:**
> - When you want to stage all your changes at once
> - When you've made multiple file changes that should be committed together
> - Quick way to prepare everything for a commit
> 
> **Note:** Be careful with `git add .` - make sure you actually want to stage ALL changes!

**Q52.** What does the `-m` flag do in the `git commit` command, and what is the best practice for commit messages?

> [!success]- Answer
> **What `-m` does:**
> - The `-m` flag allows you to add a log message without opening a text editor
> - Stands for "message"
> - Lets you include the commit message directly in the command line
> - Example: `git commit -m "Adding a README."`
> 
> **Why log messages are useful:**
> - Provides context for what changed
> - Helps you and others understand the history
> - Makes it easier to find specific changes later
> - Documents the purpose of each commit
> 
> **Best practice for commit messages:**
> - **Short and concise** (as stated in the chapter)
> - Clearly describe what changed
> - Use present tense (e.g., "Add feature" not "Added feature")
> - Keep it under 50 characters when possible
> - Focus on what and why, not how
> 
> Example good messages: "Add login feature", "Fix typo in README", "Update data analysis code"

**Q53.** List the three basic terminal commands covered in the chapter and explain what each one does.

> [!success]- Answer
> The three basic terminal commands are:
> 
> **1. `pwd` (Print Working Directory)**
> - Shows your current location in the file system
> - Displays the full path to where you are
> - Example output: `/home/repl/Documents`
> - Useful when you need to know your current directory
> 
> **2. `ls` (List)**
> - Lists files and directories in the current directory
> - Shows the contents of where you are
> - Example output: `archive finance.csv finance_data_clean.csv`
> - Helps you see what files and folders are available
> 
> **3. `cd` (Change Directory)**
> - Changes to a different directory/folder
> - Navigates you to a new location
> - Example: `cd archive` moves you into the archive folder
> - Essential for moving around the file system
> 
> These three commands are fundamental for navigating and working in the terminal/shell environment where Git commands are executed.

---

## Section D: Command Practice & Scenarios (3 questions × 2 points = 6 points - Bonus)

**Q54.** Put these Git workflow steps in the correct order:
A) git commit -m "message"
B) Edit files
C) git add .

> [!success]- Answer
> **Correct Order: B, C, A**
> 
> 1. **B) Edit files** - First, make changes to your files
> 2. **C) git add .** - Second, add changes to staging area
> 3. **A) git commit -m "message"** - Finally, commit the staged changes
> 
> This follows the Git workflow: Edit → Stage → Commit

**Q55.** What command would you use to initialize a new Git repository called "data-project"?

> [!success]- Answer
> ```bash
> git init data-project
> ```
> 
> **Explanation:**
> - `git init` initializes a Git repository
> - `data-project` is the name of the new directory to create
> - This command both creates the directory AND initializes Git in it
> - After running this, you would use `cd data-project` to move into it
> 
> **Alternative approach:**
> ```bash
> mkdir data-project
> cd data-project
> git init
> ```
> This achieves the same result but in three steps instead of one.

**Q56.** You're in the `/home/user/Documents` directory. You run `cd projects` then `pwd`. What will `pwd` display?

> [!success]- Answer
> **Output: `/home/user/Documents/projects`**
> 
> **Explanation:**
> - You started in `/home/user/Documents`
> - `cd projects` moved you into the projects subdirectory
> - `pwd` displays your new current location
> - The full path is the original path plus the new directory
> 
> **Step by step:**
> 1. Current location: `/home/user/Documents`
> 2. Run: `cd projects`
> 3. New location: `/home/user/Documents/projects`
> 4. `pwd` confirms: `/home/user/Documents/projects`

---

## Answer Key & Scoring

### Section A: Multiple Choice (60 points)
1. B  |  2. C  |  3. D  |  4. B  |  5. C  
2. B  |  7. B  |  8. C  |  9. D  |  10. B  
3. B  |  12. B  |  13. C  |  14. A  |  15. B  
4. B  |  17. C  |  18. B  |  19. A  |  20. C  
5. B  |  22. B  |  23. C  |  24. C  |  25. B  
6. C  |  27. B  |  28. C  |  29. B  |  30. C  

### Section B: True/False (15 points)
31. F  |  32. T  |  33. T  |  34. F  |  35. T  
32. F  |  37. T  |  38. F  |  39. T  |  40. F  
33. T  |  42. F  |  43. T  |  44. T  |  45. F  

### Section C: Short Answer (24 points)
- Q46-Q53: See detailed answers in collapsible sections (3 points each)

### Section D: Command Practice (6 points - Bonus)
- Q54-Q56: See detailed answers in collapsible sections (2 points each)

---

## Quick Reference Guide

### Essential Terminal Commands
```bash
pwd                    # Print working directory
ls                     # List files and directories
cd <directory>         # Change to directory
git --version          # Check Git version
```

### Git Repository Commands
```bash
git init               # Initialize repo in current directory
git init <name>        # Create new directory and initialize
git status             # Check repository status
```

### Git Workflow Commands
```bash
git add <file>         # Add specific file to staging
git add .              # Add all files to staging
git commit -m "msg"    # Commit with message
```

---

## Key Concepts Summary

| Concept | Description |
|---------|-------------|
| **Version Control** | Processes to manage changes to files/directories |
| **Git** | Open source, scalable version control system |
| **Shell/Terminal** | Program for executing commands |
| **Repository (Repo)** | Directory with files and Git storage (.git) |
| **Staging Area** | Temporary area for preparing commits |
| **Commit** | Permanent snapshot of files at a point in time |
| **Working Directory** | Where you edit files |
| **.git Directory** | Where Git stores version information (don't edit!) |
| **Nested Repos** | Git repo inside another (avoid this!) |

---

## Common Mistakes to Avoid

❌ Manually editing the `.git` directory  
❌ Creating nested repositories (repo inside repo)  
❌ Forgetting to use `git add` before `git commit`  
❌ Writing long, unclear commit messages  
❌ Confusing `pwd` (print working directory) with password  
❌ Using `git init` inside an existing Git repository  
❌ Not checking `git status` before committing  
❌ Thinking staging and committing are the same thing  

---

## Study Strategy (3-Day Plan)

### Day 1: Version Control & Terminal Basics
- Understand what version control is and why it's important
- Learn the benefits of Git
- Practice terminal commands: `pwd`, `ls`, `cd`
- Get comfortable navigating directories in terminal
- Check Git version on your system

### Day 2: Repositories
- Understand what a Git repo is
- Practice `git init` to create repos
- Learn about the `.git` directory (and why not to edit it)
- Use `git status` to check repository state
- Understand tracked vs untracked files
- Learn why nested repos are problematic

### Day 3: Git Workflow
- Master the three-step workflow: Edit → Stage → Commit
- Practice `git add` for single files and all files
- Learn `git commit -m` with good messages
- Understand staging vs committing
- Review all commands together
- Complete practice quiz

---

## Pre-Exam Checklist

✅ Know what version control is and what it can do  
✅ Understand Git's key benefits  
✅ Remember Git commands are run in shell/terminal  
✅ Know basic terminal commands: pwd, ls, cd  
✅ Understand what a Git repo is  
✅ Know `.git` directory stores Git data (don't edit!)  
✅ Remember `git init` initializes a repository  
✅ Understand nested repos are problematic  
✅ Know the three-step workflow: Edit → Stage → Commit  
✅ Remember `git add` stages files  
✅ Know `git add .` adds all files  
✅ Understand `git commit -m` creates snapshots  
✅ Best practice: short, concise commit messages  

---

## During the Exam

💡 **Version control basics** - Remember the four main capabilities  
💡 **Git benefits** - Git stores everything, can revert changes  
💡 **Terminal vs GUI** - Git uses shell/terminal  
💡 **Directory = Folder** - Same thing in command line  
💡 **pwd** - Print working directory (not password!)  
💡 **.git warning** - Never manually edit this directory  
💡 **Nested repos** - Don't create repo inside repo  
💡 **Workflow order** - Always Edit → Stage → Commit  
💡 **git add .** - The dot means ALL files  
💡 **Commit messages** - Should be short and concise  

---

#git #version-control #terminal #shell #repository #staging #commit #exam-prep