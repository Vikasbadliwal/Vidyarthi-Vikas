# MAVEN | POM.XML | INSTALLATION & SETUP SOP

---

## Document Information

| Author | Created On | Version | Last Updated By | Last Edited On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | -------------- | --------------- | -------------- | ------------------ |
| Vikas  | 31-08-2026 | 1.0     |                 |                | Deepak Kushwaha | Faisal/Mohit K | Mahesh Kumar/Varun |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Step-by-Step Installation](#3-step-by-step-installation)

   * [3.1 Install Java](#31-install-java)
   * [3.2 Verify Java](#32-verify-java)
   * [3.3 Install Maven](#33-install-maven)
   * [3.4 Verify Maven](#34-verify-maven)
   * [3.5 Create Maven Project](#35-create-maven-project)
   * [3.6 Create pom.xml](#36-create-pomxml)
   * [3.7 Add Dependency](#37-add-dependency)
   * [3.8 Build Project](#38-build-project)
   * [3.9 Run Tests](#39-run-tests)
   * [3.10 Package Project](#310-package-project)
4. [Common Commands](#5-common-commands)
5. [Troubleshooting](#6-troubleshooting)
6. [Best Practices](#7-best-practices)
7. [Conclusion](#8-conclusion)
8. [Contact Information](#9-contact-information)
9. [References](#10-references)
10. [FAQs](#11-faqs)

---

# 1. Introduction

Maven is a build and dependency management tool used for Java projects.

Maven uses a configuration file called **`pom.xml`**.

This SOP explains how to install Java and Maven and create a basic Maven project with `pom.xml`.

---

# 2. Prerequisites

Before starting, make sure you have:

| Requirement         | Purpose                         |
| ------------------- | ------------------------------- |
| Linux server/system | Installation environment        |
| Java JDK            | Required by Maven               |
| Maven               | Build and dependency management |
| Terminal            | Run commands                    |
| Internet access     | Download packages/dependencies  |
| Git                 | Recommended for source control  |

---

# 3. Step-by-Step Installation

## 3.1 Install Java

Maven requires Java.

For Ubuntu/Debian:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

> Use the Java version required by your project.

---

## 3.2 Verify Java

Check Java:

```bash
java -version
```

Check Java compiler:

```bash
javac -version
```

Expected:

```text
openjdk version "17.x.x"
javac 17.x.x
```

If both commands return a version, Java is installed successfully.

---

## 3.3 Install Maven

Install Maven using the package manager:

```bash
sudo apt update
sudo apt install maven -y
```

Wait for the installation to complete.

---

## 3.4 Verify Maven

Run:

```bash
mvn -version
```

Expected output contains:

```text
Apache Maven 3.x.x
Java version: 17.x.x
```

If Maven displays its version, installation is successful.

---

## 3.5 Create Maven Project

Create a project directory:

```bash
mkdir my-maven-app
cd my-maven-app
```

Create the Maven directories:

```bash
mkdir -p src/main/java
mkdir -p src/test/java
```

Create `pom.xml`:

```bash
touch pom.xml
```

---

## 3.6 Create pom.xml

Open the file:

```bash
vi pom.xml
```

Add:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
         http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-maven-app</artifactId>
    <version>1.0.0</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```

Save the file.

### Important POM Fields

| Field        | Purpose                             |
| ------------ | ----------------------------------- |
| `groupId`    | Identifies the project/organization |
| `artifactId` | Name of the application             |
| `version`    | Application version                 |
| `properties` | Stores reusable configuration       |

---

## 3.7 Add Dependency

Dependencies are external libraries required by the application.

Add the following inside `<project>`:

```xml
<dependencies>

    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.18.0</version>
    </dependency>

</dependencies>
```

The structure is:

```text
Dependency
 ├── groupId
 ├── artifactId
 └── version
```

Maven downloads the dependency automatically during the build.

---

## 3.8 Build Project

Run:

```bash
mvn compile
```

Maven reads `pom.xml` and compiles the source code.

For a complete build:

```bash
mvn clean install
```

---

## 3.9 Run Tests

Run project tests:

```bash
mvn test
```

Expected result:

```text
Tests run: X
Failures: 0
Errors: 0
```

If tests fail, check the Maven output and fix the issue before deployment.

---
# 5. Common Commands

| Command               | Purpose                  |
| --------------------- | ------------------------ |
| `java -version`       | Check Java version       |
| `mvn -version`        | Check Maven version      |
| `mvn validate`        | Validate project         |
| `mvn compile`         | Compile code             |
| `mvn test`            | Run tests                |
| `mvn package`         | Create package           |
| `mvn clean`           | Remove build output      |
| `mvn install`         | Install artifact locally |
| `mvn clean install`   | Clean and build          |
| `mvn dependency:tree` | Display dependencies     |

### Recommended Build Command

```bash
mvn clean install
```

---

# 6. Troubleshooting

## 6.1 Maven Not Found

Error:

```text
mvn: command not found
```

Check:

```bash
mvn -version
```

Install Maven:

```bash
sudo apt update
sudo apt install maven -y
```

---

## 6.2 Java Not Found

Error:

```text
java: command not found
```

Check:

```bash
java -version
```

Install Java:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

Verify:

```bash
java -version
```

---

## 6.3 pom.xml Not Found

Error:

```text
There is no POM in this directory
```

Check:

```bash
pwd
ls -la
```

Check for POM:

```bash
ls -l pom.xml
```

Navigate to the project directory:

```bash
cd <project-directory>
```

Then run:

```bash
mvn clean install
```

---

## 6.4 Dependency Download Failure

If Maven cannot download dependencies, check:

* Internet connectivity.
* Repository configuration.
* Proxy configuration.
* Dependency name and version.

Run:

```bash
mvn dependency:tree
```

Review the Maven error message.

---

## 6.5 Compilation Failure

Run:

```bash
mvn compile
```

Check:

```bash
java -version
mvn -version
```

Also check:

* Java version.
* Compiler configuration.
* Source code errors.
* Dependency versions.

---

## 6.6 Test Failure

Run:

```bash
mvn test
```

Review the test output.

Do not continue to deployment until required tests pass.

---

# 7. Best Practices

| Practice                       | Reason                          |
| ------------------------------ | ------------------------------- |
| Keep `pom.xml` in Git          | Maintains project configuration |
| Define dependency versions     | Provides predictable builds     |
| Avoid unnecessary dependencies | Keeps project simple            |
| Run tests before deployment    | Detects issues early            |
| Review dependency changes      | Avoids unexpected updates       |
| Use the required Java version  | Prevents compatibility issues   |

---

# 8. Conclusion
The Maven installation and setup workflow provides a structured process for setting up and building Java applications. By installing and verifying Java and Maven, creating the project and pom.xml, and managing dependencies, developers can easily compile, test, and package applications. Overall, Maven simplifies the build process and helps maintain a consistent and manageable Java development environment.

The key point is:

> **Java is required by Maven, and Maven uses `pom.xml` to understand the project, dependencies, and build process.**

---

# 9. Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |

---

# 10. References

| Resource              | Purpose                             |
| --------------------- | ----------------------------------- |
| Maven Documentation   | Official Maven documentation        |
| Maven POM Reference   | POM configuration                   |
| Java Documentation    | Java installation and configuration |

---

# 11. FAQs

### Q1. What is Maven?

Maven is a build automation and dependency management tool for Java projects.

### Q2. What is `pom.xml`?

`pom.xml` is the main configuration file of a Maven project.

### Q3. Do we install `pom.xml`?

No. We install Java and Maven. `pom.xml` is then created inside the Maven project.

### Q4. Why is Java required?

Maven runs on Java, so a Java JDK is required.

### Q5. How do I verify Maven?

```bash
mvn -version
```
