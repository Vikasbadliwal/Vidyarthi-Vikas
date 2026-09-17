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
<img width="1497" height="278" alt="postgreSQL screen-11" src="https://github.com/user-attachments/assets/99b71ae2-dc4a-435b-a7a8-93a33321e349" />

---

# 4. PostgreSQL Setup

## 4.1 Update the EC2 Instance

Update the package index and installed packages:

```bash
sudo apt update -y
sudo apt upgrade -y
```
---

## 4.2 Install PostgreSQL

Install PostgreSQL and additional PostgreSQL utilities:

```bash
sudo apt install postgresql postgresql-contrib -y
```

### Screenshot

<img width="1768" height="513" alt="postgreSQL screen-1" src="https://github.com/user-attachments/assets/9c8aaaef-e0df-41f2-ad06-96f67c701684" />

<img width="1569" height="54" alt="postgreSQL screen-2" src="https://github.com/user-attachments/assets/48b33d8c-c433-4147-b1a4-599b690b7d63" />

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

<img width="1559" height="147" alt="postgreSQL screen-3" src="https://github.com/user-attachments/assets/66700289-31ef-426c-93c7-2387f45cdd16" />

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

<img width="1560" height="136" alt="postgreSQL screen-4" src="https://github.com/user-attachments/assets/4130a6ec-01b6-4336-acea-5dd8c3aea893" />

<img width="1540" height="52" alt="postgreSQL screen-5" src="https://github.com/user-attachments/assets/ea1e5e1f-839a-46d1-b2fc-bf0a12fa438a" />


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
<img width="1540" height="684" alt="postgreSQL screen-6" src="https://github.com/user-attachments/assets/98c25fd5-10ed-4568-bedf-165fb4cab334" />


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
<img width="1500" height="690" alt="postgreSQL screen--7" src="https://github.com/user-attachments/assets/3e62bf5f-6ace-4db4-b998-49fa40f9c5d4" />

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
<img width="1500" height="421" alt="postgreSQL screen-10" src="https://github.com/user-attachments/assets/3a195c83-8449-4ed8-90b8-c3f9d65b710a" />

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
<img width="1500" height="489" alt="postgreSQL screen-8" src="https://github.com/user-attachments/assets/8d009d87-2a81-442c-b763-dde32bcbfbb7" />

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
<img width="1500" height="267" alt="postgreSQL screen-9" src="https://github.com/user-attachments/assets/df0f9236-fd22-4043-9281-48b3a812cbc1" />

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
