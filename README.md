# AWS Three-Tier High-Availability Architecture
**Author:** [Ahmet Erdoğan]
**Project Status:** 🟢 Day 1 Complete (Infrastructure & Networking)

## 📌 Project Overview
This repository contains the architecture and configuration for a secure, highly available three-tier web application on AWS. The design ensures that each layer (Web, App, and Database) is physically and logically isolated to follow security best practices while maintaining zero downtime through a Multi-Availability Zone (Multi-AZ) deployment.

## 🏗️ Architecture Design
The infrastructure is divided into three distinct tiers across two Availability Zones (`us-east-1a` and `us-east-1b`):

1. **Public Web Tier (ALB)**: Contains the Internet Gateway and Public Subnets. This is the only layer reachable from the public internet.
2. **Private Application Tier**: Contains the EC2 instances. This layer is isolated and can only communicate with the Web Tier and the Database Tier.
3. **Private Database Tier**: Contains the RDS PostgreSQL instance. This layer is the most restricted and has no internet access.

## 🛠️ Current Infrastructure (Day 1)
The following networking components have been successfully deployed:

| Component | Specification |
| :--- | :--- |
| **VPC CIDR** | 10.0.0.0/16 |
| **Region** | us-east-1 (N. Virginia) |
| **Public Subnets** | 10.0.1.0/24 (1a), 10.0.2.0/24 (1b) |
| **Private App Subnets** | 10.0.3.0/24 (1a), 10.0.4.0/24 (1b) |
| **Private DB Subnets** | 10.0.5.0/24 (1a), 10.0.6.0/24 (1b) |
| **Gateways** | 1 Internet Gateway (IGW) attached to ce-project-vpc |

## 🚦 Routing & Security
- **Public Route Table**: Routes `0.0.0.0/0` traffic through the IGW.
- **Private Route Table**: Local routing only (Isolation Mode enabled).
- **Security Strategy**: All private subnets are isolated by default until Security Groups are configured in Day 2.

## 📂 Repository Structure
- `/architecture`: Contains the VPC Resource Map and Network Diagrams.
- `/config`: Technical specifications including CIDR blocks and Subnet IDs.
- `ARCHITECTURE.md`: Detailed rationale for the chosen design.
