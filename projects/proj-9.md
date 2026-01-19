---
layout: post
title: 'Flydigi Vader 5 Linux Driver'
---

## Userspace Linux Driver for Flydigi Vader 5 Pro Gamepad

**flydigi-vader5** is a C++ userspace driver that enables full functionality of the Flydigi Vader 5 Pro gamepad on Linux via 2.4G USB dongle, featuring Xbox Elite emulation, gyro support, and QMK-style layer system.

![Steam Elite Config](https://raw.githubusercontent.com/BANANASJIM/flydigi-vader5/main/docs/images/steam_elite.png)

### Key Features

- **Xbox Elite Emulation**: Full Steam paddle support (M1-M4) for competitive gaming
- **Gyro Integration**: Mouse mode or right stick mapping for games lacking native gyro
- **QMK-Style Layer System**: Tap-hold mechanics allowing single buttons to perform different functions based on press duration
- **Input Remapping**: Reassign buttons to keyboard and mouse controls
- **Systemd Integration**: Service support with udev rules for seamless system integration

### Technical Stack

- **Language**: C++20
- **Build System**: CMake
- **Configuration**: TOML-based config files
- **System Integration**: Systemd service + udev rules

### Quick Start

```bash
# Build and run
sudo ./build/vader5d

# Custom configuration
sudo ./build/vader5d -c /path/to/config.toml

# Full system installation
./install/install.sh install
```

### Configuration

The driver supports customizable:
- Gyro behavior and sensitivity
- Stick sensitivity (mouse to gamepad modes)
- Layer triggers with hold/toggle activation
- Button remapping to keyboard/mouse

[View on GitHub](https://github.com/BANANASJIM/flydigi-vader5)
