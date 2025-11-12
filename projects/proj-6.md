---
layout: post
title: 'Violet Engine'
---

## A Modern Vulkan-Based 3D Graphics Engine

**Violet Engine** is a high-performance 3D graphics engine written in C++20, leveraging modern Vulkan features including RenderGraph architecture, bindless rendering, and runtime shader reflection with Slang.

![Violet Engine Screenshot](https://raw.githubusercontent.com/BANANASJIM/Violet/main/assets/screenshot.jpg)

### Key Features

- **RenderGraph System**: Declarative multi-pass rendering with automatic barrier generation and transient resource management
- **Bindless Rendering**: Texture arrays with UPDATE_AFTER_BIND, eliminating per-draw descriptor updates
- **Slang Reflection**: Runtime shader reflection for automatic descriptor layout and resource management
- **PBR Pipeline**: Physically-based rendering with IBL, cascaded shadow maps, and physical light units
- **Post-Processing**: Histogram-based auto-exposure and ACES Filmic tone mapping
- **glTF 2.0 Support**: Full scene loading with async resource streaming

### Technical Highlights

**RenderGraph Architecture**
- Declarative per-frame rebuild pattern
- Automatic synchronization and barrier insertion
- Transient resource management for optimal memory usage

**Bindless Rendering Pipeline**
- Set 0: Per-frame resources with dynamic offsets for triple buffering
- Set 1: Bindless texture arrays (1024 textures, 64 cubemaps)
- Set 2: Materials SSBO with all material data
- Push Constants: Model matrix and material ID

**Technology Stack**: Vulkan, EASTL, EnTT, Slang, VulkanMemoryAllocator, ImGui

### Rendering Pipeline

1. **Shadow Pass**: Cascaded shadow maps (4 cascades, 8192x8192 atlas)
2. **Main Pass**: PBR rendering to HDR buffer (rgba16f)
3. **Auto-Exposure**: Histogram-based luminance analysis
4. **Tone Mapping**: ACES Filmic with adaptive exposure
5. **Present**: Final output to swapchain

[View on GitHub](https://github.com/BANANASJIM/Violet)
