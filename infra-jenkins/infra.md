# Jenkins Infrastructure Setup 

<p align="center">
  <img width="200" height="200" alt="Jenkins Infrastructure" src="https://github.com/user-attachments/assets/b0044854-bf55-4fb4-82e2-6a4199a6eb22" />
</p>

# Document Information

| Author | Created On | Version | L0 Reviewer              | L1 Reviewer      | L2 Reviewer          |
| :----- | :--------- | :------ | :----------------------- | :--------------- | :------------------- |
| Vikas  | 05-10-2026 | 1.0     | Deepak Kushwaha / Ayushi | Faisal / Mohit K | Mahesh Kumar / Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Infrastructure Diagram](#3-infrastructure-diagram)
4. [Description of the Infrastructure](#4-description-of-the-infrastructure)
5. [Security Groups and NACL Details](#5-security-groups-and-nacl-details)
6. [Conclusion](#6-conclusion)
7. [Contact Information](#7-contact-information)
8. [References](#8-references)

---

# 1. Introduction

This document provides a simple and clear overview of the AWS cloud infrastructure setup for Jenkins. It details the network setup, required AWS resources, and firewall security rules needed to provide a secure and reliable environment for Jenkins CI operations.

---

# 2. Prerequisites

| Category              | Requirement               | Details / Purpose                                                         |
| :-------------------- | :------------------------ | :------------------------------------------------------------------------ |
| **Cloud Account**     | AWS Account               | IAM permissions for VPC, EC2, Subnets, Security Groups, and Route Tables. |
| **CI Tool**           | Jenkins                   | For continuous integration and automated build execution.                 |
| **Compute**           | Amazon EC2                | Hosts the Jenkins server and CI workloads.                                |
| **CLI Tool**          | AWS CLI                   | To verify credentials and manage AWS resources from the terminal.         |
| **SSH Key**           | RSA 2048-bit Key (`.pem`) | For secure remote administrative access to the Jenkins instance.          |
| **Network Knowledge** | CIDR & Subnetting         | Basic understanding of IP ranges and routing.                             |

---

# 3. Infrastructure Diagram

<img width="1282" height="1401" alt="Jenkins infrastructure setup" src="https://github.com/user-attachments/assets/1f716f96-1f35-47b3-acfa-65b6d8fd408d" />

### Workflow Steps:

| Step  | Stage                      | Description                                                                                         |
| :---- | :------------------------- | :-------------------------------------------------------------------------------------------------- |
| **1** | **Developer**              | Developer pushes application code to the Git repository.                                            |
| **2** | **Internet Gateway (IGW)** | Provides internet connectivity between the VPC and external services.                               |
| **3** | **Jenkins EC2**            | Receives the CI trigger and executes the configured Jenkins pipeline.                               |
| **4** | **Source Repository**      | Jenkins checks out the latest application source code.                                              |
| **5** | **Build and Test**         | Jenkins executes build and automated testing stages.                                                |
| **6** | **External Services**      | Jenkins communicates with required services such as dependency repositories and code-quality tools. |

---

# 4. Description of the Infrastructure

| Layer              | Resource Name               | Sizing / CIDR                  | Purpose & Requirement                                                    |
| :----------------- | :-------------------------- | :----------------------------- | :----------------------------------------------------------------------- |
| **Network**        | Virtual Private Cloud (VPC) | `10.0.0.0/16` (65,536 IPs)     | Provides an isolated network environment for Jenkins.                    |
| **Tier 1**         | Public Subnet               | `10.0.1.0/24`                  | Hosts the Jenkins EC2 instance.                                          |
| **Routing**        | Internet Gateway (IGW)      | 1 per VPC                      | Enables communication between Jenkins and the internet.                  |
| **Compute**        | Jenkins EC2 Instance        | `t3.medium` (2 vCPU, 4 GB RAM) | Runs Jenkins server and CI workloads.                                    |
| **Storage**        | EBS Volume                  | Attached to EC2                | Stores Jenkins configuration, jobs, plugins, workspaces, and build data. |
| **CI Tool**        | Jenkins                     | Jenkins LTS                    | Automates application build, testing, and CI processes.                  |
| **Source Control** | Git / GitHub                | External service               | Stores application source code accessed by Jenkins.                      |

---

# 5. Security Groups and NACL Details

### Security Groups (Stateful - Instance Level)

| Security Group   | Inbound Rules                                                       | Outbound Rules                    | Purpose                                                             |
| :--------------- | :------------------------------------------------------------------ | :-------------------------------- | :------------------------------------------------------------------ |
| **`sg-jenkins`** | Port `22` (SSH) from administrator IP                               | Port `443` (HTTPS) to `0.0.0.0/0` | Provides secure administrative access and external CI connectivity. |
| **`sg-jenkins`** | Port `8080` (Jenkins) from authorized users/network                 | Port `80` (HTTP) when required    | Allows users to access the Jenkins web interface.                   |
| **`sg-jenkins`** | Port `50000` only when Jenkins agents require inbound communication | Required CI traffic               | Supports Jenkins agent communication when configured.               |

### Network Access Control Lists (Stateless - Subnet Level)

| Subnet     | Rule # | Direction        | Protocol | Port Range             | Source / Destination | Action    |
| :--------- | :----- | :--------------- | :------- | :--------------------- | :------------------- | :-------- |
| **Public** | 100    | Inbound          | TCP      | 22                     | Administrator IP     | **ALLOW** |
| **Public** | 110    | Inbound          | TCP      | 8080                   | Authorized Network   | **ALLOW** |
| **Public** | 120    | Inbound          | TCP      | 1024–65535 (Ephemeral) | `0.0.0.0/0`          | **ALLOW** |
| **Public** | 100    | Outbound         | TCP      | 443                    | `0.0.0.0/0`          | **ALLOW** |
| **Public** | 110    | Outbound         | TCP      | 80                     | `0.0.0.0/0`          | **ALLOW** |
| **Public** | 120    | Outbound         | TCP      | 1024–65535             | `0.0.0.0/0`          | **ALLOW** |
| **All**    | *      | Inbound/Outbound | ALL      | ALL                    | `0.0.0.0/0`          | **DENY**  |

---

# 6. Conclusion

This cloud infrastructure design provides a secure and reliable environment for running Jenkins on AWS EC2.

By using a dedicated VPC, public subnet, Internet Gateway, Security Group, and Network ACL rules, Jenkins can securely communicate with source-code repositories and required CI services while maintaining controlled access to the Jenkins server.

---

# 7. Contact Information

| Name           | Email                                                                                   |
| :------------- | :-------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 8. References

| Resource Name                  | Link                                                                                             |
| :----------------------------- | :----------------------------------------------------------------------------------------------- |
| Jenkins Documentation          | [Jenkins](https://www.jenkins.io/doc/)                                                           |
| AWS Well-Architected Framework | [AWS Architecture](https://aws.amazon.com/architecture/well-architected/)                        |
| Amazon VPC Documentation       | [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/)                                  |
| Amazon EC2 Documentation       | [Amazon EC2](https://docs.aws.amazon.com/ec2/)                                                   |
| Security Groups Documentation  | [AWS Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) |
| Network ACL Documentation      | [AWS Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)       |
