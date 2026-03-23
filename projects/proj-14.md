---
layout: post
title: 'padctl'
---

Universal Linux gamepad compatibility layer.

padctl is a userspace HID gamepad daemon that maps vendor-specific USB/HID reports to standard Linux input events via uinput. Device support is fully declarative via TOML configs — no kernel patching and no custom drivers.

### Features
- Declarative device configs (`.toml`) for fast device onboarding
- Layer system (hold/toggle/tap-hold) with independent remaps and stick/gyro modes
- Gyro mouse + stick mouse/scroll with sensitivity/deadzone/smoothing controls
- Multi-device hotplug support and hot-reload via `SIGHUP`
- Force-feedback (FF_RUMBLE) passthrough to physical devices

### Tech Stack
Zig · libusb · uinput · TOML · systemd

[GitHub →](https://github.com/BANANASJIM/padctl)
[Docs →](https://bananasjim.github.io/padctl/)
