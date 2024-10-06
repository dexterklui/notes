---
date: 2024-10-05 (Sat)
---

# OSI 7-Layer Model

Open Systems Interconnection (OSI) model is a conceptual framework that
standardizes the functions of a telecommunication or computing system into seven
abstraction layers.

## 7 Layers

One layer stacking on another forming a **_networking stack_**:

- **_Host layer_** - how data is chopped up and reassembled, and formatted
  - Layer 7: Application
  - Layer 6: Presentation
  - Layer 5: Session
  - [Layer 4: Transport](layer-4-transport.md)
- **_Media layer_** - how data is transmitted
  - [Layer 3: Network](layer-3-network.md)
  - [Layer 2: Data Link](layer-2-data-link.md)
  - [Layer 1: Physical](layer-1-physical.md)

While upper layer uses lower layers to communicate, conceptually each layer
communicates within the same layer.

## Layer device

A layer 1 device has capability for layer 1 data. A layer 2 device has
capabilities for **both** layer 1 and layer 2 data, and so on.

## 🧭 Navigation

- [🔼 Back to top](#osi-7-layer-model)
- [◀️ Back](../networking.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
