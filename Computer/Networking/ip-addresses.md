---
date: 2024-10-07 (Mon)
---

# IP Addresses

## IPv4 Addresses

Dotted decimal notation: 4 octets, each 8 bits.

There are two parts: network and host.

### IPv4 Space

Totally 4,294,967,296 IPv4 addresses

- **_Class A_**: 0.0.0.0/1
  - 1 octet for network: 0.0.0.0/8 to 127.0.0.0/8 (128 networks)
- **_Class B_**: 128.0.0.0/2
  - 2 octets for network: 128.0.0.0/16 to 192.255.0.0/16 (16,384 networks)
- **_Class C_**: 192.0.0.0/4
  - 3 octets for network: 192.0.0.0/24 to 223.255.255.0/24 (2,097,152 networks)
- **_Class D_**: for multicast
- **_Class E_**: reserved

### Subnet Mask

Historically, IP addresses were only divided into classes, and we cannot further
divide them into sub-networks. With Classless Inter-Domain Routing (CIDR), we
can divide a network into sub-networks.

**_Subnet mask_** is used to determine the network and host parts of an IP
address.

A `255.255.0.0` subnet mask or `/16` prefix means the first 16 bits are the
network part.

We can further divide a network into sub-networks by borrowing bits from the
host part. This is called **_subnetting_**. For example:

- We start with 10.16.0.0/16
- Split to two: 10.16.0.0/17 and 10.16.128.0/17
- Further split each into two:
  - 10.16.0.0/18 and 10.16.64.0/18
  - 10.16.128.0/18 and 10.16.192.0/18

Usually networks are split into 2, 4, 8, etc. sub-networks. But you can also
have odd number splits as well.

### Private IP Addresses

Defined in _RFC1918_.

Private IP address space:

- 1 class A networks: 10.0.0.0/8
  - Usually used in cloud environment, chopped into smaller networks
- 16 class B networks: 172.16.0.0/12 (172.16.0.0/16 - 172.31.0.0/16)
- 256 class C networks: 192.168.0.0/16 (192.168.0.0/24 - 192.168.255.0/24)

## IPv6 Addresses

## Static vs Dynamic IP Address

Static IP addresses are manually assigned, while dynamic IP addresses are
assigned by a DHCP server.

## 🧭 Navigation

- [🔼 Back to top](#ip-addresses)
- [◀️ Back](networking.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
