# Ansible Role – Continuous Deployment (CD) Workflow

## Document Information

| Author | Created On | Version | Last Updated By | Last Updated On |
| ------ | ---------- | ------- | --------------- | --------------- |
|        | 29-08-2026 | v1.0    |                 |                 |

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Prerequisites](#2-prerequisites)
3. [What is Continuous Deployment](#3-what-is-continuous-deployment)
4. [What is an Ansible Role](#4-what-is-an-ansible-role)
5. [Ansible Role Structure](#5-ansible-role-structure)
6. [CD Workflow](#6-cd-workflow)
7. [Git Workflow](#7-git-workflow)
8. [Jenkins Pipeline](#8-jenkins-pipeline)
9. [Ansible Deployment](#9-ansible-deployment)
10. [Deployment Verification](#10-deployment-verification)
11. [Rollback Strategy](#11-rollback-strategy)
12. [Troubleshooting](#12-troubleshooting)
13. [Best Practices](#13-best-practices)
14. [Advantages](#14-advantages)
15. [Conclusion](#15-conclusion)
16. [FAQs](#16-faqs)

---

# 1. Purpose

The purpose of this documentation is to explain the **Continuous Deployment (CD) workflow for an Ansible Role**.

The workflow automates the deployment of application configurations and infrastructure changes from a Git repository to target servers using **Jenkins and Ansible**.

The overall objective is to:

* Automate deployment.
* Reduce manual intervention.
* Maintain consistent server configuration.
* Provide repeatable deployments.
* Integrate Git, Jenkins, and Ansible.
* Support controlled deployments to different environments.

---

# 2. Prerequisites

Before implementing the CD workflow, ensure the following components are available.

## 2.1 Git

Git is required for source-code and Ansible role management.

Verify Git:

```bash
git --version
```

---

## 2.2 Jenkins

Jenkins is used to automate the CD pipeline.

The Jenkins server should have:

* Jenkins installed and running.
* Required credentials configured.
* Git access.
* Ansible installed or available to the Jenkins agent.
* Network connectivity to target servers.

---

## 2.3 Ansible

Verify Ansible:

```bash
ansible --version
```

---

## 2.4 Target Server

The target server should:

* Be reachable from the Jenkins agent.
* Have SSH access configured.
* Have the required user permissions.
* Have Python available if required by the Ansible modules being used.

Test connectivity:

```bash
ansible all -i inventory -m ping
```

Expected result:

```text
SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# 3. What is Continuous Deployment

Continuous Deployment (CD) is the process of automatically deploying validated changes to a target environment.

In this workflow, Git stores the Ansible code, Jenkins manages the pipeline, and Ansible performs the deployment.

```text
Developer
    |
    | git push
    v
Git Repository
    |
    | Webhook
    v
Jenkins
    |
    +---- Checkout
    |
    +---- Validation
    |
    +---- Approval
    |
    +---- Ansible Deployment
    |
    v
Target Server
    |
    v
Deployment Verification
```

---

# 4. What is an Ansible Role

An Ansible Role is a reusable and organized way to group Ansible tasks, variables, templates, handlers, files, and metadata.

A role helps maintain a clean and reusable Ansible project structure.

For example:

```yaml
- name: Deploy application
  hosts: application
  become: true

  roles:
    - application
```

The playbook calls the role, and the role performs the required configuration or deployment tasks.

---

# 5. Ansible Role Structure

A typical Ansible role can have the following structure:

```text
application-role/
│
├── defaults/
│   └── main.yml
│
├── handlers/
│   └── main.yml
│
├── tasks/
│   └── main.yml
│
├── templates/
│   └── application.conf.j2
│
├── files/
│   └── application.conf
│
├── vars/
│   └── main.yml
│
├── meta/
│   └── main.yml
│
└── README.md
```

## Role Directories

| Directory    | Purpose                                 |
| ------------ | --------------------------------------- |
| `tasks/`     | Contains tasks executed by the role     |
| `defaults/`  | Contains default variables              |
| `vars/`      | Contains role variables                 |
| `templates/` | Contains Jinja2 templates               |
| `files/`     | Contains static files                   |
| `handlers/`  | Contains handlers                       |
| `meta/`      | Contains role metadata and dependencies |

---

# 6. CD Workflow

The complete CD workflow is:

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
Main Branch
    |
    v
Jenkins Pipeline
    |
    +------------------+
    |                  |
    v                  v
Checkout          Validation
                       |
                       v
                   Approval
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
                       |
                       v
                  Notification
```

---

## 6.1 Developer Creates a Feature Branch

Create a feature branch:

```bash
git checkout -b feature/application-deployment
```

Or:

```bash
git switch -c feature/application-deployment
```

---

## 6.2 Make Changes

Update the required Ansible role files.

For example:

```text
tasks/main.yml
defaults/main.yml
templates/application.conf.j2
handlers/main.yml
```

---

## 6.3 Check Changes

```bash
git status
```

Review changes:

```bash
git diff
```

---

## 6.4 Stage Changes

```bash
git add .
```

---

## 6.5 Commit Changes

```bash
git commit -m "Update application deployment role"
```

---

## 6.6 Push Changes

```bash
git push origin feature/application-deployment
```

---

## 6.7 Pull Request

Create a Pull Request:

```text
feature/application-deployment
              |
              v
             main
```

The Pull Request should go through the required code-review process.

---

# 7. Git Workflow

The Git workflow used by the CD process can be represented as:

```text
Feature Branch
      |
      v
Development
      |
      v
git add
      |
      v
git commit
      |
      v
git push
      |
      v
Pull Request
      |
      v
Code Review
      |
      v
Merge to main
      |
      v
Jenkins CD Pipeline
```

Before working on a feature, developers should ensure that their local branch is based on the latest code.

For example:

```bash
git checkout main
git pull origin main
git checkout -b feature/application-deployment
```

---

# 8. Jenkins Pipeline

Jenkins automates the CD workflow.

A typical pipeline consists of the following stages:

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

---

## 8.1 Checkout

Jenkins checks out the latest source code.

Example:

```groovy
stage('Checkout') {
    steps {
        checkout scm
    }
}
```

---

## 8.2 Validation

Validate the Ansible playbook before deployment.

```bash
ansible-playbook --syntax-check deploy.yml
```

If `ansible-lint` is configured:

```bash
ansible-lint
```

The deployment should proceed only if validation succeeds.

---

## 8.3 Approval

For production deployments, a manual approval step can be added.

```text
Validation
    |
    v
Manual Approval
    |
    +---- Approved ----> Deployment
    |
    +---- Rejected ----> Pipeline Stops
```

---

## 8.4 Deployment

After approval, Jenkins executes the Ansible playbook.

```bash
ansible-playbook -i inventory deploy.yml
```

---

# 9. Ansible Deployment

A deployment playbook can call the Ansible role.

Example:

```yaml
---
- name: Deploy Application
  hosts: application
  become: true

  roles:
    - application
```

The role performs the required deployment tasks.

Example:

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

---

## 9.1 Inventory

Example inventory:

```ini
[application]
app-server-01
app-server-02
```

The inventory defines the target hosts where Ansible will execute the role.

---

## 9.2 Execute Playbook Manually

Before integrating with Jenkins, the playbook can be tested manually:

```bash
ansible-playbook -i inventory deploy.yml
```

For a specific host:

```bash
ansible-playbook -i inventory deploy.yml --limit app-server-01
```

---

# 10. Deployment Verification

After deployment, verify that the required service or application is running.

Example:

```bash
systemctl status nginx
```

Check connectivity:

```bash
ansible all -i inventory -m ping
```

Application verification can also be performed using:

```bash
curl http://<server-ip>
```

The Jenkins pipeline should mark the deployment as successful only when the required verification checks pass.

---

# 11. Rollback Strategy

If a deployment causes an issue, the deployment should be rolled back using the project's approved rollback procedure.

Possible approaches include:

* Revert the Git commit.
* Restore the previous configuration.
* Deploy the previous known-good version.
* Re-run the previous Ansible role version.

Git revert example:

```bash
git revert <commit-id>
```

After the rollback change is reviewed and merged, the CD pipeline can deploy the reverted configuration.

> Rollback procedures should be tested before production use.

---

# 12. Troubleshooting

## 12.1 Ansible Connection Failure

Test connectivity:

```bash
ansible all -i inventory -m ping
```

Check:

* SSH connectivity.
* Inventory hostname.
* SSH credentials.
* Firewall rules.
* Target server availability.

---

## 12.2 Playbook Syntax Error

Run:

```bash
ansible-playbook --syntax-check deploy.yml
```

Correct the reported YAML or Ansible syntax issue and run the validation again.

---

## 12.3 Permission Error

If elevated privileges are required, verify the playbook configuration:

```yaml
become: true
```

Also verify that the Ansible user has the required permissions.

---

## 12.4 Inventory Error

Display the inventory:

```bash
ansible-inventory -i inventory --list
```

Verify that the expected hosts and groups are present.

---

## 12.5 Jenkins Pipeline Failure

Check:

* Jenkins console output.
* Git checkout status.
* Ansible installation.
* Jenkins credentials.
* SSH connectivity.
* Inventory configuration.
* Playbook errors.

---

# 13. Best Practices

* Use feature branches for changes.
* Use Pull Requests for code review.
* Keep the `main` branch stable.
* Run `ansible-playbook --syntax-check` before deployment.
* Use `ansible-lint` where applicable.
* Never hardcode passwords or private keys in the repository.
* Store secrets using an approved secret-management solution.
* Use Jenkins credentials for authentication.
* Use separate inventories for different environments where appropriate.
* Use manual approval for production deployments when required.
* Keep deployment logs.
* Implement deployment verification.
* Maintain a tested rollback procedure.
* Keep Ansible roles reusable and modular.

---

# 14. Advantages

| Advantage         | Description                                                   |
| ----------------- | ------------------------------------------------------------- |
| Automation        | Reduces manual deployment work                                |
| Consistency       | Applies the same configuration repeatedly                     |
| Repeatability     | Deployment can be executed multiple times                     |
| Version Control   | Ansible code is maintained in Git                             |
| Code Review       | Changes can be reviewed through Pull Requests                 |
| CI/CD Integration | Jenkins can automate validation and deployment                |
| Faster Deployment | Reduces deployment time                                       |
| Rollback          | Previous configurations can be restored using version control |

---

# 15. Conclusion

The Ansible CD workflow provides an automated and repeatable method for deploying application configurations and infrastructure changes.

The overall workflow is:

```text
Developer
    |
    v
Git Feature Branch
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
    v
Validation
    |
    v
Approval
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

By integrating **Git, Jenkins, and Ansible**, the deployment process becomes more consistent, controlled, repeatable, and suitable for CI/CD environments.

---

# 16. FAQs

## Q1. What is the purpose of Ansible in the CD workflow?

Ansible automates configuration and deployment tasks on target servers.

## Q2. What is Jenkins used for?

Jenkins automates the CD pipeline and executes the required validation and deployment stages.

## Q3. Why do we use an Ansible Role?

Roles organize and reuse Ansible tasks, variables, templates, files, and handlers.

## Q4. Why do we use a feature branch?

A feature branch allows developers to work on changes independently without directly modifying the `main` branch.


---

# References

* Ansible Documentation
* Jenkins Documentation
* Git Documentation
* Project-specific deployment documentation
