Investigate the contacted domains.  
Investigate the domains by using VirusTotal.  
According to VirusTotal, there is a domain marked as malicious/suspicious.  
  
1 - What is the full URL of the malicious/suspicious domain address?
Enter your answer in defanged format.

`tshark -r teamwork.pcap -T fields -e dns.qry.name -E header=y | awk NF | sort | uniq -c`

`hxxp[://]www[.]paypal[.]com4uswebappsresetaccountrecovery[.]timeseaways[.]com/`

2 - When was the URL of the malicious/suspicious domain address first submitted to VirusTotal?

`2017-04-17 22:52:53 UTC`

3 - Which known service was the domain trying to impersonate?

`PayPal`

4 - What is the IP address of the malicious domain?
Enter your answer in defanged format.

`184[.]154[.]127[.]226`

5 - What is the email address that was used?
Enter your answer in defanged format. (**format:** aaa[at]bbb[.]ccc)

`tshark -r teamwork.pcap -Y "http.request.method == POST" -V | grep @`

`johnny5alive[at]gmail[.]com`

``