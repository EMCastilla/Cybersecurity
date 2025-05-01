#### Task Manager

GUI-based windows utility that allows users to see what is running on the Windows system.
Processes are categorized as: `Apps`, `Background Processes` and `Windows Processes`.

The columns are very minimal. The columns Name, Status, CPU, and Memory are the only ones visible. To view more columns, right-click on any column header to open more options. 
*Details of each column:
* Type - Each process falls into 1 of 3 categories (Apps, Background process, or Windows process).
* Publisher - Think of this column as the name of the author of the program/file.
* PID - This is known as the process identifier number. Windows assigns a unique process identifier each time a program starts. If the same program has multiple running processes, each will have its unique process identifier (PID).
* Process name - This is the file name of the process. In the above image, the file name for Task Manager is Taskmrg.exe. 
* Command line - The full command used to launch the process. 
* CPU - The amount of CPU (processing power) the process uses.
* Memory - The amount of physical working memory utilized by the process.

In `Details tab` good columns to add are *Image path name* and *Command line* . These 2 columns can quickly alert an analyst of any outliers with a given process.

*Example:*

![[Pasted image 20250417003141.png]]
PID 384 is paired with a process named svchost.exe, a Windows process, but if the image path name or command line is not what its expected to be, then we can perform a deeper analysis of the preocess.

> [!hint]
> Use Process Hacker or Process Explorer to see parent process information.

PID 384 parent process ==must== be services.exe


---

#### System

The first Windows process on the list is System (always PID 4).

*Normal behaviour for this process:*
Image Path:  N/A
Parent Process:  None
Number of Instances:  One
User Account:  Local System
Start Time:  At boot time

*What is unusual behaviour for this process?*
* A parent process (aside from System Idle Process (0))
* Multiple instances of System. (Should only be one instance) 
* A different PID. (Remember that the PID will always be PID 4)
* Not running in Session 0


---

#### System > smss.exe

`Session Manager Subsystem` also known as the `Windows Session Manager`, is responsible for creating new sessions. It is the first user-mode process started by the kernel.
This subsystem includes `win32k.sys` (kernel mode), `winsrv.dll` (user mode) and `csrss.exe` (user mode).

Session 0 - csrss.exe & wininit.exe
Session 1 - csrss.exe & winlogon.exe

Any other subsystem listed in the Required value of `HKLM\System\CurrentControlSet\Control\Session Manager\Subsystems` is also launched.

*Normal behaviour of this process:*
Image Path:  %SystemRoot%\System32\smss.exe
Parent Process:  System
Number of Instances:  One master instance and child instance per session. The child instance exits after creating the session.
User Account:  Local System
Start Time:  Within seconds of boot time for the master instance

*What is unusual behaviour for this process?*
* What is unusual?
* A different parent process other than System (4)
* The image path is different from C:\Windows\System32
* More than one running process. (children self-terminate and exit after each new session)
* The running User is not the SYSTEM user
* Unexpected registry entries for Subsystem


---

#### csrss.exe

Client Server Runtime Process is the user-mode side of the Windows subsystem. This process is always running and is critical to the system operation.

This process is also responsible for making the Windows API available to other processes, mapping drive letters, and handling the Windows shutdown process.

*Normal behaviour of this process:*
Image Path:  %SystemRoot%\System32\csrss.exe
Parent Process:  Created by an instance of smss.exe
Number of Instances:  Two or more
User Account:  Local System
Start Time:  Within seconds of boot time for the first two instances (for Session 0 and 1). Start times for additional instances occur as new sessions are created, although only Sessions 0 and 1 are often created.

*What is unusual behaviour for this process?*
- An actual parent process. (smss.exe calls this process and self-terminates)
- Image file path other than C:\Windows\System32
- Subtle misspellings to hide rogue processes masquerading as csrss.exe in plain sight
- The user is not the SYSTEM user.


---

#### wininit.exe

The `Windows Initialization Process`, wininit.exe, is responsible for launching services.exe (Service Control Manager), lsass.exe (Local Security Authority), and lsaiso.exe within Session 0. It is another critical Windows process that runs in the background, along with its child processes.

> [!note]
> lsaiso.exe is a process associated with **Credential Guard and KeyGuard**. You will only see this process if Credential Guard is enabled.

*Normal behaviour of this process:*
Image Path:  %SystemRoot%\System32\wininit.exe
Parent Process:  Created by an instance of smss.exe
Number of Instances:  One
User Account:  Local System
Start Time:  Within seconds of boot time

*What is unusual behaviour for this process?*
- An actual parent process. (smss.exe calls this process and self-terminates)
- Image file path other than C:\Windows\System32
- Subtle misspellings to hide rogue processes in plain sight
- Multiple running instances
- Not running as SYSTEM


---

#### wininit.exe > services.exe

