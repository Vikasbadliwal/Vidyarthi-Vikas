# PYTHON | VIRTUAL ENVIRONMENT | PYTHON VIRTUAL ENVIRONMENT Documentation

---

# Author Table

| **Author**    | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** |   **L2 Reviewer**   |
| ------------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | ---------------     |
|   vikas       | 24-08-2026     | 1.0         |                     |                    | Deepak Kushwaha | Faisal/Mohit K  | Mahesh Kumar/Varun  |
                                                                                                                                       

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Introduction](#2-introduction)

<details>
<summary><strong>3. Operational Commands</strong></summary>

- [3.1 Creation](#31-creation)
- [3.2 Activation](#32-activation)
- [3.3 Package Installation](#33-package-installation)
- [3.4 Freezing Dependencies](#34-freezing-dependencies)
- [3.5 Deactivation](#35-deactivation)
- [3.6 Deletion](#36-deletion)

</details>

4. [Troubleshooting](#4-troubleshooting)
5. [Conclusion](#5-conclusion)
6. [FAQs](#6-faqs)
7. [Contact Information](#7-contact-information)
8. [References](#8-references)

---

# 1. Introduction

Python applications commonly require multiple external packages and libraries.

Different applications may require different versions of the same package. Installing all packages globally can therefore create dependency conflicts and make application environments difficult to reproduce.

A **Python Virtual Environment** provides an isolated Python environment for an individual project.

---

# 2. What is Python Virtual Environment

A Python Virtual Environment is an isolated directory containing a Python environment and its project-specific packages.

The standard Python `venv` module can be used to create a virtual environment.

Example:

```bash
python3 -m venv venv
```

<img width="907" height="316" alt="python vitual venv screenshot(SOP-1)" src="https://github.com/user-attachments/assets/601f3f35-0023-4301-a7b8-88d527c8a02f" />


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

<img width="775" height="291" alt="SOP-1(Screen-1)" src="https://github.com/user-attachments/assets/5cecf320-a5d4-40ce-81e9-4b4418dcb0ac" />

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
<img width="901" height="144" alt="source pip (SOP-1)" src="https://github.com/user-attachments/assets/410ad797-849c-488b-a359-e21c193a2113" />


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
<img width="1418" height="866" alt="pip (SOP-1 screenshot-2)" src="https://github.com/user-attachments/assets/f28a00d0-4a30-4fd7-89f8-6c65bc6b2f97" />

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

<img width="1417" height="452" alt="pip freeze (SOP-1 )" src="https://github.com/user-attachments/assets/74d37cac-61bd-423e-b8ae-d91548645d15" />

---

## 10.5 Deactivation

Deactivate the environment:

```bash
deactivate
```

<img width="809" height="96" alt="deactivate (SOP-1)" src="https://github.com/user-attachments/assets/0a89b8ca-67e4-4e74-a928-67bedc703bb1" />

---

## 10.6 Deletion

Deactivate first:

```bash
deactivate
```

Delete the environment:

<img width="809" height="227" alt="deleting venv (SOP-1)" src="https://github.com/user-attachments/assets/0d9b1c9d-6b92-460d-866e-c06faeb3642b" />


The `venv/` directory should no longer be present.

> **Note:** Deleting the virtual environment removes its installed packages. It does not normally remove the application source code or `requirements.txt`.

---

# 11. Troubleshooting

## 11.1 Activation Error

### Error

```bash
source venv/bin/activate
```

<img width="809" height="71" alt="source error (SOP-1)" src="https://github.com/user-attachments/assets/20e81420-4ee8-4fd8-88db-33ff685fb81b" />

### Check

```bash
pwd
ls -la
```

<img width="810" height="276" alt="pwd (SOP-1)" src="https://github.com/user-attachments/assets/c6e9ac65-8a8c-4052-9c41-02c10783774b" />


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

<img width="774" height="93" alt="check permission issue (SOP-1)" src="https://github.com/user-attachments/assets/3763fb2b-9c18-4d57-ba2a-9be6326c0ef6" />


Check ownership:

<img width="809" height="123" alt="ls -l (SOP-1)" src="https://github.com/user-attachments/assets/596c02a8-3010-445c-8e97-1e5446b48721" />


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

<img width="774" height="118" alt="check python (SOP-1)" src="https://github.com/user-attachments/assets/3e806569-3aaf-49a8-9b79-460a4e0f1aad" />

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

# 12. Conclusion

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

---
