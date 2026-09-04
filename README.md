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

### 2️⃣ Private Route Tables
Private Route Tables configured for internal routing and NAT Gateway traffic.
![Private RT 1](./screenshots/Private-RT1.png)
![Private RT 2](./screenshots/Private-RT2.png)

### 3️⃣ IAM Role for Systems Manager (SSM)
IAM Role configured with necessary permissions to allow EC2 instances to communicate via SSM.
![SSM Role](./screenshots/SSM-Role-created1.png)
![SSM Role EC2](./screenshots/SSM-Role-to-ec2-created.png)

### 4️⃣ Web Server Instances Details
Instances deployed across the private subnets.
![Instances](./screenshots/instance.png)
![Web Server 1](./screenshots/Web-server1-created.png)

### 5️⃣ Web Servers Browser / Curl Tests
Verifying application response and network connectivity from both backend servers.
![Web Server 1 Test](./screenshots/Web-server%201%20Browser%20Test.png)
![Web Server 2 Test](./screenshots/Web-server%202%20Browser%20Test.png)

### 6️⃣ Security Groups Configuration
Security Group rules restricting access to backend instances and allowing public access to ALB.
![ALB SG Inbound](./screenshots/ALB-SG-inbound.png)
![ALB SG Outbound](./screenshots/ALB-SG-outbound.png)

### 7️⃣ Launch Template & Auto Scaling Group Creation
Launch Template configured and Auto Scaling Group initialized.
![Create LT](./screenshots/Create-LT.png)
![Template Created](./screenshots/Template-created.png)
![Create ASG](./screenshots/Create-ASG.png)
![Create ASG Instance](./screenshots/Create-ASG-instance.png)

### 8️⃣ Auto Scaling Group Networking & Manual Scaling
Auto Scaling Group scaling configuration, networking setup, and activity history.
![ASG Networking](./screenshots/Auto-Scaleing-Group-networkin....png)
![ASG Manual](./screenshots/Auto-scaleing-Group-manual.png)
![ASG Activity History](./screenshots/Auto-scaling-Group-activity-his....png)

### 9️⃣ Target Group & Load Balancer Testing
Target Group health checks healthy and Application Load Balancer distributing HTTP requests.
![TG Targets Testing](./screenshots/TG-targets-testing.png)
![ALB Test 1](./screenshots/ALB-Browser-test1.png)
![ALB Test 2](./screenshots/ALB-Browser-test%202.png)
![ALB Final Testing](./screenshots/ALB-browser-final-testing.png)

---

## 🌍 Final Result

After the ALB and ASG setup, the application is served securely from private EC2 instances via the ALB. Scaling is automatic and traffic is balanced between instances.

---

## 🔐 Security Configuration Summary

| Security Group | Inbound Rules | Outbound Rules |
| :--- | :--- | :--- |
| **WebSG** | HTTP (80) from `ALBSG` | Allow all |
| **ALBSG** | HTTP (80) from `0.0.0.0/0` | HTTP (80) to `WebSG` |

---

## 🧹 Cleanup Steps

To avoid charges:
* Delete EC2 Instances and Auto Scaling Group.
* Delete NAT Gateways and release Elastic IPs.
* Delete ALB and Target Group.
* Delete Custom VPC.
*
