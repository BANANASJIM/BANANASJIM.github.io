---
layout: post
title: 'Goggles'
---

## Vulkan Frame Streaming with Shader Processing

**Goggles** captures Vulkan application frames and streams them across processes using Linux DMA-BUF with DRM format modifiers, with support for RetroArch shader processing (CRT effects, scanlines, etc.).

![CRT Shader Showcase](https://raw.githubusercontent.com/BANANASJIM/Goggles/main/showcase-zfast-crt.webp)

### Architecture

```
Target Application (Vulkan)
  └─ Capture Layer (vk_capture)
       └─ Export DMA-BUF via Unix Socket (SCM_RIGHTS)
            └─ Goggles Viewer
                 └─ RetroArch Shader Pipeline (Slang)
                      └─ Display / Stream
```

### Key Features

- **Zero-copy frame capture** — DMA-BUF sharing avoids GPU-to-CPU readback
- **Vulkan layer injection** — hooks `vkQueuePresentKHR` transparently
- **RetroArch shader support** — apply CRT-Royale, zfast-crt, and other classic shaders
- **Cross-process streaming** — Unix domain sockets with SCM_RIGHTS for fd passing

### Links

- [GitHub Repository](https://github.com/BANANASJIM/Goggles)
