**Date**: Thursday, May 22, 2025, 9:00 PM

## Topics Covered:
- Linux File System and Partition Management 
- Partition Table Type and Max Partition Size
- Linux Partition ID

## 1. File System
- Windows
	- FAT32 (File allocation Table)
	- NTFS(New Technology File System)
- Linux: 
	- ext2
	- ext3
	- **ext4** 
	- **xfs**
	- zfs
	- swap
	- vfat
	- btrfs

## 2. Partition Table Type
- BIOS(Basic Input Output System): 
	- MBR(Master Boot Record)
		- Problem in MBR: max 2 TB in a single partition
	- UEFI: GPT => Max: 9.4 ZB

## 3. Linux Partition ID

**❗Note:** Ext3, Ext4, XFS => 83

## 4. See free disk space
```bash
df -Th
```
- `T` : Print File system Type
- `h` : Human readable

**MBR**: Max primary partition => 4
		Extended partition => Many


### ❓Ques: Create a 15G Primary partition on `/home/pdisk` and use XFS File system (insert a new disk)

**Solution**: 

Mount on `/home/pdisk`:
```bash
mkdir -p /home/pdisk
```
- `-p` : Parent (Create parent folders as necessary)

**Note:**
- Small size(20G) so, MBR is enough. 
- MBR => `fdisk` and GPT => `gdisk`

**Steps:**
1. `fdisk /dev/nvme0n2`
2. `n`
3. `p`
4. `partprobe` : update partition table **\[Ignore error for NVMe\]**
5. `lsblk -f`: see file system too. we notice there is no file system formatted
6. `mkfs.ext4 /dev/nvme0n2p1`
7. `mount /dev/nvme0n2p1 /home/pdisk/`
	1. or use Block ID. To get block ID `blkid /dev/nvme0n2p1`
8. Edit `/etc/fstab` file for permanent writing
9. Check any error with `mount -a`
10. DONE!

### ❓Ques: Add 2 more 20G HDD, and make a single big partition.

See Free space in nvme0n2
```bash
parted /dev/nvme0n2
p free
```

**Step 1: Create LVMs**

**Step 2: Make each of hose LVM partitions a PV(physical volume)**

```bash
pvcreate /dev/nvme0n2p5 /dev/nvme0n3p5 /dev/nvme0n4p5
```

**Step 3:**