## Overview
This chapter covers how Git stores data, viewing commit history with `git log`, comparing versions with `git diff`, and restoring/reverting files with `git revert`, `git checkout`, and `git restore`.

## The Commit Structure

### Three Parts of Git Commits

**Git commits have three parts:**

1. **Commit**
   - Contains the metadata
   - Author
   - Log message
   - Commit time

2. **Tree**
   - Tracks the names and locations of files and directories in the repo
   - Like a dictionary - mapping keys to files/directories

3. **Blob**
   - Binary Large OBject
   - May contain data of any kind
   - A compressed snapshot of a file's contents

## Git Hash

**Characteristics:**
- Example: `b22eb75a82a68b9c0f1c45b9f5a9b7abe281683a`
- Pseudo-random number generator—hash function
- Hashes allow data sharing between repos
- **If two files are the same, then their hashes are the same**
- Git only needs to compare hashes

> [!info] Short Hashes
> Only need the first 8-10 characters of the hash for most Git operations

## Git Log

### Basic git log

```bash
git log
```

**Purpose:** Shows commits from newest to oldest

**Output format:**
```
commit ad8accfe94cb924444c488132bdef7c54b9bca68
Author: Rep Loop <repl@datacamp.com>
Date: Wed Jul 24 07:48:27 2022 +0000

    Added reminder to cite funding sources.
```

**Navigation:**
- Press **space** to show more recent commits
- Press **q** to quit the log and return to the terminal

## Version History Tips and Tricks

### Restricting the Number of Commits

**Problem:** Larger project = more commits = larger output

**Solution:** Restrict using `-<number>`

```bash
# Restrict to the 3 most recent commits
git log -3
```

### Restricting to a Specific File

To only look at the commit history of one file:

```bash
git log report.md
```

### Combining Techniques

You can combine directory navigation, number restriction, and file specification:

```bash
cd data
git log -2 mental_health_survey.csv
```

**Example output:**
```
commit f35b9487c063d3facc853c1789b0b77087a859fa
Author: Rep Loop <repl@datacamp.com>
Date: Fri Jul 26 15:14:32 2024 +0000

    Add two new participants' data.

commit 7f71eadea60bf38f53c8696d23f8314d85342aaf
Author: Rep Loop <repl@datacamp.com>
Date: Fri Jul 19 09:58:21 2024 +0000

    Adding fresh data for the survey.
```

### Customizing the Date Range

**Restrict git log by date:**

```bash
git log --since='Month Day Year'
```

**Examples:**

```bash
# Commits since 2nd April 2024
git log --since='Apr 2 2024'

# Commits between 2nd and 11th April
git log --since='Apr 2 2024' --until='Apr 11 2024'
```

### Acceptable Filter Formats

**Natural language:**
- `"2 weeks ago"`
- `"3 months ago"`
- `"yesterday"`

**Date format:**
- `"07-15-2024"`
- **Recommend ISO Format 8601**: `"YYYY-MM-DD"`
- `"15 Jul 2024"` or `"15 July 2024"`
- **Invalid**: `"15 Jul, 2024"`

> [!warning] System Settings
> Check system settings for compatibility - `12-06-2024` could be 6th Dec or 12th June!

## Finding a Particular Commit

### Using git show

```bash
git show c27fa856
```

**Key point:** Only need the first 8-10 characters of the hash

**Output:** Shows the commit details and the changes made in that commit

## Comparing Versions with git diff

### Basic git diff

```bash
git diff
```

**Purpose:** Difference between versions

**Default behavior:** Compare last committed version with latest version not in the staging area

### Compare Specific Unstaged File

```bash
git diff report.md
```

**Purpose:** Compare last committed version with latest version of a specific file (not in staging area)

### git diff Output Format

**Lines starting with:**
- `+` = Added lines (in green)
- `-` = Removed lines (in red)
- Context lines shown without prefix

### Comparing to a Staged File

**Add file to staging area:**
```bash
git add report.md
```

**Compare staged version with last commit:**
```bash
git diff --staged report.md
```

**Purpose:** Compare last committed version of report.md with the version in the staging area

### Comparing Multiple Staged Files

```bash
git diff --staged
```

**Purpose:** Compare all staged files to versions in the last commit

### Comparing Two Commits

**Find the commit hashes:**
```bash
git log
```

**Compare them:**
```bash
git diff 35f4b4d 186398f
```

**What it shows:** What changed from first hash to second hash

