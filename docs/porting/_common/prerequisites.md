---
title: "Prerequisites"
weight: 1
description: "Required knowledge, skills, and resources for porting Ubuntu Touch"
---

# Prerequisites for Porting Ubuntu Touch

## Required Knowledge
- **Basic Requirements**
  - Linux command line usage
  - Basic Git operations
  - Understanding of Android system architecture
  - Basic knowledge of build systems (Make, CMake)

- **Recommended Skills**
  - Kernel compilation experience
  - Android device tree understanding
  - Basic C/C++ knowledge for debugging
  - Experience with system logs and debugging tools

## Hardware Requirements

### Development System
- Linux-based operating system (Ubuntu 20.04 LTS or newer recommended)
- Minimum 16GB RAM (32GB recommended for full system builds)
- At least 100GB free disk space
- Reliable internet connection

### Target Device Requirements
- Unlocked bootloader
- Working recovery environment (TWRP recommended)
- USB debugging enabled
- Backup of original device firmware
- Root access (recommended but not always required)

## Software Requirements

### Essential Tools
```bash
# Core development packages
sudo apt install git curl wget rsync zip unzip adb fastboot

# Build dependencies
sudo apt install build-essential ccache libncurses5 libncurses5-dev \
                 libssl-dev python-is-python3 python3-pip
```

### Additional Software
- Android platform tools (recent version)
- Device-specific vendor binaries
- Original device kernel source code

## Access Requirements
- GitHub account for contributing changes
- UBports forum account (recommended for community support)
- [Halium](https://github.com/halium) reference documentation access

## Before Starting
1. **Device Research**
   - Confirm device compatibility
   - Locate kernel source code
   - Identify required binary blobs
   - Document device specifications

2. **Environment Verification**
   ```bash
   # Verify core tools
   git --version
   adb version
   fastboot --version
   ```

3. **Documentation Access**
   - Bookmark relevant documentation pages
   - Join community support channels
   - Prepare device-specific documentation

## Recommended Setup

### Development Directory Structure
```bash
~/ubports-ports/
├── kernels/          # Kernel sources
├── devices/          # Device-specific files
├── builds/           # Build outputs
└── logs/            # Build and debug logs
```

### Version Control Setup
```bash
# Configure Git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Next Steps
After confirming all prerequisites:
1. Proceed to [Environment Setup](../environment/)
2. Choose between [Modern Porting Guide](../../modern-porting/) or [Legacy Porting Guide](../../legacy-porting/)
3. Review the [Quick Start Guide](../../modern-porting/quick-start/) for your chosen method
