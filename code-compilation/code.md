# Python CI Checks | Attendance & Notification | Local POC

<p align="center">
  <img width="90" height="90" alt="Python CI" src="https://img.icons8.com/fluency/96/python.png" />
</p>

---

## Author Information

| Author         | Created On | Version | Last Updated By | Last Edited On | Pre Reviewer | L0 Reviewer              | L1 Reviewer | L2 Reviewer          |
| -------------- | ---------- | ------- | --------------- | -------------- | ------------ | ------------------------ | ----------- | -------------------- |
| Vikas          | 01-10-2026 | 1.0     | Vikas           | 01-10-2026     | -            | Deepak Kushwaha / Ayushi | Mohit Kumar | Mahesh Kumar / Varun |

---

## Table of Contents

* [1. Introduction](#1-introduction)
* [2. Objective](#2-objective)
* [3. Local CI Workflow](#3-local-ci-workflow)
* [4. Prerequisites](#4-prerequisites)
* [5. Clone Repositories](#5-clone-repositories)
* [6. Attendance API POC](#6-attendance-api-poc)
* [7. Notification Worker POC](#7-notification-worker-poc)
* [8. CI Check Summary](#8-ci-check-summary)
* [9. Expected Result](#9-expected-result)
* [10. Best Practices](#10-best-practices)
* [11. Advantages](#11-advantages)
* [12. Recommendation](#12-recommendation)
* [13. Conclusion](#13-conclusion)
* [14. Contact Information](#14-contact-information)
* [15. References](#15-references)

---

## 1. Introduction

This POC demonstrates **local Python CI checks** for the Attendance API and Notification Worker projects.

The checks validate Python source code before it is considered ready for further development or deployment.

| Application         | Repository                                                    |
| ------------------- | ------------------------------------------------------------- |
| Attendance API      | `https://github.com/OT-MICROSERVICES/attendance-api.git`      |
| Notification Worker | `https://github.com/OT-MICROSERVICES/notification-worker.git` |

---

## 2. Objective

The objective is to perform the following checks locally:

1. Install project dependencies.
2. Check Python code compilation/syntax.
3. Run configured lint/code-quality checks.
4. Run formatting checks where configured.
5. Execute available automated tests.
6. Record PASS/FAIL results.

---

## 3. Local CI Workflow

```text
Source Code
    |
    +----------------------+
    |                      |
    v                      v
Attendance API      Notification Worker
    |                      |
    +----------+-----------+
               |
               v
       Install Dependencies
               |
               v
        Compilation Check
               |
               v
        Code Quality Check
               |
               v
         Format Check
               |
               v
           Run Tests
               |
               v
          PASS / FAIL
```

---

## 4. Prerequisites

Verify Python:

```bash
python3 --version
```

Verify Git:

```bash
git --version
```

For Attendance API, verify Poetry:

```bash
poetry --version
```

---

## 5. Clone Repositories

Create a workspace:

```bash
mkdir -p ~/python-ci-poc
cd ~/python-ci-poc
```

Clone Attendance:

```bash
git clone https://github.com/OT-MICROSERVICES/attendance-api.git
```

Clone Notification:

```bash
git clone https://github.com/OT-MICROSERVICES/notification-worker.git
```

Verify:

```bash
ls
```

Expected:

```text
attendance-api
notification-worker
```

---

## 6. Attendance API POC

Go to the project:

```bash
cd ~/python-ci-poc/attendance-api
```

Inspect:

```bash
ls -la
```

The project uses Poetry for dependency management and provides project-level configuration such as `pyproject.toml`, `poetry.lock`, `pytest.ini`, and `Makefile`.

### 6.1 Install Dependencies

```bash
poetry install
```

Check environment:

```bash
poetry env info
```

### 6.2 Compilation Check

```bash
poetry run python -m compileall .
```

This checks Python files for syntax/compilation errors.

### 6.3 Test Check

```bash
poetry run pytest
```

Optional coverage:

```bash
poetry run pytest --cov=.
```

### 7.4 Code Quality / Formatting

Check the project's Makefile:

```bash
cat Makefile
```

Run the project's configured commands where available.

Example:

```bash
make fmt
```

If Pylint is configured:

```bash
poetry run pylint .
```

> Use the commands already defined by the repository instead of adding unnecessary tools.

---

## 7. Notification Worker POC

Go to the project:

```bash
cd ~/python-ci-poc/notification-worker
```

Inspect:

```bash
ls -la
find . -maxdepth 2 -type f | sort
```

### 7.1 Setup Environment

If the project uses `requirements.txt`:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

If it uses Poetry:

```bash
poetry install
```

### 7.2 Compilation Check

For a virtual environment:

```bash
python -m compileall .
```

For Poetry:

```bash
poetry run python -m compileall .
```

### 8.3 Test Check

For a virtual environment:

```bash
pytest
```

For Poetry:

```bash
poetry run pytest
```

### 7.4 Code Quality / Formatting

Check the project's existing configuration:

```bash
find . -maxdepth 2 -type f | sort
```

If a Makefile exists:

```bash
cat Makefile
```

Run the configured lint/format commands.

---

## 8. CI Check Summary

| Check          | Purpose                                 |
| -------------- | --------------------------------------- |
| `compileall`   | Detect Python syntax/compilation errors |
| Pytest         | Execute automated tests                 |
| Pylint/Ruff    | Detect code-quality issues              |
| Black/Makefile | Check code formatting                   |
| Poetry/pip     | Install project dependencies            |

The exact command should follow the tooling already configured in each repository.

---

## 9. Expected Result

Successful local CI checks should result in:

```text
Compilation     PASS
Code Quality    PASS
Formatting      PASS
Tests           PASS
-----------------------
Final Result    PASS
```

If any check fails:

```text
Final Result: FAIL
```

Fix the reported issue and run the checks again.

---

## 10. Best Practices

* Use an isolated Python environment.
* Follow the dependency manager already used by the project.
* Run compilation checks before testing.
* Keep automated tests as part of validation.
* Follow existing linting and formatting configuration.
* Do not hard-code passwords, tokens, or secrets.
* Keep CI checks reproducible.
* Record important PASS/FAIL results.
* Avoid unnecessary tools or configuration changes.

---

## 11. Advantages

* Detects syntax errors early.
* Improves code quality.
* Finds test failures before deployment.
* Reduces manual validation.
* Provides repeatable local checks.
* Gives quick developer feedback.

---

## 12. Recommendation

Use the following local validation sequence:

```text
Install Dependencies
        |
        v
Compile Python Code
        |
        v
Lint / Code Quality
        |
        v
Format Check
        |
        v
Run Tests
        |
        v
Review Result
```

Prefer the existing tools and commands already configured in each repository.

---

## 13. Conclusion

This POC demonstrates how the **Attendance API** and **Notification Worker** Python applications can be validated locally using compilation, code-quality, formatting, and testing checks.

The complete process runs directly on the local machine.

**Docker, Jenkins, GitHub Actions, and other CI servers are not part of this POC.**

---

## 14. Contact Information

**Owner:** Vikas
**Purpose:** Python CI Checks – Attendance & Notification
**Execution:** Local Environment

---

## 15. References

* Python: `https://docs.python.org/3/`
* Pytest: `https://docs.pytest.org/`
* Poetry: `https://python-poetry.org/docs/`
