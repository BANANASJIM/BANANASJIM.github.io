---
layout: post
title: 'TinyLama'
---

## Minimal LLM Inference Engine from Scratch

**TinyLama** is a ~3,000-line C++20 LLM inference engine with zero external dependencies. Built for learning and understanding how large language models work at the lowest level.

### Features

- **GGUF parser** with mmap and quantization support (Q4_0, Q6_K, F16, F32)
- **BPE tokenizer** extracted directly from model vocabulary
- **Vulkan compute backend** — 16 Slang shaders for GPU inference
- **CUDA compute backend** — 16 hand-written PTX kernels
- **Dual-GPU inference** — split model across Vulkan and CUDA via `--split` flag
- **Full LLaMA architecture** — attention, RoPE, GQA, SwiGLU FFN

### Why Build This?

Instead of using existing frameworks, TinyLama implements every component from scratch — tokenization, model loading, GPU compute kernels, and memory management. This provides deep understanding of:

- How quantized weights are stored and dequantized on GPU
- How attention mechanisms map to GPU compute shaders
- How to schedule work across multiple heterogeneous GPUs
- The full pipeline from text input to token generation

### Links

- [GitHub Repository](https://github.com/BANANASJIM/TinyLama)
