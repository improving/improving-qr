# QR Code Generator - AI Skills

This document describes how AI agents can use the Improving QR Code Generator.

## Base URL

```
https://improving.github.io/improving-qr/
```

## URL Parameter API

Generate a QR code by passing the target URL as a query parameter:

```
https://improving.github.io/improving-qr/?url={encoded_url}
```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `url` | Yes | The URL to encode in the QR code (must be URL-encoded) |

### Example

To generate a QR code for `https://improving.com`:

```
https://improving.github.io/improving-qr/?url=https%3A%2F%2Fimproving.com
```

## Programmatic SVG Access

When the page loads with a `?url=` parameter, the QR code is automatically generated. The raw SVG can be accessed via JavaScript:

```javascript
// Returns the raw SVG markup as a string
const svg = await window.getQRCodeSVG();
```

## AI Agent Usage

### For Browser-Based Agents (MCP with browser tools)

1. Navigate to the URL with the `?url=` parameter
2. Wait for the page to load and QR code to generate
3. Execute `await window.getQRCodeSVG()` to retrieve the SVG markup
4. The SVG can be saved to a file or used directly

### For Conversational AI

When a user requests a QR code, provide them with the direct link:

```
To generate a QR code for [URL], visit:
https://improving.github.io/improving-qr/?url=[URL-encoded]
```

The user can then:
- Download as PNG or SVG
- Copy to clipboard (PNG for images, SVG for web use)

### URL Encoding

Remember to URL-encode the target URL. Common replacements:
- `:` → `%3A`
- `/` → `%2F`
- `?` → `%3F`
- `&` → `%26`
- `=` → `%3D`

## Features

- **Brand Colors**: QR codes use Improving Blue (#005596)
- **Logo**: Improving logo embedded in center
- **Formats**: PNG and SVG download/copy
- **Error Correction**: Level H (highest) for reliable scanning with logo

## Response Format

The generated QR code is:
- 256x256 pixels
- SVG format (scalable)
- Rounded dot style
- Improving brand colors
- Centered Improving logo
