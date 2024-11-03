---
title: "Overlay System Guide"
weight: 4
description: "Guide for using the Ubuntu Touch overlay system for device customization"
---

# Overlay System Guide

The Ubuntu Touch overlay system allows you to customize your device port by adding or modifying system files without altering the base system image. This guide explains how to use the various overlay types effectively.

## Overview

### Available Overlay Types
1. **System Overlay** (`overlay/`)
   - Main system and vendor customizations
   - Applied during build process

2. **Recovery Overlay** (`ramdisk-recovery-overlay/`)
   - Recovery-specific modifications
   - Applied to recovery ramdisk

3. **Vendor Ramdisk Overlay** (`vendor-ramdisk-overlay/`)
   - Vendor-specific boot modifications
   - Applied to vendor ramdisk (Android 12+)

## System Overlay (`overlay/`)

### Directory Structure
```
overlay/
├── system/
│   ├── etc/
│   │   ├── init/
│   │   ├── udev/
│   │   └── modules-load.d/
│   └── usr/
│       ├── lib/
│       └── share/
└── vendor/
    ├── etc/
    │   └── init/
    └── firmware/
```

### Common Use Cases

1. **Device Init Scripts**
```bash
# overlay/system/etc/init/device-specific.conf
description "Device-specific initialization"
start on filesystem

script
    # Device initialization commands
    echo 1 > /sys/devices/example/enable
end script
```

2. **Hardware Configuration**
```bash
# overlay/system/etc/udev/rules.d/99-device.rules
# Enable device-specific hardware
SUBSYSTEM=="input", KERNEL=="event*", ATTRS{name}=="example", TAG+="example_tag"
```

3. **Kernel Module Loading**
```bash
# overlay/system/etc/modules-load.d/device-modules.conf
# Load required kernel modules
example_module
another_module
```

4. **Vendor Firmware**
```
overlay/vendor/firmware/
├── example.fw
└── another.fw
```

### File Permissions
- System files: `644` (rw-r--r--)
- Executable files: `755` (rwxr-xr-x)
- Configuration files: `644` (rw-r--r--)
- Init scripts: `644` (rw-r--r--)

## Recovery Overlay (`ramdisk-recovery-overlay/`)

### Purpose
- Customize recovery environment
- Add recovery-specific tools
- Modify recovery behavior

### Structure
```
ramdisk-recovery-overlay/
├── etc/
│   └── recovery.conf
├── sbin/
│   └── recovery-tools/
└── vendor/
    └── firmware/
```

### Example: Recovery Configuration
```bash
# ramdisk-recovery-overlay/etc/recovery.conf
# Recovery-specific settings
allow_data_wipe=true
support_encryption=true
```

## Vendor Ramdisk Overlay (`vendor-ramdisk-overlay/`)

### Usage
- Required for Android 12+ devices
- Provides vendor-specific boot modifications
- Handles vendor kernel modules

### Structure
```
vendor-ramdisk-overlay/
├── first_stage_ramdisk/
├── lib/
│   └── modules/
└── vendor/
    └── etc/
```

### Example: Kernel Modules Configuration
```bash
# vendor-ramdisk-overlay/lib/modules/modules.load
# Load vendor kernel modules in specific order
vendor_module1.ko
vendor_module2.ko
platform_module.ko
```

## Best Practices

### 1. File Organization
```bash
# Group related files logically
overlay/
├── system/
│   ├── etc/
│   │   ├── device-settings/      # Device-specific settings
│   │   └── hardware-configs/     # Hardware configurations
│   └── usr/
│       └── share/
│           └── device-scripts/   # Helper scripts
└── vendor/
    └── firmware/                 # Device firmware
```

### 2. Documentation
```bash
# Add README files to document changes
overlay/system/etc/README.md
```
Example content:
```markdown
# Device Configuration Files
- `example.conf`: Controls device power management
- `hardware.conf`: Hardware-specific settings
```

### 3. Version Control
```bash
# Create .gitignore for overlay directories
overlay/**/*.fw        # Ignore firmware files
overlay/**/*.bin      # Ignore binary files
```

## Common Configurations

### 1. Audio Configuration
```bash
# overlay/system/usr/share/alsa/ucm2/[device]/[device].conf
SectionDevice."Speaker" {
    Comment "Internal Speaker"
    Value {
        PlaybackPriority 100
        PlaybackPCM "hw:0,0"
    }
}
```

### 2. Display Configuration
```bash
# overlay/system/etc/udev/rules.d/99-display.rules
# Configure display properties
SUBSYSTEM=="drm", KERNEL=="card0", ATTR{status}=="connected", RUN+="/usr/bin/display-setup"
```

### 3. Power Management
```bash
# overlay/system/etc/init/power-management.conf
description "Device power management"
start on started unity8

script
    # Set CPU governor
    echo "performance" > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
end script
```

## Debugging Overlays

### 1. Check Applied Overlays
```bash
# List all overlayed files
find /system -type f -exec sh -c 'test -e /overlay/system/{} && echo {}' \;
```

### 2. Verify File Permissions
```bash
# Check file permissions
find overlay/ -type f -exec stat -c "%a %n" {} \;
```

### 3. Test Overlay Changes
```bash
# Before building, verify overlay structure
tree overlay/
tree ramdisk-recovery-overlay/
tree vendor-ramdisk-overlay/
```

## Troubleshooting

### Common Issues

1. **Files Not Applied**
   - Check file paths match target system
   - Verify file permissions
   - Ensure no syntax errors in configuration files

2. **Boot Issues**
   - Check init script syntax
   - Verify module load order
   - Validate firmware file placement

3. **Permission Problems**
   - Set correct file permissions
   - Check SELinux contexts if applicable
   - Verify owner/group settings

## Next Steps

1. Review your device's needs
2. Create appropriate overlay structure
3. Test configurations
4. Check [Testing Guide](../testing/) for validation steps

Need help? Check:
- [UBports Forum](https://forums.ubports.com)
- [Halium Documentation](https://docs.halium.org)
- UBports Porting Telegram group
