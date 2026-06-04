# Image Compress & Convert

A tiny, **100% offline** image compressor and converter that runs entirely in your browser. No server, no uploads — your images never leave your computer.

👉 Open `ImageCompressor.html` in any modern browser (just double-click it).

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

## Run it

No build step. Open the HTML file directly, **or** serve the folder:

```bash
python -m http.server 8000
# then visit http://localhost:8000/ImageCompressor.html
```

Keep `ImageCompressor.html` and the `vendor/` folder together.

## License

MIT. Bundled libraries retain their own licenses (all MIT).
