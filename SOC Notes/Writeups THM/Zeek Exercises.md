> Anomalous DNS

An alert triggered: "Anomalous DNS Activity"
Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive.

1 - Investigate the **dns-tunneling.pcap** file. Investigate the **dns.log** file. What is the number of DNS records linked to the IPv6 address?
`cat dns.log | zeek-cut qtype_name | grep AAAA | wc`
`320`

2 - Investigate the **conn.log** file. What is the longest connection duration?
`zeek conn.log | zeek-cut duration | sort`
`9.420791`

3 - Investigate the **dns.log** file. Filter all unique DNS queries. What is the number of unique domain queries?
cat 
`cat dns.log | zeek-cut query |rev | cut -d '.' -f 1-2 | rev | sort | uniq`
`6`

4 - There are a massive amount of DNS queries sent to the same domain. This is abnormal. Let's find out which hosts are involved in this activity. Investigate the **conn.log** file. What is the IP address of the source host?
`cat conn.log | zeek-cut id.orig_h | uniq -c`
`10.20.57.3`


> Phishin

An alert triggered: "Phishing Attempt".

The case was assigned to you. Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive.

1 - Investigate the logs. What is the suspicious source address? Enter your answer in **defanged format**.
`cat conn.log | zeek-cut id.orig_h | uniq`
`10[.]6[.]27[.]102`

2 - Investigate the **http.log** file. Which domain address were the malicious files downloaded from? Enter your answer in defanged format.
`cat http.log | zeek-cut host`
`smart-fax[.]com`

3 - Investigate the malicious document in VirusTotal. What kind of file is associated with the malicious document?
`md5sum extract-1561667889.703239-HTTP-FB5o2Hcauv7vpQ8y3`
`Virutotal -> Relations -> VBA`

4 - Investigate the extracted malicious **.exe** file. What is the given file name in Virustotal?
`md5sum extract-1561667899.060086-HTTP-FOghls3WpIjKpvXaEl`
`Virustotal -> PleaseWaitWindow.exe`

5 - Investigate the malicious **.exe** file in VirusTotal. What is the contacted domain name? Enter your answer in **defanged format**.
`Virustotal -> Relations -> Contacted Domains`
`hopto[.]org`

6 - Investigate the http.log file. What is the request name of the downloaded malicious **.exe** file?
`cat http.log | zeek-cut uri | grep .*exe`
`knr.exe`

>Log4J

**An alert triggered:** "Log4J Exploitation Attempt".

The case was assigned to you. Inspect the PCAP and retrieve the artefacts to confirm this alert is a true positive.

1 - Investigate the log4shell.pcapng file with detection-log4j.zeek script. Investigate the signature.log file. What is the number of signature hits?
`cat signatures.log | zeek-cut sig_id`
`3`

2 - Investigate the **http.log** file. Which tool is used for scanning?
`cat http.log | zeek-cut user_agent`
`Nmap`

3 - Investigate the **http.log** file. What is the extension of the exploit file?
`cat http.log | zeek-cut uri | uniq`
`.class`

4 - Investigate the log4j.log file. Decode the base64 commands. What is the name of the created file?
`cat log4j.log | zeek-cut value | uniq`
`echo dG91Y2ggL3RtcC9wd25lZAo= | base64 -d`
`pwned`