> [!info] Best Practice
> Put most recent hash second

**Using HEAD:**
- `HEAD` = State in latest commit
- `HEAD~1` = Second most recent commit
- `HEAD~2` = Third most recent commit

```bash
# Compare second most recent with the most recent commit
git diff HEAD~1 HEAD
```

## git diff Command Summary

| Command | Function |
|---------|----------|
| `git diff` | Show changes between all unstaged files and the latest commit |
| `git diff report.md` | Show changes between an unstaged file and the latest commit |
| `git diff --staged` | Show changes between all staged files and the latest commit |
| `git diff --staged report.md` | Show changes between a staged file and the latest commit |
| `git diff 35f4b4d 186398f` | Show changes between two commits using hashes |
| `git diff HEAD~1 HEAD~2` | Show changes between two commits using HEAD instead of commit hashes |

## Restoring and Reverting Files

### git revert

**Purpose:** Restoring a repo to the state prior to a previous commit

```bash
git revert HEAD
```

**Characteristics:**
- Reinstates previous versions and **makes a commit**
- Restores all files updated in the given commit
- Can use commit hashes: `a845edcb`, `ebe93178`, etc.
- Can use HEAD syntax: `HEAD`, `HEAD~1`, etc.

**Process:**
1. Opens a text editor for commit message
2. Save: `Ctrl + O`, then `Enter`
3. Exit: `Ctrl + X`

**Output:**
```
[main 7d11f79] Revert "Adding fresh data for the survey."
Date: Tue Jul 30 14:17:56 2024 +0000
1 file changed, 3 deletions(-)
```

### git revert Flags

**Avoid opening the text editor:**
```bash
git revert --no-edit HEAD
```

**Revert without committing (bring files into the staging area):**
```bash
git revert -n HEAD
```

### Reverting a Single File

**Problem:** `git revert` works on commits, not individual files

**Solution:** Use `git checkout`

```bash
git checkout HEAD~1 -- report.md
```

**Syntax:** Use commit hash or HEAD syntax

**Check the result:**
```bash
git status
# Output:
# On branch main
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         modified: report.md
```

**Then commit:**
```bash
git commit -m "Checkout previous version of report.md"
```

### Unstaging Files

**Problem:** File was added to staging but you want to remove it from staging (without deleting changes)

### Unstaging a Single File

```bash
git restore --staged summary_statistics.csv
```

**After unstaging, you can:**
- Edit the file
- Re-add it: `git add summary_statistics.csv`
- Commit: `git commit -m "Adding age summary statistics"`

### Unstaging All Files

```bash
git restore --staged
```

**Purpose:** Remove all files from the staging area

## Restore and Revert Command Summary

| Command | Result |
|---------|--------|
| `git revert HEAD` | Revert all files from a given commit |
| `git revert HEAD --no-edit` | Revert without opening a text editor |
| `git revert HEAD -n` | Revert without making a new commit |
| `git checkout HEAD~1 -- report.md` | Revert a single file from the previous commit |
| `git restore --staged report.md` | Remove a single file from the staging area |
| `git restore --staged` | Remove all files from the staging area |

## Chapter Recap

### Chapter 1 Review
- Benefits and applications of Git for version control
- Navigating the terminal - `ls`, `cd`
- How to initiate a Git project - `git init`
- How to use Git to track your files - `git add .`, `git commit -m`

### Chapter 2 Key Concepts

**How Git stores data:**
1. Commit (metadata)
2. Tree (file/directory structure)
3. Blob (file contents)

**Viewing history:**
- `git log` with filters (`-3`, `--since`, `--until`)
- `git show` for specific commits

**Comparing versions:**
- `git diff` for unstaged changes
- `git diff --staged` for staged changes
- `git diff` with commit hashes or HEAD syntax

**Restoring files:**
- `git revert` for commits
- `git checkout` for single files
- `git restore --staged` for unstaging

## Important Distinctions

| Concept | Purpose | Creates Commit? |
|---------|---------|-----------------|
| `git revert` | Undo a commit by creating opposite commit | Yes |
| `git checkout` | Get old version of file | No (must commit manually) |
| `git restore --staged` | Remove from staging area | No |

## Key Terminology

| Term | Definition |
|------|------------|
| **Hash** | Unique identifier for each commit |
| **HEAD** | Pointer to the latest commit |
| **HEAD~1** | One commit before HEAD |
| **HEAD~2** | Two commits before HEAD |
| **Staging area** | Where files wait before commit |
| **Blob** | Binary Large OBject containing file data |
| **Tree** | Structure mapping file names to locations |

