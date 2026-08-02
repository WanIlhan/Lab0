# Lab 0: Environment Setup Report

**Course:** IKB42603 Cloud Computing Security Essentials  
**Lab:** Lab 0 - Environment Setup  
**Guide used:** `IKB42603_Lab0_Environment_Setup_Cheatsheet.pdf`  
**Name:** `WAN ILHAN HUZAIRY BIN WAN HASHIM`

## 1. Objective

The objective of this lab was to install, configure, and verify the local tools required before starting Lab 1. The setup follows the Lab 0 Environment Setup Cheatsheet and prepares a local cloud security lab environment using Docker, AWS CLI, LocalStack, kind, kubectl, OpenSSL, and oathtool.

The lab environment is designed to run locally without using a real AWS account. AWS CLI commands are pointed to LocalStack using the endpoint `http://localhost:4566`.

## 2. Tools Required by the Guide

| Tool | Purpose | Used In |
| --- | --- | --- |
| Docker | Runs containers and the LocalStack cloud simulator | All labs |
| AWS CLI v2 | Sends AWS commands to LocalStack | Labs 1, 3, 5 |
| kind | Runs a local Kubernetes cluster inside Docker | Labs 1, 2, 4 |
| kubectl | Controls the Kubernetes cluster | Labs 1, 2, 4 |
| OpenSSL | Supports encryption, keys, and certificates | Lab 3 |
| oathtool | Generates MFA/TOTP codes | Lab 4 |
| Trivy | Scans container images for vulnerabilities through Docker | Lab 4 |

## 3. Docker Installation and Verification

Docker serves as the foundational container engine required to host both the LocalStack environment and the Kubernetes node instances provisioned by kind.

### Steps

1. Configure Docker and permissions.
2. Verify Docker functionality:

```bash
docker --version
