📁 Step 01 - collect the first Evedince, Create a folder named {Analysis\Registry}
E:\Windows\System32\config
  Default
  SAM
  SECURITY
  SOFTWARE
  SYSTEM
E:\Users\{User-Name}
  NTUSER.DAT
E:\Users\{User-Name}\AppDATA\Local\Microsoft\Windows
  UsrClass.dat
📁 Step 02 - Collection of System Information [ ZimmerMan Registry Explorer ]
  ComputerName
  HKLM\System\CurrentControlSet\Control\Computername\
  Windows Version 
  HKLM\System\Microsoft\Windows NT\Currentversion\
  Timezone
  HKLM\System\CurrentControlSet\Control\TimeZoneInformation\
  Network Information
  HKLM\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{interface-name}
  Defender Settings
  HKLM\Software\Microsoft\Windows Defender\

📁 Step 03 - Using "regripper" and Find these Question 
  rip.exe -r C:\Cases\Analysis\Registry\SOFTWARE -p winver
  rip.exe -r C:\Cases\Analysis\Registry\SYSTEM -p timezone
  rip.exe -r C:\Cases\Analysis\Registry\SYSTEM -p nic2
  rip.exe -r C:\Cases\Analysis\Registry\SOFTWARE -p networklist [keeps track of networks a system has connected to]
  rip.exe -r C:\Cases\Analysis\Registry\SYSTEM -p shutdown
  rip.exe -r C:\Cases\Analysis\Registry\SOFTWARE -p defender

-----------------------------------------------------------------------------------

Windows Registry Forensics: NTUSER.DAT vs UsrClass.dat
📁 Overview

Two critical user registry hives in Windows forensics that store different types of user activity and configuration data.

🔍 NTUSER.DAT

Location: C:\Users\[Username]\NTUSER.DAT
Loaded Registry Hive: HKEY_CURRENT_USER (HKCU)
Primary Function: Stores user profile-specific settings and preferences tied directly to the user's environment.

Forensic Significance & Key Paths
| Category | Example Key Path | Forensic Data |
|----------|-----------------|---------------|
| **Program Execution** | `Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` | Recently opened files |
| **User Activity** | `Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist` | Program usage tracking |
| **Persistence** | `Software\Microsoft\Windows\CurrentVersion\Run` | Startup programs |
| **Network Information** | `Software\Microsoft\Windows NT\CurrentVersion\NetworkList` | WiFi/Network connections |
| **Application Settings** | `Software\Microsoft\Office\..` | User-specific application configurations |
| **Browser Activity** | `Software\Microsoft\Internet Explorer\TypedURLs` | Browsing history/typed URLs |

📁 UsrClass.dat

Location: C:\Users\[Username]\AppData\Local\Microsoft\Windows\UsrClass.dat
Loaded Registry Hive: Contributes to HKEY_CLASSES_ROOT (HKCR) - merged view of HKLM\SOFTWARE\Classes + user's UsrClass.dat
Primary Function: Stores user-specific COM class registrations, shellbag data, and file association info (Windows Shell/Explorer behavior).

| Category | Example Key Path | Forensic Data |
|----------|-----------------|---------------|
| **Shellbags** | `Local Settings\Software\Microsoft\Windows\Shell\BagMRU` | Folders viewed, folder structure, timestamps |
| **File Associations** | `Software\Classes\.txt` | Program associations for file types |
| **Explorer Preferences** | `Software\Classes\Local Settings\Software\Microsoft\Windows\Shell\Bags` | Folder view preferences |
| **COM Objects** | `Software\Classes\CLSID\{GUID}` | Registered COM components for the user |

🎯 Key Differences
🔬 Forensic Analysis Tips
    NTUSER.DAT is essential for understanding user-specific application usage and system interaction
    UsrClass.dat provides critical evidence of file system navigation and user habits
    Both hives should be analyzed together for comprehensive user profiling
    Shellbag data in UsrClass.dat can reveal deleted folder structures
    UserAssist and RecentDocs in NTUSER.DAT show temporal activity patterns

| Aspect | NTUSER.DAT | UsrClass.dat |
|--------|------------|--------------|
| **Primary Content** | User environment settings | Shell/Explorer behavior |
| **Registry Root** | HKEY_CURRENT_USER | HKEY_CLASSES_ROOT (user portion) |
| **Forensic Focus** | User activity, persistence, applications | File browsing, associations, shell artifacts |
| **Location** | User profile root | `AppData\Local\Microsoft\Windows\` |
| **Loading** | At user login | When shell/Explorer starts |
-----------------------------------------------------------------------------------

📁 Step 4 - Create non-hidden file for "NTUSER.dat" , "USRCLASS.dat"
  attrib *
  attrib -h NTUSER.DAT
  attrib -h UsrClass.dat
  attrib *

Step 5 - Instead of "step 04 " using this batch file in cmd
  for /r %i in (*) do (C:\Tools\RegRipper\rip.exe -r %i -a > %i.txt)

Step 6 - 
## References
- [Understand Security Identifiers](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers) - Microsoft documentation on Windows Server Security Identifiers


