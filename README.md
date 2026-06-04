# Image Compress & Convert

A tiny, **100% offline** image compressor and converter that runs entirely in your browser. No server, no uploads, no install — your images never leave your computer.

## ▶️ How to open the app

Pick whichever is easiest for you:

### Option 1 — Use it online (nothing to download)
Just open this link:

**https://krocosolas.github.io/image-compressor/**

It runs in your browser; images are still processed locally on your device.

### Option 2 — Run it on your computer (works offline)
1. Download the project: on the [GitHub page](https://github.com/KrocoSOLAS/image-compressor), click the green **`Code`** button → **Download ZIP**.
2. **Unzip** it (don't run it from inside the ZIP).
3. Open the unzipped folder and **double-click `index.html`** — it opens in your default browser.

⚠️ Keep `index.html` and the **`vendor/`** folder together in the same folder. If you separate them, the TIFF/EXR/TGA features stop working (JPEG/PNG/WebP/AVIF still work).

That's it — no installation, and no internet connection needed once downloaded.

## Features

- **Drag & drop** one or many images
- **Convert** between JPEG, WebP, AVIF, PNG, TIFF, and TGA
- **Compress** with a quality slider, or a **target file-size** mode that auto-picks the quality to hit a size (e.g. "under 200 KB")
- **Resize** — simple presets (4K → 320px) or detailed control (fit / stretch / scale by %)
- **Before/after compare** slider for every result
- **Strip or keep metadata** (EXIF/GPS), with EXIF orientation handled safely
- **Batch download** as a single ZIP
- Shows **dimensions** and **% saved** per image

### Supported input formats
JPEG, PNG, WebP, GIF, BMP, **TIFF**, **TGA**, **EXR** (HDR, tone-mapped to sRGB)

## How it works

Standard formats use the browser's built-in Canvas codecs. The extra formats use small libraries bundled locally in `vendor/` so the app still works with no internet:

- **TIFF** — [UTIF.js](https://github.com/photopea/UTIF.js)
- **EXR** — [parse-exr](https://github.com/repalash/parse-exr) (+ [fflate](https://github.com/101arrowz/fflate))
- **TGA** — a small hand-written codec

## Run it from a local server (optional)

Opening `index.html` directly is enough. If you'd rather serve it:

```bash
python -m http.server 8000
# then visit http://localhost:8000/
```

## License

MIT — see [LICENSE](LICENSE). Bundled libraries retain their own licenses (all MIT).
