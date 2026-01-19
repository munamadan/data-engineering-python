## Overview
This chapter introduces Git branches, explaining why they're useful, how to create and manage them, how to compare branches, and how to merge branches together.

## Course Context

### What We Will Cover
- Branches
- Remotes
- Conflicts

### What You Should Know (Prerequisites)
- How Git stores data
- How to create repos
- How to make commits
- How to compare versions
- How to revert versions

## Branches

### What is a Branch?
**Branch** = an individual version of a repo

**Key concept:** Git uses branches to systematically track multiple versions of files

**In each branch:**
- Some files might be the same
- Others might be different
- Some may not exist at all

### Why Use Branches?

**1. Separation of concerns:**
- **Live system** (main branch)
  - Works as expected
  - Default branch = main
- **Feature development** (feature branches)
  - Might encounter issues during development and testing
  - Doesn't affect the live system

**2. Collaboration benefits:**
- Multiple developers can work on a project simultaneously
- Compare the state of a repo between branches
- Combine contents, pushing new features to a live system

**3. Organization:**
- Each branch should have a specific purpose

### Branching Workflows

**Common scenarios:**
1. **Branching off** - Creating a new branch from an existing branch
2. **Feature development** - Working on new features in isolation
3. **Merging back into main** - Integrating completed features
4. **Fixing a bug** - Creating bug-fix branches

## Identifying Branches

### Listing All Branches

```bash
git branch
```

**Output:**
```
  main
* ai-assistant
```

**Note:** `*` = current branch

### Switching Between Branches

```bash
git switch main
# Output: Switched to branch 'main'
```

### Creating a New Branch

**Method 1: Create, then switch**
```bash
# Create a new branch called speed-test
git branch speed-test

# Move to the speed-test branch
git switch speed-test
# Output: Switched to branch 'speed-test'
```

**Method 2: Create and switch in one command**
```bash
# Create a new branch called speed-test and switch to it
git switch -c speed-test
# Output: Switched to a new branch 'speed-test'
```

> [!info] Best Practice
> Use `git switch -c branch-name` to create and switch in one step

### Terminology
- **Creating a new branch** = "branching off"
- **Creating speed-test from main** = "branching off main"

## Modifying and Comparing Branches

### Diff Recap

| Command | Function |
|---------|----------|
| `git diff` | Show changes between all unstaged files and the latest commit |
| `git diff report.md` | Show changes between an unstaged file and the latest commit |
| `git diff --staged` | Show changes between all staged files and the latest commit |
| `git diff --staged report.md` | Show changes between a staged file and the latest commit |
| `git diff 35f4b4d 186398f` | Show changes between two commits using hashes |
| `git diff HEAD~1 HEAD~2` | Show changes between two commits using HEAD |

### Comparing Branches

```bash
git diff main summary-statistics
```

**Purpose:** Compare the state between two branches

**Output:** Shows differences between the branches (can be large)

### Navigating Large Git Outputs

Git diff between branches can produce large outputs:
- Press **space** to progress through
- Press **q** to exit

### Renaming a Branch

**Scenario:** Need to rename a branch for clarity

```bash
git branch
# Output:
#   main
# * feature_dev

# Rename feature_dev to chatbot
git branch -m feature_dev chatbot
```

**Check the result:**
```bash
git branch
# Output:
#   main
# * chatbot
```

> [!info] Rename Flag
> `git branch -m` where `-m` stands for "move" or "rename"

### Deleting a Branch

**Why delete branches:**
- Large projects can have many branches
- Delete branches once we are finished with them

**Delete a merged branch:**
```bash
git branch -d chatbot
# Output: Deleted branch chatbot (was 3edb989).
```

### Deleting an Unmerged Branch

**Problem:** If chatbot hasn't been merged to main, `git branch -d` produces an error:

```
error: The branch 'chatbot' is not fully merged.
If you are sure you want to delete it, run 'git branch -D chatbot'.
```

**Solution:** Use uppercase `-D` flag:
```bash
git branch -D chatbot
# Output: Deleted branch chatbot (was 3edb989).
```

