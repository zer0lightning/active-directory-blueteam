# Enable Netlogon Logging
Sources: 
- https://www.manageengine.com/products/active-directory-audit/how-to/how-to-enable-netlogon-logging.html
- https://community.spiceworks.com/how_to/154561-tracking-failed-logon-attempts-and-lockouts-on-your-network

As an IT administrator one of the most common issues you'll have to resolve quite frequently is unlocking user accounts and checking why a user was not properly authenticated into the domain. Event 4740 in the Event Viewer describes a user account that was locked out. However if you're looking for more detailed information about the account lockout such as tracking the source of a bad password, you can refer to the Netlogon log file.

Netlogon is a Local Security Authority service that runs in the background. It is responsible for authenticating users in to the domain. Executing a few commands from an elevated Command Prompt enables the logging of Netlogon events. After this you can access the Netlogon file to check logon events and troubleshoot them. Of course reading through a log file and looking for a specific event is a cumbersome process. So to simplify this, you can click on the ADAudit Plus tab. ADAudit Plus is a real-time Active Directory (AD) change auditing solution that helps you track changes to your AD infrastructure and provides you an intuitive interface to view all your network activity.

Step 1: Enable Netlogon Logging
In an elevated Command Prompt, enter the following command:
```
Nltest /DBFlag:2080FFFF
```
Note: To restore/disable Netlogon Logging
```
Nltest /DBFlag:0x0
```
After executing the above command, you can stop and start your Netlogon service, just to ensure that the logs are being written to the Netlogon file. The following commands help you do that.
```
net stop netlogon
net start netlogon
```

Step 2: Increase log file capacity
The default log file capacity of Netlogon is 20MB. When maximum file capacity is reached, the existing Netlogon file is renamed as Netlogon.bak and a new Netlogon.log is created to record new events.
Something to keep in mind is that the disk space that you allot to Netlogon files should be doubled. This is because the disk space is used to store the current Netlogon file and and equal amount is used to store backup log files. For example, if you want to allot 50MB to Netlogon files, configure diskspace to 100 MB so that 50MB is maintained for Netlogon.log and another 50MB for Netlogon.bak.

Run GPMC.msc to launch the Group Policy Management Console.
Right-click your Default Domain Policy and select 'Edit' to configure it. In the Group Policy Management Editor, select Computer Configuration--->AdministrativeTemplates-->System-->NetlogonDouble-click the 'Specify maximum log file size' setting and set it to Enabled. Enter the file size in the Bytes drop down and click OK.


Step 3: Access your Netlogon files and understand common Netlogon codes
You can view your Netlogon files by entering the following command in the 'Run' Dialog box.
```
%SYSTEMROOT%\debug\netlogon.log
```
Below is a snippet of the Netlogon log file showing a successful logon event.


Here are a few codes you can use to understand the LOGON activity in your log file.

Log Code	Description
```
0x0	Successful login
0xC000006D	Unsuccessful attempt to login due to bad username
0xC0000072	Disabled user account
0xC000006F	Unsuccessful login attempt due to time restrictions
0xC0000071	An account's password has expired
0xC000006A	Incorrect password entered
0xC000006C	Password policy has not been followed
0xC0000224	Password must be changed before the first login attempt
0xC000006E	Login has failed due to user account restrictions
0xC0000193	User account has expired
0xC0000234	User account has been automatically locked
0xC0000064	User does not exist
```
# Discussion
The netlogon.log file located in %SystemRoot%\Debug can be invaluable for troubleshooting client logon and related issues. When enabled at the highest setting (0x2000ffff), it logs useful information, such as the site the client is in, the domain controller the client authenticated against, additional information related to the DC Locator process, account password expiration information, account lockout information, and even Kerberos failures.

The NetLogon logging level is stored in the following registry value:

HKLM\System\CurrentControlSet\Services\Netlogon Parameters\DBFlag
If you set that registry value manually, instead of using nltest, you’ll need to restart the NetLogon service for it to take effect.

One of the issues with the netlogon.log file is that it can quickly grow to several megabytes, which makes it difficult to peruse. A new tool available for Windows XP and Windows Server 2003 called nlparse can filter the contents of the netlogon.log file so that you’ll only see certain type of log entries. 

# Increase Logging for NETLOGON 
Specify maximum log file size
This policy setting specifies the maximum size in bytes of the log file netlogon.log in the directory %windir%\debug when logging is enabled.

By default, the maximum size of the log file is 20MB. If you enable this policy setting, the maximum size of the log file is set to the specified size. Once this size is reached the log file is saved to netlogon.bak and netlogon.log is truncated. A reasonable value based on available storage should be specified.
If you disable or do not configure this policy setting, the default behavior occurs as indicated above.
Supported on: At least Windows Server 2003
```
Bytes:
Registry Hive	HKEY_LOCAL_MACHINE
Registry Path	Software\Policies\Microsoft\Netlogon\Parameters
Value Name	MaximumLogFileSize
Value Type	REG_DWORD
Default Value	536936447
Min Value	
Max Value	4294967295
```
Source: https://admx.help/?Category=Windows_10_2016&Policy=Microsoft.Policies.NetLogon::Netlogon_MaximumLogFileSize
