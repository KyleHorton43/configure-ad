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

- Preparing the AD Infrastructure
- Installing and Configuring Active Directory
- Joining the Client Machine to the Domain
- Managing Users and Remote Access

<h1>Phase 1: Preparing the AD Infrastructure</h1>
<h2>1. Setting Up the Domain Controller in Azure</h2>

<p>
Before setting up Active Directory, we need to create a Domain Controller (DC).

1. Create a Resource Group in Azure.

2. Set up a Virtual Network (VNet) with a dedicated subnet.

3. Create a Virtual Machine (VM) for the Domain Controller:

   -  Name: DC-1

    - OS: Windows Server 2022

    - Username: labuser

    - Password: Cyberlab123!

4. Set the NIC Private IP to Static for DC-1.

5. Disable the Windows Firewall (for testing connectivity).
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>
<br />

<h2>2. Setting Up the Client Machine in Azure</h2>

<p>
Once the Domain Controller is ready, the next step is configuring a Client machine.

1. Create a Windows 10 VM:

    - Name: Client-1

    - Username: labuser

    - Password: Cyberlab123!

2. Attach Client-1 to the same Virtual Network as DC-1.

3. Set Client-1’s DNS settings to DC-1’s Private IP.

4. Restart the Client-1 VM.

5. Test connectivity:

    - Open PowerShell on Client-1 and run: ping DC-1's_Private_IP
    - Run ipconfig /all and confirm DNS settings point to DC-1.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>
<br />

<h1>Phase 2: Installing and Configuring Active Directory</h1>
<h2>1. Install Active Directory Domain Services (AD DS)</h2>

<p>
1. Log in to DC-1.

2. Install Active Directory via Server Manager.

3. Promote DC-1 to a Domain Controller:

    - Set up a new forest: mydomain.com.

4. Restart and log back into DC-1 as mydomain.com\labuser.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>
<br />

<h2>12. Creating a Domain Admin Account</h2>

<p>
1. Open Active Directory Users and Computers (ADUC).

2. Create an Organizational Unit (OU) named _EMPLOYEES.

3. Create another OU named _ADMINS.

4. Add a new Admin User:

    - Name: Jane Doe

    - Username: jane_admin

    - Password: Cyberlab123!

    - Add jane_admin to the Domain Admins group.

5. Log out of DC-1 and log back in as mydomain.com\jane_admin.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>
<br />

<h1>Phase 3: Joining the Client Machine to the Domain</h1>

<p>
1. Ensure Client-1’s DNS settings are pointing to DC-1.

2. Restart Client-1.

3. Join Client-1 to the domain (mydomain.com):

    - Log in as labuser (local admin).

    - Navigate to System Settings > About > Domain & Workgroup.

    - Select Join a Domain and enter mydomain.com.

    - Restart Client-1.

4. Verify in ADUC that Client-1 appears under the _CLIENTS OU.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>
<br />

<h1>Phase 4: Managing Users and Remote Access</h1>
<h2>1. Enabling Remote Desktop for Domain Users</h2>

<p>
1. Log into Client-1 as mydomain.com\jane_admin.

2. Open System Properties > Select Remote Desktop.

3. Allow Domain Users to connect via Remote Desktop.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>
<br />

<h2>2. Bulk User Creation & Testing Logins</h2>

<p>
1. Log into DC-1 as jane_admin.

2. Open PowerShell_ise as Administrator.

3. Run a PowerShell script to create multiple users:
   
      for ($i=1; $i -le 10; $i++) {
          $password = ConvertTo-SecureString "Cyberlab123!" -AsPlainText -Force
          New-ADUser -Name "User$i" -SamAccountName "user$i" -UserPrincipalName "user$i@mydomain.com" `
          -Path "OU=_EMPLOYEES,DC=mydomain,DC=com" -AccountPassword $password -Enabled $true
      }

4. Confirm user creation in ADUC.

5. Attempt logging into Client-1 with one of the new users.

</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>

<h1>Conclusion</h1>

<p>
Active Directory Infrastructure has successfully been set up in Azure, including:
  
  ✅ Deploying a Domain Controller
  
  ✅ Configuring a Client machine
  
  ✅ Installing and configuring AD DS
  
  ✅ Creating users & managing domain access
  
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt=""/>
</p>






