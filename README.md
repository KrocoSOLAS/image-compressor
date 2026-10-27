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

- **Drag & drop** one or many images — or a whole folder
- **Convert** between JPEG, WebP, AVIF, PNG, TIFF, and TGA
- **Compress** with a quality slider, or a **target file-size** mode that auto-picks the quality to hit a size (e.g. "under 200 KB")
- **Resize** — simple presets (4K → 320px) or detailed control (fit / stretch / scale by %)
- **Save straight back to the source folder**, keeping or replacing the originals (see below)
- **Before/after compare** slider for every result
- **Strip or keep metadata** (EXIF/GPS), with EXIF orientation handled safely
- **Batch download** as a single ZIP
- Processes images **in parallel across several CPU cores**, with a progress bar, so the page stays responsive on big batches
- Shows **dimensions** and **% saved** per image

### Saving back to the source folder

By default results land in your Downloads folder. Set **Save results to → The folder the images came from** and the app writes each result next to its original — no download prompts, including for images in subfolders.

Pick a folder with **Choose folder…**, or just drop a folder onto the box. Then choose what happens to the originals:

- **Keep them** (default) — results are saved alongside as `name-compressed.jpg`. Nothing is overwritten: if that name is taken you get `name-compressed_1.jpg`. Files already ending in `-compressed` are skipped, so running twice over the same folder won't recompress your own output.
- **Replace them** — the result takes the original's place. Converting to a different format writes the new file first and only then deletes the original. **This cannot be undone**, so it asks you to confirm, telling you exactly how many files will be overwritten and how many deleted.

> ⚠️ Writing to your disk needs the File System Access API, which today means **Chrome, Edge or Opera**. [Firefox has taken the position that the local-disk pickers are harmful](https://caniuse.com/mdn-api_window_showdirectorypicker) and Safari ships only a sandboxed private filesystem, so in those browsers this option is greyed out and you'll get downloads instead. The browser asks your permission for the folder before anything is written, and a folder scan stops at 500 images.
>
> Files added through **browse** carry no write permission — only a folder you picked or dropped does. Those results stay as downloads and the card says so.

### Supported input formats
JPEG, PNG, WebP, GIF, BMP, **TIFF**, **TGA**, **EXR** (HDR, tone-mapped to sRGB), **HEIC/HEIF** (e.g. iPhone photos)

> **AVIF export** depends on the browser. Canvas is specified to quietly hand back a PNG when it can't encode the format you asked for, and most browsers currently decline AVIF, which would mean a `.avif` file that is really a PNG. The app tests each encoder at startup and greys out any format your browser can't genuinely produce.

> HEIC is **input only** — you can convert HEIC *to* JPEG/PNG/WebP/etc., but exporting *to* HEIC isn't supported (no client-side HEIC encoder exists). The HEIC decoder (`libheif`, ~1.4 MB) is the heaviest bundled library, and it handles modern tiled/grid HEIC such as iPhone photos.

## How it works

Standard formats use the browser's built-in Canvas codecs. The extra formats use small libraries bundled locally in `vendor/` so the app still works with no internet:

- **TIFF** — [UTIF.js](https://github.com/photopea/UTIF.js)
- **EXR** — [parse-exr](https://github.com/repalash/parse-exr) (+ [fflate](https://github.com/101arrowz/fflate))
- **HEIC/HEIF** — [libheif-js](https://github.com/catdad-experiments/libheif-js) (current libheif compiled to WASM, embedded)
- **TGA** — a small hand-written codec

Resizing and encoding run in a pool of Web Workers, sized from the number of CPU cores, so a batch doesn't lock up the page. The worker is built from a Blob URL rather than a separate `worker.js`, because Chromium refuses to start a worker from a script file on a `file://` page — this way double-clicking `index.html` still works, and the app stays a single file.

The vendor decoders can't be loaded into a worker on a `file://` page either, so TIFF/EXR/TGA/HEIC are decoded on the page and only their pixels are handed to a worker. Older Safari has no `OffscreenCanvas.convertToBlob`; the app checks for it at startup and falls back to doing the work on the page.

EXR files are HDR, so values can run far above white. Any file that stays within 0–1 is converted exactly as before; one that doesn't is tone-mapped with extended Reinhard, using the image's own brightest sample as the white point, so highlights keep their detail instead of flattening out.

## Run it from a local server (optional)

Opening `index.html` directly is enough. If you'd rather serve it:

```bash
python -m http.server 8000
# then visit http://localhost:8000/
```

## License

MIT — see [LICENSE](LICENSE). Bundled libraries retain their own licenses (all MIT).