---

#git #version-control #git-log #git-diff #git-revert #commit-history #git-restore

# Chapter 2: Version History - Practice Exam

**Course:** Introduction to Git  
**Chapter:** Viewing the Version History  
**Total Points:** 100  
**Passing Score:** 70/100 (70%)  
**Time Limit:** 60 minutes

---

## Section A: Multiple Choice Questions (30 questions × 2 points = 60 points)

### Topic 1: Commit Structure and Git Hashes (10 questions)

**Q1.** How many parts does a Git commit have?
- [ ] A) Two
- [ ] B) Three
- [ ] C) Four
- [ ] D) Five

> [!success]- Answer
> **Correct Answer: B) Three**
> 
> Git commits have three parts: Commit, Tree, and Blob.

**Q2.** Which part of a Git commit contains the metadata (author, log message, commit time)?
- [ ] A) Tree
- [ ] B) Blob
- [ ] C) Commit
- [ ] D) Hash

> [!success]- Answer
> **Correct Answer: C) Commit**
> 
> The Commit part contains the metadata including author, log message, and commit time.

**Q3.** What does the Tree part of a Git commit do?
- [ ] A) Stores file contents
- [ ] B) Tracks the names and locations of files and directories
- [ ] C) Contains metadata
- [ ] D) Generates hash values

> [!success]- Answer
> **Correct Answer: B) Tracks the names and locations of files and directories**
> 
> The Tree works like a dictionary, mapping keys to files/directories.

**Q4.** What does Blob stand for?
- [ ] A) Binary Link Object Base
- [ ] B) Basic Large Object Block
- [ ] C) Binary Large OBject
- [ ] D) Block Large Object Binary

> [!success]- Answer
> **Correct Answer: C) Binary Large OBject**
> 
> Blob = Binary Large OBject

**Q5.** What does a Blob contain?
- [ ] A) Only text files
- [ ] B) Only metadata
- [ ] C) A compressed snapshot of a file's contents (data of any kind)
- [ ] D) Directory structure

> [!success]- Answer
> **Correct Answer: C) A compressed snapshot of a file's contents (data of any kind)**
> 
> A Blob may contain data of any kind and is a compressed snapshot of file contents.

**Q6.** What is a Git hash?
- [ ] A) A file name
- [ ] B) A pseudo-random number generated by a hash function
- [ ] C) A directory path
- [ ] D) A user password

> [!success]- Answer
> **Correct Answer: B) A pseudo-random number generated by a hash function**
> 
> Git hashes are generated using a pseudo-random number generator (hash function).

**Q7.** If two files are the same, what can we say about their hashes?
- [ ] A) Their hashes are different
- [ ] B) Their hashes are the same
- [ ] C) One hash is longer than the other
- [ ] D) Hashes cannot be compared

> [!success]- Answer
> **Correct Answer: B) Their hashes are the same**
> 
> The chapter states: "If two files are the same, then their hashes are the same"

**Q8.** How many characters of a hash do you typically need to identify a commit?
- [ ] A) All characters
- [ ] B) The first 2-3 characters
- [ ] C) The first 8-10 characters
- [ ] D) The last 8-10 characters

> [!success]- Answer
> **Correct Answer: C) The first 8-10 characters**
> 
> Only need the first 8-10 characters of the hash for most Git operations.

**Q9.** What advantage do hashes provide for Git?
- [ ] A) Faster file editing
- [ ] B) Git only needs to compare hashes (not entire files)
- [ ] C) Smaller file sizes
- [ ] D) Better security

> [!success]- Answer
> **Correct Answer: B) Git only needs to compare hashes (not entire files)**
> 
> Hashes allow efficient comparison - Git only needs to compare hashes instead of full file contents.

**Q10.** What do hashes allow between repos?
- [ ] A) Data encryption
- [ ] B) Data compression
- [ ] C) Data sharing
- [ ] D) Data deletion

> [!success]- Answer
> **Correct Answer: C) Data sharing**
> 
> "Hashes allow data sharing between repos"

### Topic 2: git log and Viewing History (10 questions)

**Q11.** What does `git log` show?
- [ ] A) Commits from oldest to newest
- [ ] B) Commits from newest to oldest
- [ ] C) Only uncommitted changes
- [ ] D) Only staged files

