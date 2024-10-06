---
date: 2024-10-05 (Sat)
---

# Layer 1: Physical

Layer 1 (Physical) specifications define the transmission and reception of **raw
bit streams** between a device and a shared [physical medium](#physical-medium).

## Physical Medium

Physical medium can be copper (electrical), fibre (light) or WIFI (RF).

Specifications defines things like voltage levels, timing of voltage changes,
physical data rates, distances, modulation and connectors.

So for two devices to be able to communicate with each other, they need to use a
shared physical medium, and follow the same specifications. E.g. same frequency
range and same type of antennas.

## Hub

Consider using a cable to connect multiple devices. The number of cables needed
is $\frac{n(n-1)}{2}$. With a **_hub_**, only one cable is needed for each
device.

Anything received on any port of a hub is forwarded to all other ports,
including errors and collisions.

## Characteristics

Layer 1 is said to have **_one broadcast and one collision domain_**, because
there is no device addressing, only one device can send at a time.

A layer 1 network cannot scale very well.

### No Device Addressing

On layer 1, there is no device addressing. The data is sent to all devices on
the network. It's like shouting in a room full of people.

### Only One Device Can Send at a Time

A collision occurs when two devices send data at the same time. The data gets
corrupted and becomes useless.

### No Media Access Control

There is no **_media access control_**: cannot control which device can send
data at what time.

### Cannot Detect Collisions

Cannot detect any error or collision.

## 🧭 Navigation

- [🔼 Back to top](#layer-1-physical)
- [◀️ Back](osi-7-layer-model.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
