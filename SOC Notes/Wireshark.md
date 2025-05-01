 Traffic analyser tool
 
![[Pasted image 20250316164519.png]]

| **Toolbar**                       | The main toolbar contains multiple menus and shortcuts for packet sniffing and processing, including filtering, sorting, summarising, exporting and merging.                                                                         |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Display Filter Bar**            | The main query and filtering section.                                                                                                                                                                                                |
| **Recent Files**                  | List of the recently investigated files. You can recall listed files with a double-click.                                                                                                                                            |
| **Capture Filter and Interfaces** | Capture filters and available sniffing points (network interfaces).  The network interface is the connection point between a computer and a network. The software connection (e.g., lo, eth0 and ens33) enables networking hardware. |
| **Status Bar**                    | Tool status, profile and numeric packet information.                                                                                                                                                                                 |
![[Pasted image 20250316165450.png]]

##### Traffic Sniffing  

You can use the blue **"shark button"** to start network sniffing (capturing traffic), the red button will stop the sniffing, and the green button will restart the sniffing process. The status bar will also provide the used sniffing interface and the number of collected packets.

##### Merge PCAP Files  

Wireshark can combine two pcap files into one single file. You can use the **"File --> Merge"** menu path to merge a pcap with the processed one. When you choose the second file, Wireshark will show the total number of packets in the selected file. Once you click "open", it will merge the existing pcap file with the chosen one and create a new pcap file. Note that you need to save the "merged" pcap file before working on it.

##### View File Details  

Knowing the file details is helpful. Especially when working with multiple pcap files, sometimes you will need to know and recall the file details (File hash, capture time, capture file comments, interface and statistics) to identify the file, classify and prioritise it. You can view the details by following "**Statistics --> Capture File Properties"** or by clicking the **"pcap icon located on the left bottom"** of the window.

![[Pasted image 20250316170234.png]]

Seven distinct layers to the packet: frame/packet, source [MAC], source [IP], protocol, protocol errors, application protocol, and application data:

* **The Frame (Layer 1):** This will show you what frame/packet you are looking at and details specific to the Physical layer of the OSI model.
* **Source [MAC] (Layer 2):** This will show you the source and destination MAC Addresses; from the Data Link layer of the OSI model.
* **Source [IP] (Layer 3):** This will show you the source and destination IPv4 Addresses; from the Network layer of the OSI model.
* **Protocol (Layer 4):** This will show you details of the protocol used (UDP/TCP) and source and destination ports; from the Transport layer of the OSI model.
* **Protocol Errors:** This continuation of the 4th layer shows specific segments from TCP that needed to be reassembled.
* **Application Protocol (Layer 5):** This will show details specific to the protocol used, such as HTTP, FTP,  and SMB. From the Application layer of the OSI model.
* **Application Data:** This extension of the 5th layer can show the application-specific data.

![[Pasted image 20250316180726.png]]

> [!hint] Useful shortcuts
> Ctrl + G -> Go to packet
> Ctrl + F -> Find Packets (content)

##### Export Objects (Files)

Wireshark can extract files tranferred through the wire. Exporting objects are available only for selected protocol's streams (DICOM, HTTP, IMF, SMB and TFTP).
***FIle -> Export Objects***
#### Expert Info (Analyze -> Expert Information)

Wireshark detects specific states of protocols to help to spot possible anomalies and problems.

| **Severity** | **Colour** | **Info**                                                 |
| ------------ | ---------- | -------------------------------------------------------- |
| **Chat**     | **Blue**   | Information on usual workflow.                           |
| **Note**     | **Cyan**   | Notable events like application error codes.             |
| **Warn**     | **Yellow** | Warnings like unusual error codes or problem statements. |
| **Error**    | **Red**    | Problems like malformed packets.                         |
 
#### Packet Filtering

`Apply as Filter (Anlayze -> Applys as Filter):` You can select content from a package and then wireshark will generate the required filter query.

`Conversation Filter (Analyze -> Conversation Filter):` This option will help you to investigate all the packets linked by focusion on IP addresses and port numbers and hide the rest of the packages.

