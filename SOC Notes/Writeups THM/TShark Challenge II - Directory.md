Investigate the DNS queries.  
Investigate the domains by using VirusTotal.  
According to VirusTotal, there is a domain marked as malicious/suspicious.

1 - What is the name of the malicious/suspicious domain?
Enter your answer in a defanged format.

`tshark -r directory-curiosity.pcap -T fields -e dns.qry.name -e ip | awk NF | sort | uniq -c`

`jx2-bavuong[.]com`

2 - What is the total number of HTTP requests sent to the malicious domain?

`tshark -r directory-curiosity.pcap -z http_req,tree -q | grep jx2-bavuong.com`

`14`

3 - What is the IP address associated with the malicious domain?
Enter your answer in a defanged format.

`tshark -r directory-curiosity.pcap -T fields -e dns.qry.name -e dns.a | awk NF | sort | uniq -c | grep jx2`

`141[.]164[.]41[.]174`

4 - What is the server info of the suspicious domain?

`tshark -r directory-curiosity.pcap -Y "ip.addr == 141.164.41.174" -T fields -e ip.dst -e http.server | sort | uniq -c`

`Apache/2.2.11 (Win32) DAV/2 mod_ssl/2.2.11 OpenSSL/0.9.8i PHP/5.2.9`

5 - Follow the "first TCP stream" in "ASCII".  
Investigate the output carefully.
What is the number of listed files?

`tshark -r directory-curiosity.pcap -Y "ip.addr == 141.164.41.174" -T fields -e ip.dst -e http.server -e tcp.stream
`
`tshark -r directory-curiosity.pcap -z follow,tcp,ascii,0 -q
`
`3`

6 - What is the filename of the first file?
Enter your answer in a defanged format.

`...` ->This step involves the same commands as before.

`123[.]php`

7 - Export all HTTP traffic objects.  
What is the name of the downloaded executable file?
Enter your answer in a defanged format.

`...`

`vlauto[.]exe`

8 - What is the SHA256 value of the malicious file?

`tshark -r directory-curiosity.pcap --export-objects http,. -q`
*utilizo . para que descarge todo en la ubicacion actual*

`sha256sum vlauto.exe`

`b4851333efaf399889456f78eac0fd532e9d8791b23a86a19402c1164aed20de`


9 - Search the SHA256 value of the file on VirtusTotal.  
What is the "PEiD packer" value?

`VirusTotal -> Details -> PEiD packer`

`NET executable`

10 - Search the SHA256 value of the file on VirtusTotal.  
What does the "Lastline Sandbox" flag this as?

`VirusTotal -> Behaviour -> The sandbox Lastline flags this file as: MALWARE TROJAN`
