<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure (Creat CLient-1 & Domain Controller VM)
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of them

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Resources Setup: 

Create the Domain Controller VM (Windows Server 2022) named “DC-1”

Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created for DC-1
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Resources Setup Continued: 

Set (DC-1)Domain Controller’s NIC Private IP address to be static. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensuring Connectivity betweek client and active directory:

Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensuring Connectivity Continued:

Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall

Check back at Client-1 to see the ping succeed
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Install Active Directory

Login to DC-1 and install Active Directory Domain Services

Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Restart and then log back into DC-1 as user: mydomain.com\labuser

Create an Admin and Normal User Account in AD:

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

Create a new OU named “_ADMINS”

Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create an Admin and Normal User Account in AD Continued:

Add jane_admin to the “Domain Admins” Security Group

Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”

User jane_admin as your admin account from now on.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Join Client-1 to your domain (mydomain.com):

From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

From the Azure Portal, restart Client-1
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Join Client-1 to your domain Continued:
  
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Join Client-1 to your domain Continued:

Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

Create a new OU named “_CLIENTS” and drag Client-1 into there</p>
<br />

