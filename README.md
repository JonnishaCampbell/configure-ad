<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
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

- Setup Resources in Azure
- Ensure Connectivity between Client and Domain Controller Virtual Machine
- Install Active Directory
- Create an Admin and Normal User Account in Active Directory
- Join Client-1 to your Domain
- Setup Remote Desktop for non-administrative users on Client-1 Virtual Machine
- Create additional users
- Attempt to log into Client-1 with one of the users created

<h2>Deployment and Configuration Steps</h2>

1. Setup Resources in Azure
	- Create a Domain Controller VM, Windows Server 2022, and name it DC-1
	- Set the Domain Controller's NIC private IP address to be static
	- Create a Client VM, Windows 10, and name it Client-1
		- Use the same resource group and Vnet created in the previous step
	- Go to Network Watcher and check topology (making sure that the Resource group and Virtual machine are running on the same virtual network)

<p>
<img src="https://i.imgur.com/DEU9x0j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/AtDHufO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

2. Ensure Connectivity between the client and domain controller VM
	- Use remote desktop to log into Client-1 and ping DC-1 private IP address (ping -t IP address)
	- Log into DC-1 and enable ICMPv4 on windows local firewall
		- This allows DC-1 to accept ICMP traffic
	- Go back to the Client-1 command prompt to see if the ping is successful

<p>
<img src="https://i.imgur.com/6XNwLdQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

3. Install Active Directory
	- Install Active Directory Domain Services in DC-1
	- Promote it as a domain controller and set up a new forest as mydomain.com or anything else. I'll be using jcdomain.com
	- Restart the machine and log into DC-1 as user: mydomain.com\labuser or whatever you created. I'll log in as jcdomain.com\labuser

<p>
<img src="https://i.imgur.com/aWqaNA9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/cBBeVVg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

4. Create an Admin and Normal User Account in AD
	- Go to Active Directory Users and Computers to create an Organization Unit called "_Employees."
	- Create another OU called "_Admins."
	- Within _Admins, create an employee name Jane Doe with the username of jane_admin
	- Go to the security group and add jane_admin to Domain Admins
	- Log out and close the RDP connection to DC-1
	- Log back into DC-1 as mydomain.com\jane_admin. My login will be jcdomain.com\jane_admin

<p>
<img src="https://i.imgur.com/JbPXPqJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

5. Join Client-1 in your domain
	- Go to the Azure portal (not in vm) and set Client-1's DNS setting to DC's private IP address
	- Restart Client-1's vm and log back into as labuser
	- Go to DC-1 and verify that Client-1 shows up in ADUC in the computers container on the root of the domain
	- Create another OU named _Clients and drag Client-1 in there

<p>
<img src="https://i.imgur.com/Ko1xuOQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/P3QVB73.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/dFwacBU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

6. Setup Remote Desktop for non-administrative users on client-1
	- Log into Client-1 as mydomain.com/jane_admin
	- Go to system properties => Remote desktop => allow "domain users" access to remote desktop
	- This will allow you to log into Client-1 as a non-administrator user
	- Note to change many systems, you would use this with a Group Policy

<p>
<img src="https://i.imgur.com/NJATXAr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

7. Create additional users and attempt to log into client-1 with one of the users
	- If not already, log into DC-1 as jane-admin
	- Open PowerShell_ise as an administrator
	- Create a new file and paste the script's contents into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
		- This script was created by instructor for practice
		- It includes hundreds of generated names, creating users
	- Run the script and watch the accounts being created
	- Go to ADUC and watch the accounts in the appropriate OU
	- Log into Client-1 with one of the accounts created
		- The password will be in the script
		- I'll be remotely logging in as the user lenur.nibo instead of admin
		- Go to Windows PowerShell or command prompt, if you want to verify
<p>
<img src="https://i.imgur.com/NPUBnDF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/7mySEDk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/HzilkBY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/r0JqiVv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<br />

