<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Managing Accounts with Azure VMs</h1>
This tutorial outlines the Enabling, Unlocking of Accounts and how to Reset Passwords a Pre-made Windows Server Virtual Machine(Azure).<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Users and Computers
- Windows 10 Settings(VM)

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Mangement Steps</h2>

- Installed Active Directory Domain Services on Windows Server VM
- Created a new forest(can be named virtually anyting, just remember it)
- Created a Domain Admin in Active Directory users and Computers, as well as a Employees Organizational Unit(Future Reference)
- Created another OU(Organizational Unit) for the Windows 10 VM
- Allowed "Domain Users" access to Windows 10 VM as non-adminsitrative users.
- Created a new file and ran a script in Powershell_ISE

<h2>Mangement Steps</h2>

<p>
<img src="https://github.com/user-attachments/assets/4740f8d5-1708-4b31-a84c-309139b7b219"/>

</p>
<p>
In the first step, I logged into the Windows Server VM, opened Server Manager, clicked on Add Roles and Features, and selected Active Directory Domain Services as the feature I wanted to add. The installation of this feature was then completed on the Windows Server VM. After the installation, I noticed the warning errors that appeared, and then I created a forest called "solaris.com" for Active Directory.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/c95b8609-d44f-40c6-b107-b8e1d2bafc3c"/>
<img src="https://github.com/user-attachments/assets/6a7db11a-a9c9-4eb4-9d02-b81f508c2e79"/>
</p>
<p>
For this step, I logged back into the Windows Server VM, which had been turned into a Domain Controller (referred to as the Domain Controller from now on) after it restarted. The first noticeable change was that I now had to log in using solaris.com\labuser. I then created a Domain Admin in Active Directory Users and Computers called "Jane Doe" (solaris.com\jane_admin, password: Password1), along with two Organizational Units: _EMPLOYEES and _ADMINS
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/d4554470-1d89-490a-92b1-beeb1ddee60c"/>
<img src="https://github.com/user-attachments/assets/9ee47b7d-9160-4445-81d0-24640d4980d7"/>
</p>
<p>
Back in the Domain Controller, within Active Directory Users and Computers, the Windows 10 VM did indeed show up in the Computers folder. However, I went ahead and created my own Organizational Unit called _CLIENTS and moved the Windows 10 VM from the Computers folder to _CLIENTS.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/f5f38b51-f5d0-4286-8228-f60cddb056b6"/>
</p>
<p>
Going back to the Windows 10 Virtual Machine (now referred to as client-1), I logged in as solaris\jane_admin, opened the Settings application, and searched for "Remote Desktop Settings." Under User Accounts, I clicked on Select users that can remotely access this PC and added Domain users.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/1ec440b6-3e29-4b42-bad1-e79800858ed0"/>
</p>
<p>
Afterwards I logged back into DC-1 as solaris.com\jane_admin, opened PowerShell ISE as an administrator, created a new file, pasted the contents of a pre-made script into it, and saved it as "multiuser_creation." I then ran the script. The script works by randomly generating a total of 7,500 users with random names. However, the users do not have random passwords, as that would be cumbersome to manage. Instead, they all share a single password: Password1.
</p>
<br />
