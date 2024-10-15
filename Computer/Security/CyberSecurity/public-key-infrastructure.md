---
date: 2024-10-15 (Tue)
---

# Public Key Infrastructure

## Definition

**_Public Key Infrastructure_** (**_PKI_**) is a set of hardware, software,
people, policies, and procedures needed to create, manage, distribute, use,
store, and revoke **_digital certificates_**.

A digital certificate is a digital document that provides proof of the identity
of the certificate holder.

## Components

1. **_Certificate Authority_** (**_CA_**): issues digital certificates.
2. **_Registration Authority_** (**_RA_**): verifies the identity of certificate
   holders.
3. **_Validation Authority_** (**_VA_**): checks the validity of certificates.

## Digital Signature

The digital signature is a **encrypted hash** of the **to-be-signed** data.

Encryption is done with the private key of the CA.

When a client wants to verify that the certificate is authentic, it can
calculate the hash of the to-be-signed portion of the certificate, and compare
it with the decrypted digital signature using the public key of the CA.

## 🧭 Navigation

- [🔼 Back to top](#public-key-infrastructure)
- [◀️ Back](../security.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
