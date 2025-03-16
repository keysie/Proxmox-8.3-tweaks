# Fixing Realtek USB 3.0 to 2.5G Ethernet Dongle r8156

- Verify hardware: run `lsusb` and look for `ID 0bda:8156 Realtek Semiconductor Corp. USB 10/100/1G/2.5G LAN`
- Check current kernel driver version: `modinfo r8152 | grep ^version:`
  If output is smth like `1.12.13` you might want to upgrade. latest is smth like `2.19.2`
- follow [this guide](https://github.com/awesometic/realtek-r8152-dkms) to get the new module.
  Note: some dependencies like kernel headers, build-essential etc. are required before this process can be successful.
