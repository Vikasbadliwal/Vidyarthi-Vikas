# SOP for Python Virtual Environment

## Document Information

| Author | Created On | Version |
| ------ | ---------- | ------- |
| Vikas  | 24-08-2026 |         |

| Reviewer |   |
| -------- | - |
|          |   |

---

## Table of Contents

1. Purpose
2. Prerequisites
3. Virtual Environment Structure
4. Creating a Virtual Environment
5. Activating the Virtual Environment
6. Installing Python Packages
7. Freezing Installed Packages
8. Deactivating the Virtual Environment
9. Deleting the Virtual Environment
10. Troubleshooting
11. Operational Checklist
12. Conclusion

---

# 1. Purpose

Python virtual environments provide an isolated environment for installing and managing Python packages for individual projects.

A virtual environment helps to:

* Isolate project dependencies.
* Avoid conflicts between packages required by different projects.
* Install project-specific Python packages without affecting the system Python environment.
* Maintain reproducible project dependencies.
* Manage different package versions for different applications.

This SOP explains how to create, activate, use, freeze, deactivate, and delete a Python virtual environment, along with common troubleshooting procedures.

---

# 2. Prerequisites

Before creating a Python virtual environment, ensure that the following prerequisites are available.

## 2.1 Operating System

This SOP is written primarily for Linux systems such as:

* Ubuntu
* Debian
* RHEL
* CentOS
* Amazon Linux

The commands shown in this document use an Ubuntu/Linux environment.

## 2.2 Required Access

The user should have:

* Access to the project directory.
* Permission to create directories and files in the project location.
* Permission to install required Python packages.
* Access to Python and pip.

## 2.3 Check Python Installation

Check whether Python is installed:

```bash
python3 --version
```

Example:

```text
Python 3.12.3
```

## 2.4 Check pip Installation

Check pip:

```bash
python3 -m pip --version
```

If the virtual environment module is not available, install it on Ubuntu/Debian:

```bash
sudo apt update
sudo apt install python3-venv -y
```

Verify:

```bash
python3 -m venv --help
```

---

# 3. Virtual Environment Structure

A virtual environment is normally created inside or near the project directory.

Example:

```text
myproject/
├── app.py
├── requirements.txt
└── venv/
    ├── bin/
    ├── lib/
    ├── include/
    └── pyvenv.cfg
```

The `venv` directory contains the isolated Python environment.

The recommended practice is to avoid committing the virtual environment directory to Git.

Example `.gitignore` entry:

```text
venv/
```

---

# 4. Creating a Virtual Environment

## 4.1 Create Project Directory

Create a project directory:

```bash
mkdir myproject
cd myproject
```

## 4.2 Create Virtual Environment

Create a virtual environment named `venv`:

```bash
python3 -m venv venv
```

The command creates a directory named:

```text
venv/
```

## 4.3 Verify Creation

Check the directory:

```bash
ls -la
```

Expected output will include:

```text
venv
```

Check the virtual environment configuration:

```bash
cat venv/pyvenv.cfg
```

---

# 5. Activating the Virtual Environment

Activation makes the virtual environment's Python and package-management commands available through the shell.

## 5.1 Linux/macOS

Activate the environment using:

```bash
source venv/bin/activate
```

After activation, the terminal normally displays the environment name:

```text
(venv) user@server:~/myproject$
```

## 5.2 Verify Activation

Check Python:

```bash
which python
```

Expected result should point to the virtual environment:

```text
/home/user/myproject/venv/bin/python
```

Check pip:

```bash
which pip
```

Expected:

```text
/home/user/myproject/venv/bin/pip
```

Check Python version:

```bash
python --version
```

---

# 6. Installing Python Packages

Once the virtual environment is activated, packages can be installed using `pip`.

## 6.1 Install a Package

Example:

```bash
pip install requests
```

Install another package:

```bash
pip install flask
```

## 6.2 Install a Specific Version

```bash
pip install requests==2.32.3
```

This installs the specified version of the package.

## 6.3 Verify Installed Packages

```bash
pip list
```

Example:

```text
Package    Version
---------- -------
Flask      3.x.x
requests   2.x.x
```

You can also check an individual package:

```bash
pip show requests
```

## 6.4 Install Packages from requirements.txt

If a project already contains a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

This installs the packages and versions specified in the file.

---

# 7. Freezing Installed Packages

Freezing records the packages currently installed in the virtual environment.

## 7.1 Generate requirements.txt

Run:

```bash
pip freeze > requirements.txt
```

This creates or updates:

```text
requirements.txt
```

Example:

```text
Flask==3.x.x
requests==2.x.x
```

## 7.2 Verify the File

```bash
cat requirements.txt
```

## 7.3 Recreate the Environment

On another system or after creating a new virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

This allows the project dependencies to be installed from the recorded requirements.

