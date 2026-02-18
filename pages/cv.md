---
layout: page
title: About
permalink: /cv
---

# Haolong (Jim) Zheng

**GPU / Graphics Engineer** · Vancouver, BC

[GitHub](https://github.com/BANANASJIM) · [LinkedIn](https://www.linkedin.com/in/bananasjim/) · bananasjim1@gmail.com

---

# Profile

Graphics programmer and GPU engineer with hands-on experience in CUDA kernel optimization, Vulkan rendering, and neural rendering systems. Built a standalone CUDA inference engine for RenderFormer (SIGGRAPH 2025) achieving 8.3× speedup over PyTorch, and a from-scratch LLM inference engine with Vulkan/CUDA dual backends. Previously shipped GPU-accelerated crowd simulation and procedural content tools in production game titles.

---

# Education

**Simon Fraser University** — MSc Computing Science (Visual Computing)
September 2024 – May 2027 (Expected) · Vancouver, BC
- Coursework: CMPT 756 Distributed Systems & Cloud, Visual Computing, Parallel Programming

**Beijing Forestry University** — BEng Vehicle Engineering
September 2019 – June 2023 · Beijing

---

# Experience

**Graduate Independent Research** · Simon Fraser University
September 2024 – Present
- Built [RenderFormer CUDA Engine](https://github.com/BANANASJIM/renderformer-cuda): standalone C++20/CUDA inference for SIGGRAPH 2025 neural renderer, 8.3× faster than PyTorch (78ms vs 1.5s at 512×512) using cuDNN fused attention, CUDA Graph replay, and full fp16 pipeline
- Built [TinyLama](https://github.com/BANANASJIM/TinyLama): minimal LLM inference engine (~3k LOC), Vulkan compute + CUDA backends with hand-written PTX kernels, GGUF quantization support
- Built [Violet Engine](https://github.com/BANANASJIM/Violet): modern Vulkan graphics engine with RenderGraph, bindless rendering, PBR/IBL, cascaded shadow maps, and runtime Slang shader reflection
- Built [Goggles](https://github.com/BANANASJIM/Goggles): zero-copy Vulkan frame capture via DMA-BUF with RetroArch shader pipeline
- Built [flydigi-vader5](https://github.com/BANANASJIM/flydigi-vader5): Linux userspace gamepad driver with Xbox Elite emulation and QMK-style layers (17⭐)

**Technical Artist** · Camel Games
November 2022 – September 2024 · Beijing
- Developed GPU-accelerated crowd simulation using VAT and Niagara in Unreal Engine
- Built PCG tools for batch asset generation, improving production scalability
- Standardized material workflows bridging art and engineering
- Prototyped a vampire-themed endless runner game

**Technical Artist** · Perfect World Games
May 2022 – November 2022 · Beijing
- Developed procedural assets with LODs using Houdini and Substance Designer
- Built custom Blender plugins to improve model production efficiency

---

# Skills

- **Languages**: C++20, C, CUDA, Python, HLSL, GLSL, Slang, C#
- **GPU Programming**: CUDA (cuBLAS, cuDNN, Tensor Cores/WMMA, CUDA Graphs, PTX), Vulkan Compute
- **Graphics**: Vulkan, OpenGL, Unreal Engine, Unity, Godot, RenderDoc, Nsight Compute
- **Tools**: Git, CMake, Docker, Linux
- **3D**: Houdini, Blender, Substance Designer
- **ML**: PyTorch, OpenCV, GGUF/safetensors formats

---

# Languages

- **English**: Fluent
- **Chinese**: Native
- **Japanese**: Intermediate

---

# Interests

- Indie game development · Electronic music production · Mechanical keyboards
