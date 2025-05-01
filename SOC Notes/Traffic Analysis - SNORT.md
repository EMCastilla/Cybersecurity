#THM/SOC/Traffic/Analysis


> [!hint]
> Security Information and Event Management (SIEM)
> Security Orchestration Automation and Response (SOAR)

> [!hint]
> IDS 2 types:
> * Network Intrusion Detection System (NIDS)
> * Host-based Intrusion Detection System (HIDS)

> [!hint]
> IPS 4 types:
> * Network Intrusion Prevention System (NIPS)
> * Behaviour-based Intrusion Prevention System (NBA)
> * Wireless Intrusion Prevention System (WIPS)
> * Host-based Intrusion Prevention System (HIPS)

 <h3>SNORT</h3>
 NIDS | NIPS | Open-source | Rule-based 

-c to identify the configuration file
-T for testing configuration (self-test)
```
 snort -c /etc/snort/snort.conf -T
```

<font color=7FFFD4>Snort successfully validated the configuration!</font>

>SNORT in Sniffer mode

| Parameter | Description                                                                                                                                                   |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **-v**    | Verbose. Display the TCP/IP output in the console.                                                                                                            |
| **-d**    | Display the packet data (payload).                                                                                                                            |
| **-e**    | Display the link-layer (TCP/IP/UDP/ICMP) headers.                                                                                                             |
| -**X**    | Display the full packet details in HEX.                                                                                                                       |
| -**i**    | This parameter helps to define a specific network interface to listen/sniff. Once you have multiple interfaces, you can choose a specific interface to sniff. |

>SNORT in Logger mode

| Parameter    | Description                                                                                                                                                                      |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -l           | Logger mode, target **log and alert** output directory. Default output folder is **/var/log/snort**<br><br>The default action is to dump as tcpdump format in **/var/log/snort** |
| **-K ASCII** | Log packets in ASCII format.                                                                                                                                                     |
| -r           | Reading option, read the dumped logs in Snort.                                                                                                                                   |
| **-n**       | Specify the number of packets that will process/read. Snort will stop after reading the specified number of packets.                                                             |

ASCII vs Binary format

![[Pasted image 20250302162436.png]]

Examples snort -r filters:
- `sudo snort -r logname.log -X`
- `sudo snort -r logname.log icmp`
- `sudo snort -r logname.log tcp`
- `sudo snort -r logname.log 'udp and port 53'`

>SNORT in IDS/IPS mode

Manage traffic according to user-defined rules.

| Parameter | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -c        | Defining the configuration file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| -T        | Testing the configuration file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **-N**    | Disable logging.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **-D**    | Background mode.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **-A**    | Alert modes;  <br><br>**full:** Full alert mode, providing all possible information about the alert. This one also is the default mode; once you use -A and don't specify any mode, snort uses this mode.<br><br>**fast:**  Fast mode shows the alert message, timestamp, source and destination IP, along with port numbers.<br><br>console: Provides fast style alerts on the console screen.<br><br>**cmg:** CMG style, basic header details with payload in hex and text format.<br><br>**none:** Disabling alerting. |
Alert file located in:
`/var/log/snort/alert`

>PCAP Read / Invesitage mode

| Parameter               | Description                                       |
| ----------------------- | ------------------------------------------------- |
| **-r / --pcap-single=** | Read a single pcap                                |
| **--pcap-list=""**      | Read pcaps provided in command (space separated). |
| **--pcap-show**         | Show pcap name on console during processing.      |
Example: Analysis with deafult rules on "pcap-name"
`sudo snort -c /etc/snort/snort.conf -A full -l . -r pcap-name`

>SNORT Rule Esrtucture

| Action                  | Protocol           | Source IP | Source Port | Direction | Dest. IP | Dest. Port | Options                        |
| ----------------------- | ------------------ | --------- | ----------- | --------- | -------- | ---------- | ------------------------------ |
| Alert<br>Drop<br>Reject | TCP<br>UDP<br>ICMP | ANY       | ANY         | <>        | ANY      | ANY        | Msg<br>Reference<br>Sid<br>Rev |

Example:
![[Pasted image 20250302213937.png]]

> [!hint]
> Snort2 supports only four protocols filters in the rules (IP, TCP, UDP and ICMP). However, you can detect the application flows using port numbers and options. For instance, if you want to detect FTP traffic, you cannot use the FTP keyword in the protocol field but filter the FTP traffic by investigating TCP traffic on port 21.

**IP and Port Numbers**

