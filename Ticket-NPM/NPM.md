# MAVEN | POM.XML | INSTALLATION & SETUP SOP

---

# Author Table

| Author | Created On | Version | Last Updated By | Last Edited On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | -------------- | --------------- | -------------- | ------------------ |
| Vikas  | 31-08-2026 | 1.0     |                 |                | Deepak Kushwaha | Faisal/Mohit K | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Maven](#2-what-is-maven)
3. [What is pom.xml](#3-what-is-pomxml)
4. [Prerequisites](#4-prerequisites)
5. [Step-by-Step Installation and Setup](#5-step-by-step-installation-and-setup)
6. [Important pom.xml Sections](#6-important-pomxml-sections)
7. [Common Maven Commands](#7-common-maven-commands)
8. [Maven Workflow](#8-maven-workflow)
9. [Maven in CI/CD](#9-maven-in-cicd)
10. [Best Practices](#10-best-practices)
11. [Troubleshooting](#11-troubleshooting)
12. [Conclusion](#12-conclusion)
13. [FAQs](#13-faqs)
14. [References](#14-references)

---

# 1. Introduction

Maven is a build and dependency management tool mainly used for Java applications.

Maven uses a configuration file called **`pom.xml`**.

The `pom.xml` file defines:

* Project information
* Dependencies
* Plugins
* Build configuration

This SOP explains how to install Java and Maven, create a Maven project, configure `pom.xml`, and build the application.

---

# 2. What is Maven?

**Maven** is a tool used to build and manage Java projects.

Maven can:

| Function              | Purpose                                 |
| --------------------- | --------------------------------------- |
| Compile               | Converts Java source code into bytecode |
| Test                  | Runs application tests                  |
| Dependency Management | Downloads required libraries            |
| Package               | Creates JAR/WAR artifacts               |
| Build                 | Automates the application build process |

Example:

```bash
mvn clean package
```

---

# 3. What is pom.xml?

`pom.xml` means **Project Object Model XML**.

It is the main configuration file of a Maven project.

Simple structure:

```text
Java Project
     |
     v
  pom.xml
     |
     +---- Project Information
     |
     +---- Dependencies
     |
     +---- Plugins
     |
     +---- Build Configuration
     |
     v
Maven Build
```

Example:

```xml
<project>

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0.0</version>

</project>
```

### Important Point

> **Maven is the tool, and `pom.xml` is the configuration file used by Maven.**

---

# 4. Prerequisites

Before setting up Maven, install the following:

| Requirement | Purpose                                     |
| ----------- | ------------------------------------------- |
| Java JDK    | Required to run Maven and Java applications |
| Maven       | Build and dependency management             |
| Terminal    | Used to execute commands                    |
| Git         | Recommended for source control              |

Check whether Java is installed:

```bash
java -version
```

Check whether Maven is installed:

```bash
mvn -version
```

---

# 5. Step-by-Step Installation and Setup

## 5.1 Install Java

Maven requires Java.

For Ubuntu/Debian:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

> Use the Java version required by your project.

---

## 5.2 Verify Java Installation

Run:

```bash
java -version
```

Also check the Java compiler:

```bash
javac -version
```

Example:

```text
openjdk version "17.x.x"
javac 17.x.x
```

If both commands return a version, Java is installed successfully.

---

## 5.3 Install Maven

For Ubuntu/Debian:

```bash
sudo apt update
sudo apt install maven -y
```

Maven will be installed using the operating system package manager.

---

## 5.4 Verify Maven Installation

Run:

```bash
mvn -version
```

Example:

```text
Apache Maven 3.x.x
Java version: 17.x.x
```

This confirms that Maven is installed and can find Java.

---

## 5.5 Create Maven Project Directory

Create a project directory:

```bash
mkdir my-maven-app
cd my-maven-app
```

Create the required directories:

```bash
mkdir -p src/main/java
mkdir -p src/test/java
```

Create the POM file:

```bash
touch pom.xml
```

Project structure:

```text
my-maven-app/
├── pom.xml
└── src/
    ├── main/
    │   └── java/
    └── test/
        └── java/
```

---

## 5.6 Create Basic pom.xml

Open `pom.xml`:

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

---

## 5.7 Understand Project Coordinates

The following values identify the project:

```xml
<groupId>com.example</groupId>
<artifactId>my-maven-app</artifactId>
<version>1.0.0</version>
```

| Element      | Meaning                         |
| ------------ | ------------------------------- |
| `groupId`    | Organization/project identifier |
| `artifactId` | Application name                |
| `version`    | Application version             |

---

## 5.8 Add a Dependency

Dependencies are external libraries required by the application.

Example:

```xml
<dependencies>

    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.18.0</version>
    </dependency>

</dependencies>
```

Maven downloads the dependency when the project is built.

Dependency structure:

```text
Dependency
   |
   +-- groupId
   +-- artifactId
   +-- version
```

---

## 5.9 Validate pom.xml

Run:

```bash
mvn validate
```

This checks whether the Maven project configuration is valid.

If there is no error, the POM configuration is valid.

---

## 5.10 Compile the Project

Run:

```bash
mvn compile
```

Maven reads `pom.xml` and compiles the application source code.

---

## 5.11 Run Tests

Run:

```bash
mvn test
```

Maven executes the project's tests.

If the tests pass, continue to the packaging step.

---

## 5.12 Package the Application

Run:

```bash
mvn package
```

Maven creates the application artifact.

For a JAR project, the output is normally created inside:

```text
target/
```

Example:

```text
target/
└── my-maven-app-1.0.0.jar
```

---

## 5.13 Perform a Clean Build

To remove old build files and create a fresh build:

```bash
mvn clean package
```

For installation into the local Maven repository:

```bash
mvn clean install
```

---

# 6. Important pom.xml Sections

| Section        | Purpose                                 |
| -------------- | --------------------------------------- |
| `modelVersion` | Defines the POM model                   |
| `groupId`      | Identifies the project/organization     |
| `artifactId`   | Defines the application name            |
| `version`      | Defines project version                 |
| `properties`   | Stores reusable configuration           |
| `dependencies` | Defines required libraries              |
| `build`        | Contains build configuration            |
| `plugins`      | Provides additional build functionality |

Example:

```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>
```

---

# 7. Common Maven Commands

| Command               | Purpose                            |
| --------------------- | ---------------------------------- |
| `mvn -version`        | Check Maven version                |
| `mvn validate`        | Validate `pom.xml`                 |
| `mvn compile`         | Compile source code                |
| `mvn test`            | Run tests                          |
| `mvn package`         | Create JAR/WAR                     |
| `mvn clean`           | Remove previous build output       |
| `mvn clean package`   | Clean and package                  |
| `mvn clean install`   | Clean, build, and install artifact |
| `mvn dependency:tree` | Display dependencies               |

### Most Common Build Command

```bash
mvn clean package
```

---

# 8. Maven Workflow

The basic Maven workflow is:

```text
Create Project
      |
      v
Create pom.xml
      |
      v
Add Dependencies
      |
      v
Write Java Code
      |
      v
mvn validate
      |
      v
mvn compile
      |
      v
mvn test
      |
      v
mvn package
      |
      v
Application Artifact
```

---

# 9. Maven in CI/CD

Maven is commonly used in CI/CD to build and test Java applications.

Typical pipeline:

```text
Git Push
   |
   v
Checkout Code
   |
   v
Read pom.xml
   |
   v
Download Dependencies
   |
   v
Compile
   |
   v
Test
   |
   v
Package
   |
   v
Deploy
```

Common CI/CD command:

```bash
mvn clean package
```

The generated JAR/WAR can then be passed to the deployment stage.

---

# 10. Best Practices

| Best Practice                    | Reason                        |
| -------------------------------- | ----------------------------- |
| Keep `pom.xml` in Git            | Required to build the project |
| Define dependency versions       | Makes builds predictable      |
| Avoid unnecessary dependencies   | Reduces project complexity    |
| Run tests before deployment      | Helps detect issues           |
| Use `mvn clean package` in CI/CD | Produces a clean build        |
| Review dependency changes        | Prevents unexpected updates   |
| Keep `pom.xml` readable          | Easier maintenance            |
| Use the required Java version    | Avoids compatibility issues   |

---

# 11. Troubleshooting

## 11.1 Maven Command Not Found

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

## 11.2 Java Command Not Found

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

---

## 11.3 pom.xml Not Found

Error:

```text
There is no POM in this directory
```

Check the current directory:

```bash
pwd
ls -la
```

Make sure `pom.xml` exists:

```bash
ls -l pom.xml
```

Move to the correct project directory:

```bash
cd <project-directory>
```

Then run:

```bash
mvn clean package
```

---

## 11.4 Dependency Download Failure

Check:

* Network connection
* Dependency name
* Dependency version
* Maven repository configuration

Run:

```bash
mvn dependency:tree
```

Review the Maven error message to identify the failed dependency.

---

## 11.5 Compilation Failure

Run:

```bash
mvn compile
```

Check:

```bash
java -version
mvn -version
```

Also review:

* Java source code
* Java version
* Dependency versions
* `pom.xml`

---

## 11.6 Test Failure

Run:

```bash
mvn test
```

Review the test output.

Fix the failed tests before deployment.

---

## 11.7 Clean Build

If old build files are causing problems:

```bash
mvn clean
```

Then run:

```bash
mvn clean package
```

---

# 12. Conclusion

Maven is used to build and manage Java applications.

`pom.xml` is the main configuration file used by Maven.

The complete setup is:

```text
Install Java
     ↓
Install Maven
     ↓
Create Project
     ↓
Create pom.xml
     ↓
Add Dependencies
     ↓
Compile
     ↓
Test
     ↓
Package
     ↓
Deploy
```

### Important Commands

```bash
java -version
mvn -version
mvn validate
mvn compile
mvn test
mvn package
mvn clean package
```

### Simple Explanation for Reviewer

> **“First, we install Java because Maven requires Java. Then we install Maven and verify the installation. We create a Maven project and configure the `pom.xml`. The POM contains project details and dependencies. Maven reads this file, downloads the dependencies, compiles the code, runs tests, and creates the final JAR/WAR artifact.”**

---

# 13. FAQs

### Q1. What is Maven?

Maven is a build and dependency management tool for Java projects.

### Q2. What is pom.xml?

`pom.xml` is the main configuration file used by Maven.

### Q3. Do we install pom.xml?

No. We install Java and Maven. `pom.xml` is created as part of the Maven project.

### Q4. Why is pom.xml required?

It tells Maven about the project, dependencies, plugins, and build configuration.

### Q5. How do I check Maven?

```bash
mvn -version
```

### Q6. How do I build the project?

```bash
mvn clean package
```

### Q7. How do I run tests?

```bash
mvn test
```

### Q8. Where is the build output created?

Normally inside:

```text
target/
```

---

# 14. References

| Reference                   | Purpose                             |
| --------------------------- | ----------------------------------- |
| Maven Documentation         | Maven usage and configuration       |
| Maven POM Reference         | `pom.xml` configuration             |
| Maven Lifecycle             | Maven build lifecycle               |
| Maven Dependency Management | Dependency management               |
| Java Documentation          | Java installation and configuration |

---
