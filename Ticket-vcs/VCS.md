# Version Control System (VCS) - Documentation

<p align="center">
  <img src="https://git-scm.com/images/logos/downloads/Git-Logo-2Color.png" alt="Git Logo" width="250"/>
</p>

---

## Document Information

| Author | Created On | Version | Last Updated By | Last Updated On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | --------------- | --------------- | -------------- | ------------------ |
| Vikas  | 30/08/2026 | v1.0    |                 |                 | Deepak Kushwaha | Faisal/Mohit K | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is VCS](#2-what-is-vcs)
3. [Why VCS is Required](#3-why-vcs-is-required)
4. [Types of VCS](#4-types-of-vcs)
5. [VCS Workflow](#7-vcs-workflow)
6. [Git Workflow Example](#8-git-workflow-example)
7. [Advantages](#9-advantages)
8. [Disadvantages](#10-disadvantages)
9. [Best Practices](#11-best-practices)
10. [Basic Git Commands](#12-basic-git-commands)
11. [Troubleshooting](#13-troubleshooting)
12. [Conclusion](#14-conclusion)
13. [Contact Information](#15-contact-information)
14. [References](#16-references)
15. [FAQs](#17-faqs)

---

# 1. Introduction

A **Version Control System (VCS)** is a tool used to track and manage changes made to files over time.

VCS is commonly used for source code, configuration files, documentation, infrastructure code, and other project files.

It helps teams:

* Track changes.
* Work together.
* Maintain project history.
* Create branches.
* Merge changes.
* Recover previous versions.
* Review changes before merging.

Version control is especially important in software development because multiple developers can work on the same project without losing previous changes.

---

# 2. What is VCS

**VCS stands for Version Control System.**

It records changes made to files and maintains their history.

For example:

```text
Project
   |
   +-- Version 1
   |
   +-- Version 2
   |
   +-- Version 3
   |
   +-- Version 4
```

If a problem occurs in Version 4, the team can inspect previous versions and identify what changed.


---

# 3. Why VCS is Required

VCS provides a controlled way to manage project changes.

| Requirement        | How VCS Helps                                          |
| ------------------ | ------------------------------------------------------ |
| Change Tracking    | Records who changed what and when                      |
| Collaboration      | Allows multiple developers to work together            |
| History            | Maintains previous versions                            |
| Recovery           | Helps restore or inspect earlier versions              |
| Branching          | Allows independent development                         |
| Code Review        | Changes can be reviewed before merging                 |

A VCS also helps identify bugs by comparing changes between versions.

---

# 4. Types of VCS

There are three commonly discussed approaches:

1. Local Version Control System
2. Centralized Version Control System
3. Distributed Version Control System

---

## 4.1 Local Version Control System

A Local VCS stores version history on the local machine.

```text
Developer
    |
    v
Local Repository
    |
    +-- Version 1
    +-- Version 2
    +-- Version 3
```

### Example

* RCS

### Advantages

* Simple.
* Works without a network.
* Useful for individual work.

### Disadvantages

* Limited collaboration.
* History may be lost if the local system fails.
* Not suitable for large development teams.

---

## 4.2 Centralized Version Control System

A **Centralized Version Control System (CVCS)** uses a central server that stores the project's version history.

Examples include:

* SVN
* CVS
* Perforce

The developer normally gets files from the central server and sends changes back to it.

### Advantages

* Simple architecture.
* Central administration.
* Easy to control user access.
* Central location for project history.

### Disadvantages

* Central server can become a single point of failure.
* Network connectivity is important.
* Many operations depend on the central server.

---

## 4.3 Distributed Version Control System

A **Distributed Version Control System (DVCS)** gives each developer a complete repository, including project history.

Examples:

* Git
* Mercurial
* Darcs

Git and Mercurial are distributed systems.

Developers can perform many operations locally without connecting to the remote repository.

### Advantages

* Full repository history is available locally.
* Local commits are fast.
* Developers can work offline for many operations.
* Repository clones provide additional copies of project history.

### Disadvantages

* Requires more local storage than a simple centralized checkout.
* Beginners may need time to understand branches, merges, and remotes.
* Poor repository practices can create unnecessary complexity.


---

# 5. VCS Workflow

A typical VCS workflow is:

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/0f08f653-8064-4800-b669-3348d8b31f2f" />


### Workflow Explanation

| Step | Activity                   |
| ---- | -------------------------- |
| 1    | Create or clone repository |
| 2    | Create a working branch    |
| 3    | Modify project files       |
| 4    | Review changes             |
| 5    | Stage required changes     |
| 6    | Commit changes             |
| 7    | Push branch to remote      |
| 8    | Create Pull Request        |
| 9    | Perform code review        |
| 10   | Merge approved changes     |

---

# 6. Git Workflow Example

Git is a popular DVCS and is commonly used with platforms such as GitHub, GitLab, and Bitbucket.

### Step 1: Clone Repository

```bash
git clone <repository-url>
cd <repository>
```

### Step 2: Create Branch

```bash
git checkout -b feature/my-change
```

Or:

```bash
git switch -c feature/my-change
```

### Step 3: Make Changes

Modify the required files.

### Step 4: Check Changes

```bash
git status
```

View the difference:

```bash
git diff
```

### Step 5: Stage Changes

```bash
git add <file>
```

Or stage all changes:

```bash
git add .
```

### Step 6: Commit

```bash
git commit -m "Add required configuration"
```

### Step 7: Push

```bash
git push origin feature/my-change
```

### Step 8: Pull Request

Create a Pull Request and request code review.

### Step 9: Merge

After approval, merge the branch into the main branch according to the project's process.

---

# 7. Advantages

| Advantage      | Description                                        |
| -------------- | -------------------------------------------------- |
| Change History | Keeps a record of project changes                  |
| Collaboration  | Multiple developers can work together              |
| Branching      | Supports isolated development                      |
| Code Review    | Changes can be reviewed before merging             |
| Recovery       | Previous versions can be inspected                 |
| Traceability   | Changes can be linked to commits                   |

Distributed systems can also perform many operations locally, reducing dependence on network connectivity.

---

# 8. Disadvantages

| Disadvantage        | Description                                              |
| ------------------- | -------------------------------------------------------- |
| Learning Curve      | New users need to learn VCS concepts                     |
| Merge Conflicts     | Changes to the same area can conflict                    |
| Repository Size     | Large repositories may require significant storage       |
| Poor Practices      | Incorrect branching can make history difficult to manage |
| Sensitive Data Risk | Secrets committed to repositories can become exposed     |
| Complexity          | Advanced workflows can be difficult for beginners        |

---

# 9. Best Practices

### Repository

* Keep repositories organized.
* Use meaningful repository names.
* Maintain a clear README.
* Do not commit passwords, API keys, or private keys.
* Add unnecessary files to `.gitignore`.

### Branching

* Use meaningful branch names.
* Keep branches focused on one task.
* Avoid keeping feature branches open unnecessarily long.
* Keep the main branch stable.

### Commits

Use small and meaningful commits.

Good:

```bash
git commit -m "Add nginx configuration"
```

Avoid:

```bash
git commit -m "changes"
```

### Pull Requests

Before merging:

1. Review the changes.
2. Run tests.
3. Resolve conflicts.
4. Obtain required approvals.
5. Merge only after validation passes.

### Security

Never commit:

```text
passwords
private keys
API tokens
cloud credentials
database credentials
```

Use approved secret-management solutions instead.

---

# 10. Basic Git Commands

| Command      | Purpose                                             |
| ------------ | --------------------------------------------------- |
| `git init`   | Create a repository                                 |
| `git clone`  | Copy a remote repository                            |
| `git status` | Show working-tree status                            |
| `git add`    | Stage changes                                       |
| `git commit` | Save changes to local history                       |
| `git push`   | Send commits to remote                              |
| `git pull`   | Fetch and integrate remote changes                  |
| `git fetch`  | Download remote changes without integrating them    |
| `git diff`   | Show changes                                        |
| `git log`    | Show commit history                                 |
| `git branch` | Manage branches                                     |
| `git switch` | Switch/create branches                              |
| `git merge`  | Combine branch histories                            |
| `git rebase` | Reapply commits on another base                     |
| `git stash`  | Temporarily save uncommitted changes                |
| `git revert` | Create a new commit that reverses a previous commit |
| `git reset`  | Move/reset HEAD and optionally staging/worktree     |

---

# 11. Troubleshooting

## 11.1 Merge Conflict

Check:

```bash
git status
```

Open the conflicted files and resolve the conflict.

Then:

```bash
git add <file>
git commit
```

---

## 11.2 Accidentally Committed Changes

First check history:

```bash
git log --oneline
```

For a shared branch, prefer:

```bash
git revert <commit-id>
```

This creates a new commit that reverses the earlier change.

---

## 11.3 Remote Changes Are Available

Download remote information:

```bash
git fetch
```

Review changes:

```bash
git log
git diff
```

Integrate when ready:

```bash
git merge
```

Or use:

```bash
git pull
```

`git pull` generally performs a fetch followed by integration of the fetched changes.

---

## 11.4 Check Current Branch

```bash
git branch
```

Or:

```bash
git status
```

---

# 12. Conclusion

Version Control Systems (VCS) provide a reliable way to manage code changes, maintain version history, and support collaboration among developers.
Using Git with proper branching, meaningful commits, code reviews, and secure repository practices helps maintain code quality and reduces the risk of losing changes.
Overall, VCS is an essential foundation for efficient software development, collaboration, and CI/CD workflows.

---

# 13. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 14. References

| Links                                                                                                                | Topic                           |
| -------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| [Git Official Documentation](https://git-scm.com/docs)                                                               | Git commands and reference      |
| [Git Book - About Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)             | VCS concepts and architectures  |
| [Atlassian - Types of Version Control](https://support.atlassian.com/bitbucket-cloud/docs/types-of-version-control/) | Centralized and distributed VCS |

---

# 15. FAQs

### Q1. What is VCS?

VCS is a system that records and manages changes to project files over time.

### Q2. Why do we use VCS?

To track changes, collaborate with other developers, maintain history, and recover previous versions.

### Q3. What are the main types of VCS?

The main types are:

* Local VCS
* Centralized VCS
* Distributed VCS

### Q4. What is a branch?

A branch is an independent line of development used to make changes without directly modifying the main development line.

### Q5. What is a commit?

A commit records a set of changes in the repository history.

### Q6. What is a Pull Request?

A Pull Request is a request to review and merge changes from one branch into another.

### Q7. What is the difference between `git fetch` and `git pull`?

`git fetch` downloads remote updates without integrating them into the current branch. `git pull` fetches updates and then integrates them according to the configured pull behavior.

