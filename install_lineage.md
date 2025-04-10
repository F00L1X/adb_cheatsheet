# Installing LineageOS Using ADB

This guide provides step-by-step instructions for installing LineageOS on Android devices using ADB and fastboot commands.

## Prerequisites

1. Download required files:
   - Latest LineageOS build for your device
   - Custom Recovery (TWRP/LineageOS Recovery)
   - GApps package (optional)
   - ADB and fastboot tools installed

2. Enable USB debugging and OEM unlocking on your device
3. Backup all important data
4. Charge your device to at least 70%

## Table of Contents

1. [Prepare Your Device](#prepare-your-device)
2. [Unlock Bootloader](#unlock-bootloader)
3. [Install Custom Recovery](#install-custom-recovery)
4. [Install LineageOS](#install-lineageos)
5. [Post-Installation](#post-installation)
6. [Troubleshooting](#troubleshooting)

## Prepare Your Device

1. Enable Developer Options:
   ```bash
   # Settings → About Phone → Tap "Build Number" 7 times
   ```

2. Enable USB Debugging and OEM Unlock:
   ```bash
   # Settings → Developer Options → Enable "USB Debugging" and "OEM Unlocking"
   ```

3. Verify ADB Connection:
   ```bash
   adb devices
   ```

## Unlock Bootloader

1. Boot into bootloader:
   ```bash
   adb reboot bootloader
   ```

2. Verify fastboot connection:
   ```bash
   fastboot devices
   ```

3. Unlock bootloader:
   ```bash
   fastboot flashing unlock
   # Or for older devices
   fastboot oem unlock
   ```

4. Confirm unlock on device screen

## Install Custom Recovery

1. Download appropriate recovery image for your device

2. Boot into bootloader if not already there:
   ```bash
   adb reboot bootloader
   ```

3. Flash recovery:
   ```bash
   fastboot flash recovery <recovery_image>.img
   ```

4. Boot into recovery:
   ```bash
   fastboot reboot recovery
   ```

## Install LineageOS

1. Wipe device in recovery:
   - Wipe → Format Data
   - Advanced Wipe → Select System, Data, Cache, Dalvik/ART Cache

2. Transfer LineageOS ZIP:
   ```bash
   adb push lineage-<version>-<device>.zip /sdcard/
   ```

3. If installing GApps:
   ```bash
   adb push <gapps-package>.zip /sdcard/
   ```

4. Install from recovery:
   - Install ZIP → Select LineageOS ZIP
   - Install ZIP → Select GApps ZIP (if using)

5. Wipe cache/dalvik:
   - Advanced Wipe → Cache, Dalvik/ART Cache

6. Reboot system:
   ```bash
   adb reboot system
   ```

## Post-Installation

1. Verify system integrity:
   ```bash
   adb shell getprop ro.build.version.release
   adb shell getprop ro.lineage.version
   ```

2. Re-enable device security:
   ```bash
   fastboot flashing lock  # If desired
   ```

3. Setup device encryption (optional):
   - Settings → Security → Encrypt Phone

## Troubleshooting

### Common Issues

1. Device not detected:
   ```bash
   # Check USB connection
   adb devices
   fastboot devices

   # Kill and restart ADB server
   adb kill-server
   adb start-server
   ```

2. Recovery boot loop:
   ```bash
   # Force reboot to bootloader
   adb reboot bootloader

   # Reflash recovery
   fastboot flash recovery <recovery_image>.img
   ```

3. Installation errors:
   ```bash
   # Verify ZIP integrity
   adb shell md5sum /sdcard/lineage-*.zip

   # Clear cache and try again
   adb shell twrp wipe cache
   adb shell twrp wipe dalvik
   ```

### Recovery Commands

```bash
# Boot directly to recovery
adb reboot recovery

# Sideload installation (alternative to push)
adb sideload lineage-<version>-<device>.zip

# View recovery logs
adb shell cat /tmp/recovery.log
```

## Important Notes

1. **Backup**: Always backup your data before proceeding
2. **Battery**: Ensure sufficient battery charge
3. **Compatibility**: Verify LineageOS build matches your device exactly
4. **Security**: Re-enable security features after installation
5. **Support**: Check LineageOS wiki for device-specific instructions

## Additional Resources

- [LineageOS Wiki](https://wiki.lineageos.org/)
- [TWRP Official Website](https://twrp.me/)
- [OpenGApps](https://opengapps.org/)

## Warning

This process will erase all data on your device and may void your warranty. Proceed at your own risk and ensure you understand each step before executing it.