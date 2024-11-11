---
date: 2024-04-18 (Thu)
---

# File System

## Device File

Located in `/dev` directory. `/dev/sd*` is a device file for a hard disk.
`/dev/xvd*` is a device file for a virtual disk. `/dev/nvme*` is a device file
for a NVMe disk.

`ls -l {device-file}` shows details of a device file

- E.g. `ls -l /dev/sd*`
- Leading `b` in the first column (of permission) indicates a block device, `c`
  indicates a character device
- After the group `disk`, it shows the major and minor number
  - Major number indicates the driver used to communicate with the device
  - Minor number identifies the device managed by the same driver, starting from
    `0`

### Block vs Character Device

System assigns block devices with a buffer cache, which flushes the data when
the buffer is full or after a period of time (or when explicitly requested).

Character device doesn't have a buffer cache. System read and writes character
by character. Keyboards are typical of character devices.

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
