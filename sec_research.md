# Android Security Research with ADB

This guide covers various security analysis techniques using ADB to investigate Android devices for potential security issues, malware, and system integrity.

## Table of Contents

1. [System Integrity Checks](#system-integrity-checks)
2. [Malware Detection](#malware-detection)
3. [Permission Analysis](#permission-analysis)
4. [Network Traffic Analysis](#network-traffic-analysis)
5. [App Forensics](#app-forensics)
6. [Root Detection](#root-detection)

## System Integrity Checks

### Check System Partition Integrity
```bash
# View mount points and permissions
adb shell mount

# Check system partition for write access (shouldn't be writable)
adb shell mount | grep system

# Verify SELinux status
adb shell getenforce
```

### Verify System App Signatures
```bash
# List all system apps
adb shell pm list packages -s

# Check specific app signature
adb shell pm dump <package_name> | grep -A 5 "Signatures"
```

## Malware Detection

### Check for Suspicious Apps
```bash
# List all third-party apps
adb shell pm list packages -3

# Show app installation sources
adb shell pm list packages -i

# Check for apps with dangerous permissions
adb shell pm list permissions -g -d
```

### Examine Running Services
```bash
# List all running services
adb shell dumpsys activity services

# Check running processes
adb shell ps -A

# View CPU usage by processes
adb shell top -m 10
```

## Permission Analysis

### List App Permissions
```bash
# Show all permissions used by an app
adb shell dumpsys package <package_name> | grep permission

# List apps using specific permission
adb shell pm list permissions -g -d | grep <permission_name>
```

### Review Dangerous Permissions
```bash
# List all dangerous permissions
adb shell pm list permissions -g -d

# Check which apps use location permission
adb shell dumpsys location | grep -A 1 "Installed applications"
```

## Network Traffic Analysis

### Monitor Network Connections
```bash
# List all network connections
adb shell netstat

# Check current network statistics
adb shell dumpsys netstats

# Monitor real-time network activity
adb shell tcpdump -n
```

### Check App Network Usage
```bash
# View app network permissions
adb shell dumpsys package | grep INTERNET

# Check network usage stats
adb shell dumpsys netpolicy
```

## App Forensics

### Extract App Data
```bash
# Pull app data (requires root)
adb pull /data/data/<package_name>/ ./forensics/

# Extract shared preferences
adb pull /data/data/<package_name>/shared_prefs/ ./forensics/prefs/
```

### Analyze App Storage
```bash
# List app directories
adb shell ls -la /data/data/<package_name>/

# Check database files
adb shell ls -la /data/data/<package_name>/databases/
```

## Root Detection

### Check for Root Indicators
```bash
# Check for su binary
adb shell which su

# Look for common root apps
adb shell pm list packages | grep -E "supersu|magisk"

# Check for modified system properties
adb shell getprop | grep -E "ro.debuggable|ro.secure"
```

### Verify Boot Status
```bash
# Check boot state
adb shell getprop ro.boot.verifiedbootstate

# Verify dm-verity status
adb shell getprop ro.boot.veritymode
```

## Important Security Notes

1. Always obtain proper authorization before performing security analysis
2. Document all findings and maintain chain of custody
3. Use isolated testing environments when possible
4. Be aware that some commands require root access
5. Follow responsible disclosure procedures for any vulnerabilities found

## Additional Resources

- [Android Security Bulletins](https://source.android.com/security/bulletin)
- [OWASP Mobile Security Testing Guide](https://owasp.org/www-project-mobile-security-testing-guide/)
- [Android Security Internals](https://source.android.com/security)

## Legal Disclaimer

This guide is for educational and authorized security research purposes only. Ensure you have proper permission before performing any security analysis on devices or applications you don't own.