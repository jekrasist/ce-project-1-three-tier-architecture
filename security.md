# Security Strategy & Design

## 🛡️ Network Isolation Strategy
We utilize a **layered defense** strategy. Only the Web Tier (ALB) is exposed to the public internet. All application logic and data reside in private subnets with no direct inbound route from the internet.

### Security Group Rules
| Tier | Protocol | Port | Source | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Web (ALB)** | TCP | 80/443 | 0.0.0.0/0 | Public Web Traffic |
| **App (EC2)** | TCP | 3000 | Web-SG | Traffic from Load Balancer only |
| **Data (RDS)** | TCP | 5432 | App-SG | Database queries from App only |



## 🔑 IAM Roles & Policies
* **EC2 Instance Role:** Granted `AmazonSSMManagedInstanceCore` for secure terminal access without SSH keys.
* **S3 Access:** Limited to read-only access for application configuration files.

## ⚠️ Potential Vulnerabilities & Mitigations
* **Vulnerability:** DDoS Attack on Public ALB.
    * *Mitigation:* Enable AWS Shield Standard and set up WAF (Web Application Firewall) rules.
* **Vulnerability:** Data Leakage from RDS.
    * *Mitigation:* Enable RDS Encryption at rest using AWS KMS and ensure the DB is in a private subnet.
# Security Strategy (Principle of Least Privilege)

## 🛡️ Security Groups (SGs) Logic
The architecture follows a strict "chain of trust" where each tier only accepts traffic from the tier directly above it.

### 1. Web-Tier-SG (Public)
* **Description**: Entry point for users.
* **Inbound**: 
    * HTTP (Port 80) from `0.0.0.0/0`
    * HTTPS (Port 443) from `0.0.0.0/0`
* **Purpose**: Allows public web traffic to reach the Load Balancer.

### 2. App-Tier-SG (Private)
* **Description**: Backend logic layer.
* **Inbound**: 
    * Custom TCP (Port 5000) from **Web-Tier-SG** (Source = SG ID)
* **Purpose**: Ensures only our web server can talk to our app server.

### 3. Database-Tier-SG (Private)
* **Description**: Data storage layer.
* **Inbound**: 
    * PostgreSQL (Port 5432) from **App-Tier-SG** (Source = SG ID)
* **Purpose**: Completely isolates data from the internet.



# Security Strategy (Principle of Least Privilege)

## 🛡️ Security Groups (SGs) Logic
The architecture follows a strict "chain of trust" where each tier only accepts traffic from the tier directly above it.

### 1. Web-Tier-SG (Public)
* **Description**: Entry point for users.
* **Inbound**: 
    * HTTP (Port 80) from `0.0.0.0/0`
    * HTTPS (Port 443) from `0.0.0.0/0`
* **Purpose**: Allows public web traffic to reach the Load Balancer.

### 2. App-Tier-SG (Private)
* **Description**: Backend logic layer.
* **Inbound**: 
    * Custom TCP (Port 5000) from **Web-Tier-SG** (Source = SG ID)
* **Purpose**: Ensures only our web server can talk to our app server.

### 3. Database-Tier-SG (Private)
* **Description**: Data storage layer.
* **Inbound**: 
    * PostgreSQL (Port 5432) from **App-Tier-SG** (Source = SG ID)
* **Purpose**: Completely isolates data from the internet.



## ✅ Security Group Implementation

| Name | Purpose | Inbound Rules | Outbound Rules |
| :--- | :--- | :--- | :--- |
| **Web-SG** | Load Balancer | Port 80/443 (0.0.0.0/0) | All traffic to App-SG |
| **App-SG** | App Instances | Port 5000 (from Web-SG) | All traffic to DB-SG |
| **DB-SG** | RDS Database | Port 5432 (from App-SG) | None (Locked) |
