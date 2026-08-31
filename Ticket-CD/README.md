# Ansible Role | Continuous Deployment (CD) Workflow | Documentation |

<img width="236" height="133" alt="image" src="https://github.com/user-attachments/assets/9e6e0771-2879-4544-8459-ee2c9881ca30" />


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
5. [Jenkins Pipeline](#6-jenkins-pipeline)
6. [Ansible Deployment](#7-ansible-deployment)
7. [Verification](#8-verification)
8. [Troubleshooting](#10-troubleshooting)
9. [Best Practices](#11-best-practices)
10. [Conclusion](#12-conclusion)
11. [FAQs](#13-faqs)

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

# 5. Jenkins CD Pipeline

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

# 6. Ansible Deployment

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

---

# 13. Contact Information

| **Name**            | **Email**                              |
| ------------------- | -----------------------------------    |
| vikas badliwal      | vikash.badliwal.snaatak@mygurukulam.co |
| <DevOps Team>       | [<email>](mailto:<email>) |

---

# 12. Quick Reference

|        Topic           |      Link                                                   |
| ---------------------- | ----------------------------------------------------------  |
| Ansible Documentation  | https://docs.ansible.com/projects/ansible/latest/index.html |
| Jenkins Documentation  |   https://www.jenkins.io/doc/                               |

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