`Colourise Conversation (View -> Coulurise Conversation):`It highlights the linked packets without applying a display filter.

`Prepare as Filter (Analyze -> Prepare as filter):`This option doesn't apply the filter after the choice. It adds required query to the pane and waits for the execution.

`Apply as Column (Analyze -> Apply as Column):` You can select information about a packet and the add it as a column in the packet pane list.

`Follow Stream (Analyze -> Follow TCP/UDP/HTTP Stream):` Reconstruct the stream and view the raw traffic as it is presented at the application level.
You will need to use the "**X** **button**" located on the right upper side of the display filter bar to remove the display filter and view all available packets in the capture file.

#### Statistics

This menu provides multiple statistics options ready to investigate to help users see the big picture in terms of the scope of the traffic, available protocols, endpoints and conversations, and some protocol-specific details like DHCP, DNS and HTTP/2.

> Resolved Addresses

 Identify IP addresses and DNS names available in the capture file by providing the list of the resolved addresses and their hostnames

> Protocol Hierarchy

This option breaks down all available protocols from the capture file and helps analysts view the protocols in a tree view based on packet counters and percentages

> Conversations

Conversation represents traffic between two specific endpoints. This option provides the list of the conversations in five base formats; ethernet, IPv4, IPv6, TCP and UDP. Thus analysts can identify all conversations and contact endpoints for the event of interest.

> Endpoints

This option provides unique information for a single information field (Ethernet, IPv4, IPv6, TCP and UDP ). Thus analysts can identify the unique endpoints in the capture file and use it for the event of interest.
Name resolution is not limited only to MAC addresses. Wireshark provides IP and port name resolution options as well. However, these options are not enabled by default. If you want to use these functionalities, you need to activate them through the **"Edit --> Preferences --> Name Resolution"** menu. Once you enable IP and port name resolution, you will see the resolved IP address and port names in the packet list pane and also will be able to view resolved names in the "Conversations" and "Endpoints" menus as well

Besides name resolution, Wireshark also provides an IP geolocation mapping that helps analysts identify the map's source and destination addresses. But this feature is not activated by default and needs supplementary data like the GeoIP database. Currently, Wireshark supports MaxMind databases, and the latest versions of the Wireshark come configured MaxMind DB resolver. However, you still need MaxMind DB files and provide the database path to Wireshark by using the "Edit --> Preferences --> Name Resolution --> MaxMind database directories" menu. Once you download and indicate the path, Wireshark will automatically provide GeoIP information under the IP protocol details for the matched IP addresses.

>IPv4 and IPv6

List all events linked to specific IP versions in a single window and use it for the event of interest

> DNS

This option breaks down all DNS packets from the capture file and helps analysts view the findings in a tree view based on packet counters and percentages of the DNS protocol.

> HTTP

This option breaks down all HTTP packets from the capture file and helps analysts view the findings in a tree view based on packet counters and percentages of the HTTP protocol.

#### Packet Filtering

> IP Filters

