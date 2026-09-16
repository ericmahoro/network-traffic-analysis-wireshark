Network Traffic Analysis with Wireshark & tcpdump

Overview

This project demonstrates practical network traffic analysis using Wireshark and tcpdump.

I analyzed a publicly available packet capture to investigate DNS resolution, TCP connections, HTTP requests and responses, and security information visible in unencrypted network traffic.

Tools

Wireshark

tcpdump

Git & GitHub

What I Analyzed

DNS: Investigated hostname resolution and identified returned IP addresses.

TCP: Examined TCP connection establishment using the three-way handshake.

HTTP: Analyzed GET requests, HTTP headers, URLs, and server responses.

Security: Identified information exposed through unencrypted HTTP traffic.

Command Line: Used tcpdump to filter DNS and HTTP traffic.

Key Security Observations

The analysis demonstrated how packet captures can reveal:

Destination hosts and IP addresses

HTTP request methods and URLs

User-Agent and Referer information

URL query parameters

Information transmitted without application-layer encryption

The project also demonstrates the importance of distinguishing between different TCP connections when investigating network traffic.

Files

analysis.md — Detailed investigation and security observations

captures/http.cap — Public sample packet capture used for analysis

Disclaimer

The packet capture is a historical public sample recorded in 2004 and is used strictly for educational network-analysis purposes. The analysis does not determine that the observed traffic was malicious.

