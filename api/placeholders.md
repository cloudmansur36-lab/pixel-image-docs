# ✨ Image Placeholders

Pixels integrates modern placeholder technologies directly into the engine, allowing you to generate lightweight representations of your images for improved User Experience (UX).

## Supported Formats
Pixels can generate three types of placeholders in a single pass:

| Format | Description | Size |
| :--- | :--- | :--- |
| **BlurHash** | A short string representing the image's blur. | ~20-30 chars |
| **ThumbHash** | A newer, more robust alternative to BlurHash that encodes alpha and aspect ratio. | ~20-30 bytes |
| **Average Color**| The mean RGB color of the image. | 4 bytes |

---

## Usage

```javascript
const result = await pixels(buffer).placeholders({
  blurhash: true,
  thumbhash: true,
  averageColor: true
});

console.log(result.blurhash);    // "LEHV6nVQClVQcmV[jZbx00V@webH"
console.log(result.averageColor); // { r: 124, g: 32, b: 24, a: 255 }
```

### Options
The `.placeholders()` method accepts an optional configuration:
- `blurhashControl`: *number* (Default: 4). Controls the detail level.
- `thumbhash`: *boolean*
- `averageColor`: *boolean*

## Why use Placeholders?
By generating these on the fly during image upload/processing, you can store the tiny strings in your database. When a user visits your site, you can show the BlurHash or Average Color immediately while the actual large image loads in the background, preventing layout shifts and improving **Core Web Vitals (LCP)**.
