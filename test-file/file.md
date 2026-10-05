# POC: Python CI Checks | Code Compilation

## Author Information

| **Author** | **Created On** | **Version** | **L0 Reviewer**    | **L1 Reviewer** | **L2 Reviewer** |
| ---------------- | -------------------- | ----------------- | ------------------------ | --------------------- | --------------------- |
| Vikas            | 01-10-2026           | 1.0               | Deepak Kushwaha / Ayushi | Mohit Kumar           | Mahesh Kumar / Varun  |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Pre-requisites](#2-pre-requisites)
3. [Step-by-Step Setup Guide](#3-step-by-step-setup-guide)
   - [3.1 Verify Application Structure](#31-verify-application-structure)
   - [3.2 Install Dependencies](#32-install-dependencies)
   - [3.3 Configure Compilation Checks](#33-configure-compilation-checks)
   - [3.4 Execute Code Compilation](#34-execute-code-compilation)
   - [3.5 Verify Bytecode and Error Handling](#35-verify-bytecode-and-error-handling)
4. [Conclusion](#4-conclusion)
5. [Contact Information](#5-contact-information)
6. [References](#6-references)

---

# 1. Introduction

This Proof of Concept (POC) demonstrates **Code Compilation and Syntax Verification** for Python microservices (Attendance API & Notification Worker) using Python's built-in `compileall` utility.
It covers project verification, dependency installation, bytecode generation (`.pyc`), and automated syntax error detection during the CI build.

---

# 2. Pre-requisites

The following components are required to execute Python code compilation:

| Requirement            | Purpose                                                                          |
| :--------------------- | :------------------------------------------------------------------------------- |
| **Python 3.11+** | Python runtime environment containing the standard`compileall` module.         |
| **Git**          | Distributed version control system used to clone service repositories.           |
| **Poetry**       | Dependency and environment manager used for the Attendance API service.          |
| **pip / venv**   | Standard Python packaging tools used for the Notification Worker service.        |
| **compileall**   | Built-in Python library that byte-compiles all`.py` files in a directory tree. |

---

# 3. Step-by-Step Setup Guide

### 3.1 Verify Application Structure

Navigate to the project root directory:

```bash
cd ~/python-ci-poc
```

Verify that both microservice directories exist:

```bash
ls -la attendance-api notification-worker
```

Verify that Python source files exist in each repository:

```bash
find attendance-api -name "*.py" | head -n 5
find notification-worker -name "*.py" | head -n 5
```

Because `compileall` is included directly in the Python standard library, zero third-party compiler installation is required.

<img width="1902" height="951" alt="image" src="https://github.com/user-attachments/assets/7606ad5e-b1f5-4637-8689-aa1e64aede17" />

---

### 3.2 Install Dependencies

Install clean dependencies for each microservice before compiling:

#### A. Attendance API (Poetry)

```bash
cd ~/python-ci-poc/attendance-api
poetry install
```

Check the command exit status:

```bash
echo $?
```

Expected output:

```text
0
```

#### B. Notification Worker (Virtualenv / pip)

```bash
cd ~/python-ci-poc/notification-worker
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Check the command exit status:

```bash
echo $?
```

Expected output:

```text
0
```

<img width="1853" height="116" alt="image" src="https://github.com/user-attachments/assets/ae39e158-1573-4a4b-a7cc-c9fd59ad3e8b" />

---

### 3.3 Configure Compilation Checks

The compilation check uses `compileall` with CI-optimized flags:

| Flag     | Purpose                                                                   |
| :------- | :------------------------------------------------------------------------ |
| `-q`   | Quiet mode: Only outputs files that contain syntax errors.                |
| `-f`   | Force compilation: Recompiles even if`.pyc` files already exist.        |
| `-j 0` | Parallel compilation: Utilizes all available CPU cores for maximum speed. |
| `.`    | Target directory: Recursively checks the current project root.            |

Standard CI Compilation Command:

```bash
python -m compileall -q -f -j 0 .
```

This configuration ensures immediate fail-fast feedback in the CI pipeline before running unit tests or security scans.

<img width="1867" height="863" alt="image" src="https://github.com/user-attachments/assets/1895a490-8fc2-40aa-a34a-4d4773ef83e6" />

---

### 3.4 Execute Code Compilation

Run the compilation command on both microservices:

#### A. Attendance API:

```bash
cd ~/python-ci-poc/attendance-api
poetry run python -m compileall -q -f -j 0 .
```

Check exit status:

```bash
echo $?
```

Expected output:

```text
0
```

#### B. Notification Worker:

```bash
cd ~/python-ci-poc/notification-worker
python -m compileall -q -f -j 0 .
```

Check exit status:

```bash
echo $?
```

Expected output:

```text
0
```

Both services compiled cleanly with zero syntax or compilation errors.

<img width="1542" height="449" alt="image" src="https://github.com/user-attachments/assets/3beab124-599f-4c93-bd93-92e106b8879e" />

---

### 3.5 Verify Bytecode and Error Handling

#### A. Verify Bytecode Generation (`.pyc`)

Confirm that byte-compiled files are created in `__pycache__` directories:

```bash
find . -name "*.pyc" | head -n 5
```

Expected output:

```text
./services/__pycache__/attendance_service.cpython-311.pyc
./services/__pycache__/notification_service.cpython-311.pyc
```

#### B. Test Syntax Error Detection

Simulate a syntax error in a test file to verify that CI fails properly:

```bash
echo "def broken_syntax(:" > test_broken.py
python -m compileall test_broken.py
```

Expected output:

```text
*** Error compiling 'test_broken.py'...
SyntaxError: invalid syntax (test_broken.py, line 1)
```

Check the command exit status:

```bash
echo $?
```

Expected output:

```text
1
```

The non-zero exit status (`1`) proves that compilation errors automatically stop the CI pipeline.

#### Compilation Results Summary:

| Microservice                  | Target Files | Syntax Errors |   Bytecode Status   |  CI Gate Status  |
| :---------------------------- | :-----------: | :-----------: | :------------------: | :--------------: |
| **Attendance API**      | All (`.py`) |  **0**  | Generated (`.pyc`) | **Passed** |
| **Notification Worker** | All (`.py`) |  **0**  | Generated (`.pyc`) | **Passed** |

<img width="1728" height="889" alt="image" src="https://github.com/user-attachments/assets/9769fdf0-c0f4-4739-838e-b19764d58911" />

---

# 4. Conclusion

This Proof of Concept successfully demonstrated automated **Code Compilation and Syntax Verification** for the Attendance API and Notification Worker microservices.
The check validated all Python files, confirmed zero syntax defects, verified `.pyc` bytecode creation, and proved that syntax errors return a non-zero exit code to block broken builds.

**Chosen Tool:**
We choose **`python -m compileall`** as our code compilation tool for Python CI. It is built into Python, requires zero third-party installation, supports multi-core parallel execution (`-j 0`), and provides an instant fail-fast quality gate.

---

# 5. Contact Information

|      Name      |           Role           |        Email Address        |
| :-------------: | :----------------------: | :--------------------------: |
| **Vikas** | Author / DevOps Engineer | vikas.snaatak@mygurukulam.co |

---

# 6. References

| Reference                          | Link                                                                |
| :--------------------------------- | :------------------------------------------------------------------ |
| **compileall Documentation** | [compileall Docs](https://docs.python.org/3/library/compileall.html) |
| **py_compile Documentation** | [py_compile Docs](https://docs.python.org/3/library/py_compile.html) |
| **Python Standard Library**  | [Python Docs](https://docs.python.org/3/)                            |
| **Poetry Documentation**     | [Poetry Docs](https://python-poetry.org/docs/)                       |
