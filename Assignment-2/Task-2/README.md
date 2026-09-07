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