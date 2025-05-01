Network monitorin tool.

> Zeek vs Snort

| **Tool**            | **Zeek**                                                                                                                                                                                                           | **Snort**                                                                                                                                               |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Capabilities**    | NSM and IDS framework. It is heavily focused on network analysis. It is more focused on specific threats to trigger alerts. The detection mechanism is focused on events.                                          | An IDS/IPS system. It is heavily focused on signatures to detect vulnerabilities. The detection mechanism is focused on signature patterns and packets. |
| **Cons**            | Hard to use.<br><br>The analysis is done out of the Zeek, manually or by automation.                                                                                                                               | Hard to detect complex threats.                                                                                                                         |
| **Pros**            | It provides in-depth traffic visibility.<br><br>Useful for threat hunting.<br><br>Ability to detect complex threats.<br><br>It has a scripting language and supports event correlation. <br><br>Easy to read logs. | Easy to write rules.<br><br>Cisco supported rules.<br><br>Community support.                                                                            |
| **Common Use Case** | Network monitoring.  <br>In-depth traffic investigation.  <br>Intrusion detecting in chained events.                                                                                                               | Intrusion detection and prevention.  <br>Stop known attacks/threats.                                                                                    |

The default log path is: `/opt/zeek/logs/`

To run Zeek as service (to listen to the live network traffic): `sudo zeekctl`
Commands:
* start
* stop
* status

To proccess pcap files: `zeek -C -r sample.pcap`

| **Parameter** | **Description**                           |
| ------------- | ----------------------------------------- |
| **-r**        | Reading option, read/process a pcap file. |
| **-C**        | Ignoring checksum errors.                 |
| **-v**        | Version information.                      |
> Zeek Logs

| Category             | Description                                                              | **Log Files**                                                                                                                                                                                                                                                                                                                      |
| -------------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Network              | Network protocol logs.                                                   | _conn.log, dce_rpc.log, dhcp.log, dnp3.log, dns.log, ftp.log, http.log, irc.log, kerberos.log, modbus.log, modbus_register_change.log, mysql.log, ntlm.log, ntp.log, radius.log, rdp.log, rfb.log, sip.log, smb_cmd.log, smb_files.log, smb_mapping.log, smtp.log, snmp.log, socks.log, ssh.log, ssl.log, syslog.log, tunnel.log._ |
| Files                | File analysis result logs.                                               | _files.log, ocsp.log, pe.log, x509.log._                                                                                                                                                                                                                                                                                           |
| NetControl           | Network control and flow logs.                                           | _netcontrol.log, netcontrol_drop.log, netcontrol_shunt.log, netcontrol_catch_release.log, openflow.log._                                                                                                                                                                                                                           |
| Detection            | Detection and possible indicator logs.                                   | _intel.log, notice.log, notice_alarm.log, signatures.log, traceroute.log._                                                                                                                                                                                                                                                         |
| Network Observations | Network flow logs.                                                       | _known_certs.log, known_hosts.log, known_modbus.log, known_services.log, software.log._                                                                                                                                                                                                                                            |
| Miscellaneous        | Additional logs cover external alerts, inputs and failures.              | _barnyard2.log, dpd.log, unified2.log, unknown_protocols.log, weird.log, weird_stats.log._                                                                                                                                                                                                                                         |
| Zeek Diagnostic      | Zeek diagnostic logs cover system messages, actions and some statistics. | _broker.log, capture_loss.log, cluster.log, config.log, loaded_scripts.log, packet_filter.log, print.log, prof.log, reporter.log, stats.log, stderr.log, stdout.log._                                                                                                                                                              |

Most commonly used logs

| **Update Frequency** | **Log Name  <br>**   | **Description**                                 |
| -------------------- | -------------------- | ----------------------------------------------- |
| **Daily**            | _known_hosts.log_    | List of hosts that completed TCP handshakes.    |
| **Daily**            | _known_services.log_ | List of services used by hosts.                 |
| **Daily**            | _known_certs.log_    | List of SSL certificates.                       |
| **Daily**            | _software.log_       | List of software used on the network.           |
| **Per Session**      | _notice.log_         | Anomalies detected by Zeek.                     |
| **Per Session**      | _intel.log_          | Traffic contains malicious patterns/indicators. |
| Per Session          | _signatures.log_     | List of triggered signatures.                   |

