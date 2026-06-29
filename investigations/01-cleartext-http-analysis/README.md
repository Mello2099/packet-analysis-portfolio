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
### Wireshark TCP Stream 0 Analysis

#### 1. The Request (Client to Server)
* **Action:** An HTTP `GET` request was sent to the root directory (`/`) on `localhost` over the loopback interface (`lo`).
* **User-Agent:** The request originated from Firefox 115 running on a Linux architecture (`Linux aarch64`).
* **Connection:** Set to `keep-alive`, maintaining the TCP connection for subsequent requests.

#### 2. The Response (Server to Client)
* **Status:** `HTTP/1.1 200 OK`, indicating the web server successfully processed the request and delivered the page.
* **Server Stack:** The underlying host is running an **Apache/2.4.57** web server on a **Debian** distribution.
* **Timestamp:** The capture was recorded on Monday, June 29, 2026, at 15:19:41 GMT.

#### 3. Security & Network Insights
* **Unencrypted Traffic:** The exchange uses standard HTTP (Port 80) rather than HTTPS (Port 443). Because the data is sent in plaintext, all headers, server versions, and payloads are fully visible to packet sniffers.
* **Information Disclosure (Banner Grabbing):** The server explicitly broadcasts its exact software versions (`Apache/2.4.57 (Debian)`). In a production environment, this exposure aids attackers in identifying known CVEs tailored to that specific release.
<img width="3024" height="1964" alt="TCP" src="https://github.com/user-attachments/assets/30c4d4e8-7da1-4963-b74a-9d278ecebdca" />
