# Task 3: Active Directory Domain Services & Domain Join

## 📌 Objective

Create an Active Directory Domain Services (AD DS) environment on a Windows Server and join a Windows client machine to the domain.

### Tasks Performed

1. Configured a Windows Server as an Active Directory Domain Controller.
2. Created an Active Directory domain named `yash.local`.
3. Created a domain user `yashadmin`.
4. Created a separate Windows client VM named `client-1`.
5. Configured the client to use the Domain Controller as its DNS server.
6. Joined `client-1` to the `yash.local` domain.
7. Verified domain membership and authenticated using the domain account.

---

# 🏗️ Architecture

```text
                    Azure
                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
     Windows Server       Windows 11 Client
   windows-client-1          client-1
             │                 │
             │                 │
             ▼                 │
      Active Directory         │
        Domain Controller      │
             │                 │
             │ DNS             │
             └─────────────────┘
                    │
                    ▼
                 yash.local
```

---

# 🖥️ Domain Controller Configuration

### Server

* VM Name: `windows-client-1`
* OS: Windows Server 2022
* Role: Active Directory Domain Controller
* Domain: `yash.local`

### Install AD DS

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

### Create the Active Directory Forest

```powershell
Install-ADDSForest -DomainName "yash.local"
```

The server was restarted after the AD DS installation and forest creation.

---

# 👤 Domain User Creation

A dedicated domain user was created for authentication:

```powershell
New-ADUser -Name "Yash Admin" `
-SamAccountName "yashadmin" `
-UserPrincipalName "yashadmin@yash.local" `
-AccountPassword (Read-Host "Enter Password" -AsSecureString) `
-Enabled $true
```

The user was then used to authenticate against the `yash.local` domain.

> Passwords and credentials are intentionally not included in this documentation.

---

# 💻 Client VM Configuration

### Client

* VM Name: `client-1`
* OS: Windows 11 Enterprise
* Role: Domain Client
* Domain: `yash.local`

The client VM was deployed in the same Azure network as the Domain Controller.

---

# 🌐 DNS Configuration

Active Directory depends heavily on DNS for locating domain services.

The client's DNS server was configured to use the private IP address of the Domain Controller.

```text
Client DNS
    │
    ▼
Domain Controller Private IP
    │
    ▼
yash.local
```

DNS resolution was verified using:

```powershell
nslookup yash.local
```

The domain successfully resolved to the Domain Controller.

---

# 🔗 Joining the Client to the Domain

The client was joined to the Active Directory domain:

```powershell
Add-Computer -DomainName "yash.local" -Credential "yashadmin@yash.local"
```

The client was restarted after joining the domain.

---

# 🔐 Domain Authentication

After the domain join, the client was accessed using:

```text
yashadmin@yash.local
```

The domain account was granted Remote Desktop access on the client to allow RDP authentication.

---

# ✅ Verification

## Client-side verification

### Check logged-in domain user

```powershell
whoami
```

Expected:

```text
yash\yashadmin
```

### Check domain membership

```powershell
(Get-CimInstance Win32_ComputerSystem) |
Select-Object Name,Domain,PartOfDomain
```

Expected:

```text
Name       Domain       PartOfDomain
----       ------       ------------
client-1   yash.local   True
```

---

# 🏢 Domain Controller Verification

### Check the domain

```powershell
Get-ADDomain
```

Expected domain:

```text
yash.local
```

### Verify the client computer exists in Active Directory

```powershell
Get-ADComputer -Identity "client-1"
```

The `client-1` computer object should be returned from Active Directory.

### Verify the domain user

```powershell
Get-ADUser -Identity "yashadmin" |
Select Name,SamAccountName,Enabled
```

Expected:

```text
Name        SamAccountName    Enabled
----        --------------     -------
Yash Admin  yashadmin         True
```

---

# 📸 Screenshot Evidence

<img width="1906" height="1003" alt="Screenshot 2026-09-08 034350" src="https://github.com/user-attachments/assets/67fdafbc-2b8e-4e01-b35e-26fbc4ffb762" />

<img width="553" height="561" alt="Screenshot 2026-09-08 034509" src="https://github.com/user-attachments/assets/ceda9626-a9a9-4472-97ad-e114e066a32b" />

<img width="1887" height="991" alt="Screenshot 2026-09-08 035658" src="https://github.com/user-attachments/assets/84aeb097-a601-4b96-8eca-6e7d80cd9dd6" />

<img width="1912" height="697" alt="Screenshot 2026-09-08 035930" src="https://github.com/user-attachments/assets/2dc29ccf-fad9-4a5e-8edd-a98de50237a0" />

---

# 🔄 Final Architecture

```text
                 Azure Virtual Network
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
     Windows Server             Windows 11
   windows-client-1              client-1
             │                       │
             │                       │
       AD DS / DNS ◄─────────────────┘
             │
             ▼
         yash.local
             │
             ├── Domain User
             │      └── yashadmin
             │
             └── Computer
                    └── client-1
```

---

# 🎯 What I Learned

* Active Directory Domain Services (AD DS)
* Domain Controller configuration
* Creating an Active Directory forest
* Creating domain users
* Active Directory DNS dependency
* Windows domain joining
* Domain authentication
* RDP authentication using domain credentials
* Computer objects and user objects in Active Directory
* Basic enterprise identity and access management
* Troubleshooting DNS, RDP, firewall and domain connectivity

---

# 💡 Real-World Use Cases

Active Directory is commonly used in enterprise environments to provide:

* Centralized user authentication
* Centralized computer management
* Role-based access control
* Group Policy management
* Password and security policies
* Centralized access to internal applications
* Windows workstation/server management
* Integration with cloud identity platforms

Microsoft's AD DS learning path also covers Group Policy, organizational units, FSMO roles, trusts, replication and hybrid identity, which are natural next steps after this lab.

---

# 🏁 Task Status

**Task 3 — Completed ✅**

A Windows Server was configured as an Active Directory Domain Controller, the `yash.local` domain was created, and the Windows client `client-1` was successfully joined and authenticated against the domain.