Extract the event of interest fields with `zeek-cut`

> Examples

Find the number of unique DNS queries from dns.log:
`cat dns.log | zeek-cut query | uniq | wc -l`

Find the longest connection duration in conn.log:
`cat conn.log | zeek-cut duration | sort -nr | head`

> Zeek Signatures

Composed of 3 logical paths: `signature id, conditions and actions`

*Signature ID* - Unique signature name

*Conditions* - Header: Filetering the packet headers for specific source anda destiantion addresses, protocol and port numers.
           - Content: Filtering the packet payload for specific value/pattern.

*Action*     - Default action: Create the "signatures.log" file in case of a signature match.
        - Additional action: Trigger a Zeek script.


| Condition Field               | Available Filters                                                                                                                                                                                                                                                                                                                                                |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Header                        | src-ip: Source IP.<br><br>dst-ip: Destination IP.<br><br>src-port: Source port.<br><br>dst-port: Destination port.<br><br>ip-proto: Target protocol. Supported protocols; TCP, UDP, ICMP, ICMP6, IP, IP6                                                                                                                                                         |
| Content                       | **payload:** Packet payload.  <br>**http-request:** Decoded HTTP requests.  <br>**http-request-header:** Client-side HTTP headers.  <br>**http-request-body:** Client-side HTTP request bodys.  <br>**http-reply-header:** Server-side HTTP headers.  <br>**http-reply-body:** Server-side HTTP request bodys.  <br>**ftp:** Command line input of FTP sessions. |
| **Context**                   | **same-ip:** Filtering the source and destination addresses for duplication.                                                                                                                                                                                                                                                                                     |
| Action                        | **event:** Signature match message.                                                                                                                                                                                                                                                                                                                              |
| **Comparison  <br>Operators** | **==**, **!=**, **<**, **<=**, **>**, **>=**                                                                                                                                                                                                                                                                                                                     |
| **NOTE!**                     | Filters accept string, numeric and regex values.                                                                                                                                                                                                                                                                                                                 |

> Example of a simple signature to detect HTTP cleartext passwords
```
signature http-password { 
	ip-proto == tcp 
	dst-port == 80 
	payload /.*password.*/ event "Password Found!" 
} 
# signature: Signature name. 
# ip-proto: Filtering TCP connection. 
# dst-port: Filtering destination port 80. 
# payload: Filtering the "password" phrase. 
# event: Signature match message.
```

Executing the signature:
`zeek -C -r http.pcap -s http-password.sig`

> [!hint]
> Para saber los campos que tiene cada archivo log podemos utilizar por ej: cat conn.log | grep "#fields"

> Zeek Scripts

| Zeek has base scripts installed by default, and these are not intended to be modified.                                                                                                                   | These scripts are located in "/opt/zeek/share/zeek/base".                    |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| User-generated or modified scripts should be located in a specific path.                                                                                                                                 | These scripts are located in  <br>**"/opt/zeek/share/zeek/site".**           |
| Policy scripts are located in a specific path.                                                                                                                                                           | These scripts are located in **"/opt/zeek/share/zeek/policy"**.              |
| Like Snort, to automatically load/use a script in live sniffing mode, you must identify the script in the Zeek configuration file. You can also use a script for a single run, just like the signatures. | The configuration file is located in "/opt/zeek/share/zeek/site/local.zeek". |
To call scripts in live monitoring mode by loading them with the command `load @/script/path` or `load @script-name` in local.zeek file.

Example and execution:
```
event dhcp_message (c: connection, is_orig: bool, msg: DHCP::Msg, options: DHCP::Options) 
{ 
print options$host_name; 
}
```

`zeek -C -r example.pcap dhcp-hostname.zeek`

> Frameworks

Zeek has 15+ frameworks that help analysts to discover the different events of interest
`@load $PATH/base/frameworks/framework-name`

File analysis framework (MD5, SHA1 and SHA256)
```
zeek -C -r case1.pcap /opt/zeek/share/zeek/policy/frameworks/files/hash-all-files.zeek
```

Extract files framework  (extract files tranferred)
```
zeek -C -r case1.pcap /opt/zeek/share/zeek/policy/frameworks/files/extract-all-files.zeek
```

Intelligence framework (correlate events and idnetify anomalies)
```
zeek -C -r case1.pcap intelligence-demo.zeek
```




