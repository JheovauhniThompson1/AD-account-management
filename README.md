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

- Got logged into the Windows Server and picked a random user
- Attempted to login to Client-1 with the wrong password multiple times 
- Configured Group Policy to restrict users to only 5 failed password attempts
- Attempted to log back into Client-1 after 6 failed attempts
- Unlocked Account in DC-1 and attempted to login with the correct password
- Observed the Logs in Client-1

<h2>Mangement Steps</h2>

<p>
<img src="https://github.com/user-attachments/assets/983b7b8a-5d2d-49c3-8bb8-aa459e43e191"/>
<img src="https://github.com/user-attachments/assets/d33e3eb3-4ee7-456e-a442-a35e47abbb1e"/>
  
</p>
<p>
For this repository, I first logged into the Domain Controller as jane_admin, opened Active Directory Users and Computers, and then chose a random user from the 7,500 we had previously generated. In this situation, it was the user "luka.jaj". After approximately 10 failed attempts, I didn’t receive any warning for "too many failed attempts" or an account lockout. Given this, I proceeded to configure the group policy to limit failed login attempts to 5 before triggering an account lockout.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/339195b3-7e3b-48f9-a693-98870ee06686"/>
</p>
<p>
To configure this specific group policy, I first returned to the Domain Controller and opened Group Policy Management. From there, I navigated to the Default Domain Policy, edited it, and went to Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy. I opened the policy and set the Account Lockout Threshold to 5, with the other settings applying automatically. Next, I linked the GPO to the _EMPLOYEES Organizational Unit by right-clicking the OU and selecting Link an Existing GPO. To speed up the process, I forced the Group Policy update by opening a command prompt and running gpupdate /force.
</p>
<br />


<p>
<img src="https://github.com/user-attachments/assets/df6daf28-77b7-4fab-8e11-5526d50dd09b"/>
<img src="https://github.com/user-attachments/assets/1b87ee59-af42-449b-b3d5-bbea4f6db167"/>
</p>
<p>
To test the new policy, I attempted to log back into Client-1 with 6 failed password attempts, and it worked as expected! Since the policy was functioning properly, I returned to the Domain Controller, re-enabled luka.jaj's account, and successfully logged in with the correct password.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/c0b2bc6c-888a-4030-8500-9753350aab70"/>
<img src="https://github.com/user-attachments/assets/1e4c1b08-273f-4a87-8007-b5ac99214433"/>
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
