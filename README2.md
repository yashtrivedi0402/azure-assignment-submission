# Assignment 2 – Task 1: Linux Web Servers on Azure

## Objective

Create two Linux virtual machines on Microsoft Azure, configure a web server on each VM, and host a unique webpage on each server instead of using only the default Nginx page.

## Architecture

```text
                    Microsoft Azure
                         |
                Resource Group
                         |
              +----------+----------+
              |                     |
        web-server-1           web-server-2
        Linux VM               Linux VM
              |                     |
            Nginx                 Nginx
              |                     |
      yash-vm1.html          yash-vm2.html
```

## VM 1 – `web-server-1`

### Configuration

* OS: Ubuntu Linux
* VM Name: `web-server-1`
* Web Server: Nginx
* Webpage: `yash-vm1.html`
* Authentication: SSH public key
* Inbound Ports: SSH (22), HTTP (80)

### Nginx Installation

```bash
sudo apt update
sudo apt install nginx -y
```

### Custom Webpage

The custom webpage was created at:

```text
/var/www/html/yash-vm1.html
```

The page contains VM-specific information to distinguish it from the second server.

### Verification

The Nginx default page was first verified to confirm that the web server was running. The custom webpage was then accessed through the VM's public IP.

## VM 2 – `web-server-2`

### Configuration

* OS: Ubuntu Linux
* VM Name: `web-server-2`
* Web Server: Nginx
* Webpage: `yash-vm2.html`
* Authentication: SSH public key
* Inbound Ports: SSH (22), HTTP (80)

### Nginx Installation

```bash
sudo apt update
sudo apt install nginx -y
```

### Custom Webpage

The custom webpage was created at:

```text
/var/www/html/yash-vm2.html
```

### Verification

The webpage was verified locally using:

```bash
curl http://localhost/yash-vm2.html
```

The custom HTML page was successfully returned, confirming that Nginx was serving the file correctly.

### Live Webpage

