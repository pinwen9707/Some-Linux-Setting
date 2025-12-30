# Ubuntu Server Disk Quota Configuration Guide

This document describes how to configure **disk quota** on an Ubuntu Server, specifically for the `/home` directory.

---

## 1. Identify the Partition Used by `/home`

```bash
df -h /home
```

Example output:

```text
Filesystem        Size  Used Avail Use% Mounted on
/dev/nvme0n1p2    3.6T   61G  3.4T   2% /
```

Confirm which partition (`/dev/nvme0n1p2` in this example) hosts `/home`.

---

## 2. Check Whether the Partition Is Mounted

```bash
mount | grep /dev/nvme0n1p2
```

Example output:

```text
/ dev/nvme0n1p2 on / type ext4 (rw,relatime,quota,usrquota,grpquota,errors=remount-ro)
```

If `usrquota` and `grpquota` are **not present**, quota is not yet enabled.

---

## 3. Enable Quota in `/etc/fstab`

Edit the fstab file:

```bash
sudo vi /etc/fstab
```

Locate the corresponding mount entry and add `usrquota,grpquota`:

```text
UUID=12345678-1234-1234-1234-112233445566  /  ext4  errors=remount-ro,usrquota,grpquota  0  1
```

> Replace the UUID and mount point with values from your system.

---

## 4. Remount the Partition

```bash
sudo mount -o remount /dev/nvme0n1p2
```

---

## 5. Configure User Disk Quota

```bash
sudo edquota -u <username>
```

Example editor view:

```text
Disk quotas for user b12345678:
  Filesystem        blocks   soft     hard   inodes  soft  hard
  /dev/nvme0n1p2       708  5000000  6000000      85     0     0
```

- **soft**: soft limit (can be exceeded temporarily)
- **hard**: hard limit (cannot be exceeded)
- Units:
  - `blocks`: KB
  - `inodes`: number of files

---

## 6. Set Grace Period for Soft Limits

```bash
sudo edquota -t
```

You can configure:
- block grace period
- inode grace period

Example prompt:

```text
Grace period before enforcing soft limits for users:
Time units may be: days, hours, minutes, or seconds
```

---

## 7. Verify Quota Configuration

```bash
sudo quota -uvs <username>
```

Example output:

```text
Filesystem        space   quota   limit   grace   files  quota  limit  grace
/dev/nvme0n1p2     708K   48829M  58594M           85      0      0
```

- The `grace` column shows remaining grace time if the soft limit is exceeded
- If empty, the user is within quota limits

---

## Notes

- Disk quota is commonly used in **multi-user, NFS, or NIS environments**
- Existing files are not affected; quota only limits future usage
- It is recommended to remount filesystems during low-usage periods

---
