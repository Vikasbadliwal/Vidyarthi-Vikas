<p align="center">

<img width="250" height="250" alt="image" src="https://github.com/user-attachments/assets/431df669-3fb6-4246-af51-836b4e02aade" />

</p>

---

# Flask + PostgreSQL on AWS EC2 — POC

## Author Table

| Author | Created On | Version | Last Updated By | Last Updated On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | --------------- | ---------------------- | -------------- | ------------------ |
| Vikas  | 27-08-2026 | v1.0    | vikas           | 27-08-2026      | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |
| Vikas  |            |         |                 |                 | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |


---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Project Setup](#4-project-setup)
5. [PostgreSQL Setup](#5-postgresql-setup)
6. [Flask Application](#6-flask-application)
7. [CRUD API Implementation](#7-crud-api-implementation)
8. [Validation](#8-validation)
9. [Troubleshooting](#9-troubleshooting)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

This POC demonstrates a simple **Employee Management API** using **Flask and PostgreSQL** on an AWS EC2 Ubuntu instance.

Flask acts as the application/API layer, while PostgreSQL stores employee data persistently.

The POC demonstrates database connectivity and basic **CRUD operations** through REST APIs.

### Architecture

```text
Client
   |
   v
Flask Application :5000
   |
   v
PostgreSQL :5432
   |
   v
employees table
```

---

# 2. Objective

The main objectives are:

* Install and configure PostgreSQL on AWS EC2.
* Create a PostgreSQL database and dedicated user.
* Create an `employees` table.
* Configure Flask to connect with PostgreSQL.
* Implement CRUD APIs.
* Validate data through both Flask APIs and PostgreSQL.
* Demonstrate PostgreSQL as the persistent data store.

---

# 3. Prerequisites

| Prerequisite     | Details                            |
| ---------------- | ---------------------------------- |
| Cloud Platform   | AWS                                |
| Compute          | EC2                                |
| Operating System | Ubuntu                             |
| Python           | Python 3                           |
| Framework        | Flask                              |
| Database         | PostgreSQL 16                      |
| Flask Port       | `5000`                             |
| PostgreSQL Port  | `5432`                             |
| Access           | SSH                                |
| Tools            | `apt`, `psql`, `curl`, `systemctl` |

---

# 4. Project Setup

Create the project directory:

```bash
mkdir -p ~/flask-postgres-poc
cd ~/flask-postgres-poc
```

Create and activate Python virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
```

Install required Python packages:

```bash
pip install Flask psycopg2-binary
```
<img width="1072" height="273" alt="PostToReady screen-1(SCRUM-78) " src="https://github.com/user-attachments/assets/e23c06f9-8ca8-4b08-997a-65f238377b84" />


---

# 5. PostgreSQL Setup

## 5.1 Install PostgreSQL

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
```

Check version:

```bash
psql --version
```

Expected:

```text
psql (PostgreSQL) 16.x
```
<img width="1072" height="273" alt="PostToReady screen-1(SCRUM-78) " src="https://github.com/user-attachments/assets/0fad7175-0305-4017-be3c-882720f68242" />


---

## 5.2 Verify PostgreSQL Service

```bash
sudo systemctl enable --now postgresql
sudo systemctl status postgresql --no-pager
```

Expected:

```text
Active: active (running)
```

<img width="1077" height="191" alt="PostToReady screen-2(SCRUM-78)" src="https://github.com/user-attachments/assets/dcec1a2d-575f-4625-be65-9adff992c2d1" />

---

## 5.3 Create Database and User

Access PostgreSQL:

```bash
sudo -u postgres psql
```

Create application user:

```sql
CREATE USER flask_user WITH PASSWORD 'Flask@123';
```

Create database:

```sql
CREATE DATABASE flask_poc;
```

Grant privileges:

```sql
GRANT ALL PRIVILEGES ON DATABASE flask_poc TO flask_user;
```

Connect to the database:

```sql
\c flask_poc
```

Grant schema privileges:

```sql
GRANT ALL ON SCHEMA public TO flask_user;
```

Verify:

```sql
\du
\l
```
<img width="1172" height="400" alt="PostToReady screnn-3(SCRUM-78)" src="https://github.com/user-attachments/assets/f882b567-bb5b-4cfa-a707-4cc7ba314d5c" />
<img width="1181" height="863" alt="PostToReady screen-4(SCRUM-78)" src="https://github.com/user-attachments/assets/a4c31c40-e709-4f07-bce3-ee57a2aac926" />


---

## 5.4 Test Database Connection

Exit PostgreSQL:

```sql
\q
```

Connect using the application user:

```bash
psql -h localhost -U flask_user -d flask_poc
```

Expected:

```text
flask_poc=>
```

Verify:

```sql
SELECT current_database();
SELECT current_user;
```

<img width="1181" height="863" alt="PostToReady screen-5(SCRUM-78)" src="https://github.com/user-attachments/assets/3ef7551e-e511-48f8-9d21-5c3a751599f9" />

Exit:

```sql
\q
```

---

# 6. Flask Application

The Flask application connects to PostgreSQL using `psycopg2`.

Database configuration:

```text
Database : flask_poc
User     : flask_user
Host     : localhost
Port     : 5432
```

The application runs on:

```text
0.0.0.0:5000
```

Start the application:

```bash
python3 app.py
```

Expected:

```text
* Running on all addresses (0.0.0.0)
* Running on http://127.0.0.1:5000
```

Test:

```bash
curl http://localhost:5000/
```

Expected:

```text
Flask application is running
```
<img width="1181" height="224" alt="PostToReady screen-6 (SCRUM-78)" src="https://github.com/user-attachments/assets/8ada76b7-8bf2-474a-919f-a2af65051abb" />


---

## 6.1 Flask to PostgreSQL Connection Test

```bash
curl http://localhost:5000/db-test
```

Expected response contains:

```json
{
  "status": "success",
  "message": "Flask connected to PostgreSQL successfully"
}
```
<img width="1702" height="94" alt="PostToReady screen-7(SCRUM-78)" src="https://github.com/user-attachments/assets/0ca5b3a9-7be8-4db8-9d05-3b191bbdb862" />


---

# 7. CRUD API Implementation

## 7.1 Create Employee — POST

```bash
curl -X POST http://localhost:5000/employees \
-H "Content-Type: application/json" \
-d '{"name":"Aman","department":"DevOps","salary":60000}'
```

Creates a new employee record in PostgreSQL.

<img width="1339" height="432" alt="PostToReady screen-8(SCRUM-78)" src="https://github.com/user-attachments/assets/1a185587-b139-4d18-bb05-f70ad33622d3" />

---

## 7.2 Read All Employees — GET

```bash
curl http://localhost:5000/employees
```

Returns all employees stored in PostgreSQL.

<img width="1786" height="119" alt="PostToReady screen-10(SCRUM-78)" src="https://github.com/user-attachments/assets/7940db80-0e12-4cd8-a448-2a1918ff4c42" />

---

## 7.3 Read One Employee — GET

```bash
curl http://localhost:5000/employees/1
```

Returns employee details for the requested ID.

<img width="1050" height="49" alt="PostToReady screen-11(SCRUM-78)" src="https://github.com/user-attachments/assets/1806120c-e0fc-46a8-9081-a39c21f74de2" />

---

## 7.4 Update Employee — PUT

```bash
curl -X PUT http://localhost:5000/employees/1 \
-H "Content-Type: application/json" \
-d '{"name":"Riya","department":"Cloud-DevOps","salary":70000}'
```

Updates the employee record in PostgreSQL.

<img width="1449" height="95" alt="PostToReady screen-9(SCRUM-78)" src="https://github.com/user-attachments/assets/302e33b0-536b-4bfd-a235-d4025f4ff48d" />

---

## 7.5 Delete Employee — DELETE

```bash
curl -X DELETE http://localhost:5000/employees/3
```

Deletes employee ID `3`.

<img width="1343" height="610" alt="PostToReady screen-12(SCRUM-78)" src="https://github.com/user-attachments/assets/666dec56-8101-4f59-b4c4-74ae1595795c" />

---

# 8. Validation

## 8.1 Verify Data Directly in PostgreSQL

Connect to the database:

```bash
psql -h localhost -U flask_user -d flask_poc
```

Run:

```sql
SELECT * FROM employees;
```

Expected current data:

```text
 id | name  |  department  | salary
----+-------+--------------+--------
  1 | Riya  | Cloud-DevOps | 70000
  2 | Vikas | DevOps       | 50000
```

<img width="934" height="564" alt="PostToReady screen-13(SCRUM-78)" src="https://github.com/user-attachments/assets/d85d9bf0-67cc-4ec3-bffe-51c23e4ecd4a" />


---

## 8.2 Final API Verification

Run:

```bash
curl http://localhost:5000/employees
```

The API should return the same employee records stored in PostgreSQL.

<img width="1786" height="119" alt="PostToReady screen-10(SCRUM-78)" src="https://github.com/user-attachments/assets/e59b3fca-a7ed-43fb-a70f-75c3d4285988" />

---

## CRUD Summary

| Operation | Method | Endpoint          | Purpose            |
| --------- | ------ | ----------------- | ------------------ |
| Create    | POST   | `/employees`      | Create employee    |
| Read All  | GET    | `/employees`      | Get all employees  |
| Read One  | GET    | `/employees/<id>` | Get employee by ID |
| Update    | PUT    | `/employees/<id>` | Update employee    |
| Delete    | DELETE | `/employees/<id>` | Delete employee    |

---

# 9. Troubleshooting

| Problem                     | Possible Cause             | Solution                                |
| --------------------------- | -------------------------- | --------------------------------------- |
| `psql: command not found`   | PostgreSQL not installed   | Install PostgreSQL packages             |
| PostgreSQL inactive         | Service stopped            | `sudo systemctl start postgresql`       |
| Flask connection error      | Incorrect DB configuration | Verify database, user and password      |
| `connection refused`        | PostgreSQL not running     | Check `systemctl status postgresql`     |
| API returns `404`           | Route not registered       | Verify route is before `app.run()`      |
| PostgreSQL permission error | User lacks privileges      | Grant privileges on database/schema     |
| Employee not found          | Invalid employee ID        | Verify using `SELECT * FROM employees;` |

---

# 10. Best Practices

| Best Practice       | Description                                                    |
| ------------------- | -------------------------------------------------------------- |
| Dedicated DB User   | Use an application-specific database user.                     |
| Password Security   | Never commit database passwords to Git.                        |
| Virtual Environment | Use a Python virtual environment for dependencies.             |
| Database Validation | Verify API changes directly in PostgreSQL.                     |
| Restricted Port     | Do not expose PostgreSQL port `5432` publicly unless required. |
| Input Validation    | Validate API request data before database operations.          |
| Backups             | Configure regular PostgreSQL backups for production workloads. |

---

# 11. Conclusion

This POC demonstrates an **Employee Management API using Flask and PostgreSQL on AWS EC2**.

PostgreSQL was installed and configured with a dedicated database and user. Flask was successfully connected to PostgreSQL, and CRUD operations were implemented and validated through REST APIs and direct PostgreSQL queries.

The POC establishes PostgreSQL as the **persistent source of truth** for employee data.

---

# 12. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 13. References

| Topic                          | Link                                              |
| ------------------------------ | ------------------------------------------------- |
| Flask Documentation            | https://flask.palletsprojects.com/                |
| PostgreSQL Documentation       | https://www.postgresql.org/docs/                  |
| PostgreSQL Ubuntu Installation | https://www.postgresql.org/download/linux/ubuntu/ |

---
