# <p align="center">Introduction</p>


The goal of this document is to help implement the DoD/NIST security settings for Windows clients on a non-DoD network.  While many people become concerned with the use of DoD security settings, they are in fact the same controls from the NIST site but in an easy to follow checklist.  Along with this paper are the Group Policy Object (GPO) Backups that can be imported, with some minor changes, and implemented in almost any organization.  The GPOs have been built using the Defense Information Systems Agency site.  (DISA).  The reason for using the DISA site is the STIG Viewer and benchmarks.  These provide an easy to use tool for viewing and documenting the DoD/NIST settings.  The STIG viewer is a simple jar file that can be downloaded from the [DISA IASE SITE](https://iase.disa.mil/stigs/Pages/stig-viewing-guidance.aspx).  Using this tool one can import the specific catalog of settings for a wide range of systems, applications and other products.

Securing an enterprise network is a daunting task.  Trying to “bake” security into an already existing network can be challenging and often overwhelming.  This leads most organizations to give up or just rely on some type of next generation endpoint product as a magic bullet.  Don’t get me wrong, an enterprise should have a good endpoint protection/logging solution, but Group Policy is the first step in securing your Active Directory environment.

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
Adversaries move laterally throughout networks to try and find administrative credentials that will allow them to transition to server or administrator client systems.  A presentation by former NSA TAO Chief Rob Joyce talks about the process to “hunt” Sysadmins. [Joyce, 2016](https://www.usenix.org/conference/enigma2016/conference-program/presentation/joyce)  By separating the accounts with administrative access to clients, servers and domain controllers this will limit the credentials an adversary can obtain to move to a server or domain controller.  What does administrative account separation really mean?  Create separate user accounts for each class of system, e.g. clients, servers, network devices, domain controllers and security devices.  By using separate accounts based on class of system it will limit credentials the adversary is able to obtain should they gain access.  This will create password fatigue for your administrators, so help them out by using a centralized password management solution, and make sure it has two factor authentication.

## Network/Host segmentation
Ideally, clients and servers should only be allowed to communicate with other systems on the network with firewalls in-between to control and restrict access, but this is often cost and manpower prohibitive.  Start out by creating unique subnets for your administrators.  For smaller organizations create one subnet for all Service Desk/NOC personnel.  This will allow the organization to create Access Control Lists (ACL) on layer 3 interfaces or rules in host based firewalls to restrict/allow access for management systems.  In an ideal environment, each device type would be segmented into unique subnets based on role and access to these devices would be restricted to only required protocols.  See table 1 as an example.  (Note: This table is a simplification of what would actually be required)
### Table 1
| Device Type          | Subnet          | Inbound Subnet Rules          | Note                           |
| -------------------- | --------------- | ----------------------------- | ------------------------------ |
| Client               | 10.0.0.0/24     | IP Any/Any                    | Allow Admin MGMT subnet Access |
| Client - Wireless    | 10.0.1.0/24     | IP Any/Any                    | Allow Admin MGMT subnet Access |
| Printers             | 10.0.2.0/24     | TCP - 9100                    | Allow user printing            |


There are a total of four client GPOs, two computer and two user based objects.  They all must be applied together to garner the complete benefit.  There are several specific security goals the GPOs are designed to achieve.  There are probably over 1000 different settings between all four of the GPOs.  This document will only cover the ones that require adjustments to provide functionality with your environment.  The reason behind breaking the GPOs into four separate policies is to allow changes to OS or application specific settings and not requiring the clients to complete refresh the entire policies, essentially make the GPOs more efficient or lightweight.  If one desires, the GPOs could be combined into two polices, one computer and one user.  There are many debates on this issue but I have found that separating them based on how often they are updated is a good practice.  Use the .htm files I have included with each GPO and scan for place holders like http://companyname.com and replace with your internal web server name.
Each of the four GPOs has its own readme.md file with further explanation.