> [!success]- Answer
> **Correct Answer: B) Commits from newest to oldest**
> 
> `git log` shows commits from newest to oldest.

**Q12.** How do you exit the git log view and return to the terminal?
- [ ] A) Press Enter
- [ ] B) Press Esc
- [ ] C) Press q
- [ ] D) Press Ctrl+C

> [!success]- Answer
> **Correct Answer: C) Press q**
> 
> Press 'q' to quit the log and return to the terminal.

**Q13.** How do you see more commits while viewing git log?
- [ ] A) Press Enter
- [ ] B) Press space
- [ ] C) Press n
- [ ] D) Type "more"

> [!success]- Answer
> **Correct Answer: B) Press space**
> 
> Press space to show more recent commits in git log.

**Q14.** What does `git log -3` do?
- [ ] A) Shows commits from the last 3 days
- [ ] B) Shows the 3 oldest commits
- [ ] C) Shows the 3 most recent commits
- [ ] D) Shows commits made by 3 people

> [!success]- Answer
> **Correct Answer: C) Shows the 3 most recent commits**
> 
> The `-3` restricts to the 3 most recent commits.

**Q15.** What does `git log report.md` show?
- [ ] A) All commits
- [ ] B) Only the commit history of report.md
- [ ] C) Commits from the last report
- [ ] D) Differences in report.md

> [!success]- Answer
> **Correct Answer: B) Only the commit history of report.md**
> 
> Specifying a filename restricts log to that file's history.

**Q16.** What does `git log --since='Apr 2 2024'` show?
- [ ] A) Commits before April 2, 2024
- [ ] B) Commits on April 2, 2024 only
- [ ] C) Commits since April 2, 2024
- [ ] D) Commits until April 2, 2024

> [!success]- Answer
> **Correct Answer: C) Commits since April 2, 2024**
> 
> `--since` shows commits from the specified date forward.

**Q17.** Which date format is recommended for `git log` filters?
- [ ] A) MM/DD/YYYY
- [ ] B) DD/MM/YYYY
- [ ] C) ISO 8601 format (YYYY-MM-DD)
- [ ] D) Month-Day-Year

> [!success]- Answer
> **Correct Answer: C) ISO 8601 format (YYYY-MM-DD)**
> 
> The chapter recommends ISO Format 8601: "YYYY-MM-DD"

**Q18.** Which of the following is a valid natural language date filter for git log?
- [ ] A) "2 weeks ago"
- [ ] B) "next month"
- [ ] C) "sometime"
- [ ] D) "recently"

> [!success]- Answer
> **Correct Answer: A) "2 weeks ago"**
> 
> Valid natural language filters include "2 weeks ago", "3 months ago", "yesterday".

**Q19.** What command shows details of a specific commit?
- [ ] A) git display
- [ ] B) git view
- [ ] C) git show
- [ ] D) git details

> [!success]- Answer
> **Correct Answer: C) git show**
> 
> `git show` displays details of a specific commit.

**Q20.** How would you show details of commit c27fa856?
- [ ] A) git log c27fa856
- [ ] B) git show c27fa856
- [ ] C) git display c27fa856
- [ ] D) git commit c27fa856

> [!success]- Answer
> **Correct Answer: B) git show c27fa856**
> 
> Use `git show` followed by the commit hash (or first 8-10 characters).

### Topic 3: git diff, revert, and restore (10 questions)

**Q21.** What does `git diff` (without arguments) compare?
- [ ] A) Staged files with last commit
- [ ] B) Unstaged files with last commit
- [ ] C) Two commits
- [ ] D) Working directory with staging area

> [!success]- Answer
> **Correct Answer: B) Unstaged files with last commit**
> 
> Default `git diff` compares last committed version with latest version not in the staging area.

**Q22.** What does `git diff --staged` show?
- [ ] A) Unstaged changes
- [ ] B) Changes between staged files and last commit
- [ ] C) All commits
- [ ] D) Deleted files

> [!success]- Answer
> **Correct Answer: B) Changes between staged files and last commit**
> 
> `--staged` flag compares staged files to the last commit.

**Q23.** In git diff output, what do lines starting with `+` indicate?
- [ ] A) Removed lines
- [ ] B) Modified lines
- [ ] C) Added lines
- [ ] D) Unchanged lines

> [!success]- Answer
> **Correct Answer: C) Added lines**
> 
> `+` indicates added lines (shown in green).

