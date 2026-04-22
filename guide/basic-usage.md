# 🚀 Basic Usage

Pixels is built around a simple principle: **Load → Transform → Output**.

## The Hello World
Here is the most basic example of loading a JPEG, resizing it, and saving it as a WebP.

```javascript
import { pixels } from '@pixels-image/core';
import { readFile, writeFile } from 'node:fs/promises';

async function main() {
  // 1. Load the image into a Buffer
  const input = await readFile('input.jpg');

  // 2. Initialise the engine and chain operations
  const output = await pixels(input)
    .resize({ width: 500 })
    .toFormat('webp')
    .toBuffer();

  // 3. Save the result
  await writeFile('output.webp', output);
}

main();
```

---

## Input Types
The `pixels()` function is flexible and accepts:
- **Node.js:** `Buffer`, `Uint8Array`.
- **Browser/WASM:** `Uint8Array`.

## The Fluent Pattern
Pixels uses a "Promise-like" builder. Operations are stored in a queue and only executed when you call one of the **Terminal Methods**:
- `.toBuffer()`
- `.metadata()`
- `.placeholders()`

### Why this matters
By queuing operations, Pixels can optimize the processing pipeline. For example, it might combine multiple crops or color operations into a single pass through the image pixels, maximizing CPU cache efficiency.

---

## Error Handling
Pixels uses structured error objects with stable codes. Always wrap your processing in a `try/catch` block.

```javascript
try {
  await pixels(badBuffer).resize({ width: 100 }).toBuffer();
} catch (err) {
  if (err.code === 'INPUT_BUFFER_CORRUPTED') {
    console.error("The uploaded file is not a valid image.");
  }
}
```
