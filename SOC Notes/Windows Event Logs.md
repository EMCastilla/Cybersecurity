#### Event Viewer

The Windows Event Logs are not text files that can be viewed using a text editor. However, the raw data can be translated into XML using the Windows API. The events in these log files are stored in a proprietary binary format with a .evt or .evtx extension. The log files with the .evtx file extension typically reside in `C:\Windows\System32\winevt\Logs` .

![[Pasted image 20250419023836.png]]

Event Viewer has three panes.

1. The pane on the left provides a hierarchical tree listing of the event log providers.
2. The pane in the middle will display a general overview and summary of the events specific to a selected provider.
3. The pane on the right is the actions pane.

> wevtutil.exe

Enables you to retrieve information about event logs and publishers. You can also use this command to install and uninstall event manifests, to run queries, and to export, archive, and clear logs.

![[Pasted image 20250419032559.png]]

`wevtutil COMMAND /?` This will provide additional information specific to a command. Example: we can use it to get more information on the command qe (query-events).
![[Pasted image 20250419150144.png]]


---
#### Get-WinEvent

Gets events from event logs and event tracing log files on local and remote computers.

*Examples:*
1. Get all logs froma computer -> `Get-WinEvent -ListLog *`
2. Get event log providers and log names -> `Get-WinEvent -ListProvider *`
3. Log filtering -> `Get-WinEvent -LogName Appication | Where-Object { $_.ProviderName -Match 'WLMS'}`

Instead of using `Where-Object` command we should use `FilterHash Table`:

`Get-WinEvent -FilterHashTable  @{
	`LogName='Application'
	`ProviderName='WLMS'
`}

Guidelines for defining a hash table are:
* Begin the hash table with an @ sign.
* Enclose the hash table in braces {}
* Enter one or more key-value pairs for the content of the hash table.
* Use an equal sign (=) to separate each key from its value.

*Accepted key/value pairs for the Get-WinEvent FilterHashtable parameter.*
![[Pasted image 20250419152107.png]]

*Helpful command:*
`Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-PowerShell/Operational'; ID=4104} | Select-Object -Property Message | Select-String -Pattern 'SecureString'`

The first part reads PowerShell log events (operational events).
The second part filters the previous results showing only the Message field.
The last part search in those messages the pattern 'SecureString'

> [!important]
> Select-String is like grep in Linux

*Why is this command useful?*
In cybersecurity, this type of command can be used to:

- **Audit the use of commands that handle passwords.**  
    It helps identify scripts that create or manipulate secure strings, which are often used to store sensitive credentials.
    
- **Detect scripts that might be trying to hide sensitive information.**  
    Attackers may use obfuscation or secure string methods to avoid detection.
    
- **Investigate malicious activity involving password capture or use.**  
    For example, the presence of `ConvertTo-SecureString` or `New-Object SecureString` may indicate attempts to reconstruct credentials or interact with secure APIs.

> [!important]
> Measure-Obejct is like wc -l in Linux

`-MaxEvents` -> specifies the maximum events to  display.


---
#### XPath Queries

Standard syntax and semantics for addressing parts of an XML document and manipulating strings, numbers, and booleans.

An XPath event query starts with ''`*` or `Event`.

```
// The following query selects all events from the channel or log file where the severity level is less than or equal to 3 and the event occurred in the last 24 hour period. 

XPath Query: *[System[(Level <= 3) and TimeCreated[timediff(@SystemTime) <= 86400000]]]
```

Click on the Details tab and select the XML View radio button. Don't worry if the log details you are viewing are slightly different. The point is understanding how to use the XML View to construct a valid XPath query.

![[Pasted image 20250419164536.png]]

*Using XPath for WinEvent*
`C:\Users\Administrator> Get-WinEvent -LogName Application -FilterXPath '*/System/EventID=100'`

*Using XPath for wevtutil*
`C:\Users\Administrator>wevtutil.exe qe Application /q:*/System[EventID=100] /f:text /c:1`

Combine 2 queries:
`Get-WinEvent -LogName Application -FilterXPath '*/System/EventID=101 and */System/Provider[@Name="WLMS"]'`

> Create XPath for elements within EventData

*Note:* The EventData element doesn't always contain information.

![[Pasted image 20250419165301.png]]

We will build the query for ***TargetUserName***. In this case, that will be System. The XPath query would be `Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="System"'`.


---
#### Event IDs

Good resources with event IDs of interest:

* [Windows+Logging+Cheat+Sheet_ver_Oct_2016.pdf](https://static1.squarespace.com/static/552092d5e4b0661088167e5c/t/580595db9f745688bc7477f6/1476761074992/Windows+Logging+Cheat+Sheet_ver_Oct_2016.pdf)
* [Wayback Machine](https://web.archive.org/web/20190115215749/https://apps.nsa.gov/iaarchive/customcf/openAttachment.cfm?FilePath=/iad/library/ia-guidance/security-configuration/applications/assets/public/upload/Spotting-the-Adversary-with-Windows-Event-Log-Monitoring.pdf&WpKes=aF6woL7fQp3dJiqyJL2LenrLxuHC7ztGtVNK3x)
* [Appendix L - Events to Monitor | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor)
* https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
* https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor
* https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/appendix-l--events-to-monitor

Note: Some events will not be generated by default, and certain features will need to be enabled/configured on the endpoint, such as PowerShell logging. This feature can be enabled via Group Policy or the Registry.

`Local Computer Policy > Computer Configuration > Administrative Templates > Windows Components > Windows PowerShell`

Another feature to enable/configure is Audit Process Creation, which will generate event ID 4688. This will allow command-line process auditing. This setting is NOT enabled in the virtual machine but feel free to enable it and observe the events generated after executing some commands.

`Local Computer Policy > Computer Configuration > Administrative Templates > System > Audit Process Creation`





