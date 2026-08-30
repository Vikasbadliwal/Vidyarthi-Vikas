# MAVEN | POM.XML | INSTALLATION & SETUP SOP

---

## Document Information

| Author | Created On | Version | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | -------------- | ------------------ |
| Vikas  | 31-08-2026 | 1.0     | Deepak Kushwaha | Faisal/Mohit K | Mahesh Kumar/Varun |

---

# Table of Contents

1. Introduction
2. What is Maven
3. What is pom.xml
4. Prerequisites
5. Step-by-Step Installation
6. Basic pom.xml Configuration
7. Common Maven Commands
8. Maven Workflow
9. Maven in CI/CD
10. Best Practices
11. Troubleshooting
12. Conclusion
13. FAQs
14. References

---

# 1. Introduction

Maven is a build and dependency management tool used for Java projects.

Maven uses a file called **`pom.xml`** to manage:

* Project information
* Dependencies
* Plugins
* Build configuration

This SOP explains how to install Maven and configure a basic `pom.xml` step by step.

---

# 2. What is Maven?

Maven helps to automate common Java project tasks.

| Task                  | Purpose                      |
| --------------------- | ---------------------------- |
| Compile               | Builds Java source code      |
| Test                  | Runs application tests       |
| Dependency Management | Downloads required libraries |
| Package               | Creates JAR/WAR files        |
| Build                 | Automates the project build  |

---

# 3. What is pom.xml?

`pom.xml` means **Project Object Model XML**.

It is the main configuration file of a Maven project.

Maven reads this file to understand the project and its dependencies.

```text
Java Project
     |
     v
  pom.xml
     |
     +---- Project Details
     +---- Dependencies
     +---- Plugins
     |
     v
Maven Build
```

Important point:

> **Maven is the tool. `pom.xml` is the configuration file used by Maven.**

---

# 4. Prerequisites

Before installing Maven, Java must be installed.

| Requirement | Purpose                         |
| ----------- | ------------------------------- |
| Java JDK    | Required by Maven               |
| Maven       | Build and dependency management |
| Terminal    | Run commands                    |
| Git         | Source control                  |

Check Java:

```bash
java -version
```

Check Java compiler:

```bash
javac -version
```

---

# 5. Step-by-Step Installation

## 5.1 Install Java

For Ubuntu/Debian:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

> Use the Java version required by your application.

---

## 5.2 Verify Java

Run:

```bash
java -version
```

Then:

```bash
javac -version
```

Example:

```text
openjdk version "17.x.x"
javac 17.x.x
```

If both commands show a version, Java is installed.

---

## 5.3 Install Maven

Run:

```bash
sudo apt update
sudo apt install maven -y
```

---

## 5.4 Verify Maven

Run:

```bash
mvn -version
```

Example:

```text
Apache Maven 3.x.x
Java version: 17.x.x
```

If Maven displays its version, the installation is successful.

---

## 5.5 Create Project Directory

Create a project:

```bash
mkdir my-maven-app
cd my-maven-app
```

Create source directories:

```bash
mkdir -p src/main/java
mkdir -p src/test/java
```

Create the POM:

```bash
touch pom.xml
```

Project structure:

```text
my-maven-app/
├── pom.xml
└── src/
    ├── main/java/
    └── test/java/
```

---

## 5.6 Create Basic pom.xml

Open the file:

```bash
vi pom.xml
```

Add:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0">

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

## 5.7 Understand Project Details

| Element      | Purpose                         |
| ------------ | ------------------------------- |
| `groupId`    | Organization/project identifier |
| `artifactId` | Application name                |
| `version`    | Application version             |

Example:

```xml
<groupId>com.example</groupId>
<artifactId>my-maven-app</artifactId>
<version>1.0.0</version>
```

---

## 5.8 Add Dependency

A dependency is an external library required by the application.

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

Maven downloads the dependency automatically during the build.

---

## 5.9 Validate pom.xml

Run:

```bash
mvn validate
```

This checks whether the Maven project configuration is valid.

---

## 5.10 Compile Project

Run:

```bash
mvn compile
```

Maven reads `pom.xml` and compiles the Java source code.

---

## 5.11 Run Tests

Run:

```bash
mvn test
```

This executes the project's tests.

Tests should pass before deployment.

---

## 5.12 Package Application

Run:

```bash
mvn package
```

Maven creates the application package.

The output is normally stored in:

```text
target/
```

For example:

```text
target/
└── my-maven-app-1.0.0.jar
```

---

## 5.13 Clean Build

To remove previous build files:

```bash
mvn clean
```

