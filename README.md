# <p align="center">Introduction</p>


The goal of this document is to help implement the DoD/NIST security settings for Windows clients on a non-DoD network.  While many people become concerned with the use of DoD security settings, they are in fact the same controls from the NIST site but in an easy to follow checklist.  Along with this paper are the Group Policy Object (GPO) Backups that can be imported, with some minor changes, and implemented in almost any organization.  The GPOs have been built using the Defense Information Systems Agency site.  (DISA).  The reason for using the DISA site is the STIG Viewer and benchmarks.  These provide an easy to use tool for viewing and documenting the DoD/NIST settings.  The STIG viewer is a simple jar file that can be downloaded from the [DISA IASE SITE](https://iase.disa.mil/stigs/Pages/stig-viewing-guidance.aspx).  Using this tool one can import the specific catalog of settings for a wide range of systems, applications and other products.

Securing an enterprise network is a daunting task.  Trying to “bake” security into an already existing network can be challenging and often overwhelming.  This leads most organizations to give up or just rely on some type of next generation endpoint product as a magic bullet.  Don’t get me wrong, an enterprise should have a good endpoint protection/logging solution, but it is not something that can be installed and forgotten about.

Let’s break it down into the following areas:
##### 1. Patch management and vulnerability scanning
##### 2. Administrative account separation
##### 3. Network/Host segmentation
##### 4. Local Administrator Password Management
##### 5. Managing File/Attachment interaction
##### 6. Host firewall
##### 7. Application execution control
There are many more areas that could be addressed, but these seven areas will have the greatest impact.  The primary focus of this document is securing windows systems that reside in an Active Directory.

# NOTE:  NONE OF THESE SETTINGS ARE A SUBSTITUTE FOR PATCHING THE OS AND APPLICATIONS.

There are a total of four client GPOs, two computer and two user based objects.  They all must be applied together to garner the complete benefit.  There are several specific security goals the GPOs are designed to achieve.  There are probably over 1000 different settings between all four of the GPOs.  This document will only cover the ones that require adjustments to provide functionality with your environment.  The reason behind breaking the GPOs into four separate policies is to allow changes to OS or application specific settings and not requiring the clients to complete refresh the entire policies, essentially make the GPOs more efficient or lightweight.  If one desires, the GPOs could be combined into two polices, one computer and one user.  There are many debates on this issue but I have found that separating them based on how often they are updated is a good practice.  Use the .htm files I have included with each GPO and scan for place holders like http://companyname.com and replace with your internal web server name.
Each of the four GPOs has its own readme.md file with further explanation.

