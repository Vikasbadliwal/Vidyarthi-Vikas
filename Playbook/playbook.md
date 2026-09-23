<p align="center">

<img width="200" height="297" alt="image" src="https://github.com/user-attachments/assets/98811b35-2e68-4f23-853c-73afdba8051b" />

</p

# | Ansible Playbook CI | Documentation

## Author Table

| Author | Created On | Version | Last Updated By | Last Updated On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | --------------- | ---------------------- | -------------- | ------------------ |
| Vikas  | 27-08-2026 | v1.0    | vikas           | 27-08-2026      | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |
| Vikas  |            |         |                 |                 | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Ansible Playbook CI?](#2-what-is-ansible-playbook-ci)
3. [Why is Ansible Playbook CI Required?](#3-why-is-ansible-playbook-ci-required)
4. [CI Workflow](#4-ci-workflow)
5. [Playbook Testing](#5-playbook-testing)
6. [Implementation](#6-implementation)
7. [Validation Flow](#7-validation-flow)
8. [Conclusion](#8-conclusion)
9. [Contact Information](#9-contact-information)
10. [References](#10-references)

---

# 1. Introduction

Ansible Playbooks are used to automate configuration and deployment tasks.

Since playbooks are maintained as code, they should be validated whenever changes are made. **Continuous Integration (CI)** helps automatically test Ansible Playbooks before they are used for deployment.

This documentation explains how Ansible Playbooks can be validated using **syntax checking and linting** to identify issues early and maintain code quality.

---

# 2. What is Ansible Playbook CI?

Ansible Playbook CI is the process of automatically validating Ansible Playbooks whenever code changes are pushed to a Git repository.

The CI process checks the playbook before it proceeds toward deployment.

The main validation checks are:

* Playbook syntax check
* Ansible Lint
* Code-quality validation

---

# 3. Why is Ansible Playbook CI Required?

Ansible Playbook CI helps identify problems before they affect the target environment.

### Key reasons

* Detect syntax errors early.
* Identify common Ansible issues.
* Maintain consistent code quality.
* Prevent invalid playbooks from proceeding.
* Provide quick feedback to developers.
* Reduce failures caused by incorrect automation code.

---

# 4. CI Workflow

The basic workflow is:

```text
Developer
    |
    v
Git Repository
    |
    v
CI Pipeline
    |
    v
Syntax Check
    |
    v
Ansible Lint
    |
    v
Validation
   / \
 FAIL PASS
  |     |
  v     v
Stop   Ready
       for
    Deployment
```

### Workflow Explanation

1. Developer creates or modifies an Ansible Playbook.
2. The changes are pushed to the Git repository.
3. CI pipeline is triggered.
4. The playbook syntax is checked.
5. Ansible Lint performs code-quality checks.
6. If validation fails, the CI pipeline stops.
7. If validation succeeds, the playbook is ready for the next deployment stage.

---

# 5. Playbook Testing

## 5.1 Syntax Check

Ansible provides a syntax-check option to validate the structure and syntax of a playbook without executing its tasks.

```bash
ansible-playbook --syntax-check playbook.yml
```

### Expected Result

```text
playbook: playbook.yml
```

A successful syntax check means the playbook can proceed to the next validation stage.

---

## 5.2 Ansible Lint

Ansible Lint checks Ansible Playbooks for common issues and coding practices.

Install Ansible Lint:

```bash
pip install ansible-lint
```

Run linting:

```bash
ansible-lint playbook.yml
```

Any reported issues should be reviewed and fixed before the playbook proceeds.

---

# 6. Implementation

## 6.1 Create an Ansible Playbook

Example:

```yaml
---
- name: Install NGINX
  hosts: webservers
  become: true

  tasks:
    - name: Install NGINX
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true

    - name: Start NGINX
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```

Save the file as:

```text
playbook.yml
```

---

## 6.2 Run Syntax Check

Execute:

```bash
ansible-playbook --syntax-check playbook.yml
```

The syntax check validates the playbook without actually running the tasks.

If an error is found, fix the playbook and run the check again.

---

## 6.3 Run Ansible Lint

Execute:

```bash
ansible-lint playbook.yml
```

Review the output and resolve the reported issues.

After fixing the issues, run the lint command again.

---

# 7. Validation Flow

The CI validation process can be represented as:

```text
        Git Push
           |
           v
    Checkout Playbook
           |
           v
     Syntax Check
           |
       +---+---+
       |       |
     FAIL     PASS
       |       |
       v       v
     Stop   Ansible Lint
               |
           +---+---+
           |       |
         FAIL     PASS
           |       |
           v       v
         Stop   Validation
                Successful
                    |
                    v
             Ready for
             Deployment
```

### Validation Rule

| Check        | Result | Action                   |
| ------------ | ------ | ------------------------ |
| Syntax Check | Failed | Stop CI                  |
| Syntax Check | Passed | Continue                 |
| Ansible Lint | Failed | Stop CI                  |
| Ansible Lint | Passed | CI validation successful |

---

# 10. Conclusion

Ansible Playbook CI provides an automated validation process for Ansible automation code.

Using **syntax checking** and **Ansible Lint** helps identify syntax and code-quality issues before the playbook is used for deployment.

The overall process is:

```text
Git
 |
 v
CI Pipeline
 |
 +--> Syntax Check
 |
 +--> Ansible Lint
 |
 v
Validation Passed
 |
 v
Ready for Deployment
```

This approach improves playbook quality and helps prevent avoidable failures before deployment.

---

# 11. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 12. References

| Reference                      | Description                                |
| ------------------------------ | ------------------------------------------ |
| Ansible Documentation          | Ansible Playbook and automation reference  |
| Ansible Lint Documentation     | Ansible code-quality and linting reference |
| Ansible Playbook Documentation | Playbook syntax and validation reference   |
