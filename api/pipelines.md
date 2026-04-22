# 🧩 Declarative Pipelines

For production systems like image proxies or CMS engines, you often want to describe the processing steps as a data structure rather than code. The `runPipeline` function allows you to pass an array of operations.

## Basic Example
```typescript
import { runPipeline } from '@pixels-image/core';

const output = await runPipeline(input, [
  { kind: 'resize', width: 800, fit: 'cover' },
  { kind: 'grayscale' },
  { kind: 'watermark', text: { text: "Draft", size: 48 } }
], { format: 'webp', quality: 75 });
```

## Operation Kinds
Every object in the array must have a `kind` field matching one of the following:
- `resize`
- `rotate` (value is a number)
- `flipH`: No value.
- `flipV`: No value.
- `crop`: Requires `left`, `top`, `width`, `height`.
- `blur` (value is a number)
- `sharpen` (requies `sigma` and `threshold`)
- `grayscale`: No value.
- `tint` (requires `r`, `g`, `b`)
- `smartCrop` (requires `width`, `height`)
- `watermark` (requires `image` or `text` spec)

---

# 💧 Watermarking

Pixels supports both image-on-image and text-on-image watermarking.

## Image Watermarks
Overlay a secondary image (e.g., your logo) onto the base image.

```javascript
await pixels(main)
  .watermarkImage({
    overlay: logoBuffer,
    gravity: 'SouthEast',
    opacity: 0.5,
    padding: 16
  })
  .toBuffer();
```

## Text Watermarks
Render text directly onto the image. Pixels ships with a default font so this works in minimal Docker environments.

```javascript
await pixels(main)
  .watermarkText({
    text: "Property of Acme Corp",
    gravity: "Center",
    size: 42,
    color: { r: 255, g: 255, b: 255 },
    opacity: 0.8
  })
  .toBuffer();
```

### Gravity Placement
You can anchor overlays to 9 different points:
- `NorthWest`, `North`, `NorthEast`
- `West`, `Center`, `East`
- `SouthWest`, `South`, `SouthEast`
