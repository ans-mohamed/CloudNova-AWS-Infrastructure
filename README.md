# ☁️ CloudNova AWS Infrastructure

[![AWS Architecture](https://img.shields.io/badge/AWS-Multi--VPC%20%7C%20Transit%20Gateway-orange?logo=amazon-aws)](https://aws.amazon.com)
[![Infrastructure](https://img.shields.io/badge/DevOps-Automation%20%7C%20Cloud-blue)](https://github.com/ans-mohamed/CloudNova-AWS-Infrastructure)
[![Status](https://img.shields.io/badge/Status-Portfolio%20Project-success)](https://github.com/ans-mohamed/CloudNova-AWS-Infrastructure)[cite: 1]

**Secure • Scalable • Highly Available • Monitored**  
A structured AWS infrastructure architecture designed for **CloudNova** to provide a secure, scalable, and highly available environment for hosting web applications, managing databases, sharing files, and automating backups.

---

## 📌 Project Overview

CloudNova is a technology project that requires a reliable cloud infrastructure to support its application, data, and operations teams.
This project demonstrates the design and implementation of a multi-VPC AWS architecture that provides:
- Secure network isolation.
- High availability across multiple Availability Zones.
- Scalable application hosting.
- Centralized network connectivity.
- Private database access.
- Shared file storage.
- Automated database backups using **Bash Scripts & Cron Jobs**.
- Infrastructure monitoring and alerting.

The architecture follows AWS security and networking best practices, applying network segmentation and least-privilege access wherever possible.

---

## 🏗️ Architecture Diagram

![CloudNova Architecture](./IMG.CloudNova.PNG)[cite: 1]

**AWS Region:** US East (N. Virginia) — `us-east-1`  
The infrastructure is divided into three independent VPCs connected through a centralized Transit Gateway.

---

## ☁️ AWS Services Used

| AWS Service | Purpose |
| :--- | :--- |
| **Amazon VPC** | Network isolation and segmentation |
| **EC2** | Application and Bastion instances |
| **Application Load Balancer** | Public application entry point |
| **Auto Scaling Group** | Automatic application scaling |
| **NAT Gateway** | Controlled outbound internet access |
| **Internet Gateway** | Internet connectivity for public subnets |
| **Amazon RDS (PostgreSQL)** | Managed relational database |
| **Amazon EFS** | Shared file storage across Availability Zones |
| **Amazon S3** | Database backup storage |
| **AWS Transit Gateway** | Centralized connectivity between VPCs |
| **Amazon CloudWatch** | Infrastructure monitoring and alarms |
| **Amazon SNS** | Email notifications and alerts |

---

## 🌐 Network Architecture

The infrastructure consists of three isolated VPCs:

### 1. Operations VPC
* **CIDR:** `100.3.0.0/16`
* **Purpose:** Designed for infrastructure administration and operational management.
* **Components:** Public Subnet, Bastion Host (EC2), Internet Gateway.
* *Note:* The Bastion Host provides a controlled SSH access point for administrators to manage private infrastructure resources.

### 2. Application VPC
* **CIDR:** `100.1.0.0/16`
* **Purpose:** Hosts the web application infrastructure.
* **Components:** Public Subnets across multiple AZs, Private Application Subnets, Internet-facing ALB, Auto Scaling Group, EC2 Web Servers, NAT Gateways, and Amazon EFS.
* **Traffic Flow:** Internet → Application Load Balancer → Private EC2 Instances (Application servers are not directly exposed to the public internet).
* **Auto Scaling Configuration:** 
  * Minimum Capacity: 2 | Desired Capacity: 2 | Maximum Capacity: 4
  * CPU Scaling Threshold: 80%

### 3. Data VPC
* **CIDR:** `100.2.0.0/16`
* **Purpose:** Dedicated to database and backup services.
* **Components:** Private Subnets across AZs, Amazon RDS (PostgreSQL), and Amazon S3 Backup Bucket.
* *Note:* The RDS instance is configured without a public IP address, and access is strictly restricted through security group rules.

---

## 🔐 Security Architecture

Security is implemented using strict network isolation and Security Groups:
* **Bastion SG:** Inbound SSH (Port 22) from authorized administrator IP.
* **ALB SG:** Inbound HTTP (Port 80) and HTTPS (Port 443) from the internet.
* **Web Server SG:** Inbound HTTP (Port 80) from ALB SG; Inbound SSH (Port 22) from Bastion SG.
* **EFS SG:** Inbound NFS (Port 2049) from Web Server SG.
* **RDS SG:** Inbound PostgreSQL (Port 5432) from authorized application sources.
* **Key Principles:** Least Privilege Access, Private Database Connectivity, Network Segmentation, and Restricted Administrative Access.

---

## 🔄 Shared Storage — Amazon EFS

Amazon EFS provides a shared file system for application servers where both EC2 instances mount the same EFS file system, ensuring:
- Shared application files and persistent storage.
- Multi-AZ accessibility and scalable file system capacity.

---

## 💾 Backup & Disaster Recovery (Bash Script & Cron Job)

Database backups are fully automated using a custom **Bash Script** combined with a Linux **Cron Job** for scheduled execution:
1. **Automation Script:** A custom Bash script (`backup.sh`) executes `pg_dump` to securely extract the PostgreSQL database.
2. **Scheduled Execution:** Linux **Cron Job** triggers the backup script automatically at regular intervals (e.g., daily).
3. **Cloud Storage:** The generated backup archive is securely uploaded to **Amazon S3** with **S3 Versioning** and lifecycle policies enabled for retention management.

---

## 📊 Monitoring & Notifications

* **Amazon CloudWatch** monitors infrastructure performance (configured with an 80% CPU utilization alarm threshold).
* **Alert Workflow:** EC2 CPU Utilization ≥ 80% → CloudWatch Alarm → Amazon SNS Topic → Operations Team Email Notification (`ops-team@cloudnova.com`).

---

## 🔗 Centralized Networking — Transit Gateway

AWS Transit Gateway acts as the central networking hub connecting the Operations VPC, Application VPC, and Data VPC through dedicated attachments, simplifying communication and providing a scalable foundation for future network expansion.

---

## 🚀 Key Features

- Multi-VPC Network Architecture with Transit Gateway.
- Multi-AZ High Availability Design.
- Secure Bastion Host Administration & Private Database.
- Internet-Facing ALB with EC2 Auto Scaling.
- Shared Storage Using Amazon EFS.
- **Automated Backup Workflow via Bash Script and Cron Job.**
- S3 Versioning and Lifecycle Management.
- CloudWatch Monitoring and SNS Notifications.

---

## 🛠️ Skills Demonstrated

* **Cloud Computing:** AWS Infrastructure Design, Networking, High Availability, and Cloud Security.
* **Networking:** VPC Design, Public/Private Subnets, Route Tables, NAT/Internet Gateways, Transit Gateway, and Security Groups.
* **Compute & Storage:** Amazon EC2, ALB, ASG, Amazon EFS, RDS, and S3.
* **Monitoring & Automation:** Amazon CloudWatch, SNS, Linux Shell Scripting (Bash), Cron Jobs, and PostgreSQL Backup Automation.

---

## 📁 Project Structure

```text
CloudNova-AWS-Infrastructure/
│
├── README.md
├── IMG.CloudNova.PNG
├── docs/
├── scripts/
│   └── backup.sh
└── .gitignore
```

---

## 👨‍💻 Author

**Ans Mohamed Abdulalim (ans-mohamed)**  
* **Role:** Cloud & DevOps Enthusiast | CS Department Student  
* **Email:** `ans.mohamed442005@gmail.com`  
* **GitHub:** [ans-mohamed](https://github.com/ans-mohamed)

---

⭐ *If you find this project interesting, feel free to explore the architecture and its implementation!*
