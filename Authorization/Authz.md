# Jenkins Role-Based Authorization Setup

## 1. Overview

This documentation explains how to configure **Role-Based Authorization Strategy** in Jenkins to manage user permissions and control access to different projects and jobs.

The main objective is to provide users with access according to their responsibilities, such as Developer, DevOps, Testing, and Admin.

Jenkins Role-Based Authorization allows administrators to control access at two levels:

- **Global Level:** Controls overall Jenkins permissions.
- **Item Level:** Controls access to specific jobs and projects using Regular Expressions (Regex).

## 2. Objectives

| S.No. | Objective | Description |
|---|---|---|
| 1 | Access Control | Implement role-based access control in Jenkins. |
| 2 | Role Creation | Create separate roles for Admin, Developer, DevOps, and Testing teams. |
| 3 | Job Visibility | Restrict job visibility based on user roles. |
| 4 | Permission Management | Assign appropriate permissions to different users. |
| 5 | Access Verification | Verify that users can access only their permitted Jenkins jobs. |

## 3. Prerequisites

| S.No. | Requirement | Description |
|---|---|---|
| 1 | Jenkins Server | Jenkins instance must be installed and running. |
| 2 | Admin Access | Administrator access to Jenkins is required. |
| 3 | Authorization Plugin | Role-based Authorization Strategy plugin must be installed. |
| 4 | Jenkins Users | Users must be created for different teams. |

## 4. Step-by-Step Configuration

### Step 1: Enable Role-Based Authorization Strategy

The first step is to enable the Role-Based Authorization Strategy in Jenkins.

**Procedure:**

1. Log in to Jenkins using an administrator account.
2. Navigate to **Manage Jenkins**.
3. Open **Configure Global Security**.
4. Locate the **Authorization** section.
5. Select **Role-Based Strategy**.
6. Click **Save**.

**Reference Screenshot:** `image_4de362.png`

### Step 2: Configure Global Roles

Global roles define the basic permissions available to users across the Jenkins instance.

**Procedure:**

1. Navigate to **Manage Jenkins**.
2. Select **Manage and Assign Roles**.
3. Click **Manage Roles**.
4. Under the **Global roles** section, create the following roles.

| Role Name | Permission | Description |
|---|---|---|
| admin | Overall/Administer | Complete Jenkins administration access |
| Developer | Overall/Read | Basic Jenkins dashboard access |
| devops | Overall/Read | Basic Jenkins dashboard access |
| testing | Overall/Read | Basic Jenkins dashboard access |

**Note:** The `Overall/Read` permission is required for users to access and view the Jenkins dashboard.

**Reference Screenshot:** `image_4ddffc.png`

### Step 3: Configure Item Roles (Project-Level Access)

Item roles are used to restrict access to specific Jenkins jobs and projects based on their names.

Jenkins uses Regular Expressions (Regex) to identify which jobs are accessible to each role.

**Procedure:**

1. Navigate to **Manage Jenkins → Manage and Assign Roles → Manage Roles**.
2. Scroll down to the **Item roles** section.
3. Create the following item roles with their respective patterns.

| Role Name | Regex Pattern | Description |
|---|---|---|
| Developer-role | `(?i)job-.*` | Access to jobs starting with `job-` |
| devops-role | `(?i)devops-.*` | Access to jobs starting with `devops-` |
| testing-role | `(?i)test-.*` | Access to jobs starting with `test-` |

**Permissions Assigned:**

The following permissions can be assigned according to team requirements:

| Permission | Description |
|---|---|
| Job/Read | View Jenkins jobs |
| Job/Build | Execute Jenkins builds |
| Job/Configure | Modify job configuration |
| Job/Workspace | Access job workspace |
| Job/Create | Create new Jenkins jobs |
| Job/Delete | Delete Jenkins jobs |
| Job/Move | Move jobs between folders |

**Note:** Developer and Testing users should receive only the permissions required for their responsibilities. DevOps users may receive additional permissions such as Create, Delete, and Move when required.

**Reference Screenshots:**

- `image_4de382.png`
- `image_4de07a.png`
- `image_4de05a.png`
- `image_4de3bc.png`

### Step 4: Assign Roles to Users

After creating global and item roles, assign the appropriate roles to Jenkins users.

**Procedure:**

1. Navigate to **Manage Jenkins**.
2. Select **Manage and Assign Roles**.
3. Click **Assign Roles**.
4. Assign global roles to the respective users.
5. Assign item roles to restrict project-level access.

**Global Role Assignments:**

| Username | Assigned Global Role |
|---|---|
| admin-devops | admin |
| dev-developer | Developer |
| dev-developer-1 | Developer |
| devops-dev | devops |
| devops-dev-1 | devops |
| test-qa | testing |
| test-qa-1 | testing |

**Item Role Assignments:**

| User Group | Assigned Item Role |
|---|---|
| Developer Users | Developer-role |
| DevOps Users | devops-role |
| Testing Users | testing-role |

Users are mapped to their respective item roles so that Jenkins displays only the jobs matching the configured Regex patterns.

**Reference Screenshots:**

- `image_4de329.png`
- `image_4de03b.png`

### Step 5: Verify Dashboard and Job Access

After assigning the roles, verify that the authorization configuration is working correctly.

**Verification Procedure:**

1. Log in to Jenkins using a Developer account.
2. Verify that only the jobs matching the Developer-role Regex pattern are accessible.
3. Log in using a DevOps account.
4. Verify that DevOps users can access their permitted jobs.
5. Log in using a Testing account.
6. Verify that Testing users can access only their permitted jobs.
7. Log in using the Admin account and verify complete administrative access.

**Dashboard Verification:**

Custom Jenkins views can also be created to organize jobs according to different teams.

Examples:

- Developer View
- DevOps View
- Testing View

Jenkins evaluates the user's global permissions and item-level permissions when determining which jobs the user can access.

**Reference Screenshots:**

- `image_4de01b.png`
- `image_4de3de.png`

## 5. Expected Result

After successful configuration:

- Admin users have complete Jenkins administrative access.
- Developer users can access jobs matching their assigned item role.
- DevOps users can access their permitted DevOps jobs.
- Testing users can access their permitted Testing jobs.
- Users cannot access jobs outside their assigned permissions.
- Jenkins dashboard views help organize jobs according to teams.

## 6. Troubleshooting

| Issue | Possible Solution |
|---|---|
| User cannot log in | Verify the Overall/Read permission. |
| Jobs are not visible | Check the assigned item role and Regex pattern. |
| User cannot trigger builds | Verify Job/Build permission. |
| User cannot configure jobs | Verify Job/Configure permission. |
| User can see unauthorized jobs | Review global and item role assignments and folder permissions. |

## 7. Conclusion

Jenkins Role-Based Authorization Strategy provides centralized access management by assigning permissions according to user responsibilities.

By configuring global roles and item roles, administrators can restrict access to specific jobs and maintain better security and organization within Jenkins.

This implementation separates Developer, DevOps, Testing, and Admin responsibilities and ensures that users receive only the permissions required for their work.

## 8. Configuration Status

| Parameter | Details |
|---|---|
| Status | Configuration Completed and Verified |
| Authorization Strategy | Role-Based Authorization Strategy |
| Access Levels | Global Level and Item Level |
| Teams Configured | Admin, Developer, DevOps, and Testing |
