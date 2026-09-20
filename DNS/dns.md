# DNS POC | Domain Setup for Application

<p align="center">
  <img width="90" height="auto" alt="dns-icon" src="https://img.icons8.com/fluency/96/domain.png" />
</p>

---

## Author Information

| Author | Created On | Version | Last Updated By | Last Edited On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | -------------- | ---------------------- | -------------- | ------------------ |
| Vikas  | 16-09-2026 | v1.1    |  Vikas          |  16-09-2026    | Deepak Kushwaha/Ayushi | Faisal/Mohit K | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Application Setup](#3-application-setup)
4. [Domain Setup](#4-domain-setup)
5. [DNS Validation](#5-dns-validation)
6. [POC Result](#6-poc-result)
7. [Conclusion](#7-conclusion)
8. [Contact Information](#8-contact-information)
9. [References](#9-references)

---

# 1. Introduction

This POC demonstrates how to configure a domain name for an application hosted on an AWS EC2 instance.

The application is hosted using **NGINX** on an Ubuntu EC2 instance. A domain is configured to resolve to the EC2 public IP, allowing the application to be accessed using a domain name instead of directly using the IP address.

### POC Flow

```text
AWS EC2
   |
   | Hosts Application
   v
NGINX
   |
   | Public IP
   v
DNS Domain
   |
   v
Application
```

---

# 2. Prerequisites

The following components are required:

* AWS account
* AWS EC2 instance
* Ubuntu/Linux system
* SSH private key
* Internet access
* DNS/domain provider account
* NGINX

### Required EC2 Security Group Rules

| Type  | Port | Source          |
| ----- | ---: | --------------- |
| SSH   |   22 | Your IP address |
| HTTP  |   80 | `0.0.0.0/0`     |
| HTTPS |  443 | `0.0.0.0/0`     |

---

# 3. Application Setup

## 3.1 Create EC2 Instance

Create an Ubuntu EC2 instance in AWS.

The POC used:

| Configuration    | Value         |
| ---------------- | ------------- |
| Instance Name    | `DNS_POC`     |
| Instance Type    | `t3.micro`    |
| Region           | `ap-south-1b` |
| Operating System | Ubuntu        |
| Public IPv4      | `3.110.51.28` |

After the instance is running, note its **Public IPv4 address**.

---

## 3.2 Install NGINX

Install NGINX:

```bash
sudo apt install -y nginx
```

Enable NGINX:

```bash
sudo systemctl enable nginx
```

Test the NGINX configuration:

```bash
sudo nginx -t
```

Expected result:

```text
syntax is ok
test is successful
```

Check the service:

```bash
sudo systemctl status nginx
```

Expected result:

```text
active (running)
```

---

## 3.3 Create Application

Create the application directory:

```bash
sudo mkdir -p /var/www/dns-poc
```

Create the application file:

```bash
sudo nano /var/www/dns-poc/index.html
```

Add the application HTML content and save the file.

The application displays a simple page showing that it is hosted on **AWS EC2** and served through **NGINX**.

---

## 3.4 Configure NGINX

Create the NGINX site configuration:

```bash
sudo nano /etc/nginx/sites-available/dns-poc
```

Configure the server to serve the application from:

```text
/var/www/dns-poc
```

Enable the configuration:

```bash
sudo ln -s /etc/nginx/sites-available/dns-poc /etc/nginx/sites-enabled/dns-poc
```

Remove the default configuration:

```bash
sudo rm /etc/nginx/sites-enabled/default
```

Validate NGINX:

```bash
sudo nginx -t
```

Reload NGINX:

```bash
sudo systemctl reload nginx
```

---

## 3.5 Validate Application

Check the application locally:

```bash
curl http://localhost
```

The application can also be accessed using the EC2 public IP:

```text
http://3.110.51.28
```

### Expected Result

The application page should be displayed successfully.

---

# 4. Domain Setup

## 4.1 Create Domain

For this POC, the domain was configured using **Hostinger**.

The configured domain was:

```text
devsecurity.shop
```

Create/configure the domain in the DNS provider dashboard.

The domain should be configured to point to the EC2 public IP:

```text
3.110.51.28
```

---

# 5. DNS Validation

## 5.1 Verify DNS Resolution

Run:

```bash
nslookup devsecurity.shop
```

Expected result:

```text
Name:    devsecurity.shop
Address: 3.110.51.28
```

This confirms that the domain resolves to the EC2 public IP.

---

## 5.2 Access Application Using Domain

Open the following URL in a browser:

```text
http://devsecurity.shop
```

### Expected Result

The application hosted on the AWS EC2 instance should be accessible using the configured domain.

### Validation Flow

```text
Domain
   |
   | DNS Resolution
   v
EC2 Public IP
   |
   v
NGINX
   |
   v
Application
```

---

# 6. POC Result

The DNS POC was successfully completed.

The domain:

```text
devsecurity.shop
```

was configured to point to the AWS EC2 public IP:

```text
3.110.51.28
```

The application was successfully accessed using the configured domain.

### POC Validation

| Validation                        | Result     |
| --------------------------------- | ---------- |
| EC2 instance created              | Successful |
| NGINX installed                   | Successful |
| Application configured            | Successful |
| NGINX configuration validated     | Successful |
| Application accessed using EC2 IP | Successful |
| DNS resolution verified           | Successful |
| Application accessed using domain | Successful |

---

# 7. Conclusion

This POC demonstrates the complete flow of hosting an application on an AWS EC2 instance and accessing it through a configured domain name.

```text
AWS EC2 → NGINX → Application
                ↑
             DNS Domain
```

The successful DNS resolution and browser validation confirm that the configured domain can be used to access the application.

---

# 8. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |


---

# 9. References

| Reference                    | Description                            |
| ---------------------------- | -------------------------------------- |
| DNS Documentation — Sprint-1 | Project DNS documentation reference    |
| AWS EC2 Documentation        | AWS EC2 configuration reference        |
| NGINX Documentation          | NGINX configuration reference          |
| DNS Provider Documentation   | Domain and DNS configuration reference |

---

