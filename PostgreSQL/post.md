# PostgreSQL Setup on AWS EC2 — POC

## Document Information

| Author | Created On | Version | Last Updated By | Last Updated On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | --------------- | ---------------------- | -------------- | ------------------ |
| Vikas  | 27-08-2026 | v1.0    | vikas           | 27-08-2026      | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |
| Vikas  |            |         |                 |                 | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |


---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [PostgreSQL Setup](#4-postgresql-setup)
5. [Database and User Configuration](#5-database-and-user-configuration)
6. [Validation](#6-validation)
7. [Troubleshooting](#7-troubleshooting)
8. [Best Practices](#8-best-practices)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

# 1. Introduction

This POC demonstrates the installation and basic configuration of PostgreSQL on an **AWS EC2 Ubuntu instance**. It covers package installation, service management, database and user creation, basic database operations, and validation.

The POC is intentionally limited to a **normal single-instance PostgreSQL setup**. PostgreSQL HA/replication is not included in this POC and can be handled as a separate bonus task.

---

# 2. Objective

The main objectives are:

- Set up PostgreSQL on an AWS EC2 Ubuntu instance.
- Start and enable the PostgreSQL service.
- Verify the PostgreSQL installation and listening port.
- Create a dedicated database and database user.
- Test database connectivity.
- Create a sample table and insert test data.
- Prepare a simple reviewer demo.
- Maintain rough notes that can later be reused for detailed documentation.

---

# 3. Prerequisites

| Prerequisite | Details |
|---|---|
| Cloud Platform | AWS |
| Compute | EC2 |
| Operating System | Ubuntu |
| EC2 User | `ubuntu` with `sudo` access |
| PostgreSQL | PostgreSQL 16 |
| PostgreSQL Port | `5432` |
| Access | SSH access to EC2 |
| Tools | `apt`, `psql`, `systemctl` |

Verify the OS:

```bash
lsb_release -a
```

Verify PostgreSQL if already installed:

```bash
psql --version
```

Check the PostgreSQL cluster:

```bash
pg_lsclusters
```

---

# 4. PostgreSQL Setup

## 4.1 Update the EC2 Instance

Update the package index and installed packages:

```bash
sudo apt update -y
sudo apt upgrade -y
```

### Rough Notes

- Logged in to the AWS EC2 Ubuntu instance.
- Updated the APT package index.
- Upgraded available system packages.

### Screenshot

**Screenshot 1:** Capture the terminal showing successful `apt update`/`apt upgrade`.

---

## 4.2 Install PostgreSQL

Install PostgreSQL and additional PostgreSQL utilities:

```bash
sudo apt install postgresql postgresql-contrib -y
```

Verify the installation:

```bash
psql --version
```

Expected:

```text
psql (PostgreSQL) 16.x
```

Check the cluster:

```bash
pg_lsclusters
```

Expected example:

```text
Ver Cluster Port Status Owner    Data directory
16  main    5432 online postgres /var/lib/postgresql/16/main
```

### Rough Notes

- Installed PostgreSQL using the Ubuntu APT package manager.
- PostgreSQL 16 is installed on the EC2 instance.
- PostgreSQL uses port `5432`.
- The `main` cluster is online.

### Screenshot

**Screenshot 2:** Capture `psql --version` and `pg_lsclusters` output.

---

## 4.3 Start and Enable PostgreSQL

Start PostgreSQL and configure it to start automatically after reboot:

```bash
sudo systemctl enable --now postgresql
```

Check the service:

```bash
sudo systemctl status postgresql --no-pager
```

Verify the service state:

```bash
sudo systemctl is-active postgresql
```

Expected:

```text
active
```

### Rough Notes

- PostgreSQL service started successfully.
- PostgreSQL enabled at system boot.
- Service status verified as active.

### Screenshot

**Screenshot 3:** Capture the PostgreSQL `systemctl status` output showing the service is active.

---

## 4.4 Verify PostgreSQL Configuration and Port

Check the PostgreSQL configuration file:

```bash
sudo -u postgres psql -c "SHOW config_file;"
```

For PostgreSQL 16 on Ubuntu, the configuration is normally:

```text
/etc/postgresql/16/main/postgresql.conf
```

Check the PostgreSQL port:

```bash
sudo -u postgres psql -c "SHOW port;"
```

Expected:

```text
5432
```

Check the listening socket:

```bash
sudo ss -lntp | grep 5432
```

### Rough Notes

- Confirmed the PostgreSQL configuration file location.
- Confirmed PostgreSQL is configured to use port `5432`.
- Verified PostgreSQL is listening on the expected port.

### Screenshot

**Screenshot 4:** Capture the configuration path, port, and `ss` output.

---

# 5. Database and User Configuration

## 5.1 Access PostgreSQL

Switch to the PostgreSQL administrative user:

```bash
sudo -u postgres psql
```

Verify the PostgreSQL version:

```sql
SELECT version();
```

List databases:

```sql
\l
```

List users/roles:

```sql
\du
```

Exit:

```sql
\q
```

### Rough Notes

- Accessed PostgreSQL using the default `postgres` administrative user.
- Verified PostgreSQL version.
- Reviewed available databases and roles.

### Screenshot

**Screenshot 5:** Capture `SELECT version();`, `\l`, and `\du`.

---

## 5.2 Create Database User and Database

Create a dedicated application/database user:

```bash
sudo -u postgres psql
```

Run:

```sql
CREATE USER attendance_user WITH PASSWORD 'ChangeThisPassword';
```

Create the database:

```sql
CREATE DATABASE attendance_db OWNER attendance_user;
```

Grant database privileges:

```sql
GRANT ALL PRIVILEGES ON DATABASE attendance_db TO attendance_user;
```

Verify:

```sql
\du
\l
```

Exit:

```sql
\q
```

### Rough Notes

- Created dedicated user `attendance_user`.
- Created database `attendance_db`.
- Assigned `attendance_user` as the database owner.
- Granted required database privileges.
- Avoided using the PostgreSQL superuser for application access.

### Screenshot

**Screenshot 6:** Capture `\du` and `\l` showing the created user and database.

> Do not capture or store the actual database password in screenshots or Jira comments.

---

# 6. Validation

## 6.1 Connect to the Database

Test the newly created user:

```bash
psql -h 127.0.0.1 -U attendance_user -d attendance_db -p 5432
```

Enter the password when prompted.

If the connection succeeds, verify:

```sql
SELECT current_database();
SELECT current_user;
```

### Rough Notes

- Tested PostgreSQL connectivity using the dedicated database user.
- Confirmed connection to `attendance_db`.
- Confirmed the authenticated PostgreSQL user.

### Screenshot

**Screenshot 7:** Capture successful `psql` login and the `current_database()` / `current_user` output.

---

## 6.2 Create a Test Table

Inside `psql`:

```sql
CREATE TABLE attendance (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    attendance_date DATE,
    status VARCHAR(20)
);
```

Verify the table:

```sql
\dt
```

### Rough Notes

- Created a sample `attendance` table.
- Verified the table using `\dt`.

### Screenshot

**Screenshot 8:** Capture the `CREATE TABLE` result and `\dt`.

---

## 6.3 Insert and Verify Test Data

Insert a test record:

```sql
INSERT INTO attendance (name, attendance_date, status)
VALUES ('Demo User', CURRENT_DATE, 'Present');
```

Query the data:

```sql
SELECT * FROM attendance;
```

Expected result will contain the inserted record.

Exit:

```sql
\q
```

### Rough Notes

- Inserted sample attendance data.
- Queried the table successfully.
- Confirmed PostgreSQL can create, insert, and retrieve data.

### Screenshot

**Screenshot 9:** Capture the `INSERT` and `SELECT` output.

---

### Demo Commands

Show the EC2/OS information:

```bash
hostname
lsb_release -a
```

Show PostgreSQL version:

```bash
psql --version
```

Show PostgreSQL cluster:

```bash
pg_lsclusters
```

Show service:

```bash
sudo systemctl status postgresql --no-pager
```

Show port:

```bash
sudo ss -lntp | grep 5432
```

Show database and user:

```bash
sudo -u postgres psql -c "\l"
sudo -u postgres psql -c "\du"
```

Show test data:

```bash
psql -h 127.0.0.1 -U attendance_user -d attendance_db \
-c "SELECT * FROM attendance;"
```

---

# 7. Troubleshooting

| Problem | Possible Cause | Solution |
|---|---|---|
| `psql: command not found` | PostgreSQL client not installed | Install PostgreSQL packages |
| PostgreSQL service inactive | Service not started | `sudo systemctl start postgresql` |
| Port 5432 not listening | PostgreSQL not running/configuration issue | Check `systemctl status` and `pg_lsclusters` |
| Password authentication failed | Incorrect password | Reset the database user's password |
| Database does not exist | Database creation was not completed | Check with `\l` |
| User cannot connect | Incorrect user/password/privileges | Check with `\du` and verify database ownership |
| Remote connection fails | Network/configuration not enabled | Check `listen_addresses`, `pg_hba.conf`, and AWS Security Group |

---

# 8. Best Practices

| Best Practice | Description |
|---|---|
| Dedicated DB User | Use a separate application user instead of the `postgres` superuser. |
| Strong Password | Use a strong password and never commit it to Git. |
| Restrict Network Access | Do not expose PostgreSQL `5432` publicly unless required. |
| Private Networking | Prefer private EC2/VPC communication for internal database access. |
| Service Management | Use `systemctl` to manage PostgreSQL. |
| Regular Backups | Configure and test PostgreSQL backups before production use. |
| Documentation | Maintain commands, configuration, issues, and screenshots in the Jira ticket. |

---

# 9. Conclusion

This POC demonstrates a normal single-instance PostgreSQL setup on an AWS EC2 Ubuntu server. PostgreSQL 16 was installed, configured as a system service, validated on port `5432`, and tested using a dedicated database and user. A sample table and record were created to confirm basic database operations.

PostgreSQL High Availability, streaming replication, automatic failover, and related HA components are intentionally excluded from this POC and can be handled as a separate bonus implementation.

---

# 10. Contact Information


| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |


---

# 11. References

| Topic                   |    Links                                                      |
| ----------------------- | ------------------------------------------------------------- |
| PostgreSQL Documentation  |   (https://www.postgresql.org/docs/)                                                |
| PostgreSQL Ubuntu Installation  |    (https://www.postgresql.org/download/linux/ubuntu/)                       |

---
