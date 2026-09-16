# Network Traffic Analysis

## Objective

This project analyzes a publicly available network packet capture using Wireshark and tcpdump. The goal is to practice identifying network protocols, examining DNS and HTTP communications, analyzing packet-level information, and documenting security observations from captured network traffic.

The analysis focuses on understanding how network traffic can reveal information about communications between a client and remote servers.

## Tools Used

- **Wireshark** — Used to inspect individual packets, protocols, HTTP requests, HTTP headers, DNS messages, and packet details.

- **tcpdump** — Used from the command line to filter and inspect network traffic by protocol and port.

- **Git/GitHub** — Used to organize and document the project.


## Capture Information

The analysis was performed using the publicly available http.cap packet capture provided for network analysis practice.

The capture contains historical network traffic recorded on May 13, 2004. The traffic includes DNS, TCP, and HTTP communications between a client and remote servers.

The capture was analyzed locally using Wireshark and tcpdump.

## DNS Analysis

A DNS query was observed for:

pagead2.googlesyndication.com

The DNS request was sent from:

145.254.160.237:3009

to the DNS server:

145.253.2.203:53

The DNS response provided the following information:

pagead2.googlesyndication.com → pagead2.google.com

pagead2.google.com → pagead.google.akadns.net

IPv4 address: 216.239.59.104

IPv4 address: 216.239.59.99

The address 216.239.59.99 was later observed as the destination IP for an HTTP connection from the client.

### tcpdump Verification

The DNS traffic was also examined using tcpdump with a port 53 filter:

```tcpdump -nn -r ~/network-traffic-analysis-wireshark/captures/http.cap 'port 53'```

This confirmed the DNS query and response at the command line.

### Security Observation

DNS analysis demonstrated how a hostname can be resolved to an IP address before a client establishes communication with the remote server. This relationship can be followed through a packet capture to understand how network connections are established.

### TCP and HTTP Analysis

### TCP Connection

A TCP three-way handshake was observed for an HTTP connection between:

Client: 145.254.160.237:3372

Server: 65.208.228.223:80

The handshake consisted of:

SYN

SYN, ACK

ACK

This demonstrated the establishment of a TCP connection before HTTP communication.

### HTTP Request

A separate HTTP connection was observed in Frame 18:

Source: 145.254.160.237:3371

Destination: 216.239.59.99:80

Request method: GET

Request path: /pagead/ads

The HTTP request was sent to:

pagead2.googlesyndication.com

The request contained several HTTP headers, including:

- `Host`

- `User-Agent`

- `Accept`

- `Accept-Language`

- `Accept-Encoding`

- `Connection`

- `Referer`

The request also contained URL query parameters, including a client identifier, a random value, and a URL identifying the webpage associated with the request.

### HTTP Response

Frame 27 contained the corresponding HTTP response:

Server: 216.239.59.99:80

Client: 145.254.160.237:3371

Status: HTTP/1.1 200 OK

Content type: text/html

Content length: 1272 bytes

This demonstrated the complete request-and-response flow between the client and the remote HTTP server.

### Important Connection Distinction

The TCP handshake documented above uses port 3372 and server 65.208.228.223.

The advertising HTTP request in Frame 18 uses port 3371 and server 216.239.59.99.

These are separate TCP connections and were documented separately to avoid incorrectly associating the handshake with the advertising request.

### Security Observations

### 1. Unencrypted HTTP Traffic

The capture contains HTTP communication over TCP port 80. HTTP does not provide encryption for the application-layer traffic.

Because the traffic is unencrypted, information contained in the HTTP request can be visible to a party capable of capturing the network traffic.

### 2. Information Exposed in the HTTP Request

Frame 18 contains information in the HTTP request headers and URL, including:

Destination hostname

User-Agent information

Referer URL

Client identifier

URL query parameters

The request also contains a URL-encoded webpage address:

http%3A%2F%2Fwww.ethereal.com%2Fdownload.html

This is encoding rather than encryption. The encoded value can be decoded to:

`http://www.ethereal.com/download.html`


### 3. Authentication-Related Headers

No Cookie or Authorization header was observed in the analyzed Frame 18 HTTP request.

This observation applies only to the examined packet and does not establish whether the broader application used authentication.

### 4. Security Analysis Limitation

The capture is a historical sample recorded in 2004. Therefore, the traffic should be treated as a learning example rather than evidence of how modern production systems operate.

The analysis identifies information visible in this particular capture and does not attempt to determine whether the observed traffic was malicious.

### tcpdump Analysis

The packet capture was also analyzed using tcpdump to demonstrate command-line network traffic analysis.

### DNS Traffic

The following command filtered traffic using DNS port 53:

```tcpdump -nn -r ~/network-traffic-analysis-wireshark/captures/http.cap 'port 53'```

The output showed the DNS query for:

pagead2.googlesyndication.com

and the corresponding DNS response containing the resolved IP addresses.

### HTTP Traffic

The following command filtered traffic using HTTP port 80:

```tcpdump -nn -r ~/network-traffic-analysis-wireshark/captures/http.cap 'port 80'```

The output showed TCP and HTTP traffic, including:

TCP connection establishment

HTTP GET requests

HTTP 200 OK responses

Communication between the client and remote HTTP servers

Using both Wireshark and tcpdump provided two different approaches to analyzing the same packet capture: detailed graphical packet inspection and command-line traffic analysis.

### Conclusion

This analysis provided hands-on practice with packet-level network investigation using Wireshark and tcpdump. The investigation followed DNS resolution, examined TCP connections, analyzed HTTP requests and responses, and identified information visible in unencrypted HTTP traffic.

The project demonstrates foundational skills in network traffic analysis, protocol identification, command-line investigation, and documenting security observations from packet captures.

