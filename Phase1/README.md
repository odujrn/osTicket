<p align="center">

  <img width="700" height="200" alt="image" src="https://github.com/user-attachments/assets/9ddcb012-79d3-49e9-8e24-94f509d34060" />
</p>

<h1 align="center">osTicket Helpdesk Deployment on AWS</h1>
<h3 align="center">Windows Server 2025 • IIS • PHP • MySQL • AWS Cloud</h3>

<p align="center">
  <img src="https://img.shields.io/badge/Phase-1%20%7C%20Phase%202-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge&logo=amazonaws" />
  <img src="https://img.shields.io/badge/osTicket-Helpdesk-success?style=for-the-badge" />
</p>

---

## Project Overview
This repository documents my full **osTicket Helpdesk Deployment journey**, from a local Windows IIS setup to a production-grade AWS deployment.  
It demonstrates my experience with:
- Windows Server administration  
- IIS, PHP, and MySQL setup  
- AWS cloud infrastructure (EC2, RDS, S3, Route 53, ACM, CloudWatch)  
- Automation using Terraform and PowerShell  
- Security hardening and performance optimization
  
It lays the foundation for the production-grade AWS deployment completed in **Phase 2**.

---
## Phase 1 — Local IIS Deployment
📁 [Phase1](./Phase1)

##  Technologies Used
- **Windows 10 / Windows Server (21H2 or later)**  
- **IIS (Internet Information Services)**  
- **PHP (7.x or later)**  
- **MySQL Database Server**  
- **HeidiSQL**  
- **osTicket v1.15.8**

### Highlights
- Configured IIS web server to host osTicket locally  
- Installed and configured PHP extensions and MySQL database  
- Enabled necessary IIS modules and permissions  
- Verified access for both end-users and agents 
---

## Step-by-Step Installation

### 1. Prepare Windows for IIS Hosting
1. Open **Control Panel → Programs → Turn Windows features on or off**  
2. Enable:
   - Internet Information Services  
   - Web Management Tools  
   - IIS Management Console  
   - Application Development Features (.NET Extensibility, CGI, ISAPI)  
