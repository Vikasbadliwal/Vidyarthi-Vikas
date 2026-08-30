# NPM (Node Package Manager) | DOCUMENTATION

## Document Information

| Author | Created On | Version |
| ------ | ---------- | ------- |
| Vikas  | 25-08-2026 | v1.0    |

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Prerequisites](#2-prerequisites)
3. [What is NPM](#3-what-is-npm)
4. [Important NPM Files](#4-important-npm-files)
5. [NPM Project Structure](#5-npm-project-structure)
6. [Common NPM Commands](#6-common-npm-commands)
7. [Dependencies](#7-dependencies)
8. [NPM Scripts](#8-npm-scripts)
9. [NPM Workflow](#9-npm-workflow)
10. [NPM in CI/CD](#10-npm-in-cicd)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)
13. [Conclusion](#13-conclusion)
14. [FAQs](#14-faqs)

---

# 1. Purpose

This document explains the basic usage of **NPM (Node Package Manager)** in Node.js projects.

NPM is mainly used to:

* Install packages.
* Manage dependencies.
* Run project commands.
* Manage package versions.
* Check dependency security.
* Support CI/CD builds.

---

# 2. Prerequisites

The main requirement is **Node.js**, which includes NPM.

Check Node.js:

```bash
node --version
```

Check NPM:

```bash
npm --version
```

Example:

```text
Node.js: v22.x.x
NPM:     10.x.x
```

---

# 3. What is NPM?

**NPM stands for Node Package Manager.**

It is used to install and manage packages required by a Node.js application.

For example, if an application needs Express:

```bash
npm install express
```

NPM downloads Express and adds it to the project dependencies.

### Simple Understanding

```text
Node.js Application
        |
        v
       NPM
        |
        v
   Install Packages
        |
        v
   Manage Dependencies
        |
        v
   Run Project Scripts
```

---

# 4. Important NPM Files

An NPM project mainly uses the following:

| File/Directory      | Purpose                                       |
| ------------------- | --------------------------------------------- |
| `package.json`      | Project information, dependencies and scripts |
| `package-lock.json` | Locks the exact dependency versions           |
| `node_modules/`     | Contains installed packages                   |
| `.gitignore`        | Specifies files not to commit to Git          |

### package.json

This is the main configuration file of an NPM project.

Example:

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "scripts": {
    "start": "node app.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

### package-lock.json

`package-lock.json` stores the exact dependency tree installed for the project.

It helps developers and CI/CD systems install consistent dependency versions.

### node_modules

`node_modules` contains the packages installed by NPM.

It should normally **not** be committed to Git.

Add:

```text
node_modules/
```

to `.gitignore`.

---

# 5. NPM Project Structure

A simple project can look like this:

```text
my-node-app/
│
├── src/
│   └── app.js
│
├── node_modules/
├── package.json
├── package-lock.json
├── .gitignore
└── README.md
```

---

# 6. Common NPM Commands

| Command                           | Purpose                             |
| --------------------------------- | ----------------------------------- |
| `npm --version`                   | Check NPM version                   |
| `npm init`                        | Create `package.json`               |
| `npm init -y`                     | Create `package.json` with defaults |
| `npm install`                     | Install project dependencies        |
| `npm install <package>`           | Install a package                   |
| `npm install <package>@<version>` | Install a specific version          |
| `npm uninstall <package>`         | Remove a package                    |
| `npm update`                      | Update packages                     |
| `npm outdated`                    | Check outdated packages             |
| `npm list`                        | Show installed packages             |
| `npm audit`                       | Check vulnerabilities               |
| `npm audit fix`                   | Fix applicable vulnerabilities      |
| `npm ci`                          | Clean installation using lock file  |
| `npm run <script>`                | Run a project script                |

---

## 6.1 Create a Project

```bash
mkdir my-node-app
cd my-node-app
npm init -y
```

This creates:

```text
package.json
```

---

## 6.2 Install a Package

```bash
npm install express
```

Short form:

```bash
npm i express
```

The package is added to `package.json` and installed in `node_modules`.

---

## 6.3 Install a Specific Version

```bash
npm install express@5.1.0
```

---

## 6.4 Install Development Dependency

Development dependencies are packages mainly required for development or testing.

Example:

```bash
npm install --save-dev jest
```

Short form:

```bash
npm i -D jest
```

They are stored under:

```json
"devDependencies": {
  "jest": "^30.0.0"
}
```

---

## 6.5 Install Existing Dependencies

If `package.json` already exists:

```bash
npm install
```

NPM reads `package.json` and installs the required dependencies.

---

## 6.6 Clean Installation

```bash
npm ci
```

`npm ci` is commonly used in **CI/CD pipelines**.

It uses the existing `package-lock.json` to install the dependency tree.

---

## 6.7 Remove a Package

```bash
npm uninstall express
```

This removes the package from the project.

---

## 6.8 Update Packages

```bash
npm update
```

---

## 6.9 Check Outdated Packages

```bash
npm outdated
```

Example:

```text
Package    Current    Wanted    Latest
express    5.0.0      5.0.1     5.1.0
```

---

## 6.10 List Packages

Show all packages:

```bash
npm list
```

Show top-level packages:

```bash
npm list --depth=0
```

---

## 6.11 Security Audit

Check dependencies:

```bash
npm audit
```

Try to fix applicable vulnerabilities:

```bash
npm audit fix
```

After changing dependencies, run the project tests:

```bash
npm test
```

---

# 7. Dependencies

Dependencies are packages required by the application.

For example:

```json
"dependencies": {
  "express": "^5.1.0"
}
```

Development dependencies are mainly used during development and testing:

```json
"devDependencies": {
  "jest": "^30.0.0"
}
```

### Difference

| Dependencies                | Dev Dependencies                        |
| --------------------------- | --------------------------------------- |
| Required by the application | Mainly required for development/testing |
| Stored in `dependencies`    | Stored in `devDependencies`             |
| Example: Express            | Example: Jest                           |

Install dependency:

```bash
npm install express
```

Install development dependency:

```bash
npm install --save-dev jest
```

---

# 8. NPM Scripts

NPM allows commands to be stored in `package.json`.

Example:

```json
{
  "scripts": {
    "start": "node app.js",
    "test": "jest",
    "build": "webpack"
  }
}
```

Run the scripts:

```bash
npm start
```

```bash
npm test
```

```bash
npm run build
```

For a custom script:

```bash
npm run <script-name>
```

### Why use scripts?

| Script         | Purpose                            |
| -------------- | ---------------------------------- |
| `start`        | Start the application              |
| `test`         | Run tests                          |
| `build`        | Create a production build          |
| Custom scripts | Automate project-specific commands |

---

# 9. NPM Workflow

A simple NPM workflow is:

```text
Create Project
      |
      v
npm init
      |
      v
package.json
      |
      v
Install Dependencies
      |
      v
Development
      |
      v
Run Tests
      |
      v
Build
      |
      v
Deployment
```

### Example

```bash
npm init -y
npm install express
npm install --save-dev jest
npm test
npm run build
```

---

# 10. NPM in CI/CD

NPM is commonly used in CI/CD pipelines to install dependencies, run tests, and build applications.

Example workflow:

```text
Developer
    |
    v
Git Push
    |
    v
CI/CD Pipeline
    |
    v
npm ci
    |
    v
npm test
    |
    v
npm run build
    |
    v
Deployment
```

### Typical CI/CD Commands

```bash
npm ci
npm test
npm run build
```

### Why `npm ci`?

In CI/CD, we generally want the same dependency versions every time.

`npm ci` uses the lock file for a clean installation.

---

# 11. Best Practices

| Practice                          | Reason                                  |
| --------------------------------- | --------------------------------------- |
| Commit `package.json`             | Required project configuration          |
| Commit `package-lock.json`        | Helps maintain consistent installations |
| Do not commit `node_modules`      | Large and generated directory           |
| Use `npm ci` in CI/CD             | Clean and lock-file-based installation  |
| Run tests after updates           | Detect dependency-related issues        |
| Run `npm audit`                   | Check known vulnerabilities             |
| Review `npm outdated`             | Keep dependencies maintained            |
| Avoid unnecessary global packages | Keep project dependencies local         |
| Review dependency updates         | Avoid unexpected breaking changes       |

---

# 12. Troubleshooting

## 12.1 NPM Command Not Found

Error:

```text
npm: command not found
```

Check:

```bash
node --version
npm --version
```

If NPM is unavailable, verify that Node.js is installed correctly.

---

## 12.2 package.json Not Found

Error:

```text
ENOENT: no such file or directory, open 'package.json'
```

Check the current directory:

```bash
pwd
ls -la
```

Then move to the project directory:

```bash
cd <project-directory>
```

Run:

```bash
npm install
```

---

## 12.3 Dependency Installation Failure

Try a clean installation.

If `package-lock.json` exists:

```bash
rm -rf node_modules
npm ci
```

If there is no lock file:

```bash
rm -rf node_modules
npm install
```

---

## 12.4 Permission Error

Error:

```text
EACCES: permission denied
```

Check the directory permissions:

```bash
ls -ld .
```

Check the NPM prefix:

```bash
npm config get prefix
```

Make sure the current user has the required permissions.

---

## 12.5 Dependency Conflict

Check installed packages:

```bash
npm list
```

Check outdated packages:

```bash
npm outdated
```

Review:

```text
package.json
package-lock.json
```

Update dependencies carefully and run tests.

---

## 12.6 Security Vulnerability

Run:

```bash
npm audit
```

If applicable:

```bash
npm audit fix
```

Then test:

```bash
npm test
```

---

## 12.7 Registry Issue

Check the configured registry:

```bash
npm config get registry
```

If required, configure the organization's approved registry:

```bash
npm config set registry <registry-url>
```

---

# 13. Conclusion

NPM is used to manage packages and dependencies in Node.js applications.

The most important concepts are:

| Concept             | Remember                 |
| ------------------- | ------------------------ |
| `package.json`      | Project configuration    |
| `package-lock.json` | Exact dependency tree    |
| `node_modules`      | Installed packages       |
| `npm install`       | Install dependencies     |
| `npm ci`            | Clean CI/CD installation |
| `npm test`          | Run tests                |
| `npm run build`     | Build application        |
| `npm audit`         | Check vulnerabilities    |

### Simple NPM Flow

```text
package.json
     |
     v
npm install / npm ci
     |
     v
node_modules
     |
     v
npm test
     |
     v
npm run build
     |
     v
Deployment
```

---

# 14. FAQs

### Q1. What is NPM?

NPM is the package manager used with Node.js to install and manage packages and dependencies.

### Q2. What is package.json?

It contains project information, dependencies, and NPM scripts.

### Q3. What is package-lock.json?

It records the resolved dependency versions used by the project.

### Q4. What is node_modules?

It contains the packages installed by NPM.

### Q5. What is the difference between `npm install` and `npm ci`?

| `npm install`                    | `npm ci`                      |
| -------------------------------- | ----------------------------- |
| Commonly used during development | Commonly used in CI/CD        |
| Can update the lock file         | Uses the existing lock file   |
| Installs dependencies            | Performs a clean installation |
