
# RFC 791 – Internet Protocol (IPv4) Summary

## 1. Overview
- Defines **IPv4** standard (Network Layer – OSI Layer 3).
- Provides:
  - **Addressing**: Identify source and destination.
  - **Routing**: Choose best path for packet.
  - **Fragmentation**: Split datagram if network MTU is too small.

**Characteristics:**
- Connectionless (no session).
- Best-Effort Delivery (no guarantee).
- Stateless.

---

## 2. IPv4 Header Structure (20–60 Bytes)

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if any)         |    Padding      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

---

### Field Description
| Field                | Size    | Description                                 |
|----------------------|---------|---------------------------------------------|
| Version             | 4 bits  | IP version (IPv4 = 4)                      |
| IHL                 | 4 bits  | Header length in 32-bit words              |
| Type of Service     | 8 bits  | QoS, DSCP                                  |
| Total Length        | 16 bits | Full packet length (header + data)         |
| Identification      | 16 bits | Used for fragmentation                     |
| Flags               | 3 bits  | DF (Don't Frag), MF (More Frag)            |
| Fragment Offset     | 13 bits | Fragment order                             |
| Time to Live (TTL)  | 8 bits  | Hop count, decrements each router          |
| Protocol            | 8 bits  | Upper layer (TCP=6, UDP=17, ICMP=1)        |
| Header Checksum     | 16 bits | Check header integrity                     |
| Source Address      | 32 bits | Sender IP                                  |
| Destination Address | 32 bits | Receiver IP                                |
| Options + Padding   | Var.    | Optional fields                            |

---

## 3. Key Mechanisms
- **Type of Service (ToS)**: QoS settings.
- **TTL**: Prevent routing loops.
- **Fragmentation**: For small MTU networks.
- **Checksum**: Header integrity check.

---

## 4. Example (Wireshark)
```
Internet Protocol Version 4, Src: 192.168.1.10, Dst: 8.8.8.8
    Version: 4
    Header Length: 20 bytes
    Total Length: 60
    TTL: 64
    Protocol: TCP (6)
    Header checksum: 0xb1e6 [correct]
    Source: 192.168.1.10
    Destination: 8.8.8.8
```

---

## 5. Features and Limitations
- Pros:
  - Flexible, interoperable, widely adopted.
- Cons:
  - Limited address space (≈4.3B).
  - No built-in security → IPsec required.
  - No guarantee of delivery/order.

---

## 6. Related RFCs
- RFC 768 – UDP
- RFC 793 – TCP
- RFC 792 – ICMP
- RFC 1918 – Private IP
- RFC 2474 – DSCP
