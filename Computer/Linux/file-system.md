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

## Growing a Disk Partition

To grow a disk partition (or called disk slice):

1. Redefine the partition table to make the partition size bigger
   - Delete old partition and create a new one with the same starting sector but
     a later ending sector
2. Use `partprobe` to inform the OS of the partition table changes
3. Grow the actual file system structure.
   - For XFS file system, use `xfs_growfs {mountpoint|device}`
   - For ext4 file system, use `resize2fs {device}`

## Logical Volume

### Logical Volume Management

**_Logical Volume Management_** (**_LVM_**) is a method of managing disk space
that abstracts away the physical disk layout. It allows creating virtual disks
called **_logical volumes_** (**_LVs_**), which use multiple **_physical
volumes_** (**_PVs_**) (a whole disk or just a partition) spanning across
multiple physical disks.

Physical volumes are combined into **_volume groups_** (**_VGs_**), which is a
pool of storage.

When a PV is added to a VG, the PV is divided into physical **_extents_** each
with the same size (usually 4MB). When a new LV is created or extended, physical
extents are allocated from the VG to the LV as the logical extents.

### Creating a Logical Volume

1. Define space as a physical volume (PV) `pvcreate /dev/sdc`
   - Check with `pvdisplay`
2. Create a volume group (VG) using PVs `vgcreate newVG /dev/sdc /dev/sdd`
   - Check with `vgdisplay`
   - Use `vgextend` to add more PVs to an existing VG
3. Create a logical volume (LV) `lvcreate -n newLV -L 100M newVG`
   - Check with `lvdisplay`
   - For `lvcreate`:
     - `-n` specifies the name
     - `-L` specifies the size
     - `-l` specifies the number of extents

### Using a Logical Volume

1. Format the logical volume `mkfs.ext4 /dev/newVG/newLV`
2. Mounting the logical volume `mount /dev/newVG/newLV /mnt/newLV`

### Growing a Logical Volume

1. Extend the logical volume `lvextend -L +100M newVG/newLV`
2. Grow the actual file system structure `xfs_growfs {device}` (for XFS) or
   `resize2fs {device}` (for ext4)

## 🧭 Navigation

- [🔼 Back to top](#)
- [📑 Notes Index](../../index.md)