| IP Filetering                 | alert icmp 192.168.1.56 any <> any any (msg:"ICMP Packet From ";sid: 100001; rev:1;)                   |
| ----------------------------- | ------------------------------------------------------------------------------------------------------ |
| Filter an IP range            | alert icmp 192.168.1.56/24 any <> any any (msg:"ICMP Packet Found";sid: 100001; rev:1;)                |
| Fileter multiple IP ranges    | alert icmp [192.168.1.56/24, 10.1.1.0/24] any <> any any (msg:"ICMP Packet Found";sid: 100001; rev:1;) |
| Exclude IP addresses / ranges | alert icmp !192.168.1.56/24 any <> any any (msg:"ICMP Packet Found";sid: 100001; rev:1;)               |
| Port Filtering                | alert tcp any any <> any 21 (msg:"FTP Port Activity Detected";sid: 100001; rev:1;)                     |
| Exclude a specific port       | alert tcp any any <> any !21 (msg:"";sid: 100001; rev:1;)                                              |
| Filter a port range (Type 1)  | alert tcp any any <> any 1:1024 (msg:"TCP 1-1024 System Port Activity"; sid: 100001; rev:1;)           |
| Filter a port range (Type 2)  | alert tcp any any <> any :1024 (msg:"TCP 0-1024 System Port Activity"; sid: 100001; rev:1;             |
| Filter a port range (Type 3)  | alert tcp any any <> any 1025: (msg:"TCP Non-system Port Activity"; sid: 100001; rev:1;                |
| Filter a port range (Type 4)  | alert tcp any any <> any [21,23] (msg:"FTP and Telnet Port Activity Detected"; sid: 100001; rev:1;     |

> [!hint] Direction
> The direction operator indicates the traffic flow to be filtered by Snort. The left side of the rule shows the source, and the right side shows the destination.
> - **->** Source to destination flow.
> -  **<>** Bidirectional flow
> Note that there is no "<-" operator in Snort

 > 3 Main Rule in Snort

* General Rule Options - Fundamental rule options for Snort.
* Payload Rule Options - Rule options that help to investigate the payload data (detect specific payload patterns).
* Non-Payload Rule Options - Rule options that focus on non-payload data (identify network issues).

##### General Rule options

| Msg       | The message field is a basic prompt and quick identifier of the rule. Once the rule is triggered, the message filed will appear in the console or log. Usually, the message part is a one-liner that summarises the event.                                                                                                                                                                                                                                                                                                                                |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sid       | Snort rule IDs (SID) come with a pre-defined scope, and each rule must have a SID in a proper format. There are three different scopes for SIDs shown below.<br><br>- <100: Reserved rules<br>- 100-999,999: Rules came with the build.<br>- >=1,000,000: Rules created by user.<br><br>Briefly, the rules we will create should have sid greater than 100.000.000. Another important point is; SIDs should not overlap, and each id must be unique.                                                                                                      |
| Reference | Each rule can have additional information or reference to explain the purpose of the rule or threat pattern. That could be a Common Vulnerabilities and Exposures (CVE) id or external information. Having references for the rules will always help analysts during the alert and incident investigation.                                                                                                                                                                                                                                                |
| Rev       | Snort rules can be modified and updated for performance and efficiency issues. Rev option help analysts to have the revision information of each rule. Therefore, it will be easy to understand rule improvements. Each rule has its unique rev number, and there is no auto-backup feature on the rule history. Analysts should keep the rule history themselves. Rev option is only an indicator of how many times the rule had revisions.<br><br>alert icmp any any <> any any (msg: "ICMP Packet Found"; sid: 100001; reference:cve,CVE-XXXX; rev:1;) |

##### Payload Detection Rule Options

| Content      | Payload data. It matches specific payload data by ASCII, HEX or both. It is possible to use this option multiple times in a single rule. However, the more you create specific pattern match features, the more it takes time to investigate a packet.<br><br>Following rules will create an alert for each HTTP packet containing the keyword "GET". This rule option is case sensitive!<br><br>- ASCII mode - alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; sid: 100001; rev:1;)<br>- HEX mode - alert tcp any any <> any 80  (msg: "GET Request Found"; content:"\|47 45 54\|"; sid: 100001; rev:1;)                                                                                                             |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nocase       | Disabling case sensitivity. Used for enhancing the content searches.<br><br>alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; nocase; sid: 100001; rev:1;)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Fast_pattern | Prioritise content search to speed up the payload search operation. By default, Snort uses the biggest content and evaluates it against the rules. "fast_pattern" option helps you select the initial packet match with the specific value for further investigation. This option always works case insensitive and can be used once per rule. Note that this option is required when using multiple "content" options. <br><br>The following rule has two content options, and the fast_pattern option tells to snort to use the first content option (in this case, "GET") for the initial packet match.  <br><br>alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; fast_pattern; content:"www";  sid:100001; rev:1;) |

##### Non-Payload Detection Rule Options

| ID     | Filtering the IP id field.<br><br>alert tcp any any <> any any (msg: "ID TEST"; id:123456; sid: 100001; rev:1;)                                                                                  |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Flags  | Filtering the TCP flags.<br><br>- F - FIN<br>- S - SYN<br>- R - RST<br>- P - PSH<br>- A - ACK<br>- U - URG<br><br>alert tcp any any <> any any (msg: "FLAG TEST"; flags:S;  sid: 100001; rev:1;) |
| Dsize  | Filtering the packet payload size.<br><br>- dsize:min<>max;<br>- dsize:>100<br>- dsize:<100<br><br>alert ip any any <> any any (msg: "SEQ TEST"; dsize:100<>300;  sid: 100001; rev:1;)           |
| Sameip | Filtering the source and destination IP addresses for duplication.<br><br>alert ip any any <> any any (msg: "SAME-IP TEST";  sameip; sid: 100001; rev:1;)                                        |

SNORT local rules location:
```
 /etc/snort/rules/local.rules
 ```

