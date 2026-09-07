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