[Open Yash VM 2](http://20.44.52.73/yash-vm2.html)

## Key Commands Used

### Check Nginx Status

```bash
sudo systemctl status nginx
```

### Test Nginx Configuration

```bash
sudo nginx -t
```

### Restart Nginx

```bash
sudo systemctl restart nginx
```

### Check Web Root

```bash
ls -l /var/www/html/
```

### Test Custom Page

```bash
curl http://localhost/yash-vm2.html
```

## AWS Comparison

| Azure                  | AWS Equivalent      |
| ---------------------- | ------------------- |
| Azure Virtual Machine  | EC2                 |
| Azure VNet             | VPC                 |
| Azure Subnet           | VPC Subnet          |
| Network Security Group | Security Group      |
| Public IP              | Public/Elastic IP   |
| Ubuntu VM              | Ubuntu EC2 Instance |
| Nginx                  | Nginx on EC2        |

## What I Learned

* Creating Linux VMs in Azure.
* Selecting an appropriate VM size for a small web-server workload.
* Connecting to an Azure Linux VM using SSH and a `.pem` private key.
* Installing and managing Nginx on Ubuntu.
* Understanding how Nginx serves files from `/var/www/html/`.
* Allowing HTTP traffic through Azure NSG port 80.
* Hosting different webpages on multiple VMs.
* Verifying web-server functionality using `curl`.

# 📸 Screenshot Evidence

<img width="1917" height="572" alt="Screenshot 2026-09-08 020142" src="https://github.com/user-attachments/assets/47494a9e-9c05-4bc9-ac9d-5632816305cc" />

<img width="1912" height="626" alt="Screenshot 2026-09-08 013129" src="https://github.com/user-attachments/assets/9e284678-474c-49df-b98e-0aba2655e48c" />

<img width="1902" height="532" alt="Screenshot 2026-09-08 013235" src="https://github.com/user-attachments/assets/158974c5-1481-4f16-9ebe-936c5cddbe38" />

<img width="1602" height="383" alt="Screenshot 2026-09-08 015606" src="https://github.com/user-attachments/assets/79e1b5cf-3e29-4f88-b675-d6cf826f9e66" />

<img width="1857" height="963" alt="Screenshot 2026-09-08 015732" src="https://github.com/user-attachments/assets/5664d448-2da4-47a0-b38a-af65f7487b70" />


## Task 1 Status

**Completed ✅**

Two Linux Azure VMs were created, Nginx was configured on both, and unique webpages were successfully hosted and verified.

# Task 2 – Windows VM in a Different Region

## Objective

Create a Windows virtual machine in a different Azure region and access the webpage hosted on the Linux web server from the Windows VM.

---

## Windows VM Configuration

A Windows Server 2022 Datacenter: Azure Edition virtual machine was created in a different Azure region from the Linux web servers.

### Configuration

- **VM Name:** `windows-client-1`
- **Operating System:** Windows Server 2022 Datacenter: Azure Edition
- **Region:** Indonesia Central
- **VM Size:** `B2ats_v2`
- **Authentication:** Username and Password
- **Remote Access:** RDP (3389)
- **Webpage accessed:** `yash-vm2.html`
- **Linux Web Server:** `web-server-2`

---

## Accessing the Linux Webpage

After creating the Windows VM, I connected to it using Remote Desktop (RDP).

The Windows Server was running in a different Azure region from the Linux web server.

From the Windows Server PowerShell, I accessed the webpage hosted on the Linux VM using:

```powershell
curl http://20.44.52.73/yash-vm2.html -UseBasicParsing
```

The request successfully returned:

```text
StatusCode        : 200
StatusDescription : OK
```

The response content also contained the custom webpage hosted on `web-server-2`.

---

## Verification

The HTTP response confirmed successful communication between the Windows VM and the Linux web server.

```powershell
(Invoke-WebRequest http://20.44.52.73/yash-vm2.html -UseBasicParsing).StatusCode
```

Output:

```text
200
```

This confirms that the webpage hosted on the Linux VM was successfully accessed from the Windows VM.

---

## Architecture

```text
                Microsoft Azure
                      |
          +-----------+-----------+
          |                       |
     South India            Indonesia Central
          |                       |
   web-server-2            windows-client-1
    Linux VM                 Windows VM
          |                       |
        Nginx                     |
          |                       |
  yash-vm2.html <-----------------+
          |
       HTTP : 80
```

---

## Key Commands Used

**Access webpage from Windows PowerShell**

```powershell
curl http://20.44.52.73/yash-vm2.html -UseBasicParsing
```

**Verify HTTP status code**

```powershell
(Invoke-WebRequest http://20.44.52.73/yash-vm2.html -UseBasicParsing).StatusCode
```

Expected output:

```text
200
```

---

## What I Learned

- How to create a Windows Server VM in Azure.
- How to deploy a VM in a different Azure region.
- How to connect to a Windows Azure VM using RDP.
- How to use PowerShell to test HTTP connectivity.
- How a Windows VM can access a webpage hosted on a Linux VM.
- How to verify successful web communication using HTTP status codes.
- Understanding basic cross-region communication between Azure resources.

---

## 📸 Screenshot Evidence – Task 2

### 1. Windows VM Overview

*Add screenshot here.*

### 2. Windows VM Region

*Add screenshot showing the Windows VM deployed in a different region.*

### 3. RDP / Windows Server

*Add screenshot showing successful access to the Windows VM.*

### 4. PowerShell – Webpage Access

Add screenshot showing:

```powershell
curl http://20.44.52.73/yash-vm2.html -UseBasicParsing
```

with:

```text
StatusCode        : 200
StatusDescription : OK
```

### 5. HTTP Status Verification

Add screenshot showing:

<img width="766" height="590" alt="Screenshot 2026-09-08 022606" src="https://github.com/user-attachments/assets/1b67eafd-a7ee-469f-8284-c84e2f12d631" />

<img width="1871" height="667" alt="Screenshot 2026-09-08 022808" src="https://github.com/user-attachments/assets/e00d21e7-0445-49a8-88b9-09792a703100" />


---

## Task 2 Status

**Completed ✅**

A Windows Server 2022 VM was successfully created in a different Azure region, accessed through RDP, and used to successfully access the webpage hosted on the Linux web server. The webpage returned an HTTP `200 OK` response.

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
