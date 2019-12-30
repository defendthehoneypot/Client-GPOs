# <p align="center">Introduction</p>


The goal of this document is to help implement the DoD/NIST security settings for Windows clients on a non-DoD network.  While many people become concerned with the use of DoD security settings, they are in fact the same controls from the NIST site but in an easy to follow checklist.  Along with this paper are the Group Policy Object (GPO) Backups that can be imported, with some minor changes, and implemented in almost any organization.  The GPOs have been built using the Defense Information Systems Agency site.  (DISA).  The reason for using the DISA site is the STIG Viewer and benchmarks.  These provide an easy to use tool for viewing and documenting the DoD/NIST settings.  The STIG viewer is a simple jar file that can be downloaded from the [DISA IASE SITE](https://public.cyber.mil/stigs/).  Using this tool one can import the specific catalog of settings for a wide range of systems, applications and other products.

Securing an enterprise network is a daunting task.  Trying to “bake” security into an already existing network can be challenging and often overwhelming.  This leads most organizations to give up or just rely on some type of next generation endpoint product as a magic bullet.  Don’t get me wrong, an enterprise should have a good endpoint protection/logging solution, but Group Policy is the first step in securing your Active Directory environment.

I have another repository that covers Active Directory naming conventions, OU structure breakdown and role based access groups.  [Naming Conventions](https://github.com/defendthehoneypot/NamingConvention)

Let’s break it down into the following areas:
##### 1. Patch management and vulnerability scanning
##### 2. Administrative account separation
##### 3. Network/Host segmentation
##### 4. Local Administrator Password Management
##### 5. Managing File/Attachment interaction
##### 6. Host firewall
##### 7. Application execution control
There are many more areas that could be addressed, but these seven areas will have the greatest impact.  The primary focus of this document is securing windows systems that reside in an Active Directory.

## Patch management and vulnerability scanning
Patching and validating the patches were applied successfully is the most important task for an enterprise.  This task cannot be emphasized enough. A well patched system will deter the most common type of escalation techniques.  Determined adversaries WILL gain a foothold on endpoints through phishing or drive by downloads.  Patching will deter some of these attacks, such as browser bugs.  The initial foothold will be in user space, and often the adversary will attempt to escalate their privileges to local admin or system.  Patching will help to deter these also, but know that there are new techniques are discovered fairly frequently.

After an adversary has their initial foothold they will also begin to map/explore the network.  They will look for Remote Code Execution (RCE) opportunities to laterally move to other systems.  MS17-010 is an example of a current RCE that was released this year.  [Microsoft Corporation, 2017](https://technet.microsoft.com/en-us/library/security/ms17-010.aspx)  Using their initial foothold, an adversary can tunnel traffic through this system to attack any systems with this vulnerability.[Example](https://www.cobaltstrike.com/help-socks-proxy-pivoting)  Even though the patch was released within a month of the vulnerability disclosure, we are still finding systems on every network with this vulnerability.  This type of vulnerability would also allow adversaries to move directly to a server system without having credentials.

This is where vulnerability scanning comes into play.  Vulnerability scanning is used to validate that the patching was successful.  Ensure that credentialed scans are run at least once a month to validate the patches have been applied.

## Administrative account separation
Adversaries move laterally throughout networks to try and find administrative credentials that will allow them to transition to server or administrator client systems.  A presentation by former NSA TAO Chief Rob Joyce talks about the process to “hunt” Sysadmins. [Joyce, 2016](https://www.usenix.org/conference/enigma2016/conference-program/presentation/joyce)  By separating the accounts with administrative access to clients, servers and domain controllers this will limit the credentials an adversary can obtain to move to a server or domain controller.  What does [administrative account separation](https://github.com/defendthehoneypot/Client-GPOs/tree/master/Client%20Computer%20Security%20v1.0#client-computer-security-group-policy) really mean?  Create separate user accounts for each class of system, e.g. clients, servers, network devices, domain controllers and security devices.  By using separate accounts based on class of system it will limit credentials the adversary is able to obtain should they gain access.  This will create password fatigue for your administrators, so help them out by using a centralized password management solution, and make sure it has two factor authentication.

## Network/Host segmentation
Ideally, clients and servers should only be allowed to communicate with other systems on the network with firewalls in-between to control and restrict access, but this is often cost and manpower prohibitive.  Start out by creating unique subnets for your administrators.  For smaller organizations create one subnet for all Service Desk/NOC personnel.  This will allow the organization to create Access Control Lists (ACL) on layer 3 interfaces or rules in host based firewalls to restrict/allow access for management systems.  In an ideal environment, each device type would be segmented into unique subnets based on role and access to these devices would be restricted to only required protocols.  See table 1 as an example.  (Note: This table is a simplification of what would actually be required. It is designed to provide a generic example to start from)
  
### <p align="center">Table 1</p>
| Device Type          | Subnet          | Inbound Subnet Rules          | Note                           |
| -------------------- | --------------- | ----------------------------- | ------------------------------ |
| Client               | 10.0.0.0/24     | IP Any/Any from MGMT subnet   | Allow admin MGMT subnet access |
| Client - Wireless    | 10.0.1.0/24     | IP Any/Any from MGMT subnet   | Allow admin MGMT subnet access |
| Printers             | 10.0.2.0/24     | TCP - 9100                    | Allow user printing            |
| WAP MGMT Interface   | 10.0.10.0/24    | TCP - 22 from MGMT subnet     | Allow admin MGMT subnet access |

In order to manage this in an enterprise environment it is necessary to standardize subnets and vlans.  I cannot emphasize enough how important it is to plan out distribution of IP space ahead of time instead of just grabbing the next available subnet.  Take some time and plan this out, better to spend a few days mapping everything out ahead of time.  Ideally, separate VRFs should be created for traversing the WAN for each subnet.   See table 2 as an example.  (Note: This table is a simplification of what would actually be required. It is designed to provide a generic example to start from)

### <p align="center">Table 2</p>
| Location             | Subnet          | VLAN                          | Note                           |
| -------------------- | --------------- | ----------------------------- | ------------------------------ |
| City#1               | 10.1.0.0/24     | 10                            | City#1 wired clients           |
| City#1 Wireless      | 10.1.1.0/24     | 20                            | City#1 wireless clients        |
| City#1 Printers      | 10.1.2.0/24     | 30                            | City#1 Printers                |
| City#2               | 10.2.0.0/24     | 10                            | City#1 wired clients           |
| City#2 Wireless      | 10.2.1.0/24     | 20                            | City#1 wireless clients        |
| City#2 Printers      | 10.2.2.0/24     | 30                            | City#1 Printers                |

For Active Directory integrated systems use the windows firewall managed through Group Policy (GPO), Linux systems use iptables or UFW and network devices use ACLs on layer 3 interfaces.

## Local Administrator Password Management
There are two parts of this task.  In a windows environment, the default administrator account does not lock after incorrect password attempts.  This leaves the system open to a brute force attack.  Both clients and servers should have the built-in administrator account renamed and disabled.  Then create a secondary local administrator account.  This account will get locked out if somebody attempts a direct brute force attempt against it, defeating the adversaries attempt at cracking the password.  The second part is to set a different password for each system local administrator account.  Microsoft has made this much easier with the release of [Local Administrator Password Solution (LAPS)](https://github.com/defendthehoneypot/Client-GPOs/tree/master/Client%20Computer%20Security%20v1.0#local-account-password-solution-laps).  [Microsoft Corporation, n.d.](https://technet.microsoft.com/en-us/mt227395.aspx)  This is an easy solution to deploy and dramatically cuts down on adversary’s ability to use local accounts for lateral movement.

## Managing File/Attachment interaction
Most modern attacks are tricking the user into opening attachments or clicking on links in e-mails that download files.  Microsoft has several settings for disallowing macros in Office documents.  The first is to disallow documents that have originated from the internet. [Microsoft Corporation, n.d.](https://blogs.technet.microsoft.com/mmpc/2016/03/22/new-feature-in-office-2016-can-block-macros-and-help-prevent-infection/)  The second is to disallow macros to run on systems.  There are several sub settings for disabling macros.  Additionally, each Office application has its own settings for macros, so ensure you set it in every application that allows it.  Ideally, you want to disable all macros without notification.  This is the safest option, since many phishing attempts have specific lures to trick users into enabling macros.  [Beaumont, p. 2017](https://doublepulsar.com/it-s-time-to-secure-microsoft-office-be50ec2797e3)  If you require macros in your organization, uses trusted locations within Office to allow them.

The other area of concern is files being downloaded from links in e-mails.  This presents a different obstacle for administrators.  Adversaries can use .hta, .sct, wsf, .dll and many other file types that the user can download and execute on the system.  One way to mitigate that is to force potentially harmful file types to open with notepad.exe, to render it harmless.  [Microsoft Corporation, n.d.](https://social.technet.microsoft.com/Forums/windowsserver/en-US/7c8b7f12-510a-435a-8053-856123cdb20d/changing-default-program-via-gpo?forum=winserverGP)

## Host Firewall
The [host firewall](https://github.com/defendthehoneypot/Client-GPOs/tree/master/Client%20Computer%20Security%20v1.0#windows-firewall) is another powerful tool to protect the clients.  As covered in an earlier section, host firewalls can be used to control inbound access to the host, but they can also be used to control outbound access.  There are several steps an organization could take to prevent malicious outbound communication.  PowerShell is something that should be blocked outbound on client systems.  (Note:  Attackers can use other methods to download malicious content, such as their own compiled version of powershell.)  There would exceptions for admins that use PowerShell for managing remote systems.  Regsvr32.exe is another application that has the potential to download malicious .dlls from the internet.

## Application Execution Control
[App locker](https://github.com/defendthehoneypot/Client-GPOs/tree/master/Client%20Computer%20Security%20v1.0#windows-applocker) is the built-in utility from Microsoft that can control execution of executables, scripts and installer files.  [Microsoft Corporation, n.d.](https://technet.microsoft.com/en-us/library/ee791899(v=ws.11).aspx)  Enabling App Locker with just the default rules will have a significant impact in stopping malware.  There will need to be some exceptions so users can download and run meeting conferencing software, i.e. GoToMeeting, zoom and WebEx.

## Summary
So why not just use endpoint protection/logging and stop caring about GPOs?  Enforcing basic security controls allows you to channel the attackers.  As an example, if I disable macros from running in Microsoft Office I can eliminate that as an attack vector and use my time to look elsewhere for more sofisticated methods.  The same holds true for Application Execution Control, by elminating the run of the mill vectors I can now focus on just bypasses for it, instead of every possible binary a user can download and run.  

There are many more steps to further protect Enterprise Networks, but these basic controls will make a significant difference.  GPOs are designed to be the foundational security baseline to help protect an Enterprise Network.  

There are a total of four client GPOs, two computer and two user based objects.  They all must be applied together to garner the complete benefit.  There are several specific security goals the GPOs are designed to achieve.  There are probably over 1000 different settings between all four of the GPOs.  This document will only cover the ones that require adjustments to provide functionality with your environment.  The reason behind breaking the GPOs into four separate policies is to allow changes to OS or application specific settings and not requiring the clients to complete refresh the entire policies, essentially make the GPOs more efficient or lightweight.  If one desires, the GPOs could be combined into two polices, one computer and one user.  There are many debates on this issue but I have found that separating them based on how often they are updated is a good practice.  Use the .htm files I have included with each GPO and scan for place holders like http://companyname.com and replace with your internal web server name.
Each of the four GPOs has its own readme.md file with further explanation.

