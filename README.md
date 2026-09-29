# Network Traffic Analysis & Incident Investigation

## Overview

This project documents the analysis of a packet capture (PCAP) file using Wireshark to identify network communications, investigate suspicious activity, and develop incident response findings.

The objective was to perform a structured network traffic investigation similar to the workflow used by Security Operations Center (SOC) analysts during threat hunting and incident response engagements.

---

## Scenario

A network traffic capture was provided for analysis. The goal was to determine:

- Which hosts were communicating
- What domains were being visited
- Whether suspicious traffic existed
- If evidence of malicious communications could be identified
- Potential Indicators of Compromise (IOCs)

---

## Tools Used

### Wireshark

Used for:

- Packet inspection
- Protocol analysis
- DNS investigation
- HTTP traffic analysis
- TCP stream reconstruction
- Endpoint identification

### Analyst Documentation

- Markdown investigation notes
- IOC collection
- Timeline development
- Incident reporting

---

## Investigation Methodology

### Phase 1: Environment Discovery

Initial analysis focused on identifying hosts within the capture.

**Wireshark Path**

```text
Statistics → Endpoints → IPv4
```

Key observations:

| Host | Packets | Traffic |
|--------|----------|----------|
| 10.9.11.135 | 73,626 | 58 MB |
| 10.9.11.2 | 1,778 | 450 KB |

Analysis indicated that **10.9.11.135** was the primary workstation generating traffic.

---

### Phase 2: DNS Analysis

Filter used:

```text
dns
```

Observed DNS requests included:

```text
quadcinema.com
code.jquery.com
www.google-analytics.com
aatthews.cfd
```

Domains such as:

```text
code.jquery.com
www.google-analytics.com
```

appeared consistent with normal web browsing activity.

During investigation an unusual domain was identified:

```text
aatthews.cfd
```

This domain became a focus of further analysis.

---

### Phase 3: Connection Analysis

The following connection sequence was observed:

```text
DNS Query
↓
DNS Resolution
↓
TCP Handshake
↓
TLS Negotiation
↓
HTTPS Communication
```

Example:

```text
Source:
10.9.11.135

Destination:
104.196.13.170

Port:
443

Protocol:
TLS 1.3
```

The successful TLS negotiation confirmed encrypted communications between the workstation and an external server.

---

### Phase 4: Endpoint Investigation

Endpoint statistics identified several external hosts.

Notable systems included:

```text
104.196.13.170
104.16.212.131
86.106.87.134
```

Of these, the host below generated significant attention:

```text
86.106.87.134
```

The endpoint participated in thousands of packets and repeated communications with the internal workstation.

---

### Phase 5: HTTP Analysis

Filter used:

```text
http.response.code == 200
```

Multiple successful HTTP responses were observed from:

```text
86.106.87.134
```

To further investigate:

```text
Right Click Packet
↓
Follow
↓
TCP Stream
```

This revealed reconstructed application-layer communications.

---

### Phase 6: TCP Stream Analysis

TCP stream reconstruction exposed the following request:

```http
POST /v/...
Host: know.mom-nower.com
Content-Type: application/octet-stream
```

Key observations:

- HTTP POST requests were observed
- Data was transmitted outbound from the workstation
- The destination domain was unusual
- Binary content was transferred

The following header was identified:

```http
Content-Type: application/octet-stream
```

This content type commonly indicates binary data transfer rather than typical web content.

---

## Indicators of Interest

### Internal Host

```text
10.9.11.135
```

### External Infrastructure

```text
86.106.87.134
```

### Domains Observed

```text
quadcinema.com
aatthews.cfd
know.mom-nower.com
code.jquery.com
www.google-analytics.com
```

---

## Findings

### Finding 1

The host:

```text
10.9.11.135
```

was responsible for the majority of network activity within the capture.

---

### Finding 2

Several external domains were contacted, including an uncommon domain:

```text
aatthews.cfd
```

which warranted additional analysis.

---

### Finding 3

The workstation established communications with:

```text
86.106.87.134
```

using HTTP.

---

### Finding 4

TCP stream reconstruction revealed outbound HTTP POST requests to:

```text
know.mom-nower.com
```

with the following characteristics:

```text
Method:
POST

Content Type:
application/octet-stream
```

---

### Finding 5

Evidence suggests the transfer of binary data between:

```text
10.9.11.135
```

and

```text
86.106.87.134
```

Further investigation would be required to determine whether the activity was legitimate application traffic or unauthorized communications.

---

## MITRE ATT&CK Mapping

| Technique | Description |
|------------|------------|
| T1071 | Application Layer Protocol |
| T1071.001 | Web Protocols |
| T1041 | Exfiltration Over C2 Channel |
| T1105 | Ingress Tool Transfer |
| T1078 | Valid Accounts (if authenticated sessions are confirmed) |

---

## Lessons Learned

This investigation reinforced several core SOC analyst skills:

- DNS analysis
- Endpoint discovery
- HTTP traffic analysis
- TCP stream reconstruction
- IOC identification
- Threat hunting methodology
- Structured incident documentation

---

## Skills Demonstrated

### Network Analysis

- Packet inspection
- TCP/IP analysis
- HTTP analysis
- DNS investigation

### Security Operations

- Incident response workflow
- IOC identification
- Evidence collection
- Threat hunting

### Tools

- Wireshark
- Linux
- Markdown Documentation

---

## Future Improvements

Future enhancements could include:

- VirusTotal enrichment
- WHOIS analysis
- GeoIP correlation
- Automated IOC extraction using Python
- Splunk ingestion and visualization
- Sigma rule development based on findings

---

## Author

**Tshegofatso Nkosi**

Aspiring SOC Analyst | ISC² CC Certified | Cybersecurity Student

LinkedIn:
https://www.linkedin.com/in/tshegofatso-nkosi/

---
