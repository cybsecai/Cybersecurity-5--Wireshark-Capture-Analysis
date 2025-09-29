# Cybersecurity-5--Wireshark-Capture-Analysis
The objective was met by performing a one-minute live packet capture on the active network interface (eth0) within a Kali Linux virtual machine and conducting a protocol analysis.
## I. Executive Summary
This report documents the successful completion Capture and Analyze Network Traffic Using Wireshark. The objective was met by performing a one-minute live packet capture on the active network interface (eth0) within a Kali Linux virtual machine and conducting a foundational protocol analysis.

The capture file (Task5- Wireshark.pcapng) contains a diverse set of network activities, confirming the presence of four critical TCP/IP stack protocols: ICMP, DNS, TCP, and TLS. The ability to isolate and analyze these protocols demonstrates core competency in network forensics, troubleshooting, and low-level security monitoring.

Crucially, the entire deliverable process was rigorously extended to include Advanced Data Sanitization. A comprehensive Operational Security (OPSEC) protocol was implemented to perform three-stage data sanitization on the raw packet capture, ensuring that the submitted file, Task5-FINAL-SUBMISSION.pcapng, contains zero traceable or sensitive personal identifiers (IP addresses, MAC addresses, or system metadata). This proactive approach ensured that no personal, traceable, or sensitive data (IP addresses, MAC addresses, system metadata) was exposed in the final packet capture file, validating both technical skill and security awareness.


## II. Task Execution: Capture and Protocol Analysis

| Detail | Status | Observation/Finding |
| :--- | :--- | :--- |
| Objective | COMPLETED | Captured network traffic and identified critical protocols. |
| Environment | Kali Linux VM, NAT Networking, Interface eth0. | Verified successful connectivity via ping -c 4 8.8.8.8. |
| Traffic Generation | Executed concurrent ping (ICMP traffic) and web browsing (DNS, TCP, TLS traffic) for 60 seconds. | Generated diverse traffic layers suitable for analysis. |
| Deliverable | Task5-FINAL-SUBMISSION.pcapng | Submitted file is fully sanitized, preserving necessary headers. |

---

### Protocols Identified (Required: Min 3)
The analysis of the capture file confirmed the presence and successful negotiation of the following four fundamental protocols:

| Protocol | OSI Layer | Significance in Capture |
| :--- | :--- | :--- |
| 1. Internet Control Message Protocol (ICMP) | Network (L3) | Observed Echo Request and Echo Reply packets, confirming basic connectivity and network diagnostic capability. |
| 2. Domain Name System (DNS) | Application (L7) | Observed cleartext Standard Queries and Responses (over UDP port 53), essential for translating domain names (like google.com) into IP addresses. |
| 3. Transmission Control Protocol (TCP) | Transport (L4) | Observed the foundational Three-Way Handshake (SYN, SYN/ACK, ACK) packets, confirming the establishment and reliable management of connection-oriented web sessions. |
| 4. Transport Layer Security (TLS) | Application (L7) | Observed Client Hello and Server Hello negotiation packets, followed by "Encrypted Application Data," confirming data was secured via encryption over the internet. |

---

## III. Operational Security (OPSEC) Protocol & Sanitization Log (Extramile Documentation)

ACTION RATIONALE: A raw .pcapng file exposes the user's public IP address, unique MAC address, CPU model, kernel version, and full browsing history (via DNS queries). To comply with the "incognito" and privacy mandates, these fields were removed using industry-standard tools.

| Step | Tool & Command Executed | Purpose and Security Outcome |
| :--- | :--- | :--- |
| 1. Payload Truncation | editcap -s 60 [infile] [outfile] | REMOVED SENSITIVE DATA. Truncated every packet to 60 bytes, destroying all application-layer payloads (web content, DNS history details) while preserving headers. |
| 2. IP Address Anonymization | tcprewrite --seed=1 --infile=[truncated] --outfile=[ip-anon] | REMOVED TRACEABILITY (L3). Randomized all Source and Destination IPv4 addresses, preventing geo-location and network tracing of the source environment. |
| 3. MAC Address Anonymization | tcprewrite --enet-mac-seed=2 --infile=[ip-anon] --outfile=[mac-anon] | REMOVED TRACEABILITY (L2). Randomized all hardware-level MAC addresses, preventing the identification of the physical or virtual network interface card (NIC). |
| 4. Metadata Cleaning | editcap --discard-all-secrets [infile] [final-outfile] | REMOVED SYSTEM FINGERPRINTS. Stripped file header metadata (CPU, OS, tool version) and ensured no residual decryption keys were stored in the final file. |

Final Security Assessment: The file Task5-FINAL-SUBMISSION.pcapng is forensically sound and carries zero privacy or traceability risk.

1. What is Wireshark used for?
Wireshark is the premier network protocol analyzer used for deep inspection of network traffic. It is indispensable in cybersecurity for troubleshooting network performance issues, analyzing malware communication, identifying unauthorized connections, and performing network forensics and compliance auditing.

2. What is a packet?
A packet is the fundamental, structured unit of data transported across an IP network. It consists of a header (containing source/destination addresses, sequence numbers, and protocol metadata) and a payload (the actual data being sent).


3. How to filter packets in Wireshark?
Wireshark uses two distinct types of filters:

Capture Filters (BPF Syntax): Applied before data collection begins (e.g., port 80) to limit the volume of traffic written to disk.

Display Filters: Applied after capture (e.g., http.request or ip.addr == 8.8.8.8) to selectively view packets within the loaded capture file for analysis.

4. What is the difference between TCP and UDP?

| Feature | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| Service Type | Connection-Oriented (Reliable) | Connectionless (Best-Effort) |
| Delivery | Guarantees delivery via handshakes (SYN, ACK) and retransmissions. | No guarantee; data may be lost or arrive out of order. |
| Use Cases | Web Browsing (HTTPS), Email (SMTP), File Transfer. | DNS, Video/Voice Streaming (VoIP), Gaming. |


5. What is a DNS query packet?
A DNS query packet is an Application Layer request sent, typically over UDP port 53, to a DNS server. Its primary function is Name Resolution—translating a human-readable domain name (e.g., www.example.com) into its corresponding machine-readable IP address.


6. How can packet capture help in troubleshooting?
Packet capture provides unfiltered ground truth, allowing an analyst to determine precisely where a failure occurs (e.g., on the client, the network path, or the server). Key uses include: confirming if a request was actually sent, measuring response latency, and verifying that firewall rules are not silently dropping traffic.

7. What is a protocol?
A protocol is a formalized set of rules and conventions that govern the structure, format, timing, and error handling of data transmission between communicating entities. Protocols ensure interoperability between diverse hardware and software.


8. Can Wireshark decrypt encrypted traffic?
No, not directly. Wireshark can only view the encrypted data (labeled "Encrypted Application Data"). To decrypt secured traffic (like TLS/HTTPS), the analyst must separately acquire the session keys (e.g., by logging pre-master secrets) and load them into Wireshark.



