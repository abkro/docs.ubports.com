---
title: "Common Issues and Troubleshooting"
weight: 3
description: "Solutions for common problems encountered during Ubuntu Touch porting"
---

# Common Issues and Troubleshooting

## Quick Solutions Table

| Issue | Common Cause | Quick Solution |
|-------|--------------|----------------|
| Kernel fails to build | Missing dependencies | Run `make oldconfig` and install missing packages |
| Device doesn't boot | Incorrect kernel config | Check `CONFIG_ANDROID_BINDERFS` and other Android options |
| Black screen after boot | Display driver issues | Verify kernel DRM/KMS configs |
| ADB not detecting device | udev rules or permissions | Update udev rules and add user to `adb` group |
| System partition mounting fails | Incorrect fstab entries | Check `/etc/fstab` against stock Android config |

## Build Issues

### Kernel Build Failures

#### Missing Dependencies
```bash
# Install common missing dependencies
sudo apt install \
    libssl-dev \
    flex \
    bison \
    build-essential \
    libncurses-dev
```

#### Compilation Errors
1. **Common Error**: `No rule to make target 'debian/canonical-certs.pem'`
   ```bash
   # Solution: Disable certificate checking in kernel config
   scripts/config --set-str SYSTEM_TRUSTED_KEYS ""
   scripts/config --set-str SYSTEM_REVOCATION_KEYS ""
   ```

2. **GCC Version Mismatch**
   ```bash
   # Check GCC version
   gcc --version
   
   # Install specific version if needed
   sudo apt install gcc-9 g++-9
   
   # Set specific version for build
   export CC=gcc-9
   export CXX=g++-9
   ```

### System Image Build Issues

#### Out of Memory
```bash
# Increase swap space
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

#### Space Issues
```bash
# Clean build directories
make clean
ccache -C

# Check space usage
du -sh build/
df -h
```

## Device Boot Issues

### Common Boot Problems

#### Device Stuck at Bootloader
1. Verify bootloader unlocking:
   ```bash
   fastboot flashing get_unlock_ability
   fastboot oem unlock # if needed
   ```

2. Check fastboot devices:
   ```bash
   fastboot devices
   ```

#### Failed to Boot
1. Collect logs:
   ```bash
   # From recovery
   adb pull /proc/last_kmsg
   adb pull /sys/fs/pstore/console-ramoops
   
   # From fastboot
   fastboot oem crash_report
   ```

2. Common kernel config issues:
   ```bash
   # Essential configs to check
   CONFIG_ANDROID_BINDERFS=y
   CONFIG_ANDROID_BINDER_IPC=y
   CONFIG_ANDROID_BINDER_DEVICES="binder,hwbinder,vndbinder"
   ```

## Hardware Support Issues

### Display Problems

#### Black Screen After Boot
1. Check kernel configs:
   ```bash
   # Essential display configs
   CONFIG_DRM=y
   CONFIG_DRM_KMS_HELPER=y
   CONFIG_DRM_PANEL=y
   ```

2. Verify display driver loading:
   ```bash
   # Check kernel logs
   dmesg | grep -i drm
   dmesg | grep -i panel
   ```

### Audio Issues

#### No Audio Output
1. Check UCM files:
   ```bash
   # Verify UCM file presence
   ls /usr/share/alsa/ucm2/
   
   # Check audio HAL
   lsmod | grep snd
   ```

2. Debug PulseAudio:
   ```bash
   # Check PulseAudio status
   pactl info
   pactl list sinks
   ```

## Development Environment Issues

### ADB Connection Problems

#### Device Not Detected
```bash
# Update udev rules
sudo curl -o /etc/udev/rules.d/51-android.rules \
    https://raw.githubusercontent.com/M0Rf30/android-udev-rules/master/51-android.rules
sudo chmod 644 /etc/udev/rules.d/51-android.rules
sudo udevadm control --reload-rules

# Add user to required groups
sudo usermod -aG plugdev,adb $USER

# Restart adb server
adb kill-server
adb start-server
```

### Build Environment Issues

#### CCache Problems
```bash
# Reset ccache
ccache -C
ccache -z

# Verify ccache configuration
ccache -s
```

## Recovery and Debugging

### Safe Recovery Steps

1. Boot to Recovery:
   ```bash
   # Force recovery boot
   adb reboot recovery
   ```

2. Backup Important Data:
   ```bash
   # Create backup
   adb pull /data/user_de
   adb pull /data/misc
   ```

3. Return to Stock:
   ```bash
   # Flash stock recovery
   fastboot flash recovery stock-recovery.img
   
   # Flash stock boot
   fastboot flash boot stock-boot.img
   ```

### Debug Information Collection

#### System Logs
```bash
# Kernel logs
adb shell dmesg > kernel.log

# System logs
adb shell logcat > system.log

# Hardware info
adb shell cat /proc/cpuinfo > cpu.log
adb shell cat /proc/meminfo > mem.log
```

## Contributing Debug Information

When reporting issues:
1. Provide full build logs
2. Include kernel config
3. Attach relevant system logs
4. Describe steps to reproduce
5. Include device specifications

## Next Steps

If you're still experiencing issues:
1. Check the [UBports Forum](https://forums.ubports.com)
2. Join the UBports porting telegram group
3. Review device-specific documentation
4. Consult the [Halium documentation](https://docs.halium.org)
