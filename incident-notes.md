\# Network Traffic Analysis Report



\## Overview



This investigation was conducted using Wireshark to analyze network traffic captured in the file `2026-09-11-traffic-analysis-exercise.pcap`.



The objective was to identify unusual network activity, investigate external communications, and determine whether any indicators of compromise were present.



\---



\## Tools Used



\- Wireshark

\- TCP Stream Analysis

\- Conversation Statistics

\- HTTP Filtering



\---



\## Findings



\### Primary Internal Host



The most active host identified during the analysis was:



10.9.11.135



This system generated the majority of network traffic within the capture and communicated with multiple external hosts.



\---



\### HTTP Traffic Analysis



HTTP traffic was reviewed using Wireshark display filters.



One observed request was:



GET /r/gsr1.crl

Host: c.pki.goog



The associated User-Agent was:



Microsoft-CryptoAPI/10.0



This activity was determined to be related to certificate validation and appeared legitimate.



\---



\### Suspicious External Communication



The internal host communicated with the external IP address:



86.106.87.134



During analysis, HTTP requests were observed to:



know.mon-nower.com



Example request:



GET /zgzly/e2fkf6...



The URI contained a long random string and differed from typical web browsing traffic.



\---



\### TCP Stream Analysis



A TCP stream was reconstructed to review the full communication.



The server responded with:



HTTP/1.1 406 Not Acceptable



Although the connection was successfully established, no evidence of:



\- Malware downloads

\- Executable files

\- Command-and-control instructions

\- Data exfiltration



was identified during the review.



\---



\## Indicators of Interest



\### Internal Host



\- 10.9.11.135



\### External IP



\- 86.106.87.134



\### Domain



\- know.mon-nower.com



\---



\## Assessment



The packet capture contained both normal and unusual network activity.



Certificate validation traffic to `c.pki.goog` was assessed as legitimate.



Communication with `86.106.87.134` and `know.mon-nower.com` appeared unusual due to the structure of the requests and external communication patterns. However, additional evidence was not found to confirm malicious activity.



Based on the available evidence, no confirmed compromise was identified.



\---



\## Conclusion



This investigation identified an internal host communicating with several external systems and included a review of HTTP traffic and TCP streams.



While some traffic appeared suspicious and required further investigation, the analysis did not reveal evidence of malware execution, command-and-control activity, or data exfiltration.



The activity should be monitored, but the capture does not provide sufficient evidence to classify the traffic as malicious.



\---



\## Skills Demonstrated



\- Network Traffic Analysis

\- Wireshark

\- HTTP Analysis

\- TCP Stream Analysis

\- IOC Identification

\- Incident Documentation

