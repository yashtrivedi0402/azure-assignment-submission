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
| **Task 3** | Microsoft 365 Licensing | ⏳ To be added |

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


---

## 📋 Completion Status

```text
Task 1 — Microsoft Entra ID + Users + Roles + MFA     ✅
Task 2 — Blob Storage + Windows Drive Mapping          ✅
Task 3 — Microsoft 365 Licensing                      ⏳
```

---

> **Next:** Task 3 — Microsoft 365 Business Basic, Business Standard and Enterprise licensing.
