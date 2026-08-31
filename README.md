# NPM Installation - SOP

<p align="center">
  <img src="https://nodejs.org/static/logos/nodejsLight.svg" alt="Node.js Logo" width="300"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-linux-blue" />
  <img src="https://img.shields.io/badge/npm-supported-orange" />
  <img src="https://img.shields.io/badge/license-open--source-lightgrey" />
</p>

---

| Author | Created On | Version | Last Updated By | Last Edited On | L0 Reviewer     | L1 Reviewer    | L2 Reviewer        |
| ------ | ---------- | ------- | --------------- | -------------- | --------------- | -------------- | ------------------ |
| Vikas  | 01/09/2026 | 1.0     |                 |                | Deepak Kushwaha | Faisal/Mohit K | Mahesh Kumar/Varun |

---

## Table of Contents

* [Introduction](#introduction)
* [NPM Installation Methods Overview](#npm-installation-methods-overview)
* [Why NPM Installation Methods Matter](#why-npm-installation-methods-matter)
* [Installation via Package Manager](#installation-via-package-manager)
* [Installation via Node.js](#installation-via-nodejs)
* [Installation via Bash Automation](#installation-via-bash-automation)
* [Verification](#verification)
* [NPM Configuration](#npm-configuration)
* [Best Practices](#best-practices)
* [Troubleshooting](#troubleshooting)
* [FAQs](#faqs)
* [References](#references)
* [Contact Information](#contact-information)

---

## Introduction

NPM (**Node Package Manager**) is the default package manager for Node.js.

It is used to:

* Install JavaScript packages.
* Manage project dependencies.
* Run project scripts.
* Manage package versions.
* Support application build and deployment.

NPM is normally installed together with Node.js.

---

## NPM Installation Methods Overview

| Method             | Description                                            | Best Use Case              | Complexity |
| ------------------ | ------------------------------------------------------ | -------------------------- | ---------- |
| Package Manager    | Installs Node.js and NPM from OS repositories          | Quick server setup         | Low        |
| Node.js Repository | Installs a specific supported Node.js version with NPM | Application environments   | Medium     |
| Bash Automation    | Installs NPM automatically using a script              | CI/CD and multiple servers | Medium     |

---

## Why NPM Installation Methods Matter

Different environments may require different Node.js and NPM versions.

The installation method should provide:

* **Consistency** – Same version across environments.
* **Version Control** – Ability to use the required Node.js version.
* **Automation** – Easy installation on multiple servers.
* **Maintainability** – Easy upgrades and troubleshooting.

For standard Linux environments, the package manager is the simplest option.

---

## Installation via Package Manager

This is the simplest method for installing Node.js and NPM on Ubuntu/Debian systems.

### Step 1: Update Package Repository

```bash
sudo apt update
```

### Step 2: Install Node.js and NPM

```bash
sudo apt install nodejs npm -y
```

### Step 3: Verify Installation

```bash
node --version
npm --version
```

Example:

```text
v22.x.x
10.x.x
```

### Characteristics

| Aspect          | Details                |
| --------------- | ---------------------- |
| Speed           | Fast                   |
| Installation    | Simple                 |
| Maintenance     | Managed by OS packages |
| Version Control | Limited                |
| Best For        | Basic server setup     |

---

## Installation via Node.js

For environments that require a specific Node.js version, an approved Node.js repository or version-management method can be used.

### Step 1: Check Available Node.js

```bash
apt-cache policy nodejs
```

### Step 2: Install Node.js

```bash
sudo apt install nodejs -y
```

NPM is normally installed along with Node.js, depending on the package source.

### Step 3: Verify

```bash
node --version
npm --version
```

> Use the Node.js version approved by your organization or application.

---

## Installation via Bash Automation

Bash automation can be used when NPM needs to be installed on multiple servers.

### Example Script

```bash
#!/bin/bash

set -e

sudo apt update
sudo apt install -y nodejs npm

echo "Node.js version:"
node --version

echo "NPM version:"
npm --version
```

Save the script:

```bash
nano install-npm.sh
```

Give execute permission:

```bash
chmod +x install-npm.sh
```

Run:

```bash
./install-npm.sh
```

### Characteristics

| Aspect      | Details                       |
| ----------- | ----------------------------- |
| Automation  | High                          |
| Reusability | High                          |
| Scalability | Suitable for multiple servers |
| Consistency | Same installation steps       |
| Best For    | Automation and CI/CD          |

---

## Verification

After installation, verify both Node.js and NPM.

### Check Node.js

```bash
node --version
```

### Check NPM

```bash
npm --version
```

### Check NPM Installation Path

```bash
which npm
```

Example:

```text
/usr/bin/npm
```

### Check NPM Configuration

```bash
npm config list
```

### Verification Table

| Check           | Command           | Expected Result             |
| --------------- | ----------------- | --------------------------- |
| Node.js version | `node --version`  | Version is displayed        |
| NPM version     | `npm --version`   | Version is displayed        |
| NPM path        | `which npm`       | Valid executable path       |
| Configuration   | `npm config list` | NPM configuration displayed |

---

## NPM Configuration

NPM can be configured according to the project or organization's requirements.

### Check Registry

```bash
npm config get registry
```

The default registry is:

```text
https://registry.npmjs.org/
```

### Set Registry

```bash
npm config set registry <registry-url>
```

For enterprise environments, use the organization's approved NPM registry.

### Check Configuration

```bash
npm config list
```

---

## Basic NPM Project Setup

After installing NPM, a project can be initialized.

### Step 1: Create Project Directory

```bash
mkdir my-node-app
cd my-node-app
```

### Step 2: Initialize NPM

```bash
npm init -y
```

This creates:

```text
package.json
```

### Step 3: Install a Package

Example:

```bash
npm install express
```

NPM creates:

```text
node_modules/
package-lock.json
```

### Project Structure

```text
my-node-app/
├── node_modules/
├── package.json
└── package-lock.json
```

---

## Best Practices

* Install the Node.js/NPM version required by the application.
* Verify the installation after setup.
* Keep `package.json` under source control.
* Commit `package-lock.json` for application projects.
* Do not commit `node_modules/`.
* Add `node_modules/` to `.gitignore`.
* Use `npm ci` in CI/CD when a lock file is available.
* Avoid installing application dependencies globally.
* Use approved package registries in enterprise environments.

Example `.gitignore`:

```text
node_modules/
```

---

## Troubleshooting

| Issue                        | Possible Cause                   | Resolution                            |
| ---------------------------- | -------------------------------- | ------------------------------------- |
| `npm: command not found`     | NPM not installed or PATH issue  | Install NPM and check PATH            |
| `node: command not found`    | Node.js not installed            | Install Node.js                       |
| Permission denied            | Directory permission issue       | Check user and directory permissions  |
| Package installation failure | Network/registry issue           | Check registry and connectivity       |
| Wrong NPM version            | Multiple Node.js installations   | Check `which npm` and `npm --version` |

### Check PATH

```bash
echo $PATH
```

### Check NPM Location

```bash
which npm
```

### Check Registry

```bash
npm config get registry
```

### Test Package Installation

```bash
npm install express
```

If the installation fails, check the error message and verify network access and registry configuration.

---

## FAQs

### 1. What is NPM?

NPM is the package manager used with Node.js to install and manage JavaScript packages.

### 2. Is NPM installed separately?

NPM is normally distributed with Node.js.

### 3. How do I check the NPM version?

```bash
npm --version
```

### 4. How do I install a package?

```bash
npm install <package-name>
```

### 5. What is `package.json`?

`package.json` contains project information, dependencies, and NPM scripts.


When a valid `package-lock.json` is available, `npm ci` provides a clean dependency installation.

---

## References

| Resource                 | Link                            |
| ------------------------ | ------------------------------- |
| NPM Documentation        | https://docs.npmjs.com/         |
| Node.js Documentation    | https://nodejs.org/docs/latest/ |
| NPM CLI Documentation    | https://docs.npmjs.com/cli/     |

---

## Contact Information

| Name           | Email                                                                                   |
| -------------- | --------------------------------------------------------------------------------------- |
| Vikas Badliwal | [vikash.badliwal.snaatak@mygurukulam.co](mailto:vikash.badliwal.snaatak@mygurukulam.co) |
| DevOps Team    | `<email>`                                                                               |

---
