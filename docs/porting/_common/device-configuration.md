---
title: "Device Configuration Guide"
weight: 3
description: "Detailed guide for configuring deviceinfo file"
---

# Device Configuration Guide

The `deviceinfo` file is the central configuration point for your device port. This guide explains all available options and their usage.

## Basic Configuration

### Device Information
```bash
# Commercial name of device
deviceinfo_name="Smartphone 12Pro 5G"

# Manufacturer name
deviceinfo_manufacturer="UBports"

# Device codename (usually ro.product.name from Android)
deviceinfo_codename="yumi"

# Architecture (arm, aarch64, x86, x86_64)
deviceinfo_arch="aarch64"

# Halium version (9, 10, 11, 12)
deviceinfo_halium_version="12"

# Ubuntu Touch release
deviceinfo_ubuntu_touch_release="focal"
```

## Kernel Configuration

### Source Configuration
```bash
# Kernel source repository
deviceinfo_kernel_source="https://github.com/your/kernel.git"

# Branch to use
deviceinfo_kernel_source_branch="android-12"

# Kernel configuration file
deviceinfo_kernel_defconfig="vendor_defconfig"
```

### Build Configuration
```bash
# Kernel command line
deviceinfo_kernel_cmdline="console=ttyMSM0,115200n8 ..."

# Clang compilation settings
deviceinfo_kernel_clang_compile="true"
deviceinfo_kernel_clang_branch="android12L-gsi"
deviceinfo_kernel_clang_revision="r416183b"
```

### Custom Toolchain
```bash
# Optional custom GCC toolchain
deviceinfo_kernel_gcc_toolchain_source="https://example.com/gcc-toolchain.tar.xz"
deviceinfo_kernel_gcc_toolchain_dir="gcc-linaro-6.3.1"
```

## Boot Configuration

### Basic Boot Settings
```bash
# Boot image header version
deviceinfo_bootimg_header_version="2"

# Flash settings
deviceinfo_flash_pagesize="4096"
deviceinfo_flash_offset_base="0x00000000"
deviceinfo_flash_offset_kernel="0x00008000"
deviceinfo_flash_offset_ramdisk="0x01000000"
deviceinfo_flash_offset_tags="0x00000100"
```

### Operating System Settings
```bash
# Android OS version
deviceinfo_bootimg_os_version="11"

# Security patch level
deviceinfo_bootimg_os_patch_level="2022-12-05"

# Partition sizes
deviceinfo_bootimg_partition_size="100663296"
deviceinfo_system_partition_size="3000M"
```

## System Configuration

### Partitioning
```bash
# Enable overlay store (recommended for 20.04+)
deviceinfo_use_overlaystore="true"

# Root filesystem settings
deviceinfo_rootfs_image_sector_size="4096"
```

### Recovery Options
```bash
# Enable recovery partition
deviceinfo_has_recovery_partition="true"

# Recovery partition size
deviceinfo_recovery_partition_size="100663296"
```

## Advanced Configuration

### DTB/DTBO Settings
```bash
# DTB settings
deviceinfo_kernel_use_dtc_ext="false"
deviceinfo_kernel_apply_overlay="false"
deviceinfo_bootimg_prebuilt_dtb="device.dtb"

# DTBO settings
deviceinfo_dtbo="false"
deviceinfo_skip_dtbo_partition="true"
```

### Ramdisk Configuration
```bash
# Ramdisk settings
deviceinfo_ramdisk_compression="gzip"
deviceinfo_prebuilt_boot_ramdisk="halium-boot-ramdisk.img"
```

## Usage Examples

### Basic Android 11 Device
```bash
deviceinfo_name="Example Phone"
deviceinfo_manufacturer="Example"
deviceinfo_codename="example"
deviceinfo_arch="aarch64"
deviceinfo_halium_version="11"
deviceinfo_ubuntu_touch_release="focal"

deviceinfo_kernel_source="https://github.com/example/kernel.git"
deviceinfo_kernel_source_branch="android-11"
deviceinfo_kernel_defconfig="vendor_defconfig"

deviceinfo_bootimg_header_version="2"
deviceinfo_use_overlaystore="true"
```

### Legacy Device (Android 9)
```bash
deviceinfo_name="Legacy Phone"
deviceinfo_manufacturer="Legacy"
deviceinfo_codename="legacy"
deviceinfo_arch="arm"
deviceinfo_halium_version="9"
deviceinfo_ubuntu_touch_release="focal"

deviceinfo_kernel_source="https://github.com/legacy/kernel.git"
deviceinfo_kernel_source_branch="android-9"
deviceinfo_kernel_defconfig="legacy_defconfig"

deviceinfo_bootimg_header_version="0"
deviceinfo_use_overlaystore="false"
```

## Common Issues

### Build Failures
1. Check kernel source and branch names
2. Verify toolchain configuration
3. Ensure correct architecture settings

### Boot Failures
1. Verify kernel command line
2. Check partition offsets
3. Confirm DTB/DTBO settings

## Next Steps

1. Configure your device's specific settings
2. Test kernel compilation
3. Build a test system
4. Check [Overlay System Guide](../overlay-system/) for system customization

Need help? Check the [UBports Forum](https://forums.ubports.com) or join the porting telegram group.
