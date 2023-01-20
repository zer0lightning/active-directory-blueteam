# How to disable “Redirected” printers with Group Policy?
 
If there are several ”redirected” printers available on the server computer in the RDP connection “Microsoft XPS Document Writer (redirected 2)” , the Microsoft Easy Print function is enabled.
NOTE: Any printers that contain the word ”redirected” in their name were not attached by Black Ice Print2RDP. These printers have been attached by Microsoft Easy Print.

One can disable Microsoft Easy Print and prevent Printers redirected on the server with a Remote Desktop connection through the local Group Policy.
On the Server go to the Local Group Policy Editor:

1.    Press Start + r
2.    In the Run window type gpedit.msc.
3.    Press Enter.

In the Local Group Policy Editor navigate to Computer Configuration > Administrative Templates > Windows Components > Remote Desktop Services >Remote Desktop Session Host > Printer Redirection.
Enable the Do not allow client printer redirection rule to prevent the server to install “Redirected” printers.

Source: https://blackice.com/Help/Internet/Print2RDP%20webhelp/WebHelp/How_to_disable_%E2%80%9CRedirected%E2%80%9D_printers_with_Group_Policy.htm

# Disable file copy through RDP with Group Policy
How to configure the Local or AD Group Policy Objects to disable Clipboard redirection, Drive mapping/redirection, LPT port redirection and/or COM port redirection through Remote Desktop Protocol

In this post we'll see how we can use the Windows Server Group Policy Management Console (GPMC) to globally disable some useful - yet potentially harmful - features that natively come with the Remote Desktop protocol, such as:

- Clipboard redirection, which can be used to cut/paste text and files from the remote PC to the local PC and vice-versa (thus allowing file copy/download).
- Drive mapping/redirection, which allows the remote user to access their local drive(s) through the remote PC (thus allowing file copy/download).
- COM port redirection, which can be used to make some local COM devices available to the remote PC.
- LPT port redirection, which can be used to make some local Line Printer Terminal devices available to the remote PC (thus allowing local printing of remote files).

As we can easily see those functions can be quite powerful, since they allow the remote user to easily access to the files hosted on the company-owned PC in various ways: this can be great when they (and/or the company they work for) have full ownership and rights to handle them in any way, yet it can also pose severe risks of unauthorized data breaches if they don't.
As a matter of fact, the remote workers rarely have full ownership rights over company documents: they are often allowed to access them only from corporate-owned devices, without being authorized to copy or print them somewhere else. When such limitations are in force, preventing those users from being able to copy, download and/or print those files to their local PC could be very useful to comply with the company policies.
Luckily enough, the Windows Server Group Policy Management Console (GPMC) can be configured to disallow those features for all RDP users with the following steps:

Access a computer upon which the Active Directory Domain Services server role is installed.

Launch Server Manager, click Tools, and then click Group Policy Management.
In the Group Policy Management console, expand the following path: Forest > example.com > Domains > example.com > Group Policy Objects, where example.com is the name of the domain where the RDP client computer policies that you want to configure are located.
Right click the Default Domain Policy node and select Modify to open the Group Policy Management Console (GPMC).

Use the GPMC user interface to navigate through the following path: Windows Configuration > Administrative Templates > Windows Components > Remote Desktop Services > Remote Desktop Session Host > Device and Resource Redirection
Access the following group policy settings and enable/disable them accordingly with your needs:

- Do not allow Clipboard redirection
- Do not allow COM port redirection
- Do not allow drive redirection
- Do not allow LPT port redirection

IMPORTANT: The above instructions are meant for Active Directory networks: if you don't have a Windows Domain you can still use the GPMC to enforce those policies, but you'll have to perform those steps on each company PC you'll want to prevent them from.
As you can see, in order to prevent the users from using each feature you need to enable the group policy that actively blocks it, thus overriding the default value that allows it for all users.

Once you have set up these new Group Policy, you might want to immediately apply them everywhere by forcing a Group Policy refresh on all the Windows client machines within the Organizational Unit. To learn how you can do that, take a look at our How to force a Remote Group Policy Refresh with GPUpdate post that explains how to pull off such task selectively or globally using either CMD, Powershell or the Group Policy Management Console (GPMC).
Conclusions
That's it, at least for now: I hope that this post will help those System Administrators that are looking for a way to prevent their users from using RDP connections to copy, download and/or print company-owned documents from their local device.

Source: https://www.ryadel.com/en/disable-remote-download-through-rdp-group-policy/

# Disable USB Storage, Printer, Audio and Scanner from Windows Registry
As useful and convenient as it is, USB ports also pose huge security risks by allowing anyone to easily steal data from a private computer by plugging in a USB flash drive and copying data. Fortunately, it is possible to disable USB storage, USB printers, USB Audio and even USB scanners by modifying a registry value in Windows. Here are the steps:

Before you continue, ensure that you have administrative privileges on the computer to make changes in the Windows registry.

1. Press the Start button, type regedit and hit Enter key.
2. Expand the left tree to the following path:
```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services```

3. There will be a lot of folders under the Services. To disable USB storage which includes USB flash drives and external hard drives that connect using USB, look for USBSTOR.
```disable usbstor```

At the right pane, double click at Start and change the value data from 3 to 4. That will disable the USB drives for that computer. To re-enable back, simply change back the value data from 4 to 3.
This will take effect immediately without the need to restart your computer. The other registry keys that you can edit are:
```
usbprint = USB printer
usbaudio = USB audio
UsbScan = USB scanner
```
Source: 
- https://www.raymond.cc/blog/disable-usb-storage-printer-audio-and-scanner-with-usb-manager/
- https://dirteam.com/sander/2020/08/07/howto-harden-remote-desktop-connections-to-domain-controllers/