**Q24.** In git diff output, what do lines starting with `-` indicate?
- [ ] A) Added lines
- [ ] B) Removed lines
- [ ] C) Modified lines
- [ ] D) Unchanged lines

> [!success]- Answer
> **Correct Answer: B) Removed lines**
> 
> `-` indicates removed lines (shown in red).

**Q25.** What does HEAD represent?
- [ ] A) The first commit
- [ ] B) The oldest commit
- [ ] C) The state in the latest commit
- [ ] D) The staging area

> [!success]- Answer
> **Correct Answer: C) The state in the latest commit**
> 
> HEAD is a pointer to the latest commit.

**Q26.** What does HEAD~1 represent?
- [ ] A) The latest commit
- [ ] B) The second most recent commit
- [ ] C) The first commit
- [ ] D) One hour ago

> [!success]- Answer
> **Correct Answer: B) The second most recent commit**
> 
> HEAD~1 is one commit before HEAD (the second most recent).

**Q27.** What does `git revert HEAD` do?
- [ ] A) Deletes the last commit
- [ ] B) Creates a new commit that undoes the last commit
- [ ] C) Edits the last commit
- [ ] D) Shows the last commit

> [!success]- Answer
> **Correct Answer: B) Creates a new commit that undoes the last commit**
> 
> `git revert` reinstates previous versions and makes a commit.

**Q28.** What flag prevents git revert from opening a text editor?
- [ ] A) --no-editor
- [ ] B) --skip-edit
- [ ] C) --no-edit
- [ ] D) -m

> [!success]- Answer
> **Correct Answer: C) --no-edit**
> 
> `git revert --no-edit HEAD` avoids opening the text editor.

**Q29.** How do you revert a single file from the previous commit?
- [ ] A) git revert HEAD -- file.txt
- [ ] B) git checkout HEAD~1 -- file.txt
- [ ] C) git restore HEAD file.txt
- [ ] D) git undo file.txt

> [!success]- Answer
> **Correct Answer: B) git checkout HEAD~1 -- file.txt**
> 
> Use `git checkout` for single files since `git revert` works on commits, not individual files.

**Q30.** What does `git restore --staged file.txt` do?
- [ ] A) Deletes the file
- [ ] B) Commits the file
- [ ] C) Removes the file from the staging area
- [ ] D) Reverts the file to previous version

> [!success]- Answer
> **Correct Answer: C) Removes the file from the staging area**
> 
> `git restore --staged` unstages files without deleting changes.

---

## Section B: True/False Questions (15 questions × 1 point = 15 points)

**Q31.** A Git commit has four main parts. **T/F**

> [!success]- Answer
> **False** - Git commits have three parts: Commit, Tree, and Blob.

**Q32.** The Tree part of a commit tracks names and locations of files and directories. **T/F**

> [!success]- Answer
> **True** - The Tree is like a dictionary mapping keys to files/directories.

**Q33.** A Blob can only contain text data. **T/F**

> [!success]- Answer
> **False** - A Blob may contain data of any kind.

**Q34.** If two files are identical, their Git hashes will be different. **T/F**

> [!success]- Answer
> **False** - If two files are the same, their hashes are the same.

**Q35.** You need the complete hash to identify a commit. **T/F**

> [!success]- Answer
> **False** - Only need the first 8-10 characters of the hash.

**Q36.** `git log` shows commits from oldest to newest. **T/F**

> [!success]- Answer
> **False** - `git log` shows commits from newest to oldest.

**Q37.** Press 'q' to quit git log and return to the terminal. **T/F**

> [!success]- Answer
> **True** - 'q' quits the log view.

**Q38.** `git log -5` shows the 5 oldest commits. **T/F**

> [!success]- Answer
> **False** - It shows the 5 most recent commits.

**Q39.** The ISO 8601 date format is YYYY-MM-DD. **T/F**

> [!success]- Answer
> **True** - This is the recommended format.

**Q40.** `git diff` with no arguments compares staged files with the last commit. **T/F**

> [!success]- Answer
> **False** - Default `git diff` compares unstaged files with the last commit.

**Q41.** In git diff output, '+' indicates removed lines. **T/F**

> [!success]- Answer
> **False** - '+' indicates added lines; '-' indicates removed lines.

**Q42.** HEAD~1 refers to the latest commit. **T/F**

> [!success]- Answer
> **False** - HEAD is the latest; HEAD~1 is the second most recent.

**Q43.** `git revert` creates a new commit. **T/F**