| Filter                     | Description                                                                                                                                             |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ip`                       | Show all IP packets.                                                                                                                                    |
| `ip.addr == 10.10.10.111`  | Show all packets containing IP address 10.10.10.111.                                                                                                    |
| `ip.addr == 10.10.10.0/24` | Show all packets containing IP addresses from 10.10.10.0/24 subnet.                                                                                     |
| `ip.src == 10.10.10.111`   | Show all packets originated from 10.10.10.111                                                                                                           |
| `ip.dst == 10.10.10.111`   | Show all packets sent to 10.10.10.111                                                                                                                   |
| ip.addr vs ip.src/ip.dst   | Note: The ip.addr filters the traffic without considering the packet direction. The ip.src/ip.dst filters the packet depending on the packet direction. |
> TCP and UDP Filters

| Filter                | Description                                     | Filter                | Expression                                      |
| --------------------- | ----------------------------------------------- | --------------------- | ----------------------------------------------- |
| `tcp.port == 80`      | Show all TCP packets with port 80               | `udp.port == 53`      | Show all UDP packets with port 53               |
| `tcp.srcport == 1234` | Show all TCP packets originating from port 1234 | `udp.srcport == 1234` | Show all UDP packets originating from port 1234 |
| `tcp.dstport == 80`   | Show all TCP packets sent to port 80            | `udp.dstport == 5353` | Show all UDP packets sent to port 5353          |
> Application Level Protocol Filters | HTTP and DNS

| Filter                          | Description                                    | Filter                    | Description              |
| ------------------------------- | ---------------------------------------------- | ------------------------- | ------------------------ |
| `http`                          | Show all HTTP packets                          | `dns`                     | Show all DNS packets     |
| `http.response.code == 200`     | Show all packets with HTTP response code "200" | `dns.flags.response == 0` | Show all DNS requests    |
| `http.request.method == "GET"`  | Show all HTTP GET requests                     | `dns.flags.response == 1` | Show all DNS responses   |
| `http.request.method == "POST"` | Show all HTTP POST requests                    | `dns.qry.type == 1`       | Show all DNS "A" records |
> Display Filter Expressions

This option stores all the supported protocol structures to help analysts create display filters.
`Analyse -> Display Filter Expression`

![[Pasted image 20250322005316.png]]

> Filter: "contains"

| Filter          | **contains**                                                                                                                                 |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Type**        | Comparison Operator                                                                                                                          |
| **Description** | Search a value inside packets. It is case-sensitive and provides similar functionality to the "Find" option by focusing on a specific field. |
| **Example**     | Find all "Apache" servers.                                                                                                                   |
| **Workflow**    | List all HTTP packets where packets' "server" field contains the "Apache" keyword.                                                           |
| **Usage**       | `http.server contains "Apache"`                                                                                                              |

> Filter: "matches"

| Filter      | **matches**                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| Type        | Comparison Operator                                                                                           |
| Description | Search a pattern of a regular expression. It is case insensitive, and complex queries have a margin of error. |
| **Example** | Find all .php and .html pages.                                                                                |
| Workflow    | List all HTTP packets where packets' "host" fields match keywords ".php" or ".html".                          |
| **Usage**   | `http.host matches "\.(php\|html)"`                                                                           |

> Filter: "in"

| Filter      | **in**                                                                         |
| ----------- | ------------------------------------------------------------------------------ |
| Type        | Set Membership                                                                 |
| Description | Search a value or field inside of a specific scope/range.                      |
| Example     | Find all packets that use ports 80, 443 or 8080.                               |
| Workflow    | List all TCP packets where packets' "port" fields have values 80, 443 or 8080. |
| Usage       | `tcp.port in {80 443 8080}`                                                    |

> Filter: "upper"

| Filter      | **upper**                                                                                                  |
| ----------- | ---------------------------------------------------------------------------------------------------------- |
| Type        | Function                                                                                                   |
| Description | Convert a string value to uppercase.                                                                       |
| Example     | Find all "APACHE" servers.                                                                                 |
| Workflow    | Convert all HTTP packets' "server" fields to uppercase and list packets that contain the "APACHE" keyword. |
| Usage       | `upper(http.server) contains "APACHE"`                                                                     |

> Filter: "lower"

| Filter      | **lower**                                                                                                       |
| ----------- | --------------------------------------------------------------------------------------------------------------- |
| Type        | Function                                                                                                        |
| Description | Convert a string value to lowercase.                                                                            |
| Example     | Find all "apache" servers.                                                                                      |
| Workflow    | Convert all HTTP packets' "server" fields info to lowercase and list packets that contain the "apache" keyword. |
| **Usage**   | `lower(http.server) contains "apache"`                                                                          |

> Filter: "string"

| Filter      | **string**                                                                               |
| ----------- | ---------------------------------------------------------------------------------------- |
| Type        | Function                                                                                 |
| Description | Convert a non-string value to a string.                                                  |
| Example     | Find all frames with odd numbers.                                                        |
| Workflow    | Convert all "frame number" fields to string values, and list frames end with odd values. |
| Usage       | `string(frame.number) matches "[13579]$"`                                                |

