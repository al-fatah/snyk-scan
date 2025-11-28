# 📦 snyk-scan  
### Terraform + AWS Lambda + Snyk Security Scans + GitHub Actions CI Pipeline

This repository implements:

- A simple **AWS Lambda** function (“Hello, World!”) deployed using **Terraform**  
- A **GitHub Actions CI workflow** performing:  
  ✔ Terraform validation checks  
  ✔ Snyk Infrastructure-as-Code (IaC) scans  
  ✔ Snyk Code scans  
- Protected **main branch** via GitHub *Rulesets*  
- Secure automation with **GitHub Actions Secrets**  
- A clean repo structure following best practices

## 📁 Repository Structure

```
snyk-scan/
├── .github/
│   └── workflows/
│       └── ci.yml
├── lambda/
│   ├── index.js
│   └── package.json   (optional)
├── main.tf
├── variables.tf
├── .gitignore
└── README.md
```

## 🛠️ Infrastructure Overview

The Terraform configuration provisions:

### **AWS Lambda Function**
- Runtime: **Node.js 18**
- Handler: `index.handler`
- Behavior: Returns a JSON `{ "message": "Hello, World!" }`
- Packaged using Terraform’s `archive_file`

### **IAM Role**
- Lambda execution role  
- AWS-managed `AWSLambdaBasicExecutionRole` policy

## 🚀 GitHub Actions CI Pipeline

Workflow file: `.github/workflows/ci.yml`

The pipeline automatically runs on:

- Pushes to `main`
- Pull requests targeting `main`

### ✔ Job 1: Terraform Checks
- `terraform init`
- `terraform fmt -check`
- `terraform validate`

### ✔ Job 2: Snyk Checks
Uses the Snyk CLI to perform:

#### 🔐 IaC Security Scan
```
snyk iac test .
```

#### 🔐 Code Security Scan
```
snyk code test lambda/
```

## 🔑 Required GitHub Secrets

Add under **Settings → Secrets and variables → Actions**

| Secret Name | Description |
|------------|-------------|
| `AWS_ACCESS_KEY_ID` | IAM user or role for Terraform |
| `AWS_SECRET_ACCESS_KEY` | IAM secret key |
| `SNYK_TOKEN` | Snyk API token |

## 🔒 Branch Protection with Rulesets

Rules applied to `main`:

- Require pull requests  
- Require passing checks:  
  - `terraform-checks`  
  - `snyk-checks`  
- Prevent force pushes  
- Prevent deletions  

## ▶ Lambda Function (NodeJS)

```
exports.handler = async () => {
  console.log("Hello, World!");
  return {
    statusCode: 200,
    body: JSON.stringify({ message: "Hello, World!" })
  };
};
```

## 📦 Terraform Commands (Local Use)

```
terraform init
terraform validate
terraform plan
terraform apply
```

## 🔍 Snyk Scans (Local Use)

```
npm install -g snyk
export SNYK_TOKEN="YOUR_TOKEN"
snyk iac test .
snyk code test lambda/
```

## 📝 Notes

- Snyk CLI in CI uses non-interactive auth via the environment variable `SNYK_TOKEN`.
- Lambda packaging is handled via the Terraform `archive_file` data source.
- Compatible with GitHub’s new Rulesets system.
