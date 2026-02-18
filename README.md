# Project 1: High-Availability Three-Tier Architecture
**Date:** February 2026  
**Engineer:** Ahmet Erdoğan 

## 📌 Project Overview
This project demonstrates a production-ready, three-tier web architecture deployed on AWS. The design focuses on **High Availability (HA)**, **Security**, and **Scalability**. It separates the presentation (ALB), application (EC2), and data (RDS) layers into distinct network tiers across multiple Availability Zones.



## 🏗 Architecture Description
* **Tier 1: Web/Load Balancer Tier** – Public subnets hosting an Application Load Balancer to manage incoming traffic.
* **Tier 2: Application Tier** – Private subnets hosting an Auto Scaling Group of EC2 instances running a Node.js/Python application.
* **Tier 3: Database Tier** – Private subnets hosting a Multi-AZ Amazon RDS instance, isolated from the internet.

## 🚀 How to Deploy
1. **Network:** Create the VPC and 6 subnets (2 Public, 4 Private) using the CIDR blocks defined in `/config/vpc-config.txt`.
2. **Database:** Launch the RDS instance in the private DB subnets.
3. **Application:** Deploy the EC2 instances using the script in `/app/deploy.sh`.
4. **Traffic:** Configure the ALB to route traffic to the application target group.

## 🧪 Testing Instructions
* **Health Check:** Access the ALB DNS name at `/health` to verify instance status.
* **Failover:** Manually terminate one EC2 instance to observe the Auto Scaling Group's self-healing capability.
* **Security:** Attempt to SSH into the DB tier from a public IP to verify network isolation (it should fail).
