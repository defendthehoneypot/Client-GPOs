#<p align="center">Client Computer Application Security Group Policy</p>

The GPO covers the computer based settings for Internet Explorer, Microsoft Office, Adobe and Java.  The Settings for IE are restrictive and any external sites needed for business use will need to be added to the Trusted Sites.  This in no way means that users will have a poor Internet surfing experience, but it is designed to cut down on malicious scripts.  It also has the basic security settings for the current version of Adobe Reader DC and Adobe Professional DC.  The Java settings keep it from auto updating so the user is not prompted.

#Internet Explorer Settings

The first section will be Windows Components > Internet Explorer > Internet Control Panel > Security Page.  The company domain name needs to be set, along with any domains that the company uses to conduct business.

1. Site to Zone Site Assignment List– Add the following entries.
  1. Change the place holder *.company.com to your organization DNS domain name.
  2. Add any entries the company needs for business use with a value of 2.
  ![alt text](https://github.com/defendthehoneypot/Client-GPOs/blob/master/screenshots/sitetozoneassignment.png "sitetozoneassignment")

2. Group Policy Preferences - These are the settings that cover Adobe and java.  Each entry has a URL where to find the settings description.
  1. Find each entry labeled trusted sites and change *.company.com to your company domain name.
  ![alt text](https://github.com/defendthehoneypot/Client-GPOs/blob/master/screenshots/adobetrustedsites.png "adobetrustedsites")