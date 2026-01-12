# avatar64

[![npm version](https://img.shields.io/npm/v/avatar64.svg)](https://www.npmjs.com/package/avatar64)
[![npm downloads](https://img.shields.io/npm/dm/avatar64.svg)](https://www.npmjs.com/package/avatar64)
[![license](https://img.shields.io/npm/l/avatar64.svg)](https://github.com/Binidu01/avatar64/blob/main/LICENSE)
[![bundle size](https://img.shields.io/bundlephobia/minzip/avatar64)](https://bundlephobia.com/package/avatar64)
[![TypeScript](https://img.shields.io/badge/TypeScript-Ready-blue.svg)](https://www.typescriptlang.org/)

A **bulletproof**, tiny, browser-only utility for **profile pictures**:

* Convert an image `File` into **WebP** and return **plain Base64 text**
* Convert **Base64 text** (or a full data URL) back into a safe `<img src>` value

**No UI. No storage. No backend. Just conversion and display.**

---

## ✨ Features

- 🛡️ **Bulletproof** - Comprehensive error handling and input validation
- 🪶 **Lightweight** - Zero dependencies, minimal bundle size
- 🔒 **Safe** - Built-in safety limits prevent browser freezing
- 📦 **TypeScript** - Full type definitions included
- 🎯 **Focused** - Does one thing exceptionally well
- ⚡ **Fast** - Optimized WebP conversion with high-quality output

---

## 📦 Install

```bash
npm install avatar64
```

```bash
yarn add avatar64
```

```bash
pnpm add avatar64
```

---

## ⚠️ Browser-only

This package uses **Canvas API** for image processing.  
It **must** run in the browser.

> Importing or executing conversion functions on the server (Node.js / SSR) will throw an error with a clear message.

---

## 🚀 Quick Start

### Convert: File → WebP Base64 (plain text)

```ts
import { fileToWebpBase64 } from "avatar64";

const input = document.querySelector<HTMLInputElement>("#file")!;
const output = document.querySelector<HTMLTextAreaElement>("#out")!;

input.addEventListener("change", async () => {
  const file = input.files?.[0];
  if (!file) return;

  try {
    const { base64, width, height, decodedBytes } = await fileToWebpBase64(file, {
      maxSize: 256,
      quality: 0.8,
    });

    // Plain base64 text (NO data: prefix)
    output.value = base64;
    console.log(`Converted to ${width}x${height}, ${decodedBytes} bytes`);
  } catch (error) {
    console.error("Conversion failed:", error.message);
  }
});
```

### Convert Options

```ts
type ConvertOptions = {
  maxSize?: number;        // default: 256px, set 0 to keep original size
  quality?: number;        // default: 0.8 (range: 0.1 to 1.0)
  allowedMime?: string[];  // default: ["image/png", "image/jpeg", "image/webp", "image/gif"]
  maxInputBytes?: number;  // default: 6MB (6 * 1024 * 1024)
};
```

### Convert Result

```ts
type ConvertResult = {
  base64: string;        // Plain base64 text (NO data: prefix)
  dataUrl: string;       // Full data URL for immediate preview
  width: number;         // Output dimensions
  height: number;
  decodedBytes: number;  // Exact size of WebP payload
};
```

---

## 🖼️ Show: Base64 → Image

### Get an `<img src>` value

```ts
import { base64ToImgSrc } from "avatar64";

try {
  img.src = base64ToImgSrc(base64Text);
  // → data:image/webp;base64,...
} catch (error) {
  console.error("Invalid base64:", error.message);
}
```

### One-liner helper

```ts
import { showBase64InImage } from "avatar64";

try {
  showBase64InImage(img, base64Text);
} catch (error) {
  console.error("Failed to display image:", error.message);
}
```

### Display Options

```ts
type ShowOptions = {
  mimeFallback?: string;     // default: "image/webp"
  stripWhitespace?: boolean; // default: true
  maxBase64Chars?: number;   // default: 3,000,000
  maxDecodedBytes?: number;  // default: 2MB (2 * 1024 * 1024)
};
```

---

## 🔧 API Reference

### `fileToWebpBase64(file, options?)`

Converts an image File to WebP format and returns base64 text.

**Parameters:**
- `file: File` - Image file to convert
- `options?: ConvertOptions` - Optional conversion settings

**Returns:** `Promise<ConvertResult>`

**Throws:** Detailed error if file is invalid, too large, unsupported type, or conversion fails

---

### `base64ToImgSrc(base64OrDataUrl, options?)`

Converts base64 text or data URL to a safe `<img src>` value.

**Parameters:**
- `base64OrDataUrl: string` - Plain base64 or full data URL
- `options?: ShowOptions` - Optional display settings

**Returns:** `string` - Safe data URL for image src

**Throws:** Detailed error if input is invalid, corrupted, or exceeds safety limits

---

### `showBase64InImage(img, base64OrDataUrl, options?)`

Convenience function to directly set an image element's src.

**Parameters:**
- `img: HTMLImageElement` - Target image element
- `base64OrDataUrl: string` - Plain base64 or full data URL
- `options?: ShowOptions` - Optional display settings

**Returns:** `void`

**Throws:** Detailed error if element is invalid or base64 is corrupted

---

### `base64DecodedByteLength(base64)`

Calculate the exact decoded byte size of a base64 string.

**Parameters:**
- `base64: string` - Base64 string to measure

**Returns:** `number` - Exact decoded bytes (0 if invalid)

---

## 🛡️ Safety Features

- ✅ Validates all inputs (File type, MIME, size, dimensions)
- ✅ Protects against corrupted or malicious files
- ✅ Prevents browser freezing with size limits
- ✅ Timeout protection for slow image loading
- ✅ Proper resource cleanup (blob URLs, canvas)
- ✅ Descriptive error messages for debugging
- ✅ Handles edge cases (empty files, invalid dimensions)

### Default Safety Limits

| Limit | Default | Purpose |
|-------|---------|---------|
| `maxInputBytes` | 6 MB | Reject oversized input files |
| `maxSize` | 256px | Resize large images |
| `maxBase64Chars` | 3,000,000 | Prevent massive base64 strings |
| `maxDecodedBytes` | 2 MB | Limit decoded image size |
| Image dimensions | 16384×16384 | Prevent canvas errors |
| Load timeout | 30 seconds | Prevent hanging |

---

## 📝 Notes & Limitations

- **Base64 inflates size** by ~33% compared to binary
- **Output format** is always WebP for optimal size/quality
- **Browser support** requires Canvas API (all modern browsers)
- **Designed for avatars** - profile pictures, not large photos
- **No server-side support** - browser environment required
- **WebP encoding** must be supported by browser (widely supported)

---

## 💡 Use Cases

- User profile picture uploads
- Avatar selection interfaces
- Image preview before upload
- Client-side image optimization
- Progressive web apps with offline image handling
- Chat applications with avatar display
- Account settings pages

---

## 🔄 Example: Complete Upload Flow

```ts
import { fileToWebpBase64, base64ToImgSrc } from "avatar64";

async function handleAvatarUpload(file: File) {
  try {
    // Convert to WebP base64
    const result = await fileToWebpBase64(file, {
      maxSize: 256,
      quality: 0.85,
    });

    // Preview locally
    const preview = document.querySelector<HTMLImageElement>("#preview")!;
    preview.src = result.dataUrl;

    // Send plain base64 to your API
    await fetch("/api/profile/avatar", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ avatar: result.base64 }),
    });

    console.log(`✓ Uploaded ${result.width}×${result.height} avatar`);
  } catch (error) {
    console.error("Upload failed:", error.message);
    alert(`Failed to upload avatar: ${error.message}`);
  }
}

// Later, display stored avatar
function displayAvatar(base64: string) {
  const img = document.querySelector<HTMLImageElement>("#avatar")!;
  try {
    img.src = base64ToImgSrc(base64);
  } catch (error) {
    console.error("Failed to display avatar:", error.message);
    img.src = "/default-avatar.png"; // fallback
  }
}
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

## 📄 License

MIT © [Binidu01](https://github.com/Binidu01)

---

## 🔗 Links

- [GitHub Repository](https://github.com/Binidu01/avatar64)
- [npm Package](https://www.npmjs.com/package/avatar64)
- [Report Issues](https://github.com/Binidu01/avatar64/issues)

---

**Made with ❤️ for the web**