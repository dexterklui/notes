---
date: 2024-10-10 (Thu)
---

# DDoS

Distribution Denial of Service (DDoS) is a type of cyber-attack where the
attacker floods the target with an overwhelming amount of traffic, rendering the
target's services unavailable.

## Botnets

A **_botnet_** is a network of compromised devices that are under the control of
a malicious actor. The attacker uses these devices to launch a DDoS attack.

## Types of DDoS Attacks

- **_Application Layer Attacks_**
- **_Protocol Attacks_**
- **_Volumetric / Amplification Attacks_**

### Application Layer Attacks

An example is HTTP Flood.

It capitalizes on the computational imbalance between the attacker and the
target: a request is simple to make but processing it is expensive.

### Protocol Attacks

An example is SYN Flood. The attacker sends a large number of SYN packets with
spoofed IP addresses to the target, to initialize a 3-way handshake.

It exhausts target resources by keeping the connection half-open.

### Volumetric / Amplification Attacks

An example is DNS Amplification. The attacker sends a lot of DNS request while
spoofing IP address to be that of the target server.

This leverages the fact that DNS responses contain more data than the requests.
Unlike the previous two types of DDoS, this attack is not about overwhelming the
target, but about overwhelming the network.

The size of botnets can be smaller compared to the other two types of DDoS.

## 🧭 Navigation

- [🔼 Back to top](#ddos)
- [◀️ Back](../security.md)
- [📑 Notes Index](../../../index.md)