> [!warning] Deletion Warning
> Difficult, but not impossible, to recover deleted branches.
> Be sure we don't need the branch any more before deleting!

## Merging Branches

### The Purpose of Merging

**Each branch should have a particular purpose:**
- Developing a new feature
- Debugging an error

**Once the task is complete:**
- We incorporate the changes into production
- Typically the main branch - "ground truth"

### Source and Destination

**When merging two branches:**
- The last commits from each branch are called **parent commits**
- **Source** - the branch we want to merge FROM
- **Destination** - the branch we want to merge INTO

**Example:** When merging ai-assistant into main:
- ai-assistant = source
- main = destination

### Merging Branches

**Standard approach:**
```bash
# 1. Move to the destination branch
git switch main

# 2. Merge the source branch
git merge source
```

**Example:**
```bash
# From main, to merge ai-assistant into main
git merge ai-assistant
```

**Alternative syntax:**
```bash
# From another branch, specify both source and destination
git merge source destination
git merge ai-assistant main
```

### Git Merge Output

**Key concepts from merge output:**

1. **Parent commits** - The last commits from each branch being merged

2. **Linear commit history** - When you branched off main to create ai-assistant

3. **Fast-forward** - When Git can simply point main to the last commit in the ai-assistant branch
   - Happens when there are no conflicting changes
   - Simple "moving the pointer forward"

## Command Summary Tables

### Branch Management Commands

| Command | Function |
|---------|----------|
| `git branch` | List all branches |
| `git switch branch-name` | Switch to a branch |
| `git switch -c new-branch` | Create and switch to new branch |
| `git branch new-branch` | Create a new branch (don't switch) |
| `git branch -m old new` | Rename branch from old to new |
| `git branch -d branch` | Delete merged branch |
| `git branch -D branch` | Delete unmerged branch (force) |

### Branch Comparison and Merging

| Command | Function |
|---------|----------|
| `git diff main chatbot` | Compare state of main and chatbot branches |
| `git merge source` | Merge source into current branch |
| `git merge source dest` | Merge source into dest from any branch |

## Key Concepts

### Branch Characteristics
- Individual version of a repo
- Files can be same, different, or non-existent between branches
- Default branch is typically called "main"
- Each branch should have specific purpose

### Branch Benefits
- ✅ Work on features without affecting live system
- ✅ Multiple developers work simultaneously
- ✅ Compare states between branches
- ✅ Safely test and develop
- ✅ Organize work by purpose

### Merge Concepts
- **Parent commits**: Last commits from each branch
- **Source branch**: Where changes come from
- **Destination branch**: Where changes go to
- **Fast-forward**: Simple pointer movement when no conflicts
- **Ground truth**: Typically the main branch

## Important Warnings

> [!warning] Deleting Branches
> - Use `-d` for merged branches (safe)
> - Use `-D` for unmerged branches (destructive)
> - Difficult to recover deleted branches
> - Be certain before deleting!

> [!warning] Current Branch
> - Can't delete the branch you're currently on
> - Switch to another branch first
> - Watch for the `*` indicator in `git branch` output

## Best Practices

1. **Branch naming**: Use descriptive names (e.g., `feature-login`, `bugfix-header`)
2. **Purpose**: Each branch for one specific task
3. **Regular merging**: Don't let branches diverge too much
4. **Clean up**: Delete branches after merging
5. **Check before delete**: Ensure branch is merged unless you're certain

---

#git #branches #git-merge #git-switch #version-control #collaboration

# Chapter 3: Branches - Practice Exam

**Course:** Intermediate Git  
**Chapter:** Introduction to Branches  
**Total Points:** 100  
**Passing Score:** 70/100 (70%)  
**Time Limit:** 60 minutes

---

## Section A: Multiple Choice Questions (30 questions × 2 points = 60 points)

### Topic 1: Branch Basics (10 questions)

**Q1.** What is a branch in Git?
- [ ] A) A type of commit
- [ ] B) An individual version of a repo
- [ ] C) A remote server
- [ ] D) A file directory