3. Click **OK** and wait for IIS installation to complete.  
4. Verify IIS is working by visiting [http://localhost](http://localhost) — you should see the default IIS welcome page.



---

### 2. Install Prerequisites

####  Install Components
| Component | Purpose |
|------------|----------|
| **PHP Manager** | A feature that will allow us to interact with the scripts through the Management Console. |
| **VC Redist** | Provides the necessary runtime components for running C++ applications, essential for certain dependencies of PHP and IIS. |
| **URL Rewrite Module for IIS** | Allows for the customization of URLs, enabling redirection and URL rewriting for osTicket. |
| **mySQL** | The database which will contain the data from osTicket. |
| **HeidiSQL:** | The database manager or GUI we will use to interact with the database. |
| **PHP:** | The server-side scripting language used to display the HTML webpages of osTicket. |


#### Install PHP
1. Create folder: `C:\PHP`  
2. Download and extract PHP to `C:\PHP`.  
3. Register PHP in IIS → PHP Manager → **Register new PHP version**.  
4. Restart IIS (`iisreset` in Command Prompt).


####  Install MySQL and HeidiSQL
1. Install MySQL → Choose **Typical Setup → Standard Configuration**  
2. Create root password (e.g., `password123!`)  
3. Install HeidiSQL and connect using root credentials.  
4. Create a new database called **osTicket**.



---

###  3. Install osTicket Web Application
1. Download **osTicket v1.15.8** from the official site.  
2. Extract the “upload” folder and copy it to `C:\inetpub\wwwroot`.  
3. Rename folder from `upload` → `osTicket`.  
4. Restart IIS.  
5. Open IIS → **Sites → Default Web Site → osTicket → Browse *:80***  
6. Confirm osTicket installer loads in your browser.



---

### 4. Enable PHP Extensions and Assign Permissions

#### Enable the following PHP Extensions in PHP Manager:
- `php_imap.dll`  
- `php_intl.dll`  
- `php_opcache.dll`

#### Configure Permissions:
1. Rename:  
C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php
→ C:\inetpub\wwwroot\osTicket\include\ost-config.php

2. Right-click → Properties → Security → Disable inheritance → Remove all.  
3. Add `Everyone` → Allow **Full Control**.



---

### 5. Web Installation Wizard
1. Navigate to [http://localhost/osTicket/setup](http://localhost/osTicket/setup)  
2. Fill in the details:

| Field | Value |
|-------|-------|
| **Helpdesk Name** | osTicket Helpdesk |
| **Default Email** | (support@example.com) |
| **MySQL Database** | osTicket |
| **MySQL Username** | root |
| **MySQL Password** | your_password |

3. Click **Install Now!**  
4. Once installed, you’ll see a success message confirming osTicket setup.



---

### 6. Post-Installation Cleanup
1. **Delete:** `C:\inetpub\wwwroot\osTicket\setup`  
2. **Set Read-Only permissions:**  
C:\inetpub\wwwroot\osTicket\include\ost-config.php




---

### 7. Access Links
- **Agent Login:** [http://localhost/osTicket/scp/login.php](http://localhost/osTicket/scp/login.php)  
- **End-User Portal:** [http://localhost/osTicket/](http://localhost/osTicket/)



---

## Learning Outcomes
- Gained experience deploying and configuring IIS web servers.  
- Understood PHP–MySQL integration and troubleshooting.  
- Practiced database management using HeidiSQL.  
- Prepared environment for migration to AWS (Phase 2).  

---

## osTicket — Phase 2: AWS Production Deployment

This phase transforms the osTicket project from a local server into a **cloud-hosted, production-ready environment** using AWS infrastructure and automation tools.

---

## Overview
The goal of Phase 2 is to migrate osTicket to the AWS cloud and enhance scalability, reliability, and security using real-world cloud practices.  
It demonstrates how to deploy and manage an enterprise-grade ticketing platform with automated provisioning and monitoring.

---

## Technologies Used
- **AWS Services:** EC2, RDS (MySQL), S3, Route 53, ACM, CloudWatch  
- **Compute Stack:** Windows Server 2025, IIS, PHP, osTicket  
- **Automation Tools:** Terraform, PowerShell  
- **Database:** Amazon RDS (MySQL 8.0)  

---

## Implementation Summary
1. **Infrastructure Setup (AWS):**
   - Created VPC, subnets, Internet Gateway, and routing.
   - Provisioned EC2 instance (Windows Server 2025 Base AMI).
   - Created RDS (MySQL) in a private subnet.
   - Configured S3 bucket for osTicket file storage.

2. **DNS & Security:**
   - Registered custom domain in Route 53.
   - Used AWS Certificate Manager (ACM) for SSL certificate.
   - Applied least-privilege IAM roles for EC2 and RDS access.

3. **Application Deployment:**
   - Installed IIS, PHP, and osTicket on EC2.
   - Updated osTicket configuration to connect to RDS.
   - Secured web access using HTTPS via ACM.

4. **Automation & Monitoring:**
   - Built Terraform scripts for infrastructure provisioning.
   - Automated server setup using PowerShell.
   - Enabled CloudWatch dashboards and alarms.

---

## Learning Outcomes
- Migrated a local system to a cloud-native architecture.  
- Learned AWS networking (VPC, subnets, security groups).  
- Implemented Infrastructure as Code using Terraform.  
- Automated server provisioning via PowerShell.  
- Gained experience with SSL, DNS, and cloud monitoring.  

---

## Screenshots
All screenshots for this phase are stored in the [screenshots folder](./screenshots).  
Recommended captures include:
- AWS VPC Dashboard  
- RDS Instance Details  
- S3 Bucket Configuration  
- IIS SSL Binding  
- Terraform Apply Output  
- osTicket Admin Portal on AWS  

---

## Future Enhancements
- CI/CD pipeline using GitHub Actions.  
- Dockerized osTicket stack.  
- RDS snapshot automation via Lambda.  
- Multi-region Route 53 failover configuration.  
  
---

## Author
**[Oduamadi Ndubuisi](https://www.linkedin.com/in/ndubuisi-oduamadi/)**  
Cloud & Systems Administrator | DevOps Enthusiast 

ndu_euro@outlook.com  

---

⭐ *If this project helped you, please consider starring the repository!*



