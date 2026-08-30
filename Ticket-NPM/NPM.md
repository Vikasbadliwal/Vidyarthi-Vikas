# MAVEN | POM.XML | MAVEN PROJECT SETUP SOP

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer**    |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | ------------------ |
| vikas      | 31-08-2026     | 1.0         |                     |                    | Deepak Kushwaha | Faisal/Mohit K  | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Maven](#2-what-is-maven)
3. [What is pom.xml](#3-what-is-pomxml)
4. [Prerequisites](#4-prerequisites)
5. [Step-by-Step Maven and pom.xml Setup](#5-step-by-step-maven-and-pomxml-setup)

   * [5.1 Install Java](#51-install-java)
   * [5.2 Verify Java](#52-verify-java)
   * [5.3 Install Maven](#53-install-maven)
   * [5.4 Verify Maven](#54-verify-maven)
   * [5.5 Create Maven Project](#55-create-maven-project)
   * [5.6 Understand Project Structure](#56-understand-project-structure)
   * [5.7 Create pom.xml](#57-create-pomxml)
   * [5.8 Add Dependencies](#58-add-dependencies)
   * [5.9 Build the Project](#59-build-the-project)
   * [5.10 Run Tests](#510-run-tests)
   * [5.11 Package the Application](#511-package-the-application)
6. [Important pom.xml Sections](#6-important-pomxml-sections)
7. [Common Maven Commands](#7-common-maven-commands)
8. [Maven Build Workflow](#8-maven-build-workflow)
9. [Best Practices](#10-best-practices)
10. [Troubleshooting](#11-troubleshooting)
11. [Conclusion](#12-conclusion)
12. [FAQs](#13-faqs)
13. [References](#14-references)

---

# 1. Introduction

Maven is a build and dependency management tool commonly used for Java applications.

Maven uses a file called **`pom.xml`** to define project information, dependencies, plugins, and build configuration.

The basic flow is:

```text
Java
  |
  v
Maven
  |
  v
pom.xml
  |
  +---- Dependencies
  |
  +---- Plugins
  |
  +---- Build Configuration
  |
  v
Build / Test / Package
```

This SOP explains how to install Java and Maven, create a Maven project, configure `pom.xml`, add dependencies, build the project, and troubleshoot common issues.

---

# 2. What is Maven?

**Maven** is a build automation and dependency management tool for Java projects.

Maven helps developers to:

* Compile Java code.
* Download dependencies.
* Run tests.
* Package applications.
* Manage project configuration.
* Run build plugins.
* Support CI/CD pipelines.

Example:

```bash
mvn clean install
```

This command performs a Maven build using the project's `pom.xml`.

---

# 3. What is pom.xml?

`pom.xml` stands for **Project Object Model XML**.

It is the main configuration file of a Maven project.

It contains information such as:

| Section        | Purpose                                    |
| -------------- | ------------------------------------------ |
| `groupId`      | Identifies the project or organization     |
| `artifactId`   | Name of the application/package            |
| `version`      | Project version                            |
| `dependencies` | External libraries required by the project |
| `plugins`      | Tools used during the build                |
| `properties`   | Reusable configuration values              |
| `build`        | Build-related configuration                |

Example:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0.0</version>

    <dependencies>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.18.0</version>
        </dependency>
    </dependencies>
</project>
```

---

# 4. Prerequisites

Before setting up Maven, install:

| Requirement | Purpose                                     |
| ----------- | ------------------------------------------- |
| Java JDK    | Required to run and build Java applications |
| Maven       | Build and dependency management             |
| Terminal    | Execute Maven commands                      |
| Git         | Recommended for source control              |

Verify Java:

```bash
java -version
```

Verify Maven:

```bash
mvn -version
```

---

# 5. Step-by-Step Maven and pom.xml Setup

# 5.1 Install Java

Maven requires a Java Development Kit (JDK).

On Ubuntu/Debian-based systems:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

> Use the Java version required by your project. The example above uses Java 17.

---

# 5.2 Verify Java

Run:

```bash
java -version
```

Also check the compiler:

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

# 5.3 Install Maven

On Ubuntu/Debian:

```bash
sudo apt update
sudo apt install maven -y
```

This installs Maven using the operating system package manager.

---

# 5.4 Verify Maven

Run:

```bash
mvn -version
```

Example:

```text
Apache Maven 3.x.x
Java version: 17.x.x
```

Maven should also display the Java version being used.

If Maven returns a version successfully, the installation is complete.

---

# 5.5 Create Maven Project

Create a project directory:

```bash
mkdir my-maven-app
cd my-maven-app
```

Create the basic project files:

```bash
mkdir -p src/main/java
mkdir -p src/test/java
```

Create the Maven configuration file:

```bash
touch pom.xml
```

The project now contains:

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

# 5.6 Understand Project Structure

A standard Maven project follows this structure:

```text
my-maven-app/
│
├── pom.xml
│
└── src/
    ├── main/
    │   ├── java/
    │   └── resources/
    │
    └── test/
        ├── java/
        └── resources/
```

| Directory            | Purpose                             |
| -------------------- | ----------------------------------- |
| `src/main/java`      | Application source code             |
| `src/main/resources` | Application resources/configuration |
| `src/test/java`      | Test code                           |
| `src/test/resources` | Test resources                      |
| `pom.xml`            | Maven project configuration         |

---

# 5.7 Create pom.xml

Add the following basic configuration:

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

The important project coordinates are:

```text
groupId    = com.example
artifactId = my-maven-app
version    = 1.0.0
```

---

# 5.8 Add Dependencies

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

The dependency structure is:

```text
Dependency
   |
   +-- groupId
   |
   +-- artifactId
   |
   +-- version
```

Maven downloads the required dependency automatically during the build.

---

# 5.9 Build the Project

From the directory containing `pom.xml`, run:

```bash
mvn compile
```

Maven reads `pom.xml` and compiles the source code.

To perform a complete build:

```bash
mvn clean install
```

---

# 5.10 Run Tests

Run project tests:

```bash
mvn test
```

Maven executes the tests configured for the project.

Expected result:

```text
Tests run: X
Failures: 0
Errors: 0
```

If tests fail, Maven reports the failure and the build should be investigated before deployment.

---

# 5.11 Package the Application

Create the application package:

```bash
mvn package
```

Depending on the project, Maven may generate:

```text
target/
└── my-maven-app-1.0.0.jar
```

The generated artifact can then be used for deployment.

---

# 6. Important pom.xml Sections

## 6.1 modelVersion

Defines the POM model version.

```xml
<modelVersion>4.0.0</modelVersion>
```

---

## 6.2 groupId

Identifies the organization or project group.

```xml
<groupId>com.example</groupId>
```

---

## 6.3 artifactId

Identifies the application or package.

```xml
<artifactId>my-maven-app</artifactId>
```

---

## 6.4 version

Defines the project version.

```xml
<version>1.0.0</version>
```

---

## 6.5 properties

Properties allow reusable configuration values.

Example:

```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>
```

---

## 6.6 dependencies

Defines libraries required by the application.

```xml
<dependencies>

    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.18.0</version>
    </dependency>

</dependencies>
```

---

## 6.7 plugins

Plugins perform specific build tasks.

Example:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

---

# 7. Common Maven Commands

| Command               | Purpose                                |
| --------------------- | -------------------------------------- |
| `mvn -version`        | Check Maven version                    |
| `mvn validate`        | Validate the project                   |
| `mvn compile`         | Compile source code                    |
| `mvn test`            | Run tests                              |
| `mvn package`         | Create application package             |
| `mvn verify`          | Run verification steps                 |
| `mvn install`         | Install artifact into local repository |
| `mvn clean`           | Remove previous build output           |
| `mvn clean install`   | Clean and build the project            |
| `mvn dependency:tree` | Display dependency tree                |

---

# 8. Maven Build Workflow

The normal Maven workflow is:

```text
Developer
    |
    v
Create / Update pom.xml
    |
    v
Add Dependencies
    |
    v
Write Source Code
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
    |
    v
Deployment
```

A common build command is:

```bash
mvn clean install
```

---


---

# 9. Best Practices

| Best Practice                         | Reason                              |
| ------------------------------------- | ----------------------------------- |
| Keep `pom.xml` in Git                 | Required for reproducible builds    |
| Define required dependency versions   | Makes builds predictable            |
| Avoid unnecessary dependencies        | Reduces project complexity          |
| Run `mvn test` before deployment      | Detects application issues          |
| Review dependency updates             | Prevent unexpected changes          |

---

# 10. Troubleshooting

## 10.1 Maven Command Not Found

Error:

```text
mvn: command not found
```

Check:

```bash
mvn -version
```

If Maven is not installed:

```bash
sudo apt update
sudo apt install maven -y
```

---

## 10.2 Java Not Found

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

Then verify:

```bash
java -version
```

---

## 10.3 JAVA_HOME Problem

Check:

```bash
echo $JAVA_HOME
```

Also check:

```bash
which java
```

Maven should use a valid Java installation.

Verify:

```bash
mvn -version
```

---

## 10.4 pom.xml Not Found

Error:

```text
The goal you specified requires a project to execute but there is no POM in this directory
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

Navigate to the Maven project:

```bash
cd <project-directory>
```

Then run:

```bash
mvn clean install
```

---

## 10.5 Dependency Download Failure

If Maven cannot download dependencies, check:

* Network connectivity.
* Maven repository configuration.
* Proxy configuration.
* Dependency coordinates.
* Repository availability.

Run:

```bash
mvn dependency:tree
```

Review the Maven error output for the failed dependency.

---

## 10.6 Compilation Failure

Run:

```bash
mvn compile
```

Check:

* Java version.
* Compiler configuration.
* Source code errors.
* Dependency versions.

Verify Java:

```bash
java -version
mvn -version
```

---

## 10.7 Test Failure

Run:

```bash
mvn test
```

Review the test output.

Do not proceed with deployment until required tests pass.

---

## 10.8 Clean Build

If the project has stale build files, run:

```bash
mvn clean
```

Then:

```bash
mvn clean install
```

---

# 11. Conclusion

Maven is used to build and manage Java applications.

The main configuration file is:

```text
pom.xml
```

The basic setup is:

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
Write Code
    ↓
mvn compile
    ↓
mvn test
    ↓
mvn package
    ↓
Deploy
```

### Important Commands

```bash
java -version

mvn -version

mvn compile

mvn test

mvn package

mvn clean install
```

The most important point to remember is:

> **Maven uses `pom.xml` to understand what the Java project needs and how it should be built.**

---

# 12. FAQs

### Q1. What is Maven?

Maven is a build automation and dependency management tool for Java projects.

### Q2. What is pom.xml?

`pom.xml` is the main configuration file used by Maven.

### Q3. Do we install pom.xml?

No. `pom.xml` is a project configuration file. We install Java and Maven, then create or configure `pom.xml`.

### Q4. What is a dependency?

A dependency is an external library required by the application.

### Q5. How do I check Maven?

```bash
mvn -version
```

---

# 13. References

| Topic                       | Description                                   |
| --------------------------- | --------------------------------------------- |
| Maven Documentation         | Official Maven documentation                  |
| Maven POM Reference         | POM configuration reference                   |
| Maven Getting Started       | Maven project and build guide                 |
| Maven Lifecycle             | Maven build lifecycle documentation           |
| Java Documentation          | Java installation and configuration reference |

---

# 15. Explanation


> **“First, we install Java because Maven requires Java. Then we install Maven and verify it using `mvn -version`. We create a Maven project with a `pom.xml` file. The `pom.xml` contains the project details and dependencies. When we run Maven commands, Maven reads the `pom.xml`, downloads the required dependencies, compiles the code, runs tests, and creates the application artifact.

### One-line workflow

```text
Java → Maven → pom.xml → Dependencies → Compile → Test → Package → Deploy
```
