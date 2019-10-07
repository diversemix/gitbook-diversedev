---
description: If your XPS fails or locks up after sleeping then try this
---

# Ubuntu Display Driver issue with DELL XPS

## Blacklist the driver

After much searching and angst I found that [these instructions worked from StackOverflow](https://askubuntu.com/a/951892)

```
cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf
sudo update-initramfs -u
sudo reboot
```





