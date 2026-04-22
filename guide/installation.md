# 📦 Installation Guide

Pixels is distributed as two primary packages depending on your target environment. Both share the same Rust core and API.

## 1. Native Node.js Addon
Use this for server-side applications where performance is critical (e.g., standard Node scripts, Express servers).

```bash
npm install @pixels-image/core
```

### Precompiled Binaries
Pixels ships with prebuilt binaries for all major platforms. You **do not** need to install `vips`, `python`, or `make` on your server.

| Operating System | Architecture | Build Type |
| :--- | :--- | :--- |
| **Windows** | x64 | MSVC |
| **macOS** | x64 (Intel), arm64 (M1/M2) | Universal |
| **Linux** | x64, arm64 | GLIBC (Ubuntu, Debian) |
| **Linux (musl)** | x64, arm64 | musl (Alpine) |

---

## 2. WebAssembly (Browser & Edge)
Use this for client-side processing, Cloudflare Workers, or Vercel Edge functions.

```bash
npm install @pixels-image/wasm
```

### Bundle Compatibility
The `@pixels-image/wasm` package is designed to work out-of-the-box with modern bundlers:
- **Vite:** Supported natively via `?url` or standard imports.
- **Webpack 5:** Supported with `experiments.asyncWebAssembly = true`.
- **Cloudflare Workers:** Supported via the Wrangler WASM bundler.

---

## ⚠️ Troubleshooting

### EOTP Error during Install
If you encounter a `2FA` or `OTP` error during installation, check your `npm` configuration. Pixels uses `Automation` tokens for its CI, but users only need standard access to the `@pixels-image` scope.

### Missing Binary for Linux
If you are on an exotic Linux distribution and the install fails, you can force the use of the `musl` build (which is more portable) by setting:
```bash
npm_config_target=x86_64-unknown-linux-musl npm install @pixels-image/core
```