> [!success]- Answer
> **True** - `git revert` reinstates previous versions and makes a commit.

**Q44.** `git checkout` can be used to revert a single file. **T/F**

> [!success]- Answer
> **True** - Use `git checkout HEAD~1 -- filename` for single files.

**Q45.** `git restore --staged` removes all files from the staging area. **T/F**

> [!success]- Answer
> **True** - Without a filename, it removes all staged files.

---

## Section C: Short Answer Questions (8 questions × 3 points = 24 points)

**Q46.** Describe the three parts of a Git commit and what each part contains.

> [!success]- Answer
> The three parts of a Git commit are:
> 
> **1. Commit**
> - Contains the metadata
> - Includes: author information
> - Includes: log message (commit message)
> - Includes: commit time (when the commit was made)
> 
> **2. Tree**
> - Tracks the names and locations of files and directories in the repo
> - Works like a dictionary
> - Maps keys to files and directories
> - Represents the directory structure
> 
> **3. Blob (Binary Large OBject)**
> - May contain data of any kind
> - Stores a compressed snapshot of a file's contents
> - Contains the actual file data
> - Can be any type of file (text, binary, etc.)

**Q47.** Explain what Git hashes are and why they are useful.

> [!success]- Answer
> **What Git hashes are:**
> - Pseudo-random numbers generated by a hash function
> - Unique identifiers for each commit
> - Long strings like: `b22eb75a82a68b9c0f1c45b9f5a9b7abe281683a`
> - Only need first 8-10 characters for most operations
> 
> **Why they are useful:**
> 1. **Efficient comparison**: Git only needs to compare hashes, not entire files
> 2. **Data sharing**: Hashes allow data sharing between repos
> 3. **Uniqueness**: If two files are the same, their hashes are the same
> 4. **Integrity**: Changes to files result in different hashes
> 5. **Identification**: Each commit can be uniquely identified by its hash
> 
> This makes Git operations fast and reliable.

**Q48.** List five ways to filter or restrict `git log` output.

> [!success]- Answer
> Five ways to filter `git log` output:
> 
> **1. Limit number of commits**
> ```bash
> git log -3  # Show only 3 most recent commits
> ```
> 
> **2. Filter by file**
> ```bash
> git log report.md  # Show only commits affecting this file
> ```
> 
> **3. Filter by start date**
> ```bash
> git log --since='Apr 2 2024'  # Commits since this date
> ```
> 
> **4. Filter by date range**
> ```bash
> git log --since='Apr 2 2024' --until='Apr 11 2024'
> ```
> 
> **5. Combine techniques**
> ```bash
> git log -2 data/file.csv  # Last 2 commits for specific file
> ```
> 
> Additional acceptable date formats: "2 weeks ago", "yesterday", "3 months ago"

**Q49.** Explain the difference between `git diff`, `git diff --staged`, and `git diff HEAD~1 HEAD`.

> [!success]- Answer
> **`git diff` (no arguments)**
> - Compares unstaged files with the last commit
> - Shows changes you've made but haven't staged yet
> - Helps you see what will be staged if you run `git add`
> 
> **`git diff --staged`**
> - Compares staged files with the last commit
> - Shows changes that are staged and ready to commit
> - Helps you review what will be in the next commit
> 
> **`git diff HEAD~1 HEAD`**
> - Compares two specific commits
> - HEAD~1 = second most recent commit
> - HEAD = most recent commit
> - Shows what changed between these two commits
> - You can also use commit hashes: `git diff 35f4b4d 186398f`
> 
> **Summary:**
> - `git diff` = unstaged vs last commit
> - `git diff --staged` = staged vs last commit
> - `git diff HEAD~1 HEAD` = commit vs commit

**Q50.** What is the difference between `git revert` and `git checkout` when undoing changes?

> [!success]- Answer
> **`git revert`**
> - **Purpose**: Undo an entire commit
> - **What it does**: Creates a NEW commit that undoes a previous commit
> - **Scope**: Works on commits, not individual files
> - **Commits**: Automatically creates a commit (unless `-n` flag used)
> - **Syntax**: `git revert HEAD` or `git revert <commit-hash>`
> - **When to use**: When you want to undo all changes from a commit
> 
> **`git checkout`**
> - **Purpose**: Restore a single file from a previous commit
> - **What it does**: Replaces current file with old version
> - **Scope**: Works on individual files
> - **Commits**: Does NOT automatically commit (you must commit manually)
> - **Syntax**: `git checkout HEAD~1 -- filename`
> - **When to use**: When you only want to revert one file, not the whole commit
> 
> **Key difference**: `git revert` works on entire commits and auto-commits; `git checkout` works on single files and requires manual commit.

