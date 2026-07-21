
# GIT & GITHUB

## What is Git?

- Git is a version control system used to track changes in source code during software development.
- It is a software tool, not a framework.
- Git works like a time machine for your code, allowing you to save and manage different versions of your project.
- It helps developers track changes, collaborate with others, and restore previous versions when needed.
- With Git, you can easily view the history of your code and revert to older versions if required.

#In simple words: 

 
Git is a version control system that tracks code changes, stores different versions of a project, and allows developers to collaborate and recover previous versions when necessary. 

#Example 

Imagine you are creating a calculator application. 

#Version 1 (Day 1) 

CODE - print("Calculator App") 

You save this code in Git. 

#Version 2 (Day 2) 

You add addition functionality: 

CODE –  

print("Calculator App") 

print("Addition Feature") 

You save the changes in Git again. 

#Version 3 (Day 3) 

You add subtraction functionality: 

CODE –  

print("Calculator App") 

print("Addition Feature") 

print("Subtraction Feature") 

You save these changes in Git. 

# What is GitHub?

- GitHub is a cloud-based platform that hosts Git repositories online.
- Git is the version control tool that you use on your local computer to track changes in your code.
- GitHub is where you store, manage, and share your Git projects on the cloud.
- It enables developers to collaborate with others, review code, track issues, and manage projects efficiently.
- GitHub also provides features such as pull requests, branching, and version history to support team-based software development.

## Example

Suppose you create a website on your laptop:

1. You use Git to track all changes made to the website.
2. You upload the project to GitHub.
3. Your teammates can access the repository from GitHub.
4. They can make changes, review your code, report issues, and collaborate on the project.
5. GitHub keeps all versions of the project and helps manage teamwork effectively.

## In One Sentence

**GitHub is a cloud-based platform that hosts Git repositories, making it easy to store, share, manage, and collaborate on software projects.**

# GIT & GITHUB

## Example: Git Version Tracking

### Version 1 (Day 1)

You create a simple Calculator App.

```python
print("Calculator App")
```

You save this version in Git.

### Version 2 (Day 2)

You add an addition feature.

```python
print("Calculator App")
print("Addition Feature")
```

You save the changes in Git again.

### Version 3 (Day 3)

You add subtraction functionality.

```python
print("Calculator App")
print("Addition Feature")
print("Subtraction Feature")
```

You save these changes in Git.

---

# What is GitHub?

- **GitHub** is a cloud-based platform that hosts **Git repositories online**.
- **Git** is the version control tool that you use on your local computer to track changes in your code.
- **GitHub** is where you store, manage, and share your Git projects on the cloud.
- It enables developers to **collaborate with others**, **review code**, **track issues**, and **manage projects** efficiently.
- GitHub also provides features such as **Pull Requests**, **Branching**, and **Version History** to support team-based software development.


## Example

Suppose you create a website on your laptop:

1. You use **Git** to track all changes made to the website.
2. You upload the project to **GitHub**.
3. Your teammates can access the repository from GitHub.
4. They can make changes, review your code, report issues, and collaborate on the project.
5. GitHub keeps all versions of the project and helps manage teamwork effectively.

## In One Sentence

**GitHub is a cloud-based platform that hosts Git repositories, making it easy to store, share, manage, and collaborate on software projects.**

---

# Simple Explanation for Git & GitHub

Think of **Git** as the tool that tracks changes in your code on your computer, and **GitHub** as the online platform where you store and share those Git projects with others.

---

# What is a Repository (Repo)?

- A **Repository (Repo)** is a project folder that is tracked and managed by **Git**.
- It contains your project's **source code, documentation, configuration files, and complete history of changes**.
- A repository can be stored **locally** on your computer or **remotely** on platforms such as GitHub, GitLab, or Bitbucket.
- Repositories help developers organize projects, track modifications, and collaborate efficiently.

## Simple Explanation

A repository is like a **storage space for your project** where Git keeps your files and records every change made over time.

## Example

Suppose you are developing a **Calculator App**:

```text
Calculator-App/
│
├── main.py
├── README.md
└── requirements.txt
```

This project folder becomes a **Git repository** when you initialize Git in it.

## In One Sentence

A **repository (repo)** is a project folder managed by Git that stores your code, project files, and the complete history of changes.

---

# Important Git Commands

## Check Repository Status

```bash
git status
```

Displays the current state of your working directory and staging area.

## Configure Git Username

```bash
git config --global user.name "<user_id>"
```

Sets your username globally.

## Configure Git Email

```bash
git config --global user.email "<mail>"
```

Sets your email globally.

## Initialize Git Repository

```bash
git init
```

Creates a new Git repository.

