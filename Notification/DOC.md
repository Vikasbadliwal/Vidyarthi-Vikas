<p align="center">
<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/12e0d0bf-08ae-4954-8a81-f6cafc851e31" />

<br/>
</p>

<h1 align="left"> Notification Worker | Detailed documentation</h1>

---

## Author Information

| **Author** | **Created on** | **Version** | **Last edited on** | **L0 Reviewer**  | **L1 Reviewer** | **L2 Reviewer**      |
| :--------- | :------------- | :---------- | :----------------- | :--------------- | :-------------- | :------------------- |
| Sahil      | 16-08-26       | v1.0        | 17-09-26           | Vishal / Divya M | Aayush Verma    | Mahesh Kumar / Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Key Objectives](#3-key-objectives)
4. [Pre-Requisites](#4-pre-requisites)

   * [4.1 System Requirements](#41-system-requirements)
   * [4.2 Dependencies](#42-dependencies)

     * [4.2.1 Build Time Dependencies](#421-build-time-dependencies)
     * [4.2.2 Run Time Dependencies](#422-run-time-dependencies)
     * [4.2.3 Other Dependencies](#423-other-dependencies)
   * [4.3 Important Ports](#43-important-ports)

     * [4.3.1 Inbound Traffic](#431-inbound-traffic)
     * [4.3.2 Outbound Traffic](#432-outbound-traffic)
5. [Architecture](#5-architecture)

   * [5.1 Architecture Overview](#51-architecture-overview)
   * [5.2 Core Components](#52-core-components)
   * [5.3 Dataflow Diagram](#53-dataflow-diagram)
6. [POC-Specific Code Changes](#6-poc-specific-code-changes)
7. [Step-by-Step Installation Guide](#7-step-by-step-installation-guide)
8. [Logging and Monitoring](#8-logging-and-monitoring)

   * [8.1 Application Logs](#81-application-logs)
   * [8.2 Health Monitoring](#82-health-monitoring)
   * [8.3 Metrics](#83-metrics)
9. [Troubleshooting](#9-troubleshooting)
10. [FAQs](#10-faqs)
11. [Disaster Recovery & High Availability](#11-disaster-recovery--high-availability)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)
14. [POC Result Validation](#14-poc-result-validation)

---

# 1. Introduction

The **Notification Worker** is a Python-based service that retrieves employee records from Elasticsearch and sends salary-slip notification emails through SMTP.

The service supports both one-time and scheduled execution modes and is designed to process employee notification data independently.

---

# 2. Purpose

The purpose of the Notification Worker is to automate notification delivery based on employee information stored in Elasticsearch.

For this Proof of Concept (POC), MailHog is used as a local SMTP server to capture outgoing emails. This allows the complete notification flow to be tested without sending emails to real external recipients.

---

# 3. Key Objectives

| Objective                     | Description                                                                 |
| ----------------------------- | --------------------------------------------------------------------------- |
| **End-to-End Validation**     | Validate the complete notification workflow on an AWS EC2 instance.         |
| **Elasticsearch Integration** | Verify employee records can be retrieved successfully from Elasticsearch.   |
| **Email Notification**        | Validate generation and delivery of salary-slip notifications through SMTP. |
| **Secure Testing**            | Use MailHog to capture emails without sending them to real recipients.      |
| **Flexible Execution**        | Support both `external` and `scheduled` execution modes.                    |
| **UI Verification**           | Verify captured emails using the MailHog Web UI.                            |
| **Multiple Recipients**       | Validate notification delivery for single and multiple employee records.    |
| **Easy Troubleshooting**      | Provide logs and validation commands for identifying common issues.         |

---

# 4. Pre-Requisites

Before deploying the Notification Worker, ensure that the following hardware, software, and network requirements are available.

---

## 4.1 System Requirements

### Hardware Specifications

| Hardware                   | POC Configuration    |
| -------------------------- | -------------------- |
| **Cloud Platform**         | AWS                  |
| **Service**                | Amazon EC2           |
| **Processor Architecture** | Linux AMD64 / x86_64 |
| **Operating System**       | Ubuntu Linux         |
| **Deployment Type**        | Single EC2 Instance  |

---

## 4.2 Dependencies

### 4.2.1 Build Time Dependencies

| Name            | Version | Description                              |
| --------------- | ------- | ---------------------------------------- |
| **Python**      | 3.x     | Runtime for `notification_api.py`        |
| **pip**         | Native  | Installs Python application dependencies |
| **Git**         | 2.x     | Used to clone the application repository |
| **Python venv** | Native  | Creates an isolated Python environment   |

---

### 4.2.2 Run Time Dependencies

| Name                 | Version | Description                     |
| -------------------- | ------- | ------------------------------- |
| **Elasticsearch**    | 7.17.29 | Stores employee records         |
| **MailHog**          | v1.0.1  | Captures outgoing SMTP emails   |
| **config-with-yaml** | 0.1.0   | Loads application configuration |
| **elasticsearch**    | 7.8.0   | Python client for Elasticsearch |
| **emails**           | 0.6     | Python email-sending library    |
| **schedule**         | 0.6.0   | Handles scheduled execution     |

---

### 4.2.3 Other Dependencies

| Name     | Version    | Description                          |
| -------- | ---------- | ------------------------------------ |
| **Java** | OpenJDK 21 | Runtime required by Elasticsearch    |
| **curl** | Native     | Used for API and health verification |
| **ss**   | Native     | Used to verify network listeners     |

---

## 4.3 Important Ports

### 4.3.1 Inbound Traffic

| Port     | Description                         |
| -------- | ----------------------------------- |
| **22**   | SSH access to the EC2 instance      |
| **9200** | Elasticsearch API; accessed locally |
| **1025** | MailHog SMTP listener               |
| **8025** | MailHog Web UI / API                |

> Port `8025` must be allowed in the EC2 Security Group if the reviewer needs to access the MailHog Web UI remotely.

---

### 4.3.2 Outbound Traffic

| Port     | Communication                       |
| -------- | ----------------------------------- |
| **9200** | Notification Worker → Elasticsearch |
| **1025** | Notification Worker → MailHog SMTP  |

---

# 5. Architecture

## 5.1 Architecture Overview

The Notification Worker follows a simple notification-processing architecture.

The worker retrieves employee information from the `employee-management` index in Elasticsearch. It processes the employee records and generates salary-slip notification emails.

The generated emails are sent through SMTP. In this POC, MailHog acts as the SMTP server and captures the outgoing emails for verification.

This architecture provides:

* Simple notification processing
* Independent execution
* Elasticsearch integration
* SMTP-based communication
* Safe email testing
* Easy notification verification

---

## 5.2 Core Components

| Component               | Description                                               |
| ----------------------- | --------------------------------------------------------- |
| **Elasticsearch**       | Stores employee records used by the Notification Worker   |
| **Notification Worker** | Reads employee information and generates notifications    |
| **SMTP**                | Communication protocol used for sending emails            |
| **MailHog**             | Captures outgoing emails during the POC                   |
| **MailHog Web UI**      | Provides visual verification of captured emails           |
| **AWS EC2**             | Hosts the Notification Worker, Elasticsearch, and MailHog |

---

## 5.3 Dataflow Diagram

```text
+----------------+
| Elasticsearch  |
+-------+--------+
        |
        | Employee Data
        v
+----------------------+
| Notification Worker  |
+----------+-----------+
           |
           | SMTP
           v
+----------------+
|    MailHog     |
+-------+--------+
        |
        | Captured Email
        v
+----------------+
|   MailHog UI   |
+----------------+
```

**Data Flow:**

```text
Elasticsearch
      ↓
Notification Worker
      ↓
SMTP
      ↓
MailHog
      ↓
Captured Email
```

---

# 6. POC-Specific Code Changes

## SMTP TLS Configuration

The original application used TLS for SMTP communication:

```yaml
tls: True
```

For the MailHog-based POC, it was changed to:

```yaml
tls: False
```

This allows the Notification Worker to communicate with MailHog's non-TLS SMTP endpoint on port `1025`.

## Backup

The original application file was backed up as:

```text
notification_api.py.bak
```

## Important Note

> The `tls: False` configuration is specific to this POC. It should not be used in production without validating the production SMTP server and required security configuration.

---

# 7. Step-by-Step Installation Guide

## Step 1: Install Elasticsearch and Java

Download Elasticsearch:

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.29-amd64.deb
```

Install Elasticsearch:

```bash
sudo dpkg -i elasticsearch-7.17.29-amd64.deb
```

Configure Elasticsearch:

```bash
sudo vi /etc/elasticsearch/elasticsearch.yml
```

Add:

```yaml
cluster.name: notification-poc
node.name: notification-node-1
network.host: 127.0.0.1
http.port: 9200
discovery.type: single-node
```

Enable and start the service:

```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

Verify the service:

```bash
sudo systemctl status elasticsearch
```

Verify Elasticsearch:

```bash
curl -s http://127.0.0.1:9200
```

Verify Java:

```bash
java -version
```

---

## Step 2: Install and Start MailHog

> Binary installation is preferred over `go install` because of Go version compatibility constraints on the EC2 instance.

Download MailHog:

```bash
wget https://github.com/mailhog/MailHog/releases/download/v1.0.1/MailHog_linux_amd64
```

Make it executable:

```bash
chmod +x MailHog_linux_amd64
```

Move the binary:

```bash
sudo mv MailHog_linux_amd64 /usr/local/bin/mailhog
```

Start MailHog:

```bash
mailhog > /tmp/mailhog.log 2>&1 &
```

Verify the listeners:

```bash
sudo ss -lntp | grep -E ':(1025|8025)\b'
```

MailHog ports:

| Port     | Purpose      |
| -------- | ------------ |
| **1025** | SMTP         |
| **8025** | Web UI / API |

---

## Step 3: Build the Notification Worker

Clone the repository:

```bash
git clone https://github.com/OT-MICROSERVICES/notification-worker.git
```

Move into the application directory:

```bash
cd notification-worker
```

Create a Python virtual environment:

```bash
python3 -m venv venv
```

Activate the virtual environment:

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Verify Python:

```bash
python3 --version
```

---

## Step 4: Create Test Employee Data

Create test employee records in the `employee-management` Elasticsearch index.

### Employee 1

```bash
curl -X PUT "http://127.0.0.1:9200/employee-management/_doc/1" \
-H 'Content-Type: application/json' \
-d '{
  "employee_id": "POC001",
  "name": "POC Employee",
  "email_id": "employee01@example.com"
}'
```

### Employee 2

```bash
curl -X PUT "http://127.0.0.1:9200/employee-management/_doc/2" \
-H 'Content-Type: application/json' \
-d '{
  "employee_id": "POC002",
  "name": "POC Employee 2",
  "email_id": "employee02@example.com"
}'
```

### Verify Employee Count

```bash
curl -s http://127.0.0.1:9200/employee-management/_count
```

The count should be greater than zero.

---

## Step 5: Configure the Worker

Create or update `config.yaml`:

```bash
vi config.yaml
```

Use the following POC configuration:

```yaml
---
smtp:
  from: "test@example.com"
  username: "test"
  password: "test"
  smtp_server: "127.0.0.1"
  smtp_port: "1025"
  tls: False

elasticsearch:
  username: "elastic"
  password: "elastic"
  host: "127.0.0.1"
  port: 9200
```

Export the configuration path:

```bash
export CONFIG_FILE=$HOME/notification-worker/config.yaml
```

Verify:

```bash
echo $CONFIG_FILE
```

> **Security Note:** The credentials shown above are POC/test values. Production credentials should be stored securely and should not be hard-coded.

---

## Step 6: Execute the Application

### External Mode

The external mode executes the notification workflow once and exits.

```bash
python3 notification_api.py --mode external
```

This mode is useful for one-time testing and validation.

### Scheduled Mode

The scheduled mode keeps the application running and executes according to the configured schedule.

```bash
python3 notification_api.py --mode scheduled
```

The current application code uses an hourly schedule.

---

## Step 7: Verify Notification

### Verify Using MailHog API

```bash
curl -s http://127.0.0.1:8025/api/v1/messages
```

The response displays the emails captured by MailHog.

### Verify Using MailHog Web UI

Open the following URL:

```text
http://<EC2-PUBLIC-IP>:8025
```

The MailHog Web UI can be used to verify:

* Sender
* Recipient
* Subject
* Email body
* Captured notification

If the UI is accessed remotely, ensure that port `8025` is allowed in the EC2 Security Group.

---

# 8. Logging and Monitoring

The POC does not include production-grade monitoring or automated alerting.

Validation is performed manually using application logs, Elasticsearch APIs, service status commands, and MailHog.

---

## 8.1 Application Logs

| Service                 | Command / Location                 |
| ----------------------- | ---------------------------------- |
| **Notification Worker** | Terminal stdout                    |
| **MailHog**             | `/tmp/mailhog.log`                 |
| **Elasticsearch**       | `sudo journalctl -u elasticsearch` |
| **Elasticsearch Logs**  | `/var/log/elasticsearch/`          |

### Notification Worker Logs

```bash
python3 notification_api.py --mode external
```

### MailHog Logs

```bash
cat /tmp/mailhog.log
```

### Elasticsearch Logs

```bash
sudo journalctl -u elasticsearch
```

---

## 8.2 Health Monitoring

Health checks are performed manually.

| Name                    | Type   | Initial Delay | Period | Timeout | Success | Failure |
| ----------------------- | ------ | ------------- | ------ | ------- | ------- | ------- |
| **Elasticsearch**       | Manual | N/A           | N/A    | N/A     | 1       | 1       |
| **MailHog**             | Manual | N/A           | N/A    | N/A     | 1       | 1       |
| **Notification Worker** | Manual | N/A           | N/A    | N/A     | 1       | 1       |

### Elasticsearch Health Check

```bash
curl -s http://127.0.0.1:9200
```

### MailHog Listener Check

```bash
sudo ss -lntp | grep -E ':(1025|8025)\b'
```

### Notification Worker Check

```bash
python3 notification_api.py --mode external
```

---

## 8.3 Metrics

The following parameters are manually validated:

| Parameter                      | Description                               | Priority | Threshold                       |
| ------------------------------ | ----------------------------------------- | -------- | ------------------------------- |
| **Elasticsearch Availability** | Checks whether Elasticsearch is reachable | High     | Service reachable               |
| **Employee Document Count**    | Confirms employee records are available   | High     | At least 1                      |
| **MailHog SMTP Listener**      | Confirms SMTP endpoint is available       | High     | Port `1025` listening           |
| **Captured Messages**          | Confirms notification delivery            | High     | Message appears after execution |

---

# 9. Troubleshooting

| Issue                                | Resolution                            | Command                               |
| ------------------------------------ | ------------------------------------- | ------------------------------------- |
| **ModuleNotFoundError: `emails`**    | Install Python dependencies           | `pip install -r requirements.txt`     |
| **Elasticsearch not starting**       | Check service status and logs         | `sudo systemctl status elasticsearch` |
| **Elasticsearch connection failure** | Verify service and port `9200`        | `curl http://127.0.0.1:9200`          |
| **MailHog installation failure**     | Use the Linux AMD64 binary            | `wget <MailHog-binary>`               |
| **MailHog UI unavailable**           | Allow port `8025` in Security Group   | `sudo ss -lntp`                       |
| **No email in MailHog**              | Check SMTP port and TLS configuration | Verify `1025` and `tls: False`        |
| **Employee count is zero**           | Create employee documents             | Elasticsearch `curl` command          |

---

# 10. FAQs

**Question:** Does this POC send real emails?

**Answer:** No. MailHog captures outgoing emails locally, so test notifications are not delivered to real external recipients.

---

**Question:** Why is MailHog used?

**Answer:** MailHog provides a local SMTP endpoint and Web UI. It allows outgoing emails to be captured and verified safely during testing.

---

**Question:** Can the Notification Worker run on another cloud platform?

**Answer:** Yes. The POC was executed on AWS EC2, but the application itself is not dependent on AWS. A similar Linux-based environment can be used with the required dependencies.

---

**Question:** What is the difference between `external` and `scheduled` mode?

**Answer:**

| Mode          | Description                                                     |
| ------------- | --------------------------------------------------------------- |
| **External**  | Executes the notification workflow once and exits               |
| **Scheduled** | Keeps running and executes according to the configured schedule |

The current application uses an hourly schedule for scheduled execution.

---

**Question:** Which ports are used by MailHog?

**Answer:**

| Port   | Purpose      |
| ------ | ------------ |
| `1025` | SMTP         |
| `8025` | Web UI / API |

---

**Question:** Why is TLS disabled?

**Answer:** MailHog's SMTP endpoint used for this POC does not require TLS. Therefore, the POC configuration uses:

```yaml
tls: False
```

This setting must be reviewed before using the application with a production SMTP server.

---

# 12. Contact Information

| Role                   | Name  | Email                                                             |
| ---------------------- | ----- | ----------------------------------------------------------------- |
| **Author / POC Owner** | Sahil | [sahil.butola@mygurukulam.co](mailto:sahil.butola@mygurukulam.co) |

---

# 13. References

| Reference                                                                                                      | Description                               |
| -------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| [Notification Worker Repository](https://github.com/OT-MICROSERVICES/notification-worker)                      | Source repository for Notification Worker |
| [Documentation Template](https://github.com/OT-MICROSERVICES/documentation-template/wiki/Application-Template) | Documentation template                    |
| [MailHog GitHub](https://github.com/mailhog/MailHog)                                                           | MailHog project and reference             |

---
