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
Result
The system returned the installed Docker engine details, confirming the service is active and accessible:
Docker version 28.5.2+dfsg4, build 9cc6dea35e9a963f281434761c656fba4ac43aed
4. AWS CLI v2 Installation and Verification
The AWS Command Line Interface (AWS CLI v2) allows execution of AWS-formatted commands directly against the local LocalStack instance without interacting with live AWS cloud infrastructure.
Steps
1. Download and install AWS CLI v2 binary.
2. Verify the installation:
aws --version
Result
AWS CLI v2 was successfully verified. The command returned:
aws-cli/2.34.56 Python/3.13.12 Linux/6.19.14+kali-amd64 source/x86_64.kali.2026
5. kind and kubectl Installation and Verification
The guide requires kind for creating a Kubernetes cluster inside Docker and kubectl for controlling that cluster.
Steps
1. Download and install kind executable binary to /usr/local/bin/kind.
2. Install kubectl via snap package manager.
3. Verify both binaries:
kind --version
kubectl version --client
Result
kind was successfully installed and verified:
kind version 0.31.0
kubectl was verified successfully:
Client Version: v1.33.4
Kustomize Version: v5.5.0
6. Helper Tools Verification
OpenSSL and oathtool are used as supporting utility tools for encryption, certificate management, and TOTP MFA code generation in upcoming labs.
Steps
1. Install helper packages using APT:
sudo apt update && sudo apt install -y openssl oathtool
1. Verify versions:
openssl version
oathtool --version
Result
OpenSSL and oathtool installed and verified successfully.
7. LocalStack Startup and Verification
LocalStack provides the local AWS-compatible emulator environment.
Steps
1. Launch LocalStack container:
docker run -d --name localstack -p 4566:4566 localstack/localstack:3.4.0
1. Confirm health status:
curl http://localhost:4566/_localstack/health
Result
LocalStack started cleanly on port 4566 and responded to health queries without errors.
8. Kubernetes Cluster Creation and Verification
The guide requires creating a local Kubernetes cluster with kind and verifying node connectivity with kubectl.
Steps
1. Provision cluster named ccse:
kind create cluster --name ccse
1. Check node status:
kubectl get nodes
Result
The ccse control-plane node reached Ready status:
NAME                 STATUS   ROLES           AGE     VERSION
ccse-control-plane   Ready    control-plane   3m38s   v1.35.0
9. One-Time AWS CLI Configuration for LocalStack
Dummy credentials were set so the AWS CLI bypasses authentication prompts when executing commands against LocalStack.
Steps
aws configure set aws_access_key_id test
aws configure set aws_secret_access_key test
aws configure set region us-east-1
EP='--endpoint-url=http://localhost:4566'
aws $EP sts get-caller-identity
Result
The call returned a valid local identity string from LocalStack:
{
    "UserId": "AKIAIOSFODNN7EXAMPLE",
    "Account": "000000000000",
    "Arn": "arn:aws:iam::000000000000:root"
}
10. Pre-Lab Verification Checklist
Check	Status
docker --version prints a version	Complete
aws --version prints AWS CLI v2	Complete
kind --version works	Complete
kubectl version --client works	Complete
OpenSSL works	Complete
oathtool works	Complete
LocalStack container starts	Complete
LocalStack is healthy	Complete
Kubernetes cluster is created	Complete
kubectl get nodes shows a ready node	Complete
AWS CLI can call LocalStack STS	Complete
11. Troubleshooting Notes
* kind File Not Found / Execution Error: Initial curl attempt fetched an incomplete binary file. Resolved by downloading the explicit v0.23.0 x86_64 binary release directly and assigning standard executable privileges (chmod +x).
* Docker Daemon Permission Denied: Non-root user lacked permissions for /var/run/docker.sock. Fixed by adding user to the docker group (sudo usermod -aG docker $USER and newgrp docker).
* LocalStack License/Deprecation Exits: Modern LocalStack Pro image tags throw a license requirement or discontinuation notice (s3-latest). Resolved by fixing container image target to standard tag 3.4.0.
12. Conclusion
The Lab 0 environment setup was completed successfully. Docker, AWS CLI v2, kind, kubectl, OpenSSL, oathtool, LocalStack, and the local Kubernetes cluster were installed and verified according to the setup guide.
The environment is ready for Lab 1.
