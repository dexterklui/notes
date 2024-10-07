---
date: 2024-10-07 (Mon)
---

# Network Address Translation

**_Network address translation_** (**_NAT_**) translates
[private **IPv4** addresses](ip-addresses.md#private-ip-addresses) to public

- Address IPv4 shortages
- Provides some security benefits

## Types of NAT

- **_Static NAT_**: 1 private to 1 fixed public address
- **_Dynamic NAT_**: a pool of public addresses, and private addresses are
  mapped to available public addresses
- **_Port address Translation_** (**_PAT_**): Many private to 1 public address
  (AWS: NATGW)

## Static NAT

The NAT device (router) maintains a NAT table, mapping 1:1 private to public
addresses.

In AWS, this is how the Internet Gateway (IGW) works.

## Dynamic NAT

The NAT device (router) maintains a NAT table, mapping private to public
addresses.

Public IP allocations are temporary allocations from a public IP pool.

If the public IP pool is exhausted, new private IP addresses will not be able to
access the internet until a public IP address is released.

## Port Address Translation (PAT)

The NAT device (router) records the private IP address and port number, and map
it to the same public IP address but with a different available public port
number, allocated from a pool which allows IP overloading (many to one).

In AWS, this is how the NATGateway (NATGW) functions.

This is also how home router works. This explains why you cannot initialize
connection from the internet to a device within your home network, because the
NAT device does not know which device to forward the incoming connection to.

## 🧭 Navigation

- [🔼 Back to top](#network-address-translation)
- [◀️ Back](networking.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)w
