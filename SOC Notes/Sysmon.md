A tool used to monitor and log events on Windows.  Remains resident across system reboots to monitor and log system activity to the Windows event log.

Events within Sysmon are stored in `Applications and Services Logs/Microsoft/Windows/Sysmon/Operational`

Sysmon requires a config file in order to tell the binary how to analyze the events that it is receiving. You can create your own Sysmon config or you can download a config.

#### Event IDs (with examples)

>Event ID 1: Process Creation

This event will look for any processes that have been created. This event will use the `CommandLine` and `Image` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<ProcessCreate onmatch="exclude">  
		<CommandLine condition="is">C:\Windows\system32\svchost.exe -k appmodel -p -s camsvc</CommandLine>  
	</ProcessCreate>  
</RuleGroup>
```

>Event ID 3: Network Connection

The network connection event will look for events that occur remotely. This will include files and sources of suspicious binaries as well as opened ports. This event will use the `Image` and `DestinationPort` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<NetworkConnect onmatch="include">  
		<Image condition="image">nmap.exe</Image>  
		<DestinationPort name="Alert,Metasploit" condition="is">4444</DestinationPort> 
	</NetworkConnect>  
</RuleGroup>
```

>Event ID 7: Image Loaded

This event will look for DLLs loaded by processes, which is useful when hunting for DLL Injection and DLL Hijacking attacks. It is recommended to exercise caution when using this Event ID as it causes a high system load. This event will use the `Image`, `Signed`, `ImageLoaded`, and `Signature` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<ImageLoad onmatch="include">  
		<ImageLoaded condition="contains">\Temp\</ImageLoaded>  
	</ImageLoad>  
</RuleGroup>
```

>Event ID 8: CreateRemoteThread

The CreateRemoteThread Event ID will monitor for processes injecting code into other processes. The CreateRemoteThread function is used for legitimate tasks and applications. However, it could be used by malware to hide malicious activity. This event will use the `SourceImage`, `TargetImage`, `StartAddress`, and `StartFunction` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<CreateRemoteThread onmatch="include">  
		<StartAddress name="Alert,Cobalt Strike" condition="end with">0B80</StartAddress>  
		<SourceImage condition="contains">\</SourceImage>  
	</CreateRemoteThread>  
</RuleGroup>
```

The above code snippet shows two ways of monitoring for CreateRemoteThread. The first method will look at the memory address for a specific ending condition which could be an indicator of a Cobalt Strike beacon. The second method will look for injected processes that do not have a parent process. This should be considered an anomaly and require further investigation.

>Event ID 11: File Created

This event ID is will log events when files are created or overwritten the endpoint. This could be used to identify file names and signatures of files that are written to disk. This event uses `TargetFilename` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<FileCreate onmatch="include">  
		<TargetFilename name="Alert,Ransomware" condition="contains">HELP_TO_SAVE_FILES</TargetFilename>  
	</FileCreate>  
</RuleGroup>
```

>Event ID 12 /13 / 14: Registry Event

This event looks for changes or modifications to the registry. Malicious activity from the registry can include persistence and credential abuse. This event uses `TargetObject` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<RegistryEvent onmatch="include">  
		<TargetObject name="T1484" condition="contains">Windows\System\Scripts</TargetObject>  
	</RegistryEvent>  
</RuleGroup>
```

>Event ID 15: FileCreateStreamHash

This event will look for any files created in an alternate data stream. This is a common technique used by adversaries to hide malware. This event uses `TargetFilename` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<FileCreateStreamHash onmatch="include">  
		<TargetFilename condition="end with">.hta</TargetFilename>  
	</FileCreateStreamHash>  
</RuleGroup>
```

>Event ID 22: DNS Event

This event will log all DNS queries and events for analysis. The most common way to deal with these events is to exclude all trusted domains that you know will be very common "noise" in your environment. Once you get rid of the noise you can then look for DNS anomalies. This event uses `QueryName` XML tags.

```
<RuleGroup name="" groupRelation="or">  
	<DnsQuery onmatch="exclude">  
		<QueryName condition="end with">.microsoft.com</QueryName>  
	</DnsQuery>  
</RuleGroup>
```

The above code snippet will get exclude any DNS events with the .microsoft.com query. This will get rid of the noise that you see within the environment.

#### Installing Sysmon

PowerShell command: `Download-SysInternalsTools C:\Sysinternals`

Suggested config files: `SwiftOnSecurity sysmon-config`, ` ION-Storm config file`.

#### Starting Sysmon

The below command it will execute the Sysmon binary, accept the end-user license agreement, and use SwiftOnSecurity config file.

`Sysmon.exe -accepteula -i ..\Configuration\swift.xml`

#### Sysmon Best Practices

* Exclude > Include
Prioritize excluding events rather than including events.

* CLI gives you futher control
CLI gives you the most control and filtering allowing for further granular control. You can use `Get-WinEvent` or `wevtutil`.

* Know your environment implementation
You should have a firm understanding of the network or environment you are working within to fully understand what is normal and what is suspicious in order to effectively craft your rules.

#### Filtering Events with PowerShell

To view and filter events with PowerShell we will be using `Get-WinEvent` along with `XPath` queries. We can use any XPath queries that can be found in the XML view of events. We will be using `wevutil.exe` to view events once filtered. 

>Basic Filters

Filter by Event ID: `*/System/EventID=<ID>`
Filter by XML Attribute/Name: `*/EventData/Data[@Name="<XML Attribute/Name>"]
Filter by Event Data: `*/EventData/Data=<Data>`

Example with various atttributes:
`Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=4444'`

> [!hint] Useful Commands
> -Oldest -> Displays the oldest events.
> -MaxEvents 1 -> Dsiplays only 1 event.
> -Property * -> Displays all the properties of an event.

#### Hunting Metasploit

Look for network connections that originate from suspicious ports such as 4444 and 5555.

Malware Common Ports:
https://docs.google.com/spreadsheets/d/17pSTDNpa0sf6pHeRhusvWG6rThciE8CsXTSlDUAZDyo

> Hunting Network Connection

The code snippet below will use event ID 3 along with the destination port to identify active connections specifically connections on port 4444 and 5555.

```
<RuleGroup name="" groupRelation="or">
	<NetworkConnect onmatch="include">
		<DestinationPort condition="is">4444</DestinationPort>
		<DestinationPort condition="is">5555</DestinationPort>
	</NetworkConnect>
</RuleGroup>
```

Example of a basic  Metasploit payload:![[Pasted image 20250426202904.png]]

>Hunting for Open Ports with PowerShell

