# Client GPOs
Refer to my site for more explanation on each GPO.  www.defendthehoneypot.com
There are a total of four client GPOs, two computer, and two user based objects.  They all must be applied together to garner the complete benefit.  There are several specific security goals the GPOs are designed to achieve.  There are probably over 1000 different settings between all four of the GPOs.  This document will only cover the ones that require adjustments to provide functionality with your environment.  The reason behind breaking the GPOs into four separate policies is to allow changes to OS or application specific settings and not requiring the clients to complete refresh the entire policies, essentially make the GPOs more efficient or lightweight.  If one desires, the GPOs could be combined into two polices, one computer and one user.  There are many debates on this issue but I have found that separating them based on how often they are updated is a good practice.  Use the .htm files I have included with each GPO and scan for place holders like http://companyname.com and replace with your internal web server name.

2016-12-01 - Updated the GPOs to include the below binaries with user permissions removed.  These are all administrative type binaries that should not cause you any issues.  If any of the restrictions on the binaries cause problem, do not just remove the entry, add the appropriate permissions back to the binary and let the GPO propagate before removing the entry.  Removing the entry without restoring the user permissions will leave the binary with the current ACLs, they do not revert to the local permissions.

%SystemRoot%\Microsoft.NET\Framework\v2.0.50727\MSBuild.exe
%SystemRoot%\Microsoft.NET\Framework\v3.5\MSBuild.exe
%SystemRoot%\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe
%SystemRoot%\Microsoft.NET\Framework64\v2.0.50727\MSBuild.exe
%SystemRoot%\Microsoft.NET\Framework64\v3.5\MSBuild.exe
%SystemRoot%\Microsoft.NET\Framework64\v4.0.30319\MSBuild.exe</br>
%SystemRoot%\System32\at.exe
%SystemRoot%\System32\cscript.exe
%SystemRoot%\System32\find.exe
%SystemRoot%\System32\ftp.exe
%SystemRoot%\System32\net.exe
%SystemRoot%\System32\net1.exe
%SystemRoot%\System32\netsh.exe
%SystemRoot%\System32\ROUTE.EXE
%SystemRoot%\System32\tasklist.exe
%SystemRoot%\System32\whoami.exe
%SystemRoot%\SysWOW64\at.exe
%SystemRoot%\SysWOW64\cscript.exe
%SystemRoot%\SysWOW64\find.exe
%SystemRoot%\SysWOW64\ftp.exe
%SystemRoot%\SysWOW64\net.exe
%SystemRoot%\SysWOW64\net1.exe
%SystemRoot%\SysWOW64\netsh.exe
%SystemRoot%\SysWOW64\ROUTE.EXE
%SystemRoot%\SysWOW64\tasklist.exe
%SystemRoot%\SysWOW64\whoami.exe
