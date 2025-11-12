---
layout: post
title: 'gamepaper'
---

## Vulkan Game as Wayland Live Wallpaper

**gamepaper** renders Vulkan games as dynamic wallpapers on Wayland compositors using the BACKGROUND layer, bringing your desktop to life with real-time 3D content.

### Features

- 🎮 **Background Layer Rendering**: Games run on the BACKGROUND layer (beneath desktop icons)
- 🖼️ **Auto-Detection**: Automatic resolution and screen rotation detection (portrait support)
- 📐 **Aspect Ratio Protection**: Letterbox/pillarbox to prevent stretching
- ⚡ **Performance Scaling**: Lower internal rendering resolution to save resources
- 🎯 **Zero-Copy Transfer**: DMA-BUF for efficient Vulkan Layer → EGL/OpenGL streaming
- 🪟 **Non-Intrusive**: Desktop operations unaffected while game continues rendering
- 🎲 **Steam Compatible**: Conditional layer loading, doesn't interfere with gamescope

### How It Works

1. **gamepaper** creates a fullscreen surface on Wayland BACKGROUND layer
2. **Nested compositor** (wlroots headless backend) provides virtual Wayland display
3. **Game** connects to nested compositor, rendering in headless environment
4. **Vulkan Layer** intercepts `vkQueuePresentKHR` calls
5. Swapchain images copied to linear tiling capture images
6. Exported as DMA-BUF via `VK_KHR_external_memory_fd`
7. Sent to gamepaper through Unix socket with `SCM_RIGHTS`
8. **gamepaper** imports DMA-BUF as EGLImage and renders to background layer
9. Automatic viewport calculation maintains aspect ratio

### Architecture Diagram

```
┌────────────────────────────────────────┐
│ Wayland Compositor (Sway/Hyprland)    │
│  ┌──────────────────────────────────┐ │
│  │ BACKGROUND Layer                 │ │
│  │  ┌────────────────────────────┐  │ │
│  │  │ gamepaper (EGL/OpenGL)     │  │ │ ← Background rendering
│  │  └────────────────────────────┘  │ │
│  └──────────────────────────────────┘ │
│              ▲                         │
│              │ DMA-BUF fd (IPC)        │
│  ┌──────────────────────────────────┐ │
│  │ Vulkan Layer                     │ │ ← API interception
│  └──────────────────────────────────┘ │
│              ▲                         │
│  ┌──────────────────────────────────┐ │
│  │ Game (Vulkan)                    │ │ ← Headless rendering
│  └──────────────────────────────────┘ │
│              ▲                         │
│  ┌──────────────────────────────────┐ │
│  │ Nested Compositor (wlroots)      │ │ ← Virtual display
│  └──────────────────────────────────┘ │
└────────────────────────────────────────┘
```

### Usage Example

```bash
# Auto-detect resolution and rotation
gamepaper -o HDMI-A-1 -- vkcube

# Specify resolution and scale factor
gamepaper -w 1920 -h 1080 -s 0.5 -o HDMI-A-1 -- vkcube

# Limit framerate for power saving
gamepaper --fps 30 -o HDMI-A-1 -- steam steam://rungameid/123
```

### Technical Stack

- **Layer Shell**: `zwlr_layer_shell_v1` (BACKGROUND layer)
- **Vulkan Interception**: Custom implicit layer + dispatch table chain
- **DMA-BUF Export**: `VK_KHR_external_memory_fd` + `VK_IMAGE_TILING_LINEAR`
- **EGL Import**: `EGL_EXT_image_dma_buf_import` + `GL_OES_EGL_image_external`
- **IPC**: Unix domain socket + `SCM_RIGHTS` (file descriptor passing)
- **Nested Compositor**: wlroots headless backend + XWayland support

[View on GitHub](https://github.com/BANANASJIM/gamepaper)