**Q51.** Explain what HEAD, HEAD~1, and HEAD~2 represent in Git.

> [!success]- Answer
> **HEAD**
> - Represents the state in the latest commit
> - Points to the most recent commit on your current branch
> - This is "where you are now" in the commit history
> 
> **HEAD~1**
> - Represents the second most recent commit
> - One commit before HEAD
> - "One step back" in history
> 
> **HEAD~2**
> - Represents the third most recent commit
> - Two commits before HEAD
> - "Two steps back" in history
> 
> **Pattern**: The number after `~` indicates how many commits back from HEAD
> - HEAD~3 = three commits back
> - HEAD~4 = four commits back
> - And so on...
> 
> **Usage examples:**
> - `git diff HEAD~1 HEAD` - compare second most recent with most recent
> - `git checkout HEAD~1 -- file.txt` - get file from one commit ago
> - `git revert HEAD~2` - undo the commit from two commits ago

**Q52.** Describe three different uses of `git diff` with examples.

> [!success]- Answer
> **Use 1: Compare unstaged file with last commit**
> ```bash
> git diff report.md
> ```
> - Shows changes made to report.md that haven't been staged
> - Helps you see what modifications you've made
> - Useful before deciding whether to stage the file
> 
> **Use 2: Compare staged files with last commit**
> ```bash
> git diff --staged
> ```
> - Shows all changes that are staged and ready to commit
> - Helps you review what will be in the next commit
> - Good final check before committing
> 
> **Use 3: Compare two commits**
> ```bash
> git diff 35f4b4d 186398f
> # OR
> git diff HEAD~1 HEAD
> ```
> - Shows what changed between two specific commits
> - Can use commit hashes or HEAD notation
> - Put most recent hash second
> - Useful for understanding what changed in a particular commit
> 
> All show `+` for added lines and `-` for removed lines.

**Q53.** Explain the difference between `git revert HEAD`, `git revert HEAD --no-edit`, and `git revert HEAD -n`.

> [!success]- Answer
> **`git revert HEAD`**
> - Reverts all files from the last commit
> - Opens a text editor for you to write/edit the commit message
> - Creates a new commit after you save and close the editor
> - Default behavior with full control over commit message
> 
> **`git revert HEAD --no-edit`**
> - Reverts all files from the last commit
> - Does NOT open a text editor
> - Uses a default commit message automatically
> - Creates the commit immediately
> - Faster when you don't need a custom message
> 
> **`git revert HEAD -n`**
> - Reverts all files from the last commit
> - Does NOT create a commit automatically
> - Brings the reverted changes into the staging area
> - You must commit manually later with `git commit`
> - Useful when you want to revert multiple commits or make additional changes before committing
> 
> **Summary:**
> - No flags = editor opens, commit created
> - `--no-edit` = no editor, commit created automatically
> - `-n` = no commit created, changes staged for manual commit

---

## Answer Key & Scoring

### Section A: Multiple Choice (60 points)
1. B  |  2. C  |  3. B  |  4. C  |  5. C  
2. B  |  7. B  |  8. C  |  9. B  |  10. C  
3. B  |  12. C  |  13. B  |  14. C  |  15. B  
4. C  |  17. C  |  18. A  |  19. C  |  20. B  
5. B  |  22. B  |  23. C  |  24. B  |  25. C  
6. B  |  27. B  |  28. C  |  29. B  |  30. C  

### Section B: True/False (15 points)
31. F  |  32. T  |  33. F  |  34. F  |  35. F  
32. F  |  37. T  |  38. F  |  39. T  |  40. F  
33. F  |  42. F  |  43. T  |  44. T  |  45. T  

### Section C: Short Answer (24 points)
- Q46-Q53: See detailed answers in collapsible sections (3 points each)

---

## Quick Reference Guide

### Viewing History
```bash
git log                              # Show all commits
git log -3                           # Show 3 most recent
git log report.md                    # Show commits for one file
git log --since='Apr 2 2024'         # Commits since date
git log --since='2 weeks ago'        # Natural language
git show c27fa856                    # Show specific commit
```

