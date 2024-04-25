---
date: 2024-04-18 (Thu)
---

# File System

## Format a USB

### File System Type

- _NTFS_ optimized for Windows
- _Ext4_ optimized for Linux
- exFAT for both

Note that exFAT and NTFS doesn't support Linux user:group privileged setting.

### Format a exFAT USB

1. Find the target USB device, e.g. `/dev/sdb`. Do necessary backup.
2. `wipefs --all /dev/sdb` to wipe all the signature and partition table.
3. `sudo dd if=/dev/zero of=/dev/sdb bs=1M` to wipe all the data. To be more
   secure but slower, use `/dev/urandom` (dunno if it's `urandom` or `random` or
   other, double check by googling) in `if=` parameter.
4. `sudo fdisk /dev/sdb`. Create a partition table, a partition, and change the
   new partition to the desired file system type (`11 - window storage` for
   `exFAT`).
5. Format the partition with `sudo mkfs.exfat -n MyLabel /dev/sdb1`

## 🧭 Navigation

- [🔼 Back to top](#)
- [📑 Notes Index](../../index.md)
