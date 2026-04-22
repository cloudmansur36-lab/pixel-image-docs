# 🍱 Cookbook & Recipes

Common patterns and "easy to use" snippets for solving real-world image processing problems with Pixels.

## Recipe: SaaS Profile Pic Optimizer
Optimally crop an image to a square, strip private EXIF data, and convert to WebP for fast loading.

```javascript
import { pixels } from '@pixels-image/core';

async function optimizeProfile(buffer) {
  return await pixels(buffer)
    .smartCrop(400, 400)      // Intelligent square crop
    .stripMetadata()          // Remove GPS and Camera info
    .toFormat('webp', 75)     // Modern, compressed format
    .toBuffer();
}
```

---

## Recipe: Dynamic E-Commerce Watermarking
Overlay a semi-transparent store logo in the bottom-right corner of a product image.

```javascript
async function protectImage(productBuf, logoBuf) {
  return await pixels(productBuf)
    .watermarkImage({
      overlay: logoBuf,
      gravity: 'SouthEast',
      opacity: 0.6,
      padding: 20
    })
    .toBuffer();
}
```

---

## Recipe: Core Web Vitals (Placeholder + Optimized AVIF)
Generate a BlurHash for the initial page load and an AVIF for the high-quality final render.

```javascript
async function prepareForWeb(buffer) {
  const p = pixels(buffer);
  
  // 1. Get the tiny BlurHash for your DB
  const { blurhash } = await p.placeholders({ blurhash: true });

  // 2. Generate the high-quality optimized file
  const image = await p
    .resize({ width: 1200 })
    .toFormat('avif', 60)
    .toBuffer();

  return { blurhash, image };
}
```

---

## Recipe: Batch Processing
Process an entire folder of images while controlling CPU and memory usage.

```javascript
import { readFile, writeFile, readdir } from 'node:fs/promises';
import { pLimit } from 'p-limit'; // Popular community pkg

const limit = pLimit(4); // Only 4 images at a time

const files = await readdir('./uploads');
const tasks = files.map(file => limit(async () => {
  const buf = await readFile(`./uploads/${file}`);
  const out = await pixels(buf).grayscale().toBuffer();
  await writeFile(`./optimized/${file}`, out);
}));

await Promise.all(tasks);
```