> [!success]- Answer
> **Correct Answer: B) An individual version of a repo**
> 
> Branch = an individual version of a repo

**Q2.** What is the default branch typically called?
- [ ] A) master
- [ ] B) primary
- [ ] C) main
- [ ] D) default

> [!success]- Answer
> **Correct Answer: C) main**
> 
> The chapter states "Default branch = main"

**Q3.** Between branches, what can we say about files?
- [ ] A) All files must be the same
- [ ] B) All files must be different
- [ ] C) Some might be same, others different, some may not exist at all
- [ ] D) Files cannot be compared between branches

> [!success]- Answer
> **Correct Answer: C) Some might be same, others different, some may not exist at all**
> 
> This is explicitly stated as a characteristic of branches.

**Q4.** What is the main advantage of using branches for feature development?
- [ ] A) Faster commits
- [ ] B) Smaller file sizes
- [ ] C) Doesn't affect the live system
- [ ] D) Automatic backups

> [!success]- Answer
> **Correct Answer: C) Doesn't affect the live system**
> 
> Feature development in branches "Doesn't affect the live system"

**Q5.** What should each branch have?
- [ ] A) The same name as main
- [ ] B) At least 10 commits
- [ ] C) A specific purpose
- [ ] D) Multiple developers

> [!success]- Answer
> **Correct Answer: C) A specific purpose**
> 
> "Each branch should have a specific purpose"

**Q6.** Can multiple developers work on a project simultaneously using branches?
- [ ] A) No, only one developer at a time
- [ ] B) Yes, branches enable simultaneous work
- [ ] C) Only on small projects
- [ ] D) Only with special permissions

> [!success]- Answer
> **Correct Answer: B) Yes, branches enable simultaneous work**
> 
> "Multiple developers can work on a project simultaneously" is listed as a benefit.

**Q7.** What does "branching off" mean?
- [ ] A) Deleting a branch
- [ ] B) Creating a new branch
- [ ] C) Merging branches
- [ ] D) Switching branches

> [!success]- Answer
> **Correct Answer: B) Creating a new branch**
> 
> Terminology: "Creating a new branch = 'branching off'"

**Q8.** What does "branching off main" mean?
- [ ] A) Deleting the main branch
- [ ] B) Renaming main
- [ ] C) Creating a new branch from main
- [ ] D) Merging into main

> [!success]- Answer
> **Correct Answer: C) Creating a new branch from main**
> 
> "Creating speed-test from main = 'branching off main'"

**Q9.** Which branch is typically considered the "ground truth"?
- [ ] A) The oldest branch
- [ ] B) The main branch
- [ ] C) The newest branch
- [ ] D) Any feature branch

> [!success]- Answer
> **Correct Answer: B) The main branch**
> 
> Main branch is "typically the main branch - 'ground truth'"

**Q10.** What topics are covered in this Intermediate Git chapter?
- [ ] A) Only branches
- [ ] B) Branches, Remotes, and Conflicts
- [ ] C) Only merging
- [ ] D) Installation and setup

> [!success]- Answer
> **Correct Answer: B) Branches, Remotes, and Conflicts**
> 
> These three topics are listed in "What we will cover"

### Topic 2: Branch Commands (10 questions)

**Q11.** What command lists all branches?
- [ ] A) git list
- [ ] B) git show branches
- [ ] C) git branch
- [ ] D) git branches

> [!success]- Answer
> **Correct Answer: C) git branch**
> 
> `git branch` lists all branches in the repository.

**Q12.** In the output of `git branch`, what does the `*` indicate?
- [ ] A) The oldest branch
- [ ] B) The current branch
- [ ] C) A deleted branch
- [ ] D) An error

> [!success]- Answer
> **Correct Answer: B) The current branch**
> 
> `* = current branch` as shown in the output example.

**Q13.** What command switches to a different branch?
- [ ] A) git change
- [ ] B) git checkout
- [ ] C) git switch
- [ ] D) git move

