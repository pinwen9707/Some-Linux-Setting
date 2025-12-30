# Ubuntu Server User, Group, and Permission Management Guide

This document provides a **clean and structured guide** for managing **users, groups, and permissions** on an Ubuntu Server.

---

## 1. Switch to Root User

```bash
sudo -i
```

This allows you to perform administrative tasks without repeatedly using `sudo`.

---

## 2. Create a New User (With Home Directory)

```bash
sudo adduser <USERNAME>
```

- A home directory will be created at:
  ```text
  /home/<USERNAME>
  ```
- You will be prompted to set a password and basic user information.

---

## 3. Add User to a Group

```bash
sudo adduser <USERNAME> <GROUPNAME>
```

### Common Group Practices

- **Regular users**  
  Add to a shared group (example):
  ```text
  research-users
  ```

- **Administrators**  
  Add to the `admin` group to grant `sudo` privileges:
  ```bash
  sudo adduser <USERNAME> admin
  ```

> Members of the `admin` group can execute commands as root using `sudo`.

---

## 4. Create a User Without a Home Directory

```bash
sudo useradd -s /bin/bash <USERNAME>
```

⚠️ This command **does NOT** create `/home/<USERNAME>` automatically.

To set a password manually:

```bash
sudo passwd <USERNAME>
```

---

## 5. List Group Memberships

To check which groups a user belongs to:

```bash
grep "<USERNAME>" /etc/group
```

---

## 6. Create a New Group

```bash
sudo groupadd <GROUPNAME>
```

---

## 7. Remove a User Account

```bash
sudo deluser -r <USERNAME>
```

- `-r` removes the user's home directory
- Useful when decommissioning accounts

---

## 8. Change a User Password

```bash
sudo passwd <USERNAME>
```

---

## 9. Remove a User from a Group

```bash
sudo deluser <USERNAME> <GROUPNAME>
```

---

## 10. File and Directory Permission Management

### Grant Full Access (Read / Write / Execute)

```bash
sudo chmod 777 <DIRECTORY_PATH>
```

⚠️ **Use with caution** — grants access to all users.

---

### Remove Write Permission

```bash
sudo chmod a-w <DIRECTORY_PATH>
```

---

### Example: Allow All Users to Access USB Media

```bash
sudo chmod -R 777 /media/
```

---

## 11. Grant Sudo Privileges to a Group

Edit the sudoers file:

```bash
sudo visudo
```

Add the following line:

```text
%<GROUPNAME> ALL=(ALL:ALL) ALL
```

Example:

```text
%research-admin ALL=(ALL:ALL) ALL
```

This allows all members of the group to run commands with `sudo`.

---

## Notes and Best Practices

- Prefer **group-based permission management** over per-user rules
- Avoid `chmod 777` unless necessary
- Always use `visudo` to edit `/etc/sudoers`
- For shared servers, maintain:
  - user groups (roles)
  - minimal privilege principle

---
