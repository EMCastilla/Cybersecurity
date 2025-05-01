> Scenario 1 | Bruteforce

First of all, start Snort in sniffer mode and try to figure out the attack source, service and port.

Then, write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop!

`Stop the attack and get the flag (which will appear on your Desktop)`

1 - Run snort in sniffer mode to collect data.
`snort -v -l .`

2 - Inspect log file.
`snort -r snort.log -X`

3 - Suspicious port 22 activity. Look for packets with that address.
`snort -r snort.log -X | grep :22`

4 - Suspicious IP `10.10.245.36`

5 - Create a rule to block the traffic
`drop tcp any 22 <> any any (msg:"SSH connection attempted"; sid:100001; rev:1;)`

6 - Run snort in IPS mode (console model)
`sudo snort -c local.rules -q -Q --daq afpacket -i eth0:eth1 -A console`

7 - If console mode was ok the run the full mode
`sudo snort -c local.rules -q -Q --daq afpacket -i eth0:eth1 -A full`

8 - Get the flag from flag.txt in Desktop.
`THM{81b7fef657f8aaa6e4e200d616738254}`

9 - The name of the service under attack is:
`SSH`

10 - What is the used protocol/port in the attack?
`TCP/22`

> Scenario 2 | Reverse-Shell

First of all, start Snort in sniffer mode and try to figure out the attack source, service and port.  
  
Then, write an IPS rule and run Snort in IPS mode to stop the brute-force attack. Once you stop the attack properly, you will have the flag on the desktop!

`Stop the attack and get the flag (which will appear on your Desktop)`

1 - Run snort in sniffer mode to collect data.
`snort -v -l .`

2 - Inspect log file.
`snort -r snort.log -X`

3 - Suspicious port 22 activity. Look for packets with that address.
`snort -r snort.log -X | grep :4444`

4 - Create a rule to block the traffic
`drop tcp any 4444 <> any any (msg:"Reverse-Shell detected"; sid:100001; rev:1;)`

5 - Run snort in IPS mode (console model)
`sudo snort -c local.rules -q -Q --daq afpacket -i eth0:eth1 -A console`

6 - If console mode was ok the run the full mode
`sudo snort -c local.rules -q -Q --daq afpacket -i eth0:eth1 -A full`

7 - Get the flag from flag.txt in Desktop.
`THM{0ead8c494861079b1b74ec2380d2cd24}`

8 - What is the used protocol/port in the attack?
`TCP/4444`

9 - Which tool is highly associated with this specific port number?
`Metasploit`
