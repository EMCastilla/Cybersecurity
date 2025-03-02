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

SNORT in Sniffer mode

| Parameter | Description                                                                                                                                                   |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **-v**    | Verbose. Display the TCP/IP output in the console.                                                                                                            |
| **-d**    | Display the packet data (payload).                                                                                                                            |
| **-e**    | Display the link-layer (TCP/IP/UDP/ICMP) headers.                                                                                                             |
| -**X**    | Display the full packet details in HEX.                                                                                                                       |
| -**i**    | This parameter helps to define a specific network interface to listen/sniff. Once you have multiple interfaces, you can choose a specific interface to sniff. |








