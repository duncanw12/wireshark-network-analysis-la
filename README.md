# Network Troubleshooting & Packet Analysis Lab

**Tools:** Wireshark, Windows Command Prompt, `nslookup`, `ping`, `curl`  
**Scope:** DNS, ICMP, TCP, and TLS packet inspection on a test workstation.

## Project overview
This hands-on lab explores how network communications are established, resolved, tested, and encrypted. I captured network traffic in Wireshark, applied display filters, inspected protocol fields, and documented the observed results.

## Evidence and findings

### 1. DNS lookup and resolution
![DNS lookup](screenshots/01_DNS_Lookup.png)

- Filter: `dns.qry.name == "example.com"`
- Observed A and AAAA queries and corresponding responses.
- A response showed `172.66.147.243` and `104.20.23.154`, TTL 138 seconds.
- DNS response time displayed: 154.99 ms.
- This demonstrates successful name resolution for this lookup, not all DNS traffic.

### 2. ICMP connectivity
![ICMP echo requests and replies](screenshots/02_ICMP_Echo_Request_Reply.png)

- Filter: `icmp`
- Observed four echo requests from `10.0.0.205` to `1.1.1.1` and four matching replies.
- No loss in this four-packet sample. It does not establish long-term network reliability.
- This direct-IP ping tests connectivity without requiring DNS resolution.

### 3. TCP three-way handshake
![TCP handshake](screenshots/03_TCP_Three_Way_Handshake.png)

- Observed packets 440 (`SYN`), 441 (`SYN, ACK`), and 442 (`ACK`) on TCP port 80.
- Confirms the three-step TCP connection establishment sequence in this capture.
- Wireshark can isolate a connection using **Follow → TCP Stream**.

### 4. TLS handshake
![TLS handshake](screenshots/04_TLS_Client_Server_Hello.png)

- Observed packet 273: TLS 1.3 Client Hello with SNI `example.com`.
- Observed packet 275: Server Hello and Change Cipher Spec.
- This demonstrates the beginning of TLS negotiation, not by itself proof of a successful HTTPS response.

### 5. TLS Application Data
![TLS Application Data](screenshots/05_TLS_Encrypted_Application_Data.png)

- Filter: `tls.record.content_type == 23`
- Observed multiple TLS Application Data records over IPv4 and IPv6.
- TLS 1.3 encrypted handshake records may also appear as outer Application Data.
- This broad filter includes traffic from multiple connections; it does not establish that all packets belong to the `example.com` request.

## Troubleshooting workflow
1. Select the correct network interface and start capture.
2. Generate controlled test traffic using `nslookup`, `ping`, or `curl`.
3. Stop capture and apply relevant display filters.
4. Compare packet direction, flags, timing, and responses.
5. Save screenshots and document findings with limitations.

## Security and privacy
Screenshots may show local and public IP addresses and other network metadata. Review/redact identifying information before wider publication. **Raw `.pcapng` files are intentionally excluded from this public-ready package** because they can contain sensitive traffic.

## Skills demonstrated
- Wireshark packet capture and protocol inspection
- DNS A/AAAA record analysis
- ICMP echo request/reply correlation
- TCP SYN / SYN-ACK / ACK identification
- TLS handshake and encrypted-record identification
- Technical documentation and evidence organization

## Resume project entry
**Network Troubleshooting & Packet Analysis Lab** — Used Wireshark and Windows diagnostic tools to capture and analyze DNS, ICMP, TCP, and TLS traffic; verified DNS responses, ICMP connectivity, TCP three-way handshakes, and encrypted TLS records; documented findings and troubleshooting limitations.

## Disclaimer
This is a self-directed training lab, not production incident-response experience. Packet observations are drawn from the screenshots shown in this repository.
