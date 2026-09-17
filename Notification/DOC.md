

<h1 align="left"> Notification Worker | Detailed documentation</h1>

---

## Author Information

| Author | Created On | Version | Last Updated By | Last Updated On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | --------------- | ---------------------- | -------------- | ------------------ |
| Vikas  | 27-08-2026 | v1.0    | vikas           | 27-08-2026      | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |
| Vikas  |            |         |                 |                 | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |

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
   * [5.4 Notification Processing Flow](#54-notification-processing-flow)
   * [5.5 Email Delivery Flow](#55-email-delivery-flow)
6. [Application Workflow](#6-application-workflow)
7. [Execution Modes](#7-execution-modes)

   * [7.1 External Mode](#71-external-mode)
   * [7.2 Scheduled Mode](#72-scheduled-mode)
8. [Configuration](#8-configuration)
9. [Logging and Monitoring](#9-logging-and-monitoring)

   * [9.1 Application Logs](#91-application-logs)
   * [9.2 Health Monitoring](#92-health-monitoring)
   * [9.3 Metrics](#93-metrics)
10. [Troubleshooting](#10-troubleshooting)
11. [FAQs](#11-faqs)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

The **Notification Worker** is a Python-based application responsible for sending notification emails based on employee information stored in Elasticsearch.

The application retrieves employee records from the `employee-management` index and uses the employee email information to generate salary-slip notifications.

The Notification Worker communicates with an SMTP server for email delivery. During the POC, **MailHog** is used as the SMTP server to safely capture and verify outgoing emails.

---

# 2. Purpose

The purpose of the Notification Worker is to provide an automated mechanism for delivering employee-related notifications through email.

The application separates notification processing from other employee-management services and retrieves the required employee information from Elasticsearch.

For testing purposes, MailHog acts as a local email server. It captures the generated emails so that the notification flow can be verified without sending messages to real external recipients.

---

# 3. Key Objectives

| Objective                   | Description                                                           |
| :-------------------------- | :-------------------------------------------------------------------- |
| **Employee Data Retrieval** | Retrieve employee information from Elasticsearch.                     |
| **Notification Processing** | Process employee records and prepare notification messages.           |
| **Email Delivery**          | Send notification emails through SMTP.                                |
| **Scheduled Processing**    | Support recurring notification execution.                             |
| **One-Time Processing**     | Support external execution for individual notification runs.          |
| **Email Verification**      | Allow captured emails to be verified using MailHog.                   |
| **Multiple Recipients**     | Support notification processing for multiple employee records.        |
| **Independent Service**     | Operate as a separate worker responsible for notification processing. |

---

# 4. Pre-Requisites

The Notification Worker depends on a Python runtime, Elasticsearch for employee information, and an SMTP service for email communication.

---

## 4.1 System Requirements

| Requirement                | Configuration  |
| :------------------------- | :------------- |
| **Cloud Platform**         | AWS            |
| **Deployment Environment** | Amazon EC2     |
| **Operating System**       | Ubuntu Linux   |
| **Architecture**           | x86_64 / AMD64 |
| **Python**                 | 3.14.4         |
| **Java**                   | OpenJDK 21     |
| **Elasticsearch**          | 7.17.29        |
| **MailHog**                | v1.0.1         |

The application is designed to run in a Linux-based environment with the required runtime dependencies.

---

## 4.2 Dependencies

### 4.2.1 Build Time Dependencies

| Dependency      | Version | Purpose                       |
| :-------------- | :------ | :---------------------------- |
| **Python**      | 3.14.4  | Application runtime           |
| **pip**         | Native  | Python package management     |
| **Git**         | 2.53.0  | Application source management |
| **Python venv** | Native  | Isolated Python environment   |

---

### 4.2.2 Run Time Dependencies

| Dependency                      | Version | Purpose                                                             |
| :------------------------------ | :------ | :------------------------------------------------------------------ |
| **Elasticsearch**               | 7.17.29 | Stores and provides employee records                                |
| **config-with-yaml**            | 0.1.0   | Handles YAML-based application configuration                        |
| **elasticsearch Python client** | 7.8.0   | Provides communication between Python application and Elasticsearch |
| **emails**                      | 0.6     | Supports email message creation and delivery                        |
| **schedule**                    | 0.6.0   | Supports scheduled notification execution                           |
| **MailHog**                     | v1.0.1  | Captures SMTP emails during testing                                 |

---

### 4.2.3 Other Dependencies

| Dependency          | Purpose                           |
| :------------------ | :-------------------------------- |
| **Java OpenJDK 21** | Required by Elasticsearch         |
| **SMTP**            | Email communication protocol      |
| **curl**            | API and service verification      |
| **MailHog Web UI**  | Email verification during the POC |

---

## 4.3 Important Ports

### 4.3.1 Inbound Traffic

| Port     | Purpose                |
| :------- | :--------------------- |
| **22**   | SSH access             |
| **9200** | Elasticsearch API      |
| **1025** | MailHog SMTP           |
| **8025** | MailHog Web UI and API |

Port `8025` is used to access captured notification emails through the MailHog interface.

---

### 4.3.2 Outbound Traffic

| Port     | Communication                        |
| :------- | :----------------------------------- |
| **9200** | Notification Worker → Elasticsearch  |
| **1025** | Notification Worker → SMTP / MailHog |

---

## 5.2 Core Components

| Component               | Responsibility                                                            |
| :---------------------- | :------------------------------------------------------------------------ |
| **Elasticsearch**       | Stores employee records and provides employee information to the worker.  |
| **Notification Worker** | Retrieves employee data and processes notification requests.              |
| **Notification Logic**  | Creates notification content based on the available employee information. |
| **SMTP**                | Provides the communication mechanism for sending emails.                  |
| **MailHog**             | Captures outgoing emails during the POC.                                  |
| **MailHog Web UI**      | Allows users to view and verify captured emails.                          |

---

## 5.3 Dataflow Diagram

```text
+----------------+
| Elasticsearch  |
| Employee Data  |
+-------+--------+
        |
        | Employee Records
        v
+----------------------+
| Notification Worker  |
+----------+-----------+
           |
           | Notification
           v
+----------------+
| SMTP Server    |
+-------+--------+
        |
        | Email
        v
+----------------+
|    MailHog     |
+-------+--------+
        |
        v
+----------------+
| Captured Email |
+----------------+
```
---

## 5.4 Notification Processing Flow

The Notification Worker processes notifications through the following logical stages:

### 1. Read Employee Data

The worker communicates with Elasticsearch and reads employee records from the `employee-management` index.

### 2. Identify Recipient

The employee record contains the email address used as the notification recipient.

### 3. Prepare Notification

The application prepares the salary-slip notification using the available employee information.

### 4. Establish SMTP Communication

The worker communicates with the configured SMTP server.

### 5. Send Notification

The notification message is submitted to the SMTP server.

### 6. Capture Email

During the POC, MailHog receives and stores the message.

### 7. Verify Notification

The captured message can be reviewed through the MailHog Web UI.

---

## 5.5 Email Delivery Flow

The email delivery process can be represented as:

```text
Employee Record
      |
      v
Email ID Retrieved
      |
      v
Notification Created
      |
      v
SMTP Connection
      |
      v
Email Submitted
      |
      v
MailHog Receives Email
      |
      v
Email Stored
      |
      v
Email Available in Web UI
```

This flow allows the complete notification lifecycle to be verified without depending on an external email provider.

---

# 6. Application Workflow

The Notification Worker follows a straightforward workflow:

```text
Start
  |
  v
Load Configuration
  |
  v
Connect to Elasticsearch
  |
  v
Read Employee Records
  |
  v
Identify Email Recipients
  |
  v
Create Notification
  |
  v
Connect to SMTP
  |
  v
Send Email
  |
  v
MailHog Captures Email
  |
  v
Notification Completed
```

### Workflow Explanation

**Load Configuration**

The application loads configuration information required for Elasticsearch and SMTP communication.

**Connect to Elasticsearch**

The worker establishes communication with Elasticsearch to access employee information.

**Read Employee Records**

Employee records are retrieved from the configured Elasticsearch index.

**Identify Recipients**

The worker uses the employee email information to determine where the notification should be delivered.

**Create Notification**

The notification content is prepared for the selected employee or employees.

**Send Email**

The worker sends the notification through the configured SMTP server.

**Capture and Verify**

In the POC environment, MailHog captures the email and makes it available for verification.

---

# 7. Execution Modes

The Notification Worker provides two execution modes to support different notification requirements.

---

## 7.1 External Mode

External mode is intended for **one-time notification processing**.

In this mode:

1. The application starts.
2. Employee information is retrieved.
3. Notifications are generated.
4. Emails are sent through SMTP.
5. Processing completes.
6. The application exits.

---

## 7.2 Scheduled Mode

Scheduled mode is intended for **continuous notification processing**.

In this mode, the worker remains active and executes notification processing according to the configured schedule.

The current application configuration uses an hourly schedule.

```text
Start
  ↓
Wait for Schedule
  ↓
Read Employees
  ↓
Process Notifications
  ↓
Send Emails
  ↓
Wait for Next Schedule
  ↓
Repeat
```

Scheduled mode is useful when notifications need to be processed automatically at regular intervals.

---

# 8. Configuration

The Notification Worker uses configuration values for its external dependencies.

The major configuration areas are:

| Configuration                   | Purpose                                                      |
| :------------------------------ | :----------------------------------------------------------- |
| **SMTP Configuration**          | Defines the sender, SMTP server, port, and TLS behavior.     |
| **Elasticsearch Configuration** | Defines Elasticsearch connection details and authentication. |
| **Execution Configuration**     | Determines how the worker processes notifications.           |

### SMTP Configuration

The SMTP configuration controls how the application communicates with the email server.

Important parameters include:

| Parameter       | Purpose                                    |
| :-------------- | :----------------------------------------- |
| **from**        | Defines the sender email address.          |
| **username**    | SMTP authentication username.              |
| **password**    | SMTP authentication password.              |
| **smtp_server** | SMTP server address.                       |
| **smtp_port**   | SMTP communication port.                   |
| **tls**         | Controls TLS usage for SMTP communication. |

### Elasticsearch Configuration

Important Elasticsearch parameters include:

| Parameter    | Purpose                                |
| :----------- | :------------------------------------- |
| **username** | Elasticsearch authentication username. |
| **password** | Elasticsearch authentication password. |
| **host**     | Elasticsearch server address.          |
| **port**     | Elasticsearch API port.                |

> The POC uses MailHog with non-TLS SMTP communication. Production environments should use the security configuration required by the production SMTP service.

---

# 9. Logging and Monitoring

The Notification Worker uses application output and dependency status to monitor notification processing.

The POC does not include production-grade centralized monitoring or automated alerting.

Monitoring focuses on:

* Application execution
* Elasticsearch availability
* SMTP availability
* Notification processing
* Captured email verification

---

## 9.1 Application Logs

Application logs provide information about the notification processing lifecycle.

The logs can be used to understand:

* Whether the application started successfully
* Whether Elasticsearch communication was successful
* Whether employee records were retrieved
* Whether notification processing occurred
* Whether SMTP communication was successful
* Whether errors occurred during processing

MailHog also maintains logs related to SMTP communication and captured messages.

Elasticsearch service logs can be used when employee data retrieval fails.

---

## 9.2 Health Monitoring

The following components should be available for successful notification processing:

| Component               | Expected State                  |
| :---------------------- | :------------------------------ |
| **Elasticsearch**       | Available and reachable         |
| **Notification Worker** | Running successfully            |
| **SMTP Server**         | Available                       |
| **MailHog**             | Running during POC              |
| **Employee Records**    | Available in Elasticsearch      |
| **Email Capture**       | Notification visible in MailHog |

A failure in any required dependency can prevent successful notification processing.

---

## 9.3 Metrics

The following metrics are useful for validating the Notification Worker:

| Metric                         | Description                                   | Priority |
| :----------------------------- | :-------------------------------------------- | :------- |
| **Elasticsearch Availability** | Checks whether employee data can be accessed. | High     |
| **Employee Record Count**      | Confirms employee records are available.      | High     |
| **Notification Processing**    | Confirms records are processed by the worker. | High     |
| **SMTP Availability**          | Confirms the email server is reachable.       | High     |
| **Notification Delivery**      | Confirms emails are successfully submitted.   | High     |
| **Captured Messages**          | Confirms emails are available in MailHog.     | High     |

---

# 10. Troubleshooting

| Issue                                    | Possible Cause                                        | Resolution                                                |
| :--------------------------------------- | :---------------------------------------------------- | :-------------------------------------------------------- |
| **Employee data unavailable**            | Elasticsearch unavailable or index has no records     | Verify Elasticsearch availability and employee data.      |
| **Notification not generated**           | Employee record does not contain required information | Check employee record fields.                             |
| **SMTP connection failure**              | SMTP server unavailable or incorrect configuration    | Verify SMTP server and port configuration.                |
| **No email captured**                    | MailHog unavailable or incorrect SMTP configuration   | Verify MailHog and SMTP settings.                         |
| **MailHog UI unavailable**               | Web interface or network access issue                 | Verify MailHog Web UI availability and port access.       |
| **Scheduled processing not occurring**   | Scheduler is not running                              | Verify that the application is running in scheduled mode. |

---

# 11. FAQs

**Question:** What is the main responsibility of the Notification Worker?

**Answer:** The Notification Worker retrieves employee information from Elasticsearch, prepares notification emails, and sends them through SMTP.

---

**Question:** Does the Notification Worker directly send emails to employees?

**Answer:** The worker sends emails through the configured SMTP server. During the POC, MailHog is used instead of a real external email server so that messages can be safely captured and verified.

---

**Question:** What is the purpose of MailHog?

**Answer:** MailHog acts as a test SMTP server. It captures outgoing emails and provides a Web UI for viewing the messages.

---

**Question:** What is the difference between external and scheduled mode?

**Answer:**

| Mode          | Behavior                                                                                   |
| :------------ | :----------------------------------------------------------------------------------------- |
| **External**  | Processes the notification workflow as a one-time execution.                               |
| **Scheduled** | Keeps the worker running and processes notifications according to the configured schedule. |

---

**Question:** Why is Elasticsearch required?

**Answer:** Elasticsearch stores the employee records required by the Notification Worker. The worker uses these records to identify employees and their email addresses.

---

**Question:** Why is SMTP required?

**Answer:** SMTP provides the communication mechanism through which the Notification Worker sends email notifications.

---

**Question:** Can the application process multiple employees?

**Answer:** Yes. The worker can process multiple employee records and generate notifications for the corresponding recipients.

---

**Question:** Is the POC configuration suitable for production?

**Answer:** The POC configuration is intended for testing and validation. Production environments should use appropriate SMTP security, credentials management, monitoring, and operational controls.

---

# 12. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 13. References

| Reference                          | Description                                                |
| :--------------------------------- | :--------------------------------------------------------- |
| **Notification Worker Repository** | Source repository for the Notification Worker application. |
| **Documentation Template**         | Standard application documentation structure.              |
| **MailHog**                        | SMTP testing and email verification tool.                  |

---
