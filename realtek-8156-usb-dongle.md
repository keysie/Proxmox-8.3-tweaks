# Fixing Realtek USB 3.0 to 2.5G Ethernet Dongle r8156

- Verify hardware: run `lsusb` and look for `ID 0bda:8156 Realtek Semiconductor Corp. USB 10/100/1G/2.5G LAN`
- Check current kernel driver version: `modinfo r8152 | grep ^version:`
  If output is smth like `1.12.13` you might want to upgrade. latest is smth like `2.19.2`
- Install pre-requisites:
  `sudo apt-get install dh-dkms dkms build-essential pve-headers`
- Download the latest release form [here](https://github.com/awesometic/realtek-r8152-dkms)
- Install it using `dpkg -i <filename.deb>`
- ideally, `reboot`

## After upgrade of kernel version (or sometimes after reboot)

- Use `dmesg | grep r8152` to see if/what is wrong
- Use `dkms status` to see versions and full names of the packages in question
- Usually, it is an issue with the dkms module not being rebuilt correctly. Fix like this:
  - Make sure kernel headers are installed and up to date wiht `sudo apt install pve-headers`
  - Remove current module using `sudo dkms remove realtek-r8152/2.19.2` (or similar)
  - Rebuild/reinstall with `sudo dkms install realtek-r8152/2.19.2` (or similar)
  - Load module using `sudo modprobe r8152`
  - Reload pve interfaces with `ifreload -a`
  - (Maybe adjust naming of interfaces in your bridges etc.)
