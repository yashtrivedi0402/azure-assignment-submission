# ☁️ Azure Cloud Engineer Assignment 1

> **Hands-on Azure lab covering Microsoft Entra ID, Identity & Access Management, MFA, Azure Blob Storage, and Windows drive mapping.**

---

## 📌 Assignment Overview

This repository documents the practical implementation of the first two tasks of the **Cloud Engineer Assignment 1**.

### Tasks Covered

| Task | Area | Status |
|---|---|---|
| **Task 1** | Microsoft Entra ID, Users, Admin Roles & MFA | ✅ Completed |
| **Task 2** | Azure Blob Storage & Drive Mapping | ✅ Completed |
| **Task 3** | Microsoft 365 Licensing | ✅ Completed |

---

# 🔐 Task 1 — Microsoft Entra ID, Users, Roles & MFA

## 🎯 Objective

Create users in Azure Active Directory (now **Microsoft Entra ID**), assign administrator roles, and enable Multi-Factor Authentication (MFA).

## 🛠️ Implementation

The task was completed using the Azure Portal.

### Step 1 — Microsoft Entra ID

Opened **Microsoft Entra ID** from the Azure Portal and used the tenant's identity-management features.

### Step 2 — Created Users

Two administrator users were created:

- **Azure Global Administrator**
- **Azure User Administrator**

### Step 3 — Assigned Roles

The required administrative roles were assigned:

| User | Assigned Role |
|---|---|
| Azure Global Administrator | **Global Administrator** |
| Azure User Administrator | **User Administrator** |

### Step 4 — Enabled MFA

Per-user MFA was enabled for both administrator users.

This adds an additional authentication factor beyond the username and password.

## 🔄 Authentication Flow

```text
Username
    ↓
Password
    ↓
MFA Verification
    ↓
Azure Access
```

## ✅ Result

- Microsoft Entra ID configured
- Two users created
- Global Administrator role assigned
- User Administrator role assigned
- MFA enabled for both users

## 📸 Evidence

Screenshots included with the submission demonstrate:

<img width="1896" height="776" alt="Screenshot 2026-09-07 004509" src="https://github.com/user-attachments/assets/b35607c4-9d92-4d50-b41a-20a241bfa1cd" />

<img width="1887" height="662" alt="Screenshot 2026-09-07 005052" src="https://github.com/user-attachments/assets/2b8f6ae1-9325-49be-b28c-da50fb2e296b" />

<img width="1872" height="627" alt="Screenshot 2026-09-07 004758" src="https://github.com/user-attachments/assets/e7a38c27-7fc0-4809-8681-fa447311c8c0" />


<img width="1158" height="600" alt="Screenshot 2026-09-07 005620" src="https://github.com/user-attachments/assets/bf836a8d-1203-43bf-890c-be555bb5643e" />

> **Security Note:** Administrator passwords and authentication secrets are intentionally not stored in this repository.

---

# 🗄️ Task 2 — Azure Blob Storage & Drive Mapping

## 🎯 Objective

Create Azure Blob Storage and make the Blob container accessible as a drive on the Windows laptop.

## 🛠️ Implementation

### Step 1 — Created Storage Account

Created an Azure Storage Account with:

- **Performance:** Standard
- **Redundancy:** LRS (Locally Redundant Storage)

The Storage Account acts as the parent resource for Azure Storage services.

### Step 2 — Created Blob Container

Created the container:

```text
assignment-data
```

A test HTML file was uploaded to the container.

```text
Storage Account
└── assignment-data
    └── ish-habit-tracker-working.html
```

### Step 3 — Installed rclone & WinFsp

Since Azure Blob Storage is object storage and is not natively mounted as an SMB Windows drive, **rclone** was used together with **WinFsp**.

```text
Azure Blob Storage
        ↓
      rclone
        ↓
      WinFsp
        ↓
   Windows File System
        ↓
       Z: Drive
```

### Step 4 — Configured rclone

An Azure Blob remote named:

```text
azureblob
```

was configured using the Storage Account credentials.

The connection was verified using:

```powershell
rclone lsd azureblob:
```

and:

```powershell
rclone ls azureblob:assignment-data
```

The container and uploaded file were successfully listed.

