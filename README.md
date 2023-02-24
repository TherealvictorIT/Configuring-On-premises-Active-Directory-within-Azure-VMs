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


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
During this lab, we will set up two virtual machines (VMs) within the same virtual network (VNET). One VM will function as a Domain Controller (DC), while the other will serve as a client machine. To ensure seamless communication between the two, we will configure the DC with a static IP address, as it will be responsible for offering Active Directory services to the client machine. Additionally, we will join the client machine to the domain and configure its DNS settings to use the DC as its primary DNS server.


<h3>Ensure Connectivity between Client and Domain Controller</h3>
<p>

<img src="https://i.imgur.com/3KuMYbX.png" height="50%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
To verify connectivity, Client One will be connecting to DC-1 by attempting to ping it. Initially, the ping may fail due to the firewall on DC-1 blocking ICMPv4. 
</p>
<p>
<img src="https://i.imgur.com/248l5zB.png" height="70%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To resolve this issue, we will need to enable ICMPv4 on the DC-1 firewall. 
<p>
<img src="https://i.imgur.com/ofifnq1.png" height="50%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
After enabling ICMPv4, we can successfully ping DC-1 from Client-1.


<h3>Installing Active Directory </h3>

<p>
Our next step will be to log back into DC-1 and install AD Users & Computers. Once this is done, we will promote the VM to a DC and set up a new forest named "mydomain.com". After completing these steps, we will restart the system and log back in as user "mydomain.com\labuser".
<p>
Once that is finalized we need to proceed with creating Organizational Units (OU) for our domain. To start, we will create an OU named "_EMPLOYEES1" and another OU named "_ADMINS". We can do this by right-clicking on the domain area, selecting "New" > "Organizational Unit", and filling out the necessary fields.
</p>
<img src="https://i.imgur.com/7O3Nn84.png" height="60%" width="70%" alt="Disk Sanitization Steps"/>
<br />
</p>
After creating the OUs, we can then create a new user account named "Jane Doe". Since Jane will be an Admin, we will name her account "Jane_admin". Finally, we will add Jane to the "Domain Admins" security group by selecting the group and adding Jane as a member.


<h3>Join Client-1 to your domain (mydomain.com)</h3>

Going forward, we will be using the Jane_admin account as our administrator account.
</p>
<img src="https://i.imgur.com/yelzvNL.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
</p>
Our next step is to join Client-1 to the domain "mydomain.com". To accomplish this, we will change Client-1's DNS settings in the Azure portal to the DC's Private IP address. 
</p>
<img src="https://i.imgur.com/XYgpi0I.png" height="60%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
After this is done, we will restart Client-1 from within the Azure portal. We can then verify that Client-1 is properly connected to DC-1's DNS as shown in the picture above.
<p>
<img src="https://i.imgur.com/lqLDj5F.png" height="70%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, to join Client-1 to the domain, we need to navigate to the system settings and select "About". On the right-hand side, select "Rename this PC (Advanced)" and choose to change the domain. Next, we will enter "mydomain.com" and provide the credentials from "mydomain.com\labuser". After the computer restarts, Client-1 will successfully be a part of "mydomain.com".

  
<h3>Setup Remote Desktop for non-administrative users on Client-1</h3> 


Wonderufl Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
