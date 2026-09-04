# 🏗️ AWS Custom VPC for a Multi-Tier Web Application
### AWS Core Services Introduction — Capstone Project 1

This project demonstrates the design and deployment of a *custom Virtual Private Cloud (VPC)* on AWS to host a *multi-tier web application*, using only the AWS Management Console.  
It's part of a hands-on capstone project to apply AWS core services.

---

## 🖼️ Architecture Diagram

![Architecture Diagram](./screenshots/Architecture-digram.png)

---

## 📐 Architecture Overview

* **Custom VPC with CIDR block:** `10.0.0.0/16`
* **2 Public Subnets:** `10.0.10.0/24`, `10.0.20.0/24`
* **2 Private Subnets:** `10.0.100.0/24`, `10.0.200.0/24`
* **Internet Gateway (IGW)**
* **2 NAT Gateways:** Each in a public subnet with Elastic IP (EIP)
* **2 EC2 Instances:** (`Web_Server1`, `Web_Server2`) in Private Subnets
* **IAM Role (`ec2tossm`):** To allow SSM Session Manager access
* **Application Load Balancer (ALB):** Internet-facing
* **Target Group (`WebTG`):** With EC2 instances registered
* **Auto Scaling Group (ASG):** Configured with Launch Template
* **Security Groups:** `WebSG`, `ALBSG`

---

## 🧱 AWS Services Used

* **Amazon VPC** (Subnets, Route Tables, IGW, NAT Gateways)
* **Amazon EC2** (t2.micro, Amazon Linux 2023)
* **AWS Systems Manager (SSM)** (Session Manager for secure access without SSH)
* **Application Load Balancer (ALB)** & Target Groups
* **Auto Scaling Group (ASG)**
* **AWS Identity and Access Management (IAM)**

---

## 📸 Deployment Steps & Screenshots

Below are the main deployment steps with corresponding screenshots from the AWS Console.

### 1️⃣ VPC Creation
Custom VPC created with CIDR `10.0.0.0/16` and subnets CIDR ranges defined.
![VPC Creation](./screenshots/Vpc-created.png)

### 2️⃣ Web Server 1 Details
Details of Web Server 1 instance created in private subnet.
![Web Server 1](./screenshots/Web-server1-created.png)

### 3️⃣ Web Server 1 Browser Test
Testing connectivity and verifying web application response from Web Server 1.
![Web Server 1 Test](./screenshots/Web-server%201%20Browser%20Test.png)

### 4️⃣ Web Server 2 Browser Test
Testing connectivity and verifying web application response from Web Server 2.
![Web Server 2 Test](./screenshots/Web-server%202%20Browser%20Test.png)

### 5️⃣ Security Groups Configuration
Security Group rules configured for ALB and Web Servers.
![ALB SG Inbound](./screenshots/ALB-SG-inbound.png)
![ALB SG Outbound](./screenshots/ALB-SG-outbound.png)

### 6️⃣ ALB Browser Test
Load Balancer distributing incoming traffic across the backend web instances.
![ALB Test 1](./screenshots/ALB-Browser-test1.png)
![ALB Test 2](./screenshots/ALB-Browser-test%202.png)
![Final Testing](./screenshots/ALB-browser-final-testing.png)

---

## 🔐 Security Configuration Summary

| Security Group | Inbound Rules | Outbound Rules |
| :--- | :--- | :--- |
| **WebSG** | HTTP (80) from `ALBSG` | Allow all |
| **ALBSG** | HTTP (80) from `0.0.0.0/0` | HTTP (80) to `WebSG` |

---

## 🧹 Cleanup Steps

To avoid incurring unexpected charges:
* Delete EC2 Instances and Auto Scaling Group.
* Delete NAT Gateways and release Elastic IPs (EIPs).
* Delete Application Load Balancer and Target Group.
* Delete the Custom VPC.
*
