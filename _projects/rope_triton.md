---
layout: page
title: RoPE with Triton
description: An optimized implementation of Rotary Position Embedding (RoPE) using Triton to significantly accelerate a key component of modern language models.
img: /assets/img/projects/rope_thumbnail.png
importance: 2
category: side projects
---

### Overview

As a developer with a keen interest in Transformer architectures, I was intrigued when I came across the Rotary Position Embedding (RoPE) technique. It proposed a clever new way to encode positional information, which is a fundamental challenge for modern transformer models with longer sequence lengths.

Driven by curiosity, I decided to implement it myself using Triton, a language and a compiler for parallel programming and writing custom GPU kernels directly in Python. This project started as a personal exercise to explore a new technique and to learn Triton, with the goal of comparing the performance between PyTorch version vs Cuda fused version vs Triton fused version.

For a deeper understanding of the underlying algorithm, you can refer to the original [RoPE paper](https://arxiv.org/abs/2104.09864).

---

### Results

<div class="row justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/RoPE performance_speed.png" title="RoPE Performance Benchmark" class="img-fluid rounded z-depth-1" caption="Performance comparison in milliseconds (ms) with transformer dimension size. The Triton version (blue) consistently outperforms the standard Torch implementation (orange) and closely approaches the performance of a manual CUDA kernel (green)." %}
    </div>
</div>

Raw data is follows: 
```markdown
| dimension_size | Triton      | Torch       | Cuda      |
|:---------------|:------------|:------------|:----------|
| 16.0           | 0.203776    | 0.089088    | 0.076800  |
| 32.0           | 0.250048    | 0.181040    | 0.111616  |
| 64.0           | 0.342016    | 0.400160    | 0.195584  |
| 128.0          | 0.539648    | 1.125376    | 0.368640  |
| 256.0          | 1.022976    | 2.401216    | 0.788480  |
| 512.0          | 2.040832    | 4.825088    | 1.742848  |
| 1024.0         | 3.968000    | 9.717248    | 3.917792  |
| 2048.0         | 8.015360    | 19.300575   | 7.745536  |
| 4096.0         | 16.146927   | 39.028225   | 15.905792 |
| 8192.0         | 32.131073   | 75.272324   | 31.818785 |
```

**1. Triton Kernels**
The core forward and backward passes of the RoPE operation are implemented as custom fused kernels in `triton_utils/kernel.py`. By using Triton, we can precisely control GPU operations, reducing memory I/O and maximizing hardware utilization to overcome the bottlenecks present in standard eager-mode implementations.

**2. PyTorch Integration**
To ensure ease of use, the custom Triton kernels are wrapped into a standard PyTorch module using `torch.autograd.Function` in `triton_utils/layer.py`. This makes it a plug-and-play component that can be used as a high-performance replacement for any existing RoPE layer within a larger PyTorch model, with no other code changes required.

**3. Benchmarking**
The project includes a detailed benchmark script (`benchmark.py`) that compares the performance of three versions:
* **Triton (Ours):** The custom Triton implementation.
* **Torch:** The baseline implementation in standard PyTorch.
* **CUDA:** A highly optimized, hand-written fused CUDA kernel for reference.

The results show that the Triton implementation is substantially faster than the native Torch version across all tested dimensions and scales more effectively. It successfully closes much of the performance gap with the manual CUDA kernel, offering near-native speed with the superior flexibility and development speed of Python.

---

### How to Use

The environment is containerized with Docker for simple and reliable reproduction.

**1. Build the Docker Image**
```bash
docker build --tag rope-triton:latest .
```

The full source code is available on [GitHub](https://github.com/taeyoungson/triton_rope). 