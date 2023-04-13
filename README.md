# configure-ad<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. If we want to work in an environment where any user account in our system can use any computer to login, then we can accomplish this using Active Directory and joining computers to the same domain. This lab goes over a brief setup of Active Directory. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/hFuAmyY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First we are going to create two Virtual Machines within Microsoft Azure. The first VM will be a Windows server and the second VM will be running Windows 10. The Windows Server VM will act as our Domain Controller. It is important to setup both machines in the same region so that they can also be set to the same network. Also when creating the Windows 10 VM, set it to use the same virtual network as the Windows Server VM in "networking" settings. After having created both VMs, set the NIC of the DC to static so that the Client VM can be joined to the same domain. 
</p>
<br />

<p>
<img src="https://i.imgur.com/Fciigog.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we can ensure connectivity between the DC and Client VMs by logging into the DC and enabling ICMPv4 on the local Windows Firewall. Open "wf.msc" and go to "Inbound Rules" and enable both rules which include "Core Networking Diagnostics". Now if we remotely connect to our other VM we should be able to test our connectivity by pinging the DC with the ping command from the command line. 
</p>
<br />

<p>
<img src="https://i.imgur.com/VOae2AV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We move on to install Active Directory now on our DC. In the Server Manager click "add roles and features" and keep selecting "next" until you reach the tab labeled "Server Roles". Add "Active Directory Domain Services". Afterwards, click the prompt to install AD. After closing your current window, you can click on a flag icon which will bring up a prompt. From this, click on "promote this server to a Domain Controller". 
</p>
<br />

<p>
<img src="https://i.imgur.com/6Suwt00.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This will bring up a prompt where we will select "add a new forest" then give your domain a name. Click next and assign your DC a password as well. Keep clicking "next" through the tabs until you reach "Installation" where you can then properly install Active Directory. Now we need to restart the VM and then reconnect remotely again. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