> [!success]- Answer
> **Correct Answer: C) git switch**
> 
> `git switch` is the command used to switch between branches.

**Q14.** What does `git switch main` do?
- [ ] A) Creates main branch
- [ ] B) Deletes main branch
- [ ] C) Switches to main branch
- [ ] D) Renames current branch to main

> [!success]- Answer
> **Correct Answer: C) Switches to main branch**
> 
> This command switches you to the main branch.

**Q15.** How do you create a new branch called "speed-test"?
- [ ] A) git create speed-test
- [ ] B) git new speed-test
- [ ] C) git branch speed-test
- [ ] D) git add speed-test

> [!success]- Answer
> **Correct Answer: C) git branch speed-test**
> 
> `git branch speed-test` creates a new branch.

**Q16.** What does `git switch -c speed-test` do?
- [ ] A) Only creates speed-test branch
- [ ] B) Only switches to speed-test
- [ ] C) Creates speed-test and switches to it
- [ ] D) Deletes speed-test branch

> [!success]- Answer
> **Correct Answer: C) Creates speed-test and switches to it**
> 
> The `-c` flag creates a new branch and switches to it in one command.

**Q17.** What flag is used to rename a branch?
- [ ] A) -r
- [ ] B) -n
- [ ] C) -m
- [ ] D) -c

> [!success]- Answer
> **Correct Answer: C) -m**
> 
> `git branch -m` renames a branch (m = move/rename).

**Q18.** How do you rename branch "feature_dev" to "chatbot"?
- [ ] A) git rename feature_dev chatbot
- [ ] B) git branch -m feature_dev chatbot
- [ ] C) git switch -m feature_dev chatbot
- [ ] D) git branch -r feature_dev chatbot

> [!success]- Answer
> **Correct Answer: B) git branch -m feature_dev chatbot**
> 
> Use `git branch -m old_name new_name` to rename.

**Q19.** What flag deletes a branch that has been merged?
- [ ] A) -D
- [ ] B) -d
- [ ] C) -r
- [ ] D) -x

> [!success]- Answer
> **Correct Answer: B) -d**
> 
> Lowercase `-d` deletes merged branches safely.

**Q20.** What flag deletes a branch that has NOT been merged?
- [ ] A) -d
- [ ] B) -f
- [ ] C) -D
- [ ] D) -force

> [!success]- Answer
> **Correct Answer: C) -D**
> 
> Uppercase `-D` force-deletes unmerged branches.

### Topic 3: Comparing and Merging Branches (10 questions)

**Q21.** What does `git diff main summary-statistics` do?
- [ ] A) Merges the branches
- [ ] B) Compares the state between the two branches
- [ ] C) Deletes both branches
- [ ] D) Creates a new branch

> [!success]- Answer
> **Correct Answer: B) Compares the state between the two branches**
> 
> `git diff` between branches shows the differences between them.

**Q22.** When viewing large git diff outputs, how do you exit?
- [ ] A) Press Enter
- [ ] B) Press Esc
- [ ] C) Press q
- [ ] D) Press Ctrl+C

> [!success]- Answer
> **Correct Answer: C) Press q**
> 
> Press 'q' to exit, same as with git log.

**Q23.** When viewing large git diff outputs, how do you progress through?
- [ ] A) Press Enter
- [ ] B) Press space
- [ ] C) Press down arrow
- [ ] D) Type "next"

> [!success]- Answer
> **Correct Answer: B) Press space**
> 
> Press space to progress through large outputs.

**Q24.** Is it difficult to recover deleted branches?
- [ ] A) Impossible to recover
- [ ] B) Very easy to recover
- [ ] C) Difficult, but not impossible
- [ ] D) Automatic recovery

> [!success]- Answer
> **Correct Answer: C) Difficult, but not impossible**
> 
> The chapter warns: "Difficult, but not impossible, to recover deleted branches"

**Q25.** In a merge, what is the "source" branch?
- [ ] A) The branch we want to merge INTO
- [ ] B) The branch we want to merge FROM
- [ ] C) Always the main branch
- [ ] D) The oldest branch

