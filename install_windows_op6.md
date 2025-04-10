# Installing Windows ARM on OnePlus 6 (Dual Boot with LineageOS)

This guide provides instructions for installing Windows ARM on the OnePlus 6 (enchilada) while maintaining a dual boot setup with LineageOS.

## Prerequisites

1. OnePlus 6 with unlocked bootloader
2. LineageOS already installed
3. TWRP recovery installed
4. Windows ARM image (Windows 10 or 11)
5. Required drivers and tools:
   - ADB and fastboot tools
   - OnePlus 6 drivers
   - Windows ARM drivers for OnePlus 6
   - Renegade Project tools

## Table of Contents

1. [Prepare Your Device](#prepare-your-device)
2. [Partition Setup](#partition-setup)
3. [Install Windows ARM](#install-windows-arm)
4. [Configure Dual Boot](#configure-dual-boot)
5. [Post-Installation](#post-installation)
6. [Troubleshooting](#troubleshooting)

## Prepare Your Device

1. Backup all important data
2. Ensure at least 64GB free space
3. Charge device fully
4. Boot into TWRP:
   ```bash
   adb reboot recovery
   ```

## Partition Setup

1. Create Windows partition in TWRP:
   ```bash
   adb shell
   sgdisk --new=0:0:+32G /dev/block/sda  # Adjust size as needed
   sgdisk --typecode=0:0700 /dev/block/sda
   ```

2. Format the new partition:
   ```bash
   mkfs.ntfs -f /dev/block/sda<X>  # Replace X with partition number
   ```

3. Verify partition table:
   ```bash
   sgdisk --print /dev/block/sda
   ```

## Install Windows ARM

1. Boot into fastboot:
   ```bash
   adb reboot bootloader
   ```

2. Flash modified bootloader:
   ```bash
   fastboot flash abl abl-windows.img
   fastboot flash xbl xbl-windows.img
   ```

3. Push Windows files:
   ```bash
   adb push windows_arm/* /sdcard/Windows/
   ```

4. Install Windows from TWRP:
   ```bash
   adb shell install-windows.sh
   ```

## Configure Dual Boot

1. Install dual boot manager:
   ```bash
   adb push bootmanager.img /sdcard/
   fastboot flash boot bootmanager.img
   ```

2. Configure boot entries:
   ```bash
   adb shell configure-dualboot.sh
   ```

3. Verify boot configuration:
   ```bash
   adb shell cat /proc/cmdline
   ```

## Post-Installation

1. Install Windows drivers:
   ```bash
   # In Windows ARM
   dism /online /add-driver /driver:C:\drivers\oneplus6 /recurse
   ```

2. Configure Windows:
   - Setup Wi-Fi and cellular
   - Install updates
   - Configure hardware features

3. Test dual boot functionality:
   ```bash
   # Switch to LineageOS
   adb reboot lineage

   # Switch to Windows
   adb reboot windows
   ```

## Hardware Support Status

| Component | Status | Notes |
|-----------|--------|-------|
| Display | Working | Native resolution |
| Touch | Working | Basic functionality |
| Wi-Fi | Working | Requires drivers |
| Cellular | Partial | Data works, calls WIP |
| Bluetooth | Working | Requires drivers |
| GPS | Working | Requires drivers |
| Sensors | Partial | Basic sensors work |
| Camera | Not Working | WIP |
| Audio | Partial | Speaker only |
| USB | Working | OTG supported |

## Known Issues

1. Sleep/wake reliability issues
2. Battery drain higher than Android
3. Camera not functional
4. Some sensors may not work
5. Performance limitations

## Troubleshooting

### Boot Issues
```bash
# Force boot to LineageOS
fastboot boot lineage-boot.img

# Force boot to Windows
fastboot boot windows-boot.img

# Recovery mode
fastboot boot twrp.img
```

### Windows ARM Issues
```bash
# Check Windows partition
adb shell blkid

# Repair Windows boot
adb shell repair-windows.sh
```

### Dual Boot Issues
```bash
# Reset boot manager
fastboot flash boot stock-boot.img
fastboot flash boot bootmanager.img

# Clear boot selection
adb shell rm /sdcard/.bootselect
```

## Important Notes

1. This is an experimental project
2. Some features may not work properly
3. Regular backups are essential
4. Windows ARM updates may break functionality
5. Keep recovery tools readily available

## Additional Resources

- [Renegade Project](https://renegade-project.org/)
- [Windows ARM Documentation](https://docs.microsoft.com/windows/arm/)
- [OnePlus 6 XDA Forum](https://forum.xda-developers.com/oneplus-6)

## Warning

This process is experimental and may damage your device. Proceed at your own risk. Neither LineageOS nor OnePlus officially support running Windows ARM on the OnePlus 6.