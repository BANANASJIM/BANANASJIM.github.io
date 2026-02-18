---
layout: post
title: 'RenderFormer CUDA Engine'
---

## Standalone CUDA Inference for Neural Rendering (SIGGRAPH 2025)

**RenderFormer CUDA Engine** is a high-performance C++20/CUDA inference engine for [RenderFormer](https://github.com/microsoft/renderformer) (SIGGRAPH 2025), a transformer-based neural renderer that converts triangle meshes with global illumination into photorealistic images.

![RenderFormer Gallery](https://raw.githubusercontent.com/BANANASJIM/renderformer-cuda/main/medias/gallery.jpg)

### Performance

Benchmarked on RTX 3080 Ti, model `renderformer-v1.1-swin-large` (483M params):

| | PyTorch fp16 | CUDA Engine (512×512) | CUDA Engine (320×320) |
|---|---|---|---|
| Per-view render | ~1.5s | **78ms (8.3× faster)** | **34ms (30 FPS)** |

### Key Features

- **Zero Python dependencies** — loads safetensors + HDF5 directly in C++
- **cuDNN fused SDPA** — tensor-core flash attention for cross-attention
- **CUDA Graph capture** — 732-node graph replay with zero CPU launch overhead
- **Full fp16 inference** — mixed-precision encoder + full fp16 decoder
- **KV cache** — cross-attention K,V computed once, reused across all views
- **Interactive viewer** — GLFW/OpenGL with trackball camera, async double-buffered rendering

### Links

- [GitHub Repository](https://github.com/BANANASJIM/renderformer-cuda)
- [RenderFormer Paper (arXiv)](https://arxiv.org/abs/2505.21925)
- [Official Project Page](https://microsoft.github.io/renderformer/)
