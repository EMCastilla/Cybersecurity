Most used Sysinternal tools for endpoint investigation:
* TCPVIew
* Process Explorer

**TCPView ->** Windows program that show you a detailed list of all TCP and UDP endopoints on your system.

**Process Explorer ->** This consists in two sub-windows. The top view always shows a list of cuirrently actives processes, in the bottom window it dependes on the mode that Process Explorer is in: if its in handle mode, youll see the hadles that the process selected in the top window has opened; if is in DLL mode youll see the DLLs and memory-mapped files that the process has loaded.

#### Endpoint Logging

>Windows Event Logs

The events in this log files are stored in a proprietary binary format with `.evt` or `.evtx` extension. This files typically reside in `C:\Windows\System32\winevt\Logs`.

3 main ways to of accessing this event logs:
* Event Viewer (GUI)
* Wevtutil (CLI)
* Get-WinEvent (cmdlet)

> Sysmon

Tool used to monitor and log events on Windows.
Sysmon gathers detailed and high-quality logs as well as event tracing that assists in identifying anomalies in your environment.

> OSQuery

Tool used to query an endpoint (or multiple endpoints) using SQL syntax.
Example:
```
C:\Users\Administrator\> osqueryi
Using a virtual database. Need help, type 'help'
osquery>
osquery> select pid,name,path from processes where name='lsass.exe';
+-----+-----------+-------------------------------+ 
| pid | name | path                               | 
+-----+-----------+-------------------------------+ 
| 748 | lsass.exe | C:\Windows\System32\lsass.exe | 
+-----+-----------+-------------------------------+
```

Osquery only allows you to query events inside the machine. But with Kolide Fleet, you can query multiple endpoints from the Kolide Fleet UI instead of using Osquery locally to query an endpoint.

> Wazuh

EDR solution. Features:
- Auditing a device for common vulnerabilities
- Proactively monitoring a device for suspicious activity such as unauthorized logins, brute-force attacks, or privilege escalations.
- Visualizing complex data and events into neat and trendy graphs
- Recording a device's normal operating behaviour to help with detecting anomalies

#### Endpoint Log Analysis

> Event Correlation

Identify significant relationships from mulitple sources such as application logs, endopint logs and network logs.

With this information, we can connect the dots of each artefact from the two data sources:

- Source and Destination IP
- Source and Destination Port
- Action Taken
- Protocol
- Process name
- User Account
- Machine Name

> Baselining

Process of knowing what is expected to be normal. In terms of endpoint security monitoring, it requires a vast amount of data-gathering to establish the standard behaviour of user activities, network traffic across infrastructure, and processes running on all machines owned by the organization.

*Example:

| **Baseline**                                                                                                                                              | **Unusual Activity**                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| The organization's employees are in London, and the regular working hours are between 9 AM and 6 PM.                                                      | A user has authenticated via VPN connecting from Singapore at 3 AM.                      |
| A single workstation is assigned to each employee.                                                                                                        | A user has attempted to authenticate to multiple workstations.                           |
| Employees can only access selected websites on their workstations, such as OneDrive, SharePoint, and other O365 applications.                             | A user has uploaded a 3GB file on Google Drive.                                          |
| Only selected applications are installed on workstations, mainly Microsoft Applications such as Microsoft Word, Excel, Teams, OneDrive and Google Chrome. | A process named firefox.exe has been observed running on multiple employee workstations. |



