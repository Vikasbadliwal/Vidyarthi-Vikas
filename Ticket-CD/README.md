# Ansible Role | Continuous Deployment (CD) Workflow | Documentation

<img width="236" height="133" alt="Ansible CD Workflow" src="https://github.com/user-attachments/assets/9e6e0771-2879-4544-8459-ee2c9881ca30" />

---

## Document Information

| Author | Created On | Version | Last Updated By | Last Updated On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | --------------- | --------------- | -------------- | ------------------ |
| Vikas  | 27-08-2026 | v1.0    |                 |                 | Deepak Kushwaha | Faisal/Mohit K | Mahesh Kumar/Varun |

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Prerequisites](#2-prerequisites)
3. [Ansible Role](#3-ansible-role)
4. [CD Workflow](#4-cd-workflow)
5. [Jenkins Pipeline](#5-jenkins-pipeline)
6. [Ansible Deployment](#6-ansible-deployment)
7. [Verification](#7-verification)
8. [Troubleshooting](#8-troubleshooting)
9. [Best Practices](#9-best-practices)
10. [Contact Information](#10-contact-information)
11. [Quick Reference](#11-quick-reference)
12. [Conclusion](#12-conclusion)
13. [FAQs](#13-faqs)

---

# 1. Purpose

This document describes the **Continuous Deployment (CD) workflow for an Ansible Role**.

The workflow uses:

* Git for source code management
* Jenkins for automation
* Ansible for deployment
* Target servers for application configuration

### Objectives

* Automate deployments.
* Reduce manual work.
* Maintain consistent configuration.
* Support code review.
* Provide repeatable deployments.
* Support controlled production deployment.

---

# 2. Prerequisites

The following are required:

| Component         | Purpose                    |
| ----------------- | -------------------------- |
| Git               | Stores Ansible code        |
| Jenkins           | Runs the CD pipeline       |
| Ansible           | Performs deployment        |
| Ansible Inventory | Defines target servers     |
| SSH Access        | Connects to target servers |
| Target Server     | Receives the deployment    |

### Required Access

The Jenkins server should have:

* Access to the Git repository.
* Ansible installed.
* SSH access to target servers.
* Required credentials.
* Permission to execute Ansible commands.

Check Ansible:

```bash
ansible --version
```

Check SSH connectivity:

```bash
ssh <user>@<server-ip>
```

---

# 3. Ansible Role

An **Ansible Role** is a reusable structure used to organize Ansible automation.

Typical role structure:

<img width="214" height="418" alt="image" src="https://github.com/user-attachments/assets/557ca0be-4032-4baa-97c6-597393d3371c" />


### Main Role Directories

| Directory    | Purpose                    |
| ------------ | -------------------------- |
| `tasks/`     | Contains deployment tasks  |
| `handlers/`  | Contains handlers          |
| `templates/` | Contains Jinja2 templates  |
| `files/`     | Contains static files      |
| `vars/`      | Contains role variables    |
| `defaults/`  | Contains default variables |
| `meta/`      | Contains role metadata     |

---

# 4. CD Workflow

The CD workflow automatically deploys approved Ansible changes to target servers.

### Workflow

<img width="682" height="1024" alt="image" src="https://github.com/user-attachments/assets/c865bb59-91b5-45c9-8452-cd6c3f1b3d32" />


### Workflow Steps

| Step | Activity                                   |
| ---- | ------------------------------------------ |
| 1    | Developer creates/updates Ansible code     |
| 2    | Code is pushed to Git                      |
| 3    | Pull Request is created                    |
| 4    | Code review is performed                   |
| 5    | Approved code is merged into `main`        |
| 6    | Jenkins starts the CD pipeline             |
| 7    | Jenkins checks the Ansible code            |
| 8    | Approval is taken if required              |
| 9    | Jenkins runs the Ansible playbook          |
| 10   | Ansible applies the role to target servers |
| 11   | Deployment is verified                     |
| 12   | Pipeline result is reported                |

---

# 5. Jenkins Pipeline

Jenkins automates the deployment process.

### Pipeline Stages

| Stage        | Purpose                       |
| ------------ | ----------------------------- |
| Checkout     | Gets the latest Git code      |
| Validation   | Checks Ansible code           |
| Approval     | Manual approval when required |
| Deployment   | Runs the Ansible playbook     |
| Verification | Checks deployment status      |
| Notification | Reports pipeline result       |


---

# 6. Ansible Deployment

## 6.1 Inventory

The inventory defines the target servers.

Example:

```ini
[application]
app-server-01
app-server-02
```

---

## 6.2 Playbook

The playbook calls the Ansible Role.

Example:

```yaml
---
- name: Deploy Application
  hosts: application
  become: true

  roles:
    - application
```

---

## 6.3 Validate Playbook

Before deployment:

```bash
ansible-playbook --syntax-check deploy.yml
```

If there is no error, the syntax is valid.

---

## 6.4 Test Connectivity

Run:

```bash
ansible all -i inventory -m ping
```

Expected result:

```text
SUCCESS
```

---

## 6.5 Run Deployment

Run:

```bash
ansible-playbook -i inventory deploy.yml
```

To deploy to a specific server:

```bash
ansible-playbook -i inventory deploy.yml \
--limit app-server-01
```

---

# 7. Verification

After deployment, verify the target server.

### Check Ansible Connectivity

```bash
ansible all -i inventory -m ping
```

### Check Service

Example:

```bash
systemctl status nginx
```

### Check Application

```bash
curl http://<server-ip>
```

### Verification Table

| Check        | Expected Result                   |
| ------------ | --------------------------------- |
| Ansible Ping | Server responds successfully      |
| Service      | Service is running                |
| Application  | Application responds successfully |
| Jenkins      | Pipeline shows success            |

---

# 8. Troubleshooting

| Problem                    | Check / Command                              |
| -------------------------- | -------------------------------------------- |
| Ansible connection failure | `ansible all -i inventory -m ping`           |
| Playbook syntax error      | `ansible-playbook --syntax-check deploy.yml` |
| Inventory issue            | `ansible-inventory -i inventory --list`      |
| SSH issue                  | Check SSH credentials/connectivity           |
| Permission issue           | Check `become` and user permissions          |
| Jenkins failure            | Check Jenkins console logs                   |
| Deployment failure         | Check Ansible task output                    |
| Service failure            | `systemctl status <service>`                 |

### Inventory Check

```bash
ansible-inventory -i inventory --list
```

### Playbook Check

```bash
ansible-playbook --syntax-check deploy.yml
```

### Jenkins Logs

If the Jenkins pipeline fails:

1. Open the Jenkins job.
2. Open the failed build.
3. Check **Console Output**.
4. Identify the failed stage.
5. Fix the issue.
6. Run the pipeline again.

---

# 9. Best Practices

| Best Practice                  | Reason                         |
| ------------------------------ | ------------------------------ |
| Use Git branches               | Keeps changes isolated         |
| Use Pull Requests              | Supports code review           |
| Keep `main` stable             | Protects production code       |
| Validate before deployment     | Finds syntax issues early      |
| Use Jenkins credentials        | Avoids hardcoded secrets       |
| Do not commit passwords/keys   | Protects sensitive information |
| Use separate inventories       | Keeps environments isolated    |
| Use approval for production    | Prevents accidental deployment |

---

# 10. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 11. Reference

| Topic                   |    Links                                                      |
| ----------------------- | ------------------------------------------------------------- |
| Ansible Documentation   |   https://docs.ansible.com/projects/ansible/latest/index.html |
| Jenkins Documentation   |             https://www.jenkins.io/doc/                       |


---

# 12. Conclusion

The CD workflow connects **Git, Jenkins, and Ansible** to automate deployment.

The overall process is:

```text
Developer
   ↓
Git
   ↓
Code Review
   ↓
Jenkins
   ↓
Validation
   ↓
Ansible
   ↓
Target Server
   ↓
Verification
```

This provides a simple, repeatable, and controlled deployment process for Ansible Roles.

---

# 13. FAQs

### Q1. What is an Ansible Role?

An Ansible Role is a reusable structure used to organize tasks, variables, templates, files, and handlers.

### Q2. What does Jenkins do?

Jenkins automates the CD pipeline and runs the Ansible deployment.

### Q3. How do we validate a playbook?

```bash
ansible-playbook --syntax-check deploy.yml
```

### Q4. How do we test server connectivity?

```bash
ansible all -i inventory -m ping
```

### Q5. How do we deploy?

```bash
ansible-playbook -i inventory deploy.yml
```
