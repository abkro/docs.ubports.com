---
title: "Development Environment Setup"
weight: 2
description: "Setting up your development environment for Ubuntu Touch porting"
---

# Development Environment Setup

This guide explains how to set up your environment for porting Ubuntu Touch to Android devices.

## Quick Start

```bash
# Install essential packages
sudo apt update && sudo apt install -y \
    android-tools-adb \
    android-tools-fastboot \
    bc \
    build-essential \
    python-is-python3 \
    python3-pip \
    git \
    ccache \
    curl \
    libssl-dev \
    unzip \
    wget \
    zip

# Create project directory
mkdir -p ~/ubports-ports/[device_codename]
cd ~/ubports-ports/[device_codename]

# Clone build tools
git clone https://gitlab.com/ubports/porting/community-ports/halium-generic-adaptation-build-tools.git build-tools

# Create minimal structure
mkdir -p overlay
touch deviceinfo
```

## Directory Structure

### Minimal Required Structure
```
[device_codename]/
├── deviceinfo          # Main device configuration file
├── build-tools/        # Cloned build tools repository
└── overlay/            # Device-specific system overlays
```

### Optional Directories
```
[device_codename]/
├── ramdisk-recovery-overlay/  # Recovery customizations
├── vendor-ramdisk-overlay/    # Vendor ramdisk customizations
└── out/                       # Build outputs (created automatically)
```

## Configuration

### 1. Device Configuration
Copy the sample deviceinfo file and configure it for your device:
```bash
cp build-tools/deviceinfo.sample deviceinfo
```

Key configuration sections in deviceinfo:
- Basic device information
- Kernel configuration
- Boot image configuration
- System partition configuration

See [Device Configuration Guide](../device-configuration/) for detailed options.

### 2. System Overlays
The `overlay/` directory allows you to add or modify system files:
```
overlay/
├── system/
│   ├── etc/
│   └── usr/
└── vendor/
    └── etc/
```

See [Overlay System Guide](../overlay-system/) for detailed usage.

## Building

### Basic Build Commands

1. Standard build:
```bash
./build-tools/build.sh
```

2. Build with local cache (faster rebuilds):
```bash
./build-tools/build.sh -b ~/ubports-cache
```

3. Build kernel only:
```bash
./build-tools/build.sh -k
```

4. Open kernel config menu:
```bash
./build-tools/build.sh -m
```

### Build Process

The build system will:
1. Set up required toolchains based on deviceinfo configuration
2. Clone kernel source and required repositories
3. Apply device-specific overlays
4. Generate necessary boot images
5. Create system image tarball

### Build Outputs

Build artifacts are placed in the `out/` directory:
```
out/
├── boot.img              # Boot image
├── recovery.img          # Recovery image (if enabled)
├── dtbo.img             # DTBO image (if required)
└── device_[codename].tar.xz  # Device tarball
```

## Troubleshooting

### Common Issues

1. Build environment problems:
```bash
# Clear ccache
ccache -C

# Remove temporary files
rm -rf out/
```

2. Permission issues:
```bash
# Add user to required groups
sudo usermod -aG plugdev,adb $USER
newgrp plugdev
newgrp adb
```

### Log Files
Build logs are located in:
- Kernel build: `out/kernel_build.log`
- System build: `out/system_build.log`

## Advanced Usage

### Custom Ramdisk Configuration
Place customizations in:
- `ramdisk-recovery-overlay/` for recovery modifications
- `vendor-ramdisk-overlay/` for vendor ramdisk changes

### Local Development
For faster development cycles:
```bash
# Use local build cache
export BUILD_DIR=~/ubports-cache

# Build with cache
./build-tools/build.sh -b $BUILD_DIR
```

## Next Steps

1. Configure your device:
   - Set up the deviceinfo file
   - Add required overlays

2. Build the system:
   - Test kernel configuration
   - Build full system
   - Test on device

3. Review specific guides:
   - [Device Configuration](../device-configuration/)
   - [Overlay System](../overlay-system/)
   - [Testing and Debugging](../testing/)
