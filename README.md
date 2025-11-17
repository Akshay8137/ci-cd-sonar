# CI/CD Pipeline Overview

This project demonstrates a complete end-to-end **CI/CD pipeline** built using **GitHub Actions**, **Maven**, **SonarQube**, and **AWS SES**. The goal of this setup is to automate code analysis, building, testing, and sending email notifications based on the pipeline's outcome.

---

## 🚀 What This Project Does

This CI/CD pipeline performs the following tasks automatically whenever code is pushed to the **main** branch:

### **1. Code Checkout**

The workflow retrieves the latest version of your code from the GitHub repository.

### **2. AWS Authentication (OIDC)**

The workflow securely authenticates with AWS using GitHub’s **OpenID Connect (OIDC)** method — no long-term secrets or credentials are stored.

This allows the workflow to access AWS services (specifically SES) using a temporary session.

### **3. Java Build Environment Setup**

The workflow installs **JDK 17** using the `actions/setup-java` action to ensure a consistent environment for builds.

### **4. Caching for Performance**

The pipeline caches:

* **SonarQube dependencies**
* **Maven packages**

This speeds up subsequent runs significantly.

### **5. Maven Build & SonarQube Analysis**

The workflow runs:

* `mvn verify` → builds and tests the project
* `sonar-maven-plugin:sonar` → sends code quality and static analysis results to **SonarQube**

This ensures:

* Code meets quality standards
* Bugs and vulnerabilities are reported
* Reports are stored in your SonarQube dashboard

### **6. Email Notifications via AWS SES**

The pipeline sends automated emails depending on the workflow result:

#### ✅ **On Success**

An email is sent indicating that the build completed successfully.

#### ❌ **On Failure**

An alert email is sent notifying that the build failed.

Each email includes:

* Repository name
* Branch name
* Commit SHA
* Link to the workflow run logs

This allows quick debugging and monitoring of the project’s CI/CD health.

---

## 🔐 Required GitHub Secrets

To securely run this pipeline, the following secrets must be configured:

| Secret Name       | Purpose                               |
| ----------------- | ------------------------------------- |
| `SONAR_TOKEN`     | Authenticates SonarQube analysis      |
| `SONAR_HOST_URL`  | URL of the SonarQube server           |
| `GITHUB_USERNAME` | Username for GitHub package access    |
| `GITHUB_TOKEN`    | Token for Maven/GitHub authentication |

---

## 🔧 AWS Requirements

### **1. IAM Role for GitHub OIDC**

An IAM role (example from your setup):

```
arn:aws:iam::760715349705:role/ci-cd
```

This role must allow the GitHub workflow to assume it using OIDC and must include permissions to send emails using SES.

### **2. AWS SES Configuration**

You must have:

* A **verified sender email address**
* Recipient email addresses verified (if SES is in sandbox mode)

---

## 🎯 Summary

This repository demonstrates:

* A fully automated CI/CD pipeline for Java projects
* Secure AWS authentication with OIDC (no stored AWS credentials)
* Quality assurance using SonarQube
* Automated success/failure email notifications using AWS SES
* Efficient build execution with caching

If you want to enhance this further with deployment steps, Docker integration, architecture diagrams, or status badges, I can add those too!
