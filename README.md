# 🖼️ Pixels

**The high-performance, Rust-powered image processing engine for Node.js and the Browser.**

Pixels solves critical industry challenges by providing a **unified core** that runs identically across servers, browsers, and edge runtimes. It prioritizes **security**, **portability**, and **speed** without the headache of complex native dependencies.

---

## 🚩 Core Problems Solved

### 1. The Native Dependency Nightmare
Unlike traditional libraries (e.g., `sharp`) that require `libvips` or `python` during installation, Pixels ships with **pre-compiled Rust binaries**. It works instantly on Alpine Linux (musl), macOS (M1/Intel), and Windows.

### 2. "Image Bomb" Security
Most image engines are vulnerable to decompression bombs. Pixels implements a strict **Resource Limit System** that enforces memory and pixel caps *before* any processing begins, protecting your infrastructure from DDoS attacks.

### 3. Runtime Fragmentation
Stop using one library for Node.js and another (like `canvas` or `fabric`) for the browser. Pixels provides an **identical Fluent API** for both environments.

### 4. Zero-Latency Edge Processing
Pixels is SIMD-optimized Rust compiled to WebAssembly. It is designed to run in **Cloudflare Workers** and **Vercel Edge** with near-native performance.

---

## 🚀 Quick Start

```bash
npm install @pixels-image/core
```

```javascript
import { pixels } from '@pixels-image/core';

const output = await pixels(inputBuffer)
  .resize({ width: 800, fit: 'cover' })
  .grayscale()
  .toFormat('webp')
  .toBuffer();
```

---

## ✨ Features at a Glance

*   **⚡ SIMD-Accelerated:** Built on top of `fast_image_resize` and `image` crates.
*   **🛡️ Resource Guards:** Custom limits on memory, input size, and pixel count.
*   **📦 All-in-One:** Built-in BlurHash, ThumbHash, and Average Color extraction.
*   **💧 Watermarking:** Native support for both text and image overlays with gravity.
*   **📊 Monitoring:** Built-in Prometheus metrics for production observability.
