---
title: GPG Key
date: 2023-07-27 (Thu)
---

# GPG Overview

**_GnuPG_** users has a _primary keypair_ and then zero or more additional
_subordinate keypairs_. Primary and subordinate keypairs are bundled to
facilitate key management and the bundled can often be considered simply as one
keypair.

# Generating Keypair

`--genkey` generates a new keypair interactively.

# Managing Keys

## Inspection

- `--edit-key <keypair>` views a keypair.
- `--list-keys`
- `--list-public-keys`
- `--list-secret-keys`

Can change key format of all commands above: `--keyid-format=long`.

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](../index.md)
- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
- [🗃️ Master Index](../../index.md)