> [!success]- Answer
> **Correct Answer: B) The branch we want to merge FROM**
> 
> Source = the branch we want to merge FROM

**Q26.** In a merge, what is the "destination" branch?
- [ ] A) The branch we want to merge FROM
- [ ] B) The branch we want to merge INTO
- [ ] C) Always a feature branch
- [ ] D) The newest branch

> [!success]- Answer
> **Correct Answer: B) The branch we want to merge INTO**
> 
> Destination = the branch we want to merge INTO

**Q27.** When merging ai-assistant into main, which is the source?
- [ ] A) main
- [ ] B) ai-assistant
- [ ] C) Both equally
- [ ] D) Neither

> [!success]- Answer
> **Correct Answer: B) ai-assistant**
> 
> When merging ai-assistant INTO main: ai-assistant = source, main = destination

**Q28.** What are "parent commits" in a merge?
- [ ] A) The first commits ever made
- [ ] B) Commits from the main branch only
- [ ] C) The last commits from each branch being merged
- [ ] D) Deleted commits

> [!success]- Answer
> **Correct Answer: C) The last commits from each branch being merged**
> 
> Parent commits = the last commits from each branch

**Q29.** What is a "fast-forward" merge?
- [ ] A) A very quick merge
- [ ] B) When Git points the destination to the last commit of source
- [ ] C) A failed merge
- [ ] D) Merging multiple branches at once

> [!success]- Answer
> **Correct Answer: B) When Git points the destination to the last commit of source**
> 
> Fast-forward: "point main to the last commit in the ai-assistant branch"

**Q30.** To merge ai-assistant into main from the main branch, what command do you use?
- [ ] A) git switch ai-assistant
- [ ] B) git merge main ai-assistant
- [ ] C) git merge ai-assistant
- [ ] D) git branch merge ai-assistant

> [!success]- Answer
> **Correct Answer: C) git merge ai-assistant**
> 
> From main branch: `git merge ai-assistant` merges ai-assistant into main

---

## Section B: True/False Questions (15 questions × 1 point = 15 points)

**Q31.** A branch is an individual version of a repository. **T/F**

> [!success]- Answer
> **True** - This is the definition of a branch in Git.

**Q32.** In branches, all files must be exactly the same. **T/F**

> [!success]- Answer
> **False** - Files can be same, different, or non-existent between branches.

**Q33.** The default branch is typically called "main". **T/F**

> [!success]- Answer
> **True** - Main is the standard default branch name.

**Q34.** Each branch should have a specific purpose. **T/F**

> [!success]- Answer
> **True** - This is a best practice emphasized in the chapter.

**Q35.** Feature development in branches can affect the live system. **T/F**

> [!success]- Answer
> **False** - Feature development in branches "Doesn't affect the live system"

**Q36.** Multiple developers cannot work simultaneously using branches. **T/F**

> [!success]- Answer
> **False** - Branches specifically enable multiple developers to work simultaneously.

**Q37.** The `*` in `git branch` output indicates the current branch. **T/F**

> [!success]- Answer
> **True** - The asterisk marks which branch you're currently on.

**Q38.** `git switch -c` creates a new branch and switches to it. **T/F**

> [!success]- Answer
> **True** - The `-c` flag combines creation and switching.

**Q39.** `git branch -m` is used to delete a branch. **T/F**

> [!success]- Answer
> **False** - `-m` is used to rename (move), not delete. Use `-d` or `-D` to delete.

**Q40.** You should use `-d` to delete an unmerged branch. **T/F**

> [!success]- Answer
> **False** - Use `-d` for merged branches, `-D` for unmerged branches.

**Q41.** It's easy to recover deleted branches. **T/F**

> [!success]- Answer
> **False** - It's "Difficult, but not impossible" to recover deleted branches.

**Q42.** Press 'q' to exit large git diff outputs. **T/F**

> [!success]- Answer
> **True** - Same navigation as git log.

