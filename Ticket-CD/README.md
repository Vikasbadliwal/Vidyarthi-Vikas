# Ansible Role – Continuous Deployment (CD) Workflow

## Document Information

| Author | Created On | Version | Last Updated By | Last Updated On |  L0 Reviewer    | L1 Reviewer    |    L2 Reviewer     |
| ------ | ---------- | ------- | --------------- | --------------- | ------------    | ------------   | -----------------  |
|  vikas | 27-08-2026 | v1.0    |                 |                 | Deepak Kushwaha | Faisal/Mohit K | Mahesh Kumar/Varun |

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Prerequisites](#2-prerequisites)
3. [Ansible Role](#3-ansible-role)
4. [CD Workflow](#4-cd-workflow)
5. [Git Workflow](#5-git-workflow)
6. [Jenkins Pipeline](#6-jenkins-pipeline)
7. [Ansible Deployment](#7-ansible-deployment)
8. [Verification](#8-verification)
9. [Rollback](#9-rollback)
10. [Troubleshooting](#10-troubleshooting)
11. [Best Practices](#11-best-practices)
12. [Conclusion](#12-conclusion)
13. [FAQs](#13-faqs)

---

# 1. Purpose

This document describes the **Continuous Deployment (CD) workflow for an Ansible Role**.

The workflow integrates **Git, Jenkins, and Ansible** to automate deployment from source code to target servers.

### Objectives

* Automate deployments.
* Reduce manual intervention.
* Maintain consistent configurations.
* Support code review and version control.
* Provide repeatable deployments.
* Enable controlled production deployments.

---

# 2. Prerequisites

The following components are required:

| 8    | Jenkins validates the Ansible code         |
| 9    | Approval is taken if required              |
| 10   | Jenkins runs the Ansible playbook          |
| 11   | Ansible applies the role to target servers |
| 12   | Deployment is verified                     |

---

# 5. Git Workflow

Create a feature branch:

```bash
git checkout main
git pull origin main

git checkout -b feature/application-deployment
```

Check changes:

```bash
git status
git diff
```

Stage and commit:

```bash
git add .
git commit -m "Update application deployment role"
```

Push the branch:

```bash
git push origin feature/application-deployment
```

Create a Pull Request:

```text
feature/application-deployment
            ↓
           main
```

After review and approval, merge the PR into `main`.

---

# 6. Jenkins CD Pipeline

Jenkins automates the deployment process.

### Pipeline Stages

| Stage        | Purpose                                     |
| ------------ | ------------------------------------------- |
| Checkout     | Gets the latest code                        |
| Validation   | Checks Ansible code                         |
| Approval     | Manual approval for production, if required |
| Deployment   | Runs the Ansible playbook                   |
| Verification | Checks deployment status                    |
| Notification | Reports success or failure                  |

### Pipeline Flow

```text
Checkout
   ↓
Validation
   ↓
Approval
   ↓
Deployment
   ↓
Verification
   ↓
Notification
```

---

## 6.1 Checkout

Jenkins checks out the repository code.

```groovy
stage('Checkout') {
    steps {
        checkout scm
    }
}
```

---

## 6.2 Validation

Check the playbook syntax:

```bash
ansible-playbook --syntax-check deploy.yml
```

If configured, run:

```bash
ansible-lint
```

If validation fails, the deployment should stop.

---

## 6.3 Approval

For production deployment, manual approval can be added.

```text
Validation
    ↓
Approval
   / \
 Yes  No
  ↓    ↓
Deploy Stop
```

---

## 6.4 Deployment

Jenkins executes the Ansible playbook:

```bash
ansible-playbook -i inventory deploy.yml
```

---

# 7. Ansible Deployment

### Inventory

Example:

```ini
[application]
app-server-01
app-server-02
```

### Playbook

```yaml
---
- name: Deploy Application
  hosts: application
  become: true

  roles:
    - application
```

### Example Role Task

```yaml
---
- name: Install application package
  ansible.builtin.package:
    name: nginx
    state: present

- name: Start application service
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
```

### Manual Test

Before using Jenkins, test the deployment manually:

```bash
ansible-playbook -i inventory deploy.yml
```

For a specific server:

```bash
ansible-playbook -i inventory deploy.yml --limit app-server-01
```

---

# 8. Deployment Verification

After deployment, verify the target server.

### Check Ansible Connectivity

```bash
ansible all -i inventory -m ping
```

### Check Service

```bash
systemctl status nginx
```

### Check Application

```bash
curl http://<server-ip>
```

| Check             | Expected Result                   |
| ----------------- | --------------------------------- |
| Ansible ping      | Server responds successfully      |
| Service status    | Service is running                |
| Application check | Application responds successfully |

---

# 9. Rollback

If the deployment causes an issue, use the approved rollback procedure.

Common options:

| Method                 | Purpose                            |
| ---------------------- | ---------------------------------- |
| `git revert`           | Revert a Git change                |
| Previous version       | Deploy the last known-good version |
| Previous configuration | Restore previous configuration     |

Example:

```bash
git revert <commit-id>
```

After the rollback change is reviewed and merged, the CD pipeline can deploy it.

---

# 10. Troubleshooting

| Problem                    | Command / Check                              |
| -------------------------- | -------------------------------------------- |
| Ansible connection failure | `ansible all -i inventory -m ping`           |
| Syntax error               | `ansible-playbook --syntax-check deploy.yml` |
| Inventory issue            | `ansible-inventory -i inventory --list`      |
| Permission issue           | Check `become: true` and user permissions    |
| Jenkins failure            | Check Jenkins console logs                   |
| SSH issue                  | Verify SSH credentials and connectivity      |

---

# 11. Best Practices

* Use feature branches.
* Use Pull Requests for code review.
* Keep `main` stable.
* Validate Ansible code before deployment.
* Do not hardcode passwords or private keys.
* Use Jenkins credentials for authentication.
* Use separate inventories for environments when required.
* Use approval for production deployments.
* Maintain deployment logs.
* Always have a tested rollback procedure.

---

# 12. Quick Reference

| Requirement     | Command                                      |
| --------------- | -------------------------------------------- |
| Check Git       | `git --version`                              |
| Check Ansible   | `ansible --version`                          |
| Check status    | `git status`                                 |
| Check changes   | `git diff`                                   |
| Stage changes   | `git add .`                                  |
| Commit          | `git commit -m "message"`                    |
| Push            | `git push origin <branch>`                   |

---

# 13. Conclusion

The CD workflow connects **Git, Jenkins, and Ansible** to automate deployment.

```text
Git
 ↓
Pull Request
 ↓
Code Review
 ↓
main
 ↓
Jenkins
 ↓
Validation
 ↓
Approval
 ↓
Ansible
 ↓
Target Server
 ↓
Verification
```

This provides a simple, repeatable, and controlled deployment process for Ansible Roles.

---

# 14. FAQs

### What is an Ansible Role?

A reusable structure that organizes Ansible tasks, variables, templates, files, and handlers.

### What is Jenkins doing in this workflow?

Jenkins automates the CD pipeline and runs the Ansible deployment

### How do we validate an Ansible playbook?

```bash
ansible-playbook --syntax-check deploy.yml
```

### How do we deploy?

```bash
ansible-playbook -i inventory deploy.yml
```

### How do we rollback a Git change?

```bash
git revert <commit-id>
```