`Service Control Manager` (SCM) or **services.exe** handles system services: loading services, interacting with services anda starting or ending services. It maintains a database that can be queried using `sc.exe`

Information regarding services is stored in the registry, `HKLM\System\CurrentControlSet\Services`.

When a user logs into a machine successfully, this process is responsible for setting the value of the Last Known Good control set (Last Known Good Configuration), `HKLM\System\Select\LastKnownGood`, to that of the CurrentControlSet.

This process is the parent to several other key processes: svchost.exe, spoolsv.exe, msmpeng.exe, and dllhost.exe, to name a few.

*Normal behaviour of this process:*
Image Path:  %SystemRoot%\System32\services.exe
Parent Process:  wininit.exe
Number of Instances:  One
User Account:  Local System
Start Time:  Within seconds of boot time

*What is unusual behaviour for this process?*
- A parent process other than wininit.exe
- Image file path other than C:\Windows\System32
- Subtle misspellings to hide rogue processes in plain sight
- Multiple running instances
- Not running as SYSTEM


---

#### wininit.exe > services.exe > svchost.exe

The `Service Host` (Host Process for Windows Services), or **svchost.exe**, is responsible for hosting and managing Windows services.

The services running in this process are implemented as DLLs. The DLL to implement is stored in the registry for the service under the `Parameters` subkey in `ServiceDLL`. The full path is `HKLM\SYSTEM\CurrentControlSet\Services\SERVICE NAME\Parameters`.

*Normal behaviour of this process:*
Image Path: %SystemRoot%\System32\svchost.exe
Parent Process: services.exe
Number of Instances: Many
User Account: Varies (SYSTEM, Network Service, Local Service) depending on the svchost.exe instance. In Windows 10, some instances run as the logged-in user.
Start Time: Typically within seconds of boot time. Other instances of svchost.exe can be started after boot.

*What is unusual behaviour for this process?*
- A parent process other than services.exe
- Image file path other than C:\Windows\System32
- Subtle misspellings to hide rogue processes in plain sight
- The absence of the -k parameter

> [!note]
> -k identifier is shown in binary path under service porperties.


---

#### lsass.exe

`Local Security Authority Subsystem Service` (**LSASS**) is a process in Microsoft Windows operating systems that is responsible for enforcing the security policy on the system. It verifies users logging on to a Windows computer or server, handles password changes, and creates access tokens. It also writes to the Windows Security Log.

It creates security tokens for SAM (Security Account Manager), AD (Active Directory), and NETLOGON. It uses authentication packages specified in `HKLM\System\CurrentControlSet\Control\Lsa`.

*Normal behaviour of this process:*
Image Path:  %SystemRoot%\System32\lsass.exe
Parent Process:  wininit.exe
Number of Instances:  One
User Account:  Local System
Start Time:  Within seconds of boot time

*What is unusual behaviour for this process?*
- A parent process other than wininit.exe
- Image file path other than C:\Windows\System32
- Subtle misspellings to hide rogue processes in plain sight
- Multiple running instances
- Not running as SYSTEM


---

#### winlogon.exe

The `Windows Logon`, **winlogon.exe**, is responsible for handling the **Secure Attention Sequence** (SAS). It is the ALT+CTRL+DELETE key combination users press to enter their username & password.

This process is also responsible for loading the user profile. It loads the user's NTUSER.DAT into HKCU, and userinit.exe loads the user's shell.

It is also responsible for locking the screen and running the user's screensaver, among other functions.

*Normal behaviour of this process:*
Image Path:  %SystemRoot%\System32\winlogon.exe
Parent Process:  Created by an instance of smss.exe that exits, so analysis tools usually do not provide the parent process name.
Number of Instances:  One or more
User Account:  Local System
Start Time:  Within seconds of boot time for the first instance (for Session 1). Additional instances occur as new sessions are created, typically through Remote Desktop or Fast User Switching logons.

*What is unusual behaviour for this process?*
- An actual parent process. (smss.exe calls this process and self-terminates)
- Image file path other than C:\Windows\System32
- Subtle misspellings to hide rogue processes in plain sight
- Not running as SYSTEM
- Shell value in the registry other than explorer.exe


---

#### explorer.exe

 This process gives the user access to their folders and files. It also provides functionality for other features, such as the Start Menu and Taskbar.

There will be many child processes for explorer.exe.

*Normal behaviour of this process:*
Image Path:  %SystemRoot%\explorer.exe
Parent Process:  Created by userinit.exe and exits
Number of Instances:  One or more per interactively logged-in user
User Account:  Logged-in user(s)
Start Time:  First instance when the first interactive user logon session begins

*What is unusual behaviour for this process?*
- An actual parent process. (userinit.exe calls this process and exits)
- Image file path other than C:\Windows
- Running as an unknown user
- Subtle misspellings to hide rogue processes in plain sight
- Outbound TCP/IP connections

