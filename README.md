# Android Debug Bridge (ADB) Cheatsheet

A comprehensive collection of guides and commands for Android Debug Bridge (ADB) operations.

## Table of Contents

### 1. Data Management
- [Data Backup and Restore](adb_data.md)
  - Full device backup
  - App-specific backup
  - Data transfer (push/pull)
  - File system navigation
  - Package management

### 2. Security Research
- [Security Analysis Tools](sec_research.md)
  - Malware detection
  - System file integrity checks
  - Permission analysis
  - Network traffic monitoring
  - App forensics
  - Root detection

### 3. Custom ROM Installation
- [LineageOS Installation Guide](install_lineage.md)
  - Bootloader unlocking
  - Recovery installation
  - ROM flashing
  - GApps installation
  - System verification

### 4. Advanced Projects
- [Windows ARM on OnePlus 6](install_windows_op6.md)
  - Dual boot setup
  - Partition management
  - Windows ARM installation
  - Driver installation
  - Boot configuration

## Prerequisites

1. Install ADB Tools
   ```bash
   # Windows:
   Download from [Android SDK Platform Tools](https://developer.android.com/studio/releases/platform-tools?hl=de)
   # Linux:
   sudo apt-get install adb
   # macOS:
   brew install android-platform-tools
   ```

2. Enable USB Debugging on your Android device:
   - Settings → About Phone → Tap "Build Number" 7 times
   - Settings → Developer Options → Enable "USB Debugging"

3. Verify ADB Connection:
   ```bash
   adb devices
   ```

## Important Notes

- Always backup your data before performing system-level operations
- Some operations may void your warranty
- Root access may be required for certain operations
- Follow device-specific instructions when available

## Contributing

Feel free to contribute to this repository by:
1. Creating pull requests for new guides
2. Improving existing documentation
3. Adding device-specific instructions
4. Reporting issues or suggesting improvements

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
