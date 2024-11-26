---
date: 2024-10-06 (Sun)
---

# Layer 4: Transport

## Layer 3 Problems

- Out of order delivery (packets is routed independently)
- Packet loss
- Packet duplication
- Packet delay
- No communication channel
- No flow control (source transmit faster than destination can receive)

## Transfer Control Protocol (TCP)

### TCP Segments

A TCP **_Segment_** has the following structure:

- TCP Header:
  - Source Port
  - Destination Port
  - Sequence Number (SEQ): 32-bit
  - Acknowledgement (ACK): 32-bit
  - Flags: SYN, ACK, FIN, RST, PSH, URG
  - Window: For flow control
  - Checksum: For error detection
  - Urgent Pointer
- Data

### Flags

- SYN: Synchronize sequence numbers
- ACK: Acknowledge sequence number

### Three-way Handshake

1. Client sets a random initial sequence number (ISN), $s_c$ and sends a SYN
   segment
2. Server picks a random ISN $s_s$, sends SYN & ACK segment, and sets ACK to
   $s_c + 1$.
3. Client sets SEQ to $s_c + 1$, ACK to $s_s + 1$, and sends ACK segment.

Then connection is established.

## User Datagram Protocol (UDP)

## Ports

- Well-known ports: 0-1023
- Registered ports: 1024-49151
- Dynamic ports: 49152-65535
- Ephemeral ports: ports used by the client (1024-65535 depending on OS)

## 🧭 Navigation

- [🔼 Back to top](#layer-4-transport)
- [◀️ Back](osi-7-layer-model.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
