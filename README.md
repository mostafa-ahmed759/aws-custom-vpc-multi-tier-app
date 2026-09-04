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
![VPC Creation](./screenshots/Vpc-created.png)  
Custom VPC created with CIDR `10.0.0.0/16` and subnets CIDR ranges defined.

![VPC Overview](./screenshots/VPC_%20us-east-1%20-%20Google%20Chro....png)  
VPC resource map and subnet distribution across Availability Zones.

### 2️⃣ Private Route Tables
![Private RT 1](./screenshots/Private-RT1.png)  
Private Route Table configured to route internet-bound traffic through NAT Gateway.

![Private RT 2](./screenshots/Private-RT2.png)  
Secondary Private Route Table configured for the second Availability Zone.

### 3️⃣ IAM Role for Systems Manager (SSM)
![SSM Role](./screenshots/SSM-Role-created1.png)  
IAM Role created with AmazonSSMManagedInstanceCore policy attached.

![SSM Role to EC2](./screenshots/SSM-Role-to-ec2-created.png)  
Attaching the IAM role to EC2 instances allowing secure Session Manager access.

### 4️⃣ EC2 Web Server Instances
![Create 2 Instances](./screenshots/create%202%20instance.png)  
Launching two EC2 instances into their respective private subnets.

![Web Server 1 Created](./screenshots/Web-server1-created.png)  
Details of Web_Server1 instance in the first private subnet.

![Web Server 1 Summary](./screenshots/Web-server1-created--.png)  
Configuration summary of Web_Server1 including private IP and IAM role.

### 5️⃣ Web Servers Browser & Curl Tests
![Web Server 1 Test](./screenshots/Web-server%201%20Browser%20Test.png)  
Curl test to Web_Server1 private IP verifying web application HTML response.

![Web Server 2 Test](./screenshots/Web-server%202%20Browser%20Test.png)  
Curl test to Web_Server2 private IP verifying separate zone web response.

### 6️⃣ Security Groups Configuration
![ALB SG Inbound](./screenshots/ALB-SG-inbound.png)  
ALB Security Group inbound rules allowing HTTP (port 80) traffic from anywhere.

![ALB SG Outbound](./screenshots/ALB-SG-outbound.png)  
ALB Security Group outbound rules routing traffic exclusively to backend WebSG.

### 7️⃣ Launch Template Creation
![Create LT](./screenshots/Create-LT.png)  
Configuring Launch Template settings, AMI, instance sizing, and user data bootstrap script.

![Template Created](./screenshots/Template-created.png)  
Launch Template successfully created and active.

### 8️⃣ Auto Scaling Group (ASG) Setup
![Create ASG](./screenshots/Create-ASG.png)  
Creating Auto Scaling Group using the defined Launch Template.

![Create ASG Instance Capacity](./screenshots/Create-ASG-instance.png)  
Setting ASG capacity (Desired, Minimum, Maximum instances).

![ASG Networking](./screenshots/Auto-Scaleing-Group-networkin....png)  
Configuring ASG networking with VPC private subnets and load balancer target group.

![ASG Manual](./screenshots/Auto-scaleing-Group-manual.png)  
Auto Scaling Group configured with manual scaling options.

![ASG Activity History](./screenshots/Auto-scaling-Group-activity-his....png)  
ASG Activity History confirming instances launching successfully.

### 9️⃣ Target Group & Load Balancer Testing
![TG Targets Testing](./screenshots/TG-targets-testing.png)  
Target Group registered targets showing Healthy status across web instances.

![ALB Test 1](./screenshots/ALB-Browser-test1.png)  
Load Balancer DNS resolving and directing traffic to the first server.

![ALB Test 2](./screenshots/ALB-Browser-test%202.png)  
Load Balancer directing traffic to the second server showing load balancing in action.

![ALB Final Testing](./screenshots/ALB-browser-final-testing.png)  
Final browser testing confirming balanced requests across scaled instances.

---

## 🌍 Final Result

After the ALB and ASG setup, the application is served securely from private EC2 instances via the ALB. Scaling is automatic and traffic is balanced between instances.

---

## 🔐 Security Configuration Summary

| Security Group | Inbound Rules | Outbound Rules |
| :--- | :--- | :--- |
| **WebSG** | HTTP 80 | from `ALBSG` | Allow all |
| **ALBSG** | HTTP 80 | from `0.0.0.0/0` | HTTP 80 to `WebSG` |

---

## 🧹 Cleanup Steps

To avoid charges:
* Delete EC2 Instances and Auto Scaling Group.
* Delete NAT Gateways and release Elastic IPs (EIPs).
* Delete ALB and Target Group.
* Delete the Custom VPC.
*
