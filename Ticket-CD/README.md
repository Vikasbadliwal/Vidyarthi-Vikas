# Ansible Role – Continuous Deployment (CD) Workflow

## Document Information

| Author | Created On | Version | Last Updated By | Last Updated On |
| ------ | ---------- | ------- | --------------- | --------------- |
|        | 29-08-2026 | v1.0    |                 |                 |

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

* Git
* Jenkins
* Ansible
* Git repository
* Target server
* SSH access
* Required Jenkins credentials
* Network connectivity between Jenkins and target servers

### Verify Git

```bash
git --version
```

### Verify Ansible

```bash
ansible --version
```

### Test Ansible Connectivity

```bash
ansible all -i inventory -m ping
```

Expected:

```text
SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# 3. Ansible Role

An **Ansible Role** is a reusable structure for organizing tasks, variables, templates, files, handlers, and dependencies.

Example role structure:

```text
application-role/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   └── application.conf.j2
├── files/
├── vars/
│   └── main.yml
├── meta/
│   └── main.yml
└── README.md
```

| Directory    | Purpose                        |
| ------------ | ------------------------------ |
| `tasks/`     | Tasks executed by the role     |
| `defaults/`  | Default variables              |
| `vars/`      | Role variables                 |
| `templates/` | Jinja2 templates               |
| `files/`     | Static files                   |
| `handlers/`  | Handlers                       |
| `meta/`      | Role dependencies and metadata |

Example playbook:

```yaml
---
- name: Deploy Application
  hosts: application
  become: true

  roles:
    - application
```

---

# 4. CD Workflow

The overall CD workflow is:

```text
Developer
    |
    v
Feature Branch
    |
    v
Git Commit
    |
    v
Pull Request
    |
    v
Code Review
    |
    v
Merge to Main
    |
    v
Jenkins Pipeline
    |
    +---- Checkout
    |
    +---- Validation
    |
    +---- Approval
    |
    +---- Deployment
    |
    +---- Verification
    |
    v
Target Server
```

### Workflow Summary

1. Developer creates a feature branch.
2. Changes are made to the Ansible role.
3. Changes are committed and pushed.
4. Pull Request is created.
5. Code is reviewed.
6. PR is merged into `main`.
7. Jenkins starts the CD pipeline.
8. Jenkins validates the Ansible code.
9. Approval is requested if required.
10. Jenkins runs the Ansible playbook.
11. Ansible applies the role to target servers.
12. Deployment is verified.

---

# 5. Git Workflow

Create a feature branch:

```bash
git checkout main
git pull origin main

git checkout -b feature/application-deployment
```

Make the required Ansible changes and check them:

```bash
git status
git diff
```

Stage the changes:

```bash
git add .
```

Commit:

```bash
git commit -m "Update application deployment role"
```

Push:

```bash
git push origin feature/application-deployment
```

Create a Pull Request:

```text
feature/application-deployment
              |
              v
             main
```

After code review and approval, merge the PR into `main`.

---

# 6. Jenkins Pipeline

Jenkins automates the deployment process.

Typical pipeline:

```text
Checkout
   |
   v
Validation
   |
   v
Approval
   |
   v
Deployment
   |
   v
Verification
   |
   v
Notification
```

## 6.1 Checkout

Jenkins checks out the source code.

```groovy
stage('Checkout') {
    steps {
        checkout scm
    }
}
```

---

## 6.2 Validation

Validate the Ansible playbook:

```bash
ansible-playbook --syntax-check deploy.yml
```

If configured:

```bash
ansible-lint
```

The deployment should proceed only when validation succeeds.

---

## 6.3 Approval

For production environments, a manual approval can be added.

```text
Validation
    |
    v
Approval
   / \
  /   \
Yes    No
 |      |
 v      v
Deploy  Stop
```

---

## 6.4 Deployment

Jenkins executes the Ansible playbook:

```bash
ansible-playbook -i inventory deploy.yml
```

---

# 7. Ansible Deployment

Example inventory:

```ini
[application]
app-server-01
app-server-02
```

Example playbook:

```yaml
---
- name: Deploy Application
  hosts: application
  become: true

  roles:
    - application
```

Example role task:

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

### Manual Deployment Test

Before running through Jenkins, test the playbook manually:

```bash
ansible-playbook -i inventory deploy.yml
```

To deploy to a specific host:

```bash
ansible-playbook -i inventory deploy.yml --limit app-server-01
```

---

# 8. Verification

After deployment, verify that the target server is working correctly.

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

The deployment should be considered successful only when the required verification checks pass.

---

# 9. Rollback

If a deployment introduces an issue, use the approved rollback process.

Common rollback methods:

* Revert the Git commit.
* Deploy the previous known-good version.
* Restore the previous configuration.
* Run the previous Ansible role version.

Example:

```bash
git revert <commit-id>
```

After the rollback change is reviewed and merged, Jenkins can deploy the reverted version.

> Rollback procedures should be tested before production use.

---

# 10. Troubleshooting

## 10.1 Ansible Connection Failure

```bash
ansible all -i inventory -m ping
```

Check:

* SSH connectivity
* Inventory hostname
* Credentials
* Firewall
* Target server availability

---

## 10.2 Playbook Syntax Error

```bash
ansible-playbook --syntax-check deploy.yml
```

Fix the reported YAML or Ansible syntax issue.

---

## 10.3 Permission Error

If elevated privileges are required:

```yaml
become: true
```

Verify that the Ansible user has the required permissions.

---

## 10.4 Inventory Error

Check the inventory:

```bash
ansible-inventory -i inventory --list
```

Verify that the expected hosts and groups are present.

---

## 10.5 Jenkins Pipeline Failure

Check:

* Jenkins console logs
* Git checkout
* Ansible installation
* Jenkins credentials
* SSH connectivity
* Inventory
* Playbook errors

---

# 11. Best Practices

* Use feature branches for development.
* Use Pull Requests for code review.
* Keep `main` stable.
* Run Ansible syntax checks before deployment.
* Use `ansible-lint` where applicable.
* Never hardcode passwords or private keys.
* Store secrets using an approved secret-management solution.
* Use Jenkins credentials for authentication.
* Maintain separate environment inventories where required.
* Use approval for production deployments.
* Keep deployment logs.
* Implement deployment verification.
* Maintain a tested rollback process.
* Keep Ansible roles reusable and modular.

---

# 12. Conclusion

The Ansible CD workflow provides an automated and repeatable deployment process by integrating **Git, Jenkins, and Ansible**.

```text
Git
 |
 v
Pull Request
 |
 v
Code Review
 |
 v
Main Branch
 |
 v
Jenkins
 |
 +---- Validation
 |
 +---- Approval
 |
 v
Ansible Playbook
 |
 v
Ansible Role
 |
 v
Target Server
 |
 v
Verification
```

This approach improves deployment consistency, reduces manual effort, and provides a controlled process for delivering changes to target environments.

---

# 13. FAQs

### Q1. What is the purpose of Ansible in CD?

Ansible automates configuration and deployment tasks on target servers.

### Q2. What is Jenkins used for?

Jenkins automates the CD pipeline, including validation and deployment.

### Q3. Why use an Ansible Role?

Roles provide a reusable and organized structure for Ansible automation.

### Q4. Why use feature branches?

Feature branches allow developers to work independently without directly modifying `main`.

---

## References

* Ansible Documentation
* Jenkins Documentation
* Git Documentation
* Project-specific deployment documentation
