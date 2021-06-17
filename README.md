# PowerShell
This project builds on the Group Policy Objectives activities.  I will create domain-hardening GPOs and revisit PowerShell fundamentals.
The screenshot below is of all the GPOs created for this project. 

Task 1: a GPO: Disable Local Link Multicast Name Resolution (LLMNR)
I will investigate and mitigate one of the attack vectors that exists within a Windows domain.

![image](https://user-images.githubusercontent.com/80080368/122442999-24c1d600-cf6d-11eb-96d5-2bb0291671b4.png)



MITRE is one of the world's leading organizations for threat intelligence in cybersecurity. MITRE maintains the Common Vulnerabilities and Exposures database, which catalogs officially known exploits. It also maintains this MITRE ATT&CK database, which catalogs attack methods and signatures of known hacking groups.




Local Link Multicast Name Resolution (LLMNR) is a vulnerability, so I will be disabling it on the Windows 10 machine (via the GC Computers OU).
A few notes about LLMNR:
LLMNR is a protocol used as a backup (not an alternative) for DNS in Windows.
When Windows cannot find a local address (e.g. the location of a file server), it uses LLMNR to send out a broadcast across the network asking if any device knows the address.
LLMNR’s vulnerability is that it accepts any response as authentic, allowing attackers to poison or spoof LLMNR responses, forcing devices to authenticate to them.
An LLMNR-enabled Windows machine may automatically trust responses from anyone in the network.
Turning off LLMNR for the GC Computers OU will prevent our Windows machine from trusting location responses from potential attackers.



Task 2: Create a GPO: Account Lockout
For security and compliance reasons, the CIO needs us to implement an account lockout policy on our Windows workstation. An account lockout disables access to an account for a set period of time after a specific number of failed login attempts. This policy defends against brute-force attacks, in which attackers can enter a million passwords in just a few minutes.
Account lockouts have some important considerations. Read about these in the following documentation:

Microsoft Security Guidance: Configuring Account Lockout
You only need to read the "Account Lockout Tradeoffs" and "Baseline Selection" sections.

To summarize, an overly restrictive account lockout policy (such as locking an account for 10 hours after 2 failed attempts), can potentially keep an account locked forever if an attacker repeatedly attempts to access it in an automated way.

![image](https://user-images.githubusercontent.com/80080368/122458695-17f9ae00-cf7e-11eb-9b2d-b4aa449215c2.png)



Task 3: Create a GPO: Enabling Verbose PowerShell Logging and Transcription
PowerShell is often used as a living off the land hacker tool. This means:

Once a hacker gains access to a Windows machine, they will leverage built-in tools, such as PowerShell and wmic, as much as possible to achieve their goals while trying to stay under the radar.

So why not just completely disable PowerShell?


Many security tools and system administration management operations, such as workstation provisioning, require heavy use of PowerShell to set up machines.


Best practices for enabling or disabling PowerShell are debated. This often leads to the solution of allowing only certain applications to run. These setups require a heavy amount of configuration using tools such as AppLocker.


This is why we're going to use a PowerShell practice that is recommended regardless of whether PowerShell is enabled or disabled: enabling enhanced PowerShell logging and visibility through verbosity.


This type of policy is important for tools like SIEM and for forensics operations, as it helps combat obfuscated PowerShell payloads.


![image](https://user-images.githubusercontent.com/80080368/122458800-32338c00-cf7e-11eb-8c8d-d1837f8c04f4.png)


Task 4: Create a Script: Enumerate Access Control Lists
Before we create a script, let's review Access Control Lists.


In Windows, access to files and directories are managed by Access Control Lists (ACLs). These identify which entities (known as security principals), such as users and groups, can access which resources. ACLs use security identifers to manage which principals can access which resources.


While you don't need to know the specific components within ACLs for this task, you do need to know how to use the Get-Acl PowerShell cmdlet to retrieve them. View Get-Acl documentation here.


Familiarize yourself with the basics of Get-Acls:


Get-Acl without any parameters or arguments will return the security descriptors of the directory you're currently in.


Get-Acl <filename> will return the specific file's ACL. We'll need to use this for our task.


![image](https://user-images.githubusercontent.com/80080368/122458944-5b541c80-cf7e-11eb-8dfa-aead179b6d7e.png)




Bonus Task 5: Verify Your PowerShell Logging GPO
For this task we'll want to test and verify that our PowerShell logging GPO is working properly.

Instructions


Ensure you're logged into the Windows 10 machine as sysadmin |  cybersecurity.


Run gpupdate in an administrative PowerShell window to pull the latest Active Directory changes.


Close and relaunch PowerShell into an administrative session.


Navigate to a directory you want to see the ACLs in. You can go to C:\Windows, as you did in Task 4.


Run the enum_acls.ps1 script using the full file path and name such as the one in Task 4.


Check the C:\Users\sysadmin\Documents for your new logs.

You should see a directory with the current date (for example, 20200908) as the directory name. Your new transcribed PowerShell logs should be inside.


![image](https://user-images.githubusercontent.com/80080368/122459021-6e66ec80-cf7e-11eb-902e-a5e68904ce67.png)




Deliverable for Task 2: Submit a screenshot of the different Account Lockout policies in Group Policy Management Editor. It should show the three values you set under the Policy and Policy Setting columns.




Deliverable for Task 3: Submit a screenshot of the different Windows PowerShell policies within the Group Policy Management Editor. Four of these should be enabled.



Deliverable for Task 4: Submit a copy of your enum_acls.ps1 script.

Deliverable for Bonus Task 5: Submit a screenshot of the contents of one of your transcribed PowerShell logs or a copy of one of the logs.


