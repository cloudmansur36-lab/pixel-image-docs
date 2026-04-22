# 🛠️ API Reference: Fluent Builder

The `Pixels` class (returned by the `pixels()` function) provides a chainable, fluent API for image manipulation. 

## Initialization
Pass a `Buffer` (Node.js) or `Uint8Array` (WASM) to get started.

```typescript
import { pixels } from '@pixels-image/core';
const p = pixels(buffer);
```

---

## 📐 Transformations

### `.resize(options)`
Resizes the image using high-performance SIMD filters.
- **`width`**: *number*
- **`height`**: *number*
- **`fit`**: `Contain`, `Cover`, `Fill`, `Inside`, `Outside` (Default: `Cover`)
- **`filter`**: `nearest`, `bilinear`, `bicubic` (Default), `lanczos`

### `.rotate(degrees)`
Rotates the image. Orthogonal angles (90, 180, 270) are extremely fast.
- **`degrees`**: *number* (0-360)

### `.smartCrop(width, height)`
Uses a saliency proxy (feature detection) to find the most "interesting" part of the image and crops to those dimensions.
- **`width`**: *number*
- **`height`**: *number*

---

## ✨ Effects & Processing

### `.blur(sigma)`
Applies a Gaussian blur.
- **`sigma`**: *number* (Recommended: 0.5 to 5.0)

### `.sharpen(sigma, threshold)`
Applies an adaptive unsharp mask.
- **`sigma`**: *number*
- **`threshold`**: *number* (Sensitivity)

### `.grayscale()`
Converts the image to 8-bit grayscale.

### `.tint({ r, g, b })`
Multiplies the RGB channels by the specified color.
- **`r, g, b`**: *number* (0-255)

---

## 💾 Output (Terminal Methods)

Methods that end the chain and return a result.

### `.toBuffer()`
Processes the chain and returns a `Promise<Buffer>`.

### `.toFormat(format, quality?)`
Sets the output compression.
- **`format`**: `webp`, `jpeg`, `png`, `gif`, `avif`
- **`quality`**: *number* (0-100)

### `.metadata()`
Returns an object containing:
- `width`, `height`
- `format`
- `exif` (Parsed EXIF tags)
- `colorType` (e.g., `Rgba8`)
