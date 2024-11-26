---
date: 2024-11-25 (Mon)
---

# IPSec VPN

To create a secure connection between two networks across an insecure internet.

## Interesting Traffic

The traffic that is matched and being sent through a particular VPN tunnel is
called **_interesting traffic_**. When there is no interesting traffic, the VPN
tunnel is torn down.

## Two Main Phases

**_Internet Key Exchange_** (**_IKE_**) is used to establish the tunnel. There
are two phases:

- IKE Phase 1: to exchange key and a channel to communicate for phase 2. It is
  slow and heavy part of the process.
- IKE Phase 2: to get VPN up and running using phase 1 keys and channels. It is
  fast and agile.

Phase 2 tunnels can be torn down when there's no _interesting traffic_. But
Phase 1 tunnel can remain, and it's used to re-establish Phase 2 tunnels when
needed.

### IKE Phase 1

- Authenticate (using certificates or pre-shared key)
- Use [**_Diffie-Hellman_**](#diffe-hellman-dh-key-exchange) (**_DH_**) key
  exchange to create a phase 1 symmetric key.
- The phase 1 tunnel, a.k.a. the **_IKE Security Association_** (**_SA_**), is
  established, where traffic is encrypted and decrypted using the phase 1
  symmetric key.

### IKE Phase 2

1. Symmetric key is used to encrypt and decrypt agreements and pass more key
   material
   - One peer informs the cipher suite that it supports, and the other peer
     picks the best that it also supports.
2. DH key and exchanged key material is used to create symmetric **_IPSec
   key_**.
   - The _phase 2 VPN tunnel_, a.k.a. the **_IPSec SA_** (or _SA pair_), is
     establish, where IPSec key is used for bulk encryption and decryption of
     interesting traffic

## Two Types of VPN

Two types of VPN differ in how they match interesting traffic.

- **_Policy-based VPN_**

  - Match traffic based on rules
  - Distinguish traffic type even if they are between the same networks using
    the **same phase 1 tunnel**
  - But each traffic type uses a **separate IPSec SA pair** (phase 2 tunnel)
    with different rules/security setting

- **_Route-based VPN_**

  - Match traffic using a routing table (e.g. matching 192.168.0.0/24)
  - All traffic between the same networks uses the same IPSec SA

With Policy-based VPN, multiple phase 2 tunnels can be established on top of one
phase 1 tunnel. But with Route-based VPN, only one phase 2 tunnel is established
on one phase 1 tunnel.

## Diffie-Hellman (DH) Key Exchange

1. Each side creates their own DH private key
2. Each side uses their DH private key to derive their own DH public key
3. Both sides exchange the DH public key
4. Each side uses their own DH private key and the other side's DH public key to
   derive the common secret, a.k.a. **_shared DH key_**
5. The shared DH key is used to exchange key material and agreements
6. Use DH key and exchange key material to derive the same final **_Phase 1
   symmetric key_** independently

## 🧭 Navigation

- [🔼 Back to top](#ipsec-vpn)
- [◀️ Back](networking.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
