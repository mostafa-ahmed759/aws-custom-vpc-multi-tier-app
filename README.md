# 🏗️ AWS Custom VPC for a Multi-Tier Web Application
### AWS Core Services Introduction — Capstone Project 1

This project demonstrates the design and deployment of a *custom Virtual Private Cloud (VPC)* on AWS to host a *multi-tier web application*, using only the AWS Management Console.  
It's part of a hands-on capstone project to apply AWS core services.

---

## 🖼️ Architecture Diagram

<img src="./screenshots/Architecture-digram.png" alt="Architecture Diagram" width="100%">

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

---

### 1️⃣ VPC Creation & Configuration
<br>

<img src="./vpc-map.png" alt="Create Custom VPC" width="100%">

*Creating Custom VPC using "VPC and more" with CIDR `10.0.0.0/16` and configuring subnets across Availability Zones.*

---

### 2️⃣ Route Tables Configuration
<br>

<img src="./screenshots/VPC.png" alt="Create Custom VPC" width="100%">

*Private Route Table 1 associated with private subnets for secure internal routing.*

<br>

<img src="./screenshots/Private-RT2.png" alt="Private Route Table 2" width="100%">

*Private Route Table 2 configured for high availability across the second Availability Zone.*

---

### 3️⃣ IAM Role for Systems Manager (SSM)
<br>

<img src="./screenshots/SSM-Role-created1.png" alt="SSM Role Created" width="100%">

*IAM Role created with AmazonSSMManagedInstanceCore policy attached.*

<br>

<img src="./screenshots/SSM-Role-to-ec2-created.png" alt="SSM Role Assigned to EC2" width="100%">

*Attaching the IAM role to EC2 instances allowing secure Session Manager access without SSH.*

---

### 4️⃣ EC2 Web Server Instances Deployment
<br>

<img src="./screenshots/Vpc-created.png" alt="Launch Web Server 2" width="100%">

*Configuring and launching web-server2 instance in the private subnet.*

<br>

<img src="./screenshots/create%202%20instance.png" alt="Create Instances" width="100%">

*Launching EC2 instances across private subnets.*

<br>

<img src="./screenshots/instance.png" alt="Instances Running" width="100%">

*Both backend EC2 web server instances running successfully.*

<br>

<img src="./screenshots/Web-server1-created.png" alt="Web Server 1 Details" width="100%">

*Web Server 1 details with private IP and IAM role attached.*

<br>

<img src="./screenshots/Web-server1-created--.png" alt="Web Server 1 Summary" width="100%">

*Configuration overview and summary for Web Server 1.*

---

### 5️⃣ Web Servers Connectivity & Tests
<br>

<img src="./screenshots/Web-server%201%20Browser%20Test.png" alt="Web Server 1 Browser Test" width="100%">

*Curl test to Web Server 1 private IP verifying web server response.*

<br>

<img src="./screenshots/Web-server%202%20Browser%20Test.png" alt="Web Server 2 Browser Test" width="100%">

*Curl test to Web Server 2 private IP verifying separate zone web response.*

---

### 6️⃣ Security Groups Configuration
<br>

<img src="./screenshots/ALB-SG-inbound.png" alt="ALB SG Inbound" width="100%">

*ALB Security Group inbound rules allowing HTTP (port 80) traffic from anywhere.*

<br>

<img src="./screenshots/ALB-SG-outbound.png" alt="ALB SG Outbound" width="100%">

*ALB Security Group outbound rules forwarding traffic to WebSG.*

---

### 7️⃣ Launch Template Creation
<br>

<img src="./screenshots/Create-LT.png" alt="Create Launch Template" width="100%">

*Configuring EC2 Launch Template with AMI, instance type, and user data bootstrap script.*

<br>

<img src="./screenshots/Template-created.png" alt="Launch Template Created" width="100%">

*Launch Template successfully created and ready for Auto Scaling deployment.*

---

### 8️⃣ Target Group Configuration
<br>

<img src="./screenshots/TG-targets-testing.png" alt="Target Group Created" width="100%">

*Successfully created the target group (Web-TG) for routing traffic to backend instances.*

---

### 9️⃣ Auto Scaling Group (ASG) Setup
<br>

<img src="./screenshots/Create-ASG.png" alt="Create ASG" width="100%">

*Creating Auto Scaling Group using the defined Launch Template.*

<br>

<img src="./screenshots/Create-ASG-instance.png" alt="ASG Instance Sizing" width="100%">

*Configuring group capacity (Desired, Minimum, Maximum instances).*

<br>

<img src="./screenshots/Auto-Scaleing-Group-networking.png" alt="ASG Networking" width="100%">

*Configuring ASG networking across VPC private subnets.*

<br>

<img src="./screenshots/Auto-scaleing-Group-manual.png" alt="ASG Manual Scaling" width="100%">

*Configuring manual scaling policies for ASG.*

<br>

<img src="./screenshots/Auto-scaling-Group-activity-history.png" alt="ASG Activity History" width="100%">

*ASG activity history confirming instances launching successfully.*

---

### 🔟 Application Load Balancer & High Availability Tests
<br>

<img src="./screenshots/ALB-Browser-test1.png" alt="ALB Browser Test 1" width="100%">

*Application Load Balancer directing traffic to Web Server 1.*

<br>

<img src="./screenshots/ALB-Browser-test%202.png" alt="ALB Browser Test 2" width="100%">

*Application Load Balancer balancing traffic to Web Server 2.*

<br>

<img src="./screenshots/ALB-browser-final-testing.png" alt="ALB Final Testing" width="100%">

*Final browser test confirming high availability across all instances.*

---
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
* Delete NAT Gateways and release Elastic IPs (EIPs).
* Delete Application Load Balancer and Target Group.
* Delete the Custom VPC.
*
