# <p align="center">Client User Security Group Policy</p>

This policy has the basic user security settings for Java.  The best way to setup Java is make all the configuration settings for java on your local computer, such as Exceptions sites and any changes needed under advanced security, then copy it to a network location.

1. Copy the following files from c:\users\%username%\AppData\LocalLow\Sun\Java\Deployment\Security\
  1. exception.sites
  2. trusted.certs
  3. trusted.jssecerts

2. Copy the following files from c:\users\%username%\AppData\LocalLow\Sun\Java\Deployment\
  1. deployment.properties
3. Group Policy Preferences
  1. Adjust the Java registry settings to copy the files from a network location that users have read access to.