### Step 5 — Mounted the Blob Container

The container was mounted as the Windows `Z:` drive using:

```powershell
rclone mount azureblob:assignment-data Z: --vfs-cache-mode full
```

The Blob container then became accessible through Windows File Explorer as:

```text
Z:\
```

## 🔄 Data Access Flow

```text
Windows Laptop
      │
      ↓
    Z: Drive
      │
      ↓
    WinFsp
      │
      ↓
    rclone
      │
      ↓
Azure Blob Storage
      │
      ↓
assignment-data
```

## ✅ Result

- Azure Storage Account created
- Blob container created
- Test file uploaded
- rclone configured successfully
- Blob container successfully mounted as `Z:` on Windows
- Azure Blob contents accessible through File Explorer

## 📸 Evidence

Screenshots included with the submission demonstrate:

<img width="922" height="622" alt="Screenshot 2026-09-07 022022" src="https://github.com/user-attachments/assets/d18e66ff-ce38-4d72-946c-64b6d4674375" />

<img width="808" height="527" alt="Screenshot 2026-09-07 033550" src="https://github.com/user-attachments/assets/f5483583-0037-4442-b740-31b38436ec9a" />

<img width="897" height="242" alt="Screenshot 2026-09-07 033705" src="https://github.com/user-attachments/assets/a2bd1e8c-0116-421f-b107-f6408d9b78f7" />

<img width="1085" height="331" alt="Screenshot 2026-09-07 033713" src="https://github.com/user-attachments/assets/ae1a89d1-3425-4190-9b83-e34d92bd9315" />

<img width="1917" height="712" alt="Screenshot 2026-09-07 033926" src="https://github.com/user-attachments/assets/6dfefe61-5a9f-40e3-abc6-96a2455b7cc9" />


> **Security Note:** Storage Account access keys are credentials and are not included in this repository.

---

# 🧠 Key Concepts Learned

### Microsoft Entra ID

Azure's cloud identity and access-management platform for managing users, roles, authentication, and access.

### Global Administrator

A highly privileged Microsoft Entra administrative role.

### User Administrator

A role focused primarily on managing users and related identity administration.

### MFA

Adds an additional authentication factor to strengthen account security.

### Azure Blob Storage

Azure's object storage service for storing unstructured data such as files, documents, images, videos, logs, and backups.

### rclone

A command-line tool that can connect cloud storage to local filesystem interfaces.

### WinFsp

Provides the Windows filesystem interface required for tools such as rclone to mount cloud storage.

---

# ☁️ AWS vs Azure — Quick Mapping

| AWS | Azure |
|---|---|
| IAM | Microsoft Entra ID |
| IAM User | Entra User |
| IAM Policy/Role model | Entra RBAC/Directory Roles |
| S3 | Azure Blob Storage |
| S3 Bucket | Blob Container |
| S3 Object | Blob |
| EFS / FSx | Azure Files / other Azure file services |

> These mappings are conceptual. AWS IAM and Microsoft Entra ID use different identity and authorization models.

---

# 🔒 Security Practices

- Administrator passwords are not stored in GitHub.
- Storage Account access keys are not stored in the repository.
- Public/anonymous Blob access was not required for this lab.
- High-privilege administrator accounts should be used only when necessary.
- Access keys should be rotated if exposed.

---

# 📧 Task 3 — Microsoft 365 Platform & Licensing

## 🎯 Objective

Study the **Microsoft 365 (M365)** platform and understand the differences between **Business Basic, Business Standard, and Enterprise** licensing.

This task was completed through research of Microsoft's official product and licensing documentation. No paid Microsoft 365 subscription was required for this study.

---

# ☁️ What is Microsoft 365?

**Microsoft 365** is Microsoft's cloud-based productivity, collaboration, communication, security, and management platform.

It brings together services and applications such as:

```text
Microsoft 365
│
├── Word
├── Excel
├── PowerPoint
├── Outlook
├── Teams
├── OneDrive
├── SharePoint
├── Exchange Online
├── Microsoft Entra ID
├── Security
└── Compliance
```

Organizations choose different Microsoft 365 plans depending on their size, productivity requirements, security needs, and management requirements.

---

