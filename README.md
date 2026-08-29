# NPM (Node Package Manager)

<img width="100" height="100" alt="image" src="https://github.com/user-attachments/assets/be1eb7ff-0db3-420d-b1a3-c6525ac74eb8" />


## Document Information

| Author | Created On | Version | Last Updated By | Last Updated On | L0 Reviewer | L1 Reviewer | L2 Reviewer |
|---|---|---|---|---|---|---|---|
| Vikas  | 25-08-2026 | v1.0    | `             ` |                 |             |             |             |

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Prerequisites](#2-prerequisites)
3. [What is NPM](#3-what-is-npm)
4. [Why NPM](#4-why-npm)
5. [Features of NPM](#5-features-of-npm)
6. [NPM Project Structure](#6-npm-project-structure)
7. [Basic NPM Commands](#7-basic-npm-commands)
8. [Advantages and Disadvantages](#8-advantages-and-disadvantages)
9. [Best Practices](#9-best-practices)
10. [Troubleshooting](#10-troubleshooting)
11. [Conclusion](#12-conclusion)
12. [FAQs](#13-faqs)
13. [References](#14-references)

---

# 1. Purpose

NPM (Node Package Manager) is a package manager used for managing packages and dependencies in Node.js and JavaScript applications.

NPM helps developers to:

- Install required packages.
- Manage application dependencies.
- Manage package versions.
- Remove unwanted packages.
- Update existing packages.
- Run project scripts.
- Perform dependency security audits.
- Support application build and deployment processes.

This documentation explains what NPM is, why it is used, its major features, basic commands, advantages, disadvantages, best practices, and troubleshooting procedures.

---

# 2. Prerequisites

Before using NPM, ensure that the following prerequisites are available.

## 2.1 Node.js

NPM is distributed with Node.js.

Verify whether Node.js is installed:

```bash
node --version
```

Example:

```text
v22.x.x
```

---

## 2.2 NPM

Verify whether NPM is installed:

```bash
npm --version
```

Example:

```text
10.x.x
```

> The exact version depends on the installed Node.js/NPM release.

---

## 2.3 Project Directory

Create or navigate to the Node.js project directory.

Example:

```bash
mkdir my-node-app
cd my-node-app
```

---

## 2.4 Required Access

The user should have:

- Access to the project directory.
- Permission to create and modify project files.
- Permission to install project dependencies.
- Access to the NPM registry or configured package registry.
- Required network access if packages need to be downloaded.

---

# 3. What is NPM

NPM stands for **Node Package Manager**.

NPM is used to install, manage, update, and remove packages used by Node.js and JavaScript applications.

A package is a reusable piece of software that provides functionality that can be used by an application.

For example:

```bash
npm install express
```

The above command installs the `express` package into the current project.

NPM also manages the dependencies required by installed packages.

---

## 3.1 NPM Components

NPM mainly consists of the following components:

| Component | Purpose |
|---|---|
| **NPM CLI** | Command-line interface used to execute NPM commands. |
| **NPM Registry** | Repository from which packages can be downloaded and where packages can be published. |
| **package.json** | Stores project metadata, dependencies, scripts, and configuration. |
| **package-lock.json** | Records the resolved dependency tree and package versions. |
| **node_modules** | Directory containing installed project packages. |

---

## 3.2 NPM Overview

```text
                    Node.js Application
                            |
                            v
                           NPM
                            |
              +-------------+-------------+
              |             |             |
              v             v             v
          NPM CLI      package.json    NPM Registry
                            |
                            v
                       Dependencies
                            |
                            v
                       node_modules/
```

---

# 4. Why NPM

NPM is used to simplify package and dependency management for Node.js applications.

## 4.1 Dependency Management

NPM allows developers to install and manage packages required by an application.

Example:

```bash
npm install express
```

NPM automatically downloads the package and its required dependencies.

---

## 4.2 Package Reusability

NPM provides access to a large ecosystem of reusable packages.

Instead of implementing common functionality from scratch, developers can install an existing package.

Example:

```bash
npm install axios
```

---

## 4.3 Version Management

NPM allows developers to install a specific version of a package.

Example:

```bash
npm install express@5.1.0
```

This installs the specified version.

---

## 4.4 Project Management

NPM uses `package.json` to store project information and dependency configuration.

Example:

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "Sample Node.js application",
  "main": "app.js"
}
```

---

## 4.5 Automation

NPM provides scripts that can be used to automate common development tasks.

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

These scripts can be executed using:

```bash
npm start
npm test
npm run build
```


---

# 5. Features of NPM

| Feature | Description |
|---|---|
| **Package Installation** | Installs packages required by the application. |
| **Dependency Management** | Manages project dependencies and their versions. |
| **Package Registry** | Provides access to a large collection of packages. |
| **Version Management** | Allows specific package versions to be installed. |
| **package.json** | Stores project metadata, dependencies, and scripts. |
| **package-lock.json** | Records the resolved dependency tree. |
| **NPM Scripts** | Allows automation of application commands. |
| **Package Update** | Provides commands to update packages. |
| **Package Removal** | Allows packages to be removed from a project. |
| **Security Audit** | Checks dependencies for known security vulnerabilities. |
| **Package Publishing** | Allows packages to be published to the NPM registry. |

---

# 6. NPM Project Structure

A typical NPM project can have the following structure:

```text
my-node-app/
│
├── node_modules/
│
├── src/
│   └── app.js
│
├── package.json
├── package-lock.json
├── .gitignore
└── README.md
```

## 6.1 package.json

`package.json` is the main configuration file for an NPM project.

It may contain:

- Project name.
- Project version.
- Description.
- Application entry point.
- Scripts.
- Dependencies.
- Development dependencies.

Example:

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "Sample Node.js application",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^5.1.0"
  },
  "devDependencies": {
    "jest": "^30.0.0"
  }
}
```

---

## 6.2 package-lock.json

`package-lock.json` records the dependency tree resolved by NPM.

It helps maintain consistent dependency versions across:

- Developer environments.
- CI/CD environments.
- Build systems.
- Deployment environments.

For application repositories, `package-lock.json` should normally be committed to source control.

---

## 6.3 node_modules

The `node_modules` directory contains installed packages.

Example:

```text
node_modules/
├── express/
├── axios/
├── jest/
└── ...
```

The `node_modules` directory should normally not be committed to Git.

Add the following to `.gitignore`:

```text
node_modules/
```

---

# 7. Basic NPM Commands

## 7.1 Check NPM Version

```bash
npm --version
```

This displays the installed NPM version.

---

## 7.2 Initialize an NPM Project

```bash
npm init
```

This creates a `package.json` file interactively.

For default configuration:

```bash
npm init -y
```

---

## 7.3 Install a Package

```bash
npm install express
```

Short form:

```bash
npm i express
```

This installs the package as a project dependency.

---

## 7.4 Install a Specific Version

```bash
npm install express@5.1.0
```

This installs the specified package version.

---

## 7.5 Install a Development Dependency

```bash
npm install --save-dev jest
```

Short form:

```bash
npm i -D jest
```

This installs the package as a development dependency.

---

## 7.6 Install Project Dependencies

```bash
npm install
```

This installs dependencies defined in `package.json`.

---

## 7.7 Clean Dependency Installation

```bash
npm ci
```

`npm ci` performs a clean installation using the project's lock file.

It is commonly used in CI/CD environments.

---

## 7.8 Remove a Package

```bash
npm uninstall express
```

This removes the package from the project.

---

## 7.9 Update Packages

```bash
npm update
```

This updates installed packages according to the dependency specifications.

---

## 7.10 Check Outdated Packages

```bash
npm outdated
```

This displays packages for which newer versions are available.

Example:

```text
Package    Current    Wanted    Latest
express    5.0.0      5.0.1     5.1.0
```

---

## 7.11 List Installed Packages

```bash
npm list
```

To display only top-level packages:

```bash
npm list --depth=0
```

---

## 7.12 Run NPM Scripts

If the `package.json` contains:

```json
{
  "scripts": {
    "start": "node app.js",
    "test": "jest",
    "build": "webpack"
  }
}
```

Run:

```bash
npm start
```

```bash
npm test
```

```bash
npm run build
```

For custom scripts:

```bash
npm run <script-name>
```

---

## 7.13 Check Security Vulnerabilities

```bash
npm audit
```

This checks project dependencies for known security vulnerabilities.

Where appropriate, a fix can be attempted using:

```bash
npm audit fix
```

After applying changes, test the application:

```bash
npm test
```

---

# 8. Advantages and Disadvantages

## 8.1 Advantages

| Advantage | Description |
|---|---|
| **Easy Package Management** | Packages can be installed and managed using simple commands. |
| **Large Ecosystem** | Provides access to a large number of reusable packages. |
| **Dependency Management** | Automatically manages project dependencies. |
| **Version Management** | Supports package version specifications. |
| **Automation** | NPM scripts can automate common development tasks. | 
| **Package Reusability** | Developers can reuse existing packages. |
| **Lock File Support** | `package-lock.json` helps provide consistent installations. |
| **Security Auditing** | `npm audit` helps identify known vulnerabilities. |
| **Package Publishing** | Developers can publish reusable packages. |

---

## 8.2 Disadvantages

| Disadvantage | Description |
|---|---|
| **Large node_modules** | Dependency directories can consume significant disk space. |
| **Complex Dependency Trees** | Applications can have many transitive dependencies. |
| **Security Risks** | Third-party packages may contain vulnerabilities. |
| **Dependency Conflicts** | Different packages may require incompatible versions. |
| **Maintenance** | Dependencies require regular review and updates. |
| **Installation Time** | Large dependency trees can increase installation time. |
| **Supply-Chain Risk** | Applications depend on third-party packages and maintainers. |
| **Breaking Changes** | Major package updates can introduce compatibility issues. |

---

# 9. Best Practices

## 9.1 Commit package.json

Always keep `package.json` under source control.

## 9.2 Commit package-lock.json

For application repositories, commit `package-lock.json` to help maintain reproducible installations.

## 9.3 Do Not Commit node_modules

Add the following entry to `.gitignore`:

```text
node_modules/
```

## 9.4 Prefer Local Dependencies

For application dependencies, use:

```bash
npm install express
```

instead of:

```bash
npm install -g express
```

Global packages should generally be reserved for CLI tools intended for system/user-wide use.

## 9.5 Check Outdated Dependencies

Regularly check:

```bash
npm outdated
```

## 9.7 Perform Security Audits

Regularly check dependencies:

```bash
npm audit
```

## 9.8 Test After Dependency Updates

After updating dependencies, run:

```bash
npm test
```

and, where applicable:

```bash
npm run build
```

---

# 10. Troubleshooting

## 10.1 NPM Command Not Found

### Error

```text
npm: command not found
```

### Check Node.js

```bash
node --version
```

### Check NPM

```bash
npm --version
```

If Node.js/NPM is not available, install Node.js using the organization's approved installation method.

---

## 10.2 package.json Not Found

### Error

```text
ENOENT: no such file or directory, open 'package.json'
```

### Check Current Directory

Linux/macOS:

```bash
pwd
ls -la
```

Windows:

```cmd
cd
dir
```

Navigate to the project directory:

```bash
cd <project-directory>
```

Then run:

```bash
npm install
```

---

## 10.3 Permission Error

### Error

```text
EACCES: permission denied
```

Check the project directory permissions.

Linux/macOS:

```bash
ls -ld .
```

Check the NPM prefix:

```bash
npm config get prefix
```

Ensure that the current user has appropriate permissions to modify the project directory.

> Avoid using `sudo npm install` as a default solution.

---

## 10.4 Dependency Installation Failure

If the dependency installation is inconsistent, check:

```text
package.json
package-lock.json
```

For a clean installation where a valid lock file exists:

```bash
rm -rf node_modules
npm ci
```

If no lock file exists:

```bash
rm -rf node_modules
npm install
```

> On Windows, use the appropriate command for removing the `node_modules` directory.

---

## 10.5 Dependency Version Conflict

Check installed dependencies:

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

Update dependencies in a controlled manner and run the application's tests afterward.

---

## 10.6 Security Vulnerability

Run:

```bash
npm audit
```

Review the reported vulnerabilities.

Where appropriate:

```bash
npm audit fix
```

After applying changes:

```bash
npm test
```

Review the resulting dependency changes before committing them.

---

## 10.7 Package Installation Is Very Slow

Check whether the issue is related to:

- Network connectivity.
- Registry configuration.
- Proxy configuration.
- Large dependency trees.
- Package registry availability.

Check the configured registry:

```bash
npm config get registry
```

---

## 10.8 NPM Registry Configuration

Check the current registry:

```bash
npm config get registry
```

The registry can be configured using:

```bash
npm config set registry <registry-url>
```

For enterprise environments, use the registry configured by the organization's development/DevOps team.

---

# 11. Conclusion

NPM provides a standard way to manage packages and dependencies in Node.js and JavaScript applications.

It simplifies:

- Package installation.
- Dependency management.
- Version management.
- Project configuration.
- Script automation.
- Security auditing.
- CI/CD dependency installation.

A typical NPM workflow is:

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
npm install <package>
      |
      v
node_modules/
      |
      v
Development
      |
      v
Testing
      |
      v
npm run build
      |
      v
CI/CD
      |
      v
npm ci
      |
      v
Deployment
```

For production projects, dependency versions should be managed carefully, `package-lock.json` should normally be maintained in source control, and `node_modules/` should not be committed to the repository.

---

# 13. FAQs

## Q1. What is NPM?

NPM stands for Node Package Manager. It is used to manage packages and dependencies in Node.js and JavaScript projects.

## Q2. Why is NPM used?

NPM is used to install, manage, update, and remove project dependencies and to automate common development tasks.

## Q3. What is a package?

A package is a reusable piece of software that can be installed and used by an application.

Example:

```bash
npm install express
```

## Q4. What is package.json?

`package.json` is the main configuration file for an NPM project. It contains project metadata, dependencies, development dependencies, and scripts.

## Q5. What is package-lock.json?

`package-lock.json` records the resolved dependency tree and package versions used by the project.



# 14. References

| Reference | Purpose |
|---|---|
| [NPM Official Documentation](https://docs.npmjs.com/) | Official NPM documentation |
| [NPM CLI Documentation](https://docs.npmjs.com/cli/v11/commands/npm) | NPM CLI command reference |
| [package.json Documentation](https://docs.npmjs.com/cli/v11/configuring-npm/package-json) | `package.json` configuration |
| [package-lock.json Documentation](https://docs.npmjs.com/cli/v11/configuring-npm/package-lock-json) | `package-lock.json` reference |
| [npm install Documentation](https://docs.npmjs.com/cli/v11/commands/npm-install) | Package installation |
| [npm audit Documentation](https://docs.npmjs.com/cli/v11/commands/npm-audit) | Security auditing |
| [npm update Documentation](https://docs.npmjs.com/cli/v11/commands/npm-update) | Package updates |

---
