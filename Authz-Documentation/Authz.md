# VCS Design + POC | VCS AuthN & AuthZ Strategy | AuthZ Documentation

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer**        | **L1 Reviewer** | **L2 Reviewer**    |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | ---------------------- | --------------- | ------------------ |
| Vikas      | 12/09/2026     | v1.0        | Vikas               | 12/09/2026         | Deepak Kushwaha/Ayushi | Faisal/Mohit K  | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Why Authorization is Required](#2-why-authorization-is-required)
3. [Authentication vs Authorization](#3-authentication-vs-authorization)
4. [Access Levels](#4-access-levels)
5. [Audit Trails](#5-audit-trails)
6. [Integration with Identity Providers](#6-integration-with-identity-providers)
7. [Types of Authorization](#7-types-of-authorization)

   * [7.1 Role-Based Access Control](#71-role-based-access-control)
   * [7.2 Attribute-Based Access Control](#72-attribute-based-access-control)
   * [7.3 Access Control Lists](#73-access-control-lists)
   * [7.4 Policy-Based Access Control](#74-policy-based-access-control)
8. [Authorization Comparison](#8-authorization-comparison)
9. [Advantages and Disadvantages](#9-advantages-and-disadvantages)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

This Documentation provides a step-by-step guide for understanding and implementing **Authorization (AuthZ)** for Version Control Systems (VCS).
It covers authorization requirements, **Authentication vs Authorization**, access levels,audit trails, and integration with Identity Providers (IdPs). 
It also explains different authorization models such as **RBAC, ABAC, ACL, and PBAC**, along with their comparison, advantages, disadvantages,and best practices help users understand and implement a secure and structured access-control strategy.

---

# 2. Why Authorization is Required

Authorization is important because not every user should have the same level of access.

For example:

* A developer may need to **clone and push code**.
* A reviewer may need to **review and approve pull requests**.
* A release manager may need to **merge release branches**.
* An administrator may need to **manage repositories and permissions**.

Without proper authorization, users may access or modify resources that are not required for their responsibilities.

---

# 3. Authentication vs Authorization

Authentication and authorization are related but serve different purposes.

| **Authentication (AuthN)**     | **Authorization (AuthZ)**               |
| ------------------------------ | --------------------------------------- |
| Verifies identity              | Verifies permissions                    |
| Answers "Who are you?"         | Answers "What can you do?"              |
| Happens before authorization   | Uses authenticated identity             |
| Uses passwords, MFA, SSO, etc. | Uses roles, permissions, policies, etc. |
| Example: Login to Git server   | Example: Permission to push code        |

---

# 4. Access Levels

Access levels define what actions a user can perform on a VCS resource.

A simple access model can contain the following levels:

| **Access Level** | **Typical Permissions**                     | **Example User** |
| ---------------- | ------------------------------------------- | ---------------- |
| Read             | View and clone repositories                 | Viewer           |
| Write            | Push code and create branches               | Developer        |
| Review           | Review and approve changes                  | Reviewer         |
| Maintain         | Manage branches and repository settings     | Maintainer       |
| Admin            | Manage users, permissions, and repositories | Administrator    |

---

# 5. Audit Trails

An **audit trail** is a record of actions performed by users or systems.

Audit trails help organizations understand:

* Who performed an action.
* What action was performed.
* Which repository or resource was accessed.

---

# 6. Integration with Identity Providers

An **Identity Provider (IdP)** is a system that manages user identities and authentication.

Examples include:

* Microsoft Entra ID
* Okta
* Keycloak
* LDAP
* Active Directory

A VCS can integrate with an Identity Provider so that user identity and access management can be centrally controlled.

---

# 7. Types of Authorization

There are several common authorization models.

The major models covered in this documentation are:

1. **RBAC – Role-Based Access Control**
2. **ABAC – Attribute-Based Access Control**
3. **ACL – Access Control List**
4. **PBAC – Policy-Based Access Control**

---

## 7.1 Role-Based Access Control

**RBAC** grants permissions based on a user's role. Instead of assigning permissions individually, permissions are assigned to roles.

### Example

| Role                | Permissions                                        | Example User |
| ------------------- | -------------------------------------------------- | ------------ |
| **Developer**       | Read, Clone, Push Code, Create Branch              | Vikas        |
| **Reviewer**        | Read, Clone, Review Pull Requests, Approve Changes | Rahul        |
| **Release Manager** | Read, Merge Branches, Create Releases              | Amit         |
| **Administrator**   | Full Repository and Permission Management          | Admin        |

In this model, users receive permissions through their assigned roles.

---

## 7.2 Attribute-Based Access Control

**ABAC** makes authorization decisions using attributes.

Attributes may include:

* User department.
* User role.
* Device.
* Time.
* Environment.

---

## 7.3 Access Control Lists

An **Access Control List (ACL)** defines permissions for specific users or groups on a particular resource.

### Example

**Repository: Project-A**

| User      | Read | Write | Admin | Access       |
| --------- | ---- | ----- | ----- | ------------ |
| **Vikas** | ✓    | ✓     | ✗     | Read + Write |
| **Rahul** | ✓    | ✗     | ✗     | Read Only    |
| **Amit**  | ✗    | ✗     | ✗     | No Access    |
| **Admin** | ✓    | ✓     | ✓     | Full Access  |

In this model, permissions are directly assigned to users or groups for the specific resource.

---

## 7.4 Policy-Based Access Control

**Policy-Based Access Control (PBAC)** uses defined policies to make authorization decisions.

A policy can evaluate multiple conditions such as:

* Identity.
* Role.
* Resource.
* Action.
* Environment.
* Risk.
---

# 8. Authorization Comparison

| **Authorization Type** | **Based On** | **Complexity** | **Scalability** | **Best Use Case**               |
| ---------------------- | ------------ | -------------- | --------------- | ------------------------------- |
| RBAC                   | Roles        | Low            | High            | Standard organizations          |
| ABAC                   | Attributes   | High           | High            | Fine-grained access             |
| ACL                    | Users/Groups | Low            | Medium          | Small environments              |
| PBAC                   | Policies     | High           | High            | Complex enterprise environments |

---
# 9. Advantages and Disadvantages

| **Advantages**                                                                                                    | **Disadvantages**                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **Better Security** – Prevents unauthorized access to protected repositories and resources.                       | **Configuration Complexity** – Large environments may require many roles, groups, and policies.      |
| **Least Privilege** – Gives users only the permissions required for their work.                                   | **Permission Management** – Permissions must be updated when users change roles or responsibilities. |
| **Centralized Access Management** – Allows users and groups to be managed centrally through an Identity Provider. | **Policy Errors** – Incorrect policies can accidentally allow or deny access.                        |
| **Reduced Risk** – Limits write and administrative permissions, reducing unauthorized changes.                    | **Maintenance** – Roles, groups, and permissions require regular review.                             |
| **Improved Auditing** – Records authorization and access events for monitoring and review.                        | **Troubleshooting** – Incorrect permissions can cause access issues that require investigation.      |


# 10. Best Practices

| **Best Practice**                  | **Description**                                                                       |
| ---------------------------------- | ------------------------------------------------------------------------------------- |
| **Follow Least Privilege**         | Give users only the permissions required for their responsibilities.                  |
| **Deny by Default**                | Deny access unless an explicit permission allows the requested action.                |
| **Use Role-Based Access**          | Use roles and groups instead of assigning permissions individually wherever possible. |
| **Centralize Identity Management** | Use an approved Identity Provider for centralized user and group management.          |
| **Protect Administrative Access**  | Restrict administrator permissions to authorized personnel only.                      |
| **Test Authorization**             | Test both allowed and denied access scenarios to verify authorization rules.          |

---

# 11. Conclusion

Authorization helps protect VCS resources by ensuring users have only the permissions required for their roles.
A secure AuthZ strategy should follow **least privilege**, use appropriate **roles and policies**, maintain **audit logs**, and regularly review access.
**RBAC with centralized identity management** is a simple and effective approach for most VCS environments.

---

# 12. Contact Information

| **Name**       | **Email**                                                                               |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 13. References

| **Description**                    | **Topic**                                        |
| ------------------------------- | ------------------------------------------------------ |
| OWASP Authorization Cheat Sheet | * [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)        |
| NIST ABAC Guide                 | * [NIST ABAC Guide](https://csrc.nist.gov/Projects/attribute-based-access-control) |
| Microsoft Identity Platform     | * [Microsoft Authorization Basics](https://learn.microsoft.com/en-us/entra/identity-platform/authorization-basics) |

---
