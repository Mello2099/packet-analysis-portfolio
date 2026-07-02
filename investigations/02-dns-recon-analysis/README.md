# Investigation 02: DNS Query and Response Analysis

## Objective
To capture and dissect Domain Name System (DNS) traffic using Wireshark, evaluating the structural components of network name resolution, analyzing packet flags, and examining the security risks associated with legacy unencrypted DNS payloads.

## Tools Used
* Wireshark (Packet Dissector)
* Native Linux CLI (`nslookup` utility)
* UTM Hypervisor (Kali Linux ARM64 Environment)

---

## Technical Skills Demonstrated
* **Protocol Analysis:** Deep packet inspection of UDP Port 53 stateless transaction streams.
* **Packet Dissection:** Verification of DNS Transaction IDs, Flags (Recursion Desired, Truncation), and Record Types (A, AAAA).
* **Security Threat Awareness:** Assessment of plaintext sniffing vulnerabilities and DNS cache poisoning vectors.

---

## Investigation Steps
1. **Live Traffic Generation:** Initialized a network capture across the active interface and generated targeted query traffic utilizing the `nslookup target.com 8.8.8.8` utility to simulate external resolution requests.
2. **Protocol Isolation:** Applied the `dns` display filter within Wireshark to isolate transaction traffic from background network chatter.
3. **Payload Inspection:** Examined the structural metadata of the stateless UDP packets, specifically auditing the Transaction ID mappings and recursive validation bits.

---

## Analysis & Findings

### Executive Summary
During this exercise, network-layer captures were analyzed to map the mechanics of a standard DNS transaction. DNS operates as an unencrypted, stateless architecture primarily over UDP port 53. By matching outbound queries to corresponding inbound responses via uniform cryptographic-lite IDs, the lookup process successfully mapped human-readable hostnames to physical IP designations. However, the analysis highlighted structural risks inherent to basic UDP transactions, emphasizing the ease with which unencrypted DNS metadata can be mapped or intercepted.

### Technical Breakdown
* **Transport Layer Protocol:** UDP (Stateless, Low-Overhead)
* **Target Port:** 53
* **Applied Display Filter:** `dns`

Dissection of the query/response pairs yielded the following forensic insights:
1. **Transaction ID Matching:** The outbound query packet and inbound response packet matched precisely via their hexadecimal `Transaction ID` string. This field serves as the sole validation mechanism for the client to confirm an arriving packet belongs to its original request.
2. **Flag Analysis:** The query packet explicitly declared `Recursion desired: True`, instructing the upstream nameserver to resolve the complete chain on the client's behalf. The response payload returned with `Recursion available: True` alongside the `Authoritative / Non-authoritative` declaration bit.
3. **Resource Record Verification:** The "Answers" section of the response packet mapped the lookup target to its static IPv4 destination via standard **Type A** resource records.

### Security Impact & Mitigation
Because standard DNS lacks structural payload authentication or encryption, this transaction is vulnerable to two primary attack models:
* **Passive Reconnaissance:** Attackers monitoring local segments can passively read plaintext DNS requests to profile user activity, mapping out network behaviors and target infrastructure.
* **DNS Spoofing / Cache Poisoning:** Because validation relies almost exclusively on matching a predictable, short Transaction ID over connectionless UDP, an attacker could forge a response packet containing rogue IP targets. If the forged packet beats the legitimate nameserver response back to the host, the target's traffic can be seamlessly hijacked to a malicious server.

### Strategic Mitigations
To eliminate these specific protocol weaknesses across contemporary enterprise environments, organizations must deploy:
1. **DNS over HTTPS (DoH) or DNS over TLS (DoT):** Encapsulating standard name resolution within an encrypted tunnel prevents third-party eavesdropping and unauthorized inspection.
2. **DNSSEC (Domain Name System Security Extensions):** Implementing cryptographic signatures for DNS records ensures authentication and cryptographic data integrity, neutralizing packet spoofing and validation hijacking vectors entirely.