---

# The `.git` Folder

When you run:

```bash
git init
```

Git automatically creates a hidden **.git** folder.

### Why is it important?

- The `.git` folder is the **heart (backbone)** of a Git repository.
- It stores:
  - Commit history
  - Branch information
  - Configuration settings
  - Version tracking data
- Often referred to as Git's **database**.
- Without it, the directory is just a normal folder.

---

# Git Branches

Display all local branches:

```bash
git branch
```

---

# Markdown Files (.md)

Files ending with `.md` are called **Markdown Files**.

Example:

```text
README.md
```

Used for:

- Project documentation
- Instructions
- Repository information

---

# Add Files to Staging Area

Add all files:

```bash
git add .
```

Add a single file:

```bash
git add filename
```

Add multiple files:

```bash
git add file1 file2 file3
```

---

# Commit Changes

```bash
git commit -m "comments"
```

Saves staged changes permanently in Git.

---

# File States

```text
Untracked (U)
      ↓
Staged
      ↓
Committed
```

Or

```text
Modified (M)
      ↓
Staged
      ↓
Committed
```

## Workflow

```text
Working Directory
        ↓
    git add .
        ↓
   Staging Area
        ↓
git commit -m "message"
        ↓
 Git Repository (.git)
```

---

# Create a File Using Terminal

```bash
echo "content inside file" > file_name
```

---

# HEAD in Git

**HEAD** is a pointer that refers to the latest commit in the currently active branch.

### Key Points

- On the main branch, HEAD points to the latest commit.
- New branches are created from the commit where HEAD currently points.
- HEAD automatically moves forward after each commit.
- HEAD helps Git identify your current position.

## Example

```text
A --- B --- C (HEAD → main)
```

Here:

- C is the latest commit.
- HEAD points to C.

Create a new branch:

```bash
git branch feature
```

```text
A --- B --- C (HEAD → main, feature)
```

## In One Sentence

**HEAD is a pointer to the latest commit of the currently active branch.**

---

# Git Logs

View complete commit history:

```bash
git log
```

Short version:

```bash
git log --oneline
```

Or:

```bash
git reflog
```

---

# API Secrets and Sensitive Files

## `.env` File

Used to store:

- API Keys
- Database Credentials
- Access Tokens
- Passwords

Example:

```env
API_KEY=your_api_key
DB_PASSWORD=your_password
```

---

## `.gitignore` File

Used to tell Git which files or folders should not be tracked.

Example:

```gitignore
.env
node_modules/
*.log
```

### Result

- `.env` is ignored.
- `node_modules` is ignored.
- All `.log` files are ignored.

### In One Sentence

- `.env` stores sensitive information.
- `.gitignore` prevents sensitive files from being committed.

---

# Tracking Empty Folders in Git

Git does not track empty folders.

## `.gitkeep`

A `.gitkeep` file is commonly used to keep empty folders in Git.

Example:

```text
uploads/
└── .gitkeep
```

> `.gitkeep` is a convention, not an official Git feature.

---

# Branch Operations

## Switch Branch

```bash
git switch branch_name
```

Or

```bash
git checkout branch_name
```

## Create New Branch

```bash
git switch -c feature_branch
```

Or

```bash
git checkout -b feature_branch
```

## Abort Merge Conflict

```bash
git merge --abort
```

## After Resolving Conflicts

```bash
git add .
git commit
```

---

# What is Merge?

Merge combines changes from one branch into another branch.

## Before Merge

```text
main
A --- B

feature
A --- B --- C --- D
```

Run:

```bash
git checkout main
git merge feature
```

## After Merge

```text
A --- B --- C --- D
```

---

# What is Git Rebase?

Git Rebase reapplies commits from one branch onto another branch and creates a cleaner history.

## Before Rebase

```text
main
A --- B --- C

feature
A --- B --- D --- E
```

## After Rebase

```text
A --- B --- C --- D' --- E'
```

## Command

```bash
git checkout feature
git rebase main
```

---

# Undo Last Commit

View commits:

```bash
git log --oneline
```

Or:

```bash
git reflog
```

Reset to previous commit:

```bash
git reset --hard HEAD~1
```

Or:

```bash
git reset --hard <commit_id>
```

## Remove Staged Changes

```bash
git reset
```

---

# Git Diff

```bash
git diff
```

Used to compare changes between:

- Files
- Commits
- Branches
- Staging Area

---

# Git Cherry-Pick

Cherry-pick copies a specific commit from one branch to another.

## Command

```bash
git cherry-pick <commit-id>
```

### Example

Before:

```text
main
A --- B

feature
A --- B --- C --- D
```

