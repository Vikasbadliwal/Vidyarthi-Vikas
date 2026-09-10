# SOP: Common Stack | Version Control | GitFlow Workflow

<img width="204" height="192" alt="GitFlow" src="https://git-scm.com/images/logos/downloads/Git-Icon-1788C.png" />

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer**    |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | ------------------ |
| Vikas      | 07-09-2026     | v1.0        | Vikas               | 07-09-2026         | Deepak Kushwaha | Faisal/Mohit K  | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Why GitFlow?](#2-why-gitflow)
3. [GitFlow Workflow](#3-gitflow-workflow)
4. [Advantages](#4-advantages)
5. [Disadvantages](#5-disadvantages)
6. [Conclusion](#6-conclusion)
7. [Contact Information](#7-contact-information)
8. [References](#8-references)

---

# 1. Introduction

**GitFlow** is a Git branching model that provides a structured approach for managing software development, releases, and hotfixes.

It was introduced by **Vincent Driessen** and uses different branches for different purposes, such as feature development, release preparation, production releases, and urgent production fixes.

GitFlow helps development teams organize their Git branches and maintain a controlled workflow from development to production.

### Main GitFlow Branches

| **Branch**  | **Purpose**                                    |
| ----------- | ---------------------------------------------- |
| `main`      | Contains stable, production-ready code         |
| `develop`   | Contains the latest completed development work |
| `feature/*` | Used to develop individual features            |
| `release/*` | Used to prepare and stabilize a new release    |
| `hotfix/*`  | Used to fix critical production issues         |

---

# 2. Why GitFlow?

GitFlow is used to provide a **structured and predictable branching strategy** for software development teams.

It helps teams:

* Separate development work from production-ready code.
* Develop multiple features independently.
* Prepare releases without blocking ongoing development.
* Handle urgent production fixes separately.
* Maintain a clear history of development and releases.
* Reduce the risk of directly modifying production code.
* Improve collaboration between developers and teams.

### When GitFlow is Useful

GitFlow can be useful for projects that:

* Have scheduled or versioned releases.
* Require a dedicated release stabilization phase.
* Maintain multiple production versions.
* Have separate development, testing, and production activities.
* Need a clearly defined branching strategy.

---

# 3. GitFlow Workflow

GitFlow uses different branch types for different stages of development.

## 3.1 GitFlow Workflow Diagram

```text
                         +----------------+
                         |      main      |
                         |   Production   |
                         +-------+--------+
                                 ^
                                 |
                          Release Merge
                                 |
                         +-------+--------+
                         |    release/*   |
                         | Release Testing|
                         +-------+--------+
                                 ^
                                 |
                          From develop
                                 |
                    +------------+------------+
                    |                         |
                    |                         |
              +-----+------+           +------+------+
              |  develop   |           |   hotfix/*  |
              | Development|           | Production  |
              +-----+------+           |    Fix      |
                    ^                  +------+-------+
                    |                         |
                    |                         |
             Feature Merge              Hotfix Merge
                    |                         |
              +-----+------+                  |
              | feature/*  |------------------+
              | New Feature|
              +------------+
```

---

## 3.2 Main Branch

The `main` branch contains **stable and production-ready code**.

Developers generally should not directly commit development changes to this branch.

Example:

```bash
git checkout main
```

The `main` branch represents the code currently released or deployed to production.

---

## 3.3 Develop Branch

The `develop` branch contains the latest integrated development work.

Feature branches are normally created from `develop` and merged back into `develop` after completion.

Example:

```bash
git checkout develop
```

---

## 3.4 Feature Branch

A `feature/*` branch is created when a developer starts working on a new feature.

Example:

```bash
git checkout develop
git checkout -b feature/login
```

After completing the feature:

```bash
git add .
git commit -m "Add login feature"
```

The feature branch is then pushed to the remote repository:

```bash
git push -u origin feature/login
```

After code review and testing, the feature branch is merged into `develop`.

```text
develop
   |
   +---- feature/login
   |
   +---- feature/payment
   |
   +---- feature/dashboard
```

---

## 3.5 Release Branch

When the features planned for a release are completed, a release branch can be created from `develop`.

Example:

```bash
git checkout develop
git checkout -b release/1.0.0
```

The release branch is used for:

* Final testing
* Bug fixing
* Version updates
* Release preparation
* Documentation updates

Once the release is ready, it is merged into `main` and usually back into `develop` so that release fixes are not lost.

---

## 3.6 Hotfix Branch

A `hotfix/*` branch is created from `main` when an urgent production issue needs to be fixed.

Example:

```bash
git checkout main
git checkout -b hotfix/1.0.1
```

After fixing and testing the issue, the hotfix is merged into `main`.

The changes should also be merged back into `develop` so that the fix is included in future development.

```text
main
 |
 +---- hotfix/1.0.1
          |
          +---- Fix production issue
          |
          +---- Merge into main
          |
          +---- Merge into develop
```

---

## 3.7 Complete GitFlow Process

The typical GitFlow process is:

```text
                Start Development
                       |
                       v
                    develop
                       |
                       v
                Create feature/*
                       |
                       v
               Develop & Test
                       |
                       v
                Code Review
                       |
                       v
             Merge into develop
                       |
                       v
             Release Preparation
                       |
                       v
                 release/*
                       |
                       v
             Testing & Bug Fixes
                       |
                       v
              Merge into main
                       |
                       v
                 Production
                       |
                       v
                Release Complete
```

For an urgent production issue:

```text
main
 |
 v
hotfix/*
 |
 v
Fix + Test
 |
 +-----------> main
 |
 +-----------> develop
```

---

## 3.8 Example GitFlow Commands

### Create a feature branch

```bash
git checkout develop
git pull origin develop
git checkout -b feature/user-login
```

### Push feature branch

```bash
git push -u origin feature/user-login
```

### Create a release branch

```bash
git checkout develop
git checkout -b release/1.0.0
```

### Create a hotfix branch

```bash
git checkout main
git checkout -b hotfix/1.0.1
```

> [!NOTE]
> The exact Git commands may vary depending on the team's branching policies and whether merge requests/pull requests are used.

---

# 4. Advantages

| **Advantage**        | **Description**                                                              |
| -------------------- | ---------------------------------------------------------------------------- |
| Structured workflow  | Provides clearly defined branches for development, releases, and hotfixes.   |
| Better organization  | Separates feature development from production code.                          |
| Release management   | Allows teams to stabilize and test releases before production deployment.    |
| Parallel development | Multiple developers can work on different features independently.            |
| Production stability | Protects the production branch from unfinished development work.             |
| Emergency fixes      | Hotfix branches allow critical production issues to be resolved quickly.     |
| Clear history        | Branch structure makes development and release history easier to understand. |
| Team collaboration   | Provides a common branching strategy for development and release activities. |

---

# 5. Disadvantages

| **Disadvantage**            | **Description**                                                                             |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| Complex workflow            | GitFlow has more branches and rules than simpler branching models.                          |
| Branch management           | Maintaining multiple long-lived branches can require additional effort.                     |
| Merge conflicts             | Long-running feature or release branches can increase the possibility of conflicts.         |
| Slower integration          | Changes may take longer to reach production compared with continuous integration workflows. |
| Not ideal for every project | Small teams and continuously deployed applications may find GitFlow unnecessarily complex.  |
| Additional maintenance      | Release and hotfix branches need to be properly merged and maintained.                      |
| Learning curve              | New team members need to understand the purpose of each branch and its workflow.            |

---

# 6. Conclusion

GitFlow provides a structured branching strategy for managing software development, releases, and production fixes.

By separating **feature, development, release, main, and hotfix branches**, teams can manage code changes in a controlled and organized manner.

GitFlow is particularly useful for projects that follow **scheduled releases and require a dedicated release-management process**. However, teams should select a branching strategy based on their project requirements, deployment model, and development practices.

---

# 7. Contact Information

| **Name**       | **Email**                                                                               |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 8. References

| **Topic**                                                                                    | **Description**                                                |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| [Git Documentation](https://git-scm.com/doc)                                                 | Official Git documentation and reference.                      |
| [Git Branch Documentation](https://git-scm.com/docs/git-branch)                              | Official documentation for Git branches.                       |
| [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/) | Original GitFlow branching model proposed by Vincent Driessen. |
| [GitHub Documentation](https://docs.github.com/en/get-started/using-git/about-git)           | Git and repository workflow documentation.                     |
