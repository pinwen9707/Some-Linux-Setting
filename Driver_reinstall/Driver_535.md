# NVIDIA Driver Clean Reinstall (Ubuntu)

## 1. Remove existing NVIDIA drivers and related DKMS modules
```bash
sudo apt purge 'nvidia-*'
sudo apt autoremove -y
sudo rm -rf /var/lib/dkms/nvidia*
```

## 2. Update package list
```bash
sudo apt update
```

## 3. Install the specified NVIDIA driver version
```bash
sudo apt install nvidia-driver-535
```

## 4. Reboot the system
```bash
sudo reboot
```
