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
