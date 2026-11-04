# 🚀 Automated Maven Build & AWS S3 Deployment Using GitHub Actions and OIDC

## 📌 Project Overview

This project demonstrates how to build a Java application using Maven and automate the build and artifact upload process using GitHub Actions and AWS.

The pipeline uses **OpenID Connect (OIDC)** to securely authenticate GitHub Actions with AWS IAM without storing long-term AWS access keys in GitHub Secrets.

The generated Maven JAR artifact is automatically uploaded to an Amazon S3 bucket.

## 🎯 Project Objectives

* Understand CI/CD pipeline automation.
* Build a Java application using Maven.
* Integrate GitHub Actions with AWS.
* Implement secure AWS authentication using OIDC.
* Configure AWS IAM roles and trust policies.
* Automatically upload build artifacts to Amazon S3.
* Troubleshoot real-world authentication issues.

## 🏗️ Architecture

```text
Developer
    |
    | Push code to GitHub
    v
GitHub Repository
    |
    v
GitHub Actions Workflow
    |
    v
Setup Java 17 & Maven
    |
    v
Maven Build
    |
    v
Generate JAR Artifact
    |
    v
GitHub OIDC Authentication
    |
    v
AWS IAM Role
    |
    v
Amazon S3 Bucket
    |
    v
JAR Artifact Uploaded Successfully
```

## 🛠️ Technologies Used

| Technology     | Purpose                        |
| -------------- | ------------------------------ |
| Git            | Version control                |
| GitHub         | Source code repository         |
| GitHub Actions | CI/CD automation               |
| Java 17        | Application development        |
| Apache Maven   | Build automation               |
| AWS IAM        | Identity and access management |
| AWS OIDC       | Secure authentication          |
| Amazon S3      | Artifact storage               |
| YAML           | Workflow configuration         |

## ⚙️ Project Implementation

### 1. Maven Project Setup

* Created a Java application using Maven.
* Configured project dependencies in `pom.xml`.
* Used Maven to compile, test and package the application.

### 2. GitHub Actions Configuration

Created a GitHub Actions workflow:

`.github/workflows/maven-build.yml`

The workflow performs the following operations:

* Checks out the source code.
* Sets up Java 17.
* Configures Maven caching.
* Executes the Maven build.
* Verifies the generated JAR file.

Build command:

```bash
mvn -B -f myapp/pom.xml clean package
```

Expected artifact:

```text
myapp/target/myapp-1.0-SNAPSHOT.jar
```

### 3. AWS OIDC Integration

Configured an OpenID Connect identity provider in AWS IAM.

OIDC allows GitHub Actions to obtain temporary AWS credentials by assuming an IAM role.

**OIDC Provider:**

```text
https://token.actions.githubusercontent.com
```

**Audience:**

```text
sts.amazonaws.com
```

### 4. IAM Role Configuration

Created an IAM role:

```text
github-actions-maven-role
```

Configured its trust policy to allow the authorised GitHub repository and branch to assume the role.

The role uses the GitHub OIDC token's claims to verify the identity of the workflow.

### 5. S3 Permissions

Configured IAM permissions to allow the role to upload objects into the designated S3 bucket.

**S3 Bucket:**

```text
maven-devops-artifacts-718harini
```

### 6. Upload JAR to S3

After successful authentication, GitHub Actions uploads the generated JAR file.

Versioned artifact:

```text
myapp-<commit-sha>.jar
```

Latest artifact:

```text
myapp-latest.jar
```

## 🔍 Troubleshooting Experience

During implementation, I encountered the following AWS OIDC authentication error:

```text
Could not assume role with OIDC:
Not authorized to perform sts:AssumeRoleWithWebIdentity
```

### Root Cause

The GitHub OIDC token's `sub` (subject) claim did not match the subject configured in the AWS IAM role's trust policy.

### Solution

1. Inspected the OIDC token claims.
2. Verified the issuer (`iss`) and audience (`aud`).
3. Identified the mismatch in the `sub` claim.
4. Updated the IAM trust policy to match the actual GitHub token subject.
5. Re-ran the GitHub Actions workflow.

This helped me understand how AWS validates external identities before allowing IAM role assumption.

## 🔐 Security Concepts Learned

* OIDC-based authentication.
* Temporary AWS credentials.
* IAM role assumption.
* IAM trust policies.
* IAM permission policies.
* Least-privilege access.
* Secure GitHub Actions integration with AWS.

**Important:** No long-term AWS access keys are required in this OIDC-based workflow.

## ✅ Final Outcome

Successfully implemented an automated pipeline that:

* Builds the Maven application.
* Generates a JAR artifact.
* Authenticates with AWS using GitHub OIDC.
* Assumes an AWS IAM role.
* Uploads the generated JAR to Amazon S3.

## 📚 Key Learnings

This project provided hands-on experience with:

* CI/CD pipeline implementation.
* Maven build automation.
* GitHub Actions workflows.
* AWS IAM and OIDC integration.
* S3 artifact management.
* Troubleshooting authentication failures.

## 👩‍💻 Author

**Harini**

GitHub: [718harini](https://github.com/718harini)

Project Repository: [maven-devops-project](https://github.com/718harini/maven-devops-project)
