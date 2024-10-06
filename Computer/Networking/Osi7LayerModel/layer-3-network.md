---
date: 2024-10-06 (Sun)
---

# Layer 3: Network

While layer 2 handles device-to-device communication within a LAN, layer 3
handles **_end-to-end_** communication across multiple networks.

## Overview

- Internet Protocol (IP), IP packets for cross network IP addresses
- Routing
- Routers - encapsulating in L2 on the way
- ARP - Find MAC address of an IP address
- No channels between a sender and receiver, src to dest IP only
- Can be delivered out of order

## IP Packets

### IPv4 Packets

- Time to Live (TTL): How many hops a packet can move through
- Protocol: L4 protocol, e.g. ICMP (1), TCP (6), UDP (17)
- Source IP Address
- Destination IP Address
- Data (Payload): L4 data

### IPv6 Packets

- Hop Limit: Replaces _TTL_ in IPv4
- Next Header: Replaces _Protocol_ in IPv4

## IP Addressing

### IPv4 Addresses

Dotted decimal notation: 4 octets, each 8 bits.

There are two parts: network and host.

#### Subnet Mask

**_Subnet mask_** is used to determine the network and host parts of an IP
address.

A `255.255.0.0` subnet mask or `/16` prefix means the first 16 bits are the
network part.

### Static vs Dynamic IP Address

Static IP addresses are manually assigned, while dynamic IP addresses are
assigned by a DHCP server.

## IP Routing

Routing is the process, where a packet is forwarded to the destination network
hop by hop across the internet.

If the destination IP address is not on the same network, the packet is sent to
a default gateway (router). Otherwise it is communicated locally.

## Router

Each routers have multiple interfaces, each connected to a different network.

They have routing tables to determine where to send packets.

| Destination    | Next Hop / Target |
| -------------- | ----------------- |
| 52.217.13.0/24 | 52.217.13.1       |
| 0.0.0.0/0      | 52.43.214.1       |
| 52.43.215.0/24 | 52.43.215.1       |

If multiple routes match the destination, the most specific route is chosen.

`0.0.0.0/0` is the default route that matches all IP address. As the least
specific route, if the destination is not in the routing table, the packet is
sent to the default route.

### Route Tables

They can be configured statically or dynamically through BGP (Border Gateway
Protocol).

## Address Resolution Protocol (ARP)

**_Address Resolution Protocol_** (**_ARP_**) is used to find the MAC address of
an IP address within the same network.

## 🧭 Navigation

- [🔼 Back to top](#layer-3-network)
- [◀️ Back](osi-7-layer-model.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
