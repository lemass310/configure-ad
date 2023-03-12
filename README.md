<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying On-premises Active Directory in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

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
<img src="https://i.imgur.com/LZQN7mV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Resources Setup: 

Create the Domain Controller VM (Windows Server 2022) named “DC-1”

Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created for DC-1
</p>
<br />

<p>
<img src="https://i.imgur.com/upkLVBc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Resources Setup Continued: 

Set (DC-1)Domain Controller’s NIC Private IP address to be static. 
</p>
<br />

<p>
<img src="https://i.imgur.com/Qiq841l.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensuring Connectivity between client and active directory:

Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
</p>
<br />

<p>
<img src="https://i.imgur.com/UlOPMmw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensuring Connectivity Continued:

Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall

Check back at Client-1 to see the ping succeed
</p>
<br />

<p>
<img src="https://i.imgur.com/2qbDSwJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Install Active Directory:

Login to DC-1 and install Active Directory Domain Services
</p>
<br />

<p>
<img src="https://i.imgur.com/PhKFJcZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Promote as a DC: Setup a new forest as activedirectory.com (can be anything, just remember what it is)
</p>
<br />

<p>
<img src="https://i.imgur.com/V8pDQUX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Restart and then log back into DC-1 as user: activedirectory.com\labuser

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

Create a new OU named “_ADMINS”

Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
</p>
<br />

<p>
<img src="https://i.imgur.com/pMOJSH0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Add jane_admin to the “Domain Admins” Security Group

Log out/close the Remote Desktop connection to DC-1 and log back in as “activedirectory.com\jane_admin”

User jane_admin as your admin account from now on.
</p>
<br />

<p>
<img src="https://i.imgur.com/rMedkWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Join Client-1 to your domain (mydomain.com):

From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

From the Azure Portal, restart Client-1
</p>
<br />

<p>
<img src="https://i.imgur.com/Q31jBVO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Join Client-1 to your domain Continued:
  
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)

  Right click start menu -> Go to system -> Rename this PC (advanced)
</p>
<br />

<p>
<img src="https://i.imgur.com/k7zDFUo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setup Remote Desktop for non-administrative users on Client-1:

Log into Client-1 as mydomain.com\jane_admin and open system properties

Click “Remote Desktop” then allow “domain users” access to remote desktop

You can now log into Client-1 as a normal, non-administrative user now
</p>
<br />

<p>
<img src="https://i.imgur.com/zZ3qKx1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Join Client-1 to your domain Continued:

Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br />

<p>
<img src="https://i.imgur.com/LzcLOQ3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 as jane_admin

Open PowerShell_ise as an administrator

Create a new File and paste the contents of the script to the right into it

Observe the the accounts being created in the "_EMPLOYEES" folder we created in ADUC
</p>
<br />

<p>
<img src="https://i.imgur.com/Hv1cglk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Select any random account and then attempt to log into Client-1 using their username. 
  
That's it, you have successfully set up an Active Directory system 
</p>
<br />
