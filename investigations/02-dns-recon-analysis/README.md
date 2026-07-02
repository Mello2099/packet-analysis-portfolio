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
