# NVIDIA Driver Clean Reinstall (Ubuntu)
## Important Notice (UEFI Secure Boot)
Before installing or reinstalling the NVIDIA driver, ensure that UEFI Secure Boot is disabled, or set the OS Type to ```Other OS``` in the BIOS/UEFI settings.
If Secure Boot remains enabled, third-party NVIDIA kernel modules may be blocked from loading, which can result in no display output, failed ```nvidia-smi``` execution, or DKMS module load errors even after a successful installation.

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