### Comparing Versions
```bash
git diff                             # Unstaged vs last commit
git diff report.md                   # Specific unstaged file
git diff --staged                    # All staged vs last commit
git diff --staged report.md          # Specific staged file
git diff 35f4b4d 186398f            # Between two commits
git diff HEAD~1 HEAD                 # Using HEAD notation
```

### Reverting and Restoring
```bash
git revert HEAD                      # Revert last commit
git revert HEAD --no-edit            # Without editor
git revert HEAD -n                   # Without committing
git checkout HEAD~1 -- file.txt      # Revert single file
git restore --staged file.txt        # Unstage single file
git restore --staged                 # Unstage all files
```

---

## Key Concepts Summary

| Concept | Description |
|---------|-------------|
| **Commit (part)** | Contains metadata (author, message, time) |
| **Tree** | Tracks file/directory names and locations |
| **Blob** | Compressed snapshot of file contents |
| **Hash** | Unique identifier for commits |
| **HEAD** | Pointer to latest commit |
| **HEAD~1** | Second most recent commit |
| **Staging area** | Files prepared for next commit |

### git diff Output Symbols

| Symbol | Meaning | Color |
|--------|---------|-------|
| `+` | Added line | Green |
| `-` | Removed line | Red |
| (none) | Context line | Normal |

---

## Common Mistakes to Avoid

❌ Thinking `git log` shows oldest first (it's newest first!)  
❌ Trying to use full hash when 8-10 characters work  
❌ Confusing unstaged and staged in `git diff`  
❌ Forgetting `--staged` flag when comparing staged files  
❌ Using `git revert` for single files (use `git checkout`)  
❌ Thinking HEAD~1 is the latest commit (it's HEAD)  
❌ Not knowing how to exit git log (press 'q')  
❌ Confusing `+` and `-` in diff output  
❌ Expecting `git revert -n` to auto-commit (it doesn't!)  
❌ Using `git restore` when you mean `git revert`  

---

## Study Strategy (3-Day Plan)

### Day 1: Commit Structure & History
- Learn the three parts of commits (Commit, Tree, Blob)
- Understand Git hashes and their purpose
- Practice `git log` with different filters
- Learn navigation in git log (space, q)
- Try `--since`, `--until`, and `-number` flags
- Practice `git show` for specific commits

### Day 2: Comparing Versions
- Master `git diff` for unstaged changes
- Learn `git diff --staged` for staged changes
- Practice comparing two commits with hashes
- Understand HEAD, HEAD~1, HEAD~2 notation
- Learn to read diff output (`+` and `-`)
- Practice combining different git diff commands

### Day 3: Reverting & Restoring
- Understand `git revert` for commits
- Learn flags: `--no-edit` and `-n`
- Practice `git checkout` for single files
- Master `git restore --staged` for unstaging
- Review differences between revert/checkout/restore
- Complete practice quiz

---

## Pre-Exam Checklist

✅ Know three parts of a commit: Commit, Tree, Blob  
✅ Understand Git hashes (first 8-10 chars sufficient)  
✅ Remember `git log` shows newest first  
✅ Know how to exit git log (press 'q')  
✅ Can restrict log by number, file, or date  
✅ ISO 8601 date format: YYYY-MM-DD  
✅ `git show` displays specific commit details  
✅ `git diff` = unstaged vs last commit  
✅ `git diff --staged` = staged vs last commit  
✅ `+` = added, `-` = removed in diff output  
✅ HEAD = latest, HEAD~1 = second most recent  
✅ `git revert` works on commits, creates new commit  
✅ `git checkout` for single files, no auto-commit  
✅ `git restore --staged` unstages files  

---

## During the Exam

💡 **Commit parts** - Remember: Commit (metadata), Tree (structure), Blob (contents)  
💡 **Hashes** - Only need first 8-10 characters  
💡 **git log order** - Newest to oldest (not oldest to newest!)  
💡 **Exit git log** - Press 'q' to quit  
💡 **Date format** - ISO 8601: YYYY-MM-DD is recommended  
💡 **git diff default** - Compares unstaged with last commit  
💡 **--staged flag** - Needed for comparing staged files  
💡 **Diff symbols** - '+' = added (green), '-' = removed (red)  
💡 **HEAD notation** - HEAD = latest, HEAD~1 = one back  
💡 **revert vs checkout** - revert=commits, checkout=single files  
💡 **restore --staged** - Unstages, doesn't delete changes  

---

#git #version-control #git-log #git-diff #git-revert #commit-history #git-checkout #git-restore #exam-prep