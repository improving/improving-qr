# Agent Instructions for improving-qr

## Project Overview

This is a GitHub Pages-hosted QR code generator that creates QR codes with the Improving logo overlay.

**Live site:** https://improving.github.io/improving-qr/

## Versioning

This project uses [Semantic Versioning](https://semver.org/):

- **MAJOR** (x.0.0): Breaking changes or major redesigns
- **MINOR** (0.x.0): New features, enhancements
- **PATCH** (0.0.x): Bug fixes, minor tweaks

## Version Location

The version is displayed on the web page and must be updated in `index.html`:

```html
<div class="version">vX.Y.Z</div>
```

This is near the end of the `<body>` tag, before the `<script>` section.

## Release Process

When making changes to this project, follow these steps:

### 1. Make your changes

Edit the necessary files (primarily `index.html`).

### 2. Update the version number

Edit `index.html` and update the version in the `.version` div to the new version number.

### 3. Commit the changes

```bash
git add <files>
git commit -m "Description of changes

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>"
```

### 4. Create an annotated tag

```bash
git tag -a vX.Y.Z -m "Brief description"
```

### 5. Push with tags

```bash
git push --follow-tags
```

### 6. Create a GitHub Release

```bash
gh release create vX.Y.Z --title "vX.Y.Z - Title" --notes "Description of changes." --latest
```

## File Structure

```
improving-qr/
├── index.html           # Main application (HTML, CSS, JS all-in-one)
├── improving_splash.svg # Improving logo for QR code overlay
└── AGENTS.md            # This file
```

## Key Implementation Details

- **QR Library:** Uses qrcodejs from CDN (`https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js`)
- **Error Correction:** Uses level H (highest) to allow logo overlay while maintaining scannability
- **Logo Size:** ~22% of QR code size, with white circular background
- **Deployment:** Automatic via GitHub Pages on push to `main` branch