To clean and package:

```bash
mvn clean package
```

For a complete local build:

```bash
mvn clean install
```

---

# 6. Basic pom.xml Sections

| Section        | Purpose                 |
| -------------- | ----------------------- |
| `modelVersion` | Defines POM model       |
| `groupId`      | Project/organization ID |
| `artifactId`   | Application name        |
| `version`      | Project version         |
| `properties`   | Common configuration    |
| `dependencies` | Required libraries      |
| `build`        | Build configuration     |
| `plugins`      | Build tools/functions   |

A simple POM can contain:

```text
pom.xml
   |
   +-- Project Information
   |
   +-- Properties
   |
   +-- Dependencies
   |
   +-- Build / Plugins
```

---

# 7. Common Maven Commands

| Command               | Purpose                  |
| --------------------- | ------------------------ |
| `mvn -version`        | Check Maven version      |
| `mvn validate`        | Validate POM             |
| `mvn compile`         | Compile code             |
| `mvn test`            | Run tests                |
| `mvn package`         | Create JAR/WAR           |
| `mvn clean`           | Remove build files       |
| `mvn clean package`   | Clean and package        |
| `mvn clean install`   | Clean, build and install |
| `mvn dependency:tree` | Show dependencies        |

### Recommended Build Command

```bash
mvn clean package
```

---

# 8. Maven Workflow

The basic workflow is:

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
Compile
     ↓
Test
     ↓
Package
     ↓
Deploy
```

---

# 9. Maven in CI/CD

Maven is commonly used in CI/CD pipelines to build Java applications.

Typical workflow:

```text
Git Push
   ↓
Checkout Code
   ↓
Read pom.xml
   ↓
Download Dependencies
   ↓
Compile
   ↓
Test
   ↓
Package
   ↓
Deploy
```

A common CI/CD command is:

```bash
mvn clean package
```

The generated JAR/WAR can be used in the deployment stage.

---

# 10. Best Practices

| Practice                       | Reason                      |
| ------------------------------ | --------------------------- |
| Keep `pom.xml` in Git          | Required for builds         |
| Define dependency versions     | Makes builds predictable    |
| Avoid unnecessary dependencies | Reduces complexity          |
| Run tests before deployment    | Finds application issues    |
| Use clean builds in CI/CD      | Avoids old build files      |
| Review dependency changes      | Prevents unexpected changes |
| Use the required Java version  | Avoids compatibility issues |
| Keep POM readable              | Easier maintenance          |

---

# 11. Troubleshooting

## 11.1 Maven Not Found

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

## 11.2 Java Not Found

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

Check the POM:

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

* Network connectivity
* Dependency name
* Dependency version
* Maven repository configuration

You can inspect dependencies using:

```bash
mvn dependency:tree
```

---

## 11.5 Build Failure

Run:

```bash
mvn clean package
```

Then check:

```bash
java -version
mvn -version
```

Review the Maven error message and fix the reported issue.

---

# 12. Conclusion

Maven is used to build and manage Java applications.

`pom.xml` is the main configuration file used by Maven.

The basic process is:

```text
Java
 ↓
Maven
 ↓
pom.xml
 ↓
Dependencies
 ↓
Compile
 ↓
Test
 ↓
Package
 ↓
Deploy
```

Important commands:

```bash
java -version
mvn -version
mvn validate
mvn compile
mvn test
mvn package
mvn clean package
```

###  Explanation

> **“Java is installed first because Maven requires Java. Then Maven is installed and verified. We create the Maven project and configure `pom.xml`. The POM contains project details and dependencies. Maven reads the POM, downloads dependencies, compiles the code, runs tests, and creates the final application artifact.”**

---

# 13. FAQs

### Q1. What is Maven?

Maven is a build and dependency management tool for Java projects.

### Q2. What is pom.xml?

`pom.xml` is the main configuration file used by Maven.

### Q3. Do we install pom.xml?

No. We install Java and Maven. `pom.xml` is created/configured inside the Maven project.

### Q4. How do I check Maven?

```bash
mvn -version
```

### Q5. How do I build the application?

```bash
mvn clean package
```

### Q6. Where is the build output?

Normally:

```text
target/
```

---

# 14. References

| Reference                   | Purpose                             |
| --------------------------- | ----------------------------------- |
| Maven Documentation         | Maven configuration and usage       |
| Maven POM Reference         | POM configuration                   |
| Maven Lifecycle             | Build lifecycle                     |
| Maven Dependency Management | Dependency management               |
| Java Documentation          | Java installation and configuration |

---