Bring only commit C to main:

```bash
git checkout main
git cherry-pick C
```

After:

```text
main
A --- B --- C'

feature
A --- B --- C --- D
```

---

# Git Stashing

Stashing temporarily saves uncommitted changes.

## Create Stash

```bash
git stash push -m "comment"
```

Example:

```bash
git stash push -m "Work in progress on login feature"
```

## View Stashes

```bash
git stash list
```

Example:

```text
stash@{0}: On main: Work in progress on login feature
stash@{1}: On feature: Bug fix changes
```

## Apply Latest Stash

```bash
git stash apply
```

## Apply Specific Stash

```bash
git stash apply stash@{1}
```

## Apply and Remove

```bash
git stash pop
```

## Delete Specific Stash

```bash
git stash drop stash@{0}
```

## Delete All Stashes

```bash
git stash clear
```

## In One Sentence

**Git Stash lets you temporarily save uncommitted work and restore it later.**

---

# Remote Repository

Connect local repository to GitHub:

```bash
git remote add origin <repository_url>
```

Check remote repositories:

```bash
git remote -v
```

Rename branch:

```bash
git branch -m branch_name
```

---

# What is Origin?

- `origin` is the default name of a remote repository.
- It is the destination where you push code and pull updates.
- Usually points to GitHub.

---

# Frequently Asked Git Commands

```bash
git status
git init
git add .
git commit -m "message"
git log
git log --oneline
git diff
git branch
git checkout
git switch
git merge
git rebase
git stash
git fetch
git pull
git push
git clone
git remote -v
```

---

# Git Workflow

```text
Working Directory
        ↓
      git add
        ↓
    Staging Area
        ↓
     git commit
        ↓
   Local Repository
        ↓
      git push
        ↓
 GitHub Repository
```

---

# Frequently Asked Interview Questions

## 1. What is Git?

**Answer:**  
Git is a distributed version control system that tracks changes in source code.

## 2. Difference Between Git and GitHub?

**Git:** Local version control tool.  
**GitHub:** Cloud platform for Git repositories.

## 3. What is a Repository?

A project folder tracked by Git.

## 4. What is a Commit?

A snapshot of your project at a specific point in time.

```bash
git commit -m "Added login feature"
```

## 5. What is HEAD?

HEAD is a pointer to the latest commit of the active branch.

## 6. Difference Between Git and SVN?

- Git is distributed.
- SVN is centralized.
- Git supports offline work.

## 7. What is a Branch?

An independent line of development.

## 8. Create a Branch

```bash
git branch feature
```

Or:

```bash
git checkout -b feature
```

## 9. Switch Branches

```bash
git switch feature
```

## 10. What is Merge?

Combines changes from one branch into another.

```bash
git merge feature
```

## 11. What is Merge Conflict?

Occurs when Git cannot automatically combine changes.

## 12. What is Rebase?

Moves commits to another branch, creating cleaner history.

```bash
git rebase main
```

## 13. Merge vs Rebase

### Merge

- Preserves history
- Creates merge commit

### Rebase

- Linear history
- No merge commit

## 14. What is Git Stash?

Temporarily saves uncommitted work.

```bash
git stash
```

## 15. View Stashes

```bash
git stash list
```

## 16. Difference Between Apply and Pop

- `apply` restores stash and keeps it.
- `pop` restores stash and removes it.

## 17. What is Origin?

Default remote repository name.

## 18. Connect Local Repo to GitHub

```bash
git remote add origin <repository_url>
```

## 19. Check Remote Repositories

```bash
git remote -v
```

## 20. Push Code

```bash
git push origin main
```

## 21. Pull Code

```bash
git pull origin main
```

## 22. What is GitHub?

A cloud platform for hosting Git repositories.

## 23. What is a Pull Request (PR)?

A request to merge code after review.

## 24. Why are Pull Requests Important?

- Code review
- Collaboration
- Quality control
- Discussion before merging

## 25. What is Forking?

Creates a personal copy of another repository.

## 26. What is GitHub Actions?

GitHub's CI/CD automation platform.

## 27. Undo a Commit

```bash
git reset HEAD~1
```

## 28. See Who Modified a Line

```bash
git blame filename
```

## 29. Compare Changes

```bash
git diff
```

## 30. Remove File from Tracking But Keep Locally

```bash
git rm --cached filename
```

## 31. What is Cherry-Pick?

Copies a specific commit to another branch.

```bash
git cherry-pick <commit-id>
```

## 32. What is .gitignore?

Specifies files Git should not track.

## 33. Why Add .env to .gitignore?

Because it may contain:

- API Keys
- Database Credentials
- Tokens
- Passwords