**Q43.** In a merge, the source branch is the branch we merge INTO. **T/F**

> [!success]- Answer
> **False** - Source is the branch we merge FROM; destination is what we merge INTO.

**Q44.** Parent commits are the last commits from each branch being merged. **T/F**

> [!success]- Answer
> **True** - This is the definition of parent commits in a merge.

**Q45.** A fast-forward merge happens when Git can simply move the pointer. **T/F**

> [!success]- Answer
> **True** - Fast-forward means pointing the destination to the source's last commit.

---

## Section C: Short Answer Questions (8 questions × 3 points = 24 points)

**Q46.** List four benefits of using branches mentioned in the chapter.

> [!success]- Answer
> Four benefits of using branches:
> 
> 1. **Work without affecting live system**
>    - Feature development doesn't impact the working production code
>    - Can test and develop safely
> 
> 2. **Multiple developers can work simultaneously**
>    - Different people can work on different features at the same time
>    - Enables parallel development
> 
> 3. **Compare the state of a repo between branches**
>    - Can see differences between branches
>    - Use `git diff` to compare branch states
> 
> 4. **Combine contents, pushing new features to live system**
>    - Can merge completed work into production
>    - Systematic way to incorporate changes
> 
> Additionally: Each branch can have a specific purpose for organization.

**Q47.** Explain the difference between `git branch speed-test` and `git switch -c speed-test`.

> [!success]- Answer
> **`git branch speed-test`**
> - Creates a new branch called speed-test
> - Does NOT switch to the new branch
> - You remain on your current branch
> - Need to run `git switch speed-test` separately to move to it
> - Two-step process
> 
> **`git switch -c speed-test`**
> - Creates a new branch called speed-test
> - Automatically switches to the new branch
> - Single command that does both actions
> - More efficient for typical workflow
> - Output: "Switched to a new branch 'speed-test'"
> 
> **Best practice**: Use `git switch -c` to create and switch in one step.

**Q48.** Explain the difference between `git branch -d` and `git branch -D` when deleting branches.

> [!success]- Answer
> **`git branch -d chatbot` (lowercase d)**
> - **Safe deletion** for merged branches
> - Deletes a branch that has been merged into another branch
> - Git checks if branch is merged before deleting
> - Protects against accidental data loss
> - Use when branch's work is already incorporated
> 
> **`git branch -D chatbot` (uppercase D)**
> - **Force deletion** for unmerged branches
> - Deletes a branch even if NOT merged
> - Git will show error with `-d` for unmerged branches
> - Error message: "The branch 'chatbot' is not fully merged"
> - Use when you're certain you don't need the branch
> - **Warning**: Difficult (but not impossible) to recover deleted branches
> 
> **When to use each:**
> - `-d`: Default, safe option after merging
> - `-D`: Only when absolutely sure you don't need the unmerged changes

**Q49.** What are "source" and "destination" branches in a merge? Provide an example.

