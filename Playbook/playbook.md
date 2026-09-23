# Ansible | Ansible Playbook CI/CD | Documentation

## Document Information

| Author         | Created On | Version | Last Updated |
| -------------- | ---------- | ------- | ------------ |
| Vikas Badliwal | 23/09/2026 | 1.0     | 23/09/2026   |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Ansible Playbook CI/CD?](#2-what-is-ansible-playbook-cicd)
3. [Why is Playbook CI/CD Required?](#3-why-is-playbook-cicd-required)
4. [CI/CD Workflow](#4-cicd-workflow)
5. [Playbook Testing and Validation](#5-playbook-testing-and-validation)
6. [Implementation](#6-implementation)
7. [Deployment](#7-deployment)
8. [Best Practices](#8-best-practices)
9. [Advantages](#9-advantages)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

# 1. Introduction

Ansible Playbooks are used to automate configuration, application deployment, and infrastructure tasks.

As playbooks are maintained as code, they should be validated before deployment. Integrating Ansible Playbooks into a CI/CD pipeline helps identify syntax and code-quality issues before changes reach the target environment.

This documentation explains how an Ansible Playbook can be tested using **syntax checking and Ansible Lint** before deployment.

---

# 2. What is Ansible Playbook CI/CD?

Ansible Playbook CI/CD is the process of integrating Ansible automation into a Continuous Integration and Continuous Deployment pipeline.

The pipeline automatically:

* Retrieves the latest playbook code.
* Checks the playbook syntax.
* Performs linting and code-quality validation.
* Stops the pipeline if validation fails.
* Executes the playbook when validation succeeds.

---

# 3. Why is Playbook CI/CD Required?

Playbook CI/CD helps reduce deployment failures by validating automation code before it is executed.

### Key reasons

* Detect syntax errors early.
* Identify common Ansible coding issues.
* Maintain consistent playbook quality.
* Prevent invalid changes from reaching deployment.
* Automate testing instead of relying only on manual checks.
* Provide a repeatable deployment process.

---

# 4. CI/CD Workflow

The basic workflow is:

```text
Developer
    |
    v
Git Repository
    |
    v
CI/CD Pipeline
    |
    +----------------------+
    |                      |
    v                      v
Syntax Check           Ansible Lint
    |                      |
    +----------+-----------+
               |
          Validation
               |
        +------+------+
        |             |
      FAIL          PASS
        |             |
        v             v
   Stop Pipeline   Deploy
                      |
                      v
              Target Environment
```

### Workflow Explanation

1. Developer pushes the Ansible Playbook to the Git repository.
2. CI/CD pipeline is triggered.
3. `ansible-playbook --syntax-check` validates the playbook syntax.
4. `ansible-lint` checks coding practices and common issues.
5. If any validation fails, the pipeline stops.
6. If all checks pass, the playbook can proceed to deployment.

---

# 5. Playbook Testing and Validation

## 5.1 Syntax Check

Ansible provides a syntax-check option to validate the structure and syntax of a playbook without executing its tasks.

```bash
ansible-playbook --syntax-check playbook.yml
```

### Expected Result

```text
playbook: playbook.yml
```

A successful syntax check allows the pipeline to continue.

---

## 5.2 Ansible Lint

Ansible Lint checks Ansible content for common issues and recommended coding practices.

Install Ansible Lint:

```bash
pip install ansible-lint
```

Run linting:

```bash
ansible-lint playbook.yml
```

If the playbook passes the configured lint rules, the pipeline can continue to the next stage.

---

# 6. Implementation

## 6.1 Example Playbook

Create a playbook:

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

    - name: Ensure NGINX is running
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

## 6.2 Test Playbook Syntax

Run:

```bash
ansible-playbook --syntax-check playbook.yml
```

If the syntax is valid, the playbook is not executed and only the syntax is checked.

---

## 6.3 Run Ansible Lint

Run:

```bash
ansible-lint playbook.yml
```

Resolve reported issues before allowing the playbook to proceed to deployment.

---

# 7. Deployment

After the syntax check and linting stages pass, the pipeline can execute the playbook.

```bash
ansible-playbook -i inventory.ini playbook.yml
```

The deployment stage should run only after the validation stages have completed successfully.

### CI/CD Stage Flow

```text
Checkout
   |
   v
Syntax Check
   |
   v
Ansible Lint
   |
   v
Deployment
```

If syntax checking or linting fails:

```text
Validation Failed
       |
       v
Pipeline Stops
       |
       v
No Deployment
```

---

# 8. Best Practices

* Store playbooks in Git.
* Run syntax checks on every change.
* Use Ansible Lint for code-quality validation.
* Fix validation errors before deployment.
* Use fully qualified Ansible module names.
* Keep playbooks simple and reusable.
* Separate variables, tasks, handlers, and templates where required.
* Use CI/CD to automate validation.
* Do not deploy when validation stages fail.

---

# 9. Advantages

| Advantage             | Description                                                           |
| --------------------- | --------------------------------------------------------------------- |
| Early Error Detection | Finds syntax problems before deployment                               |
| Better Code Quality   | Linting identifies common coding issues                               |
| Automation            | Testing is performed automatically                                    |
| Consistency           | Same validation process is used for every change                      |
| Safer Deployment      | Failed validation prevents deployment                                 |
| Faster Feedback       | Developers receive errors before changes reach the target environment |

---

# 10. Conclusion

Ansible Playbook CI/CD provides a controlled process for validating and deploying automation code.

Using **syntax checking** and **Ansible Lint** before deployment helps identify problems early and improves playbook quality.

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
Ansible Playbook Deployment
```

This approach helps ensure that only validated Ansible automation proceeds toward deployment.

---

# 11. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 12. References

| Reference                      | Description                                         |
| ------------------------------ | --------------------------------------------------- |
| Ansible Documentation          | Ansible automation and playbook reference           |
| Ansible Lint Documentation     | Ansible playbook linting and code-quality reference |
| Ansible Playbook Documentation | Playbook syntax and execution reference             |
