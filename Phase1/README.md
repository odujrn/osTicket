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

##  Overview
This phase covers the **local installation and configuration of osTicket** — the open-source helpdesk ticketing system — hosted on **Internet Information Services (IIS)**.  
It lays the foundation for the production-grade AWS deployment completed in **Phase 2**.

---

##  Technologies Used
- **Windows 10 / Windows Server (21H2 or later)**  
- **IIS (Internet Information Services)**  
- **PHP (7.x or later)**  
- **MySQL Database Server**  
- **HeidiSQL**  
- **osTicket v1.15.8**

---

## ⚙️ Step-by-Step Installation

###  1. Prepare Windows for IIS Hosting
1. Open **Control Panel → Programs → Turn Windows features on or off**  
2. Enable:
   - Internet Information Services  
   - Web Management Tools  
   - IIS Management Console  
   - Application Development Features (.NET Extensibility, CGI, ISAPI)  
3. Click **OK** and wait for IIS installation to complete.  
4. Verify IIS is working by visiting [http://localhost](http://localhost) — you should see the default IIS welcome page.

📸 *Screenshot Suggestion:*  
`./screenshots/iis-enabled.png`

---

### 💾 2. Install Prerequisites

####  Install Components
| Component | Purpose |
|------------|----------|
| **PHP Manager** | Integrates PHP with IIS Management Console. |
| **VC Redist** | Provides runtime components for C++-based PHP and IIS dependencies. |
| **URL Rewrite Module** | Enables clean and dynamic URLs for osTicket. |

📸 *Screenshot Suggestion:*  
`./screenshots/php-manager-installed.png`

####  Install PHP
1. Create folder: `C:\PHP`  
2. Download and extract PHP to `C:\PHP`.  
3. Register PHP in IIS → PHP Manager → **Register new PHP version**.  
4. Restart IIS (`iisreset` in Command Prompt).

📸 *Screenshot Suggestion:*  
`./screenshots/php-registration.png`

####  Install MySQL and HeidiSQL
1. Install MySQL → Choose **Typical Setup → Standard Configuration**  
2. Create root password (e.g., `password123!`)  
3. Install HeidiSQL and connect using root credentials.  
4. Create a new database called **osTicket**.

📸 *Screenshot Suggestion:*  
`./screenshots/heidisql-database.png`

---

###  3. Install osTicket Web Application
1. Download **osTicket v1.15.8** from the official site.  
2. Extract the “upload” folder and copy it to `C:\inetpub\wwwroot`.  
3. Rename folder from `upload` → `osTicket`.  
4. Restart IIS.  
5. Open IIS → **Sites → Default Web Site → osTicket → Browse *:80***  
6. Confirm osTicket installer loads in your browser.

📸 *Screenshot Suggestion:*  
`./screenshots/osticket-install-page.png`

---

### 🔧 4. Enable PHP Extensions and Assign Permissions

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

📸 *Screenshot Suggestion:*  
`./screenshots/permissions.png`

---

###  5. Web Installation Wizard
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

📸 *Screenshot Suggestion:*  
`./screenshots/setup-complete.png`

---

###  6. Post-Installation Cleanup
1. **Delete:** `C:\inetpub\wwwroot\osTicket\setup`  
2. **Set Read-Only permissions:**  
C:\inetpub\wwwroot\osTicket\include\ost-config.php


📸 *Screenshot Suggestion:*  
`./screenshots/cleanup.png`

---

### 🔗 7. Access Links
- **Agent Login:** [http://localhost/osTicket/scp/login.php](http://localhost/osTicket/scp/login.php)  
- **End-User Portal:** [http://localhost/osTicket/](http://localhost/osTicket/)

📸 *Screenshot Suggestion:*  
`./screenshots/agent-enduser-login.png`

---

##  Learning Outcomes
- Gained experience deploying and configuring IIS web servers.  
- Understood PHP–MySQL integration and troubleshooting.  
- Practiced database management using HeidiSQL.  
- Prepared environment for migration to AWS (Phase 2).  

---

##  Next Phase
Proceed to [Phase 2 — AWS Production Deployment](../Phase2)  
where the osTicket instance is migrated and automated using **AWS EC2, RDS, S3, and Terraform**.

---

##  Author
**[Oduamadi Ndubuisi https://www.linkedin.com/in/ndubuisi-oduamadi/)]**  
Cloud & Systems Administrator | DevOps Enthusiast  
📧 ndu_euro@outlook.com 

---

⭐ *If this project helped you, please consider starring the repository!*
