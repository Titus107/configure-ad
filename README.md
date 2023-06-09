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
This will bring up a prompt where we will select "add a new forest" then give your domain a name. Click next and assign your DC a password as well. Keep clicking "next" through the tabs until you reach "Installation" where you can then properly install Active Directory. Now we need to restart the VM and then reconnect remotely again. Login with remote desktop control with your DC name.com\username and the user password.
</p>
<br />

<p>
<img src="https://i.imgur.com/R5VUAdJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Navigate to Active Directory Users and Computers (ADUC), and right-click to create an Organizational Unit (OU) called “_EMPLOYEES”. Then create a new OU called "_ADMINS". After this, both organizational groups should be empty. Open the _ADMINS OU and right-click to create a new user. Name the user "Jane Doe" and create a password for the user as well. From here on we will login to the DC with this account. Make sure to right-click on the user, go to ->member of ->add and type "domain admin" then click "check names". It will add the user as a Domain Administrator.  
</p>
<br />

<p>
<img src="https://i.imgur.com/KlGENaE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The next step is to add Client-1 to the domain. From Azure, set Client-1's DNS settings to the DC's private address. For this we can get DC's private IP address by navigating to DC VM in Azure. Go back to Client-1 and go to ->networking ->Network Interface ->DNS Servers and select "custom" then add DC's private IP address. Now restart Client-1 from Azure. Login to Client-1 again. 
  <p>
Right-click the windows key and select "system". From this menu select "Rename this PC (Advanced)". Click "change", "domain", and type in mydomain.com or whatever name you have for your domain. We must then restart the computer for the domain change to take place. Now we should be able to login to Client-1 with our DC Admin account. Go to system again then ->Remote Desktop ->select users that can remotely access this PC ->add and type "domain users" then click "check names". This will make it to where any account on our domain can login to this machine.  
</p>
<br />

<p>
<img src="https://i.imgur.com/gUSs8ws.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, we will create a bunch of users for our domain at all once. All of them should be able to login to any machine on our domain besides the DC. 
  <p>
    The first step is to open our Server VM and open Powershell ISE as an Administrator. We will create many users at once by copy/pasting the contents of the following script: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 - recommend changing the number of total users created to a smaller amount. Run the script. All of the accounts will be created in the _EMPLOYEES folder. 
    <p>
      Congratulations! We now have a Windows Server running Active Directory, learned how to add a machine to our domain, and created users for our domain. 
</p>
<br />
