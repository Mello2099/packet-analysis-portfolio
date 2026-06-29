# Investigation 01: Unencrypted HTTP Credential Analysis

## Objective
To analyze cleartext HTTP network streams using Wireshark to identify plaintext credential exposure and demonstrate the security risks of unencrypted protocols.

## Tools Used
* Wireshark (Packet Dissector)
* UTM Hypervisor (Kali Linux ARM64 Environment)

## Investigation Steps
1. **Filter Traffic:** Isolated HTTP web traffic using the `http` display filter.
2. **Reconstruct Streams:** Followed the TCP stream of the target conversation to view the raw application-layer payload.
3. **Data Extraction:** Identified sensitive strings transmitted across the wire without TLS/SSL encryption.

## Analysis & Findings
*(Documentation and screenshots will be added here during the lab exercise)*
