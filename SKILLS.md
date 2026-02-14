# QR Code Generator - AI Skills

**Skill Version:** 1.5.2

## Self-Update Check (Do This First!)

Before executing this skill, check for the latest version:

1. **Fetch the latest SKILLS.md:**
   ```
   https://raw.githubusercontent.com/improving/improving-qr/refs/heads/main/SKILLS.md
   ```

2. **Compare the `Skill Version:` at the top of the file**
   - If the remote version is newer than your cached version, use the remote instructions instead
   - This ensures you're always working with instructions compatible with the latest website

3. **Then proceed with the instructions below**

---

This document describes how AI agents can programmatically generate QR codes using the Improving QR Code Generator.

## Base URL

```
https://improving.github.io/improving-qr/
```

## Quick Reference

To generate a QR code for a URL, append it as a query parameter (URL-encoded):

```
https://improving.github.io/improving-qr/?url={encoded_url}
```

## Using Playwright CLI

The recommended way for AI agents to generate QR codes programmatically is using `playwright-cli` (or `npx playwright`).

### Prerequisites

Ensure Playwright is available:
```bash
npx playwright install chromium
```

### Step-by-Step: Generate and Save QR Code as SVG

```bash
# 1. Set the target URL (the URL you want the QR code to point to)
TARGET_URL="https://improving.com"

# 2. URL-encode the target (replace special characters)
ENCODED_URL=$(printf '%s' "$TARGET_URL" | jq -sRr @uri)

# 3. Use Playwright to navigate, wait for generation, and extract SVG
npx playwright \
  --browser chromium \
  "https://improving.github.io/improving-qr/?url=${ENCODED_URL}" \
  --wait-for "svg" \
  --eval "await window.getQRCodeSVG()" \
  > qrcode.svg
```

### Alternative: Using Playwright in JavaScript

If you have more complex needs, use Playwright's Node.js API:

```javascript
const { chromium } = require('playwright');

async function generateQRCode(targetUrl, outputPath) {
    const browser = await chromium.launch();
    const page = await browser.newPage();

    // Navigate with the URL parameter
    const encodedUrl = encodeURIComponent(targetUrl);
    await page.goto(`https://improving.github.io/improving-qr/?url=${encodedUrl}`);

    // Wait for the QR code SVG to be generated
    await page.waitForSelector('#qrcode svg');

    // Extract the raw SVG markup
    const svg = await page.evaluate(() => window.getQRCodeSVG());

    // Save to file
    require('fs').writeFileSync(outputPath, await svg);

    await browser.close();
}

// Usage
generateQRCode('https://improving.com', 'qrcode.svg');
```

### Getting PNG Instead of SVG

To get a PNG, use the download functionality:

```javascript
const { chromium } = require('playwright');

async function generateQRCodePNG(targetUrl, outputPath) {
    const browser = await chromium.launch();
    const page = await browser.newPage();

    const encodedUrl = encodeURIComponent(targetUrl);
    await page.goto(`https://improving.github.io/improving-qr/?url=${encodedUrl}`);

    // Wait for QR code to generate
    await page.waitForSelector('#qrcode svg');

    // Set up download handler
    const [download] = await Promise.all([
        page.waitForEvent('download'),
        page.click('button:has-text("Download PNG")')
    ]);

    // Save the downloaded file
    await download.saveAs(outputPath);

    await browser.close();
}

generateQRCodePNG('https://improving.com', 'qrcode.png');
```

## URL Encoding Reference

The target URL must be URL-encoded. Common replacements:

| Character | Encoded |
|-----------|---------|
| `:` | `%3A` |
| `/` | `%2F` |
| `?` | `%3F` |
| `&` | `%26` |
| `=` | `%3D` |
| ` ` | `%20` |

**Using jq (recommended):**
```bash
echo "https://example.com/path?param=value" | jq -sRr @uri
```

**Using Python:**
```bash
python3 -c "import urllib.parse; print(urllib.parse.quote('https://example.com', safe=''))"
```

## API Reference

### URL Parameter

| Parameter | Required | Description |
|-----------|----------|-------------|
| `url` | Yes | The URL to encode in the QR code (must be URL-encoded) |

### JavaScript API

Once the page has loaded and a QR code is generated, the following function is available:

```javascript
// Returns a Promise that resolves to the raw SVG markup string
await window.getQRCodeSVG()
```

## Output Specifications

Generated QR codes have these properties:

- **Dimensions:** 256x256 pixels
- **Format:** SVG (scalable) or PNG
- **Dot Style:** Rounded
- **Colors:** Improving Blue (#005596) dots, Light Blue (#4597D3) corner dots
- **Logo:** Improving logo centered
- **Error Correction:** Level H (30% redundancy, allows logo overlay)

## Example URLs

| Target URL | Generator URL |
|------------|---------------|
| `https://improving.com` | `https://improving.github.io/improving-qr/?url=https%3A%2F%2Fimproving.com` |
| `https://github.com/improving` | `https://improving.github.io/improving-qr/?url=https%3A%2F%2Fgithub.com%2Fimproving` |
