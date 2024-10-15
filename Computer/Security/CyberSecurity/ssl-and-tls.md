---
date: 2024-10-15 (Tue)
---

# SSL and TLS

## Overview

- **_SSL_**: **_Secure Socket Layer_**
- **_TLS_**: **_Transport Layer Security_** (SSL is the predecessor of TLS)
- SSL and TLS provide the following between client and server
  - **Privacy**: communication is encrypted
    - Asymmetric and then symmetric
  - **Identity verification**: for server or even client
  - **Data integrity**: data is not altered during transmission

## Phases

After TCP connection is established, these phases occur:

1. Cipher Suites
2. Authentication
3. Key exchange

### Cipher Suites

To agree on an encryption algorithm:

- Key exchange algorithm
- Bulk encryption algorithm
- Message Authentication Code (MAC) algorithm

These are what is done:

1. Client sends a list of cipher suites it supports
2. Server picks one and sends it back, with its certificate, which contains its
   public key

### Authentication

To ensure that the server's certificate and public key are authentic.

The certificate is signed by a **_Certificate Authority_** (**_CA_**).

OS and browsers have a list of trusted Certificate Authorities (CAs).

The client checks that:

- The certificate is signed by a trusted CA (can be a chain of CAs)
- The certificate is not expired
- The certificate is for the server it is talking to
- The certificate is not revoked

Then the client use the public key in the certificate to encrypt a message, and
wait for server's response to check if the server has the private key.

### Key Exchange

1. Client generates a premaster key, and send to the server.
2. Both use the agreed key exchange secret to generate the master secret.
3. Master secret is used throughout the session to create many session keys,
   which encrypt and decrypt the data.

## 🧭 Navigation

- [🔼 Back to top](#ssl-and-tls)
- [◀️ Back](../security.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