> [!success]- Answer
> **Definitions:**
> 
> **Source branch:**
> - The branch we want to merge FROM
> - Contains the changes we want to incorporate
> - The branch being merged in
> 
> **Destination branch:**
> - The branch we want to merge INTO
> - Receives the changes from source
> - Where the combined code will live
> 
> **Example:**
> When merging ai-assistant into main:
> - **ai-assistant** = source (we're taking changes FROM here)
> - **main** = destination (we're putting changes INTO here)
> 
> **Workflow:**
> ```bash
> git switch main              # Move to destination
> git merge ai-assistant       # Merge source into destination
> ```
> 
> **Think of it as**: Source provides, destination receives.

**Q50.** Explain what "parent commits" and "fast-forward" mean in the context of merging.

> [!success]- Answer
> **Parent Commits:**
> - The last commits from each branch being merged
> - When merging two branches, each branch has a "last commit"
> - These last commits are called parent commits
> - They represent the state of each branch at merge time
> 
> **Fast-Forward:**
> - A type of merge that happens when commit history is linear
> - Git can simply "point main to the last commit in the source branch"
> - No new merge commit is created
> - Just moves the branch pointer forward
> - Happens when:
>   - You branched off main to create feature branch
>   - Feature branch has new commits
>   - Main hasn't changed since branching
>   - No conflicts exist
> 
> **Analogy**: Instead of combining two paths, you're just extending one path forward (hence "fast-forward").

**Q51.** Describe the two-step process for merging ai-assistant into main, assuming you're on a different branch.

> [!success]- Answer
> The two-step process for merging:
> 
> **Step 1: Move to the destination branch**
> ```bash
> git switch main
> ```
> - Switch to the branch you want to merge INTO (destination)
> - In this case, main is the destination
> - Output: "Switched to branch 'main'"
> 
> **Step 2: Merge the source branch**
> ```bash
> git merge ai-assistant
> ```
> - Merge the source branch into current branch (main)
> - ai-assistant is the source
> - Git will perform the merge and show output
> 
> **Alternative (one command from any branch):**
> ```bash
> git merge ai-assistant main
> ```
> - Specifies both source and destination
> - Can be run from any branch
> - Format: `git merge source destination`

**Q52.** Why should you be careful before deleting a branch? What does the chapter warn about recovery?

> [!success]- Answer
> **Why be careful:**
> 
> 1. **Risk of data loss**
>    - Deleting an unmerged branch loses all its unique changes
>    - Changes not in other branches will be gone
> 
> 2. **Difficult recovery**
>    - The chapter explicitly warns: "Difficult, but not impossible, to recover deleted branches"
>    - Recovery is not guaranteed
>    - Requires advanced Git knowledge
> 
> 3. **Check merge status**
>    - Git protects you with `-d` (requires branch to be merged)
>    - `-D` bypasses this protection
>    - Always verify branch is merged unless you're certain
> 
> **Best practices:**
> - ✅ Use `git branch -d` by default (safe)
> - ✅ Verify work is merged before deleting
> - ✅ Only use `-D` when absolutely certain
> - ✅ Ask: "Do we need this branch anymore?" before deleting
> - ❌ Don't delete unmerged branches casually

**Q53.** List the prerequisites (what you should know) before starting this Intermediate Git chapter.

> [!success]- Answer
> Prerequisites for Intermediate Git - Chapter 3:
> 
> 1. **How Git stores data**
>    - Understanding of commits, trees, and blobs
>    - Knowledge of Git's internal structure
> 
> 2. **How to create repos**
>    - Using `git init`
>    - Understanding what a repository is
> 
> 3. **How to make commits**
>    - Using `git add` and `git commit`
>    - Understanding the staging process
> 
> 4. **How to compare versions**
>    - Using `git diff`
>    - Comparing files and commits
> 
> 5. **How to revert versions**
>    - Using `git revert`, `git checkout`, `git restore`
>    - Understanding how to undo changes
> 
> These topics were covered in the Introduction to Git course (Chapters 1-2).

---

## Answer Key & Scoring

### Section A: Multiple Choice (60 points)
1. B  |  2. C  |  3. C  |  4. C  |  5. C  
2. B  |  7. B  |  8. C  |  9. B  |  10. B  
3. C  |  12. B  |  13. C  |  14. C  |  15. C  
4. C  |  17. C  |  18. B  |  19. B  |  20. C  
5. B  |  22. C  |  23. B  |  24. C  |  25. B  
6. B  |  27. B  |  28. C  |  29. B  |  30. C  

### Section B: True/False (15 points)
31. T  |  32. F  |  33. T  |  34. T  |  35. F  
32. F  |  37. T  |  38. T  |  39. F  |  40. F  
33. F  |  42. T  |  43. F  |  44. T  |  45. T  

### Section C: Short Answer (24 points)
- Q46-Q53: See detailed answers in collapsible sections (3 points each)

---

## Quick Reference Guide

### Branch Management
```bash
git branch                           # List all branches
git branch new-branch                # Create new branch
git switch branch-name               # Switch to branch
git switch -c new-branch             # Create and switch
git branch -m old new                # Rename branch
git branch -d branch                 # Delete merged branch
git branch -D branch                 # Force delete unmerged branch
```

### Comparing and Merging
```bash
git diff branch1 branch2             # Compare two branches
git switch main                      # Move to destination
git merge feature                    # Merge feature into main
git merge feature main               # Merge from any branch
```

### Navigation (same as git log)
- **space** - Progress through output
- **q** - Quit/exit

---

## Key Concepts Summary

| Concept | Description |
|---------|-------------|
| **Branch** | Individual version of a repo |
| **Main** | Default branch, "ground truth" |
| **Branching off** | Creating a new branch |
| **Current branch** | Marked with `*` in git branch output |
| **Source** | Branch we merge FROM |
| **Destination** | Branch we merge INTO |
| **Parent commits** | Last commits from each branch in merge |
| **Fast-forward** | Merge by moving pointer (linear history) |

### Flag Reference

| Flag | Command | Purpose |
|------|---------|---------|
| `-c` | `git switch -c` | Create and switch to branch |
| `-m` | `git branch -m` | Rename (move) branch |
| `-d` | `git branch -d` | Delete merged branch (safe) |
| `-D` | `git branch -D` | Force delete unmerged branch |

---

## Common Mistakes to Avoid

❌ Forgetting which branch you're on before making commits  
❌ Using `-D` casually (should verify branch isn't needed)  
❌ Not switching to destination before merging  
❌ Confusing source and destination in merges  
❌ Trying to delete the branch you're currently on  
❌ Deleting unmerged branches without confirmation  
❌ Not checking `git branch` output for current branch (`*`)  
❌ Using `git branch` instead of `git switch -c` for new branches  
❌ Forgetting that deleted branches are difficult to recover  

---

## Study Strategy (3-Day Plan)

### Day 1: Understanding Branches
- Learn what branches are and why they're useful
- Understand branch characteristics (same/different/non-existent files)
- Study benefits: isolation, collaboration, comparison
- Learn terminology: "branching off", "ground truth"
- Practice listing branches with `git branch`

### Day 2: Branch Operations
- Master `git switch` for changing branches
- Learn `git switch -c` for creating and switching
- Practice `git branch -m` for renaming
- Understand deletion: `-d` vs `-D`
- Study the recovery warning
- Compare branches with `git diff`

### Day 3: Merging
- Understand source vs destination
- Learn parent commits concept
- Master merge workflow: switch then merge
- Understand fast-forward merges
- Practice merge commands
- Complete practice quiz

---

## Pre-Exam Checklist

✅ Know what a branch is (individual version of repo)  
✅ Remember default branch is "main"  
✅ Understand files can differ between branches  
✅ Know benefits: isolation, collaboration, comparison  
✅ `git branch` lists branches, `*` = current  
✅ `git switch` changes branches  
✅ `git switch -c` creates and switches  
✅ `-m` renames, `-d` deletes merged, `-D` force deletes  
✅ Difficult to recover deleted branches  
✅ `git diff branch1 branch2` compares branches  
✅ Source = merge FROM, Destination = merge INTO  
✅ Switch to destination first, then merge source  
✅ Parent commits = last commits from each branch  
✅ Fast-forward = pointer movement in linear history  

---

## During the Exam

💡 **Branch definition** - Individual version of a repo  
💡 **Default branch** - Called "main"  
💡 **Current branch** - Marked with `*` in output  
💡 **Create and switch** - Use `git switch -c`  
💡 **Rename flag** - `-m` (think "move")  
💡 **Delete flags** - `-d` safe, `-D` force  
💡 **Recovery warning** - Difficult but not impossible  
💡 **Comparing** - `git diff branch1 branch2`  
💡 **Merge direction** - Source FROM, destination INTO  
💡 **Merge workflow** - Switch to destination, then merge source  
💡 **Navigation** - space to progress, q to quit  
💡 **Fast-forward** - Simple pointer movement  

---

#git #branches #git-merge #git-switch #version-control #collaboration #intermediate-git #exam-prep