# 1️⃣ Microsoft 365 Business Basic

### Overview

**Business Basic** is designed mainly for small and medium-sized businesses that need cloud-based productivity and collaboration services.

### Key Features

* Web and mobile versions of Word, Excel, PowerPoint and Outlook
* Business email through Exchange Online
* Microsoft Teams
* OneDrive cloud storage
* SharePoint
* Planner
* Forms
* Bookings
* Up to **300 users** for the Business plan family

### Important Limitation

Business Basic **does not include the full desktop Office applications**.

Therefore, users primarily work through web/mobile applications when using this plan.

### Best suited for

Small businesses where employees mainly need:

```text
Email
+
Teams
+
Cloud Storage
+
Web Office Apps
```

---

# 2️⃣ Microsoft 365 Business Standard

### Overview

Business Standard provides the cloud services of Business Basic while also including the **desktop Office applications**.

### Key Features

* Everything provided by Business Basic
* Desktop Word
* Desktop Excel
* Desktop PowerPoint
* Desktop Outlook
* Web and mobile Office apps
* Exchange Online
* Microsoft Teams
* OneDrive
* SharePoint
* Up to **300 users** for the Business plan family

### Best suited for

Businesses where employees need to install and use Office applications directly on their Windows or Mac computers.

Example:

```text
Employee Laptop
│
├── Outlook Desktop
├── Word Desktop
├── Excel Desktop
└── PowerPoint Desktop
```

---

# 3️⃣ Microsoft 365 Enterprise

Microsoft 365 Enterprise plans are designed for larger organizations and environments requiring broader productivity, security, identity, compliance, and management capabilities.

The main information-worker enterprise plans include:

* **Microsoft 365 E3**
* **Microsoft 365 E5**

Enterprise licensing is not simply a larger version of Business Standard. It provides capabilities aimed at larger and more complex organizational requirements.

---

## 🔹 Microsoft 365 E3

### Overview

Microsoft 365 E3 provides enterprise productivity capabilities along with Windows Enterprise, identity and access management, security, compliance, and device-management capabilities.

### Key Capabilities

* Desktop, web and mobile Office applications
* Exchange Online
* Microsoft Teams
* OneDrive
* SharePoint
* Windows Enterprise
* Microsoft Entra ID capabilities
* Microsoft Intune capabilities
* Enterprise security and compliance features

### Best suited for

Large organizations that need:

```text
Productivity
+
Enterprise Identity
+
Device Management
+
Security
+
Compliance
```

---

## 🔹 Microsoft 365 E5

### Overview

Microsoft 365 E5 builds on the enterprise capabilities of E3 and adds more advanced security, compliance, analytics, and voice-related capabilities.

### Key Capabilities

* Everything broadly provided by E3
* Advanced security capabilities
* Microsoft Defender capabilities
* Advanced Microsoft Purview capabilities
* Microsoft Entra ID advanced capabilities
* Power BI Pro
* Advanced compliance and information protection capabilities
* Advanced analytics and security features

### Best suited for

Organizations with strong requirements around:

```text
Advanced Security
+
Threat Protection
+
Compliance
+
Data Protection
+
Analytics
```

---

# 📊 Business Basic vs Business Standard vs Enterprise

| Feature / Capability          | Business Basic | Business Standard | Microsoft 365 E3  | Microsoft 365 E5  |
| ----------------------------- | -------------- | ----------------- | ----------------- | ----------------- |
| Target organization           | Small / Medium | Small / Medium    | Enterprise        | Enterprise        |
| Business-plan user limit      | Up to 300      | Up to 300         | Enterprise plan   | Enterprise plan   |
| Word / Excel / PowerPoint Web | ✅              | ✅                 | ✅                 | ✅                 |
| Mobile Office apps            | ✅              | ✅                 | ✅                 | ✅                 |
| Desktop Office apps           | ❌              | ✅                 | ✅                 | ✅                 |
| Business Email                | ✅              | ✅                 | ✅                 | ✅                 |
| Microsoft Teams               | ✅              | ✅                 | ✅                 | ✅                 |
| OneDrive                      | ✅              | ✅                 | ✅                 | ✅                 |
| SharePoint                    | ✅              | ✅                 | ✅                 | ✅                 |
| Windows Enterprise            | ❌              | ❌                 | ✅                 | ✅                 |
| Enterprise device management  | Limited        | Limited           | ✅                 | ✅                 |
| Advanced security             | Limited        | Limited           | More capabilities | **Advanced**      |
| Advanced compliance           | Limited        | Limited           | ✅                 | **More advanced** |
| Power BI Pro                  | ❌              | ❌                 | ❌*                | ✅                 |
| Advanced analytics            | Limited        | Limited           | More              | **Advanced**      |

