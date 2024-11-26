---
date: 2024-10-05 (Sat)
---

# Layer 2: Data Link

Supports device to device communication within the same **_local area network_**
(**_LAN_**).

## Overview

- Identifiable devices with [MAC addresses](#mac-addresses).
- [Media access control](#control-access-to-physical-medium)
- [Collision detection](#collision-detection)
- **_Unicast_**: one-to-one communication
- **_Broadcast_**: one-to-all communication
- [Switches](#switches)

## Data Frames

A **_data frame_** (**_frame_**) is the unit of data that is transmitted over
the network. **_Ethernet frame_** is the most common frame format used in LANs.

**_Ethernet frame_** consist of several parts:

- **_Preamble_**: 7 bytes of alternating 1s and 0s. It is used to synchronize
  the receiver and sender. **_Start Frame Delimiter_** (**_SFD_**) is the last
  byte of the preamble, which is `10101011`.
- **_Header_**: 14 bytes:
  - **_Destination MAC Address_**: 6 bytes.
  - **_Source MAC Address_**: 6 bytes.
  - **_Ether Type_**: 2 bytes. It specifies which layer 3 protocol is being
    used.
- **_Payload_**: 46-1500 bytes. It is the actual data **_encapsulated_** in the
  frame.
- **_Trailer_**: contains **_Frame Check Sequence_** (**_FCS_**), which is 4
  bytes. It is used to check if the frame is received correctly. It is
  calculated using the **_Cyclic Redundancy Check_** (**_CRC_**) algorithm.

### Jumbo Frames

**_Jumbo frames_** are Ethernet frames with a payload greater than 1500 bytes,
usually up to 9000 bytes.

## MAC Addresses

Devices at L2, e.g. _network interface cards_, have a unique **hardware** **_MAC
address_**. It is 48 bits, in hex format.

A MAC address has two parts:

- First 24 bits are the **_organizationally unique identifier_** (**_OUI_**),
  the manufacturer's ID.
- Later 24 bits are unique to the **_network interface controller_**
  (**_NIC_**). It uniquely identifies the device from the manufacturer.

`ff:ff:ff:ff:ff:ff` is reserved for the **broadcast address**. And a frame can
be addressed to a specific MAC address.

## Control Access to Physical Medium

To prevent _cross-talk_ and _collisions_, we need to control access to the
shared medium.

**_Carrier Sense Multiple Access_** (**_CSMA_**) is a protocol used to determine
when to send data. It listens to the shared medium for the carrier signal and
sends data when the medium is free (when it doesn't detect any carrier).

## Collision Detection

Even with CSMA, when two devices send data at the same time, a **_collision_**
occurs.

On L2, when a collision is detected, the device sends a **_jam signal_** to
inform other, and wait for a random amount of time (backoff period) before
attempting to retransmit.

If a collision happens again, the backoff period is increased.

## Transmission Process

1. Sender's L2 creates a frame
2. [Checks if the medium is free](#control-access-to-physical-medium)
3. If free, sender's L1 sends the frame
4. Receiver's L1 receives the frame
5. Receiver's L2 checks MAC address is self

## Switches

A **_switch_** works similar to Hub but as a L2 device also understands L2.

It has a MAC address table, which maps MAC addresses to ports. When a frame is
received, it:

- Checks if the frame is valid
- If it is valid, stores it in a buffer
- Updates the MAC address table with the source MAC address and port
- Checks the destination MAC address and forwards the frame to the correct port
- If the destination MAC address is not in the table, it floods the frame to all
  ports except the source port. This is called **_unknown unicast flooding_**.
- Any frames with all f's in the destination MAC address are forwarded to all
  ports.

Each port on a switch is a **separate collision domain**.

## 🧭 Navigation

- [🔼 Back to top](#layer-2-data-link)
- [◀️ Back](osi-7-layer-model.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
