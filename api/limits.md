# 🛡️ Operational Safety (Limits)

Image processing is resource-intensive. A single malicious file, known as a **"Decompression Bomb"**, can appear small (e.g., 50KB) but expand into gigabytes of memory once decoded. Pixels protects your infrastructure with a mandatory **resource guard** system.

## The Limits System
Every time you initialize a `Pixels` instance, you can provide a `Limits` configuration.

```javascript
const result = await pixels(buffer, {
  limits: {
    maxInputBytes: 10 * 1024 * 1024, // 10MB limit on the encoded file
    maxPixels: 20_000_000,           // 20MP limit on decoded dimensions
    maxAllocBytes: 256 * 1024 * 1024 // 256MB limit on peak memory usage
  }
})
.resize({ width: 800 })
.toBuffer();
```

---

## Limit Definitions

### `maxInputBytes`
The maximum size of the raw input buffer passed to Pixels.
- **Default:** 30MB
- **Why?** Prevents large file uploads from overwhelming your process before they are even decoded.

### `maxPixels`
The maximum number of pixels (width * height) the image can have once decoded.
- **Default:** 30,000,000 (30MP)
- **Why?** This is the primary defense against decompression bombs.

### `maxAllocBytes`
A "hard cap" on the total memory that the engine is allowed to allocate.
- **Default:** 512MB
- **Why?** This protects your server's RAM if an operation (like a very large blur or watermark) requires significant intermediate space.

---

## Best Practices

### For Bulk Uploads
If you are running a SaaS platform, set these limits as conservatively as possible based on your expected user behavior. If your users only upload profile photos, a 5MP limit is more than enough and very safe.

### Error Handling
When a limit is exceeded, Pixels will throw an error with the code `MEMORY_LIMIT_EXCEEDED`.

```javascript
try {
  await pixels(largeFile).resize({ width: 100 }).toBuffer();
} catch (err) {
  if (err.code === 'MEMORY_LIMIT_EXCEEDED') {
    // Notify user to upload a smaller image
  }
}
```