> **Note:** Microsoft 365 features, limits, and licensing entitlements can change over time and may vary by market or licensing arrangement. Always verify the current Microsoft documentation before making a purchasing decision.

---

# 🧩 Simple Difference

The easiest way to remember the plans:

```text
Business Basic
       │
       └── Cloud/Web productivity

Business Standard
       │
       └── Business Basic
             +
           Desktop Office Apps

Microsoft 365 E3
       │
       └── Enterprise productivity
             +
           Windows
             +
           Identity
             +
           Device Management
             +
           Security & Compliance

Microsoft 365 E5
       │
       └── E3
             +
           Advanced Security
             +
           Advanced Compliance
             +
           Advanced Analytics
```

---

# 🏢 When Would an Organization Choose Each Plan?

### Scenario 1 — Small Startup

A 20-person startup needs:

* Business email
* Teams
* OneDrive
* Web versions of Office

**Suitable choice:** Business Basic

---

### Scenario 2 — Growing Business

A company has employees who regularly use:

* Excel desktop
* Word desktop
* Outlook desktop
* PowerPoint desktop
* Teams
* OneDrive

**Suitable choice:** Business Standard

---

### Scenario 3 — Large Enterprise

A large organization needs:

* Office applications
* Windows Enterprise
* Centralized device management
* Enterprise identity
* Security
* Compliance

**Suitable choice:** Microsoft 365 E3

---

### Scenario 4 — Security-Focused Enterprise

A highly regulated or security-focused organization needs:

* Advanced threat protection
* Data protection
* Advanced compliance
* Security analytics
* Advanced identity capabilities

**Suitable choice:** Microsoft 365 E5

---

# 🔐 Business vs Enterprise — Key Difference

The biggest difference is **not simply the number of users**.

Business plans are primarily designed for **small and medium-sized organizations**, while Enterprise plans are designed for organizations with more advanced requirements around:

```text
Identity
Security
Device Management
Compliance
Data Protection
Enterprise Administration
```

Business plans have a **300-user limit**, while Enterprise licensing is designed for larger organizational deployments.

---

# 🧠 Key Learnings

Through this task, I learned that:

1. Microsoft 365 is more than just Word, Excel and PowerPoint.
2. It combines productivity, communication, cloud storage, collaboration, identity, security and compliance services.
3. Business Basic focuses on web/mobile productivity and cloud services.
4. Business Standard adds desktop Office applications.
5. Microsoft 365 Enterprise plans are designed for larger and more complex organizations.
6. E3 provides broad enterprise productivity, security, Windows and management capabilities.
7. E5 adds more advanced security, compliance and analytics capabilities.
8. The correct license depends on business requirements rather than simply choosing the most expensive plan.

---

# 📚 Official Microsoft References

* [Microsoft 365 Business Plans](https://www.microsoft.com/en-in/microsoft-365/business/compare-all-microsoft-365-business-products-b)
* [Microsoft 365 Enterprise](https://www.microsoft.com/en-in/microsoft-365/enterprise)
* [Microsoft 365 Plan Options — Microsoft Learn](https://learn.microsoft.com/en-us/office365/servicedescriptions/office-365-platform-service-description/office-365-plan-options)
* [Microsoft 365 Enterprise Licensing](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)

---

## ✅ Task 3 Status

**Completed — Microsoft 365 platform and Business Basic, Business Standard, E3 and E5 licensing studied and documented.**

---

## 📋 Completion Status

```text
Task 1 — Microsoft Entra ID + Users + Roles + MFA     ✅
Task 2 — Blob Storage + Windows Drive Mapping          ✅
Task 3 — Microsoft 365 Licensing                      ✅
```

---

