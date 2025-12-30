# Ubuntu Server Disk Mounting Guide

This document describes how to **mount and unmount a disk** on an Ubuntu Server after physically inserting the drive.

---

## 1. Identify the Newly Inserted Disk

After inserting a new hard drive, use the following command to list all block devices:

```bash
lsblk
```

### Example Output (Anonymized)

```text
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0         7:0    0    74M  1 loop /snap/core22/XXXX
loop1         7:1    0     4K  1 loop /snap/bare/XXXX
loop2         7:2    0    74M  1 loop /snap/core22/XXXX
loop3         7:3    0 250.4M  1 loop /snap/firefox/XXXX
loop4         7:4    0 250.8M  1 loop /snap/firefox/XXXX
loop5         7:5    0  18.5M  1 loop /snap/firmware-updater/XXXX
loop6         7:6    0  16.4M  1 loop /snap/firmware-updater/XXXX
loop7         7:7    0 516.2M  1 loop /snap/gnome-42-2204/XXXX
loop8         7:8    0   516M  1 loop /snap/gnome-42-2204/XXXX
loop9         7:9    0  91.7M  1 loop /snap/gtk-common-themes/XXXX
loop10        7:10   0  10.8M  1 loop /snap/snap-store/XXXX
loop11        7:11   0  17.5M  1 loop /snap/snap-store/XXXX
loop12        7:12   0  50.8M  1 loop /snap/snapd/XXXX
loop13        7:13   0  50.9M  1 loop /snap/snapd/XXXX
loop14        7:14   0   576K  1 loop /snap/snapd-desktop-integration/XXXX
sda           8:0    0   7.3T  0 disk
└─sda1        8:1    0   7.3T  0 part /HDD
sdb           8:16   0   4.5T  0 disk
└─sdb1        8:17   0   4.5T  0 part /mnt/usb1
sdc           8:32   0   4.5T  0 disk
└─sdc1        8:33   0   4.5T  0 part
nvme0n1     259:0    0   1.8T  0 disk
├─nvme0n1p1 259:1    0     1G  0 part /boot/efi
└─nvme0n1p2 259:2    0   1.8T  0 part /
```

### How to Interpret

- Newly inserted disks typically appear as **`/dev/sdX`** (e.g., `sda`, `sdb`, `sdc`)
- In this example, **`sdc`** is the newly detected disk
- The partition to mount is usually **`/dev/sdc1`**
- No mount point is shown, indicating the disk is **not yet mounted**

---

## 2. Create a Mount Point (If Needed)

```bash
sudo mkdir -p /mnt/usb2
```

You may replace `/mnt/usb2` with any directory name you prefer.

---

## 3. Mount the Disk

```bash
sudo mount /dev/sdc1 /mnt/usb2
```

Once mounted, the disk will be accessible at:

```text
/mnt/usb2
```

You can verify using:

```bash
df -h | grep sdc
```

---

## 4. Unmount the Disk (Safe Removal)

Before physically removing the disk, always unmount it:

```bash
sudo umount /mnt/usb2
```

After this, the disk can be safely removed.

---

## Notes

- Always ensure no process is using the mount point before unmounting
- If `umount` fails, check open files with:
  ```bash
  lsof +D /mnt/usb2
  ```
- Persistent mounting across reboots requires configuration in `/etc/fstab` (not covered in this document)

---
