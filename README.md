<h1>Active Directory (Oracle VirtualBox) | Add Users w/PowerShell</h1>

<h2>Description</h2>
This is a walkthrough of how I created A Virtual Machine environment using Oracle VM Virtualbox running Microsft Windows 11 and Microsoft Server 2022. I did this project to gain experience with Active Directory and PowerShell to deepen my uderstanding on how organizations utilize these tools to create and manage user accounts and control access to resources on corporate networks. This project will showcase the steps I took to create a corporate domain with Windows Server 2022 utilizing Oracle VM Virtualbox to configure an internal netowrk with DHCP, create over 1,000 users with credentials with PowerShell, and manage user groups and security policies. It will also diplay the steps taken to create a Client using Windows 11, add the Client to the domain, and login to the Client using created credentials to test the functionality of the domain. 
<br/>


<h2>Key Learnings</h2>

- Hands-on experience with Active Directory concepts and administration.
- Understanding of virtual machine setup and networking in VirtualBox.
- Enhanced PowerShell skills for managing Active Directory objects.
- Troubleshooting and problem-solving in a lab environment.
<br/>


<h2>Utilities Used</h2>

- <b>Active Directory</b>
- <b>PowerShell</b>
- <b>CMD</b>


<h2>Environments Used </h2>

- <b>Oracle VM Virtualbox</b> 
- <b>Windows 11</b>
- <b>Windows Server 2022</b>

<h2>Links</h2>

- <b>Oracle VM Virtualbox:</b> https://www.virtualbox.org/
- <b>Windows Server 2022:</b> https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
- <b>Windows 11 ISO:</b> https://www.microsoft.com/software-download/windows11


<h2>Program walk-through:</h2>

<p align="center">
Oracle VM VirtualBox is the Virtual Machine (VM) that I will download, configure, and manage this lab. I also downloaded the VirtualBox Extension Pack to support the VM. <br/>
<img src="https://imgur.com/OeSOJfK.png" height="80%" width="80%" alt="Nessus Essentials"/> 
<br />
<br />
Downloaded Windows Server 2022 ISO and installed it to the VM. The reason for the server is to handle the organizational IT administrative related activities including user and device management, enterprise software file and storage management, etc.  <br/>
<img src="https://imgur.com/y7NeEIw.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
As a requirement for the virtual machine, I must install an Operating System. I chose to install Windows 11. <br/>
<img src="https://imgur.com/mQbQbGG.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br /> 
Once the VM is created and Windows 2022 is installed, the next step is to configure the IP settings. For this an internal NIC was created separate from the public facing internet (my home router). The IP for the internal NIC was set as 172.16.0.1, subnet mask of 255.255.255.0, and a DNS as 127.0.0.1.  <br/>
<img src="https://imgur.com/r4Wt8FY.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
Now that the network is configured, I then installed Active Directory Domain Services (AD DS) and created a domain. Using the Server Manager, I selected the server in which I was going to install AD. The Fully Qualified Domain Name (FQDN) was set to ‘mydomain.com'.  <br/>
<img src="https://imgur.com/BOOwk3y.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/ELJmJHc.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
Next I created a dedicated domain admin account. This was done by creating an Organizational Unit (OU), which I named ‘Admin’, then creating a user to add to the Admin OU. For the purpose of this lab, the password policy was set to Password never expires. Once the user was successfully created, it was then added to the Domain Admins group. Finally, to ensure that the user was successfully created I logged into windows using credentials of the newly created user. <br/>
<img src="https://imgur.com/AQGZJn4.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/I4vWPQB.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/0P3qeaA.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/rg1M70h.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/37uVYKg.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/1Svj0cg.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
To allow the soon to be created Client to have access to be on the private virtual network and still be able to access the internet through the domain controller I installed the Remote Access Server (RAS) and Network Address Translation (NAT) through the Server Manager. The steps were as followed: Select Remote Access, add routing, selected Network Address Translation (NAT), and selected to use the previously configured IP addresses. <br/>
<img src="https://imgur.com/VF5Pk2R.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/laLmN9a.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/kmzXbSH.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/NcYvzoM.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
The next step is to Setup DHCP Server on the domain controller which will allow computers/ client computers on the network to automatically get their IP Address on the internal network. The steps were as followed: select DHCP Server, set a new scope which included an IP range of 172.16.0.100 – 172.16.0.200, a length of 24, and a subnet mask of 255.255.255.0. A Lease Duration was set to 8 days. The new scope was then successfully created. <br/>
<img src="https://imgur.com/etcPtVl.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/FBFBJw4.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/AU1zc9H.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/VpMgj1X.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/ZW8DpVZ.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
A PowerShell Script was used to create 1,052 users to add to Active Directory. A PowerShell script was created to set the password for all 1,052 users to ‘Password1’ and retrieve the list of users from a .txt file that was created. Furthermore, a loop script was used to format the first name, last name, and username for each user. To bypass the security feature in PowerShell I had to unrestrict the security policy. The script was then run to add the users to Active Directory. <br/>
<img src="https://imgur.com/PuoRSJC.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/Pt4vuBy.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/Ky3j4Bo.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
To verify that all users were added to Active Directory, I opened the created ‘_USERS’ Organizational Unit. All users were successfully added. <br/>
<img src="https://imgur.com/Z5HDcJT.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
A separate VM was created and configured for the Client in which Windows 11 Pro was installed. This VM will be used to connect to the domain. During the installation of Windows 11, there was an error in need of troubleshooting. To troubleshoot this error, I opened the Registry Editor from the command line window, created a new key and named it ‘LabConfig’ for the local machine then create four new DWORD (32-bit) Values; Bypass BypassTPMCheck, BypassCPUCheck, BypassRAMCheck, and BypassSecureBootCheck. The troubleshoot was successful, and I was then able to finish installing Windows 11 to the VM.  <br/>
<img src="https://imgur.com/XvcbwJc.png" height="80%" width="80%" alt="Nessus Essentials"/> 
<img src="https://imgur.com/opAuQ9m.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/T19k2lR.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/71K3NRo.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/mh8Vzvp.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/dUB6nN4.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/5ZNxwzS.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
Next, I verified that the IP configuration is correct and I’m able to ping something on the internet to verify that DNS Server is working. I was able to successfully ping both, www.google.com and also the domain I created. <br/>
<img src="https://imgur.com/2XlTiEX.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/z9yekyf.png" height="80%" width="80%" alt="Nessus Essentials"/> 
<img src="https://imgur.com/v4s2lXG.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br/>
<br/>
Next, I changed the default hostname from DESKTOP-LTUV7 to Client1 and added Client1 to the domain (mydomain.com). <br/>
<img src="https://imgur.com/2i1sl46.png" height="80%" width="80%" alt="Nessus Essentials"/> 
<img src="https://imgur.com/qtXqGVO.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
I then logged back into the Server Manager and verified that the client computer (Client1) that I created was added to the domain and I’m able to login to the client computer with a created account. Using the Windows 11 VM (Client 1), I then attempted to login with the created user ‘astuart’ using the created credentials. The login attempt was successful. <br />
<img src="https://imgur.com/CNt6ht1.png" height="80%" width="80%" alt="Nessus Essentials"/>
<img src="https://imgur.com/iFsRsgl.png" height="80%" width="80%" alt="Nessus Essentials"/> 
<img src="https://imgur.com/ZopQzGu.png" height="80%" width="80%" alt="Nessus Essentials"/>
<br />
<br />
</p>
