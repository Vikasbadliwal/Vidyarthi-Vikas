# PYTHON | VIRTUAL ENVIRONMENT | PYTHON VIRTUAL ENVIRONMENT Documentation

---

# Author Table

| **Author**    | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| vikas         | 24-08-2026     | 1.0         |                     |                    | <L0 Reviewer>   | <L1 Reviewer>   | <L2 Reviewer>   |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Python Virtual Environment](#2-what-is-python-virtual-environment)
3. [Why Python Virtual Environment is Required](#3-why-python-virtual-environment-is-required)
4. [Python Virtual Environment Workflow](#4-python-virtual-environment-workflow)

   * [4.1 Workflow Diagram](#41-workflow-diagram)
5. [Features of Python Virtual Environment](#5-features-of-python-virtual-environment)
6. [Virtual Environment Tools](#6-virtual-environment-tools)
7. [Tool Comparison](#7-tool-comparison)
8. [Advantages and Disadvantages](#8-advantages-and-disadvantages)
9. [Best Practices](#9-best-practices)
10. [Operational Commands](#10-operational-commands)

    * [10.1 Creation](#101-creation)
    * [10.2 Activation](#102-activation)
    * [10.3 Package Installation](#103-package-installation)
    * [10.4 Freezing Dependencies](#104-freezing-dependencies)
    * [10.5 Deactivation](#105-deactivation)
    * [10.6 Deletion](#106-deletion)
11. [Troubleshooting](#11-troubleshooting)
12. [Recommendation / Conclusion](#12-recommendation--conclusion)
13. [FAQs](#13-faqs)
14. [Contact Information](#14-contact-information)
15. [References](#15-references)

---

# 1. Introduction

Python applications commonly require multiple external packages and libraries.

Different applications may require different versions of the same package. Installing all packages globally can therefore create dependency conflicts and make application environments difficult to reproduce.

A **Python Virtual Environment** provides an isolated Python environment for an individual project.

It allows developers and DevOps teams to:

* Maintain project-specific dependencies.
* Install different package versions for different applications.
* Avoid modifying the system Python environment.
* Recreate application dependencies consistently.
* Simplify development, testing, and deployment.

This documentation explains the purpose, features, workflow, tools, operational commands, best practices, and troubleshooting procedures for Python virtual environments.

---

# 2. What is Python Virtual Environment

A Python Virtual Environment is an isolated directory containing a Python environment and its project-specific packages.

The standard Python `venv` module can be used to create a virtual environment.

Example:

```bash
python3 -m venv venv
```

This creates a directory:

```text
venv/
```

The environment can then be activated:

```bash
source venv/bin/activate
```

After activation, packages installed using `pip` are installed into the virtual environment instead of the system-wide Python environment.

### Example Project Structure

```text
myproject/
├── app.py
├── requirements.txt
├── .gitignore
└── venv/
    ├── bin/
    ├── include/
    ├── lib/
    └── pyvenv.cfg
```

The virtual environment provides isolation between the application and the system Python installation.

---

# 3. Why Python Virtual Environment is Required

Python virtual environments are required to maintain **dependency isolation and reproducibility**.

### 3.1 Dependency Isolation

Different applications may require different package versions.

Example:

```text
Application A
    requests==2.28.x

Application B
    requests==2.32.x
```

A virtual environment allows both applications to maintain their required versions independently.

### 3.2 Avoid System Python Modification

Packages can be installed inside the project's virtual environment without modifying the system-wide Python installation.

### 3.3 Reproducibility

Dependencies can be recorded using:

```bash
pip freeze > requirements.txt
```

Another environment can then install the same dependencies:

```bash
pip install -r requirements.txt
```

### 3.4 Easier Troubleshooting

Because dependencies are isolated, package conflicts can be identified and resolved within the application environment.

---

# 4. Python Virtual Environment Workflow

The standard Python virtual environment workflow is:

```text
Developer
    |
    v
Project Source Code
    |
    v
Create Virtual Environment
    |
    v
Activate Environment
    |
    v
Install Dependencies
    |
    v
Run / Test Application
    |
    v
Freeze Dependencies
    |
    v
requirements.txt
    |
    v
CI/CD Pipeline
    |
    v
Create Clean Environment
    |
    v
Install requirements.txt
    |
    v
Build / Test / Deploy
```

## 4.1 Workflow Diagram

<details>
<summary>Click to Expand Python Virtual Environment Workflow Diagram</summary>

```text
                    Python Project
                         |
                         v
              +----------------------+
              | Create Virtual Env   |
              | python3 -m venv venv |
              +----------+-----------+
                         |
                         v
              +----------------------+
              | Activate Environment |
              | source venv/bin/     |
              | activate             |
              +----------+-----------+
                         |
                         v
              +----------------------+
              | Install Packages     |
              | pip install ...      |
              +----------+-----------+
                         |
                         v
              +----------------------+
              | Test Application     |
              +----------+-----------+
                         |
                         v
              +----------------------+
              | Freeze Dependencies  |
              | pip freeze >         |
              | requirements.txt     |
              +----------+-----------+
                         |
                         v
                  CI/CD Pipeline
                         |
                         v
              +----------------------+
              | Create Clean venv    |
              +----------+-----------+
                         |
                         v
              +----------------------+
              | Install requirements |
              +----------+-----------+
                         |
                         v
                  Build / Test
                         |
                         v
                     Deploy
```

</details>

---

# 5. Features of Python Virtual Environment

| **Feature**                 | **Description**                                                                      |
| --------------------------- | ------------------------------------------------------------------------------------ |
| Dependency Isolation        | Keeps project packages isolated from the system Python environment.                  |
| Package Version Management  | Allows applications to use different versions of packages.                           |
| Reproducibility             | Dependencies can be recorded in `requirements.txt`.                                  |
| Easy Creation               | Environments can be created using `python3 -m venv`.                                 |
| Easy Deletion               | The environment can be removed by deleting its directory.                            |
| Project Isolation           | Each application can maintain its own dependencies.                                  |
| CI/CD Support               | Clean environments can be created during CI/CD execution.                            |
| Lightweight                 | Virtual environments are relatively lightweight compared with full virtual machines. |
| Easy Recovery               | The environment can be recreated from `requirements.txt`.                            |
| No Global Package Pollution | Project dependencies do not need to be installed globally.                           |

---

# 6. Virtual Environment Tools

Different tools can be used to create and manage Python environments.

| **Tool**     | **Description**                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------- |
| `venv`       | Python's built-in module for creating virtual environments.                                        |
| `virtualenv` | Third-party tool for creating isolated Python environments.                                        |
| Conda        | Environment and package management system commonly used for data science and scientific workloads. |
| Poetry       | Python dependency and project management tool that also manages environments.                      |

---

# 7. Tool Comparison

| **Tool**     | **Main Purpose**                            | **Speed** | **Ease of Use** | **Best Use Case**                                  |
| ------------ | ------------------------------------------- | --------- | --------------- | -------------------------------------------------- |
| `venv`       | Create standard Python virtual environments | Fast      | Easy            | Standard Python applications                       |
| `virtualenv` | Advanced virtual environment creation       | Fast      | Easy            | Projects requiring additional environment features |
| Conda        | Environment + package management            | Medium    | Easy            | Data science / scientific applications             |
| Poetry       | Dependency + project management             | Medium    | Easy            | Modern Python project management                   |

### Recommended Default

For standard Python applications, `venv` is generally the simplest starting point because it is provided as part of Python and does not require a separate environment-management tool.

---

# 8. Advantages and Disadvantages

| **Advantages**                     | **Disadvantages**                                          |
| ---------------------------------- | ---------------------------------------------------------- |
| Isolates application dependencies  | Environment must be recreated on another machine           |
| Prevents package version conflicts | Packages must be installed separately for each environment |
| Easy to create and delete          | Requires activation before working with the environment    |
| Supports reproducible environments | `venv` itself does not manage system-level dependencies    |
| Works well with CI/CD              | Dependency versions still need to be managed properly      |
| Does not require a virtual machine | Large dependency sets can require additional disk space    |

---

# 9. Best Practices

| **Best Practice**                        | **Description**                                                            |
| ---------------------------------------- | -------------------------------------------------------------------------- |
| Use one environment per project          | Prevent dependencies from different projects from mixing.                  |
| Use `requirements.txt`                   | Record required package versions.                                          |
| Do not commit `venv/`                    | Add the environment directory to `.gitignore`.                             |
| Use `python -m pip`                      | Ensures pip is associated with the selected Python interpreter.            |
| Avoid `sudo pip`                         | Do not install project packages globally when using a virtual environment. |
| Pin important dependencies               | Define package versions where reproducibility is required.                 |
| Recreate instead of copying environments | Create a fresh environment and install dependencies.                       |
| Verify active Python                     | Use `which python` before installing packages.                             |
| Keep Python versions consistent          | Use the same compatible Python version across development and CI/CD.       |
| Regularly test requirements              | Ensure `requirements.txt` can recreate the application environment.        |

Recommended `.gitignore`:

```text
venv/
```

---

# 10. Operational Commands

## 10.1 Creation

Create a project directory:

```bash
mkdir myproject
cd myproject
```

Create a virtual environment:

```bash
python3 -m venv venv
```

Verify:

```bash
ls -la
```

Expected:

```text
venv/
```

Check the environment configuration:

```bash
cat venv/pyvenv.cfg
```

---

## 10.2 Activation

Activate the environment:

```bash
source venv/bin/activate
```

Expected terminal:

```text
(venv) user@server:~/myproject$
```

Verify:

```bash
which python
```

Expected:

```text
/home/user/myproject/venv/bin/python
```

Verify pip:

```bash
which pip
```

---

## 10.3 Package Installation

Install a package:

```bash
python -m pip install requests
```

Install a specific version:

```bash
python -m pip install requests==2.32.3
```

Install from `requirements.txt`:

```bash
python -m pip install -r requirements.txt
```

Verify installed packages:

```bash
python -m pip list
```

Check a package:

```bash
python -m pip show requests
```

---

## 10.4 Freezing Dependencies

Display installed dependencies:

```bash
pip freeze
```

Save dependencies:

```bash
pip freeze > requirements.txt
```

Verify:

```bash
cat requirements.txt
```

Example:

```text
Flask==3.x.x
requests==2.x.x
```

Reinstall dependencies:

```bash
python -m pip install -r requirements.txt
```

---

## 10.5 Deactivation

Deactivate the environment:

```bash
deactivate
```

Verify:

```bash
which python3
```

The path should point to the system Python rather than:

```text
venv/bin/python
```

---

## 10.6 Deletion

Deactivate first:

```bash
deactivate
```

Delete the environment:

```bash
rm -rf venv
```

Verify:

```bash
ls -la
```

The `venv/` directory should no longer be present.

> **Note:** Deleting the virtual environment removes its installed packages. It does not normally remove the application source code or `requirements.txt`.

---

# 11. Troubleshooting

## 11.1 Activation Error

### Error

```bash
source venv/bin/activate
```

returns:

```text
No such file or directory
```

### Check

```bash
pwd
ls -la
```

Verify:

```bash
ls -la venv/
```

If the directory does not exist:

```bash
python3 -m venv venv
```

Then:

```bash
source venv/bin/activate
```

---

## 11.2 `venv` Module Not Available

### Error

```text
No module named venv
```

### Resolution

Ubuntu/Debian:

```bash
sudo apt update
sudo apt install python3-venv -y
```

Then recreate:

```bash
python3 -m venv venv
```

---

## 11.3 Permission Problems

### Error

```text
Permission denied
```

Check:

```bash
ls -ld .
ls -ld venv
```

Check ownership:

```bash
ls -l
```

If the project directory has incorrect ownership, an administrator can correct it:

```bash
sudo chown -R $USER:$USER myproject
```

Then retry:

```bash
python3 -m venv venv
```

---

## 11.4 pip Permission Error

Check the active environment:

```bash
which python
which pip
```

Both should point to:

```text
.../venv/bin/
```

Activate if required:

```bash
source venv/bin/activate
```

Install using:

```bash
python -m pip install <package>
```

Avoid:

```bash
sudo pip install <package>
```

---

## 11.5 ModuleNotFoundError

Check:

```bash
which python
```

Check the package:

```bash
python -m pip show <package>
```

Install it:

```bash
python -m pip install <package>
```

For a project with dependency definitions:

```bash
python -m pip install -r requirements.txt
```

---

## 11.6 Wrong Python Environment

Run:

```bash
which python
```

Expected:

```text
/home/user/myproject/venv/bin/python
```

If the result points to system Python, activate:

```bash
source venv/bin/activate
```

Then verify again.

---

# 12. Recommendation / Conclusion

For standard Python applications, the recommended approach is to use Python's built-in `venv` module.

The recommended lifecycle is:

```text
Create
  ↓
Activate
  ↓
Install
  ↓
Test
  ↓
Freeze
  ↓
requirements.txt
  ↓
CI/CD
  ↓
Recreate
  ↓
Deploy
```

The recommended commands are:

```bash
python3 -m venv venv

source venv/bin/activate

python -m pip install -r requirements.txt

pip freeze > requirements.txt

deactivate
```

The `venv/` directory should normally not be committed to source control.

Instead, source code and dependency definitions such as `requirements.txt` should be maintained so that the environment can be recreated consistently.

---

# 13. FAQs

### Q1. What is a Python Virtual Environment?

A virtual environment is an isolated Python environment used to manage application-specific dependencies separately from the system Python installation.

### Q2. Why do we need a virtual environment?

It prevents dependency conflicts and allows different applications to use different package versions.

### Q3. Is `venv` included with Python?

The `venv` module is part of Python, although some Linux distributions provide it through a separate OS package such as `python3-venv`.

### Q4. How do I create a virtual environment?

```bash
python3 -m venv venv
```

### Q5. How do I activate it?

```bash
source venv/bin/activate
```

### Q6. How do I verify that it is active?

Run:

```bash
which python
```

The result should point to:

```text
venv/bin/python
```

### Q7. How do I install packages?

```bash
python -m pip install <package-name>
```

### Q8. What is `pip freeze`?

It displays installed packages and their versions.

```bash
pip freeze
```

### Q9. Why do we use `requirements.txt`?

It records project dependencies so that the environment can be recreated.

```bash
pip freeze > requirements.txt
```

### Q10. Should `venv/` be pushed to Git?

No. Normally add:

```text
venv/
```

to `.gitignore`.

### Q11. Can I copy the virtual environment to another server?

It is recommended to create a new environment and install the dependencies from `requirements.txt` instead of copying the existing environment.

### Q12. How do I deactivate the environment?

```bash
deactivate
```

### Q13. How do I delete the environment?

```bash
rm -rf venv
```

Deactivate it first if it is active.

### Q14. What should I do if activation gives "No such file or directory"?

Check:

```bash
ls -la
```

If `venv/` does not exist, create it again:

```bash
python3 -m venv venv
```

### Q15. What should I do if I get a permission error?

Check:

```bash
ls -ld .
ls -ld venv
```

Verify that the current user owns or has write access to the project directory.

### Q16. Should I use `sudo pip install`?

No. When using a virtual environment, install packages using:

```bash
python -m pip install <package>
```

### Q17. Can two applications use different versions of the same package?

Yes. Each application can have its own virtual environment and package versions.

---

# 15. Contact Information

| **Name**            | **Email**                              |
| ------------------- | -----------------------------------    |
| vikas badliwal      | vikash.badliwal.snaatak@mygurukulam.co |
| <DevOps Team>       | [<email>](mailto:<email>) |

---

# 16. References

| **Topic**                                                                                            | **Description**                                                                |
| ---------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [Python `venv` Documentation](https://docs.python.org/3/library/venv.html)                           | Official Python documentation for virtual environments.                        |
| [Python Packaging User Guide](https://packaging.python.org/)                                         | Official guide for Python packaging and dependency management.                 |
| [pip Documentation](https://pip.pypa.io/en/stable/)                                                  | Official pip documentation for Python package installation and management.     |
| [Requirements File Documentation](https://pip.pypa.io/en/stable/reference/requirements-file-format/) | Official pip documentation for requirements files.                             |
| [Python Packaging Guide](https://packaging.python.org/en/latest/tutorials/installing-packages/)      | Guide for installing Python packages.                                          |
| [DOC-README-Template.md](INTERNAL_REPOSITORY_URL)                                                    | Internal documentation template used for the structure of this document.       |
| [Python Virtual Environment POC](POC_URL)                                                            | Practical demonstration of Python virtual environment creation and management. |

---
