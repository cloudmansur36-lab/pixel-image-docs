# 📈 Metrics & Monitoring

Pixels is designed for mission-critical production environments. It provides native visibility into its internal processing state via Prometheus-compatible metrics.

## Usage

```javascript
import { metricsPrometheus } from '@pixels-image/core';

// In your monitoring endpoint
app.get('/metrics', (req, res) => {
  res.set('Content-Type', 'text/plain');
  res.send(metricsPrometheus());
});
```

---

## Exposed Metrics

The engine exports several counts and histograms:

| Metric | Type | Description |
| :--- | :--- | :--- |
| `pixels_ops_total` | Counter | Total number of operations (resize, blur, etc.) attempted. |
| `pixels_ops_failed_total` | Counter | Total number of operations that threw an error. |
| `pixels_bytes_processed_total`| Counter | Cumulative sum of processed input bytes. |
| `pixels_processing_duration_seconds`| Histogram | Latency distribution of individual operations. |

---

# 🌐 WASM & Edge Runtimes

The `@pixels-image/wasm` package brings the power of Pixels to environments where native addons aren't supported.

## How it works
The core Rust code is compiled to WebAssembly (WASM). While WASM lacks some of the SIMD optimizations available in our native builds (due to browser support variance), it still outperforms pure JavaScript alternatives by a significant margin.

## Initialization
Unlike the native Node.js package, the WASM engine is **asynchronous** and must be initialized before use.

```javascript
import init, { pixels } from '@pixels-image/wasm';

async function processImage(data) {
  // 1. Fetch and compile the WASM binary (only needs to be called once)
  await init();

  // 2. The API is now identical to the native version
  const out = await pixels(data).resize({ width: 200 }).toBuffer();
}
```

## Memory Management
WASM has a fixed linear memory space. If you are processing many images in a row in a long-lived Worker or browser tab, ensure you are staying within the WASM memory limits. You can configure these using the `limits` object, just like in Node.js.
