Translate Wireshark GUI features to the TShark CLI and investigate events of interest.

| **Parameter** | **Purpose**                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| --color       | - Wireshark-like colourised output.<br>- `tshark --color`                                                                                                                                                                                                                                                                                                                                                      |
| -z            | - Statistics<br>- There are multiple options available under this parameter. You can view the available filters under this parameter with:<br><br>- `tshark -z help`<br><br>- Sample usage.<br><br>- `tshark -z filter`<br><br>- Each time you filter the statistics, packets are shown first, then the statistics provided. You can suppress packets and focus on the statistics by using the `-q` parameter. |
#### Statistics | Protocol Hierarchy

Displays the protocols used, frame numbers, and size of packets in a tree view based on packet numbers.

`tshark -r demo.pcapng -z io,phs -q`

**-z io,phs** Generates Protocol Hierarchy Statistics
**-q** quiet mode

#### Statistics | Packet Lengths Tree

Overview the general distribution of packets by size in a tree view.

`tshark -r demo.pcang -z plen,tree -q`

#### Statistics | Endpoints

Overview the unique endpoints. It also shows the nyumber of packets associated with each endpoint.

| **Filter** | **Purpose**                                       |
| ---------- | ------------------------------------------------- |
| eth        | - Ethernet addresses                              |
| ip         | - IPv4 addresses                                  |
| ipv6       | - IPv6 addresses                                  |
| tcp        | - TCP addresses<br>- Valid for both IPv4 and IPv6 |
| udp        | - UDP addresses<br>- Valid for both IPv4 and IPv6 |
| wlan       | - IEEE 802.11 addresses                           |
`tshark -r demo.pcapng -z endpoints,ip -q`

#### Statistics | Conversations

Overview the traffic flow between two particular connection points.

`tshark -r demo.pcapng -z conv,ip -q`

#### Statistics | Expert Info

Helps analysts to view the automatic comments provided by Wireshark.

`tshark -r demo.pcapng -z expert -q`

#### Statistics | IPv4 and IPv6

Provides statistics on IPv4 and IPv6 packets.

`tshark -r demo.pcapng -z ptype,tree -q`

You can filter all IP addresses using the parameters given below.

- **IPv4:** `-z ip_hosts,tree -q`
- **IPv6:** `-z ipv6_hosts,tree -q`

You can filter all source and destination addresses using the parameters given below.

- IPv4: `-z ip_srcdst,tree -q`
- IPv6: `-z ipv6_srcdst,tree -q`

You can filter all outgoing traffic by using the parameters given below.

- IPv4: `-z dests,tree -q`
- IPv6: `-z ipv6_dests,tree -q`

#### Statistics | DNS

Provides statistics on DNS packets by summarising the available info.

`tshark -r demo.pcapng -z dns,tree -q`

#### Statistics | HTTP

provides statistics on HTTP packets by summarising the load distribution, requests, packets, and status info. You can filter the packets and view the details using the parameters given below.  

- **Packet and status counter for HTTP:** `-z http,tree -q`
- **Packet and status counter for HTTP2:** `-z http2,tree -q`
- **Load distribution:** `-z http_srv,tree -q`
- **Requests:** `-z http_req,tree -q`
- **Requests and responses:** `-z http_seq,tree -q`

![[Pasted image 20250410232921.png]]

#### Follow Stream

This option helps analysts to follow traffic streams similar to Wireshark.

| **Main Parameter** | **Protocol**                        | **View Mode**    | **Stream Number**    | **Additional Parameter** |
| ------------------ | ----------------------------------- | ---------------- | -------------------- | ------------------------ |
| -z follow          | - TCP<br>- UDP<br>- HTTP<br>- HTTP2 | - HEX<br>- ASCII | 0 \| 1 \| 2 \| 3 ... | -q                       |
- **TCP Streams:** `-z follow,tcp,ascii,0 -q`
- **UDP Streams:** `-z follow,udp,ascii,0 -q`
- **HTTP Streams:** `-z follow,http,ascii,0 -q`

#### Export Objects

This option helps analysts to extract files from DICOM, HTTP, IMF, SMB and TFTP.

| **Main Parameter** | **Protocol**                                  | **Target Folder**                | **Additional Parameter** |
| ------------------ | --------------------------------------------- | -------------------------------- | ------------------------ |
| --export-objects   | - DICOM<br>- HTTP<br>- IMF<br>- SMB<br>- TFTP | Target folder to save the files. | -q                       |
![[Pasted image 20250411000652.png]]

#### Credentials

This option helps analysts to detect and collect cleartext credentials from FTP, HTTP, IMAP, POP and SMTP.

`tshark -r demo.pcapng -z credentials -q`

#### Extract Fields

 Extract specific parts of data from the packets.

| **Main Filter** | **Target Field** | **Show Field Name** |
| --------------- | ---------------- | ------------------- |
| -T fields       | -e <field name>  | -E header=y         |
> [!hint] Note
>  You need to use the -e parameter for each field you want to display.

`tshark -r demo.pcapng -T fields -e ip.src -e ip.dst -E header=y -c 5`

#### Filter: "contains"

| Filter      | contains                                                                                                                                     |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Type        | Comparison operator                                                                                                                          |
| Description | Search a value inside packets. It is case-sensitive and provides similar functionality to the "Find" option by focusing on a specific field. |
| Example     | Find all "Apache" servers.                                                                                                                   |
| Workflow    | List all HTTP packets where the "server" field contains the "Apache" keyword.                                                                |
| Usage       | `http.server contains "Apache"`                                                                                                              |

#### Filter: "matches"

| Filter      | matches                                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| Type        | Comparison operator                                                                                           |
| Description | Search a pattern of a regular expression. It is case-insensitive, and complex queries have a margin of error. |
| Example     | Find all .php and .html pages.                                                                                |
| Workflow    | List all HTTP packets where the "request method" field matches the keywords "GET" or "POST".                  |
| Usage       | `http.request.method matches "(GET\|POST)"`                                                                   |

#### Extract Hostnames

Example exrtacting hostnames from DHCP packets:

`tshark -r demo.pcapng -T fields -e dhcp.hostname | awk NF | sort -r | uniq -c | sort -r`

| **Query**                                                      | **Purpose**                                                                         |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `tshark -r hostnames.pcapng -T fields -e dhcp.option.hostname` | Main query.  <br>Extract the DHCP hostname value.                                   |
| `awk NF`                                                       | Remove empty lines.                                                                 |
| `sort -r`                                                      | Sort recursively before handling the values.                                        |
| `uniq -c`                                                      | Show unique values, but calculate and show the number of occurrences.               |
| `sort -r`                                                      | The final sort process.  <br>Show the output/results from high occurrences to less. |
#### Extract DNS Queries

`tshark -r dns-queries.pcap -T fields -e dns.qry.name | awk NF | sort -r | uniq -c | sort -r`

#### Extract User Agents

`tshark -r user-agents.pcap -T fields -e http.user_agent | awk NF | sort -r | uniq -c | sort -r`


