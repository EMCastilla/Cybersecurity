Compilation of over 70+ Windows-based tools. Each of the tools falls into one of the following categories:

- File and Disk Utilities
- Networking Utilities
- Process Utilities
- Security Utilities
- System Information
- Miscellaneous

The Sysinternals tool(s) can be downloaded and run from the local system, or the tool(s) can be run from the web.

A PowerShell module can download and install all of the Sysinternals tools:
`Download-SysInternalsTools C:\Tools\Sysint`

#### File and Disk Utilities

> Sigcheck

Command-line utility that shows file version number, timestamp information, and digital signature details, including certificate chains. It also includes an option to check a file’s status on VirusTotal.

*Use case:* check for unsigned files in C:\Windows\System32.
*Command:* `sigcheck -u -e C:\Windows\System32`
*Parameter usage:* 
* -u If VirusTotal check is enabled, show files that are unknown by VirusTotal or have non-zero detection, otherwise show only unsigned files.
* -e scan executables images only.

> Streams

The NTFS file system provides applications the ability to create alternate data streams of information. By default, all data is stored in a file's main unnamed data stream, but by using the syntax `file:stream`, you are able to read and write to alternates.

Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System). Every file has at least one data stream ($DATA) and ADS allows files to contain more than one stream of data. Natively Window Explorer doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but Powershell gives you the ability to view ADS for files.

Malware writers have used ADS to hide data in an endpoint, but not all its uses are malicious. When you download a file from the Internet unto an endpoint, there are identifiers written to ADS to identify that it was downloaded from the Internet.

*Example:*
![[Pasted image 20250419005253.png]]


> Sdelete

Secure Delete is a command line utility that takes a number of options. In any given use, it allows you to delete one or more files and/or directories, or to cleanse the free space on a logical disk.

#### Networking Utilities

> TCP View

Windows program that will show you detailed listings of all TCP and UDP endpoints on your system, including the local and remote addresses and state of TCP connections.

`C:\>tcpview -accepeula`

Windows has a built-in utility that provides the same functionality. This tool is called Resource Monitor. There are many ways to open this tool. From the command line use `resmon`.

#### Process Utilities

> Autoruns

shows you what programs are configured to run during system bootup or login, and when you start various built-in Windows applications like Internet Explorer, Explorer and media players.

`C:\>autoruns`

> ProcDump

Command-line utility whose primary purpose is monitoring an application for CPU spikes and generating crash dumps during a spike that an administrator or developer can use to determine the cause of the spike.

`C:\>procdump`

> Process Explorer

The Process Explorer display consists of two sub-windows. The top window always shows a list of the currently active processes, including the names of their owning accounts, whereas the information displayed in the bottom window depends on the mode that Process Explorer is in: if it is in handle mode you'll see the handles that the process selected in the top window has opened; if Process Explorer is in DLL mode you'll see the DLLs and memory-mapped files that the process has loaded.

`C:\>procexp -accepteula`

There is an option within ProcExp to Verify Signatures. Once enabled, it shows up as a column within the Process view.

![[Pasted image 20250419013523.png]]

> Process Monitor

Advanced monitoring tool for Windows that shows real-time file system, Registry and process/thread activity.

`C:\>procmon -accepteula`

To use ProcMon effectively you *must* use the Filter and *must* configure it properly.

> PsExec

Light-weight telnet-replacement that lets you execute processes on other systems, complete with full interactivity for console applications, without having to manually install client software.

#### Security Utilities

> Sysmon

System Monitor (Sysmon) is a Windows system service and device driver that, once installed on a system, remains resident across system reboots to monitor and log system activity to the Windows event log. It provides detailed information about process creations, network connections, and changes to file creation time. By collecting the events it generates using Windows Event Collection or SIEM agents and subsequently analyzing them, you can identify malicious or anomalous activity and understand how intruders and malware operate on your network.

#### System Information

> WinObj

Is a 32-bit Windows NT program that uses the native Windows NT API (provided by NTDLL.DLL) to access and display information on the NT Object Manager's name space.

Remember that *Session 0* is the OS session and *Session 1* is the User session. Also recall that there will be at least 2 csrss.exe processes running, one for each session. Note Session 1 will be for the first user logged into the system.  

`C:\>winobj -accepteula`

#### Miscellaneous

> BgInfo

It automatically displays relevant information about a Windows computer on the desktop's background, such as the computer name, IP address, service pack version, and more.

`C:\>bginfo -accepteula`

> RegJump

It takes a registry path and makes Regedit open to that path. It accepts root keys in standard (e.g. HKEY_LOCAL_MACHINE) and abbreviated form (e.g. HKLM).

There are multiple ways to query the Windows Registry without using the Registry Editor, such as via the command line (`reg query`) and PowerShell (`Get-Item/Get-ItemProperty`).

*Example:*
`C:\>regjump HKLM\System\CurrentControlSet\Services\WebClient -accepteula`

> Strings

Scans the file you pass it for UNICODE (or ASCII) strings of a default length of 3 or more UNICODE (or ASCII) characters.

*Example:*
Search within the ZoomIt binary for any string containing the word 'zoom'.
 
![[Pasted image 20250419015610.png]]