---

# 8. Deactivating the Virtual Environment

When work is completed, deactivate the virtual environment using:

```bash
deactivate
```

After deactivation, the `(venv)` prefix should disappear from the terminal.

Verify:

```bash
which python3
```

The command should now point to the system Python installation rather than the virtual environment.

---

# 9. Deleting the Virtual Environment

A Python virtual environment can be deleted when it is no longer required.

## 9.1 Deactivate First

If the environment is currently active:

```bash
deactivate
```

## 9.2 Remove the Virtual Environment

If the environment directory is named `venv`:

```bash
rm -rf venv
```

Verify:

```bash
ls -la
```

The `venv` directory should no longer exist.

### Important

Deleting the virtual environment removes the installed packages inside that environment.

The project source code and `requirements.txt` are not removed unless they are inside the directory being deleted.

---

# 10. Troubleshooting

## 10.1 Activation Error

### Problem

Running:

```bash
source venv/bin/activate
```

returns:

```text
No such file or directory
```

### Possible Causes

* Virtual environment was not created.
* Incorrect directory.
* Incorrect virtual environment name.
* `venv` directory was deleted.

### Resolution

Check the current directory:

```bash
pwd
```

Check available files:

```bash
ls -la
```

Check whether the environment exists:

```bash
ls -la venv/
```

If it does not exist, create it again:

```bash
python3 -m venv venv
```

Then activate:

```bash
source venv/bin/activate
```

---

## 10.2 `python3 -m venv` Error

### Problem

The following command fails:

```bash
python3 -m venv venv
```

### Resolution

On Ubuntu/Debian, install the required package:

```bash
sudo apt update
sudo apt install python3-venv -y
```

Then create the environment again:

```bash
python3 -m venv venv
```

---

## 10.3 Permission Problems

### Problem

The user receives an error such as:

```text
Permission denied
```

while creating the virtual environment or installing files.

### Check Directory Ownership

```bash
ls -ld .
```

Check the project directory:

```bash
ls -ld myproject
```

Check the virtual environment:

```bash
ls -ld venv
```

### Resolution

Ensure the current user has appropriate ownership and permissions for the project directory.

For example:

```bash
sudo chown -R $USER:$USER myproject
```

Then retry:

```bash
python3 -m venv venv
```

### Important

Avoid using:

```bash
sudo pip install <package>
```

inside a virtual environment.

The virtual environment should normally be owned and managed by the user who is using it.

---

## 10.4 Package Installation Permission Error

### Problem

Running:

```bash
pip install requests
```

results in a permission-related error.

### Verify the Virtual Environment

Run:

```bash
which python
```

and:

```bash
which pip
```

Both should point to the `venv` directory.

Example:

```text
/home/user/myproject/venv/bin/python
/home/user/myproject/venv/bin/pip
```

If they do not, activate the environment:

```bash
source venv/bin/activate
```

Then retry:

```bash
pip install requests
```

---

## 10.5 `pip` Command Not Found

### Problem

The system reports:

```text
pip: command not found
```

### Resolution

Use Python to invoke pip:

```bash
python3 -m pip --version
```

After activating the virtual environment:

```bash
python -m pip --version
```

Packages can then be installed using:
 
```bash
python -m pip install requests
```

---

## 10.6 Package Installed but Python Cannot Import It

### Problem

The package appears to be installed, but the application reports:

```text
ModuleNotFoundError
```

### Check Which Python Is Being Used

```bash
which python
```

Check installed packages:

```bash
python -m pip list
```

Check the package:

```bash
python -m pip show <package-name>
```

If the virtual environment is not active: 

```bash
source venv/bin/activate
```

Then install the required package:

```bash
python -m pip install <package-name>
```

---

## 10.7 Wrong Python Environment Is Active

Check:

```bash
which python
```

Expected:

```text
/home/user/myproject/venv/bin/python
```

If the output points to `/usr/bin/python3` or another location, activate the correct environment:

```bash
source venv/bin/activate
```

Verify again:

```bash
which python
```

---

# 11. Conclusion

Python virtual environments provide an isolated and controlled environment for managing project-specific dependencies.

The standard operational process is:

```text
Create
   ↓
Activate
   ↓
Install Packages
   ↓
Test Application
   ↓
Freeze Dependencies
   ↓
requirements.txt
   ↓
Deactivate
   ↓
Delete when no longer required
```

For a new project, the recommended basic workflow is:

```bash
mkdir myproject
cd myproject

python3 -m venv venv

source venv/bin/activate

python -m pip install requests

pip freeze > requirements.txt

deactivate
```

Before deleting an environment, ensure that important dependencies are captured in `requirements.txt`.

Maintaining a clean virtual environment and a reliable dependency file helps ensure consistent application setup across development, testing, and deployment environments.

